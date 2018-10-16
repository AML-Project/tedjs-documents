## AJAX
### مقدمه
ای جکس به فرایند ارسال درخواست به وسیله جاوا اسکریپت گفته می شود که در گذشته با داده های XML صورت می گرفته است و امروزه با JSON. البته بدان معنا نیست که نمی توان از XML استفاده نمود.

نحوه عملکرد فرایند ارسال بدین صورت می باشد که ابتدا شی XMLHttpRequest می بایست ایجاد گردد ، سپس داده ها را بر حسب نوع درخواست (GET , POST) به سرور ارسال می نماییم و بر روی درخواست توسط رویدادی شنود می نماییم تا به محض دریافت پاسخ به صورت متنی ، آن را پردازش و عملیاتی را انجام دهیم.

این عملیات در تد جی اس با فرایند آسان تر و خواناتری انجام می گردد و اختیارات مضاعفی را ارائه می دهد.

### متد های ارسال اطلاعات
درخواست ها از طریق سه متد قابل ارسال می باشند:

__GET__ , __POST__ , __JSONP__

*متد پیشفرض : (GET)*

### انواع داده قابل دریافت
داده ها پس از دریافت از سمت سرور می توانند به انواع زیر تبدیل گردند و سپس در دسترس قرار گیرند :

__TEXT__ , __XML__ , __HTML__ , __JSON__

*نوع پیشفرض : (TEXT)*

### ساختار
```javascript
tedApi.http(

    Ajax_Settings : object | string

) : any
```

! مقدار بازگشتی
! درصورتی که ورودی دارای خطا باشد ، مقدار -1 بازگشت داده می شود. درصورتی که درخواست به صورت سنکرون ارسال شده باشد ، مقدار بازگشتی و در غیر این صورت چیزی بازگشت داده نمی شود

### مقدار پارامتر ورودی
مقدار ورودی تابع می تواند متنی (String) و یا شی (object) باشد. درصورتی که ورودی ، متنی باشد ، به عنوان آدرس مقصد لحاظ می گردد و تنها درخواست Async به آدرس مورد نظر ارسال می نماید.

درصورتی که ورودی تابع ، object باشد ، ابتدا به تابع `tedApi.httpSetting` ارسال می گردد و از مقادیر بازگشته به عنوان مشخصات درخواست استفاده می گردد.

### تابع httpSetting
تابع httpSetting ، شی ای را به عنوا ورودی دریافت می نماید و با تنظیمات پیش فرض خود تطابق می دهد . درصورت مطابقت جایگزین می نماید و در انتها تمامی شی تنظیمات درخواست Ajax را بازمیگرداند.

#### ساختار
```javascript
tedApi.httpSetting(

    settings : {
        async : boolean,
        callback : function( res : object ),
        condition : function( readyState : number, status : number ),
        success : function( res : any ),
        error : function( error : object ),
        //default : [Empty]
        data : object,
        //default : [Empty]
        header : object,
        //default : [Current Url]
        url : string,
        //default : 1
        repeat : number,
        //default : false
        repeatToDone : boolean,
        //default : get
        method : "post" | "get" | "jsonp",
        //default : text
        type : "text" | "html" | "xml" | "json"
    }

) : object
```

#### انواع مشخصه های تنظیمات
##### مشخصه async
این مشخصه از نوع `Boolean` می باشد . درصورت درست بودن مقدار ، درخواست به صورت ناهمزمان (Async) ارسال می گردد.

<br/>

##### مشخصه callback
مشخصه callback از نوع `function` می باشد که درصورت ارسال درخواست با متد `jsonp` مورد استفاده قرار می گیرد

<br/>

#### مشخصه condition
این مشخصه یک تابع می باشد که در هر تغییر وضعیت درخواست اجرا می گردد و مقادیر وضعیت را به تابع ارسال نمی نماید.

<br/>

#### مشخصه success و error
دو تابع `success` و `error` ، توابع رخداد می باشند که به ترتیب در زمان موفق بودن ارسال و دریافت پاسخ از سرور و بروز خطا از سمت سرور اجرا می گردند

<br/>

