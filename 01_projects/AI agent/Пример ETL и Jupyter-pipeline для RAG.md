---
class: note
area: professional
tags:
  - ai
created: 2025-10-24
---
**MOC:[[MOC - Professional]]**

# Пример ETL и Jupyter-pipeline для RAG

> [!tldr] ai review
> 

## etl_rag_pipeline.py

```python
import pandas as pd
from langchain_text_splitters import RecursiveCharacterTextSplitter
from openai import OpenAI
import chromadb
import re

# 1. Загрузка тикетов
df = pd.read_csv("tickets.csv")

# 2. Очистка текста (удаление PII)
def redact_pii(text):
    text = re.sub(r"\b[\w.-]+@[\w.-]+\b", "[REDACTED_EMAIL]", text)
    text = re.sub(r"\+?\d[\d\s()-]{8,}\d", "[REDACTED_PHONE]", text)
    return text

df["text_clean"] = df["text"].apply(redact_pii)

# 3. Разбиение на чанки
splitter = RecursiveCharacterTextSplitter(chunk_size=800, chunk_overlap=100)
documents = []
for _, row in df.iterrows():
    for chunk in splitter.split_text(row["text_clean"]):
        documents.append({
            "ticket_id": row["ticket_id"],
            "chunk": chunk
        })

# 4. Генерация эмбеддингов
client = OpenAI()
embed = lambda t: client.embeddings.create(model="text-embedding-3-small", input=t).data[0].embedding

for d in documents:
    d["embedding"] = embed(d["chunk"])

# 5. Индексирование во векторную БД
chroma_client = chromadb.Client()
collection = chroma_client.get_or_create_collection("tickets_rag")
collection.add(
    ids=[d["ticket_id"] + str(i) for i, d in enumerate(documents)],
    embeddings=[d["embedding"] for d in documents],
    documents=[d["chunk"] for d in documents]
)

print("✅ RAG база обновлена")

```

## Jupyter Notebook шаги

```md
1️⃣ Импорт библиотек и подключение к источнику тикетов  
2️⃣ Очистка и анонимизация данных  
3️⃣ Разбиение текста (chunking)  
4️⃣ Генерация эмбеддингов (вызов OpenAI / локальной модели)  
5️⃣ Загрузка в векторную БД  
6️⃣ Проверка качества: hit@5, precision@k  
7️⃣ Экспорт отчёта и графиков качества

```


### Additional materials