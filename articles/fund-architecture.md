# Fundamentals of Software Architecture | Mark Richards, Neal Ford
[Telegram: Contact @kydavoiti](https://t.me/kydavoiti/60)

[Купить книгу](https://www.oreilly.com/library/view/fundamentals-of-software/9781492043447/). Прочитал про эту книгу у [Алименкова на канале](https://t.me/xpinjection_channel/450).
[Telegram: Contact @kydavoiti](https://t.me/kydavoiti/60)

Далее будут мои ~заметки~ по книге. Я описывал, что казалось интересным. Последние главы мне показались бизнес-тренерскими приколами, поэтому там я перестал подробно конспектировать. Кому-то другому вторая часть книги может показаться более полезной. Для меня чем дальше в лес — тем меньше света. Я читал на английском. Заметки в большинстве на русском. Могут встречаться трудности перевода. Если я буду неоч уверен, то будут простые цитаты.

- - - -

В предисловии подчеркивается *процесс развития* отрасли. Самой важной цитатой я считаю следующее:
> “Developers have long wished to change software development ~from a craft~, where skilled artisans can create one-off works, ~to an engineering discipline~, which implies repeatability, rigor, and effective analysis”  

Но там если посмотреть далее. Они от этого отходят и начинают опять заводить речи о неком *art*. Как мне кажется art ничего общего с *engineering discipline* не имеет.

Введение дает интересные мыли: 
> “When studying architecture, readers must keep in mind that, ~like much art, it can only be understood in context~.  Many of the decisions architects made were based on realities of the environment they found themselves in.”  
Вот уже во введении они скатились в *art*, а не *engineering discipline*. Далее такие фразы подсвечивать не буду. **Архитектура** ~отличается~ от **искусства** тем, что искусство (часто) строиться (воспринимается) не на реалиях, а субъективно, а архитектура должна быть  объективна. Остальное тут дельно. Ничего не может существовать само-для-себя. ~У всего есть причины и последствия; а также плюсы и минусы~. 

- - - -

От архитектора ожидают:
- **Принятия архитектурных решений**. **Архитектурное решение** — это решение, которое влияет на структуру системы и её характеристики. ~Команда принимает решение, если оно не влияет на архитектуру~. Например выбор **БД** может ограничивать систему в масштабировании, поэтому его должен делать архитектор. 
- **Постоянно анализировать настоящую архитектуру приложения**
- **Поддерживать актуальность решения внутри и за пределами компании**
- **Проверять, что решение соответствует требованиям**
- **Расширять круг зрения и опыт**. Авторы говорят, что надо стараться разбираться во многих областях, но я считаю, что это нереально. Я бы предложил скорее уметь ориентироваться во многих областях, а также слушать и понимать то, что говорят эксперты. Но я не архитектор 😅
- **Понимать устройство предметной области**
- **Soft-Skills**
- **Быть политиком**. Надо быть политиком, чтобы понимать цели людей и куда движется бизнес, чтобы лучше ему соответствовать. Например, предвидеть будущие интеграции. Я бы отнес это к **Soft-Skills**, а не ставил отдельно.

Вообще после всех этих пунктов их книги кажется, что архитектор должен быть ~супер человеком~, который просто умеет все.

- - - -

> “However, projects also fall victim to unknown unknowns: things no one knew were going to crop up yet have appeared unexpectedly. This is why all “Big Design Up Front” software efforts suffer: architects cannot design for **unknown unknowns**. “  
Интересное понятие **unknown unknowns**, которое нельзя учет никак, кроме как делать систему достаточно гибкой и двигаться итеративно.

Для архитектора важно составить **метрики системы**, которые должны собираться для каждого релиза компонента. Это надо, чтобы прослеживать динамику системы и не запустить критические моменты.

Авторы дают **законы архитектуры**: 
- *“Everything in software architecture is a trade-off”*. Полностью согласен, это верно не только архитектуры. Насколько это вернО, настолько же это и важно.
- *“Why is more important than how”*. Есть много способов сделать решения. Бесконечно много. Но есть конечное кол-во причин по которым принимаются решения — поэтому важно их знать.

- - - -

~Архитектор не должен быть разделен в своей работе от команд разработки~. Они должны работать вместе, чтобы дополнять друг друга и обсуждать решения. 

Надо подходить к решению задачи без предрассудков, холодно оценивая её. Без фанатизма, основанного на старом опыте.
> **FROZEN CAVEMAN ANTI-PATTERN**  
> “describes an architect who always reverts back to their pet irrational concern for every architecture. “  

**Главная задача архитектора** — это находить ~не только плюсы~ решений, ~но и минусы~. Я бы еще добавил *why*, как авторы и говорили в своем законе 🤔

- - - -

В данной книге для измерения качественного состояния разбиения системы на модули используются характеристики: **cohesion**, **coupling**, **connascence**.

## Cohesion
> “**Cohesion** refers to what extent the parts of a module should be contained within the same module. In other words, it is a measure of how related the parts are to one another.”  

Я перевожу это как ~согласованность внутри себя~. Модуль должен быть самодостаточен. Далее авторы приводят метрики по измерению согласованности в порядке уменьшения показательности: 
- **Функциональная согласованность**.
Каждая часть зависит только от другой части и имеет все нужное для решения своей задачи в себе или ней.
- **Последовательная согласованность**
Когда результаты работы одной части являются входными данными для торой части.
- **Коммуникационная согласованность**
Когда одна часть строит свое поведение на результатах работы другой части. Например, запись лога в **БД** или отправка **email**.
- **Процедурная согласованность**
Части должны запускаться в определенном порядке.
- **ВременнАя согласованность**
Части функциональности, которые должны запускать в одном интервале относительно приложения. Например, каждый день работы или на старте.
- **Логическая согласованность**
> “The data within modules is related logically but not functionally. For example, consider a module that converts information from text, serialized objects, or streams. Operations are related, but the functions are quite different. A common example of this type of cohesion exists in virtually every Java project in the form of the StringUtils package: a group of static methods that operate on String but are otherwise unrelated.”  
Сложно было перевести — оставил как есть. Далее спорный пример к пункту. *related functionally* is okay, когда у тебя модуль — есть просто набор без отрывных от строк функций. Понятно, что проверка строки на то **email** ли она — это не вписывается в `StringUtils`, но функция `substringAfter` очень даже полезна в некоторых задачах со строками. Поэтому тут скорее надо в таких `*Utils` классах делать правило, что функция в нем должна иметь смысл для строк вне задачи, быть возможной для выполнения на ~произвольной строке в произвольном проекте~.
- **Случайная согласованность** 
Так исторически сложилось — нетленочка.
 
## Coupling
Просто **связность** ничего нового и интересного нет, я бы даже сказал, что там это описано плохо.

Вводятся характеристики: 
- **Абстрактность**
> “Abstractness is the ratio of abstract artifacts (abstract classes, interfaces, and so on) to concrete artifacts (implementation). It represents a measure of abstractness versus implementation.”  

- **Нестабильность**
> “The instability metric determines the volatility of a code base. A code base that exhibits high degrees of instability breaks more easily when changed because of high coupling.”  

## Connascence
Новое для меня понятие было, довольно интересное. Я бы назвал это связностью, но можно и отделить, думается мне.
> “Two components are **connascent** if a change in one would require the other to be modified in order to maintain the overall correctness of the system.”  
> Meilir Page-Jones  
На русский я бы перевел это как **рассредоточенность**. 

-  **Static connascence**. *“Static connascence refers to source-code-level coupling it is a refinement of the afferent and efferent couplings defined by Structured Design.”*
	- **Connascence of Name** (CoN). *“Multiple components must agree on the name of an entity. Names of methods represents the most common way that code bases are coupled and the most desirable, especially in light of modern refactoring tools that make system-wide name changes trivial.”*
	- **Connascence of Type** (CoT). *“Multiple components must agree on the type of an entity. This type of connascence refers to the common facility in many statically typed languages to limit variables and parameters to specific types. However, this capability isn’t purely a language feature—some dynamically typed languages offer selective typing, notably Clojure and Clojure Spec.”*
	- **Connascence of Meaning** (CoM) or **Connascence of Convention** (CoC). *“Multiple components must agree on the meaning of particular values. The most common obvious case for this type of connascence in code bases is hard-coded numbers rather than constants. For example, it is common in some languages to consider defining somewhere int TRUE = 1; int FALSE = 0. Imagine the problems if someone flips those values.”*
	- **Connascence of Position** (CoP). *“Multiple entities must agree on the order of values. This is an issue with parameter values for method and function calls even in languages that feature static typing. For example, if a developer creates a method void updateSeat(String name, String seatLocation) and calls it with the values updateSeat("14D", "Ford, N"), the semantics aren’t correct even if the types are.”*
	- **Connascence of Algorithm** (CoA). *“Multiple components must agree on a particular algorithm. A common case for this type of connascence occurs when a developer defines a security hashing algorithm that must run on both the server and client and produce identical results to authenticate the user. Obviously, this represents a high form of coupling—if either algorithm changes any details, the handshake will no longer work.”*
- **Dynamic connascence**. *“The other type of connascence Page-Jones defined was dynamic connascence, which analyses calls at runtime. ”*
	- **Connascence of Execution** (CoE). *“The order of execution of multiple components is important. It won’t work correctly because certain properties must be set in order.”*
```java
Consider this code:
email = new Email();
email.setRecipient("foo@example.com");
email.setSender("me@me.com");
email.send();
email.setSubject("whoops");
```
	- **Connascence of Timing** (CoT)

> “The timing of the execution of multiple components is important. The common case for this type of connascence is a race condition caused by two threads executing at the same time, affecting the outcome of the joint operation”  
	- **Connascence of Values** (CoV). *“Occurs when several values relate on one another and must change together. Consider the case where a developer has defined a rectangle as four points, representing the corners. To maintain the integrity of the data structure, the developer cannot randomly change one of points without considering the impact on the other points. The more common and problematic case involves transactions, especially in distributed systems. When an architect designs a system with separate databases, yet needs to update a single value across all of the databases, all the values must change together or not at all.”*
	- **Connascence of Identity** (CoL).  *“Occurs when several values relate on one another and must change together. The common example of this type of connascence involves two independent components that must share and update a common data structure, such as a distributed queue.”*

Советы как улучшить модульность системы: 
- Уменьшать общую системную **рассредоточенность** по средствам **инкапсулирования**. 
- Уменьшать межмодульную **рассредоточенность**.
- Увеличивать **рассредоточенность** внутри модуля. Я бы его перефразировал немного. Не бояться **рассредоточенности** в модуле. 

- - - -

Авторы используют термин **характеристик архитектуры** (*architecture characteristic*) в схожем с не **функциональные требования** (*non functional requirements*).

Чтобы что-то являлось **архитектурной характеристикой** оно должно удовлетворять критериям: 
- ~Определять~ не предметное решение. Не функциональные требования. 
- ~Влияет~ на структуру решения. Разные характеристики требуют разных решений. 
- Это ~важно или необходимо для успеха приложения~, Брать в учет только важные характеристики, умея отсекать ненужные.

Перечисление основные характеристик: 
- **Availability**. 
- **Continuity**. Возможно ли ее поднять в случае аварии?
- **Performance**. 
- **Recoverability**. Как просто её поднимать после аварии (я бы не отделял это от **Continuity**)
- **Reliability/safety**.
- **Robustness**. Насколько точно можно гарантировать, что все ошибки будут замечены и их можно будет обработать
- **Scalability**.

Структурные характеристики: 
- **Configurability**. 
- **Extensibility**. 
- **Installability**. 
- **Leverageability/reuse**. 
- **Localization**. 
- **Maintainability**. 
- **Portability**. 
- **Supportability**. 
- **Upgradeability**.

Ортогональные характеристики: 
- **Accessibility**. 
- **Archivability**. 
- **Authentication**. 
- **Authorization**. 
- **Legal**. 
- **Privacy**. 
- **Security**. 
- **Supportability**. 
- **Usability/achievability**

Мне кажется они могли не перечислять все это. А могли просто сослаться на [ISO 25010](https://iso25000.com/index.php/en/iso-25000-standards/iso-25010). Что они и сделали в книге после перечисления.

- - - -

Подсвечиваются проблемы характеристик: 
- **They aren’t physics**. Они не точны. Их сложно выразить количественно. 
- **Wildly varying definitions**. Сильно разнящиеся термины.
- **Too composite**. Зависимость от исполнителей. 

Как хороший способ предлагается заводить **целевые функции**. Надо настроить **метрики** и функции, которые будут определять ~как близко **метрики** до ожидаемого состояния кода/системы~. Они приводят такие **метрики** как кол-во циклических зависимостей, их надо минимизировать.

**Важно!** Чтобы разработчики ~понимали~ и разделяли ценности метрик и целевых показателей, чтобы это были не просто значения из головы архитектора, а ценности команды.

- - - -

Вводится еще новое понятие: 
Architecture quantum
> “An **independently deployable** artifact with **high functional cohesion** and **synchronous connascence**”  

- - - -

Они рассказывают как по их мнению должен мыслить архитектор. Он должен ~уметь разделять систему на **компоненты**~. Делать он это должен чуть ли не в первую очередь. Еще архитектор должен верно выбирать ~способ разделить архитектуру~ (по техническим компонентам или предметным).

- - - -

Важные отличия распределенной архитектуры, от монолитного приложения: 
- Сеть не надежна
- Есть задержка
- Конечная пропускная способность
- Сеть не безопасна
- Структура сети меняется
- У сетя не один администратор
- Нет расходы на транспортный слой приложения
- Сеть неоднородна

В дополнение к этому они выделяют еще трудности: 
- Распределенное логгирование
- Распределенные транзакции
- Поддержание контракта и версионрование

Я бы тут еще добавил телеметрия распределенная, кроме логов.

- - - -

## Слоеная архитектура
Прекрасная цитата: 
> “If a developer or architect is unsure which architecture style they are using, or if an Agile development team “just starts coding,” chances are good that it is the **layered architecture** style they are implementing.”  

Также тут упоминается понятие **открытого** и **закрытого слоев**. Мне оно кажется не говорящим, я бы скорее называл их сплошные и прозрачные. Потому что они и показывают может ли слой быть пропущен.  

## Пайплайновая архитектура
То, что мы видим в **MapReduce** или и, если думать более приземленно, то это **Stream API**. Авторы приводят пример **bash**.

## Микроядерная архитектура
Редко в **WEB-разработке** сейчас пишут такие монолитные приложения, особенно разработчики не фронта. Актуальный пример сейчас — это добавление своего кода в **Hazelcast**. Или то, как работают **Spring Starter’ы**, они по своей природе ~являются расширениями для **ядра**~. Ядро является местом, что предоставляет порты, а к ним пишутся адаптеры, если говорить в терминах гексагональной архитектуры. Это немного другой уровень дискуссии, но выглядит схоже. 

Они там предлагают думать о **плагинах** не только в терминах модулей приложения, но и как о сервисах отдельных, которые работают через **MQ** или **RPC**. Они тут начинают забывать, что они сначала говорили, что у них монолитная архитектура (они не подразумевают свою истому как распределенный монолит). Но как пример архитектуры — хороший. Я думаю, что в каждой системе есть что-то, что можно так организовать и будет только лучше.

## SOA 
Для авторов — это ~слоеная архитектура применённая к распределенной системе~. Как мне кажется это можно упростить сказав, что там просто одна **БД** для всех сервисов. Из этого вытекает все остальное. И они подчеркивают, что основное отличие — общая **БД** и способы работы с ней (то есть модель данных) для одного контекста (набора сервисов).

Прекрасная цитата: 
> “The practice of creating a single shared library of entity objects is the least effective way of implementing service-based architecture. ”  
Мне почему-то не верят, когда я это говорю.

## Событийно ориентированная архитектура
Они позиционируют её как ~самую-самую в терминах масштабирования и производительности~. Они разделяют две топологии систем с такой архитектурой: 
- **broker topology**. 
	- **initiating event**. Это начальное событие, которые служит точкой входа в процесс.
	- **event broker**. Служит местом, через которое пропускается событие дальше для обработки.
	- **event processor**. Обработчик событий производит реакцию на событие ,которое он прочитал.
	- **processing event**. Каждый обработчик должен как результат своей обработки создать событие конца обработки.
Обычно для реализации этой модели используется **PubSub** модель. Предпочтительно из любого обработчика всегда отдавать события результата, чтобы позволить дальнейшее расширение системы.
- **mediator topology**. 
	- **initiating event**. Отличается от прошлой схемы только тем, что это сообщение в данной архитектуре всегда читает дирижёр.
	- **event queue**. Очередь с сообщениями, аналог брокера из прошлой архитектуры. Служит только для входных сообщений.
	- **event mediator**. Знает только о процессе, не содержит бизнес данных.
	- **event channels**. Служит для обмена информацией между дирижёром и обработчиком.
	- **event processors**. Не общается с другими обработчиками, а только с дирижером через каналы.

Я бы сказал, что они просто описали два способа к оформлению **саг**. Это **хореография** и **дирижёр**. 

Они рассказали подход с **dead letter queue** для обработки ошибок. Они поднимают вопросы упорядоченности записей в таком способе обработки ошибок. Рассматривают проблему ~потери данных~. Решается она сохранением на диск, ассоциацией потребителя с сообщением им прочитанным на уровне брокера. Примерно просто можно почитать в документации **Kafka**. Рассказывают про **broadcast** сообщения, что это хороший способ отвязаться сервисами архитектурно.

Также упоминают **Request-Reply** подход, что он имеет место быть. Они показывают как можно его реализовать. Два способа: **correlation id**, **temporary queue**.

## Пространственно ориентированная архитектура
Архитектура, которая предполагает отсутсвие базы данных, в каждом юните процесса есть копия данных с которым работает приложение, поэтому в такой конфигурации проще расширяться ,чем в какой-либо другой. Обычно горлышком является **БД**, тут оно исключено.
Она состоит из: 
- **Processing Unit**. Содержит в себе бизнес логику или её часть. Также он содержит в себе один экземпляр распределенной сети данных, например Hazelcast, Apache Ignite, Apache Coherence.
- **Virtualized Middleware**. Средства ,которые предназначены для функционирования системы. Инфраструктурные компоненты приложения. Эти части обычно представлены другими.
	- **Messaging Grid**. Аналог **API Gateway**. Обычно **HA Proxy** или **nginx**.
	- **Data Grid**. Ключевая часть. Она отвечает за хранение и **репликацию** данных.
	- **Processing Grid**. Оркестратор выполнения многочасовых процессов.
	- **Deployment Manager**. Управляет запуском и приостановкой сервисов, может работать на основе загрузки кластера и других метриках. 
	- **Data Pumps**. Это очереди, которые переносят данные об обновлениях из юнитов в **БД** за сетью сервисов.
	- **Data Writers**. Они являются потребителя данных из **Data Pumps**.
	- **Data Readers**.  Они читают данные из хранилища в которое писали **Data Writers**, чтобы восстановить данные при поднятии нового кластера.

Тут надо следить на тем, что могу быть коллизии, это самый важный момент. Также данные могут потеряться, надо следить за консенсусом.

## SOA с оркестратором
Данная архитектура сконцентрирована на переиспользовании компонентов всеми частями архитектуры. Ранее она была популярна из-за дорогих лицензий, железа и разработки. 

Она состоит из: 
- **Business Services**. Это предметные сервисы, которые содержат верхнеуровневые бизнес правила. Могут в отдельных случаях быть созданы даже не программистами.
- **Enterprise Services**. Слой переиспользуемых сервисов. Они могут использоваться предметными сервисами решающими разные задачи.
- **Application Service**. Сервис, который надо для предметного сервиса, но его не хочется делать полноценным Enterprise сервисом, потому что он не будет переиспользоваться.
- **Infrastructure Services**. Тут без комментариев. Все что связано с инфраструктурой.
- **Orchestration Engine**. Служит для интеграции предметных сервисов с **Enterprise & Application** сервисами. Его можно назвать клеем. По закону Конвея эта часть станет узким горлышком, потому что она не так сильно масштабируется как остальные. Ладно бы технически, но там скорее всего появится куча бюрократии, потому что интеграции куча.  

## Микросервисы 
Архитектура, которая выбираем ~максимум независимости сервисов друг от друга~. Это делается за счёт абстрагирования (контейониризации сервиса и инфраструктуры) и без попыток переиспользовать ради **переиспользования**. 

Также они дают ссылочку на [классику](https://martinfowler.com/articles/microservices.html).

> “The purpose of service boundaries in microservices is to capture a domain or workflow. In some applications, those natural boundaries might be large for some parts of the system—some business processes are more coupled than others.”  

Они предлагают вот такие подходы к пониманию размера сервиса: 
- **Purpose**. Логическая связь компонентов внутри сервиса. Они все должны быть cohesive, в рамках одного контекста. 
- **Transactions**. Если что-то должно быть в транзакции, то это что-то должно быть в 1 микросервисе.
- **Choreography**. Если у нас есть несколько сервисов и они не могут функционировать друг без друга (оригинал *“a set of services that offer excellent domain isolation yet require extensive communication to function”*), то это должен быть один сервис.

Также они упоминают, что сервисы ~не должны разделять модели **БД**~. Потому что это противоречит всей философии. Создает связность.

Авторы говорят: **не надо делать транзакции в микросервисах**. Надо правильно предусмотреть границы транзакций, сказать просто — сложно сделать.

- - - -

**Soft-Skils** анти-паттерны: 
- **Covering Your Assets**. Боязнь принять решение. Для того ,чтобы не бояться этого надо общаться с командой разработки, чтобы предупредить ошибки заранее.
- **Groundhog Day**. Когда применяются решения, они должны быть описаны. Особенно причины, чтобы потомки не ломали себе голову каждый день.
- **Email-Driven Architecture**. Не надо фиксировать архитектурные решения в почте. Они должны закрепляться в месте, где их удобно читать и менять.

Они рекомендуют использовать [ADR](https://adr.github.io) для запечатление архитектуры. Разбирать, что такое **ADR** не буду. Все есть по ссылке.

Они советуют не хранить их в **Git**, но мне кажется это плохая практика. Я думаю бизнес может и **Git** посмотреть, не такие они и глупые. Поднимается вопрос о том можно ли использовать **ADR** как альтернативу документации ([C4 Model](https://c4model.com) или [ArchiMate](https://www.opengroup.org/archimate-forum/archimate-overview)). Они говорят, что это хорошее решение, но именно для архитектурных решений, е схем и другого. 

- - - -

Первый делом надо научиться близко к объективному оценивать риск. С этим поможет матрица рисков. Если коротко, то надо рассматривать не только насколько большая проблема, но и насколько часто она может возникать. В дали и мамонт не больше мыши кажется. Они предлагают ознакомиться с активностью: [risk storming](https://riskstorming.com).

- - - -

Они подсвечивают, что какой бы крутой ни был архитектор. Если ~он не может показать и рассказать почему стоит делать так, а не иначе, то грош ему цена~. 

При выборе приложения для рисования схем надо смотреть, чтобы там было: 
- **Слои**. Они позволяю удобно скрывать ненужные детали при демонстрации.
- **Шаблоны**. Для того, чтобы переиспользовать.
- **Автоматическое выравнивание**. Чтобы диаграммы не выглядели беспорядочно.

Надо делать диаграммы однозначными. Неверно понятая диаграмма — хуже, чем её отсутствие. Надо уметь различать презентации-для-рассказа, от презентации-как-описания. В первой важно рассказать пользуясь возможность управлять порядком и вниманием зрителя. Во второй презентация должна быть исчерпывающая. Подробнее про презентации можно почитать в [тут](https://presentationpatterns.com).

- - - -

Ох! Как мне не нравится это название главы 22 (*Делаем команду эффективнее*). Пахнет паршивым менеджментом.

Они указывают, что архитектор не только должен спустить архитектуру в команде. Он должен сопроводить команду по тонкому мостику реализации этого решения. Архитектор должен смотреть за тем, что команда не выходит за ограничения системы, которую они реализуют. 

Далее они выделяют три типа архитекторов 🤦
- **control freak architect**
- **armchair architect**
- **effective architect**
Я освобожу тебя от удовольствия описывать эти персоналии. Кому интересно — почитайте сами. Я предпочел бы вообще н выделять типы, а просто предоставил характеристики. 

Далее они предложили выбрать уровень контроля по: 
- Сплоченность команды больше -> меньше 
- Размер команды больше -> больше
- Опыт команды больше -> меньше
- Сложность проекта больше -> больше 
- Длина проекта больше -> больше.

Не стоит забывать о таких вещах, как: 
- **Process loss**. Больше людей — не значит быстрее работать.
- **Pluralistic ignorance**. Люди бояться выглядеть как дураки.
- **Diffusion of responsibility**. Непонятно кто за что ответственен. Часто проблемы просто пропускаются у всех на глазах.

> “An effective software architect should continually observe facial expressions and body language during any sort of collaborative meeting or discussion and act as a facilitator if they sense an occurrence of pluralistic ignorance”  
🤦

Надо использовать чек-листы, чтобы не забывать рутинные мелочи. Чтобы понять силу этого инструмента они советуют почитать [The Checklist Manifesto](http://atulgawande.com/book/the-checklist-manifesto/). 

Архитектор должен следить, чтобы разработчики не начинали вдаваться сильно ов технические детали.
