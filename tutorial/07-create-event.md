---
ms.openlocfilehash: dc5816eb653053d63bfe3bf48413b3557583d109
ms.sourcegitcommit: e0d9b18d2d4cbeb4a48890f3420a47e6a90abc53
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/17/2020
ms.locfileid: "49347848"
---
<!-- markdownlint-disable MD002 MD041 -->

В этом разделе описывается добавление события в календарь пользователя с помощью пакета SDK Microsoft Graph.

## <a name="implement-a-dialog"></a>Реализация диалогового окна

Сначала создайте новое настраиваемое диалоговое окно, в котором пользователю предлагается ввести значения, необходимые для добавления события в календарь. В этом диалоговом окне будет использоваться **ватерфаллдиалог** для выполнения следующих действий.

- Запрос темы
- Спросите, хочет ли пользователь пригласить людей
- Запрашивать участников (если пользователь сказал "Да" на предыдущем шаге)
- Запрашивать дату и время начала
- Запрашивать дату и время окончания
- Отображение всех собранных значений и попросите пользователя подтвердить
- Если пользователь подтверждает, получает маркер доступа из **оауспромпт**
- Создание события

1. Создайте новый файл в каталоге **./диалогс** с именем **NewEventDialog.CS** и добавьте следующий код.

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.Bot.Builder;
    using Microsoft.Bot.Builder.Dialogs;
    using Microsoft.Bot.Schema;
    using Microsoft.Extensions.Configuration;
    using Microsoft.Extensions.Logging;
    using Microsoft.Graph;
    using CalendarBot.Graph;
    using Microsoft.Recognizers.Text.DataTypes.TimexExpression;
    using TimexTypes = Microsoft.Recognizers.Text.DataTypes.TimexExpression.Constants.TimexTypes;

    namespace CalendarBot.Dialogs
    {
        public class NewEventDialog : LogoutDialog
        {
            protected readonly ILogger _logger;
            private readonly IGraphClientService _graphClientService;

            public NewEventDialog(
                IConfiguration configuration,
                IGraphClientService graphClientService)
                : base(nameof(NewEventDialog), configuration["ConnectionName"])
            {

            }

            // Generate a DateTime from the list of
            // DateTimeResolutions provided by the DateTimePrompt
            private static DateTime GetDateTimeFromResolutions(IList<DateTimeResolution> resolutions)
            {
                var timex = new TimexProperty(resolutions[0].Timex);

                // Handle the "now" case
                if (timex.Now ?? false)
                {
                    return DateTime.Now;
                }

                // Otherwise generate a DateTime
                return TimexHelpers.DateFromTimex(timex);
            }
        }
    }
    ```

    Это оболочка нового диалогового окна, в котором пользователю предлагается ввести значения, необходимые для добавления события в календарь.

1. Добавьте указанную ниже функцию в класс **невевентдиалог** , чтобы запросить у пользователя тему.

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="PromptForSubjectSnippet":::

1. Добавьте указанную ниже функцию в класс **невевентдиалог** , чтобы сохранить субъекта, который пользователь дал на предыдущем шаге, и спросите, хотите ли вы добавить участников.

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="PromptForAddAttendeesSnippet":::

1. Добавьте указанную ниже функцию в класс **невевентдиалог** , чтобы проверить ответ пользователя на предыдущий шаг и при необходимости запросить у пользователя список участников.

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="PromptForAttendeesSnippet":::

1. Добавьте указанную ниже функцию в класс **невевентдиалог** , чтобы сохранить список участников из предыдущего этапа (если он есть) и запросить у пользователя дату и время начала.

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="PromptForStartSnippet":::

1. Добавьте указанную ниже функцию в класс **невевентдиалог** , чтобы сохранить начальное значение из предыдущего действия, и запросите у пользователя дату и время окончания.

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="PromptForEndSnippet":::

1. Добавьте указанную ниже функцию в класс **невевентдиалог** , чтобы сохранить конечное значение из предыдущего действия, и попросите пользователя подтвердить все входные данные.

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="ConfirmNewEventSnippet":::

1. Добавьте указанную ниже функцию в класс **невевентдиалог** , чтобы проверить ответ пользователя, полученный на предыдущем шаге. Если пользователь подтверждает входные данные, используйте класс **оауспромпт** для получения маркера доступа. В противном случае завершите диалоговое окно.

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="GetTokenSnippet":::

1. Добавьте указанную ниже функцию в класс **невевентдиалог** , чтобы использовать пакет SDK Microsoft Graph для создания нового события.

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="AddEventSnippet":::

    Рассмотрите, что делает этот код.

    - Он получает **MailboxSettings** пользователя для определения предпочтительного часового пояса пользователя.
    - Он создает объект **event** , используя значения, предоставленные пользователем. Обратите внимание, что свойства **Start** и **End** задаются с помощью часового пояса пользователя.
    - Он создает событие в календаре пользователя.

## <a name="add-validation"></a>Добавление проверки

Теперь добавьте проверку на входные данные пользователя, чтобы избежать ошибок при создании события с помощью Microsoft Graph. Мы хотим убедиться, что:

- Если пользователь предоставляет список участников, это должен быть список допустимых адресов электронной почты, разделенных точкой с запятой.
- Дата и время начала должны быть действительными датой и временем.
- Дата и время окончания должны быть допустимыми датой и временем, а также должно быть позже начала.

1. Добавьте указанную ниже функцию в класс **невевентдиалог** , чтобы проверить запись пользователя для участников.

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="AttendeesValidatorSnippet":::

1. Добавьте следующие функции в класс **невевентдиалог** , чтобы проверить запись пользователя на наличие даты и времени начала.

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="StartValidatorSnippet":::

1. Добавьте указанную ниже функцию в класс **невевентдиалог** , чтобы проверить запись пользователя на наличие даты и времени окончания.

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="EndValidatorSnippet":::

## <a name="add-steps-to-waterfalldialog"></a>Добавление шагов в Ватерфаллдиалог

Теперь, когда у вас есть все действия в диалоговом окне, необходимо добавить их в **ватерфаллдиалог** в конструкторе, а затем добавить **невевентдиалог** в **маиндиалог**.

1. Откройте конструктор **невевентдиалог** и добавьте в него следующий код.

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/NewEventDialog.cs" id="ConstructorSnippet":::

    При этом добавляются все используемые диалоговые окна и добавляются все функции, реализованные в качестве действий в **ватерфаллдиалог**.

1. Откройте **./диалогс/маиндиалог.КС** и добавьте в конструктор следующую строку.

    ```csharp
    AddDialog(new NewEventDialog(configuration, graphClientService));
    ```

1. Замените код внутри `else if (command.StartsWith("add event"))` блока на `ProcessStepAsync` приведенный ниже код.

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/MainDialog.cs" id="AddEventSnippet" highlight="3":::

1. Сохраните все изменения и перезапустите Bot.

1. С помощью эмулятора Bot Framework подключитесь к Bot и войдите в систему. Нажмите кнопку **Добавить событие** .

1. Ответьте на приглашения создать новое событие. Обратите внимание, что при запросе начальных и конечных значений можно использовать фразу "сегодня в 3PM" или "Now".

    ![Снимок экрана с диалоговым окном добавления события в эмуляторе Bot Framework](images/add-event.png)
