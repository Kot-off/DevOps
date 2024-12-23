**Forwarded from [Антон Вяткин](https://t.me/vuekot)**

Конечно! Я покажу тебе пример простого Telegram-бота на языке Go, который будет использовать меню, авторизацию пользователей и дополнительные функции. Мы будем использовать библиотеку `telegram-bot-api`, которая предоставляет удобный интерфейс для работы с API Telegram.

### Шаг 1: Установка зависимостей

Для начала установим необходимые зависимости:

```
go get github.com/go-telegram-bot-api/telegram-bot-api/v5
```
Это установит последнюю версию библиотеки `telegram-bot-api`.

### Шаг 2: Структура проекта

Создадим структуру проекта:

```
bot/
├── main.go
└── auth.go
```
- **main.go** – основной файл программы, где мы настроим бота и обработчики команд.
- **auth.go** – файл для управления аутентификацией пользователей.

### Шаг 3: Кодирование

#### Файл `auth.go`

Этот файл будет содержать логику для хранения и проверки состояния авторизации пользователей.

```
package main

import (
"sync"
)

// User представляет информацию о пользователе
type User struct {
ID        int64
IsAdmin   bool
IsAuthenticated bool
}

var users = make(map[int64]*User)
var mu sync.RWMutex

// Authenticate проверяет, является ли пользователь аутентифицированным
func Authenticate(userID int64) bool {
mu.RLock()
defer mu.RUnlock()
user, ok := users[userID]
if !ok || !user.IsAuthenticated {
  return false
}
return true
}

// AddUser добавляет нового пользователя
func AddUser(userID int64, isAdmin bool) *User {
mu.Lock()
defer mu.Unlock()
user := &User{
  ID: userID,
  IsAdmin: isAdmin,
}
users[userID] = user
return user
}

// SetAuthentication устанавливает статус аутентификации пользователя
func SetAuthentication(userID int64, authenticated bool) {
mu.Lock()
defer mu.Unlock()
if user, ok := users[userID]; ok {
  user.IsAuthenticated = authenticated
} else {
  AddUser(userID, false).IsAuthenticated = authenticated
}
}
```
#### Файл `main.go`

Теперь создадим сам бот:

```go
package main

import (
"fmt"
"log"
"os"
"strings"

tb "github.com/go-telegram-bot-api/telegram-bot-api/v5"
)

const token = "<YOUR_TELEGRAM_BOT_TOKEN>"

func main() {
bot, err := tb.NewBotAPI(token)
if err != nil {
  log.Fatalf("Ошибка при создании бота: %v", err)
}

log.Printf("Авторизовался на аккаунте %s", bot.Self.UserName)

u := tb.NewUpdate(0)
u.Timeout = 60

updates, err := bot.GetUpdatesChan(u)

for update := range updates {
  if update.Message == nil { // игнорируем любые обновления без сообщений
   continue
  }

  msg := update.Message
  userID := msg.From.ID

  switch strings.ToLower(msg.Text) {
  case "/start":
   handleStart(bot, msg)
  case "/login":
   handleLogin(bot, msg)
  case "/menu":
   handleMenu(bot, msg)
  default:
   handleDefault(bot, msg)
  }
}
}

func handleStart(bot *tb.BotAPI, msg *tb.Message) {
text := fmt.Sprintf("Приветствую, %s! Это мой тестовый бот.", msg.From.FirstName)
sendMessage(bot, msg.Chat.ID, text)
}

func handleLogin(bot *tb.BotAPI, msg *tb.Message) {
userID := msg.From.ID
SetAuthentication(userID, true)
text := "Вы успешно вошли."
sendMessage(bot, msg.Chat.ID, text)
}

func handleMenu(bot *tb.BotAPI, msg *tb.Message) {
if !Authenticate(msg.From.ID) {
  text := "Пожалуйста, войдите через команду /login перед использованием меню."
  sendMessage(bot, msg.Chat.ID, text)
  return
}

keyboard := [][]tb.KeyboardButton{
  []tb.KeyboardButton{tgbtn("Команда 1"), tgbtn("Команда 2")},
  []tb.KeyboardButton{tgbtn("Команда 3"), tgbtn("Команда 4")},
}
replyMarkup := tb.ReplyKeyboardMarkup{
  Keyboard:        keyboard,
  ResizeKeyboard:  true,
  OneTimeKeyboard: true,
  Selective:       true,
}

sendMessageWithMarkup(bot, msg.Chat.ID, "Меню:", replyMarkup)
}

func handleDefault(bot *tb.BotAPI, msg *tb.Message) {
text := "Неизвестная команда. Попробуйте /start или /menu."
sendMessage(bot, msg.Chat.ID, text)
}

func sendMessage(bot *tb.BotAPI, chatID int64, text string) {
msg := tb.NewMessage(chatID, text)
_, _ = bot.Send(msg)
}

func sendMessageWithMarkup(bot *tb.BotAPI, chatID int64, text st...

---
Время:18:54:22 | Дата: 03.11.2024
