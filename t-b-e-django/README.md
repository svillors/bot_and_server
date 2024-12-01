# 📄 **Telegram-бот для проведения интерактивных мероприятий**

---

## 🛠 **Команда для запуска бота**

1. Создайте виртуальное окружение командой:

1.1 
```plaintext
python3 -m venv env
```

1.2
Для МакОС
```bash
source env/bin/activate
```

Для Виндовс
```bash
. env\Scripts\activate
```

2. Установие все необходимые библиотеки командой

```bash
   pip install -r requirements.txt
```

3. Создайте в директории файл с расширением .env

4. Внутри .env нужно вписать:

```plaintext
   SECRET_KEY=ваш_секректный ключ
   DEBUG=True
   TELEGRAM_BOT_TOKEN=ваш_тг_апи
```

(SECRET_KEY=django-insecure-dvcr(o^1o(6pu(obewasy3ev*l%-2l)ryol6d10b&%7^cxz@#k)

5. Перейдите в директорию проект - BOT_PROJECT 

```bash
cd путь/к/BOT_PROJECT
```

6. Сделайте миграции
```bash
   python manage.py makemigrations
   python manage.py migrate
```

7. Запустите бота
```bash
python event_planner/bot.py
```

Если все пошло по плану, в консоли должно отобразиться сообщение "Бот запущен" -> Можно заходить в телеграм и тестировать


---

## ⚙️ **Основные компоненты**

### 📌 **bot.py**

- Настраивает Django и подключает необходимые модули.
- Создаёт объект `TeleBot` с помощью токена из переменной окружения `TELEGRAM_BOT_TOKEN`.
- Определяет основные команды и обработчики сообщений:
  - `/start`: Регистрация пользователя в базе и настройка его роли (слушатель/докладчик).
  - `⚙️ О программе`: Отображает расписание мероприятий.
  - `📝 Задать вопрос`: Позволяет слушателю выбрать докладчика и задать ему вопрос.
  - `📜 Посмотреть вопросы`: Показывает докладчику список вопросов.
  - `💷 Донат`: Отправляет информацию о реквизитах для доната.

---

### 📌 **helpers.py**

Содержит вспомогательные функции:
- Создание клавиатур (`create_reply_keyboard`, `create_inline_keyboard`).
- Логика для проверки текста команд (`is_about_command`, `is_ask_question_command` и др.).
- Управление состояниями пользователей (`user_states`).

---

### 📌 **admin.py**

Добавляет поддержку моделей через Django Admin:
- **Модели**: `Event`, `Speaker`, `Session`, `SpeakerSession`, `User`, `Question`, `Organizer`.
- Реализует действие рассылки уведомлений через команду админа: `send_message_to_all_users`.

---

### 📌 **models.py**

Описывает модели для хранения данных:
- **Event**: Мероприятие с названием, датой и местом.
- **Speaker**: Докладчик с его именем и стэком технологий.
- **Session**: Сессия с привязкой к мероприятию.
- **User**: Пользователь Telegram с его ролью.
- **Question**: Вопросы пользователей докладчикам.
- **Organizer**: Организатор с информацией о его реквизитах.

---

### 📌 **utils.py**

Содержит утилитарные функции, такие как:
- `get_schedule`: Возвращает расписание мероприятий.
- `remove_expired_speakers`: Удаляет старых докладчиков.

---

### 📌 **migrations/**

Папка миграций для управления изменениями в структуре базы данных.

---

### 📌 **settings.py**

Файл конфигурации Django:
- Настройки базы данных.
- Установленные приложения.
- Конфигурация для интеграции с Telegram.

---

## 📚 **Пример использования**

### 1️⃣ **Начало работы**

Пользователь отправляет команду `/start`. Бот:
- Регистрирует пользователя в базе.
- Присваивает роль (слушатель или докладчик).
- Отображает приветственное сообщение с клавиатурой.

---

### 2️⃣ **Выдача расписания**

Пользователь нажимает `⚙️ О программе`. Бот:
- Получает расписание через функцию `get_schedule`.
- Отправляет его в чат.

---

### 3️⃣ **Задание вопроса**

Пользователь нажимает `📝 Задать вопрос`. Бот:
- Показывает список докладчиков.
- После выбора докладчика сохраняет текст вопроса в базу.

---

### 4️⃣ **Просмотр вопросов**

Докладчик нажимает `📜 Посмотреть вопросы`. Бот:
- Извлекает вопросы из базы и отправляет их в чат.

---

### 5️⃣ **Отправка доната**

Пользователь нажимает `💷 Донат`. Бот:
- Проверяет, указан ли номер карты организатора.
- Отправляет информацию о реквизитах.

---

## 🔧 **Особенности**

1. **Управление состояниями**:
   Используется словарь `user_states` для хранения текущих действий пользователей.

2. **Кастомные клавиатуры**:
   - **ReplyKeyboardMarkup** для основных кнопок.
   - **InlineKeyboardMarkup** для выбора докладчиков.

3. **Django ORM**:
   Используется для работы с базой данных, в том числе:
   - Создание и выбор пользователей/вопросов.
   - Фильтрация данных.

---

## 📋 **Команды и кнопки**

| **Команда/Кнопка**     | **Описание**                                   |
|-------------------------|-----------------------------------------------|
| `/start`               | Регистрация и отображение клавиатуры.         |
| `⚙️ О программе`       | Отображение расписания.                       |
| `📝 Задать вопрос`      | Выбор докладчика и задание вопроса.           |
| `📜 Посмотреть вопросы` | Просмотр вопросов докладчика.                 |
| `💷 Донат`             | Отправка реквизитов для доната.               |

---

## 🧹 **Обслуживание**

1. **Миграции**:
   Создание и применение миграций:
   ```bash
   python manage.py makemigrations
   python manage.py migrate
   ```

2. **Админ-панель**:
   Создание суперпользователя:
   ```bash
   python manage.py createsuperuser
   ```

3. **Логирование ошибок**:
   Все исключения выводятся в консоль для отладки.