#### مشخصه data
مشخصه `data` ، شی از مشخصه ها و مقادیری می باشد که می بایست ارسال گردند.

<br/>

#### مشخصه header
مشخصه `header` ، شی ای از مشخصه ها می باشد که داده های درونش به عنوان `custom header` ها به درخواست اضافه می گردند و به سرور ارسال می شوند.


<br/>

#### مشخصه url
آدرس سرور مقصد به منظور ارسال درخواست

<br/>

#### مشخصه repeat
از طریق این مشخصه می توان مشخص کرد ، درخواست چه تعداد مرتبه ارسال شود.

<br/>

#### مشخصه repeatToDone
مشخصه `repeatToDone` همانند مشخصه `repeat` عمل می نماید ، با این تفاوت که ، به جای ارسال به تعداد مشخص ، درصورتی که پاسخ سرور دچار خطا شده باشد ، تا زمان دریافت پاسخ صحیح از سرور تا حد امکان تلاش می نماید.


## مدیریت کوکی ها
کوکی ها راه حلی ساده و در دسترس برای تبادل اطلاعات بین client و server می باشد و مدیریت آسان آن ها بسیار مورد نیاز می باشد. 

تابع کوکی در تد جی اس این امکان را فراهم می آورند و امکاناتی نظیر ، ایجاد ، حذف و ویرایش و... را در اختیار قرار می دهد.

فضای نام `tedApi.cookie` دارای چندین تابع به منظور دسترسی به امکانات می باشد :


### ایجاد کوکی
#### ساختار
```javascript
tedApi.cookie.set(

    key : string,

    value : any,
    // optional , Default : 1
    expire_day : number

) : number
```
! مقدار براگشتی
! درصورتی که کوکی ایجاد شده ، قبلا وجود داشته باشد ، مثدار `-1` و درصورتی که مقدار `value`  و یا `key` تعیین نشده باشد ، مقدار `0` و در غیر این صورت `1` بازگشت داده می شود.

<br/>

#### مثال
```javascript
tedApi.cookie.set("a", [1,2,3], 2); /* return 1 */

tedApi.cookie.set("a",2); /* return -1 */

tedApi.cookie.set(1,2); /* return 0 */
```



### ویرایش کوکی
#### ساختار
```javascript
tedApi.cookie.edit(

    key : string,

    value : any

) : number
```
! مقدار براگشتی
! درصورتی که کوکی وجود نداشته باشد ، مثدار `-1` و درصورتی که مقدار `value`  و یا `key` تعیین نشده باشد ، مقدار `0` و در غیر این صورت `1` بازگشت داده می شود.

<br/>

#### مثال
```javascript
tedApi.cookie.edit("a",{a:1}); /* return 1 */

tedApi.cookie.edit(1,2); /* return 0 */

tedApi.cookie.edit("b",[1,2]); /* return -1 */
```

### بررسی وجود کوکی
#### ساختار
```javascript
tedApi.cookie.has(

    key : string

) : boolean
```

<br/>

#### مثال
```javascript
tedApi.cookie.has("a"); /* return true */

tedApi.cookie.has("b"); /* return false */

tedApi.cookie.has([]); /* return false */
```

### بازیابی اطلاعات کوکی
#### ساختار
```javascript
tedApi.cookie.get(

    key : string

) : object
```
! مقدار براگشتی
! درصورت وجود کوکی ، اطلاعات آن بازگشت می گردد و در غیر این صورت مقدار `undefined` بازگشت داده می شود.

<br/>

#### مثال
```javascript
tedApi.cookie.get("a"); /* return Object {name: "a", value: Object, exp: ...} */

tedApi.cookie.get("b"); /* return undefined */
```

### بازیابی مقدار کوکی
#### ساختار
```javascript
tedApi.cookie.val(

    key : string

) : any
```
! مقدار براگشتی
! درصورت وجود کوکی ، مقدار آن بازگشت می گردد و در غیر این صورت مقدار `undefined` بازگشت داده می شود.

<br/>

#### مثال
```javascript
tedApi.cookie.val("a"); /* return Object{a:1} */

tedApi.cookie.val("b"); /* return undefined */
```

