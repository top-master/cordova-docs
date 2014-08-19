# Последующие шаги

Для разработчиков, иметь представление о том, как использовать Cordova CLI и использовать плагины, есть несколько вещей, вы можете хотеть рассматривать исследования рядом с построения лучшего, более мощные приложений Cordova. Следующий документ предлагает советы по различным темам, касающимся передовой практики, испытаний, модернизации и другие темы, но не предназначен быть директивный характер. Рассмотрим это вашей отправной точкой для вашего роста, как разработчик Кордова. Кроме того если вы видите то, что можно улучшить, пожалуйста, [внести][1]!

 [1]: http://cordova.apache.org/#contribute

Данное руководство содержит следующие разделы:

*   Лучшие практики
*   Обработка обновлений
*   Тестирование
*   Отладка
*   Пользовательский интерфейс
*   Особые соображения
*   Ногу
*   Получение справки 

# Лучшие практики

## 1) Спа-ваш друг

Прежде всего - приложений Cordova следует принять дизайн спа (одной страницы приложения). Слабо определены, Спа это клиентское приложение, которое запускается из одного запроса веб-страницы. Пользователь загружает начальный набор ресурсов (HTML, CSS и JavaScript) и дальнейшего обновления (показывая новый взгляд, загрузки данных) делается через AJAX. Курорты широко используются для более сложных клиентских приложений. GMail является прекрасным примером этого. После загрузки GMail, mail просмотров, редактирования и Организации выполняются путем обновления DOM вместо фактически оставляя на текущей странице для загрузки полностью новый.

С помощью спа может помочь вам организовать ваше приложение более эффективным образом, но она также имеет конкретные преимущества для приложений Cordova. Cordova-приложение необходимо дождаться события deviceready прежде чем какие-либо плагины могут быть использованы. Если вы не использовать спа, и пользователь нажимает перейти от одной страницы к другой, вам придется ждать deviceready огонь снова прежде чем вы делаете использовать плагин. Это легко забыть, как ваше приложение получает больше.

Даже если вы не хотите использовать Cordova, создание мобильного приложения без использования архитектуры одной страницы будет иметь серьезные производительность последствия. Это потому, что Навигация между страницами потребует скрипты, активов и т.д., необходимо перезагрузить. Даже если эти активы кэшируются, по-прежнему будет проблем производительности.

Примеры спа библиотек, которые можно использовать в приложениях Cordova являются:

*   [AngularJS][2]
*   [EmberJS][3]
*   [Костяк][4]
*   [Кэндо пользовательского интерфейса][5]
*   [Монака][6]
*   [ReactJS][7]
*   [Sencha Touch][8]
*   [jQuery Mobile][9]

 [2]: http://angularjs.org
 [3]: http://emberjs.com
 [4]: http://backbonejs.org
 [5]: http://www.telerik.com/kendo-ui
 [6]: http://monaca.mobi/en/
 [7]: http://facebook.github.io/react/
 [8]: http://www.sencha.com/products/touch/
 [9]: http://jquerymobile.com

И много, много, больше.

## 2) производительность.

Один из самых больших ошибок, можно сделать новый разработчик Кордова является предположить, что производительность, они получают на столе машины является то же самое, они получат на мобильном устройстве. Хотя наших мобильных устройств получили более мощные каждый год, они по-прежнему не хватает мощности и производительности рабочего стола. Мобильные устройства, как правило, имеют гораздо меньше оперативной памяти и GPU, что является далеко от их стола (или даже ноутбук) братьев. Полный список советов здесь будет слишком много, но вот несколько вещей, чтобы иметь в виду (с перечнем больше ресурсов в конце для дальнейших исследований).

**Щелкните по сравнению с Touch** - крупнейший и самый простой ошибкой, вы можете сделать должна использовать события click. Хотя эти «работы» просто отлично на мобильный, большинство устройств вводят 300 мс задержки на них для того, чтобы различить прикосновение и коснуться событие «hold». С помощью `touchstart` , или `touchend` , приведет к резкому улучшению - 300 мс не звучит как много, но это может привести к отрывисто обновления пользовательского интерфейса и поведения. Следует также учитывать тот факт, что «touch» события не поддерживаются на не webkit-браузерами, см [CanIUse][10]. Для того, чтобы справиться с эти ограничения, вы можете оформить различные библиотеки, такие как HandJS и Fastouch.

 [10]: http://caniuse.com/#search=touch

