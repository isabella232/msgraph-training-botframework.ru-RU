---
ms.openlocfilehash: ced0e75c32d26aed43afa61d498f2f0c438bf2d0
ms.sourcegitcommit: e0d9b18d2d4cbeb4a48890f3420a47e6a90abc53
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/17/2020
ms.locfileid: "49347919"
---
<!-- markdownlint-disable MD002 MD041 -->

В этом разделе вы будете использовать пакет SDK Microsoft Graph, чтобы получить вошедшего в систему пользователя.

## <a name="create-a-graph-service"></a>Создание службы Graph

Начните с реализации службы, которую может использовать Bot для получения **GraphServiceClient** из пакета SDK Microsoft Graph, а затем сделайте эту службу доступной для ленты с помощью внедрения зависимостей.

1. Создайте новый каталог в корневом каталоге проекта с именем **Graph**. Создайте новый файл в каталоге **./граф** с именем **IGraphClientService.CS** и добавьте следующий код.

    :::code language="csharp" source="../demo/GraphCalendarBot/Graph/IGraphClientService.cs" id="IGraphClientServiceSnippet":::

1. Создайте новый файл в каталоге **./граф** с именем **GraphClientService.CS** и добавьте следующий код.

    :::code language="csharp" source="../demo/GraphCalendarBot/Graph/GraphClientService.cs" id="GraphClientServiceSnippet":::

1. Откройте **./Startup.CS** и добавьте следующий код в конец `ConfigureServices` функции.

    :::code language="csharp" source="../demo/GraphCalendarBot/Startup.cs" id="AddGraphServiceSnippet":::

1. Открыть **./диалогс/маиндиалог.КС**. Добавьте приведенные ниже `using` операторы в начало файла.

    ```csharp
    using System;
    using System.IO;
    using CalendarBot.Graph;
    using AdaptiveCards;
    using Microsoft.Graph;
    ```

1. Добавьте в класс **маиндиалог** следующее свойство.

    ```csharp
    private readonly IGraphClientService _graphClientService;
    ```

1. Нахождение конструктора для класса **маиндиалог** и обновление его подписи для получения параметра **играфсервицеклиент** .

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/MainDialog.cs" id="ConstructorSignatureSnippet" highlight="4":::

1. Добавьте следующий код в конструктор.

    ```csharp
    _graphClientService = graphClientService;
    ```

## <a name="get-the-logged-on-user"></a>Получение пользователя, выполнившего вход в систему

В этом разделе вы будете использовать Microsoft Graph, чтобы получить имя пользователя, адрес электронной почты и фотографию. Затем вы создадите адаптивную карточку для отображения информации.

1. Создайте в корневом каталоге проекта новый файл с именем **CardHelper.CS**. Добавьте указанный ниже код в файл.

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

    В этом коде используется пакет NuGet **адаптивекард** для создания адаптивной карточки для отображения пользователя.

1. Добавьте указанную ниже функцию в класс **маиндиалог** .

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/MainDialog.cs" id="DisplayLoggedInUserSnippet":::

    Рассмотрите, что делает этот код.

    - Он использует **графклиент** , чтобы [получить вошедшего в систему пользователя](https://docs.microsoft.com/graph/api/user-get?view=graph-rest-1.0).
        - Он использует `Select` метод, чтобы ограничить возвращаемые поля.
    - Он использует **графклиент** , чтобы [получить фотографию пользователя](https://docs.microsoft.com/graph/api/profilephoto-get?view=graph-rest-1.0), запросив минимальный поддерживаемый размер в 48x48 пикселях.
    - Он использует класс **кардхелпер** для создания адаптивной карточки и отправляет карту в виде вложения.

1. Замените код внутри `else if (command.StartsWith("show me"))` блока на `ProcessStepAsync` приведенный ниже код.

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/MainDialog.cs" id="ShowMeSnippet" highlight="3":::

1. Сохраните все изменения и перезапустите Bot.

1. С помощью эмулятора Bot Framework подключитесь к Bot и войдите в систему. Нажмите кнопку **Show Me** , чтобы отобразить пользователя, вошедшего в систему.

    ![Снимок экрана со страницей адаптивной карточки, в которой отображается пользователь](images/user-card.png)
