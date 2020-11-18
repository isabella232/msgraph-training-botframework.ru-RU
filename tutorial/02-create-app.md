---
ms.openlocfilehash: 12a6c18b52e2bc9aba5ad69c8bb9ab8091c11a64
ms.sourcegitcommit: e0d9b18d2d4cbeb4a48890f3420a47e6a90abc53
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/17/2020
ms.locfileid: "49347889"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="7a3bf-101">В этом разделе описывается создание проекта Bot Framework.</span><span class="sxs-lookup"><span data-stu-id="7a3bf-101">In this section you'll create a Bot Framework project.</span></span>

1. <span data-ttu-id="7a3bf-102">Откройте интерфейс командной строки (CLI) в каталоге, в котором нужно создать проект.</span><span class="sxs-lookup"><span data-stu-id="7a3bf-102">Open your command-line interface (CLI) in a directory where you want to create the project.</span></span> <span data-ttu-id="7a3bf-103">Выполните следующую команду, чтобы создать новый проект с помощью шаблона **Microsoft. Bot. Framework. CSharp. ечобот** .</span><span class="sxs-lookup"><span data-stu-id="7a3bf-103">Run the following command to create new project using the **Microsoft.Bot.Framework.CSharp.EchoBot** template.</span></span>

    ```dotnetcli
    dotnet new echobot -n GraphCalendarBot
    ```

    > [!NOTE]
    > <span data-ttu-id="7a3bf-104">При получении `No templates matched the input template name: echobot.` сообщения об ошибке установите шаблон, выполнив приведенную ниже команду, и повторно выполните предыдущую команду.</span><span class="sxs-lookup"><span data-stu-id="7a3bf-104">If you receive an `No templates matched the input template name: echobot.` error, install the template with the following command and re-run the previous command.</span></span>
    >
    > ```dotnetcli
    > dotnet new -i Microsoft.Bot.Framework.CSharp.EchoBot
    > ```

1. <span data-ttu-id="7a3bf-105">Переименуйте класс **ечобот** по умолчанию на **календарбот**.</span><span class="sxs-lookup"><span data-stu-id="7a3bf-105">Rename the default **EchoBot** class to **CalendarBot**.</span></span> <span data-ttu-id="7a3bf-106">Откройте **./Ботс/ечобот.КС** и замените все экземпляры `EchoBot` на `CalendarBot` .</span><span class="sxs-lookup"><span data-stu-id="7a3bf-106">Open **./Bots/EchoBot.cs** and replace all instances of `EchoBot` with `CalendarBot`.</span></span> <span data-ttu-id="7a3bf-107">Переименуйте файл в **CalendarBot.CS**.</span><span class="sxs-lookup"><span data-stu-id="7a3bf-107">Rename the file to **CalendarBot.cs**.</span></span>

1. <span data-ttu-id="7a3bf-108">Замените все экземпляры `EchoBot` `CalendarBot` в остальных **CS** файлы.</span><span class="sxs-lookup"><span data-stu-id="7a3bf-108">Replace all instances of `EchoBot` with `CalendarBot` in the remaining **.cs** files.</span></span>

1. <span data-ttu-id="7a3bf-109">В интерфейсе командной строки смените текущий каталог на каталог **графкалендарбот** и выполните следующую команду, чтобы подтвердить построение проекта.</span><span class="sxs-lookup"><span data-stu-id="7a3bf-109">In your CLI, change the current directory to the **GraphCalendarBot** directory and run the following command to confirm the project builds.</span></span>

    ```dotnetcli
    dotnet build
    ```

## <a name="add-nuget-packages"></a><span data-ttu-id="7a3bf-110">Добавление пакетов NuGet</span><span class="sxs-lookup"><span data-stu-id="7a3bf-110">Add NuGet packages</span></span>

<span data-ttu-id="7a3bf-111">Прежде чем переходить, установите некоторые дополнительные пакеты NuGet, которые будут использоваться позже.</span><span class="sxs-lookup"><span data-stu-id="7a3bf-111">Before moving on, install some additional NuGet packages that you will use later.</span></span>