**Переходы CSS против манипуляции DOM** - с использованием аппаратного ускорения переходы CSS будет значительно лучше, чем использование JavaScript для создания анимации. Смотрите список ресурсов в конце этого раздела для примеров.

**Сетей сосать** - ОК, сети не всегда сосать, но задержку мобильных сетей, даже хорошие мобильные сети, намного хуже, чем вы, вероятно, думаете. Приложение рабочего стола, который хлебает вниз 500 строк данных JSON, каждые 30 секунд, будет медленнее на мобильное устройство, а также батарея hog. Имейте в виду, что Кордова apps имеют несколько способов для сохранения данных в приложении (LocalStorage и файловой системе, например). Кэшировать данные локально и учитывать количество данных, отправляемых и обратно. Это особенно важно, когда ваше приложение подключается через сотовую сеть.

**Дополнительный выигрыш в производительности статьи и ресурсы**

*   [«Вы наполовину assed он»][11]
*   [«Топ 10 Советы по повышению производительности для PhoneGap и гибридных приложений»][12]
*   «Быстрые приложения и сайты с JavaScript»: http://channel9.msdn.com/Events/Build/2013/4-313

 [11]: http://sintaxi.com/you-half-assed-it
 [12]: http://coenraets.org/blog/2013/10/top-10-performance-techniques-for-phonegap-and-hybrid-apps-slides-available/

## 3) признавать и обрабатывать автономный статус

Смотрите предыдущий совет о сетях. Не только вы можете быть на медленных сетях, это вполне возможно для вашего приложения в полностью автономном режиме. Приложение должно обрабатывать это интеллигентая(ый) образом. Если ваше приложение не произойдет, люди будут думать, что ваше приложение не работает. Учитывая, насколько легко это для обработки (Кордова поддерживает прослушивание для оффлайн и онлайн событие) нет абсолютно никаких оснований для вашего приложения не реагировать, хорошо, когда запускать в автономном режиме. Будьте уверены, чтобы проверить (см. ниже раздел тестирование) вашего приложения и не забудьте проверить, как ваше приложение обрабатывает при запуске в одном государстве и затем переключиться на другой.

Обратите внимание, что онлайн и оффлайн события, а также API подключения сети не является совершенным. Вам придется полагаться на с помощью XHR-запрос, чтобы увидеть, если оно действительно оффлайн или онлайн. В конце дня быть уверен, добавить той или иной форме поддержку для сетевых проблем - в самом деле, в Apple store (и возможно другие магазины) будут отвергать apps которые не обрабатывает должным образом оффлайн/онлайн государств. Для более подробного обсуждения этой темы см [«Является эта вещь по?»][13]

 [13]: http://blogs.telerik.com/appbuilder/posts/13-04-23/is-this-thing-on-%28part-1%29

# Обработка обновлений

## Обновление проектов Кордова

Если ваш существующий проект был создан с помощью Cordova 3.x, можно обновить проект, выполнив следующие действия:

    cordova platform update platform-name ios, android, etc.
    

Если ваш существующий проект был создан в версии до Кордова 3.x, вероятно было бы лучше, чтобы создать новый проект 3.x Кордова, а затем скопируйте код и ресурсы вашего существующего проекта в новый проект. Типичные действия:

*   Создайте новый проект 3.x Кордова (Кордова создание...)
*   Скопируйте папку www из старого проекта в новый проект
*   Скопируйте любые параметры конфигурации из старого проекта в новый проект
*   Добавьте любые плагины, используемые в старого проекта в новый проект
*   Создайте свой проект
*   Тест, тест, тест!

Независимо от предыдущей версии проекта абсолютно важно, что вы читаете на то, что было изменено в обновленной версии, как обновление может нарушить ваш код. Лучшее место, чтобы найти эту информацию будет в выпуске опубликованы на блоге Кордова и в хранилищах. Необходимо тщательно протестировать приложение, чтобы проверить правильность работы после выполнения обновления.

