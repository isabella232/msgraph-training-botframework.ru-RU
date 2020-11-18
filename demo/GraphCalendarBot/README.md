---
ms.openlocfilehash: c0fd9a9f5529f202c125d7f1e7c6b50fa9dc630d
ms.sourcegitcommit: e0d9b18d2d4cbeb4a48890f3420a47e6a90abc53
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/17/2020
ms.locfileid: "49347896"
---
# <a name="graphcalendarbot"></a><span data-ttu-id="7c0a9-101">графкалендарбот</span><span class="sxs-lookup"><span data-stu-id="7c0a9-101">GraphCalendarBot</span></span>

<span data-ttu-id="7c0a9-102">Образец поэхо-Bot для ленты версии 4.</span><span class="sxs-lookup"><span data-stu-id="7c0a9-102">Bot Framework v4 echo bot sample.</span></span>

<span data-ttu-id="7c0a9-103">Этот робот создан с помощью [Bot Framework](https://dev.botframework.com), он показывает, как создать простой Bot, который принимает входные данные от пользователя и возвращает его обратно.</span><span class="sxs-lookup"><span data-stu-id="7c0a9-103">This bot has been created using [Bot Framework](https://dev.botframework.com), it shows how to create a simple bot that accepts input from the user and echoes it back.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7c0a9-104">Предварительные условия</span><span class="sxs-lookup"><span data-stu-id="7c0a9-104">Prerequisites</span></span>

- <span data-ttu-id="7c0a9-105">[Пакет SDK для .NET Core](https://dotnet.microsoft.com/download) версии 3,1</span><span class="sxs-lookup"><span data-stu-id="7c0a9-105">[.NET Core SDK](https://dotnet.microsoft.com/download) version 3.1</span></span>

  ```bash
  # determine dotnet version
  dotnet --version
  ```

## <a name="to-try-this-sample"></a><span data-ttu-id="7c0a9-106">Для пробного использования этого примера</span><span class="sxs-lookup"><span data-stu-id="7c0a9-106">To try this sample</span></span>

- <span data-ttu-id="7c0a9-107">В терминале перейдите к разделу `GraphCalendarBot`</span><span class="sxs-lookup"><span data-stu-id="7c0a9-107">In a terminal, navigate to `GraphCalendarBot`</span></span>

    ```bash
    # change into project folder
    cd GraphCalendarBot
    ```

- <span data-ttu-id="7c0a9-108">Запустите Bot с терминала или из Visual Studio, выберите вариант A или B.</span><span class="sxs-lookup"><span data-stu-id="7c0a9-108">Run the bot from a terminal or from Visual Studio, choose option A or B.</span></span>

  <span data-ttu-id="7c0a9-109">A) с терминала</span><span class="sxs-lookup"><span data-stu-id="7c0a9-109">A) From a terminal</span></span>

  ```bash
  # run the bot
  dotnet run
  ```

  <span data-ttu-id="7c0a9-110">B) или из Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7c0a9-110">B) Or from Visual Studio</span></span>

  - <span data-ttu-id="7c0a9-111">Запуск Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7c0a9-111">Launch Visual Studio</span></span>
  - <span data-ttu-id="7c0a9-112">Файл > открыть > проект или решение</span><span class="sxs-lookup"><span data-stu-id="7c0a9-112">File -> Open -> Project/Solution</span></span>
  - <span data-ttu-id="7c0a9-113">Перейти к `GraphCalendarBot` папке</span><span class="sxs-lookup"><span data-stu-id="7c0a9-113">Navigate to `GraphCalendarBot` folder</span></span>
  - <span data-ttu-id="7c0a9-114">Выбор `GraphCalendarBot.csproj` файла</span><span class="sxs-lookup"><span data-stu-id="7c0a9-114">Select `GraphCalendarBot.csproj` file</span></span>
  - <span data-ttu-id="7c0a9-115">Нажмите `F5` , чтобы запустить проект</span><span class="sxs-lookup"><span data-stu-id="7c0a9-115">Press `F5` to run the project</span></span>

## <a name="testing-the-bot-using-bot-framework-emulator"></a><span data-ttu-id="7c0a9-116">Тестирование Bot с помощью эмулятора Bot Framework</span><span class="sxs-lookup"><span data-stu-id="7c0a9-116">Testing the bot using Bot Framework Emulator</span></span>

