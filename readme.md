Приложение собирает данные с сайта и сохраняет их в базу данных. В зависимости от настроек может выполнять сбор практически любых интересующих данных с любого сайта.
Сохранение происходит в указанную таблицу БД PostrgeSQL 9.5+. Работа с сайтом происходит с помощью браузера Firefox 55+, т.о. поддерживается возможность собирать данные с динамически формируемых (javascript) страниц
Сбор данных происходит с "целевых" страниц сайта - они должны обладать однотипным url (уникальным для каждой). Набор данных с каждой из таких страниц представляет собой одну запись в таблице БД.
Алгоритм работы:
1. Заходит на стартовую страницу
2. Ищет и сохраняет ссылки на целевые страницы
3. Переходит на следующую страницу
4. Повторяет п.2-3 пока не достигнут лимит на количество целевых страниц или перейти на следующую не удается
5. Последовательно заходит на все целевые страницы и выполняет их разбор (ищет и сохраняет значения полей)
6. Сохраняет поля страниц в БД


Минимальные системные требования:
Java 8,
OS Windows/Linux x64,
Mozilla Firefox 55
соединяется с БД PostgreSQL 9.5

Настройки:
config/site_config.xml настройки сайта
    * targetPageXPath - путь до ссылок на целевые страницы в формате XPath
    * siteStartingUrl - стартовая страница, с которой начинается поиск
    * nextPageXPath - путь до ссылки на следующую страницу в формате XPath
    * pagesLimit - максимальное количество разбираемых страниц
    * pageName - название целевой страницы, совпадает с именем таблицы в БД
config/DB_config.xml настройки БД
    * connectionUrl - строка соединения с PostgreSQL, например "jdbc:postgresql://localhost:5432/crawler_db1"
    * user - имя пользователя, рбаотающего с базой
    * password - пароль пользователя
config/fields.xml настройки полей страниц (веб-элементов с интересующими данными), каждое новое поле пишется в новом элементе Field
    * name - имя поля
    * тип данных поля (для сохранения в БД). Поддерживаются типы varchar, integer, date, timestamp (информацию о типах см. в документации PostgreSQL https://www.postgresql.org/docs/9.2/static/datatype.html)
    * xpath - путь до поля в формате XPath


Запуск:
Для запуска достаточно файлов и папок находящихся в \out\artifacts\crawler2db_jar:
* папка config с настройками
* драйвер браузера geckodriver для соответствующей ОС (для Linux необходимо установить права на выполнение файла)
* исполнямый файл crawler2db.jar
Для запуска необходимо выполнить "java -jar crawler2db.jar"

Замечания:
В таблице сохраняются поля прописанные в fields.xml, а также уникальный ид. записи, url страницы и время сохранения
Сохранение можно выполнять при отсутствии указанной таблицы в БД  (будет создана заново) и при наличии (записи с новыми url будут добавлены, при совпадении url - обновлены, остальные записи в таблице не изменятся)
В папке config приведены настройки, позволяющие собирать данные с сайтов zarplata.ru (корректные названия файлов настроек) и zoo-zoo.ru (site2_config.xml и fields_zoo.xml). С сайта zarplata.ru собираются не все данные, а только данные раздела "бухгалтерия-финансы-банки" с ограничением количества резюме