### بازیابی زمان انقضای کوکی
#### ساختار
```javascript
tedApi.cookie.timeRemain(

    key : string

) : CookieTime
```
! مقدار براگشتی
! درصورت وجود کوکی ، اطلاعات تاریخ انقضای آن بازگشت می گردد و در غیر این صورت مقدار `undefined` بازگشت داده می شود.

<br/>

#### مثال
```javascript
tedApi.cookie.timeRemain("a"); /* return CookieTime{...} */

tedApi.cookie.timeRemain("b"); /* return undefined */
```

### بازیابی تمامی کوکی ها
#### ساختار
```javascript
tedApi.cookie.all( ) : object
```

<br/>

#### مثال
```javascript
tedApi.cookie.all(); /* return Object{a:Object} */
```

### حذف کوکی
#### ساختار
```javascript
tedApi.cookie.remove(

    key : string

) : boolean
```
! مقدار براگشتی
! درصورتی که `key` به درستی وارد نشده باشد مقدار `undefined` ، درصورت موفقیت ، `true` و در صورت عدم موفقیت ، مقدار `false` بازگشت داده می شود.

<br/>

#### مثال
```javascript
tedApi.cookie.remove("a"); /* return true */
```


### حذف تمامی کوکی ها
#### ساختار
```javascript
tedApi.cookie.clear( ) : void
```
<br/>

#### مثال
```javascript
tedApi.cookie.clear(); 
```


## اطلاعات درخواست (GET)
ارسال اطلاعات به سرور از دو طریق امکان پذیر می باشد. یا می بایست از طریق متد `POST` اطلاعات به Header درخواست الصاق گردند و یا از طریق الصاق داده ها به صورت کوئری متد `GET` به آدرس سرور صورت می پذیرد.

داده هایی که از طریق متد `GET` ارسال می شوند ، توسط جاوا اسکریپت نیز قابل دستیابی می باشند. فضای نام `tedApi.GET` ، مسیری است برای دسترسی به این داده ها.

!! نکته
! داده های آدرس پردازش می شوند و به صورت آبجکت جاوا اسکریپت تبدیل می گردند. مقادیر داده ها می تواند آرایه ، شی ، عدد و... باشد

<br/>

### ساختار
```javascript
tedApi.GET : object
```

### نمونه آدرس 
```curl
http://domain.com/?a=1&b=[1,2,3]&c={a:1}&d="test"&e=2b
```


### مثال
```javascript
tedApi.GET // return GET{a: 1, b: Array[3], c: Object, d: "test", e: "2b"}

tedApi.GET["a"] // return 1

tedApi.GET.b // return [1, 2, 3]
```

## اطلاعات درخواست (URL Hash)
علاوه بر متد های ارسال اطلاعات به سرور ، متدی در سمت client وجود دارد که می توان از طریق آن نیز بین صفحات پویا ، اطلاعات منتقل نمود. علامت شارپ (#) 
درصورتی که پس از آدرس سرور نوشته شود ، تغییرات اطلاعات پس از آن ، باعث بارگذاری مجدد صفحه نمی گردند. این باعث می شود تا راه حل خوبی در انتقال اطلاعات بین صفحات پویا باشد.

ساختار کلی این متد نیز تقریبا همانند `GET` می باشد.
تنها تفاوت ، نحوه ارسال اطلاعات در آدرس سرور می باشد. 
- در متد `GET` ، نشانه شروع ارسال parameter علامت `?` می باشد ، اما در `URL Hash` علامت `#` می باشد.

- مقدار (value) و نام پارامتر (key) درمتد `GET` توسط علامت `=` از یک دیگر متمایز می گردند ، اما در `URL Hash` توسط علامت `:` متمایز می گردند.

<br/>

### ساختار
```javascript
tedApi.urlHash : object
```

### نمونه آدرس 
```curl
http://domain.com/#a:1&b:[1,2,3]&c:{a:1}&d:"test"&e:2b
```

### مثال
```javascript
tedApi.urlHash // return UrlHashData{a: 1, b: Array[3], c: Object, d: "test", e: "2b"}

tedApi.urlHash["a"] // return 1

tedApi.urlHash.b // return [1, 2, 3]
```