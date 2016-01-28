# Генерация каталога #

## Метод передачи данных ##
Размещаемые материалы должны быть опубликованы на сайте Клиента по URL-адресу, согласованному с компанией Ritm-Z, и доступны по протоколам HTTP для автоматического скачивания файла роботом Ritm-Z.
В случае изменения URL-адреса и/или авторизационных данных Клиент должен оповестить менеджера проекта.


## Формат данных ##
Передаваемые данные должны соответствовать спецификации формата YML с дополнительными расширениями формата (DTD). Описание YML представлено по ссылке: http://partner.market.yandex.ru/legal/tt/#id1172362643283, расширения см.ниже.

## Пример структуры генерируемого XML каталога ##
### Для каталога 1 страницей: ###
```
<?xml version="1.0" encoding="UTF-8"?>
<yml_catalog date="2013-06-03 15:47">
    <shop>
        .............
    </shop>
    <currencies>
        .............
    </currencies>
    <categories>
        .............
    </categories>
    <offers>
        .............
    </offers>
</yml_catalog>
```

Так же работать будет и такой вариант, но предпочтительней первый:

```
<?xml version="1.0" encoding="UTF-8"?>
<yml_catalog date="2013-06-03 15:47">
    <shop>
        .............
        <currencies>
            .............
        </currencies>
        <categories>
            .............
        </categories>
        <offers>
            .............
        </offers>
    </shop>
</yml_catalog>
```

### Для каталога в несколько страниц: ###
Что бы получить каталог в несколько страниц, робот отправляет дополнительные GET параметры. Есть 2 типа постраничных запросов:

  * в качестве параметров отправляется номер страницы (page) и количество товаров на странице(count). Значение count стандартно равно 100, значение page начинается с 1, и увеличивается на 1 до тех пор, пока в теге offers не окажется больше элементов offer. Удобно использовать для Bitrix сайтов, в которых можно выставлять страницы в запросах.

  * в качестве параметров отправляется начальный элемент (start) и количество товаров на странице(count). Значение count стандартно равно 100, значение start начинается с 1, и увеличивается на 100 до тех пор, пока в теге offers не окажется больше элементов offer.

#### структура для I страницы: ####
```
<?xml version="1.0" encoding="UTF-8"?>
<yml_catalog date="2013-06-03 15:47">
    <shop>
        .............
    </shop>
    <currencies>
        .............
    </currencies>
    <categories>
        .............
    </categories>
    <offers>
        .............
    </offers>
</yml_catalog>
```

#### структура для II и последующих страниц: ####
```
<?xml version="1.0" encoding="UTF-8"?>
<yml_catalog date="2013-06-03 15:47">
    <offers>
        .............
    </offers>
</yml_catalog>
```

### Описание вложения тега shop ###
```
<shop>
    <name>Название магазина</name>
    <company>Название компании</company>
    <url>Ссылка на магазин</url>
</shop>
```

### Описание вложения тега currencies ###
Элемент currencies задает список курсов валют магазина. Каждая из валют описывается отдельным элементом currency. Параметр id элемента currency указывает код валюты, а параметр rate — курс этой валюты к валюте, взятой за единицу.
```
<currencies>
    <currency id="RUB" rate="1"/>
    <currency id="USD" rate="30"/>
</currencies>
```

### Описание вложения тега categories ###
В элементе categories содержится список категорий магазина. Каждая категория описывается отдельным элементом category. Описание категории должно включать ее идентификатор (параметр id) для всех категорий и идентификатор категории более высокого уровня для подкатегорий. Идентификатор категории должен быть уникальным положительным целым числом. Ни у одной категории параметр id не может быть равен "0". Если элемент parentId не указан, то категория считается корневой.

Параметры:

**id** — идентификатор Вашей категории товаров

**parentId** — идентификатор предыдущей по иерархии категории товаров
```
<categories>
    <category id="1">Книги</category>
    <category id="2">Видео</category>
    <category id="3" parentId="1">Детективы</category>
    <category id="4" parentId="1">Боевики</category>
    <category id="5" parentId="2">Комедии</category>
    <category id="6">Принтеры</category>
    <category id="7">Оргтехника</category>
</categories>
```

### Описание вложения тега offers ###
  * rz\_Active - принимает значение «true», если  товар активен и участвует в продажах, «false» - если товар неактивен, даже если количество товара положительное. Значение по умолчанию - «true»;

Эти параметры должны присутствовать, даже если и не имеют каких то значений, пусть будут просто пустые:
  * rz\_Quantity - cодержит количество товара на складе ИМ в штуках;
  * rz\_Weight - cодержит вес товара с упаковкой в килограммах;
  * rz\_Length - cодержит длину упаковки в метрах;
  * rz\_Width - cодержит ширину упаковки в метрах;
  * rz\_Height - содержит высоту упаковки в метрах;
  * rz\_SupplierName - наименование поставщика;
  * rz\_SupplierCode - код поставщика;
  * rz\_SupplierPrice - цена поставщика. Указывается в случае, если требуется получать отчетность по маржинальной прибыли или установить приоритетность выбора поставщиков одного и того же товара по приоритету цены.
