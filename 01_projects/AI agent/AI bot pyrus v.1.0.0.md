---
class: note
area: professional
tags:
  - ai
created: 2025-10-23
---
**MOC: [[MOC - Professional]]**

# AI bot pyrus v.1.0.0

> [!tldr] ai review
> 


## Краткое резюме

Цель: создать безопасного, объяснимого и расширяемого AI-агента, который будет консультировать пользователей по работе с ЭДО, уметь извлекать и обогащать контекст через MCP (Model Context Protocol), отвечать с использованием LLM и опираться на RAG-базу знаний, сформированную из прошлых решённых обращений.

## Фазы и общий план

Каждая фаза даёт конкретные артефакты, входы/выходы, зависимости и критерии приемки.

### Фаза A — Исследование и бизнес-требования

Задачи:
- Собрать и формализовать бизнес-цели (снижение времени решения, повышение CSAT, снижение нагрузки на L1).
- Сбор заинтересованных лиц: служба поддержки, ИБ, DevOps, руководители групп\отделов\управлений.
- Определить KPI/SLO/SLA (KPI: CSAT, % автоматических ответов, % эскалаций, среднее TAT).
Артефакты:
- BRD (Business Requirements Document).
- Stakeholder map, RACI
Входы: интервью с SME, статистика тикетов, SLA текущей службы поддержки.  
Выходы: утверждённые цели и KPI, список ограничений (например, запрет на передачу персональных данных третьим лицам).
Критерии приемки:
- BRD подписан всеми ключевыми стейкхолдерами.
- KPI определены и формально измеримы.

### Фаза B — Solution & System Requirements

Задачи:
- Сформулировать функциональные и нефункциональные требования: поддерживаемые каналы (чат в приложении, веб, e-mail, мессенджер), fallback/эскалация, требования к латентности, доступности, шифрованию и аудиту.
- Определить модель прав доступа к данным (RBAC), GDPR/PDPA/локальные требования банка.
Артефакты:
- Software Requirements Specification (SRS)
- Data classification matrix (типы данных, чувствительность)
Входы: BRD, регуляторные требования.
Критерии приемки:
- SRS покрывает 95% случаев использования из анализа тикетов.
- Требования утверждены ИБ, инфраструктурой.

### Фаза C — Архитектура высокого уровня и интеграция MCP

Задачи:
- Нарисовать архитектуру: каналы → фронт-енд → Orchestration/Agent layer → MCP client/server → tools/connectors → RAG pipeline (vector DB) → LLM (ChatGPT via API) → Audit & Logs → Monitoring.
- Спроектировать MCP-серверы/коннекторы (как expose data sources / tools через MCP).
- Решения по хранению векторных эмбеддингов (векторная БД), и индексам.
Артефакты:
- Архитектурные диаграммы (контейнеры, sequence diagrams)
- MCP manifest / описание коннекторов
- Таблица решений по infra (векторная БД, message broker, secrets management)
Входы: SRS, существующая инфраструктура банка.  
Выходы: утверждённая архитектура, список необходимых коннекторов.
Критерии приемки:
- Архитектура покрывает канал доставки ответов + fallback + аудит.
- MCP manifests описаны для всех критичных источников: тикеты, документы, база пользователей.

### Фаза D — RAG: подготовка данных и pipeline

Задачи:
- Экспорт исторических тикетов (метаданные, теги, вложения), очистка (PII redaction), нормализация, аннотация (категории, intent).
- Разработка ETL: тексты → chunking → embedding → vector DB (k-hub).
- Добавить retrieval signals: recency, priority, similarity thresholds.
Артефакты:
- Data-schema для RAG (fields, chunk size, metadata)
- ETL pipeline (код + infra)
- Dataset sample + data quality report
Входы: экспорт тикетов, политики хранения данных.  
Выходы: индексированная RAG-база, метрики качества.
Критерии приемки:
- PII обнаруживается и редактируется автоматически на 99% корректности (или ручная верификация для случаев сомнения).
- MRR/hit@k > целевой порог (определяется бизнесом) на валидационной выборке.

### Фаза E — Интеграция LLM

Задачи:
- Интегрировать LLM
- Подготовить prompt-templates, system messages, temperature, max tokens, safety guards.
- Интеграция с MCP: LLM вызывает MCP-tools (например, запрашивает документ, делает поиск в intranet).
Артефакты:
- API integration
- Prompt library + prompt testing corpus
- Safety policy + content filters
Входы: RAG outputs, SRS, MCP manifests.
Критерии приемки:
- 90% ответов корректны
- Latency per response не превышает заданного уровня (SLO).

