# Effective Mobile — Test Task

Простое веб-приложение на Python, развернутое в Docker с использованием Nginx в качестве reverse proxy.

## Как работает схема:
1. Пользователь отправляет HTTP-запрос на порт 80 хост-машины ("http://localhost" / "http://<VM_IP_ADDR>" при развертывани на ВМ)
2. Запрос попадает в контейнер `nginx`, который работает как reverse proxy
3. Nginx читает конфигурацию, находит `upstream backend` и перенаправляет запрос по внутреннему имени сервиса Docker (`http://backend:8080`)
4. Контейнер `backend` (Python HTTP-сервер) обрабатывает запрос и возвращает ответ
5. Nginx передает ответ обратно пользователю, добавляя необходимые заголовки (`X-Real-IP`, `X-Forwarded-For` и др.)

## Использованный стек:
- **Backend:** Python 3.12 (http.server)
- **Reverse Proxy:** Nginx 1.27 (Alpine)
- **Containerization:** Docker + Docker Compose
- **Networking:** Docker bridge network (изоляция backend)

## Запуск
### 1. Клонировать репозиторий
```bash
    git clone <repo-url>
    cd effective-mobile-test
```

### 2. Собрать и запустить
```bash
    docker compose up -d --build
```

### 3. Проверить работоспособность
```bash
    # С хоста:
    curl http://localhost
    # Ожидаемый ответ: Hello from Effective Mobile!

    #Если запускать все на виртуальной машины:
    curl http://<VM_IP_ADDR>

    # Проверить, что backend НЕ доступен напрямую с хоста
    curl http://localhost:8080
    # Ожидаемый ответ: Connection refused
```

### 4. Остановить 
```bash
    docker compose down
```