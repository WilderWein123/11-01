# Домашнее задание к занятию «Базы данных, их типы» - Серегин Андрей

### Инструкция по выполнению домашнего задания

1. Сделайте fork [репозитория c шаблоном решения](https://github.com/netology-code/sys-pattern-homework) к себе в Github и переименуйте его по названию или номеру занятия, например, https://github.com/имя-вашего-репозитория/gitlab-hw или https://github.com/имя-вашего-репозитория/8-03-hw).
2. Выполните клонирование этого репозитория к себе на ПК с помощью команды `git clone`.
3. Выполните домашнее задание и заполните у себя локально этот файл README.md:
   - впишите вверху название занятия и ваши фамилию и имя;
   - в каждом задании добавьте решение в требуемом виде: текст/код/скриншоты/ссылка;
   - для корректного добавления скриншотов воспользуйтесь инструкцией [«Как вставить скриншот в шаблон с решением»](https://github.com/netology-code/sys-pattern-homework/blob/main/screen-instruction.md);
   - при оформлении используйте возможности языка разметки md. Коротко об этом можно посмотреть в [инструкции по MarkDown](https://github.com/netology-code/sys-pattern-homework/blob/main/md-instruction.md).
4. После завершения работы над домашним заданием сделайте коммит (`git commit -m "comment"`) и отправьте его на Github (`git push origin`).
5. Для проверки домашнего задания преподавателем в личном кабинете прикрепите и отправьте ссылку на решение в виде md-файла в вашем Github.
6. Любые вопросы задавайте в чате учебной группы и/или в разделе «Вопросы по заданию» в личном кабинете.

Желаем успехов в выполнении домашнего задания.

---

### Задание 1. СУБД

### Кейс
Крупная строительная компания, которая также занимается проектированием и девелопментом, решила создать 
правильную архитектуру для работы с данными. Ниже представлены задачи, которые необходимо решить для
каждой предметной области. 

Какие типы СУБД, на ваш взгляд, лучше всего подойдут для решения этих задач и почему? 
 
1.1. Бюджетирование проектов с дальнейшим формированием финансовых аналитических отчётов и прогнозирования рисков.
СУБД должна гарантировать целостность и чёткую структуру данных.

Явно, это должна быть реляционная БД, так как судя по кейсу, данные должны быть доступны широкому спектру сотрудников, в числе которых в т.ч. и администрация, и экономисты; создать T-SQL запрос значительно проще, чем обращаться к NoSQL СУБД. реляционные БД также предоставляют целостность.

1.1.* Хеширование стало занимать длительно время, какое API можно использовать для ускорения работы? 

Если под хешированием подразумевается то что сразу приходит в голову, то кажется что имеем базу данных авторизации (пользователь - хеш пароля). Если у нас таковая БД стала работать с большими задержками - это явно повод мигрироваться на NoSQL, видимо, количество записей уже настолько огромно, что обычный SELECT занимает много времени. 

1.2. Под каждый девелоперский проект создаётся отдельный лендинг, и все данные по лидам стекаются в CRM к 
маркетологам и менеджерам по продажам. Какой тип СУБД лучше использовать для лендингов и для CRM? 
СУБД должны быть гибкими и быстрыми.

Реляционная БД с теми же аргументами, как и в пункте 1.1

1.2.* Можно ли эту задачу закрыть одной СУБД? И если да, то какой именно СУБД и какой реализацией?

Если я правильно понял, лендинг - это одноразовая страничка с заполнением анкетных данных и сохранением в БД. В таком случае, скорость кажется нам совершенно ни к чему, в отличии от отказоустойчивости. А с отказоустойчиовстью лучше справится NoSQL БД. 

1.3. Отдел контроля качества решил создать базу по корпоративным нормам и правилам, обучающему материалу 
и так далее, сформированную согласно структуре компании. СУБД должна иметь простую и понятную структуру.

Документоориентированная СУБД. 

1.3.* Можно ли под эту задачу использовать уже существующую СУБД из задач выше и если да, то как лучше это 
реализовать?

Кажется, что документов будет немного, поэтому берем реляционную БД из пунктов 1.1 / 1.2. 

1.4. Департамент логистики нуждается в решении задач по быстрому формированию маршрутов доставки материалов 
по объектам и распределению курьеров по маршрутам с доставкой документов. СУБД должна уметь быстро работать
со связями.

Графова СУБД, в качестве связей можно указывать физическое местоположение объектов

1.4.* Можно ли к этой СУБД подключить отдел закупок или для них лучше сформировать свою СУБД в связке с СУБД 
логистов?

Можно, но настройки связей будут уже по другим параметрам; либо это будет наименование поставщиков, либо тип товара. 

1.5.* Можно ли все перечисленные выше задачи решить, используя одну СУБД? Если да, то какую именно?

*Приведите ответ в свободной форме.*

Релятивная СУБД. Кажется, что ни одна из вышеперечисленных задач не является очень уж привиредливой по скорости и масштабам.

---

### Задание 2. Транзакции

2.1. Пользователь пополняет баланс счёта телефона, распишите пошагово, какие действия должны произойти для того, чтобы 
транзакция завершилась успешно. Ориентируйтесь на шесть действий.

Отправляем запрос на пополнение счета телефона в банк
Запрос принят, со стороны банка отправляем запрос мобильному оператору о наличии и работоспособности пополняемого номера.
Если от мобильного оператора приходит ОК, то запрашиваем баланс банковского счета, достаточно ли средств
Замораживаем средства на счете в банке (если будет потеря связи, чтобы средства вернулись обратно, либо если случится какая-то встречная операция, чтобы средства не списались ниже нуля)
Отправляем сумму на счет мобильного оператора. Если от мобильного оператора пришло "ОК", то замороженные срества делаем полностью недоступными для использования (уменьшаем количество денег на счету на сумму которая ушла)

2.1.* Какие действия должны произойти, если пополнение счёта телефона происходило бы через автоплатёж?

*Приведите ответ в свободной форме.*

Да в целом то же самое :)
---

### Задание 3. SQL vs NoSQL

3.1. Напишите пять преимуществ SQL-систем по отношению к NoSQL. 

Атомарность - при обработке сложной транзации и ошибке на каком-то шаге, либо техническом сбое данные в источнике не меняются. 
Согласованность - уверенность, что данные в любом случае будут консистентны, выполнилась ли транзакция или нет.
Изоляция - выполняя операции минимизировать блокировки к данным. 
Долговечность - ведение логов транзацкий, возможность оперативного отката или восстановления. 
Универсальность и простота, писать T-SQL запросы гораздо проще, чем например http в elasticsearch. 

3.1.* Какие, на ваш взгляд, преимущества у NewSQL систем перед SQL и NoSQL.

*Приведите ответ в свободной форме.*
Высокая скорость и отзывчивость
Масштабирование

---

### Задание 4. Кластеры

Необходимо производить большое количество вычислений при работе с огромным количеством данных, под эту задачу 
выделено 1000 машин. 

На основе какого критерия будете выбирать тип СУБД и какая модель *распределённых вычислений* 
здесь справится лучше всего и почему?

*Приведите ответ в свободной форме.*

Кажется, это должна быть какая-то Coumn-oriented СУБД, т.к. если у нас огромное количество нод с данными, то нужно по максимуму фильтровать запросы; если лучше понять задачу и заранее произвести деление данных, к примеру, "1 машина = 1 таблица", то на выходе будем иметь точечное попадание в нужную таблицу, при этом не нагружая сеть.

---

Задания,помеченные звёздочкой, — дополнительные, то есть не обязательные к выполнению, и никак не повлияют на получение вами зачёта по этому домашнему заданию. Вы можете их выполнить, если хотите глубже разобраться в материале.