Примечание: некоторые плагины не могут быть совместимы с новой версией Кордова. Если плагин не совместим, вы можете быть в состоянии найти замену плагин, который делает то, что вам нужно, или может потребоваться отложить модернизацию вашего проекта. Кроме того изменить плагин, чтобы он работать под новой версии и вносить обратно в общины.

## Обновления плагинов

По состоянию на Cordova 3.4 не существует механизма для обновления измененных плагины, с помощью одной команды. Вместо этого удалить плагин и добавить его обратно в ваш проект, и будет установлена новая версия:

    cordova plugin rm com.some.plugin
    cordova plugin add com.some.plugin
    

Не забудьте проверить обновленный плагин документации, как вам может потребоваться настроить код для работы с новой версией. Кроме того проверьте, что новая версия плагина работает с версией вашего проекта Кордова.

Всегда проверяйте свои приложения, чтобы обеспечить, что установка нового плагина не нарушил что-то, что вы не планировали.

Если ваш проект имеет много плагинов, которые необходимо обновить, он может сэкономить время для создания оболочки или пакетный сценарий, который удаляет и добавляет плагины с помощью одной команды.

# Тестирование

Супер важно, тестирование приложений. Кордова команда использует Жасмин, но любой дружественный отряд веб-тестирования решение будет делать.

## Тестирование на тренажере против на реальном устройстве

Это не редкость для использования настольных браузеров и тренажеры/эмуляторы устройств при разработке Cordova-приложение. Однако это невероятно важно, что вы протестировали свое приложение на столько физических устройств, как вы, возможно, может:

*   Тренажеры являются именно: тренажеры. Например ваше приложение может работать в симуляторе iOS без проблем, но он может не на реальном устройстве (особенно в определенных обстоятельствах, например состояние нехватки памяти). Или, ваше приложение может на самом деле не на тренажере, хотя она прекрасно работает на реальном устройстве. 
*   Эмуляторы являются именно: Эмуляторы. Они не представляют, насколько хорошо ваше приложение будет работать на физическом устройстве. Например некоторые эмуляторы может сделать ваше приложение с искаженными дисплеем, в то время как реальное устройство не имеет никаких проблем. (Если вы столкнулись с этой проблемой, отключите узел GPU в эмуляторе).
*   Тренажеры обычно быстрее, чем физическое устройство. Эмуляторы, с другой стороны, как правило, медленнее. Не судите производительность вашего приложения, как он выполняет в тренажере или эмуляторе. Судья производительность вашего приложения, как он работает на спектр реальных устройств.
*   Невозможно получить хорошую почувствовать, как приложение реагирует на ваши прикосновения, используя эмулятор или симулятор. Вместо этого запуск приложения на реальном устройстве можно отметить проблемы с размерами элементов пользовательского интерфейса, отзывчивость и др.
*   Хотя было бы неплохо иметь возможность проверить только на одно устройство на платформе, то лучше проверить на многих устройствах, спортивные много различных версий ОС. К примеру что работает на вашей конкретной Android смартфон может не на другом устройстве Android. То, что работает на устройстве iOS 7 может не на устройстве iOS 6.

Это, конечно же, невозможно проверить на все возможные устройства на рынке. По этой причине то целесообразно набирать много тестеров, которые имеют различные устройства. Хотя они не будут ловить каждую проблему, имеются хорошие шансы, что они откроют причуды и вопросы, которые вы бы никогда не найти в одиночку.

Подсказка: Это возможно на устройствах Android Nexus легко прошить различных версий Android на устройстве. Этот простой процесс позволит вам легко тестировать приложения на различных уровнях с помощью одного устройства Андроид без аннулирования гарантии или требуя «побег из тюрьмы» или «корень» ваше устройство. Изображения фабрика Google Android и инструкции находятся в: https://developers.google.com/android/nexus/images#instructions

# Отладка

Отладка Cordova требуется небольшая Настройка. В отличие от настольных приложений вы не можете просто открыть dev инструменты на вашем мобильном устройстве и начать отладку, к счастью, есть некоторые большие альтернативы.

## Safari удаленной отладки

