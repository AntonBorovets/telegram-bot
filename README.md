# Telegram Bot — RedBot/Node-RED Test Task

## How to run

1. Deploy RedBot following the docs: https://red-bot.io/
2. Open the Node-RED editor: http://localhost:1880
3. Import `flows.json` (Menu → Import → select file).
4. Create a bot via @BotFather in Telegram, get the token.
5. Open the **Telegram Receiver** and **Telegram Sender** nodes, paste the token into the token field (replace `YOUR_TELEGRAM_TOKEN`).
6. Click **Deploy**.
7. Message the bot with `/start` to begin.

## What's implemented

- Main menu with inline keyboard: Calculator, Exchange Rate, About bot, Operator/SOS.
- Every branch returns to the main menu via a "⬅ Back to menu" button.
- `/start` resets the user to the beginning at any point.
- Fallback message + menu re-display for unrecognized input.
- Calculator (custom Function node, JavaScript):
  - Input format: `number operator number`, e.g. `12 + 7` (supports `+ - * / x × ÷`, comma or dot as decimal separator).
  - Validates: non-numeric input, division by zero, empty input, input longer than 50 characters — each returns a clear message instead of crashing.
- Exchange rate branch: HTTP request to NBU API (`https://bank.gov.ua/NBUStatService/v1/statdirectory/exchange?json`), parses USD/EUR, shows rate + date in human-readable form.
- Support ticket generation on operator request.
- Unified Logger function node: builds a structured log entry (timestamp, chatId, step, status, summary) and outputs it via `node.warn()` for every key step (calc input, calc result, calc validation errors, NBU request/response/error).

## Known issues / not done

- **Debug output not visible in Node-RED debug sidebar**: the Unified Logger node calls `node.warn(...)` with the structured log line at every key step, and the code itself is verified correct (confirmed via direct testing of upstream nodes). However, no entries appear in the debug sidebar even with debug nodes enabled, unpaused, and set to "all nodes" — including a test debug node connected directly to the Telegram Receiver output with no other logic in between. Tried: re-enabling debug nodes, checking pause state, filter settings, refreshing the browser, testing at multiple points in the flow. Root cause not identified within the time limit. Debug sidebar visibility was tested extensively (enabling debug nodes, checking pause/filter state, testing at multiple points in the flow), but the underlying RedBot/Node-RED process terminal was not checked directly due to time constraints — it remains possible that `node.warn()` output is reaching stdout even though it doesn't appear in the UI sidebar. Log point placement (chatId, step, status, summary) and the code are implemented as intended; only sidebar/console visibility is unresolved.

## Time spent

~3.5 hours

## Checklist

| Пункт                                       | Статус | Коментар                                                                   |
| ------------------------------------------- | ------ | -------------------------------------------------------------------------- |
| RedBot розгорнуто, бот відповідає на /start | ✅     |                                                                            |
| Меню з 3 пунктів + кнопка «Назад»           | ✅     |                                                                            |
| Fallback на нерозпізнаний ввід              | ✅     |                                                                            |
| Калькулятор рахує коректно                  | ✅     |                                                                            |
| Валідація: нечислове значення               | ✅     |                                                                            |
| Валідація: ділення на нуль                  | ✅     |                                                                            |
| Валідація: порожній / задовгий ввід         | ✅     |                                                                            |
| Курс валют з API НБУ                        | ✅     |                                                                            |
| Обробка недоступності API                   | ✅     |                                                                            |
| Логи: успішний сценарій                     | ⚠️     | Код логування є, вивід у debug sidebar не підтверджено (див. Known issues) |
| Логи: некоректний ввід                      | ⚠️     | Те саме                                                                    |
| Логи: збій API                              | ⚠️     | Те саме                                                                    |
| README англійською                          | ✅     |                                                                            |
