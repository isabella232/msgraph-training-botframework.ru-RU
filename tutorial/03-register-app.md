---
ms.openlocfilehash: 9a320225c7e7e76506d73909a311fa019b1f74f3
ms.sourcegitcommit: e0d9b18d2d4cbeb4a48890f3420a47e6a90abc53
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/17/2020
ms.locfileid: "49347859"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="82161-101">В этом упражнении вы создадите новую регистрацию каналов Bot и регистрацию веб-приложения Azure AD с помощью портала Azure.</span><span class="sxs-lookup"><span data-stu-id="82161-101">In this exercise, you will create a new Bot Channels registration and an Azure AD web application registration using the Azure Portal.</span></span>

## <a name="create-a-bot-channels-registration"></a><span data-ttu-id="82161-102">Создание регистрации каналов канала ленты</span><span class="sxs-lookup"><span data-stu-id="82161-102">Create a Bot Channels registration</span></span>

1. <span data-ttu-id="82161-103">Откройте браузер и перейдите на [портал Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="82161-103">Open a browser and navigate to the [Azure Portal](https://portal.azure.com).</span></span> <span data-ttu-id="82161-104">Войдите, используя учетную запись, связанную с вашей подпиской Azure.</span><span class="sxs-lookup"><span data-stu-id="82161-104">Login using the account associated with your Azure subscription.</span></span>

1. <span data-ttu-id="82161-105">Выберите меню в левом верхнем углу, а затем выберите команду **создать ресурс**.</span><span class="sxs-lookup"><span data-stu-id="82161-105">Select the upper-left menu, then select **Create a resource**.</span></span>

    ![Снимок экрана: меню портала Azure](images/create-resource.png)

1. <span data-ttu-id="82161-107">На **новой** странице найдите `Bot Channel` и выберите **Регистрация каналов ленты**.</span><span class="sxs-lookup"><span data-stu-id="82161-107">On the **New** page, search for `Bot Channel` and select **Bot Channels Registration**.</span></span>

1. <span data-ttu-id="82161-108">На странице **Регистрация каналов Bot** нажмите кнопку **создать**.</span><span class="sxs-lookup"><span data-stu-id="82161-108">On the **Bot Channels Registration** page, select **Create**.</span></span>

1. <span data-ttu-id="82161-109">Заполните обязательные поля и оставьте поле **Конечная точка обмена сообщениями** пустыми.</span><span class="sxs-lookup"><span data-stu-id="82161-109">Fill in the required fields, and leave **Messaging endpoint** blank.</span></span> <span data-ttu-id="82161-110">Поле **маркера Bot** должно быть уникальным.</span><span class="sxs-lookup"><span data-stu-id="82161-110">The **Bot handle** field must be unique.</span></span> <span data-ttu-id="82161-111">Обязательно изучите различные ценовые категории и выберите, что имеет смысл для вашего сценария.</span><span class="sxs-lookup"><span data-stu-id="82161-111">Be sure to review the different pricing tiers and select what makes sense for your scenario.</span></span> <span data-ttu-id="82161-112">Если это всего лишь учебное упражнение, вы можете выбрать параметр Free (бесплатный).</span><span class="sxs-lookup"><span data-stu-id="82161-112">If this is just a learning exercise, you may want to select the free option.</span></span>

1. <span data-ttu-id="82161-113">Выберите **идентификатор приложения и пароль**, а затем выберите **создать новый**.</span><span class="sxs-lookup"><span data-stu-id="82161-113">Select the **Microsoft App ID and password**, then select **Create New**.</span></span>

1. <span data-ttu-id="82161-114">Выберите **создать идентификатор приложения на портале регистрации приложений**.</span><span class="sxs-lookup"><span data-stu-id="82161-114">Select **Create App ID in the App Registration Portal**.</span></span> <span data-ttu-id="82161-115">Откроется новое окно или вкладка в колонке **регистрации приложений** на портале Azure.</span><span class="sxs-lookup"><span data-stu-id="82161-115">This will open a new window or tab to the **App registrations** blade in the Azure Portal.</span></span>

1. <span data-ttu-id="82161-116">В колонке **Регистрация приложений** выберите пункт **создать регистрацию**.</span><span class="sxs-lookup"><span data-stu-id="82161-116">In the **App registrations** blade, select **New registration**.</span></span>

1. <span data-ttu-id="82161-117">Задайте значения следующим образом.</span><span class="sxs-lookup"><span data-stu-id="82161-117">Set the values as follows.</span></span>

    - <span data-ttu-id="82161-118">Введите **имя** `Graph Calendar Bot`.</span><span class="sxs-lookup"><span data-stu-id="82161-118">Set **Name** to `Graph Calendar Bot`.</span></span>
    - <span data-ttu-id="82161-119">Введите **поддерживаемые типы учетных записей** для **учетных записей в любом каталоге организаций и личных учетных записей Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="82161-119">Set **Supported account types** to **Accounts in any organizational directory and personal Microsoft accounts**.</span></span>
    - <span data-ttu-id="82161-120">Оставьте поле **URI перенаправления** пустым.</span><span class="sxs-lookup"><span data-stu-id="82161-120">Leave **Redirect URI** empty.</span></span>

    ![Снимок страницы "регистрация приложения"](./images/aad-register-an-app.png)

1. <span data-ttu-id="82161-122">Нажмите **Зарегистрировать**.</span><span class="sxs-lookup"><span data-stu-id="82161-122">Select **Register**.</span></span> <span data-ttu-id="82161-123">На странице " **робот календарного календаря** " СКОПИРУЙТЕ значение **идентификатора Application (Client)** и сохраните его, он понадобится в следующих шагах.</span><span class="sxs-lookup"><span data-stu-id="82161-123">On the **Graph Calendar Bot** page, copy the value of the **Application (client) ID** and save it, you will need it in the following steps.</span></span>

    ![Снимок экрана с ИДЕНТИФИКАТОРом приложения для новой регистрации приложения](./images/aad-application-id.png)

1. <span data-ttu-id="82161-125">Выберите **Сертификаты и секреты** в разделе **Управление**.</span><span class="sxs-lookup"><span data-stu-id="82161-125">Select **Certificates & secrets** under **Manage**.</span></span> <span data-ttu-id="82161-126">Нажмите кнопку **Новый секрет клиента**.</span><span class="sxs-lookup"><span data-stu-id="82161-126">Select the **New client secret** button.</span></span> <span data-ttu-id="82161-127">Введите значение в поле **Описание** и выберите один из вариантов **истечения срока действия** , а затем нажмите кнопку **добавить**.</span><span class="sxs-lookup"><span data-stu-id="82161-127">Enter a value in **Description** and select one of the options for **Expires** and select **Add**.</span></span>

1. <span data-ttu-id="82161-128">Скопируйте значение секрета клиента, а затем покиньте эту страницу.</span><span class="sxs-lookup"><span data-stu-id="82161-128">Copy the client secret value before you leave this page.</span></span> <span data-ttu-id="82161-129">Это потребуется в следующих шагах.</span><span class="sxs-lookup"><span data-stu-id="82161-129">You will need it in the following steps.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="82161-130">Это секрет клиента, он никогда не отображается еще раз, поэтому убедитесь, что вы скопировали его.</span><span class="sxs-lookup"><span data-stu-id="82161-130">This client secret is never shown again, so make sure you copy it now.</span></span> <span data-ttu-id="82161-131">Необходимо ввести это значение в несколько мест, чтобы обеспечить безопасность.</span><span class="sxs-lookup"><span data-stu-id="82161-131">You will need to enter this value in multiple places so keep it safe.</span></span>

1. <span data-ttu-id="82161-132">Вернитесь в окно регистрации канала Bot в браузере и вставьте идентификатор приложения в поле **идентификатор приложения Microsoft** .</span><span class="sxs-lookup"><span data-stu-id="82161-132">Return to the Bot Channel Registration window in your browser, and paste the application ID into the **Microsoft App ID** field.</span></span> <span data-ttu-id="82161-133">Вставьте секрет клиента в поле **Password (пароль** ).</span><span class="sxs-lookup"><span data-stu-id="82161-133">Paste your client secret into the **Password** field.</span></span> <span data-ttu-id="82161-134">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="82161-134">Select **OK**.</span></span>

1. <span data-ttu-id="82161-135">На странице **Регистрация каналов Боты** нажмите кнопку **создать**.</span><span class="sxs-lookup"><span data-stu-id="82161-135">On the **Bots Channels Registration** page, select **Create**.</span></span>

1. <span data-ttu-id="82161-136">Дождитесь создания регистрации каналов ленты.</span><span class="sxs-lookup"><span data-stu-id="82161-136">Wait for the Bot Channels registration to be created.</span></span> <span data-ttu-id="82161-137">После создания вернитесь на домашнюю страницу на портале Azure, а затем выберите **службы Bot**.</span><span class="sxs-lookup"><span data-stu-id="82161-137">Once created, return to the Home page in the Azure Portal, then select **Bot Services**.</span></span> <span data-ttu-id="82161-138">Выберите новую регистрацию канала Боты, чтобы просмотреть ее свойства.</span><span class="sxs-lookup"><span data-stu-id="82161-138">Select your new Bots Channel registration to view its properties.</span></span>

## <a name="create-a-web-app-registration"></a><span data-ttu-id="82161-139">Создание регистрации веб-приложения</span><span class="sxs-lookup"><span data-stu-id="82161-139">Create a web app registration</span></span>

1. <span data-ttu-id="82161-140">Вернитесь к разделу **Регистрация приложений** на портале Azure.</span><span class="sxs-lookup"><span data-stu-id="82161-140">Return to the **App registrations** section of the Azure Portal.</span></span>

1. <span data-ttu-id="82161-141">Выберите **Новая регистрация**.</span><span class="sxs-lookup"><span data-stu-id="82161-141">Select **New registration**.</span></span> <span data-ttu-id="82161-142">На странице **Зарегистрировать приложение** задайте необходимые значения следующим образом.</span><span class="sxs-lookup"><span data-stu-id="82161-142">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="82161-143">Введите **имя** `Graph Calendar Bot Auth`.</span><span class="sxs-lookup"><span data-stu-id="82161-143">Set **Name** to `Graph Calendar Bot Auth`.</span></span>
    - <span data-ttu-id="82161-144">Введите **поддерживаемые типы учетных записей** для **учетных записей в любом каталоге организаций и личных учетных записей Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="82161-144">Set **Supported account types** to **Accounts in any organizational directory and personal Microsoft accounts**.</span></span>
    - <span data-ttu-id="82161-145">В разделе **URI адрес перенаправления** введите значение в первом раскрывающемся списке `Web` и задайте значение `https://token.botframework.com/.auth/web/redirect`.</span><span class="sxs-lookup"><span data-stu-id="82161-145">Under **Redirect URI**, set the first drop-down to `Web` and set the value to `https://token.botframework.com/.auth/web/redirect`.</span></span>

1. <span data-ttu-id="82161-146">Нажмите **Зарегистрировать**.</span><span class="sxs-lookup"><span data-stu-id="82161-146">Select **Register**.</span></span> <span data-ttu-id="82161-147">На странице "поэтапная **Проверка подлинности для календаря Graph** " СКОПИРУЙТЕ значение **идентификатора Application (Client)** и сохраните его, он понадобится в следующих шагах.</span><span class="sxs-lookup"><span data-stu-id="82161-147">On the **Graph Calendar Bot Auth** page, copy the value of the **Application (client) ID** and save it, you will need it in the following steps.</span></span>

1. <span data-ttu-id="82161-148">Выберите **Сертификаты и секреты** в разделе **Управление**.</span><span class="sxs-lookup"><span data-stu-id="82161-148">Select **Certificates & secrets** under **Manage**.</span></span> <span data-ttu-id="82161-149">Нажмите кнопку **Новый секрет клиента**.</span><span class="sxs-lookup"><span data-stu-id="82161-149">Select the **New client secret** button.</span></span> <span data-ttu-id="82161-150">Введите значение в поле **Описание** и выберите один из вариантов **истечения срока действия** , а затем нажмите кнопку **добавить**.</span><span class="sxs-lookup"><span data-stu-id="82161-150">Enter a value in **Description** and select one of the options for **Expires** and select **Add**.</span></span>

1. <span data-ttu-id="82161-151">Скопируйте значение секрета клиента, а затем покиньте эту страницу.</span><span class="sxs-lookup"><span data-stu-id="82161-151">Copy the client secret value before you leave this page.</span></span> <span data-ttu-id="82161-152">Это потребуется в следующих шагах.</span><span class="sxs-lookup"><span data-stu-id="82161-152">You will need it in the following steps.</span></span>

1. <span data-ttu-id="82161-153">Выберите **разрешения API**, а затем нажмите кнопку **Добавить разрешение**.</span><span class="sxs-lookup"><span data-stu-id="82161-153">Select **API permissions**, then select **Add a permission**.</span></span>

1. <span data-ttu-id="82161-154">Выберите **Microsoft Graph**, а затем выберите **делегированные разрешения**.</span><span class="sxs-lookup"><span data-stu-id="82161-154">Select **Microsoft Graph**, then select **Delegated permissions**.</span></span>

1. <span data-ttu-id="82161-155">Выберите указанные ниже разрешения и нажмите кнопку **Добавить разрешения**.</span><span class="sxs-lookup"><span data-stu-id="82161-155">Select the following permissions, then select **Add permissions**.</span></span>

    - <span data-ttu-id="82161-156">**openid**</span><span class="sxs-lookup"><span data-stu-id="82161-156">**openid**</span></span>
    - <span data-ttu-id="82161-157">**profile**</span><span class="sxs-lookup"><span data-stu-id="82161-157">**profile**</span></span>
    - <span data-ttu-id="82161-158">**Calendars.ReadWrite**</span><span class="sxs-lookup"><span data-stu-id="82161-158">**Calendars.ReadWrite**</span></span>
    - <span data-ttu-id="82161-159">**MailboxSettings.Read**</span><span class="sxs-lookup"><span data-stu-id="82161-159">**MailboxSettings.Read**</span></span>

    ![Снимок экрана с настроенными разрешениями](images/configured-permissions.png)

### <a name="about-permissions"></a><span data-ttu-id="82161-161">Сведения о разрешениях</span><span class="sxs-lookup"><span data-stu-id="82161-161">About permissions</span></span>

<span data-ttu-id="82161-162">Обратите внимание на то, что каждая из этих областей разрешений разрешает ему выполнить, а также о том, для чего они используются в качестве ленты.</span><span class="sxs-lookup"><span data-stu-id="82161-162">Consider what each of those permission scopes allows the bot to do, and what the bot will use them for.</span></span>

- <span data-ttu-id="82161-163">**OpenID** and **Profile**: позволяет botу подписывать пользователей и получать основные сведения из Azure AD в маркере удостоверения.</span><span class="sxs-lookup"><span data-stu-id="82161-163">**openid** and **profile**: allows the bot to sign users in and get basic information from Azure AD in the identity token.</span></span>
- <span data-ttu-id="82161-164">**Calendars. ReadWrite**: позволяет элементу Bot читать календарь пользователя и добавлять новые события в календарь пользователя.</span><span class="sxs-lookup"><span data-stu-id="82161-164">**Calendars.ReadWrite**: allows the bot to read the user's calendar and to add new events to the user's calendar.</span></span>
- <span data-ttu-id="82161-165">**MailboxSettings. Read**: позволяет роботу читать параметры почтового ящика пользователя.</span><span class="sxs-lookup"><span data-stu-id="82161-165">**MailboxSettings.Read**: allows the bot to read the user's mailbox settings.</span></span> <span data-ttu-id="82161-166">Bot будет использовать этот элемент для получения выбранного пользователем часового пояса.</span><span class="sxs-lookup"><span data-stu-id="82161-166">The bot will use this to get the user's selected time zone.</span></span>
- <span data-ttu-id="82161-167">**User. Read**: позволяет объекту Bot получить профиль пользователя из Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="82161-167">**User.Read**: allows the bot to get the user's profile from Microsoft Graph.</span></span> <span data-ttu-id="82161-168">Bot будет использовать для получения имени пользователя.</span><span class="sxs-lookup"><span data-stu-id="82161-168">The bot will use this to get the user's name.</span></span>

## <a name="add-oauth-connection-to-the-bot"></a><span data-ttu-id="82161-169">Добавление подключения OAuth к Bot</span><span class="sxs-lookup"><span data-stu-id="82161-169">Add OAuth connection to the bot</span></span>

1. <span data-ttu-id="82161-170">Перейдите на страницу регистрации каналов ленты Bot на портале Azure.</span><span class="sxs-lookup"><span data-stu-id="82161-170">Navigate to your bot's Bot Channels Registration page on the Azure Portal.</span></span> <span data-ttu-id="82161-171">Выберите **Параметры** в разделе **Управление Bot**.</span><span class="sxs-lookup"><span data-stu-id="82161-171">Select **Settings** under **Bot Management**.</span></span>

1. <span data-ttu-id="82161-172">В разделе **Параметры подключения OAuth** в нижней части страницы нажмите кнопку **Добавить параметр**.</span><span class="sxs-lookup"><span data-stu-id="82161-172">Under **OAuth Connection Settings** near the bottom of the page, select **Add Setting**.</span></span>

1. <span data-ttu-id="82161-173">Заполните форму следующим образом, а затем нажмите кнопку **сохранить**.</span><span class="sxs-lookup"><span data-stu-id="82161-173">Fill in the form as follows, then select **Save**.</span></span>

    - <span data-ttu-id="82161-174">**Имя**: `GraphBotAuth`</span><span class="sxs-lookup"><span data-stu-id="82161-174">**Name**: `GraphBotAuth`</span></span>
    - <span data-ttu-id="82161-175">**Поставщик**: **Azure Active Directory v2**</span><span class="sxs-lookup"><span data-stu-id="82161-175">**Provider**: **Azure Active Directory v2**</span></span>
    - <span data-ttu-id="82161-176">**Идентификатор клиента**: идентификатор приложения для регистрации **подлинности Bot** почтового календаря.</span><span class="sxs-lookup"><span data-stu-id="82161-176">**Client id**: The application ID of your **Graph Calendar Bot Auth** registration.</span></span>
    - <span data-ttu-id="82161-177">**Секрет клиента**: Секрет клиента для регистрации **подлинности Bot** почтового календаря.</span><span class="sxs-lookup"><span data-stu-id="82161-177">**Client secret**: The client secret of your **Graph Calendar Bot Auth** registration.</span></span>
    - <span data-ttu-id="82161-178">**URL-адрес для обмена маркерами**: оставьте пустым</span><span class="sxs-lookup"><span data-stu-id="82161-178">**Token Exchange URL**: Leave blank</span></span>
    - <span data-ttu-id="82161-179">**Идентификатор клиента**: `common`</span><span class="sxs-lookup"><span data-stu-id="82161-179">**Tenant ID**: `common`</span></span>
    - <span data-ttu-id="82161-180">**Области**: `openid profile Calendars.ReadWrite MailboxSettings.Read User.Read`</span><span class="sxs-lookup"><span data-stu-id="82161-180">**Scopes**: `openid profile Calendars.ReadWrite MailboxSettings.Read User.Read`</span></span>

1. <span data-ttu-id="82161-181">Выберите запись **графботаус** в разделе **Параметры подключения OAuth**.</span><span class="sxs-lookup"><span data-stu-id="82161-181">Select the **GraphBotAuth** entry under **OAuth Connection Settings**.</span></span>

1. <span data-ttu-id="82161-182">Выберите **проверить подключение**.</span><span class="sxs-lookup"><span data-stu-id="82161-182">Select **Test Connection**.</span></span> <span data-ttu-id="82161-183">Откроется новое окно браузера или вкладка для запуска процесса OAuth.</span><span class="sxs-lookup"><span data-stu-id="82161-183">This opens a new browser window or tab to start the OAuth flow.</span></span>

1. <span data-ttu-id="82161-184">При необходимости выполните вход.</span><span class="sxs-lookup"><span data-stu-id="82161-184">If necessary, sign in.</span></span> <span data-ttu-id="82161-185">Просмотрите список запрошенных разрешений, а затем нажмите кнопку **принять**.</span><span class="sxs-lookup"><span data-stu-id="82161-185">Review the list of requested permissions, then select **Accept**.</span></span>

1. <span data-ttu-id="82161-186">Должно появиться **тестовое подключение к сообщению "графботаус"** .</span><span class="sxs-lookup"><span data-stu-id="82161-186">You should see a **Test Connection to 'GraphBotAuth' Succeeded** message.</span></span>

> [!TIP]
> <span data-ttu-id="82161-187">Вы можете нажать кнопку **Копировать маркер** на этой странице и вставить маркер в, [https://jwt.ms](https://jwt.ms) чтобы увидеть утверждения внутри маркера.</span><span class="sxs-lookup"><span data-stu-id="82161-187">You can select the **Copy Token** button on this page, and paste the token into [https://jwt.ms](https://jwt.ms) to see the claims inside the token.</span></span> <span data-ttu-id="82161-188">Это полезно при устранении ошибок проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="82161-188">This is useful when troubleshooting authentication errors.</span></span>
