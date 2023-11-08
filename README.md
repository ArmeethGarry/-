# Внешняя обработка для работы с файлами
С помощью данных обработок можно загрузить данные из файла и выгрузить данные в файл, в формате TXT, CSV, XLS(XLSX), DBF и XML

I. Внешняя обработка чтения и загрузки данных из файла в Справочник Номенклатура.

Реализованы следующие возможности.
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
6. Процедура ПрочитатьФайлDBF с помощью объекта <XBase> открываем и читаем файл DBF, после чего проверяем Базу на целостность с помощью
   метода Первая(), если строка не существует, значит Возврат. В противном случае в Цикла с помощью методов ВКонце() и Следующая() заполняем
   построчно таблицу данными.
7. Процедура ПрочитатьФайлXML предназначена для чтения файлов XML. В процедуре реализовано два способа прочтения файла. Способ №1 более
   "тяжеловесный" в плане производительности и реализуется с помощью объекта <ЧтениеXML>. Способ №2 более "легковесный", а так же более
   прост в плане написания кода, реализован с помощью объекта <ФабрикаXDTO>.
   
Создана <Команда> на клиенте и серверная процедура ЗагрузитьДанныеНаСервере для загрузки данных файла в Справочник Номенклатура.
Данную функциональность реализовали следующим способом. Создали Массив Товаров по Артикулу, с его помощью мы исключим "за двоение" данных
и избежим при проверке наличия Товара в справочнике Запроса к серверу в цикле. В цикле проверим наличие Товара в справочнике, если товар
имеется продолжим обход данных, если Товара нет запишем в справочник.

II. Внешняя обработка для выгрузки данных в файл.

1. Реализованы процедура выбора каталога сохранения файла и тумблер выбора формата в котором нужно сохранить файл. Так же созданы команда
   Заполнить и процедура ЗаполнитьНаСервере с помощью которых заполняется Табличная часть формы СписокКонтрагентов. Команда Выгрузить
   производит запись в файл списка контрагентов, в нужном формате.
2. Процедуры ВыгрузитьДанныеВФайлTXT и ВыгрузитьДанныеВФайлCSV реализованы одним способом, с помощью объекта <ТекстовыйДокумент>. В данных
   процедурах из строк табличной части формы заполняется Массив значений, после чего построчно с разделителем ";" записывается в файл.
   
3. Для записи в формате XLS созданы процедуры ВыгрузитьДанныеВФайлXLS и ВыгрузитьДанныеВФайлXLSНаСервере (так как мы работаем с Табличным 
   документом, который имеет доступность на сервере). На клиенте мы создаем Табличный документ и процедура на сервере с помощью объекта
   <ПостроительОтчета> заполняет таблицу и выводит Табличный документ.
4. Процедура ВыгрузитьДанныеВФайлDBF. Создает объект <XBase>, добавляет необходимые поля и создает файл, после чего в цикле проходит
   по табличной части Списка контрагентов, добавляет значения в файл и записывает его.
5. Процедура ВыгрузитьДанныеВФайлXML. Создаем файл XML с помощью объекта ЗаписьXML и открываем файл. Записываем Объявление и значение
   списка. После чего в цикле заполняем структуру документа значениями элементов и значениями текста.

