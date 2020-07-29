## 1. Overview
This repository contains the Seccoll collection for the NER task in the information security domain and related materials.
**ATTENTION! Dividing the dataset into folds used in the papers will be added soon**

## 2. RuCyBERT Model
The RuCyBERT model trained on Russian information security news over 70 epoches can be obtained from https://drive.google.com/file/d/1JEbXaQjNd51vtaIGjw3MHr60qWgxQzbH/view?usp=sharing

| Label | RuBERT F1 span | RuCyBERT F1 span |
|-|-|-|
| DEVICE | 0.429 | **0.535** |
| EVENT | 0.662 | **0.688** |
| HACKER | 0.589 | **0.684** |
| LOC | **0.911** | **0.911** |
| ORG | 0.792 | **0.808** |
| PER | 0.838 | **0.863** |
| PROGRAM | 0.654 | **0.683** |
| TECH | 0.673 | **0.712** |
| VIRUS | 0.459 | **0.613** |
| F1-micro | 0.718 | **0.752** |
| F1-macro | 0.667 | **0.723** |

## 3. Инструкция для разметки текстов по информационной безопасности
Данная инструкция разработана для обеспечения последовательности и единообразия разметки текстов по информационной безопасности. Инструкция включает в себя:
    * подробное описание тегов, которые использовались при разметке корпуса текстов по информационной безопасности (Sec_col);
    * правила выбора тега для именованных сущностей различных типов;
    * описание сложных и неоднозначных контекстов, которые встречаются в текстах по информационной безопасности.

**Общие замечания**<br/>
Именованной сущностью является все то, что начинается или должно начинаться с большой буквы.

&nbsp;&nbsp;&nbsp;&nbsp;(1) премьер **Медведев** (PER) <br/>
&nbsp;&nbsp;&nbsp;&nbsp;Размечается только «Медведев».<br/>
&nbsp;&nbsp;&nbsp;&nbsp;(2) **Московский государственный университет имени М.В.Ломоносова** (ORG)<br/>
&nbsp;&nbsp;&nbsp;&nbsp;Все, начиная со слова «Московский», будет размечено как организация.

Если в тексте встречается именованная сущность, ошибочно написанная не с заглавной буквы, то такая сущность все равно размечается:

&nbsp;&nbsp;&nbsp;&nbsp;(3)  язык программирования **java** (TECH)

Транслитерированные иностранные названия также размечаются вне зависимости от регистра:

&nbsp;&nbsp;&nbsp;&nbsp;(4) … в Mac OS отродясь серийных номеров не было - чай не **виндовс** (PROGRAM).<br/>
&nbsp;&nbsp;&nbsp;&nbsp;(5) Попробуй написать в **МАК** (ORG) писмицо мол, **макос** (PROGRAM) сдох.

Отдельную проблему составляют многословные названия: в одних случаях многословное название (имя) содержит одну именованную сущность, в некоторых – две (вероятно, может быть и более, но на данный момент такие примеры не зафиксированы). Далее будут описаны наиболее распространенные примеры многословных названий (имен).

Если имя человека содержит несколько слов (например, имя и фамилия, фамилия и инициалы или ФИО), то вся цепочка слов выделяется как одна сущность:

&nbsp;&nbsp;&nbsp;&nbsp;(6) **Евгений Шауро** (PER)  поделился своими впечатлениями от платного семинара ЦБ по НПС, на котором выступал **Андрей Петрович Курило** (PER).<br/>
&nbsp;&nbsp;&nbsp;&nbsp;(7) Также **Курило А.П.** (PER)  напомнил, что…

Титулы, звания, должности в сущность не включаются (см. пример (1)).

Если первое слово многословного названия обозначает неуникальную именованную сущность, и при этом второе слово (или несколько последующих слов) также обозначает некоторую независимую сущность, уточняя, таким образом, содержание первого слова, то в таких случаях выделяются две независимые именованные сущности:

&nbsp;&nbsp;&nbsp;&nbsp;(8) **Министерство торговли** (ORG) **США** (LOC) запретило продажу этих чипов…<br/>
&nbsp;&nbsp;&nbsp;&nbsp;(9) Он перешел из ГУБЗИ в **Департамент регулирования расчетов** (ORG) **Банка России** (ORG).