### Фаза F — Диалоговый менеджмент, UX и эскалации

Задачи:
- Дизайн диалоговой логики: состояние сессии, контекст, multi-turn, handover к человеку.
- UI/UX требования для агентов поддержки (видимость источников, метрики доверия).  
Артефакты:
- Диаграммы состояний диалога, интерфейс оператора.  
Критерии приемки:
- Оператор может перехватить сессию и увидеть RAG-источники и цепочку действий.

### Фаза G — Тестирование и валидация

Задачи:
- Unit, integration, e2e tests; test harness для оценки RAG+LLM (позитивные/негативные кейсы, adversarial prompts).
- Human evaluation: blind A/B с реальными операторами.
Артефакты:
- Test plans, test cases, evaluation reports, security pen-test report.
Критерии приемки:
- Тесты покрывают 95% кода/функционала.
- Прошли security и privacy review.

### Фаза H — Развертывание и эксплуатация

Задачи:
- CI/CD
- мониторинг и алертинг
- логирование
Артефакты:
- Runbook, escalation playbooks, SLO/SLA dashboards, runbooks для инцидентов.
Критерии приемки:
- Runbook проверен на играх (game days), on-call расписание настроено.

## Архитектура — элементы и интеграции

Ключевые блоки:
- Каналы: Pyrus
- Orchestration Layer / Agent Manager: маршрутизирует запросы, отслеживает сессии, отвечает за политик хранения контекста.
- MCP layer:
	- MCP client внутри Orchestrator: обращается к MCP-серверу(ам) для чтения/выполнения инструментов.
	- MCP server(s): экспонируют источники (тикеты, внутренние документы, корпоративная файловая система, user directory).
	- Каждый источник описан manifest'ом (operations, permissions).
	- MCP используется как стандартный «мост» между LLM и инструментами/данными. Позволяет: безопасно выставлять инструменты, стандартизировать manifests, упрощать интеграцию и аудит.
	- Обязательно: permissions model (who/what tool can access), user consent, ограничение экспорта данных, sandboxing для инструментов.
	- Тестирование: имитация злонамеренных manifest-вызовов (prompt injection через tools) и проверка правил разрешений.
- RAG pipeline:
	- ETL → chunking → embeddings (model choice) → vector DB.
	- Retriever service с фильтрами и reranking.
