---
ms.openlocfilehash: 05b3223967bf2a6d321c00cca079cc5c5b8ff0db
ms.sourcegitcommit: e0d9b18d2d4cbeb4a48890f3420a47e6a90abc53
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/17/2020
ms.locfileid: "49347878"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="7eb6b-101">В этом упражнении вы будете использовать **Оауспромпт** платформы Bot для реализации проверки подлинности в Bot и получения маркеров доступа для вызова API Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="7eb6b-101">In this exercise you will use the Bot Framework's **OAuthPrompt** to implement authentication in the bot, and acquire access tokens for calling the Microsoft Graph API.</span></span>

1. <span data-ttu-id="7eb6b-102">Откройте **./appsettings.js** и внесите следующие изменения.</span><span class="sxs-lookup"><span data-stu-id="7eb6b-102">Open **./appsettings.json** and make the following changes.</span></span>

    - <span data-ttu-id="7eb6b-103">Измените значение `MicrosoftAppId` на идентификатор приложения, на который будет зарегистрирована регистрация приложения "робот" в **календаре** .</span><span class="sxs-lookup"><span data-stu-id="7eb6b-103">Change the value of `MicrosoftAppId` to the application ID of your **Graph Calendar Bot** app registration.</span></span>
    - <span data-ttu-id="7eb6b-104">Измените значение `MicrosoftAppPassword` на секрет клиента клиента для клиента почтового **календаря Bot** .</span><span class="sxs-lookup"><span data-stu-id="7eb6b-104">Change the value of `MicrosoftAppPassword` to your **Graph Calendar Bot** client secret.</span></span>
    - <span data-ttu-id="7eb6b-105">Добавьте значение с именем `ConnectionName` `GraphBotAuth` .</span><span class="sxs-lookup"><span data-stu-id="7eb6b-105">Add a value named `ConnectionName` with a value of `GraphBotAuth`.</span></span>

    :::code language="json" source="../demo/GraphCalendarBot/appsettings.example.json":::

    > [!NOTE]
    > <span data-ttu-id="7eb6b-106">Если вы использовали значение, отличное от `GraphBotAuth` имени записи в **параметрах подключения OAuth** на портале Azure, используйте это значение для этой `ConnectionName` записи.</span><span class="sxs-lookup"><span data-stu-id="7eb6b-106">If you used a value other than `GraphBotAuth` for the name of your entry in **OAuth Connection Settings** in the Azure Portal, use that value for the `ConnectionName` entry.</span></span>

## <a name="implement-dialogs"></a><span data-ttu-id="7eb6b-107">Реализация диалоговых окон</span><span class="sxs-lookup"><span data-stu-id="7eb6b-107">Implement dialogs</span></span>

1. <span data-ttu-id="7eb6b-108">Создайте новый каталог в корне **диалогового окна** проекта с именем.</span><span class="sxs-lookup"><span data-stu-id="7eb6b-108">Create a new directory in the root of the project named **Dialogs**.</span></span> <span data-ttu-id="7eb6b-109">Создайте новый файл в каталоге **./диалогс** с именем **LogoutDialog.CS** и добавьте следующий код.</span><span class="sxs-lookup"><span data-stu-id="7eb6b-109">Create a new file in the **./Dialogs** directory named **LogoutDialog.cs** and add the following code.</span></span>

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/LogoutDialog.cs" id="LogoutDialogSnippet":::

    <span data-ttu-id="7eb6b-110">Это диалоговое окно предоставляет базовый класс для всех других диалоговых окон в Bot, от которых наследуется.</span><span class="sxs-lookup"><span data-stu-id="7eb6b-110">This dialog provides a base class for all of the other dialogs in the bot to derive from.</span></span> <span data-ttu-id="7eb6b-111">Это позволяет пользователю выйти из независимо от того, где они находятся в диалоговых окнах ленты.</span><span class="sxs-lookup"><span data-stu-id="7eb6b-111">This allows the user to log out no matter where they are in the bot's dialogs.</span></span>

