# Модуль 6: Теория — Наблюдаемость и Production QA

## 6.1 Стек наблюдаемости
| Инструмент | Тип | QA использует для |
|-----------|-----|------------------|
| **Langfuse** | AI-специфичный | Трейсы, промпты, оценки |
| **Prometheus** | Метрики | Латентность, ошибки, ресурсы |
| **Grafana** | Дашборды | Визуальный мониторинг |
| **Loki/Логи** | Логирование | Отладка ошибок |

## 6.2 Langfuse: что содержит трейс
Metadata (user_id, app, timestamp) → Spans: prompt_resolution → tool_planning → mcp_retrieval → response_generation → Total duration

### QA-использование Langfuse
- Отладка неверного ответа → проверить retrieved документы
- Отладка медленного ответа → найти самый медленный span
- Проверка выбора инструмента → какие MCP tools вызваны
- Проверка промпта → правильная версия?
- Проверка доступа → конфиденциальные документы не должны появляться

## 6.3 Ключевые метрики Prometheus
| Метрика | Порог тревоги |
|---------|-------------|
| llm_request_duration (p95) | > 10s |
| llm_errors_total (rate) | > 5% |
| vllm_gpu_memory_usage | > 90% |
| ingestion_queue_length | > 100 |

## 6.4 Workflow отладки
```
1. Найти трейс в Langfuse
2. Проверить retrieval (правильные документы?)
3. Проверить LLM input (контекст полный? промпт верный?)
4. Проверить LLM output (соответствует документам?)
5. Проверить access control
6. Задокументировать с trace ID
```

## 6.5 Чеклисты для Production
**Pre-release:** версия промпта, golden set регрессия, spot check качества, baseline латентности
**Post-release:** 1ч: error rate, 24ч: жалобы пользователей, 1 нед: тренды

> 🌐 [English version](../module-06-observability-debugging/theory.md)