- LLM connector:
	- Обёртка для LLM API (retry, rate-limit, caching, prompt templates).
	- Prompt engineering: system prompt с ограничениями, использование RAG context + instruction to cite docs and include confidence score.
	- Safety: hallucination detection — если ответ содержит фактические утверждения, требовать наличие RAG-source или отказ (I don't know).
- Safety & Governance:
	- Filters, redaction, hallucination detector, explainability layer (traceability: какие источники, цепочка вызовов).
- Observability & Auditing:
	- Trace per query: inputs, retrieved docs, prompts, model response, final answer, operator actions.
- Ops infra:
	- Secrets, Key management, Network segmentation, Data residency.

## RAG-проектирование

- Chunking: разбивать тикеты и вложения на фрагменты ~500–1000 токенов, сохранять метаданные (ticket_id, date, author, tags, resolution).
- Relevance signals: recency, resolved_by_rating, frequency of similar issues.
- Vett-process: human-in-the-loop проверка выборки ответов и дообучение prompt-templates.  
Metrics to track: hit@k, MRR, precision@k, rouge/f1 на извлечённой информации.

## Безопасность, комплаенс, приватность

Обязательные действия:
- PII detection & redaction (automated + manual QA).
- Data residency: хранение эмбеддингов/кейсов по банковским требованиям (on-prem vs cloud region).
- RBAC и least privilege для MCP tools.
- Auditing: immutable logs, tamper-evident storage.
- Penetration testing, threat modelling (STRIDE), review третьей стороны.  
Критерии приемки:
- SOC2 type II / internal security checklist выполнены для production.
- Pen-test критические уязвимости устранены.

## Качество и тестирование

Тесты:
- Unit + integration tests для ETL, retriever, MCP connectors.
- Regression tests for prompt outputs (snapshot tests).
- Adversarial tests: prompt injection attempts, malformed MCP manifests.
- Human eval: blind A/B (bot vs human) по наборам типичных тикетов.
Критерии приемки:
- Автоматическая точность ответов ≥ X% (определяется бизнесом).
- Уровень эскалаций ≤ целевого уровня.
- CSAT при общении с ботом не хуже baseline.

## Эксплуатация / SRE

- Мониторинг: metrics for latency, error rate, retrieval quality, cost.
- Alerting: degraded retrieval, high hallucination rate, MCP tool failures.
- Runbook: шаги на случай утечки данных, неожиданных ответов LLM, превышения затрат.
- Backups: vector DB snapshots, ETL reproducibility.
SLO:
- Availability of agent control plane ≥ X% (определить с SRE).
- 95-percentile response latency ≤ defined limit.

## Scrum Backlog — эпики, истории и acceptance criteria

Каждая story разбивается на таски dev/test/doc

### Epic 1 — Исходные данные и RAG

- Story: Как инженер данных, я хочу экспортировать и очистить 100k исторических тикетов для индексирования, чтобы подготовить RAG-источник.  
    Acceptance: ETL выполняет экспорт с 0% raw PII в индекс, sample QA = 99% корректности.
    
- Story: Как инженер, я хочу настроить embedding pipeline и загрузить 1M chunks в векторную БД.  
    Acceptance: Query latency < target, hit@5 > threshold на валидации.

### Epic 2 — MCP интеграция

- Story: Как архитектор, я хочу выставить MCP manifest для тикетов и internal docs, чтобы LLM мог безопасно запрашивать документы через MCP.  
    Acceptance: Manifest проходит security review, tool permissions работают (deny/allow) при тестах.
    
- Story: Как dev, я хочу тестовую MCP sandbox среду, чтобы эмулировать tool calls.  
    Acceptance: sandbox блокирует внешние сетевые вызовы, логирует все вызовы.

### Epic 3 — Core Agent (MVP)

- Story: Как пользователь, хочу задать вопрос в чат и получить ответ с сылкой на источник в RAG.  
    Acceptance: Ответ содержит ссылку на источник или ясное "не знаю", % ошибок ниже порога.
    
- Story: Как оператор, хочу иметь экран мониторинга с контекстом сессии и кнопкой перехвата.  
    Acceptance: Перехват работает, история видна.

### Epic 4 — Safety & Compliance

- Story: Как офицер ИБ, хочу, чтобы каждый ответ проходил фильтр на утечку PII и соответствие политике.  
    Acceptance: Любая выдача с PII блокируется и заносится в аудит.

### Epic 5 — Observability & Ops

- Story: Настроить метрики и дашборды SLO/SLI.  
    Acceptance: При сбое создаётся инцидент и выполняется runbook.
    

## Пример критериев приёмки для ключевых модулей

- RAG retrieval: для тест-сета из 500 кейсов hit@5 ≥ 0.85 и MRR ≥ 0.6 (порогы настраиваются).
- Ответы бота: на контролируемом наборе 100 реальных тикетов human eval показывает ≥ 90% корректных ответов.
- Latency: 95-й перцентиль общего TTFB (time-to-first-bot-response) < заданный порог (зависит от infra).
- Security: критических уязвимостей нет в отчёте pen-test.

## Риски

- Риск: утечка PII через ответы — mitigation: PII redaction + mandatory cite + human-in-loop.
- Риск: prompt injection через MCP tools — mitigation: strict permissions, input sanitization, review tool manifests.
- Риск: низкое качество RAG retrieval — mitigation: iterative curation, human feedback loop, reranking.
- Риск: расходы на LLM — mitigation: caching, summarization, hybrid strategy (on-prem small models for embeddings/retrieval).

## Набор артефактов, которые нужно подготовить

- BRD, SRS
- Architecture diagrams (C4), sequence diagrams
- MCP manifests & connector specs
- Data schema & ETL code (with samples)
- Prompt library & prompt test corpus
- Test plans, security assessment reports
- Runbooks, monitoring dashboards
- Backlog (Jira/CSV) + Definition of Done (DoD)
- Конфигурация CI/CD + infra as code

## MVP и эволюции

- MVP минимум: поддержка одного канала + базовая RAG + ChatGPT минимальный prompt + ручная эскалация.
- Следующие итерации: multi-channel, расширенные коннекторы (MCP), улучшение RAG, аналитика эффективности, автоматическая дообучаемость (human feedback loop).

[[Scrum Backlog (AI-бот-агент сопровождения)]]
[[SRS (Software Requirements Specification)]]
[[BRD (Business Requirements Document)]]
[[Архитектура высокого уровня (C4-стиль)]]
[[MCP Manifest]]
[[Пример ETL и Jupyter-pipeline для RAG]]
[[Security Checklist и Threat Model (STRIDE)]]

[[Техническое задание и план разработки AI бота-агента]]






## 📘 Приложение A. Глоссарий терминов и аббревиатур

Настоящий глоссарий определяет ключевые термины и аббревиатуры, используемые в проекте по созданию **AI бота-агента для службы сопровождения и технической поддержки пользователей**.  
Документ обеспечивает единое понимание терминологии всеми участниками проекта.

| **Термин / Аббревиатура**                     | **Расшифровка / Определение**                                                                                                               | **Применение в проекте**                                                                              |
| --------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------- |
| **MCP (Model Context Protocol)**              | Протокол взаимодействия моделей искусственного интеллекта с внешними системами через безопасный обмен контекстом.                           | Используется для интеграции AI-агента с корпоративными системами (HelpDesk, базы данных, API).        |
| **RAG (Retrieval-Augmented Generation)**      | Подход к генерации ответов, при котором модель сначала извлекает релевантные документы из базы знаний, а затем формирует ответ с их учётом. | Обеспечивает точные и контекстные ответы пользователям на основе истории решённых тикетов.            |
| **CSAT (Customer Satisfaction Score)**        | Показатель удовлетворённости пользователей качеством обслуживания.                                                                          | Метрика для оценки эффективности AI-агента и уровня удовлетворённости пользователей.                  |
| **KPI (Key Performance Indicator)**           | Ключевой показатель эффективности, используемый для измерения достижения целей.                                                             | Применяется для оценки успеха проекта (например, % автоматических ответов, время обработки).          |
| **SLO (Service Level Objective)**             | Целевое значение для показателя качества сервиса, входящее в SLA.                                                                           | Определяет конкретные цели по времени ответа или точности работы бота.                                |
| **SLA (Service Level Agreement)**             | Формальное соглашение об уровне предоставляемого сервиса.                                                                                   | Устанавливает гарантированные параметры качества обслуживания (время отклика, доступность).           |
| **TAT (Turnaround Time)**                     | Время обработки запроса от поступления до решения.                                                                                          | Используется для анализа эффективности поддержки и скорости работы AI-агента.                         |
| **RACI**                                      | Модель распределения ролей: Responsible, Accountable, Consulted, Informed.                                                                  | Применяется при описании ответственности участников проекта (разработчики, архитекторы, SME и др.).   |
| **SME (Subject Matter Expert)**               | Эксперт по предметной области.                                                                                                              | Помогает в разработке сценариев общения и наполнении базы знаний AI-агента.                           |
| **BRD (Business Requirements Document)**      | Документ бизнес-требований, фиксирующий цели и задачи проекта со стороны заказчика.                                                         | Является исходным артефактом для подготовки ТЗ и SRS.                                                 |
| **SRS (Software Requirements Specification)** | Спецификация требований к программному обеспечению.                                                                                         | Описывает функциональные и нефункциональные требования к системе AI-агента.                           |
| **ETL (Extract, Transform, Load)**            | Процесс извлечения, преобразования и загрузки данных.                                                                                       | Используется для подготовки данных тикетов в RAG-базу знаний.                                         |
| **STRIDE**                                    | Методика анализа угроз: Spoofing, Tampering, Repudiation, Information Disclosure, Denial of Service, Elevation of Privilege.                | Применяется для моделирования и анализа угроз безопасности архитектуры бота.                          |
| **RBAC (Role-Based Access Control)**          | Ролевая модель управления доступом.                                                                                                         | Используется для разграничения прав пользователей, администраторов и операторов в системе.            |
| **TTFB (Time To First Byte)**                 | Метрика времени до первого байта ответа от сервера.                                                                                         | Применяется для мониторинга производительности API и LLM-запросов.                                    |
| **MRR (Monthly Recurring Revenue)**           | Ежемесячный регулярный доход.                                                                                                               | В корпоративных проектах — аналог экономического эффекта или сокращения затрат за счёт автоматизации. |













### Additional materials