1. <span data-ttu-id="7eb6b-112">Создайте новый файл в каталоге **./диалогс** с именем **MainDialog.CS** и добавьте следующий код.</span><span class="sxs-lookup"><span data-stu-id="7eb6b-112">Create a new file in the **./Dialogs** directory named **MainDialog.cs** and add the following code.</span></span>

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

    <span data-ttu-id="7eb6b-113">Изучите этот код.</span><span class="sxs-lookup"><span data-stu-id="7eb6b-113">Take a moment to review this code.</span></span>

    - <span data-ttu-id="7eb6b-114">В конструкторе он настраивает [ватерфаллдиалог](https://docs.microsoft.com/azure/bot-service/bot-builder-concept-waterfall-dialogs?view=azure-bot-service-4.0) с набором действий, выполняемых по порядку.</span><span class="sxs-lookup"><span data-stu-id="7eb6b-114">In the constructor, it sets up a [WaterfallDialog](https://docs.microsoft.com/azure/bot-service/bot-builder-concept-waterfall-dialogs?view=azure-bot-service-4.0) with a set of steps that occur in order.</span></span>
        - <span data-ttu-id="7eb6b-115">В `LoginPromptStepAsync` нем отправляется **оауспромпт**.</span><span class="sxs-lookup"><span data-stu-id="7eb6b-115">In `LoginPromptStepAsync` it sends an **OAuthPrompt**.</span></span> <span data-ttu-id="7eb6b-116">Если пользователь не вошел в систему, он будет отправлен пользователю запрос пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="7eb6b-116">If the user isn't logged in, this will send a UI prompt to the user.</span></span>
        - <span data-ttu-id="7eb6b-117">`ProcessLoginStepAsync`Проверяет успешность входа и отправляет подтверждение.</span><span class="sxs-lookup"><span data-stu-id="7eb6b-117">In `ProcessLoginStepAsync` it checks if the login was successful and sends a confirmation.</span></span>
        - <span data-ttu-id="7eb6b-118">В `PromptUserStepAsync` нем отправляется **чоицепромпт** с доступными командами.</span><span class="sxs-lookup"><span data-stu-id="7eb6b-118">In `PromptUserStepAsync` it sends a **ChoicePrompt** with the available commands.</span></span>
        - <span data-ttu-id="7eb6b-119">В `CommandStepAsync` нем сохраняется выбор пользователя, а затем повторно отправляется **оауспромпт**.</span><span class="sxs-lookup"><span data-stu-id="7eb6b-119">In `CommandStepAsync` it saves the user's choice, then resends an **OAuthPrompt**.</span></span>
        - <span data-ttu-id="7eb6b-120">В `ProcessStepAsync` нем выполняются действия на основе полученной команды.</span><span class="sxs-lookup"><span data-stu-id="7eb6b-120">In `ProcessStepAsync` it takes action based on the command received.</span></span>
        - <span data-ttu-id="7eb6b-121">В `ReturnToPromptStepAsync` нем выполняется каскадный переход, но перед этим передается флажок для пропуска начального имени пользователя.</span><span class="sxs-lookup"><span data-stu-id="7eb6b-121">In `ReturnToPromptStepAsync` it starts the waterfall over, but passes a flag to skip the initial user login.</span></span>

## <a name="update-calendarbot"></a><span data-ttu-id="7eb6b-122">Обновление Календарбот</span><span class="sxs-lookup"><span data-stu-id="7eb6b-122">Update CalendarBot</span></span>

<span data-ttu-id="7eb6b-123">Следующий шаг — обновление **календарбот** для использования этих новых диалоговых окон.</span><span class="sxs-lookup"><span data-stu-id="7eb6b-123">The next step is to update **CalendarBot** to use these new dialogs.</span></span>

1. <span data-ttu-id="7eb6b-124">Откройте **/Ботс/календарбот.КС** и замените все его содержимое приведенным ниже кодом.</span><span class="sxs-lookup"><span data-stu-id="7eb6b-124">Open **./Bots/CalendarBot.cs** and replace its entire contents with the following code.</span></span>

    :::code language="csharp" source="../demo/GraphCalendarBot/Bots/CalendarBot.cs" id="CalendarBotSnippet":::

    <span data-ttu-id="7eb6b-125">Ниже приведен краткий обзор изменений.</span><span class="sxs-lookup"><span data-stu-id="7eb6b-125">Here's a brief summary of the changes.</span></span>

    - <span data-ttu-id="7eb6b-126">Класс **календарбот** изменился на класс шаблона и получает **диалоговое окно**.</span><span class="sxs-lookup"><span data-stu-id="7eb6b-126">Changed the **CalendarBot** class to be a template class, receiving a **Dialog**.</span></span>
    - <span data-ttu-id="7eb6b-127">Изменен класс **календарбот** для расширения **теамсактивитихандлер**, позволяющий выполнять вход в Microsoft Teams.</span><span class="sxs-lookup"><span data-stu-id="7eb6b-127">Changed the **CalendarBot** class to extend **TeamsActivityHandler**, allowing it to sign in in Microsoft Teams.</span></span>
    - <span data-ttu-id="7eb6b-128">Добавлены дополнительные переопределения метода для включения проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="7eb6b-128">Added additional method overrides to enable authentication.</span></span>

## <a name="update-startupcs"></a><span data-ttu-id="7eb6b-129">Обновление Startup.cs</span><span class="sxs-lookup"><span data-stu-id="7eb6b-129">Update Startup.cs</span></span>

<span data-ttu-id="7eb6b-130">Последним шагом является обновление `ConfigureServices` метода для добавления служб, необходимых для проверки подлинности, и нового диалогового окна.</span><span class="sxs-lookup"><span data-stu-id="7eb6b-130">The final step is to update the `ConfigureServices` method to add the services needed for authentication and the new dialog.</span></span>

1. <span data-ttu-id="7eb6b-131">Откройте **./Startup.CS** и удалите `services.AddTransient<IBot, Bots.CalendarBot>();` строку из `ConfigureServices` метода.</span><span class="sxs-lookup"><span data-stu-id="7eb6b-131">Open **./Startup.cs** and remove the `services.AddTransient<IBot, Bots.CalendarBot>();` line from the `ConfigureServices` method.</span></span>

1. <span data-ttu-id="7eb6b-132">Вставьте следующий код в конце `ConfigureServices` метода.</span><span class="sxs-lookup"><span data-stu-id="7eb6b-132">Insert the following code at the end of the `ConfigureServices` method.</span></span>

    :::code language="csharp" source="../demo/GraphCalendarBot/Startup.cs" id="ConfigureServiceSnippet":::

## <a name="test-authentication"></a><span data-ttu-id="7eb6b-133">Проверка подлинности</span><span class="sxs-lookup"><span data-stu-id="7eb6b-133">Test authentication</span></span>

1. <span data-ttu-id="7eb6b-134">Сохраните все изменения и запустите Bot с помощью `dotnet run` .</span><span class="sxs-lookup"><span data-stu-id="7eb6b-134">Save all of your changes and start the bot with `dotnet run`.</span></span>

1. <span data-ttu-id="7eb6b-135">Откройте эмулятор Bot Framework.</span><span class="sxs-lookup"><span data-stu-id="7eb6b-135">Open the Bot Framework Emulator.</span></span> <span data-ttu-id="7eb6b-136">Выберите меню **файл** , затем **создать конфигурацию ленты..**..</span><span class="sxs-lookup"><span data-stu-id="7eb6b-136">Select the **File** menu, then **New Bot Configuration...**.</span></span>

1. <span data-ttu-id="7eb6b-137">Заполните поля, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="7eb6b-137">Fill in the fields as follows.</span></span>

    - <span data-ttu-id="7eb6b-138">**Имя ленты:**`CalendarBot`</span><span class="sxs-lookup"><span data-stu-id="7eb6b-138">**Bot name:** `CalendarBot`</span></span>
    - <span data-ttu-id="7eb6b-139">**URL-адрес конечной точки:**`https://localhost:3978/api/messages`</span><span class="sxs-lookup"><span data-stu-id="7eb6b-139">**Endpoint URL:** `https://localhost:3978/api/messages`</span></span>
    - <span data-ttu-id="7eb6b-140">**Идентификатор приложения Microsoft:** идентификатор приложения, на котором находится регистрация приложения "робот" в **календаре** .</span><span class="sxs-lookup"><span data-stu-id="7eb6b-140">**Microsoft App ID:** the application ID of your **Graph Calendar Bot** app registration</span></span>
    - <span data-ttu-id="7eb6b-141">**Пароль Microsoft App:** секрет клиента для клиента в **виде графика**</span><span class="sxs-lookup"><span data-stu-id="7eb6b-141">**Microsoft App password:** your **Graph Calendar Bot** client secret</span></span>
    - <span data-ttu-id="7eb6b-142">**Зашифруйте ключи, хранящиеся в конфигурации Bot:** Доступ</span><span class="sxs-lookup"><span data-stu-id="7eb6b-142">**Encrypt keys stored in your bot configuration:** Enabled</span></span>

    ![Снимок экрана с диалоговым окном создания конфигурации Bot](images/new-bot-config.png)

1. <span data-ttu-id="7eb6b-144">Нажмите кнопку **сохранить и подключиться**.</span><span class="sxs-lookup"><span data-stu-id="7eb6b-144">Select **Save and connect**.</span></span> <span data-ttu-id="7eb6b-145">После подключения эмулятора должно отобразиться `Welcome to Microsoft Graph CalendarBot. Type anything to get started.`</span><span class="sxs-lookup"><span data-stu-id="7eb6b-145">After the emulator connects, you should see `Welcome to Microsoft Graph CalendarBot. Type anything to get started.`</span></span>

1. <span data-ttu-id="7eb6b-146">Введите некоторый текст и отправьте его в Bot.</span><span class="sxs-lookup"><span data-stu-id="7eb6b-146">Type some text and send it to the bot.</span></span> <span data-ttu-id="7eb6b-147">Bot реагирует на приглашение с приглашением на вход.</span><span class="sxs-lookup"><span data-stu-id="7eb6b-147">The bot responds with a login prompt.</span></span>

1. <span data-ttu-id="7eb6b-148">Нажмите кнопку **Вход** .</span><span class="sxs-lookup"><span data-stu-id="7eb6b-148">Select the **Login** button.</span></span> <span data-ttu-id="7eb6b-149">Эмулятор предложит подтвердить URL-адрес, с которого начинается `oauthlink://https://token.botframeworkcom` .</span><span class="sxs-lookup"><span data-stu-id="7eb6b-149">The emulator prompts you to confirm the URL that starts with `oauthlink://https://token.botframeworkcom`.</span></span> <span data-ttu-id="7eb6b-150">Нажмите кнопку **подтвердить** , чтобы продолжить.</span><span class="sxs-lookup"><span data-stu-id="7eb6b-150">Select **Confirm** to continue.</span></span>

1. <span data-ttu-id="7eb6b-151">Во всплывающем окне Войдите с помощью учетной записи Microsoft 365.</span><span class="sxs-lookup"><span data-stu-id="7eb6b-151">In the pop-up window, login with your Microsoft 365 account.</span></span> <span data-ttu-id="7eb6b-152">Просмотрите запрошенные разрешения и примите.</span><span class="sxs-lookup"><span data-stu-id="7eb6b-152">Review the requested permissions and accept.</span></span>

1. <span data-ttu-id="7eb6b-153">После выполнения проверки подлинности и согласия всплывающее окно предоставляет код проверки.</span><span class="sxs-lookup"><span data-stu-id="7eb6b-153">Once authentication and consent are complete, the pop-up window provides a validation code.</span></span> <span data-ttu-id="7eb6b-154">Скопируйте код и закройте окно.</span><span class="sxs-lookup"><span data-stu-id="7eb6b-154">Copy the code and close the window.</span></span>

    ![Снимок экрана кода проверки эмулятора Bot Framework](images/validation-code.png)

1. <span data-ttu-id="7eb6b-156">Введите код проверки в окне Chat, чтобы завершить вход.</span><span class="sxs-lookup"><span data-stu-id="7eb6b-156">Enter the validation code in the chat window to complete the login.</span></span>

    ![Снимок экрана: Беседа с примером входа с примером ленты](images/bot-login.png)

1. <span data-ttu-id="7eb6b-158">Если вы выберете кнопку **Показать маркер** (или тип `show token` ), Bot отображает маркер доступа.</span><span class="sxs-lookup"><span data-stu-id="7eb6b-158">If you select the **Show token** button (or type `show token`), the bot displays the access token.</span></span> <span data-ttu-id="7eb6b-159">При **нажатии кнопки выход** (или вводе `log out` ) выводится журнал.</span><span class="sxs-lookup"><span data-stu-id="7eb6b-159">The **Log out** button (or typing `log out`) will log you out.</span></span>

> [!TIP]
> <span data-ttu-id="7eb6b-160">При запуске беседы с Bot может появиться следующее сообщение об ошибке в эмуляторе Bot Framework.</span><span class="sxs-lookup"><span data-stu-id="7eb6b-160">You may receive the following error message in the Bot Framework Emulator when starting a conversation with the bot.</span></span>
>
> ```text
> Failed to generate an actual sign-in link: Error: Failed to connect to ngrok instance for OAuth postback URL:
> FetchError: request to http://127.0.0.1:4041/api/tunnels failed, reason: connect ECONNREFUSED 127.0.0.1:4041
> ```
>
> <span data-ttu-id="7eb6b-161">Если это произойдет, закройте эмулятор и перезапустите его.</span><span class="sxs-lookup"><span data-stu-id="7eb6b-161">If this happens, close the emulator and restart it.</span></span>
