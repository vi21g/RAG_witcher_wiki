# RAG witcher wiki

## Введение

Данный проект реализует систему Retrieval-Augmented Generation (RAG) для работы с базой знаний, собранной из спарсенных страниц по миру "Ведьмака" Анджея Сапковского. Система использует фреймворк LlamaIndex для организации базы данных и поиска релевантной информации, а также ChatGPT 4.0 Mini в качестве языковой модели (LLM) для генерации ответов на запросы пользователя.

Процесс трассировки и мониторинга работы системы осуществляется с помощью инструмента Arize Phoenix, что позволяет улучшить качество и эффективность системы путем анализа ее работы в реальном времени.

## Этапы работы

### База знаний

1.   С [сайта](https://vedmak.fandom.com/) парсим urls
2.   По спарсенным ссылкам парсим страницы
3.   Сохраняем в txt базу знаний с разбивкой по нужным категориям

### LLM

1.   Подключаем Arize Phoenix для трассировки
2.   Разбиваем текст на узлы (nodes) и загружаем в docstore
3.   Задаём настройки для работы с openai api

### Pipeline в зависимости от потребностей:

1.  query → BM25Retriever → n docs → LLM prompt → response
2.  query → BM25Retriever → n docs → Colbert Rerank → n/2 docs → LLM prompt → response
3.  query → NemoGuardrails → BM25Retriever → n docs → Colbert Rerank → n/2 docs → LLM prompt → response

### Подход KnowledgeMaps:
1. Разбивка на мелкие базы знаний
2. query → LLM prompt →  answer
   
     2.1  answer 1 → query → BM25Retriever → n docs from docstore_1 → Colbert Rerank → n/2 docs → LLM prompt → response
   
     2.2  answer 2 → query → BM25Retriever → n docs from docstore_2 → Colbert Rerank → n/2 docs → LLM prompt → response
