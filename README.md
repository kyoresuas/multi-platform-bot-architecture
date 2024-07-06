# Multi-Platform Bot Architecture

Этот проект предоставляет удобную архитектуру для создания ботов, работающих в Telegram, Discord и VK, с поддержкой команд, которые могут быть настроены для работы на определенных платформах.

## Установка

1. Склонируйте репозиторий:
    ```bash
    git clone <URL вашего репозитория>
    cd <имя директории>
    ```

2. Установите зависимости:
    ```bash
    npm install
    ```

3. Создайте файл `.env` на основе `.env.example` и заполните его:
    ```plaintext
    # Токены для ботов
    TELEGRAM_TOKEN=<ваш_токен_телеграм>
    DISCORD_TOKEN=<ваш_токен_дискорд>
    VK_TOKEN=<ваш_токен_вк>

    # Префикс команд
    COMMAND_PREFIX=!

    # Включение/выключение ботов
    ENABLE_TELEGRAM_BOT=true
    ENABLE_DISCORD_BOT=true
    ENABLE_VK_BOT=true
    ```

## Запуск

Для запуска всех активированных ботов выполните:
```bash
npm start
```

## Создание команд

### Основная структура команды

Каждая команда должна быть определена в отдельном файле в директории `src/commands`. Пример простой команды `ping`:

```javascript
module.exports = {
  // Название команды
  name: 'ping',

  // Алиасы для команды: альтернативные способы вызова команды (можно использовать регулярные выражения)
  aliases: ['p', /^p(?:ing)?$/i],

  // Платформы, на которых команда будет доступна
  // Если platforms не указаны, команда будет работать на всех платформах

  // Ответ команды
  response: 'Pong!',
};
```

### Команда с выполнением функции

Пример команды `weather`, которая выполняет асинхронную функцию для получения погоды:

```javascript
const { getWeather } = require('../services/weatherService');

module.exports = {
  // Название команды
  name: 'weather',

  // Алиасы для команды: альтернативные способы вызова команды
  aliases: ['w'],

  // Платформы, на которых команда будет доступна
  platforms: ['telegram', 'vk'], // Команда доступна только в Telegram и VK

  // Функция выполнения команды
  async execute(context, args) {
    // Проверка наличия аргументов
    if (args.length === 0) {
      return 'Пожалуйста, укажите город.';
    }

    const city = args.join(' ');
    try {
      // Получение погоды с помощью сервиса
      const weather = await getWeather(city);
      return `Текущая погода в ${city}:\n${weather}`;
    } catch (error) {
      return 'Не удалось получить погоду. Пожалуйста, попробуйте позже.';
    }
  }
};
```

### Добавление новых команд

1. Создайте новый файл в директории `src/commands`, например, `hello.js`.
2. Определите команду в этом файле

    Пример простой команды:
    ```javascript
    module.exports = {
      // Название команды
      name: 'hello',

      // Алиасы для команды: альтернативные способы вызова команды
      aliases: ['hi', /^h(?:ello)?$/i],

      // Платформы, на которых команда будет доступна
      // Если platforms не указаны, команда будет работать на всех платформах

      // Ответ команды
      response: 'Hello, world!',
    };
    ```

    - `name`: Это основное имя команды, которое будет использоваться для вызова команды.
    - `aliases`: Это альтернативные имена или сокращения для команды. Вы можете использовать строки или регулярные выражения. Например:
      - `'hi'`: Команда будет вызываться при вводе `hi`.
      - `/^h(?:ello)?$/i`: Команда будет вызываться при вводе `h` или `hello`, независимо от регистра букв.
    - `platforms`: Это массив с именами платформ, на которых команда будет доступна. Если `platforms` не указаны, команда будет работать на всех платформах. Возможные значения:
      - `'telegram'`: Команда будет доступна в Telegram.
      - `'discord'`: Команда будет доступна в Discord.
      - `'vk'`: Команда будет доступна в VK.

3. Команда будет автоматически загружена при запуске ботов и станет доступной для использования.

## Контакты

Если у вас есть вопросы или предложения, пожалуйста, свяжитесь со мной по адресу Kyoresuas@tellery.ru.