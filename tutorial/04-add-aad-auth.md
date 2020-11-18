---
ms.openlocfilehash: 05b3223967bf2a6d321c00cca079cc5c5b8ff0db
ms.sourcegitcommit: e0d9b18d2d4cbeb4a48890f3420a47e6a90abc53
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/17/2020
ms.locfileid: "49347878"
---
<!-- markdownlint-disable MD002 MD041 -->

В этом упражнении вы будете использовать **Оауспромпт** платформы Bot для реализации проверки подлинности в Bot и получения маркеров доступа для вызова API Microsoft Graph.

1. Откройте **./appsettings.js** и внесите следующие изменения.

    - Измените значение `MicrosoftAppId` на идентификатор приложения, на который будет зарегистрирована регистрация приложения "робот" в **календаре** .
    - Измените значение `MicrosoftAppPassword` на секрет клиента клиента для клиента почтового **календаря Bot** .
    - Добавьте значение с именем `ConnectionName` `GraphBotAuth` .

    :::code language="json" source="../demo/GraphCalendarBot/appsettings.example.json":::

    > [!NOTE]
    > Если вы использовали значение, отличное от `GraphBotAuth` имени записи в **параметрах подключения OAuth** на портале Azure, используйте это значение для этой `ConnectionName` записи.

## <a name="implement-dialogs"></a>Реализация диалоговых окон

1. Создайте новый каталог в корне **диалогового окна** проекта с именем. Создайте новый файл в каталоге **./диалогс** с именем **LogoutDialog.CS** и добавьте следующий код.

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/LogoutDialog.cs" id="LogoutDialogSnippet":::

    Это диалоговое окно предоставляет базовый класс для всех других диалоговых окон в Bot, от которых наследуется. Это позволяет пользователю выйти из независимо от того, где они находятся в диалоговых окнах ленты.

