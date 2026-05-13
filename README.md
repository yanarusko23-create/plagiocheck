# 🔍 PlagioCheck — Система перевірки на плагіат

![Build Status](https://github.com/yanarusko23-create/plagiocheck/actions/workflows/main.yml/badge.svg)
![License](https://img.shields.io/badge/license-MIT-green)
![Docker](https://img.shields.io/badge/docker-ready-blue)

PlagioCheck — вебсервіс для автоматичної перевірки текстових документів на плагіат.  
Система порівнює завантажений документ із індексованою базою джерел і генерує детальний звіт із відсотком збігів.

---

## 📦 Швидкий старт

### Передумови

- [Docker](https://www.docker.com/) ≥ 24.x
- [Docker Compose](https://docs.docker.com/compose/) ≥ 2.x

### Запуск через Docker Compose

```bash
# 1. Клонувати репозиторій
git clone https://github.com/your-username/plagiocheck.git
cd plagiocheck

# 2. Скопіювати файл змінних середовища
cp .env.example .env

# 3. Запустити всі сервіси
docker compose up --build -d
```

Додаток буде доступний за адресою: http://localhost:8080

### Зупинка сервісів

```bash
docker compose down
```

---

## 🏗️ Структура проєкту

```
plagiocheck/
├── .github/
│   └── workflows/
│       └── main.yml          # CI/CD pipeline
├── src/
│   ├── api/                  # REST контролери
│   ├── services/             # Бізнес-логіка (Algorithm, Report)
│   ├── models/               # Сутності (Document, Source, Report)
│   └── utils/                # Допоміжні утиліти
├── tests/
│   ├── unit/
│   └── integration/
├── docker-compose.yml
├── Dockerfile
├── .env.example
└── README.md
```

---

## 👨‍💻 Для розробників

Цей розділ описує правила і процеси для всіх, хто хоче зробити внесок у проєкт.

### Налаштування локального середовища

```bash
# Встановити залежності (Python 3.11+)
pip install -r requirements-dev.txt

# Запустити сервер у режимі розробки (без Docker)
python -m uvicorn src.main:app --reload --port 8080
```

### Гілки та стратегія роботи з Git

| Гілка | Призначення |
|---|---|
| `main` | Стабільна production-версія. Пряме злиття **заборонено**. |
| `develop` | Основна гілка для розробки. |
| `feature/<назва>` | Нові функції (відгалуження від `develop`). |
| `fix/<назва>` | Виправлення помилок. |
| `hotfix/<назва>` | Критичні виправлення у production. |

**Приклад:**
```bash
git checkout develop
git checkout -b feature/add-url-source-indexing
```

### Правила оформлення комітів

Використовуємо стандарт **Conventional Commits**:

```
<тип>(<область>): <короткий опис>

[необов'язкове детальне пояснення]

[необов'язкове посилання на задачу: Closes #42]
```

**Допустимі типи:**

| Тип | Використання |
|---|---|
| `feat` | Нова функціональність |
| `fix` | Виправлення помилки |
| `docs` | Зміни в документації |
| `refactor` | Рефакторинг без зміни поведінки |
| `test` | Додавання або виправлення тестів |
| `chore` | Технічні зміни (залежності, конфіги) |

**Приклади хороших комітів:**
```bash
git commit -m "feat(algorithm): add cosine similarity scoring for source comparison"
git commit -m "fix(report): correct percentage rounding in getSimilarity() method"
git commit -m "docs(readme): update developer setup instructions"
```

**Приклади поганих комітів** (так не робити):
```bash
git commit -m "fix"
git commit -m "changed stuff"
git commit -m "WIP"
```

### Процес внесення змін (Pull Request)

1. Переконайтесь, що у вас є завдання (issue) на GitHub Projects.
2. Відгалужте гілку від `develop` за шаблоном `feature/<назва>`.
3. Напишіть або оновіть тести для нової функціональності.
4. Переконайтесь, що всі тести проходять локально:
   ```bash
   pytest tests/ -v
   ```
5. Переконайтесь, що лінтер не виявляє помилок:
   ```bash
   flake8 src/ && black --check src/
   ```
6. Відкрийте Pull Request у `develop`. Назва PR має відповідати формату комітів.
7. PR вимагає як мінімум **одного схвалення** від іншого розробника.
8. Після схвалення — **squash and merge** у `develop`.

> ⚠️ Прямі коміти у гілки `main` та `develop` заблоковані правилами захисту гілок.

### Стандарти написання коду

- **Мова коду**: англійська (назви змінних, коментарі, документація).
- **Форматування**: `black` з довжиною рядка 88 символів.
- **Лінтинг**: `flake8` без попереджень.
- **Типізація**: обов'язкові анотації типів для всіх публічних функцій та методів.
- **Документаційні рядки**: Google-style docstrings для всіх класів і публічних методів.

```python
def analyze(self, document: Document) -> Report:
    """Analyze a document for plagiarism.

    Args:
        document: The Document entity to be checked.

    Returns:
        A Report entity containing similarity scores and matched sources.

    Raises:
        ValueError: If the document content is empty.
    """
    ...
```

### Тестування

```bash
# Всі тести
pytest tests/ -v

# Тільки юніт-тести
pytest tests/unit/ -v

# З покриттям
pytest tests/ --cov=src --cov-report=html
```

Мінімальне порогове значення покриття коду: **80%**.  
Перевірка покриття автоматично виконується у CI.

### Оновлення залежностей

```bash
# Додати нову залежність
pip install <пакет>
pip freeze > requirements.txt

# Оновити requirements-dev.txt (dev-залежності)
pip install <пакет> --dev
```

> Не додавайте dev-залежності (pytest, black, flake8) до основного `requirements.txt`.

---

## 🔒 Безпека

Якщо ви виявили вразливість безпеки, **не** відкривайте публічний issue.  
Надішліть повідомлення безпосередньо: security@plagiocheck.example.com

---

## 📄 Ліцензія

MIT License © 2026 Yana Rusko