В случаях, когда название некоторого продукта содержит название производящей компании, название организации отдельно не выделяется и считается частью единой именованной сущности. Номер версии продукта также включается в аннотацию сущности:

&nbsp;&nbsp;&nbsp;&nbsp;(10) **MS Internet Explorer XML Parsing Remote Buffer Overflow Exploit** (PROGRAM)<br/>
&nbsp;&nbsp;&nbsp;&nbsp;(11) Множественные уязвимости в **Apple iOS** (PROGRAM) **Apple iOS 5.x** (PROGRAM).

Однако, если идет перечисление версий продукта, и при этом версия содержит только цифры и пунктуаторы, то в состав именованной сущности включается только первая в порядке упоминания версия:

&nbsp;&nbsp;&nbsp;&nbsp;(12) Выпущены версии **Android 7.1** (PROGRAM), 7.1.1 и 7.1.2.

Если же помимо цифр и знаков препинания в названия версий продукта входят буквы, то сущность выделяется на каждой версии

&nbsp;&nbsp;&nbsp;&nbsp;(13) … все компьютеры Macintosh на процессорах **PowerPC G3** (DEVICE), **G4** (DEVICE) и **G5** (DEVICE).<br/>
Если за аббревиатурой в скобках (или после знаков препинания типа «-», «:», «,») следует ее расшифровка или наоборот, за полным названием следует аббревиатура, то вся такая последовательность выделяется как единая именованная сущность:

&nbsp;&nbsp;&nbsp;&nbsp;(14) **Chrome OS (Google Chrome Operating System)** (PROGRAM)  операционная система компании Google…<br/>
&nbsp;&nbsp;&nbsp;&nbsp;(15) Уязвимость в **Trend Micro Virus Control System(VCS)** (PROGRAM).

Если за именем персоны (названием организации) в скобках (или после знаков препинания типа «-», «:», «,») следует то же имя (название), но записанное буквами латинского алфавита, то вся такая последовательность выделяется как единая именованная сущность:

&nbsp;&nbsp;&nbsp;&nbsp;(16) **Карстен Нол (Karsten Nohl)** (PER), получивший образование в американском **Университете Вирджинии (University of Virginia)** (ORG)…<br/>
Если именованная сущность заключена в парные знаки препинания (скобки или кавычки), то оба знака включаются в аннотацию: <br/>
&nbsp;&nbsp;&nbsp;&nbsp;(17) … чего в ней только нет: сканер уязвимостей **(nessus)** (PROGRAM), система обнаружения вторжений **(snort)** (PROGRAM)…<br/>

Конструкции с дефисом, где первая часть является именованной сущностью, целиком выделяются как именованная сущность.

&nbsp;&nbsp;&nbsp;&nbsp;(18) Прошедшая неделя запомнилась крупнейшими за всю историю **DDoS-атаками** (VIRUS)…

То, как выбрать тег для таких сущностей, будет подробно описано в разделе «Сложные случаи».

Если упоминания организаций, мест, людей встречаются внутри названия фильма, книги, организации  и т.п., то они отдельно не размечаются:

&nbsp;&nbsp;&nbsp;&nbsp;(19) Площадь **Индиры Ганди** (LOC)<br/>
&nbsp;&nbsp;&nbsp;&nbsp;(20) В **Университете Джексонвиля** (ORG) в США

**Размечаемые сущности**<br/>
Для разметки текстов используются следующие теги:
    * Org
    * Per
    * Loc 
    * Hacker
    * Program 
    * Device
    * Tech
    * Event
    * Virus
**Org**<br/>
Тег Org используется для разметки организаций (в том числе СМИ). Этим же тегом размечаются отделы, отделения и подразделения организаций в том случае, если они имеют свое собственное название (см. пример (9)). 

**Per**<br/>
Тег Per используется для разметки персон. Кроме собственно имен, фамилий и ФИО размечаются также никнеймы (их наличие и разнообразные варианты оформления в текстах является спецификой данной предметной области), при этом все небуквенные символы включаются в аннотацию:

&nbsp;&nbsp;&nbsp;&nbsp;(21) …ряд исследователей безопасности, в частности **@ErrataRob** (PER)…<br/>
&nbsp;&nbsp;&nbsp;&nbsp;(22) Цитата **medvid_ik3** (PER) пишет:
    
Если имя человека содержит несколько слов (например, имя и фамилия, фамилия и инициалы или ФИО), то вся цепочка слов выделяется как одна сущность (6) и (7). Титулы, звания, должности в сущность не включаются (см. (1)).

**Loc** 
Тег Loc используется для разметки локаций.
&nbsp;&nbsp;&nbsp;&nbsp;(23) Толпы заполнили проспект **Сахарова** (LOC)…
&nbsp;&nbsp;&nbsp;&nbsp;(24) … позволяет охватывать целиком весь периметр острова **Свободы** (LOC)…
**Event**
Тег Event используется для разметки событий, мероприятий, конференций.
&nbsp;&nbsp;&nbsp;&nbsp;(25) Отгремели две шумные майские конференции **PHD** (EVENT) и **CISO-forum** (EVENT).
&nbsp;&nbsp;&nbsp;&nbsp;(26) Конкурс **Young School** (EVENT), предназначенный для студентов…
**Hacker**
Тег Hacker используется для разметки никнеймов, псевдонимов хакеров, а также для разметки никнеймов, псевдонимов, самоназваний групп хакеров. То есть обычные ФИО и обычные организации не могут быть размечены как Hacker.
&nbsp;&nbsp;&nbsp;&nbsp;(27) … удалось поймать 23-летнего хакера **Iserdo** (HACKER)…
&nbsp;&nbsp;&nbsp;&nbsp;(28) … главный бот-мастер по прозвищу **"Netkairo"** (HACKER) или **"hamlet1917"** (HACKER)…
&nbsp;&nbsp;&nbsp;&nbsp;(29) … хакеры команды **CTF** (HACKER) были не единственными…
&nbsp;&nbsp;&nbsp;&nbsp;(30) … страна противостояла хакерской группировке **Anonymous** (HACKER)…
**Program** 
Тег Program используется для разметки программ – то есть продуктов или фрагментов, компонентнов этих продуктов, которые могут быть использованы для решения тех или иных задач.
К программам относятся:
&nbsp;&nbsp;&nbsp;&nbsp;• операционные системы;
&nbsp;&nbsp;&nbsp;&nbsp;(31) … TFO используется в **iOS 9** (PROGRAM)…  
&nbsp;&nbsp;&nbsp;&nbsp;• браузеры и другие скачиваемые и устанавливаемые на компьютер программы;
&nbsp;&nbsp;&nbsp;&nbsp;(32) … распространяются на браузеры **Microsoft Internet Explorer 10** (PROGRAM) и **Google Chrome** (PROGRAM)…
&nbsp;&nbsp;&nbsp;&nbsp;(33) **Adblock** (PROGRAM) выперли из магазина…
&nbsp;&nbsp;&nbsp;&nbsp;(34) При закрытии окна из процессов **Диспетчера задач** (PROGRAM) исчезают…
&nbsp;&nbsp;&nbsp;&nbsp;• сайты
&nbsp;&nbsp;&nbsp;&nbsp;(35) … презентации с серии семинаров доступны на **SlideShare** (PROGRAM)…  
&nbsp;&nbsp;&nbsp;&nbsp;• файлы и процессы
&nbsp;&nbsp;&nbsp;&nbsp;(36) **Autorun.exe** (PROGRAM) - программа, которая обычно запускает… 
&nbsp;&nbsp;&nbsp;&nbsp;(37) … одна будет внедрена в процесс mu.exe (PROGRAM), другая в процесс **SpiService.exe** (PROGRAM).
&nbsp;&nbsp;&nbsp;&nbsp;(38) … не забудьте использовать файлик **ipfilter.dat** (PROGRAM).
&nbsp;&nbsp;&nbsp;&nbsp;• различные программы-службы
&nbsp;&nbsp;&nbsp;&nbsp;(39) Насчет службы **"Запуск серверных процессов DCOM"**  (PROGRAM) я наблюдал следующие странности…
&nbsp;&nbsp;&nbsp;&nbsp;(40) … что делает служба **"Удаленный вызов RPC-процедур"** (PROGRAM)?
&nbsp;&nbsp;&nbsp;&nbsp;• отдельно упомянутые методы, функции и команды на каком-либо языке программирования
&nbsp;&nbsp;&nbsp;&nbsp;(41) … из-за ошибки завышения на единицу в функции **png_formatted_warning()** (PROGRAM).
&nbsp;&nbsp;&nbsp;&nbsp;• утилиты
&nbsp;&nbsp;&nbsp;&nbsp;(42) Например, утилита **Autorun** (PROGRAM)…
&nbsp;&nbsp;&nbsp;&nbsp;• библиотеки, каталоги, средства разработки
&nbsp;&nbsp;&nbsp;&nbsp;(43) … **SDK** (PROGRAM) для разработки…
&nbsp;&nbsp;&nbsp;&nbsp;(44) Теперь контекстные и **PCRE** (PROGRAM) опции правила могут…
К программам НЕ относятся:
    * Ссылки и директории
    * Крупные непрерывные части кода
    * Сокращения типа «ПО», «ОС»,  «СПО», которые являются аббревиатурами, но не именованными сущностями