1. Создайте новый файл в каталоге **./диалогс** с именем **MainDialog.CS** и добавьте следующий код.

    ```csharp
    using System.Collections.Generic;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.Bot.Builder;
    using Microsoft.Bot.Builder.Dialogs;
    using Microsoft.Bot.Builder.Dialogs.Choices;
    using Microsoft.Bot.Schema;
    using Microsoft.Extensions.Configuration;
    using Microsoft.Extensions.Logging;

    namespace CalendarBot.Dialogs
    {
        public class MainDialog : LogoutDialog
        {
            const string NO_PROMPT = "no-prompt";
            protected readonly ILogger _logger;

            public MainDialog(IConfiguration configuration, ILogger<MainDialog> logger)
                : base(nameof(MainDialog), configuration["ConnectionName"])
            {
                _logger = logger;

                // OAuthPrompt dialog handles the authentication and token
                // acquisition
                AddDialog(new OAuthPrompt(
                    nameof(OAuthPrompt),
                    new OAuthPromptSettings
                    {
                        ConnectionName = ConnectionName,
                        Text = "Please login",
                        Title = "Login",
                        Timeout = 300000, // User has 5 minutes to login
                    }));

                AddDialog(new ChoicePrompt(nameof(ChoicePrompt)));

                AddDialog(new WaterfallDialog(nameof(WaterfallDialog), new WaterfallStep[]
                {
                    LoginPromptStepAsync,
                    ProcessLoginStepAsync,
                    PromptUserStepAsync,
                    CommandStepAsync,
                    ProcessStepAsync,
                    ReturnToPromptStepAsync
                }));

                // The initial child Dialog to run.
                InitialDialogId = nameof(WaterfallDialog);
            }

            private async Task<DialogTurnResult> LoginPromptStepAsync(
                WaterfallStepContext stepContext,
                CancellationToken cancellationToken)
            {
                // If we're going through the waterfall a second time, don't do an extra OAuthPrompt
                var options = stepContext.Options?.ToString();
                if (options == NO_PROMPT)
                {
                    return await stepContext.NextAsync(cancellationToken: cancellationToken);
                }

                return await stepContext.BeginDialogAsync(nameof(OAuthPrompt), null, cancellationToken);
            }

            private async Task<DialogTurnResult> ProcessLoginStepAsync(
                WaterfallStepContext stepContext,
                CancellationToken cancellationToken)
            {
                // If we're going through the waterfall a second time, don't do an extra OAuthPrompt
                var options = stepContext.Options?.ToString();
                if (options == NO_PROMPT)
                {
                    return await stepContext.NextAsync(cancellationToken: cancellationToken);
                }

                // Get the token from the previous step. If it's there, login was successful
                if (stepContext.Result != null)
                {
                    var tokenResponse = stepContext.Result as TokenResponse;
                    if (!string.IsNullOrEmpty(tokenResponse?.Token))
                    {
                        await stepContext.Context.SendActivityAsync(
                            MessageFactory.Text("You are now logged in."), cancellationToken);
                        return await stepContext.NextAsync(null, cancellationToken);
                    }
                }

                await stepContext.Context.SendActivityAsync(
                    MessageFactory.Text("Login was not successful please try again."), cancellationToken);
                return await stepContext.EndDialogAsync();
            }

            private async Task<DialogTurnResult> PromptUserStepAsync(
                WaterfallStepContext stepContext,
                CancellationToken cancellationToken)
            {
                var options = new PromptOptions
                {
                    Prompt = MessageFactory.Text("Please choose an option below"),
                    Choices = new List<Choice> {
                        new Choice { Value = "Show token" },
                        new Choice { Value = "Show me" },
                        new Choice { Value = "Show calendar" },
                        new Choice { Value = "Add event" },
                        new Choice { Value = "Log out" },
                    }
                };

                return await stepContext.PromptAsync(
                    nameof(ChoicePrompt),
                    options,
                    cancellationToken);
            }

            private async Task<DialogTurnResult> CommandStepAsync(
                WaterfallStepContext stepContext,
                CancellationToken cancellationToken)
            {
                // Save the command the user entered so we can get it back after
                // the OAuthPrompt completes
                var foundChoice = stepContext.Result as FoundChoice;
                // Result could be a FoundChoice (if user selected a choice button)
                // or a string (if user just typed something)
                stepContext.Values["command"] = foundChoice?.Value ?? stepContext.Result;

                // There is no reason to store the token locally in the bot because we can always just call
                // the OAuth prompt to get the token or get a new token if needed. The prompt completes silently
                // if the user is already signed in.
                return await stepContext.BeginDialogAsync(nameof(OAuthPrompt), null, cancellationToken);
            }

            private async Task<DialogTurnResult> ProcessStepAsync(
                WaterfallStepContext stepContext,
                CancellationToken cancellationToken)
            {
                if (stepContext.Result != null)
                {
                    var tokenResponse = stepContext.Result as TokenResponse;

                    // If we have the token use the user is authenticated so we may use it to make API calls.
                    if (tokenResponse?.Token != null)
                    {
                        var command = ((string)stepContext.Values["command"] ?? string.Empty).ToLowerInvariant();

                        if (command.StartsWith("show token"))
                        {
                            // Show the user's token - for testing and troubleshooting
                            // Generally production apps should not display access tokens
                            await stepContext.Context.SendActivityAsync(
                                MessageFactory.Text($"Your token is: {tokenResponse.Token}"),
                                cancellationToken);
                        }
                        else if (command.StartsWith("show me"))
                        {
                            await stepContext.Context.SendActivityAsync(
                                MessageFactory.Text("I don't know how to do this yet!"),
                                cancellationToken);
                        }
                        else if (command.StartsWith("show calendar"))
                        {
                            await stepContext.Context.SendActivityAsync(
                                MessageFactory.Text("I don't know how to do this yet!"),
                                cancellationToken);
                        }
                        else if (command.StartsWith("add event"))
                        {
                            await stepContext.Context.SendActivityAsync(
                                MessageFactory.Text("I don't know how to do this yet!"),
                                cancellationToken);
                        }
                        else
                        {
                            await stepContext.Context.SendActivityAsync(
                                MessageFactory.Text("I'm sorry, I didn't understand. Please try again."),
                                cancellationToken);
                        }
                    }
                }
                else
                {
                    await stepContext.Context.SendActivityAsync(
                        MessageFactory.Text("We couldn't log you in. Please try again later."),
                        cancellationToken);
                    return await stepContext.EndDialogAsync(cancellationToken: cancellationToken);
                }

                // Go to the next step
                return await stepContext.NextAsync(cancellationToken: cancellationToken);
            }

            private async Task<DialogTurnResult> ReturnToPromptStepAsync(
                WaterfallStepContext stepContext,
                CancellationToken cancellationToken)
            {
                // Restart the dialog, but skip the initial login prompt
                return await stepContext.ReplaceDialogAsync(InitialDialogId, NO_PROMPT, cancellationToken);
            }
        }
    }
    ```

    Изучите этот код.

    - В конструкторе он настраивает [ватерфаллдиалог](https://docs.microsoft.com/azure/bot-service/bot-builder-concept-waterfall-dialogs?view=azure-bot-service-4.0) с набором действий, выполняемых по порядку.
        - В `LoginPromptStepAsync` нем отправляется **оауспромпт**. Если пользователь не вошел в систему, он будет отправлен пользователю запрос пользовательского интерфейса.
        - `ProcessLoginStepAsync`Проверяет успешность входа и отправляет подтверждение.
        - В `PromptUserStepAsync` нем отправляется **чоицепромпт** с доступными командами.
        - В `CommandStepAsync` нем сохраняется выбор пользователя, а затем повторно отправляется **оауспромпт**.
        - В `ProcessStepAsync` нем выполняются действия на основе полученной команды.
        - В `ReturnToPromptStepAsync` нем выполняется каскадный переход, но перед этим передается флажок для пропуска начального имени пользователя.

## <a name="update-calendarbot"></a>Обновление Календарбот

Следующий шаг — обновление **календарбот** для использования этих новых диалоговых окон.

1. Откройте **/Ботс/календарбот.КС** и замените все его содержимое приведенным ниже кодом.

    :::code language="csharp" source="../demo/GraphCalendarBot/Bots/CalendarBot.cs" id="CalendarBotSnippet":::

    Ниже приведен краткий обзор изменений.

    - Класс **календарбот** изменился на класс шаблона и получает **диалоговое окно**.
    - Изменен класс **календарбот** для расширения **теамсактивитихандлер**, позволяющий выполнять вход в Microsoft Teams.
    - Добавлены дополнительные переопределения метода для включения проверки подлинности.

## <a name="update-startupcs"></a>Обновление Startup.cs

Последним шагом является обновление `ConfigureServices` метода для добавления служб, необходимых для проверки подлинности, и нового диалогового окна.

1. Откройте **./Startup.CS** и удалите `services.AddTransient<IBot, Bots.CalendarBot>();` строку из `ConfigureServices` метода.

1. Вставьте следующий код в конце `ConfigureServices` метода.

    :::code language="csharp" source="../demo/GraphCalendarBot/Startup.cs" id="ConfigureServiceSnippet":::

## <a name="test-authentication"></a>Проверка подлинности

1. Сохраните все изменения и запустите Bot с помощью `dotnet run` .

1. Откройте эмулятор Bot Framework. Выберите меню **файл** , затем **создать конфигурацию ленты..**..

1. Заполните поля, как показано ниже.

    - **Имя ленты:**`CalendarBot`
    - **URL-адрес конечной точки:**`https://localhost:3978/api/messages`
    - **Идентификатор приложения Microsoft:** идентификатор приложения, на котором находится регистрация приложения "робот" в **календаре** .
    - **Пароль Microsoft App:** секрет клиента для клиента в **виде графика**
    - **Зашифруйте ключи, хранящиеся в конфигурации Bot:** Доступ

    ![Снимок экрана с диалоговым окном создания конфигурации Bot](images/new-bot-config.png)

1. Нажмите кнопку **сохранить и подключиться**. После подключения эмулятора должно отобразиться `Welcome to Microsoft Graph CalendarBot. Type anything to get started.`

1. Введите некоторый текст и отправьте его в Bot. Bot реагирует на приглашение с приглашением на вход.

1. Нажмите кнопку **Вход** . Эмулятор предложит подтвердить URL-адрес, с которого начинается `oauthlink://https://token.botframeworkcom` . Нажмите кнопку **подтвердить** , чтобы продолжить.

1. Во всплывающем окне Войдите с помощью учетной записи Microsoft 365. Просмотрите запрошенные разрешения и примите.

1. После выполнения проверки подлинности и согласия всплывающее окно предоставляет код проверки. Скопируйте код и закройте окно.

    ![Снимок экрана кода проверки эмулятора Bot Framework](images/validation-code.png)

1. Введите код проверки в окне Chat, чтобы завершить вход.

    ![Снимок экрана: Беседа с примером входа с примером ленты](images/bot-login.png)

1. Если вы выберете кнопку **Показать маркер** (или тип `show token` ), Bot отображает маркер доступа. При **нажатии кнопки выход** (или вводе `log out` ) выводится журнал.

> [!TIP]
> При запуске беседы с Bot может появиться следующее сообщение об ошибке в эмуляторе Bot Framework.
>
> ```text
> Failed to generate an actual sign-in link: Error: Failed to connect to ngrok instance for OAuth postback URL:
> FetchError: request to http://127.0.0.1:4041/api/tunnels failed, reason: connect ECONNREFUSED 127.0.0.1:4041
> ```
>
> Если это произойдет, закройте эмулятор и перезапустите его.
