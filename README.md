# myapp

Учебный Go-проект, демонстрирующий правильную структуру раскладки кода
по практическому занятию №2 (дисциплина «Технологии создания программного
обеспечения»).

## Что это

Минимальный HTTP-сервис на стандартной библиотеке `net/http` со следующей
структурой:

myapp/
├── cmd/
│ └── myapp/
│ └── main.go # точка входа: только вызов app.Run()
├── internal/
│ └── app/
│ ├── app.go # сборка mux, middleware, запуск сервера
│ └── handlers/
│ └── ping.go # хендлер /ping (Упр. C)
├── utils/
│ ├── logger.go # LogRequest / LogInfo / LogError / NewID16
│ └── httpjson.go # WriteErr — единый формат ошибок (Упр. B)
├── go.mod
├── .gitignore
└── README.md


**Эндпоинты:**

| Метод | Путь    | Ответ                                                    |
|-------|---------|----------------------------------------------------------|
| GET   | `/`     | `text/plain` — `Hello, Go project structure!`            |
| GET   | `/ping` | `application/json` — `{"status":"ok","time":"..."}`      |
| GET   | `/fail` | `application/json` — `{"error":"bad_request_example"}` (400) |

**Особенности:**
- Логирование каждого запроса (время, адрес, метод, путь).
- Middleware `withRequestID`: генерирует `X-Request-Id`, если клиент не передал свой, и возвращает его в заголовке ответа.
- Единый формат ошибок через `utils.WriteErr`.

## Требования

- Go ≥ 1.21 (`go version`)
- Git (`git --version`)
- Опционально: VS Code + расширение Go от Google, либо GoLand

## Запуск

```bash
# из корня проекта
go run ./cmd/myapp

Сервер слушает :8080. Проверка:

curl -i http://localhost:8080/
curl -i http://localhost:8080/ping
curl -i http://localhost:8080/fail
curl -i -H "X-Request-Id: demo-123" http://localhost:8080/ping

Сборка бинарника
Windows:
go build -o bin\myapp.exe ./cmd/myapp
./bin\myapp.exe

macOS / Linux:
go build -o bin/myapp ./cmd/myapp
./bin/myapp

Git
git init
git add .
git commit -m "PZ2: base project layout with logger and /ping"
