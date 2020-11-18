---
ms.openlocfilehash: 0022448db76695894bfefca2543ad39c4fd07dcc
ms.sourcegitcommit: e0d9b18d2d4cbeb4a48890f3420a47e6a90abc53
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/17/2020
ms.locfileid: "49347895"
---
<!-- markdownlint-disable MD002 MD041 -->

В этом руководстве рассказывается, как создать робота Bot Framework, который использует API Microsoft Graph для получения сведений о календаре для пользователя.

> [!TIP]
> Если вы предпочитаете просто скачать заполненный учебник, вы можете скачать или клонировать [репозиторий GitHub](https://github.com/microsoftgraph/msgraph-training-botframework). Ознакомьтесь с файлом README в папке **Demo** , чтобы получить инструкции по настройке приложения с идентификатором и секретом приложения.

## <a name="prerequisites"></a>Предварительные условия

Прежде чем приступить к работе с этим руководством, на вашем компьютере для разработки должны быть установлены следующие компоненты.

- [Пакет SDK для .NET Core](https://dotnet.microsoft.com/download)
- [Эмулятор ленты](https://github.com/microsoft/BotFramework-Emulator/blob/master/README.md)

Кроме того, у вас также должна быть личная учетная запись Майкрософт с почтовым ящиком на Outlook.com или рабочей или учебной учетной записью Майкрософт. Если у вас нет учетной записи Майкрософт, у вас есть несколько вариантов для получения бесплатной учетной записи:

- Вы можете [зарегистрироваться для создания новой личной учетной записи Майкрософт](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).
- Вы можете [зарегистрироваться в программе для разработчиков office 365](https://developer.microsoft.com/office/dev-program) , чтобы получить бесплатную подписку на Office 365.
- Подписка на Azure. Если у вас его нет, создайте [бесплатную учетную запись](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) , прежде чем приступать к работе.

> [!NOTE]
> Это руководство было написано в пакете SDK для .NET Core версии 3.1.401. Действия, описанные в этом руководстве, могут работать с другими версиями, но не тестировались.

## <a name="feedback"></a>Отзывы

Сообщите о нем в [репозиторий GitHub](https://github.com/microsoftgraph/msgraph-training-botframework).
