---
ms.openlocfilehash: ced0e75c32d26aed43afa61d498f2f0c438bf2d0
ms.sourcegitcommit: e0d9b18d2d4cbeb4a48890f3420a47e6a90abc53
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/17/2020
ms.locfileid: "49347919"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="3f60d-101">В этом разделе вы будете использовать пакет SDK Microsoft Graph, чтобы получить вошедшего в систему пользователя.</span><span class="sxs-lookup"><span data-stu-id="3f60d-101">In this section you'll use the Microsoft Graph SDK to get the logged-in user.</span></span>

## <a name="create-a-graph-service"></a><span data-ttu-id="3f60d-102">Создание службы Graph</span><span class="sxs-lookup"><span data-stu-id="3f60d-102">Create a Graph service</span></span>

<span data-ttu-id="3f60d-103">Начните с реализации службы, которую может использовать Bot для получения **GraphServiceClient** из пакета SDK Microsoft Graph, а затем сделайте эту службу доступной для ленты с помощью внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="3f60d-103">Start by implementing a service that the bot can use to get a **GraphServiceClient** from the Microsoft Graph SDK, then making that service available to the bot via dependency injection.</span></span>

1. <span data-ttu-id="3f60d-104">Создайте новый каталог в корневом каталоге проекта с именем **Graph**.</span><span class="sxs-lookup"><span data-stu-id="3f60d-104">Create a new directory in the root of the project named **Graph**.</span></span> <span data-ttu-id="3f60d-105">Создайте новый файл в каталоге **./граф** с именем **IGraphClientService.CS** и добавьте следующий код.</span><span class="sxs-lookup"><span data-stu-id="3f60d-105">Create a new file in the **./Graph** directory named **IGraphClientService.cs** and add the following code.</span></span>

    :::code language="csharp" source="../demo/GraphCalendarBot/Graph/IGraphClientService.cs" id="IGraphClientServiceSnippet":::

1. <span data-ttu-id="3f60d-106">Создайте новый файл в каталоге **./граф** с именем **GraphClientService.CS** и добавьте следующий код.</span><span class="sxs-lookup"><span data-stu-id="3f60d-106">Create a new file in the **./Graph** directory named **GraphClientService.cs** and add the following code.</span></span>

    :::code language="csharp" source="../demo/GraphCalendarBot/Graph/GraphClientService.cs" id="GraphClientServiceSnippet":::

1. <span data-ttu-id="3f60d-107">Откройте **./Startup.CS** и добавьте следующий код в конец `ConfigureServices` функции.</span><span class="sxs-lookup"><span data-stu-id="3f60d-107">Open **./Startup.cs** and add the following code to the end of the `ConfigureServices` function.</span></span>

    :::code language="csharp" source="../demo/GraphCalendarBot/Startup.cs" id="AddGraphServiceSnippet":::

1. <span data-ttu-id="3f60d-108">Открыть **./диалогс/маиндиалог.КС**.</span><span class="sxs-lookup"><span data-stu-id="3f60d-108">Open **./Dialogs/MainDialog.cs**.</span></span> <span data-ttu-id="3f60d-109">Добавьте приведенные ниже `using` операторы в начало файла.</span><span class="sxs-lookup"><span data-stu-id="3f60d-109">Add the following `using` statements to the top of the file.</span></span>

    ```csharp
    using System;
    using System.IO;
    using CalendarBot.Graph;
    using AdaptiveCards;
    using Microsoft.Graph;
    ```

1. <span data-ttu-id="3f60d-110">Добавьте в класс **маиндиалог** следующее свойство.</span><span class="sxs-lookup"><span data-stu-id="3f60d-110">Add the following property to the **MainDialog** class.</span></span>

    ```csharp
    private readonly IGraphClientService _graphClientService;
    ```

1. <span data-ttu-id="3f60d-111">Нахождение конструктора для класса **маиндиалог** и обновление его подписи для получения параметра **играфсервицеклиент** .</span><span class="sxs-lookup"><span data-stu-id="3f60d-111">Locate the constructor for the **MainDialog** class and update its signature to take an **IGraphServiceClient** parameter.</span></span>

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/MainDialog.cs" id="ConstructorSignatureSnippet" highlight="4":::

1. <span data-ttu-id="3f60d-112">Добавьте следующий код в конструктор.</span><span class="sxs-lookup"><span data-stu-id="3f60d-112">Add the following code to the constructor.</span></span>

    ```csharp
    _graphClientService = graphClientService;
    ```

## <a name="get-the-logged-on-user"></a><span data-ttu-id="3f60d-113">Получение пользователя, выполнившего вход в систему</span><span class="sxs-lookup"><span data-stu-id="3f60d-113">Get the logged on user</span></span>

<span data-ttu-id="3f60d-114">В этом разделе вы будете использовать Microsoft Graph, чтобы получить имя пользователя, адрес электронной почты и фотографию.</span><span class="sxs-lookup"><span data-stu-id="3f60d-114">In this section you'll use the Microsoft Graph to get the user's name, email address, and photo.</span></span> <span data-ttu-id="3f60d-115">Затем вы создадите адаптивную карточку для отображения информации.</span><span class="sxs-lookup"><span data-stu-id="3f60d-115">Then you'll create an Adaptive Card to show the information.</span></span>