```
<offers>
    <offer id="артикул" availaible="true">
        <name>название товара</name>
        <url>ссылка на страницу с товаром</url>
        <price>цена товара</price>
        <currencyId>id валюты</currencyId>
        <categoryId>id на категорию товара</categoryId>
        <picture>ссылка на картинку товара</picture>
        <rz_Active>принимает то же самое значение что и availaible тега offer</rz_Active>
        <rz_Quantity>количество товара на складе</rz_Quantity>
        <rz_Weight>вес за единицу</rz_Weight>
        <rz_SupplierName>название поставщика</rz_SupplierName>
        <rz_SupplierCode>артикул поставщика</rz_SupplierCode>
        <rz_SupplierPrice>цена поставщика</rz_SupplierPrice>
    </offer>
</offers>
```
id - необходимо ставить значение артикула товара. ВАЖНО ЗНАТЬ!!! Артикул уникален, и в каталоге Ritm-z используется именно его уникальность. Так же менеджеры, при оформлении заказа видят этот артикул, и очень желательно что бы он был выведен на сайте.
availaible - значение активности товара на сайте. Принимает значение true или false.
name - название товара.

Если поставщиков несколько то их можно передать через поля param, вместо отдельных полей rz\_SupplierName, rz\_SupplierCode, rz\_SupplierPrice, пример:
```
<param rz_SupplierCode ="TOP-P28" rz_SupplierName ="Рога и копыта" rz_SupplierPrice ="1000"/>
<param rz_SupplierCode ="В23" rz_SupplierName ="Олма" rz_SupplierPrice ="950"/>
```

Часто бывает такая ситуация, когда покупатель может выбирать какие то характеристики товара, например размер или цвет. Именно их необходимо передавать в полях param:
```
<param name="цвет">красный</param>
<param name="цвет">синий</param>
<param name="размер">39</param>
<param name="размер">40</param>
```
Редки ситуации, когда бывают наборы характеристик. К примеру у нас есть куртка, покупатель может выбрать её цвет и размер, допустим есть 2 цвета, белый и черный, и для белой куртки в наличии 42 и 43 размеры доступны, а для черной, 39 и 41, тогда параметры должны выглядеть так:
```
<param name="цвет">белый</param>
<param name="размер">42</param>
<param name="цвет">белый</param>
<param name="размер">43</param>
<param name="цвет">черный</param>
<param name="размер">39</param>
<param name="цвет">черный</param>
<param name="размер">41</param>
```
но подобные ситуации обычно актуальны для большого количества наименований разных характеристик.

Бывают ситуации, когда для разных характеристик товара имеются разные артикулы. Например куртка белая и черная с разными артикулами, в таком случае лучше их разбить на несколько объектов offer, со своим значением id (артикулом), набором параметров размеров, и комбинированным название товара (пример: Куртка белая).

Самый оптимальный вариант, максимально избавится от параметров в товаре. Для этого можно комбинировать артикулы, не забывая про разделители, и названия товара. Пример всё с той же курткой:
```
<offers>
    <offer id="kurtka;белый;42" availaible="true">
        <name>куртка; белый; 42</name>
        <url>http://www.yandex.ru/</url>
        <price>10000</price>
        <currencyId>RUB</currencyId>
        <categoryId>1</categoryId>
        <picture>http://yandex.st/morda-logo/i/logo.svg</picture>
        <rz_Active>true</rz_Active>
        <rz_Quantity />
        <rz_Weight />
        <rz_SupplierName />
        <rz_SupplierCode />
        <rz_SupplierPrice />
    </offer>
    <offer id="kurtka;белый;43" availaible="true">
        <name>куртка;белый;43</name>
        <url>http://www.yandex.ru/</url>
        <price>10000</price>
        <currencyId>RUB</currencyId>
        <categoryId>1</categoryId>
        <picture>http://yandex.st/morda-logo/i/logo.svg</picture>
        <rz_Active>true</rz_Active>
        <rz_Quantity />
        <rz_Weight />
        <rz_SupplierName />
        <rz_SupplierCode />
        <rz_SupplierPrice />
    </offer>
    <offer id="kurtka;черный;39" availaible="true">
        <name>куртка; черный; 39</name>
        <url>http://www.yandex.ru/</url>
        <price>10000</price>
        <currencyId>RUB</currencyId>
        <categoryId>1</categoryId>
        <picture>http://yandex.st/morda-logo/i/logo.svg</picture>
        <rz_Active>true</rz_Active>
        <rz_Quantity />
        <rz_Weight />
        <rz_SupplierName />
        <rz_SupplierCode />
        <rz_SupplierPrice />
    </offer>
    <offer id="kurtka;черный;41" availaible="true">
        <name>куртка; черный; 41</name>
        <url>http://www.yandex.ru/</url>
        <price>10000</price>
        <currencyId>RUB</currencyId>
        <categoryId>1</categoryId>
        <picture>http://yandex.st/morda-logo/i/logo.svg</picture>
        <rz_Active>true</rz_Active>
        <rz_Quantity />
        <rz_Weight />
        <rz_SupplierName />
        <rz_SupplierCode />
        <rz_SupplierPrice />
    </offer>
</offers>
```