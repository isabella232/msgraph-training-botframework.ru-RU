---
ms.openlocfilehash: 0022448db76695894bfefca2543ad39c4fd07dcc
ms.sourcegitcommit: e0d9b18d2d4cbeb4a48890f3420a47e6a90abc53
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/17/2020
ms.locfileid: "49347895"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="00d39-101">В этом руководстве рассказывается, как создать робота Bot Framework, который использует API Microsoft Graph для получения сведений о календаре для пользователя.</span><span class="sxs-lookup"><span data-stu-id="00d39-101">This tutorial teaches you how to build a Bot Framework bot that uses the Microsoft Graph API to retrieve calendar information for a user.</span></span>

> [!TIP]
> <span data-ttu-id="00d39-102">Если вы предпочитаете просто скачать заполненный учебник, вы можете скачать или клонировать [репозиторий GitHub](https://github.com/microsoftgraph/msgraph-training-botframework).</span><span class="sxs-lookup"><span data-stu-id="00d39-102">If you prefer to just download the completed tutorial, you can download or clone the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-botframework).</span></span> <span data-ttu-id="00d39-103">Ознакомьтесь с файлом README в папке **Demo** , чтобы получить инструкции по настройке приложения с идентификатором и секретом приложения.</span><span class="sxs-lookup"><span data-stu-id="00d39-103">See the README file in the **demo** folder for instructions on configuring the app with an app ID and secret.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="00d39-104">Предварительные условия</span><span class="sxs-lookup"><span data-stu-id="00d39-104">Prerequisites</span></span>

<span data-ttu-id="00d39-105">Прежде чем приступить к работе с этим руководством, на вашем компьютере для разработки должны быть установлены следующие компоненты.</span><span class="sxs-lookup"><span data-stu-id="00d39-105">Before you start this tutorial, you should have the following installed on your development machine.</span></span>

- [<span data-ttu-id="00d39-106">Пакет SDK для .NET Core</span><span class="sxs-lookup"><span data-stu-id="00d39-106">.NET Core SDK</span></span>](https://dotnet.microsoft.com/download)
- [<span data-ttu-id="00d39-107">Эмулятор ленты</span><span class="sxs-lookup"><span data-stu-id="00d39-107">Bot Framework Emulator</span></span>](https://github.com/microsoft/BotFramework-Emulator/blob/master/README.md)

<span data-ttu-id="00d39-108">Кроме того, у вас также должна быть личная учетная запись Майкрософт с почтовым ящиком на Outlook.com или рабочей или учебной учетной записью Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="00d39-108">You should also have either a personal Microsoft account with a mailbox on Outlook.com, or a Microsoft work or school account.</span></span> <span data-ttu-id="00d39-109">Если у вас нет учетной записи Майкрософт, у вас есть несколько вариантов для получения бесплатной учетной записи:</span><span class="sxs-lookup"><span data-stu-id="00d39-109">If you don't have a Microsoft account, there are a couple of options to get a free account:</span></span>

- <span data-ttu-id="00d39-110">Вы можете [зарегистрироваться для создания новой личной учетной записи Майкрософт](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span><span class="sxs-lookup"><span data-stu-id="00d39-110">You can [sign up for a new personal Microsoft account](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span></span>
- <span data-ttu-id="00d39-111">Вы можете [зарегистрироваться в программе для разработчиков office 365](https://developer.microsoft.com/office/dev-program) , чтобы получить бесплатную подписку на Office 365.</span><span class="sxs-lookup"><span data-stu-id="00d39-111">You can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>
- <span data-ttu-id="00d39-112">Подписка на Azure.</span><span class="sxs-lookup"><span data-stu-id="00d39-112">An Azure subscription.</span></span> <span data-ttu-id="00d39-113">Если у вас его нет, создайте [бесплатную учетную запись](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) , прежде чем приступать к работе.</span><span class="sxs-lookup"><span data-stu-id="00d39-113">If you do not have one, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

> [!NOTE]
> <span data-ttu-id="00d39-114">Это руководство было написано в пакете SDK для .NET Core версии 3.1.401.</span><span class="sxs-lookup"><span data-stu-id="00d39-114">This tutorial was written with .NET Core SDK version 3.1.401.</span></span> <span data-ttu-id="00d39-115">Действия, описанные в этом руководстве, могут работать с другими версиями, но не тестировались.</span><span class="sxs-lookup"><span data-stu-id="00d39-115">The steps in this guide may work with other versions, but that has not been tested.</span></span>

## <a name="feedback"></a><span data-ttu-id="00d39-116">Отзывы</span><span class="sxs-lookup"><span data-stu-id="00d39-116">Feedback</span></span>

<span data-ttu-id="00d39-117">Сообщите о нем в [репозиторий GitHub](https://github.com/microsoftgraph/msgraph-training-botframework).</span><span class="sxs-lookup"><span data-stu-id="00d39-117">Please provide any feedback on this tutorial in the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-botframework).</span></span>