Первый вариант — Safari удаленной отладки. Это работает только на OSX и только с iOS 6 (и выше). Он использует Safari для подключения к устройству (или симулятор) и будет подключаться к Cordova-приложение браузера dev инструменты. Вы получаете то, что вы ожидаете от dev tools - инспекции/манипуляции DOM, отладчик JavaScript, сеть инспекции, консоли и многое другое. Для более подробной информации, см. эту прекрасную блоге: <http://moduscreate.com/enable-remote-web-inspector-in-ios-6/>

## Хром удаленной отладки

Практически же, как в версии Safari, это работает только с Android, но может быть использован с любой настольной операционной системы. Это требует как минимум Андроид 4.4 (KitKat), минимальный уровень API 19 и хром 30 + (на столе). После подключения, вы получите тот же опыт хром Dev Tools для мобильных приложений, как приложения для настольных. Даже лучше Chrome Dev инструменты имеют зеркальный вариант, который показывает ваше приложение работает на мобильном устройстве. Это больше, чем просто представление - вы можете прокручивать и нажмите от dev tools и обновляет на мобильном устройстве. Подробнее об удаленной отладки Chrome можно найти здесь: <https://developers.google.com/chrome/mobile/docs/debugging>

Это позволяет использовать Chrome Dev инструменты для проверки приложений iOS, через прокси WebKit: <https://github.com/google/ios-webkit-debug-proxy/>

## Пульсации

Пульсация — обои на основе эмулятор для Кордова проектов. По сути он позволяет запускать Cordova-приложение в вашем настольном приложении и поддельные различные функции Кордова. Например он позволяет имитировать акселерометр для тестирования трясти события. Это подделки камеры API, позволяя выбрать картинку с вашего жесткого диска. Пульсации позволяет вам сосредоточиться на пользовательский код, а не беспокоиться о Кордове плагины. Вы можете узнать больше о пульсации здесь: <http://ripple.incubator.apache.org/>

## Weinre

Weinre создает локальный сервер, который может содержать клиент удаленной отладки для приложений Cordova. После того как установил и начал его, вы скопировать строку кода в Cordova-приложение и перезапустите его. Затем можно открыть панель инструментов dev на рабочем столе для работы с приложением. Weinre не как фантазии, как Chrome и Safari удаленной отладки, но имеет преимущества работы с гораздо большей спектр операционных систем и платформ. Дополнительную информацию можно найти здесь: <http://people.apache.org/~pmuellr/weinre/docs/latest/>

## Другие варианты

*   BlackBerry 10 поддерживает отладку также: [Документация][14]
*   Вы можете отлаживать, а также с помощью диспетчера приложений Firefox, [этот блог][15] и эту [статью MDN][16].
*   Дополнительные примеры и объяснения выше отладки советы, см: <http://developer.telerik.com/featured/a-concise-guide-to-remote-debugging-on-ios-android-and-windows-phone/>

 [14]: https://developer.blackberry.com/html5/documentation/v2_0/debugging_using_web_inspector.html
 [15]: https://hacks.mozilla.org/2014/02/building-cordova-apps-for-firefox-os/
 [16]: https://developer.mozilla.org/en-US/Apps/Tools_and_frameworks/Cordova_support_for_Firefox_OS#Testing_and_debugging

# Пользовательский интерфейс

Здание Cordova-приложение, которое выглядит хорошо на мобильный может быть сложной задачей, особенно для разработчиков. Многие люди решили использовать UI framework, чтобы сделать это проще. Вот краткий список опций, которые вы можете хотеть рассматривать.

*   [jQuery Mobile][9] - jQuery Mobile автоматически повышает ваш макет для мобильных оптимизации. Он также обрабатывает создание спа для вас автоматически.
*   [Ionic][17] -это мощная инфраструктура пользовательского интерфейса на самом деле имеет свой собственный CLI для обработки создания проекта. 
*   [Храповик][18] - привлечены к вам люди, которые создали Bootstrap. 
*   [Кэндо UI][5] - открытым исходным кодом пользовательского интерфейса и платформа приложений от Telerik.
*   [Автоэмаль][19]
*   [ReactJS][7]

 [17]: http://ionicframework.com/
 [18]: http://goratchet.com/
 [19]: http://topcoat.io

