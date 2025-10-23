---
class: note
area: professional
tags:
  - ai
created: 2025-10-24
---
**MOC:[[MOC - Professional]]**

# Security Checklist и Threat Model (STRIDE)

> [!tldr] ai review
> 

## Security Checklist (короткая)

|№|Контроль|Статус|
|---|---|---|
|1|Шифрование каналов (TLS 1.3)|☐|
|2|PII-редакция до передачи в LLM|☐|
|3|MCP tools имеют минимальные права|☐|
|4|Все tool-calls логируются|☐|
|5|Sandbox блокирует внешние вызовы|☐|
|6|Pen-test MCP manifest выполнен|☐|
|7|RBAC и audit trail включены|☐|
|8|Rate limiting и retry policy для ChatGPT|☐|
|9|Мониторинг на утечки и токсичность|☐|
|10|Отчёты по комплаенсу (GDPR/локальные)|☐|

## Threat Model (STRIDE

|Категория|Угроза|Меры защиты|
|---|---|---|
|**S – Spoofing**|Подмена запроса/пользователя|OAuth 2.0, подписанные JWT-токены|
|**T – Tampering**|Изменение MCP manifest или RAG-данных|Контроль версий, readonly storage|
|**R – Repudiation**|Отсутствие аудита действий|Иммутабельные audit-логи|
|**I – Information Disclosure**|Утечка PII через LLM|PII-редактор, контент-фильтры|
|**D – Denial of Service**|Перегрузка LLM-API|Rate-limit, circuit breaker|
|**E – Elevation of Privilege**|Несанкционированный вызов MCP tools|RBAC, whitelisting tools|

### Additional materials