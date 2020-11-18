---
ms.openlocfilehash: c0fd9a9f5529f202c125d7f1e7c6b50fa9dc630d
ms.sourcegitcommit: e0d9b18d2d4cbeb4a48890f3420a47e6a90abc53
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/17/2020
ms.locfileid: "49347896"
---
# <a name="graphcalendarbot"></a>графкалендарбот

Образец поэхо-Bot для ленты версии 4.

Этот робот создан с помощью [Bot Framework](https://dev.botframework.com), он показывает, как создать простой Bot, который принимает входные данные от пользователя и возвращает его обратно.

## <a name="prerequisites"></a>Предварительные условия

- [Пакет SDK для .NET Core](https://dotnet.microsoft.com/download) версии 3,1

  ```bash
  # determine dotnet version
  dotnet --version
  ```

## <a name="to-try-this-sample"></a>Для пробного использования этого примера

- В терминале перейдите к разделу `GraphCalendarBot`

    ```bash
    # change into project folder
    cd GraphCalendarBot
    ```

- Запустите Bot с терминала или из Visual Studio, выберите вариант A или B.

  A) с терминала

  ```bash
  # run the bot
  dotnet run
  ```

  B) или из Visual Studio

  - Запуск Visual Studio
  - Файл > открыть > проект или решение
  - Перейти к `GraphCalendarBot` папке
  - Выбор `GraphCalendarBot.csproj` файла
  - Нажмите `F5` , чтобы запустить проект

## <a name="testing-the-bot-using-bot-framework-emulator"></a>Тестирование Bot с помощью эмулятора Bot Framework

[Эмулятор ленты](https://github.com/microsoft/botframework-emulator) — это настольное приложение, которое позволяет разработчикам-роботам тестировать и отлаживать свои боты на локальном сервере или выполнять их удаленно через туннель.

- Установите эмулятор Bot Framework версии 4.9.0 или выше [here](https://github.com/Microsoft/BotFramework-Emulator/releases)

### <a name="connect-to-the-bot-using-bot-framework-emulator"></a>Подключение к Bot с помощью эмулятора Bot Framework

- Запуск эмулятора Bot Framework
- Файл > открыть Bot
- Введите URL-адрес Bot `http://localhost:3978/api/messages`

## <a name="deploy-the-bot-to-azure"></a>Развертывание ленты в Azure

Для получения дополнительных сведений о развертывании Bot в Azure, ознакомьтесь со статьей [Deploy to Azure to Azure, чтобы](https://aka.ms/azuredeployment) получить полный список инструкций по развертыванию.

## <a name="further-reading"></a>Дополнительные материалы

- [Документация по инфраструктуре Bot](https://docs.botframework.com)
- [Основные сведения о Bot](https://docs.microsoft.com/azure/bot-service/bot-builder-basics?view=azure-bot-service-4.0)
- [Обработка действий](https://docs.microsoft.com/en-us/azure/bot-service/bot-builder-concept-activity-processing?view=azure-bot-service-4.0)
- [Введение в службу Azure Bot](https://docs.microsoft.com/azure/bot-service/bot-service-overview-introduction?view=azure-bot-service-4.0)
- [Документация по службе Azure Bot](https://docs.microsoft.com/azure/bot-service/?view=azure-bot-service-4.0)
- [Средства инфраструктуры .NET Core](https://docs.microsoft.com/en-us/dotnet/core/tools/?tabs=netcore2x)
- [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest)
- [Портал Azure](https://portal.azure.com)
- [Понимание языка с помощью Луис](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/)
- [Каналы и служба соединителя Bot](https://docs.microsoft.com/en-us/azure/bot-service/bot-concepts?view=azure-bot-service-4.0)

Создано с помощью `dotnet new echobot` v 4.10.2