- <span data-ttu-id="7a3bf-112">[Адаптивекардс](https://www.nuget.org/packages/AdaptiveCards/) , чтобы позволить роботу отправлять в ответах адаптивные карты.</span><span class="sxs-lookup"><span data-stu-id="7a3bf-112">[AdaptiveCards](https://www.nuget.org/packages/AdaptiveCards/) to allow the bot to send Adaptive Cards in responses.</span></span>
- <span data-ttu-id="7a3bf-113">[Microsoft. Bot. Builder. Dialogs](https://www.nuget.org/packages/Microsoft.Bot.Builder.Dialogs/) для добавления поддержки диалоговых окон в Bot.</span><span class="sxs-lookup"><span data-stu-id="7a3bf-113">[Microsoft.Bot.Builder.Dialogs](https://www.nuget.org/packages/Microsoft.Bot.Builder.Dialogs/) to add dialog support to the bot.</span></span>
- <span data-ttu-id="7a3bf-114">[Microsoft. Распознаватели. Text. types. тимексекспрессион](https://www.nuget.org/packages/Microsoft.Recognizers.Text.DataTypes.TimexExpression/) для преобразования выражений Тимекс, возвращаемых из приглашений Bot, в объекты **DateTime** .</span><span class="sxs-lookup"><span data-stu-id="7a3bf-114">[Microsoft.Recognizers.Text.DataTypes.TimexExpression](https://www.nuget.org/packages/Microsoft.Recognizers.Text.DataTypes.TimexExpression/) to convert the TIMEX expressions returned from bot prompts into **DateTime** objects.</span></span>
- <span data-ttu-id="7a3bf-115">[Microsoft. Graph](https://www.nuget.org/packages/Microsoft.Graph/) для совершения звонков в Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="7a3bf-115">[Microsoft.Graph](https://www.nuget.org/packages/Microsoft.Graph/) for making calls to Microsoft Graph.</span></span>

1. <span data-ttu-id="7a3bf-116">Выполните следующие команды в интерфейсе командной строки, чтобы установить зависимости.</span><span class="sxs-lookup"><span data-stu-id="7a3bf-116">Run the following commands in your CLI to install the dependencies.</span></span>

    ```Shell
    dotnet add package AdaptiveCards --version 2.2.0
    dotnet add package Microsoft.Bot.Builder.Dialogs --version 4.10.3
    dotnet add package Microsoft.Bot.Builder.Integration.AspNet.Core --version 4.10.3
    dotnet add package Microsoft.Recognizers.Text.DataTypes.TimexExpression --version 1.4.1
    dotnet add package Microsoft.Graph --version 3.18.0
    ```

## <a name="test-the-bot"></a><span data-ttu-id="7a3bf-117">Тестирование ленты</span><span class="sxs-lookup"><span data-stu-id="7a3bf-117">Test the bot</span></span>

<span data-ttu-id="7a3bf-118">Перед добавлением кода проверьте, правильно ли он работает, и убедитесь, что эмулятор ленты настроен для тестирования.</span><span class="sxs-lookup"><span data-stu-id="7a3bf-118">Before adding any code, test the bot to make sure that it works correctly, and that the Bot Framework Emulator is configured to test it.</span></span>

1. <span data-ttu-id="7a3bf-119">Запустите Bot, выполнив следующую команду.</span><span class="sxs-lookup"><span data-stu-id="7a3bf-119">Start the bot by running the following command.</span></span>

    ```dotnetcli
    dotnet run
    ```

    > [!TIP]
    > <span data-ttu-id="7a3bf-120">Несмотря на то, что для редактирования исходных файлов в проекте можно использовать любой текстовый редактор, рекомендуется использовать [Visual Studio Code](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="7a3bf-120">While you can use any text editor to edit the source files in the project, we recommend using [Visual Studio Code](https://code.visualstudio.com/).</span></span> <span data-ttu-id="7a3bf-121">Visual Studio Code предоставляет поддержку отладки, IntelliSense и многое другое.</span><span class="sxs-lookup"><span data-stu-id="7a3bf-121">Visual Studio Code offers debugging support, Intellisense, and more.</span></span> <span data-ttu-id="7a3bf-122">Если используется Visual Studio Code, программу Bot можно запустить с помощью меню **Run**  ->  **Отладка** запуска.</span><span class="sxs-lookup"><span data-stu-id="7a3bf-122">If using Visual Studio Code, you can start the bot using the **Run** -> **Start Debugging** menu.</span></span>

1. <span data-ttu-id="7a3bf-123">Подтвердите, что Bot работает, открыв браузер и перейдя в `http://localhost:3978` .</span><span class="sxs-lookup"><span data-stu-id="7a3bf-123">Confirm the bot is running by opening your browser and going to `http://localhost:3978`.</span></span> <span data-ttu-id="7a3bf-124">Вы должны увидеть, что **ваш Bot готов!**</span><span class="sxs-lookup"><span data-stu-id="7a3bf-124">You should see a **Your bot is ready!**</span></span> <span data-ttu-id="7a3bf-125">Сообщение.</span><span class="sxs-lookup"><span data-stu-id="7a3bf-125">message.</span></span>

1. <span data-ttu-id="7a3bf-126">Откройте эмулятор Bot Framework.</span><span class="sxs-lookup"><span data-stu-id="7a3bf-126">Open the Bot Framework Emulator.</span></span> <span data-ttu-id="7a3bf-127">Выберите меню **файл** , а затем **откройте файл Bot**.</span><span class="sxs-lookup"><span data-stu-id="7a3bf-127">Choose the **File** menu, then **Open Bot**.</span></span>

1. <span data-ttu-id="7a3bf-128">Введите `http://localhost:3978/api/messages` **URL-адрес Bot** и нажмите кнопку **подключить**.</span><span class="sxs-lookup"><span data-stu-id="7a3bf-128">Enter `http://localhost:3978/api/messages` in the **Bot URL**, then select **Connect**.</span></span>

1. <span data-ttu-id="7a3bf-129">Bot реагирует на то, что `Hello and welcome!` в окне чата.</span><span class="sxs-lookup"><span data-stu-id="7a3bf-129">The bot responds with `Hello and welcome!` in the chat window.</span></span> <span data-ttu-id="7a3bf-130">Отправьте сообщение на Bot и подтвердите его обратную отправку.</span><span class="sxs-lookup"><span data-stu-id="7a3bf-130">Send a message to the bot and confirm it echoes it back.</span></span>

    ![Снимок экрана: эмулятор Bot, подключенный к Bot](images/test-emulator.png)
