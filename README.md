# 🔐 VulRAG – RAG-система для обнаружения и исправления уязвимостей C/C++

VulRAG — это Retrieval-Augmented Generation (RAG) система, которая:
- 🔎 **Определяет CWE-уязвимости** в коде на C/C++
- 🛠 **Генерирует исправленный фрагмент** (patch), устраняющий проблему

Модель объединяет поиск по базе уязвимых и безопасных примеров (FAISS) с генерацией кода при помощи LLM (**Qwen2.5-3B-Instruct**).

---

## 📦 Установка

```bash
git clone https://github.com/yourusername/vulrag.git
cd vulrag

pip install langchain langchain_community sentence-transformers datasets faiss-cpu torch transformers
pip install -U langchain-huggingface transformers-stream-generator accelerate
```

## 🧠 Как это работает

### 1. Загрузка и предобработка данных
- Используется датасет **Juliet Test Suite**
- Из кода извлекаются:
  - описание (`@description`)
  - CWE-метка (если есть)
  - чистый код (`#include ...`)

### 2. Создание векторного хранилища
- Эмбеддинги формируются моделью `sentence-transformers/all-MiniLM-L6-v2`
- Векторное пространство индексируется с помощью **FAISS** и сохраняется локально

### 3. RAG-инференс
- **Вход:** фрагмент C/C++-кода
- Ретривер достаёт 3 наиболее похожих примера
- **Qwen2.5-3B-Instruct** получает промпт с контекстом + кодом
- **Вывод:**
  ```text
  cwe: CWE-XXX
  fixed:
  <исправленный код>
```
> **Примечание:** Эта программа разрабатывалась и тестировалась в **Kaggle** с использованием видеокарты **NVIDIA Tesla P100**.

# Все зависимости проекта перечислены здесь и должны быть установлены через:
# pip install -r requirements.txt