<span data-ttu-id="7c0a9-117">[Эмулятор ленты](https://github.com/microsoft/botframework-emulator) — это настольное приложение, которое позволяет разработчикам-роботам тестировать и отлаживать свои боты на локальном сервере или выполнять их удаленно через туннель.</span><span class="sxs-lookup"><span data-stu-id="7c0a9-117">[Bot Framework Emulator](https://github.com/microsoft/botframework-emulator) is a desktop application that allows bot developers to test and debug their bots on localhost or running remotely through a tunnel.</span></span>

- <span data-ttu-id="7c0a9-118">Установите эмулятор Bot Framework версии 4.9.0 или выше [here](https://github.com/Microsoft/BotFramework-Emulator/releases)</span><span class="sxs-lookup"><span data-stu-id="7c0a9-118">Install the Bot Framework Emulator version 4.9.0 or greater from [here](https://github.com/Microsoft/BotFramework-Emulator/releases)</span></span>

### <a name="connect-to-the-bot-using-bot-framework-emulator"></a><span data-ttu-id="7c0a9-119">Подключение к Bot с помощью эмулятора Bot Framework</span><span class="sxs-lookup"><span data-stu-id="7c0a9-119">Connect to the bot using Bot Framework Emulator</span></span>

- <span data-ttu-id="7c0a9-120">Запуск эмулятора Bot Framework</span><span class="sxs-lookup"><span data-stu-id="7c0a9-120">Launch Bot Framework Emulator</span></span>
- <span data-ttu-id="7c0a9-121">Файл > открыть Bot</span><span class="sxs-lookup"><span data-stu-id="7c0a9-121">File -> Open Bot</span></span>
- <span data-ttu-id="7c0a9-122">Введите URL-адрес Bot `http://localhost:3978/api/messages`</span><span class="sxs-lookup"><span data-stu-id="7c0a9-122">Enter a Bot URL of `http://localhost:3978/api/messages`</span></span>

## <a name="deploy-the-bot-to-azure"></a><span data-ttu-id="7c0a9-123">Развертывание ленты в Azure</span><span class="sxs-lookup"><span data-stu-id="7c0a9-123">Deploy the bot to Azure</span></span>

<span data-ttu-id="7c0a9-124">Для получения дополнительных сведений о развертывании Bot в Azure, ознакомьтесь со статьей [Deploy to Azure to Azure, чтобы](https://aka.ms/azuredeployment) получить полный список инструкций по развертыванию.</span><span class="sxs-lookup"><span data-stu-id="7c0a9-124">To learn more about deploying a bot to Azure, see [Deploy your bot to Azure](https://aka.ms/azuredeployment) for a complete list of deployment instructions.</span></span>

## <a name="further-reading"></a><span data-ttu-id="7c0a9-125">Дополнительные материалы</span><span class="sxs-lookup"><span data-stu-id="7c0a9-125">Further reading</span></span>

- [<span data-ttu-id="7c0a9-126">Документация по инфраструктуре Bot</span><span class="sxs-lookup"><span data-stu-id="7c0a9-126">Bot Framework Documentation</span></span>](https://docs.botframework.com)
- [<span data-ttu-id="7c0a9-127">Основные сведения о Bot</span><span class="sxs-lookup"><span data-stu-id="7c0a9-127">Bot Basics</span></span>](https://docs.microsoft.com/azure/bot-service/bot-builder-basics?view=azure-bot-service-4.0)
- [<span data-ttu-id="7c0a9-128">Обработка действий</span><span class="sxs-lookup"><span data-stu-id="7c0a9-128">Activity processing</span></span>](https://docs.microsoft.com/en-us/azure/bot-service/bot-builder-concept-activity-processing?view=azure-bot-service-4.0)
- [<span data-ttu-id="7c0a9-129">Введение в службу Azure Bot</span><span class="sxs-lookup"><span data-stu-id="7c0a9-129">Azure Bot Service Introduction</span></span>](https://docs.microsoft.com/azure/bot-service/bot-service-overview-introduction?view=azure-bot-service-4.0)
- [<span data-ttu-id="7c0a9-130">Документация по службе Azure Bot</span><span class="sxs-lookup"><span data-stu-id="7c0a9-130">Azure Bot Service Documentation</span></span>](https://docs.microsoft.com/azure/bot-service/?view=azure-bot-service-4.0)
- [<span data-ttu-id="7c0a9-131">Средства инфраструктуры .NET Core</span><span class="sxs-lookup"><span data-stu-id="7c0a9-131">.NET Core CLI tools</span></span>](https://docs.microsoft.com/en-us/dotnet/core/tools/?tabs=netcore2x)
- [<span data-ttu-id="7c0a9-132">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="7c0a9-132">Azure CLI</span></span>](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest)
- [<span data-ttu-id="7c0a9-133">Портал Azure</span><span class="sxs-lookup"><span data-stu-id="7c0a9-133">Azure Portal</span></span>](https://portal.azure.com)
- [<span data-ttu-id="7c0a9-134">Понимание языка с помощью Луис</span><span class="sxs-lookup"><span data-stu-id="7c0a9-134">Language Understanding using LUIS</span></span>](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/)
- [<span data-ttu-id="7c0a9-135">Каналы и служба соединителя Bot</span><span class="sxs-lookup"><span data-stu-id="7c0a9-135">Channels and Bot Connector Service</span></span>](https://docs.microsoft.com/en-us/azure/bot-service/bot-concepts?view=azure-bot-service-4.0)

<span data-ttu-id="7c0a9-136">Создано с помощью `dotnet new echobot` v 4.10.2</span><span class="sxs-lookup"><span data-stu-id="7c0a9-136">Generated using `dotnet new echobot` v4.10.2</span></span>