Разметке подлежат «русифицированные» названия программ:
&nbsp;&nbsp;&nbsp;&nbsp;(45) Еще удобная фича **огнелиса** (PROGRAM) - кастомный юзерагент…
&nbsp;&nbsp;&nbsp;&nbsp;(46) Восстановив пароль от **гмыла** (PROGRAM)…

**Device**
Тег Device используется для разметки электронного оборудования. Сюда относятся компьютеры, смартфоны, карты памяти, жесткие диски, материнские платы, видеокарты, сервера, модемы, роутеры и др.
&nbsp;&nbsp;&nbsp;&nbsp;(47) Модем **D link500t** (DEVICE)
&nbsp;&nbsp;&nbsp;&nbsp;(48) Ранние версии OS X поддерживали все компьютеры Macintosh на процессорах **PowerPC G3** (DEVICE).
В качестве устройств НЕ выделяются:
    * Автомобили
    * Банковские карты
    * Сокращения типа «МФУ», «БП», «GPU», «ПК», «АРМ», «ЦОД», «СВТ», которые являются аббревиатурами, но не именованными сущностями
    * Словосочетания типа «Smart TV», которые пишутся с большой буквы, но при этом не являются именованными сущностями (они семантически близки словам «ноутбук», «смартфон», которые также не являются именованными сущностями). Данное решение носит дискуссионный характер.

