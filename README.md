# Внешняя обработка для работы с файлами
С помощью данной обработки можно загрузить данные из файла, в формате TXT, CSV, XLS(XLSX), DBF и XML

Реалезованны следующие возможности.
1. С помощью процедур (ПутьКФайлуНачалоВыбора и ПослеВыбораФайла) и тумблера ФорматФайла пользователь имеет возможность отфильтровать
   файлы по типу (TXT, CSV. XLS, DBF и XML). После чего в реквизите ПутьКФайлу подставляется полный путь до нужного файла.
2. Для чтения различных типов файлов создано 6 Процедур: ПрочитатьФайлTXT, ПрочитатьФайлCSV, ПрочитатьФайлXLS, ПрочитатьФайлXLSНаСрвере,
   ПрочитатьФайлDBF, ПрочитатьФайлXML.
3. Процедура ПрочитатьФайлTXT реализована с помощью последовательного чтения файла (Объект <ТекстовыйДокумент>). Данный способ больше
   подходит для чтения небольших текстовых файлов.
4. Процедура ПрочитатьФайлCSV реализована с помощью построчного чтения файла (Объект <ЧтениеТекста>). Данный способ больше подходит для
   чтения текстовым больших размеров.
   
   Оба способа чтения текстовых файлов (<ТекстовыйДокумент>, <ЧтениеТекста>) подходят для файлов формата как TXT, так и файлов формата CSV.
   
5. Процедуры ПрочитатьФайлXLS и ПрочитатьФайлXLSНаСервере реализуют функциональность чтения файлов формата XLSX. Реалицация подразумевает
   Клиент - Серверное взаимодействие (будет работать как в файловом режиме, так и при размещении ИБ на сторонем сервере). На клиента 
   помещаем с помощью объекта <ДвоичныеДанные> и Глобального контекста <ПоместитьВоВременноеХранилище> данные файла во временное хранилище,
   в свою очередь получаем на стороне сервера с помощью Глобального контекста <ПоместитьВоВременноеХранилище> и <ПолучитьИмяВременногоФайла>
   данные файла, которые помещаем в Табличный документ.