При создании интерфейса пользователя, важно думать о всех платформ, которые вы ориентируетесь и различия между ожиданиям пользователя. Например, Android приложение, которое имеет интерфейс iOS стиль будет вероятно не хорошо сочетаются с пользователей. Это иногда даже применяется в различных магазинах приложений. Из-за этого, важно, что вы уважаете конвенций каждой платформы и поэтому знакомы с различными руководящими принципами человеческого интерфейса: * [iOS][20] * [Android][21] * [Windows Phone][22]

 [20]: https://developer.apple.com/library/ios/documentation/userexperience/conceptual/MobileHIG/index.html
 [21]: https://developer.android.com/designWP8
 [22]: http://dev.windowsphone.com/en-us/design/library

## Дополнительные пользовательского интерфейса статьи и ресурсы

Хотя браузеров становится все больше и больше стандартов жалобы, мы все еще живем в мире, префиксом (-webkit и - г-жа) следующие статьи ценно при разработке UI в для кросс-браузер приложений: <http://blogs.windows.com/windows_phone/b/wpdev/archive/2012/11/15/adapting-your-webkit-optimized-site-for-internet-explorer-10.aspx>

# Особые соображения

Хотя Cordova упрощает кросс платформенной разработки, это просто не возможно обеспечить 100% изоляция от родной базовой платформы. Так что надо знать ограничений.

## Особенности платформы

При чтении документации, искать разделы, которые структура различных поведений или требования на различных платформах. Если они присутствуют, они будут в разделе под названием «Android причуды», «причуды iOS», и т.д. Прочитайте эти причуды и быть осведомлены о них, как вы работаете с Кордова.

## Загрузка удаленного содержимого

Вызов функции Cordova JavaScript с удаленной загрузки HTML-страницы (HTML-страницы, не хранятся локально на устройстве) является неподдерживаемая конфигурация. Это потому что Кордова была не предназначены для этого, и сообщество Apache Cordova делает не тестирования данной конфигурации. Хотя она может работать в некоторых случаях, она не рекомендуется и не поддерживается. Есть проблемы с политики единого происхождения, сохраняя JavaScript и родной части Cordova синхронизированы на той же версии (так как они соединены через частной API, которые могут изменяться), в надежности удаленного контента, вызов собственных функций местных и потенциальный отказ магазина app.

Отображение удаленного загрузки HTML-содержимое в webview должно быть сделано с помощью Кордова в InAppBrowser. InAppBrowser разработан таким образом, что JavaScript, работает там не имеют доступа к API JavaScript Кордова по причинам, перечисленным выше. Пожалуйста, обратитесь к руководству по безопасности.

# Ногу

Вот несколько способов, чтобы держать в курсе Кордова.

*   Подписаться на [блог Кордова][23].
*   Подписаться на [список разработчиков][24]. Обратите внимание - это не группа поддержки! Скорее это место, где обсуждается развитие Кордова.

 [23]: http://cordova.apache.org/#news
 [24]: http://cordova.apache.org/#mailing-list

# Получение справки

Следующие ссылки являются лучшими местами, чтобы получить помощь для Кордова:

*   StackOverflow: <http://stackoverflow.com/questions/tagged/cordova> с помощью тега Cordova, можно просматривать и просматривать все вопросы Кордова. Обратите внимание, что StackOverflow автоматически преобразует тег «Phonegap», чтобы «Кордова», так что таким образом вы будете иметь доступ к исторических вопросов, а также
*   PhoneGap группы Google: [https://groups.google.com/forum/#! форум/phonegap][25] этой группе Google был старый форум поддержки для когда Кордова по-прежнему называется PhoneGap. Хотя есть еще много Cordova пользователей, которые часто эта группа, Кордова сообщество выразило заинтересованность в упор меньше на этой группы и вместо этого с помощью StackOverflow для поддержки
*   Встреча: <http://phonegap.meetup.com> - рассмотреть возможность найти местных Кордова/PhoneGap встреча группы

 [25]: https://groups.google.com/forum/#!forum/phonegap