1. <span data-ttu-id="3f60d-116">Создайте в корневом каталоге проекта новый файл с именем **CardHelper.CS**.</span><span class="sxs-lookup"><span data-stu-id="3f60d-116">Create a new file in the root of the project named **CardHelper.cs**.</span></span> <span data-ttu-id="3f60d-117">Добавьте указанный ниже код в файл.</span><span class="sxs-lookup"><span data-stu-id="3f60d-117">Add the following code to the file.</span></span>

    ```csharp
    using AdaptiveCards;
    using Microsoft.Graph;
    using System;
    using System.IO;

    namespace CalendarBot
    {
        public class CardHelper
        {
            public static AdaptiveCard GetUserCard(User user, Stream photo)
            {
              // Create an Adaptive Card to display the user
                // See https://adaptivecards.io/designer/ for possibilities
                var userCard = new AdaptiveCard("1.2");

                var columns = new AdaptiveColumnSet();
                userCard.Body.Add(columns);

                var userPhotoColumn = new AdaptiveColumn { Width = AdaptiveColumnWidth.Auto };
                columns.Columns.Add(userPhotoColumn);

                userPhotoColumn.Items.Add(new AdaptiveImage {
                    Style = AdaptiveImageStyle.Person,
                    Size = AdaptiveImageSize.Small,
                    Url = GetDataUriFromPhoto(photo)
                });

                var userInfoColumn = new AdaptiveColumn {Width = AdaptiveColumnWidth.Stretch };
                columns.Columns.Add(userInfoColumn);

                userInfoColumn.Items.Add(new AdaptiveTextBlock {
                    Weight = AdaptiveTextWeight.Bolder,
                    Wrap = true,
                    Text = user.DisplayName
                });

                userInfoColumn.Items.Add(new AdaptiveTextBlock {
                    Spacing = AdaptiveSpacing.None,
                    IsSubtle = true,
                    Wrap = true,
                    Text = user.Mail ?? user.UserPrincipalName
                });

                return userCard;
            }

            private static Uri GetDataUriFromPhoto(Stream photo)
            {
                // Copy to a MemoryStream to get access to bytes
                var photoStream = new MemoryStream();
                photo.CopyTo(photoStream);

                var photoBytes = photoStream.ToArray();

                return new Uri($"data:image/png;base64,{Convert.ToBase64String(photoBytes)}");
            }
        }
    }
    ```

    <span data-ttu-id="3f60d-118">В этом коде используется пакет NuGet **адаптивекард** для создания адаптивной карточки для отображения пользователя.</span><span class="sxs-lookup"><span data-stu-id="3f60d-118">This code uses the **AdaptiveCard** NuGet package to build an Adaptive Card to display the user.</span></span>

1. <span data-ttu-id="3f60d-119">Добавьте указанную ниже функцию в класс **маиндиалог** .</span><span class="sxs-lookup"><span data-stu-id="3f60d-119">Add the following function to the **MainDialog** class.</span></span>

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/MainDialog.cs" id="DisplayLoggedInUserSnippet":::

    <span data-ttu-id="3f60d-120">Рассмотрите, что делает этот код.</span><span class="sxs-lookup"><span data-stu-id="3f60d-120">Consider what this code does.</span></span>

    - <span data-ttu-id="3f60d-121">Он использует **графклиент** , чтобы [получить вошедшего в систему пользователя](https://docs.microsoft.com/graph/api/user-get?view=graph-rest-1.0).</span><span class="sxs-lookup"><span data-stu-id="3f60d-121">It uses the **graphClient** to [get the logged-in user](https://docs.microsoft.com/graph/api/user-get?view=graph-rest-1.0).</span></span>
        - <span data-ttu-id="3f60d-122">Он использует `Select` метод, чтобы ограничить возвращаемые поля.</span><span class="sxs-lookup"><span data-stu-id="3f60d-122">It uses the `Select` method to limit which fields are returned.</span></span>
    - <span data-ttu-id="3f60d-123">Он использует **графклиент** , чтобы [получить фотографию пользователя](https://docs.microsoft.com/graph/api/profilephoto-get?view=graph-rest-1.0), запросив минимальный поддерживаемый размер в 48x48 пикселях.</span><span class="sxs-lookup"><span data-stu-id="3f60d-123">It uses the **graphClient** to [get the user's photo](https://docs.microsoft.com/graph/api/profilephoto-get?view=graph-rest-1.0), requesting the smallest supported size of 48x48 pixels.</span></span>
    - <span data-ttu-id="3f60d-124">Он использует класс **кардхелпер** для создания адаптивной карточки и отправляет карту в виде вложения.</span><span class="sxs-lookup"><span data-stu-id="3f60d-124">It uses the **CardHelper** class to construct an Adaptive Card and sends the card as an attachment.</span></span>

1. <span data-ttu-id="3f60d-125">Замените код внутри `else if (command.StartsWith("show me"))` блока на `ProcessStepAsync` приведенный ниже код.</span><span class="sxs-lookup"><span data-stu-id="3f60d-125">Replace the code inside the `else if (command.StartsWith("show me"))` block in `ProcessStepAsync` with the following.</span></span>

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/MainDialog.cs" id="ShowMeSnippet" highlight="3":::

1. <span data-ttu-id="3f60d-126">Сохраните все изменения и перезапустите Bot.</span><span class="sxs-lookup"><span data-stu-id="3f60d-126">Save all of your changes and restart the bot.</span></span>

1. <span data-ttu-id="3f60d-127">С помощью эмулятора Bot Framework подключитесь к Bot и войдите в систему.</span><span class="sxs-lookup"><span data-stu-id="3f60d-127">Use the Bot Framework Emulator to connect to the bot and log in.</span></span> <span data-ttu-id="3f60d-128">Нажмите кнопку **Show Me** , чтобы отобразить пользователя, вошедшего в систему.</span><span class="sxs-lookup"><span data-stu-id="3f60d-128">Select the **Show me** button to display the logged-on user.</span></span>

    ![Снимок экрана со страницей адаптивной карточки, в которой отображается пользователь](images/user-card.png)
