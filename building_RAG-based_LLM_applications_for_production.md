# Building RAG-based LLM Applications for Production

Source: https://www.anyscale.com/blog/a-comprehensive-guide-for-building-rag-based-llm-applications-part-1

## Goals

* RAG 기반의 LLM application을 만든다
* load, chunk, embed ,index, serve 와 같은 주요 워크로드를 서로 다른 리소스를
  가진 여러 개의 워커로 확장한다
* Evaluate different configurations of our application to optimize for both
  per-component (e.g., `retrieval_score`) and overall performance
  (`quality_score`) 
* 하이브리드 방식의 라우팅 방식을 구현: 가장 성능이 좋고 비용 효과적인 어플리케이션을 만들기 위해.
  * 오픈소스 모델과 폐쇄형 LLM 을 섞어서 사용하자는 것
* Serve the application in a highly scalable and available manner
* Fine-tuning, 프롬프트 엔지니어링, 구문 검색, reranking, data flywheel 같은
  방법들이 어플리케이션의 성능에 어떤 영향을 미치는지 안다

## Overview

* LLM이 가진 문제: 각 모델이 학습된 정보에 대해서만 알고 있다. 
* RAG기반의 LLM 응용은 이 문제를 해결하고, LLM의 효용을 우리의 특정한 데이터 소스로 확장한다.
* 이 가이드에서는 [Ray](https://github.com/ray-project/ray)에 대한 assistant 를 만들 것임.

흐름도.

1. Pass the query to the embedding model to semantically represent it as an embedded query vector
2. Pass the embedded query vector to our vector DB
3. Retrieve the top-k relevant contexts -- measured by distance between the
   query embedding and all the embedded chunks in our knowledge base.
4. Pass the query text and retrieved context text to our LLM.
5. The LLM will generate a response using the provided content.

단지 LLM 응용을 만드는 것 뿐만 아니라, 우리는 production 환경에서 scaling과 serving을 고려.

> Besides just building our LLM application, we're also going to be focused on evaluation
> and performance. Our application involves many moving pieces: embedding models, chunking
> logic, the LLM itself, etc. and so it's important that we experiment with
> differet configurations to optimize for the best quality responses.
> However, it's non-trivial to evaluate and quantitatively compare different configurations
> for a generative task. 

## Vector DB creation

![Vector DB Creation](https://images.ctfassets.net/xjan103pcp94/3q5HUANQ4kS0V23cgEP0JF/ef3b62c5bc5c5c11b734fd3b73f6ea28/image3.png)

### Load data

Ray documentation을 로컬 디렉토리에 저장하는 것으로 시작.

```
export EFS_DIR=/desired/output/directory
wget -e robots=off --recursive --no-clobber --page-requisites \
    --html-extension --convert-links --restrict-file-names=windows \
    --domains docs.ray.io --no-parent --accept=html \
    -P $EFS_DIR https://docs.ray.io/en/master/
```

이 문서를 [Ray Dataset](https://docs.ray.io/en/latest/data/data.html)에 넣어서
embed, index 등의 operation을 수행한다.

```
# Ray Dataset
DOCS_DIR = Path(EFS_DIR, "docs.ray.io/en/master/")
ds = ray.data.from_items([{"path": path} for path in DOCS_DIR.rglob("*.html") if not path.is_dir()])
print(f"{ds.count()} documents")
```
(Note: `rglob`는 recursive 하게 glob 하는 것. 하위 디렉토리까지 다 찾는다.)

사족이지만 Bulk Load 는 중요한 기능이다. 
"Architecture of a Database System" 에서 다음과 같이 설명한다. 

> As such, it is crucial that data warehouses be bulk-loadable very
> quickly. Although one could program warehouse loads with a sequence
> of SQL insert statements, this tactic is never used in practice. Instead
> a bulk loader is utilized that will stream large numbers of records into
> storage without the overhead of the SQL layer, and taking advantage of
> special bulk-load methods for access methods like B+-trees. In round
> numbers, a bulk loader is an order of magnitude faster than SQL inserts,
> and all major vendors offer a high performance bulk loader.

### Sections

* 문서 파일에서 어떻게 내용을 extract 할 것인지?
* HTML 페이지에서 section을 찾아낸다
* Section 사이의 텍스트를 extract 한다
* We save all of this into a list of dictionaries that map the text within a section
  to a specific url with a section anchor id

```json
{
    'source': 'https://docs.ray.io/en/master/rllib/rllib-env.html#environments',
    'text': '\nEnvironments#\nRLlib works with several different types of environments, including Farama-Foundation Gymnasium, user-defined, multi-agent, and also batched environments.\nTip\nNot all environments work with all algorithms. Check out the algorithm overview for more information.\n'
}
```

Ray Data의
[`flat_map`](https://docs.ray.io/en/latest/data/api/doc/ray.data.Dataset.flat_map.html)
을 이용하여 extraction process 를 병렬로 처리할 수 있다.

```python
# Extract sections
sections_ds = ds.flat_map(extract_sections)
sections = sections_ds.take_all()
print(len(sections))
```

### Chunk data

* 섹션의 list 가 있지만, 이것을 바로 쓸 수는 없다
* 각 섹션의 길이가 다르기 때문. 특히 한 섹션이 너무 길면?
* LLM은 maximum context length 제한을 가짐
* 따라서 각 섹션을 작은 chunk로 나눈다
* 직관적으로 보면 작은 chunk가 좋음. less noisy.
  * typical value: `chunk_size=300`

```python
from langchain.document_loaders import ReadTheDocsLoader
from langchain.text_splitter import RecursiveCharacterTextSplitter

# Text splitter
chunk_size = 300
chunk_overlap = 50
text_splitter = RecursiveCharacterTextSplitter(
    separators=["\n\n", "\n", ","],
    chunk_size=chunk_size,
    chunk_overlap=chunk_overlap,
    length_function=len,
)

# Chunk a sample section
sample_section = sections_ds.take(1)[0]
chunks = text_splitter.create_documents(
    texts=[sample_section["text"]],
    metadatas=[{"source": sample_section["source"]}]
)

print(chunks[0])

