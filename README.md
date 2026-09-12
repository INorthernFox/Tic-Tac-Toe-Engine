# Tic-Tac-Toe Engine

Правила игры, контракты и бот. **Ни одной ссылки на Unity.**

Крестики-нолики, в которых у каждого игрока на поле не больше трёх знаков: ставишь
четвёртый — самый старый исчезает.

## Что внутри

| Сборка | Что это |
|---|---|
| `TicTacToe.Contracts` | команды, события, снимок состояния. Не зависит ни от кого |
| `TicTacToe.Engine` | доска, очереди знаков, вытеснение, определение победы |
| `TicTacToe.Bot` | стратегии бота поверх той же модели |
| `TicTacToe.Engine.Tests` | тесты правил |

У всех трёх runtime-сборок в asmdef стоит `noEngineReferences: true` — независимость
от Unity обеспечивается сборкой, а не обещанием.

## Подключение

В разработке — подмодулем:

```bash
git submodule add git@github.com:INorthernFox/Tic-Tac-Toe-Engine.git Packages/com.tictactoe.engine
```

В любой другой Unity-проект — строкой в `Packages/manifest.json`:

```json
"com.tictactoe.engine": "https://github.com/INorthernFox/Tic-Tac-Toe-Engine.git#v0.1.0"
```

Чтобы тесты пакета появились в Unity Test Runner, проект должен объявить его в `testables`.

## Без Unity

```bash
dotnet build "Standalone~/TicTacToe.Engine.Standalone.csproj"
dotnet test  "Standalone~/TicTacToe.Engine.Tests.Standalone.csproj"
```

Первая команда — проверка, что движок действительно не знает про Unity: если он собрался
обычным .NET, значит ссылок нет. Вторая гоняет те же тесты, что и Unity Test Runner,
без редактора и без лицензии.

Проекты лежат в папке с `~` на конце — такие папки Unity не импортирует. Туда же,
в `artifacts~/`, уходит вывод сборки: иначе Unity подхватил бы готовую DLL и получил
дубликат типов.

## Правила языка

`netstandard2.1`, C# 9 — профиль, который понимает Unity 6. `record struct`, primary
constructors и коллекционные выражения не используются: они не соберутся в редакторе.