**Virus**
Тег Virus используется для разметки зловредного обеспечения разной природы (вирусы, закладки), а также технологий, которые используются хакерами (атаки, уязвимости):
&nbsp;&nbsp;&nbsp;&nbsp;(49) Часто злоумышленники внедряют в сайт **XSS-уязвимость** (VIRUS)…
&nbsp;&nbsp;&nbsp;&nbsp;(50) Закладка **Внутренности гурмана (GOURMETTROUGH)** (VIRUS) Позволяет полностью управлять маршрутизатором
&nbsp;&nbsp;&nbsp;&nbsp;(51) … система ASSIST подверглась **DDOS-атаке** (VIRUS).
&nbsp;&nbsp;&nbsp;&nbsp;(52) … арест автора набора вредоносных кодов **"Бабочка" (Butterfly)** (VIRUS).
Кроме того, тег Virus приписывается идентификаторам уязвимостей:
&nbsp;&nbsp;&nbsp;&nbsp;(53) Все эксплуатируемые уязвимости мягко говоря с душком: **CVE-2008-5353**(VIRUS).
**Tech**
Тег Tech используется для разметки технологий. Под технологиями мы понимаем сущности, которые не имеют физической формы, могут быть реализованы различными способами и могут использоваться разными людьми на основе разного программного и аппаратного обеспечения. 
&nbsp;&nbsp;&nbsp;&nbsp;(54) Уже к 2020 году в пространстве **IoE** (TECH)  будет насчитываться более 50 миллиардов новых устройств.
&nbsp;&nbsp;&nbsp;&nbsp;(55) … поддержка двух SIM-карт **(Dual SIM Dual Standby)** (TECH)…
&nbsp;&nbsp;&nbsp;&nbsp;(56) … полная поддержка **Drag&Drop** (TECH) и копи-пейста…
&nbsp;&nbsp;&nbsp;&nbsp;(57) В отличие от традиционных **SIEM-решений** (TECH), современные решения используют алгоритмы, основанные на техниках машинного обучения.
&nbsp;&nbsp;&nbsp;&nbsp;(58) … для доступа в документе к значению первого тега через **DOM** (TECH) может быть использован…
&nbsp;&nbsp;&nbsp;&nbsp;(59) Устройства, содержащие в себе такие модули беспроводной связи как: **Wi-Fi** (TECH), **Bluetooth** (TECH), **GPRS** (TECH)…
&nbsp;&nbsp;&nbsp;&nbsp;(60) мобильная связь (**2G** (TECH)/**3G** (TECH)/**4G** (TECH))
К технологиям относятся:
&nbsp;&nbsp;&nbsp;&nbsp;• Форматы и расширения
&nbsp;&nbsp;&nbsp;&nbsp;(61) Сервер S1 считывает **JSON** (TECH) ответ…
&nbsp;&nbsp;&nbsp;&nbsp;(62) **XML** (TECH) сегодня это попытка сделать тоже самое…
&nbsp;&nbsp;&nbsp;&nbsp;• Языки программирования
&nbsp;&nbsp;&nbsp;&nbsp;(63) … поддержка **AppleScript** (TECH) для автоматизации работы…
&nbsp;&nbsp;&nbsp;&nbsp;• Протоколы
&nbsp;&nbsp;&nbsp;&nbsp;(64) … там либо **pptp** (TECH) либо **l2tp** (TECH) протокол…
&nbsp;&nbsp;&nbsp;&nbsp;(65) Использует **SSDP** (TECH) для обнаружения интернет шлюзов (маршрутизаторов) и отправляем устройству **SOAP** (TECH) команду.
&nbsp;&nbsp;&nbsp;&nbsp;(66) Хорошее **EPP-решение** (TECH) должно быть в состоянии в любое время идентифицировать изменения…
&nbsp;&nbsp;&nbsp;&nbsp;• Стандарты
&nbsp;&nbsp;&nbsp;&nbsp;(67) Конфиденциальность обеспечивается с помощью симметричного шифрования (например, **AES256** (TECH)).
&nbsp;&nbsp;&nbsp;&nbsp;(68) … сертификацию информационной системы компании на соответствие стандарту **PCI DSS** (TECH).
&nbsp;&nbsp;&nbsp;&nbsp;(69) … влияния на бизнес, в терминологии **ГОСТ 27005-2010** (TECH).  
&nbsp;&nbsp;&nbsp;&nbsp;• Некоторые русскоязычные аббревиатуры (и их расшифровки при написании с заглавной буквы), обозначающие типовые подходы к решению тех или иных задач: 
        * СЗ - средство защиты 
        * СЗИ, СрЗИ - средство защиты информации
        * СКЗИ - средство криптографической защиты информации
        * СЗПДн - система защиты персональных данных
        * СУБД - система управления базами данных
        * СТР-К - специальные требования и рекомендации по технической защите конфиденциальной информации
        * СТР - специальные требования и рекомендации 
        * СЭД - система электронного документооборота
        * СКУД - система контроля и управления доступом
        * СОИ – система обработки информации
        * ИТ – информационные технологии
        * ИКТ - информационно-коммуникационные технологии
        * КИИ - критическая информационная инфраструктура
        * СТО БР ИББС - стандарт Банка России по обеспечению информационной безопасности организаций банковской системы
        * ИСПДн - информационная система персональных данных
        * АСР - автоматизированная система расчётов
        * ТЗКИ - техническая защита конфиденциальной информации
        * СУИБ - система управления информационной безопасностью
        * ВТСС – вспомогательные технические средства и системы
        * ОТСС – основные технические средства и системы
        * СИБ – система информационной безопасности
        * СДЗ - средство доверенной загрузки
        * АСУ ТП - автоматизированная система управления технологическим процессом
        * АБС - автоматизированная банковская система

К технологиям НЕ относятся:
    * Аббревиатуры BYOD, CYOD и их расшифровки Bring Your Own Device, Choose Your Own, так как они обозначают некоторые бизнес-подходы, но не технологические подходы.
    * Именованные сущности типа «СЗИ 3.5», которые, несмотря на то что содержат аббревиатуру из списка выше, являются примерами конкретных продуктов (в данном случае, это программа).
Сложные случаи
При анализе текстов коллекции были зафиксированы случаи, когда выбор тега для именованной сущности зависит от контекста. 
    1. Организация/Локация
&nbsp;&nbsp;&nbsp;&nbsp;(70) … даже в систему управления **Кремля** (ORG)…
«Кремль» может также являться локацией. Однако, в случаях, когда речь идет о «системе управления» или о «решениях» Кремля, тег Org представляется более удачным.
    2. Организация/Программа
&nbsp;&nbsp;&nbsp;&nbsp;(71) Если ваш работодатель слил ваши данные без вашего согласия в администрацию/УВД и **Яндекс** (PROGRAM).
&nbsp;&nbsp;&nbsp;&nbsp;(72) Как не потерять клиентов или технологии **Яндекса** (ORG) для вашего бизнеса.
В случаях, когда название компании совпадает с производимым продуктом, при выборе тега необходимо учитывать контекст.
    3. Дефисное написание 
Как уже упоминалось выше, могут возникать сложности при выборе тега для слов с дефисным написанием. В общем случае тег определяется словом, следующим за дефисом. 
        3.1. Программа/Девайс
&nbsp;&nbsp;&nbsp;&nbsp;(73) Еще один банковский троян для **Android-устройств** (DEVICE).
При независимом употреблении «Android» получает тэг Program, поскольку является программным обеспечением. В данном контексте речь идет об устройстве, имеющем физическую форму, следовательно, выбирается тег Device.
        3.2. Технология/Девайс
&nbsp;&nbsp;&nbsp;&nbsp;(74) … они не смогут обслуживать абонентов со старыми **GSM-телефонами** (DEVICE).
При независимом употреблении «GSM» получает тэг Tech, поскольку это стандарт цифровой мобильной сотовой связи. В данном контексте речь идет об устройстве, имеющем физическую форму, следовательно, выбирается тег Device.
    4. Программно-аппаратное обеспечение
В нашей выборке встречаются сущности, которые могут обозначать как программные, так и программно-аппаратные средства и комплексы. Например, межсетевые экраны. Такие сущности размечаются как Program, если это не противоречит более широкому контексту. Данное решение несет дискуссионный характер.
    5. Отдельные команды внутри блоков кода
Как уже упоминалось ранее, большие непрерывные части программного кода не являются именованными сущностями и не размечаются. При этом отдельные команды и методы, которые упоминаются в тексте, размечаются и получают тег Program. Названия команд и методов, которые входят в непрерывные части кода, не считаются именованными сущностями и не размечаются.
Прочие релевантные сущности
Помимо именованных сущностей, перечисленных выше, следует упомянуть еще несколько, которые являются релевантными для данной предметной области.
    а. Названия кнопок
Backspace, Shift, Ctrl
    б. Названия директорий
C:Program Files (x86)Javajre7bin; D/windows/sys32/x
    в. Ccылки
http://nonverbalicon.blogspot.com/, http://www.gemalto.com/emv
    г. Названия ошибок
Checksum Error
    д. Криптовалюта
Ethereum (Эфириум), биткоин
    е. Банковские карты/Платежные системы
Visa, MasterCard
    ж. Параметры
MTU, Read only
    з. Факторы
Organizational Loss Factors, External Loss Factors, Sensitivity
Соотнести данные сущности с какими-либо из используемых тегов не представляется возможным, поэтому на данном этапе проекта они не размечаются.
