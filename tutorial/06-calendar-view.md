---
ms.openlocfilehash: 65fd8e133174c6ac7bbbbc00487cabc72ebe561d
ms.sourcegitcommit: e0d9b18d2d4cbeb4a48890f3420a47e6a90abc53
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/17/2020
ms.locfileid: "49347925"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="0a2a0-101">В этом разделе вы будете использовать пакет SDK Microsoft Graph для получения следующих 3 ближайших событий в календаре пользователя за текущую неделю.</span><span class="sxs-lookup"><span data-stu-id="0a2a0-101">In this section you'll use the Microsoft Graph SDK to get the next 3 upcoming events on the user's calendar for the current week.</span></span>

## <a name="get-a-calendar-view"></a><span data-ttu-id="0a2a0-102">Получение представления календаря</span><span class="sxs-lookup"><span data-stu-id="0a2a0-102">Get a calendar view</span></span>

<span data-ttu-id="0a2a0-103">Представление календаря — это список событий в календаре пользователя, которые находятся между двумя значениями даты и времени.</span><span class="sxs-lookup"><span data-stu-id="0a2a0-103">A calendar view is a list of events on a user's calendar that fall between two date/time values.</span></span> <span data-ttu-id="0a2a0-104">Преимущество использования представления календаря состоит в том, что оно включает все экземпляры повторяющихся собраний.</span><span class="sxs-lookup"><span data-stu-id="0a2a0-104">The advantage of using a calendar view is that it includes any occurrences of recurring meetings.</span></span>

1. <span data-ttu-id="0a2a0-105">Откройте **./CardHelper.CS** и добавьте приведенную ниже функцию в класс **кардхелпер** .</span><span class="sxs-lookup"><span data-stu-id="0a2a0-105">Open **./CardHelper.cs** and add the following function to the **CardHelper** class.</span></span>

    :::code language="csharp" source="../demo/GraphCalendarBot/CardHelper.cs" id="GetEventCardSnippet":::

    <span data-ttu-id="0a2a0-106">Этот код создает адаптивную карточку для отображения события календаря.</span><span class="sxs-lookup"><span data-stu-id="0a2a0-106">This code builds an Adaptive Card to render a calendar event.</span></span>

1. <span data-ttu-id="0a2a0-107">Откройте **/диалогс/маиндиалог.КС** и добавьте приведенную ниже функцию в класс **маиндиалог** .</span><span class="sxs-lookup"><span data-stu-id="0a2a0-107">Open **./Dialogs/MainDialog.cs** and add the following function to the **MainDialog** class.</span></span>

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/MainDialog.cs" id="DisplayCalendarViewSnippet":::

    <span data-ttu-id="0a2a0-108">Рассмотрите, что делает этот код.</span><span class="sxs-lookup"><span data-stu-id="0a2a0-108">Consider what this code does.</span></span>

    - <span data-ttu-id="0a2a0-109">Он получает **MailboxSettings** пользователя, чтобы определить предпочитаемый пользователем часовой пояс и форматы даты и времени.</span><span class="sxs-lookup"><span data-stu-id="0a2a0-109">It gets the user's **MailboxSettings** to determine the user's preferred time zone and date/time formats.</span></span>
    - <span data-ttu-id="0a2a0-110">Значения **startDateTime** и **endDateTime** задаются на текущий момент и конец недели соответственно.</span><span class="sxs-lookup"><span data-stu-id="0a2a0-110">It sets the **startDateTime** and **endDateTime** values to now and the end of the week, respectively.</span></span> <span data-ttu-id="0a2a0-111">Этот параметр определяет окно времени, которое используется представлением календаря.</span><span class="sxs-lookup"><span data-stu-id="0a2a0-111">This defines the window of time that the calendar view uses.</span></span>
    - <span data-ttu-id="0a2a0-112">Она вызывается `graphClient.Me.CalendarView` со следующими сведениями.</span><span class="sxs-lookup"><span data-stu-id="0a2a0-112">It calls `graphClient.Me.CalendarView` with the following details.</span></span>
        - <span data-ttu-id="0a2a0-113">Он задает `Prefer: outlook.timezone` для заголовка предпочтительный часовой пояс пользователя, что приводит к тому, что время начала и окончания события будут находиться на часовом поясе пользователя.</span><span class="sxs-lookup"><span data-stu-id="0a2a0-113">It sets the `Prefer: outlook.timezone` header to the user's preferred time zone, causing the start and end times for the events to be in the user's timezone.</span></span>
        - <span data-ttu-id="0a2a0-114">Он использует `Top(3)` метод, чтобы ограничить результаты только первыми 3 событиями.</span><span class="sxs-lookup"><span data-stu-id="0a2a0-114">It uses the `Top(3)` method to limit the results to only the first 3 events.</span></span>
        - <span data-ttu-id="0a2a0-115">Он используется `Select` , чтобы ограничить возвращаемые поля только теми полями, которые используются элементом Bot.</span><span class="sxs-lookup"><span data-stu-id="0a2a0-115">It uses `Select` to limit the fields returned to just those fields used by the bot.</span></span>
        - <span data-ttu-id="0a2a0-116">Он используется `OrderBy` для сортировки событий по времени начала.</span><span class="sxs-lookup"><span data-stu-id="0a2a0-116">It uses `OrderBy` to sort the events by start time.</span></span>
    - <span data-ttu-id="0a2a0-117">Добавляет адаптивную карточку для каждого события в ответное сообщение.</span><span class="sxs-lookup"><span data-stu-id="0a2a0-117">It adds an Adaptive Card for each event to the reply message.</span></span>

1. <span data-ttu-id="0a2a0-118">Замените код внутри `else if (command.StartsWith("show calendar"))` блока на `ProcessStepAsync` приведенный ниже код.</span><span class="sxs-lookup"><span data-stu-id="0a2a0-118">Replace the code inside the `else if (command.StartsWith("show calendar"))` block in `ProcessStepAsync` with the following.</span></span>

    :::code language="csharp" source="../demo/GraphCalendarBot/Dialogs/MainDialog.cs" id="ShowCalendarSnippet" highlight="3":::

1. <span data-ttu-id="0a2a0-119">Сохраните все изменения и перезапустите Bot.</span><span class="sxs-lookup"><span data-stu-id="0a2a0-119">Save all of your changes and restart the bot.</span></span>

1. <span data-ttu-id="0a2a0-120">С помощью эмулятора Bot Framework подключитесь к Bot и войдите в систему.</span><span class="sxs-lookup"><span data-stu-id="0a2a0-120">Use the Bot Framework Emulator to connect to the bot and log in.</span></span> <span data-ttu-id="0a2a0-121">Нажмите кнопку **Показать календарь** , чтобы открыть представление календаря.</span><span class="sxs-lookup"><span data-stu-id="0a2a0-121">Select the **Show calendar** button to display the calendar view.</span></span>

    ![Снимок экрана: Адаптивная карточка, в которой показаны три следующих события](images/calendar-view.png)
