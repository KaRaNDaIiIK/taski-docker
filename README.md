# Taski — Менеджер задач
## 📋 О проекте
Taski — это веб-приложение для управления задачами. Проект состоит из backend на Django REST Framework и frontend на React, упакованных в Docker-контейнеры.

## ⚙️ Установка окружения для локальной разработки
Склонируйте проект в папку Dev/

bash
cd Dev
git clone https://github.com/ваш-логин/taski-docker.git
cd taski-docker

## Должна получиться такая структура:
Dev/
└── taski-docker/
    ├── .github/
    │   └── workflows/
    │       └── main.yml
    ├── backend/
    │   ├── api/
    │   ├── backend/
    │   ├── manage.py
    │   ├── Dockerfile
    │   └── requirements.txt
    ├── frontend/
    │   ├── public/
    │   ├── src/
    │   ├── Dockerfile
    │   └── package.json
    ├── gateway/
    │   ├── Dockerfile
    │   └── nginx.conf
    ├── docker-compose.yml
    ├── docker-compose.production.yml
    ├── .env.example
    ├── .gitignore
    └── README.md

## 🐍 Настройка backend (Django)
### Создайте виртуальное окружение
Запустите редактор Visual Studio Code и через меню «Файл» / «Открыть директорию» откройте папку Dev/taski-docker/.
Запустите терминал в VS Code, удостоверьтесь, что вы работаете из директории taski-docker/ (если вы работаете под Windows, убедитесь, что в терминале запущен Git Bash, а не PowerShell).

### Выполните команду для создания виртуального окружения:
Linux/macOS:
bash
python3 -m venv venv
Windows (Git Bash):

bash
python -m venv venv
После выполнения в директории taski-docker/ появится папка venv/.

### Активация виртуального окружения
В терминале перейдите в корневую директорию проекта и выполните команду:

Linux/macOS:
bash
source venv/bin/activate
Windows (Git Bash):

bash
source venv/Scripts/activate
Теперь все команды в терминале будут предваряться строкой (venv).

💡 Все дальнейшие команды для backend выполняйте с активированным виртуальным окружением.

### Обновите pip
bash
python -m pip install --upgrade pip

### Установка зависимостей backend
Находясь в корне проекта, выполните:

bash
pip install -r backend/requirements.txt

## 🐳 Запуск проекта через Docker (локально)
### Убедитесь, что Docker установлен
bash
docker --version
Создайте файл с переменными окружения
Скопируйте пример файла .env:

bash
cp .env.example .env
Отредактируйте .env, укажите свои значения (особенно SECRET_KEY).

### Запустите контейнеры
bash
docker-compose up -d

### Примените миграции
bash
docker-compose exec backend python manage.py migrate

### Соберите статику
bash
docker-compose exec backend python manage.py collectstatic
docker-compose exec backend cp -r /app/collected_static/. /backend_static/static/

### Остановка контейнеров
bash
docker-compose down

## 🧪 Запуск тестов
### Backend-тесты
bash
cd backend
python manage.py test

### Frontend-тесты
bash
cd frontend
npm ci
npm test

## 🚀 Развёртывание на сервере
### Настройка сервера
Подключитесь к серверу по SSH
Установите Docker и Docker Compose

### Создайте папку проекта:
bash
mkdir -p ~/taski
cd ~/taski
Создайте файл .env с необходимыми переменными
Скопируйте docker-compose.production.yml на сервер

### Запуск на сервере
bash
sudo docker compose -f docker-compose.production.yml pull
sudo docker compose -f docker-compose.production.yml down
sudo docker compose -f docker-compose.production.yml up -d
sudo docker compose -f docker-compose.production.yml exec backend python manage.py migrate
sudo docker compose -f docker-compose.production.yml exec backend python manage.py collectstatic
sudo docker compose -f docker-compose.production.yml exec backend cp -r /app/collected_static/. /backend_static/static/

## 🤖 CI/CD (GitHub Actions)
При каждом пуше в ветку main автоматически:
Запускаются тесты:
Backend: flake8 + Django-тесты с PostgreSQL
Frontend: npm test

### Собираются и публикуются образы на Docker Hub:
karandaiiik/taski_backend:latest
karandaiiik/taski_frontend:latest
karandaiiik/taski_gateway:latest

### Выполняется деплой на сервер:
Копирование docker-compose.production.yml
docker pull новых образов

## Перезапуск контейнеров
### Миграции и сборка статики
Отправляется уведомление в Telegram об успешном деплое.

## 🔐 Настройка секретов для GitHub Actions
В репозитории GitHub необходимо добавить следующие секреты (Settings → Secrets and variables → Actions):

### Secret	Описание
DOCKER_USERNAME	Логин на Docker Hub
DOCKER_PASSWORD	Пароль или токен доступа
HOST        	IP-адрес сервера
USER	        Имя пользователя на сервере (например, yc-user)
SSH_KEY	        Приватный SSH-ключ для подключения к серверу
SSH_PASSPHRASE	Пароль от ключа (если есть)
TELEGRAM_TO	    ID чата в Telegram
TELEGRAM_TOKEN	Токен Telegram-бота

### 🌐 Доступ к проекту
После успешного деплоя проект доступен по адресу:
https://taski-karandaiiik.ddns.net

## 📁 Структура проекта
taski-docker/
├── .github/workflows/     # CI/CD пайплайн
├── backend/               # Django backend
│   ├── api/               # API приложения
│   ├── backend/            # Настройки Django
│   ├── manage.py
│   ├── Dockerfile
│   └── requirements.txt
├── frontend/              # React frontend
│   ├── public/
│   ├── src/
│   ├── Dockerfile
│   └── package.json
├── gateway/               # Nginx gateway
│   ├── Dockerfile
│   └── nginx.conf
├── docker-compose.yml     # Локальный запуск
├── docker-compose.production.yml  # Продакшен
├── .env.example           # Пример переменных
└── README.md

## ✅ Финальный чек-лист
Склонирован репозиторий
Создано и активировано виртуальное окружение
Установлены зависимости backend
Создан файл .env из .env.example
Запущен проект через Docker
Применены миграции
Проект открывается в браузере (http://localhost:8000)
Настроены секреты в GitHub
Выполнен git push — CI/CD отработал успешно

## 🏆 Прогресс достигнут!
Ручной деплой "на железо" 
→ Docker вручную 
→ Docker Compose вручную 
→ Полный CI/CD в GitHub Actions
# Проект готов к разработке и продакшену! 🚀