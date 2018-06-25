## مقدمه

کتابخانه ها در فریم ورک تد جی اس بعد از نسخه `1.2.0` به دو دسته تقسیم می شوند.

کتابخانه های خارجی و کتابخانه های داخلی. 

کتابخانه های خارجی به نوعی گفته می شود که از طریق توابع `include` و `ainclude` به پروژه اضافه می شوند و به نوع دیگر که از طریق تگ `script` به پروژه الصاق می شوند نوع داخلی گفته می شود.

## کتابخانه نمونه

در ابتدا یک نمونه کتابخانه می نویسیم تا بتوان بهتر با روند کار پیش رفت.

```javascript
// mylib.js

var 
    myElm = ted.define("myelm",["a","b"]),
    multiple = function(a,b){
        return a * b;
    };

ted.create(myElm,function(a,b){
    this.html( multiple( parseInt(a), parseInt(b) ) );
    this.show();
});

this.multiple = multiple;
```

!! نکته
! کد بالا به صورت نمونه نوشته شده و به صورت فرضی در فایل `mylib.js` ذخیره شده است

## کتابخانه خارجی

همانطور که گفته شد ، کتابخانه های نوع خارجی به کتابخانه هایی اطلاق می شود که به وسیله دو تابع `include` و `ainclude` به پروژه الصاق شوند.

دلیل این اتصاب این است که کتابخانه ها بر خلاف نوع داخلی می بایست به صورت یک فایل خارجی طراحی شود و نمی توان به صورت درون خطی در کد های `html` از آن ها استفاده کرد.

### انواع داده ورودی توابع

دو تابع مذکور هر دو فرایند تقریبا یکسانی را انجام می دهند و بلطبع ورودی های یک سانی نیز دارند.

هردو این توابع دو ورودی دریافت می کنند. ورودی اول نام فایل(آفلاین) یا آی دی کتابخانه(آنلاین) و ورودی دوم که به صورت اختیاری می باشد نوع فایل برای پردازش می باشد. با این دو تابع می توان به جز کتابخانه فایل های دیگر را نیز به صورت `ajax` خواند.

ورودی دوم می تواند نوع داده ای از انواع داده زیر باشد:

`js` , `html` , `xml` , `json`

<br/>

#### نمونه ورودی برای فایل های محلی
```javascript
include("file: ./path/file.js"); // Default => js
//------
include("file: ./path/file.json","json");
```

<br/>

#### نمونه ای برای فایل های آنلاین
```javascript
include("system.aml.std"); // Default => js
//--------
include("system.aml.std","xml");
```

### تابع include

این تابع برای وارد سازی کتابخانه یا فایل ها به صورت همزمان استفاده می شود. درصورتی که فایل موردنظر دارای حجم به نسبت زیاد یا دارای پیچیدگی پردازشی باشد، باعث ایجاد اختلال در روند طبیعی اجرای اپلیکیشن شما می شود

!!!! اخطار
! توصیه می شود از این تابع برای وارد سازی کتابخانه یا فایل استفاده نکنید.
! توابع synchronous تاثیر منفی زیادی بر روی رندر صفحه اعمال می کند

#### ساختار

```javascript
include(

    LibraryName : String ,

    (Optional) DataType : String => (Default : "js")

) : any
```

<br/>

#### مثال

@code(width:100%,height:300px)

```html
<myelm a="2" b="3"></myelm>
<br/>
<span id="Multiple"></span>
```js
var myLib = include( "file: mylib.js" );

tedApi.html( "#Multiple", myLib.multiple(1024,2048) );
```css
body{
    color:#fff
}
```ext{js}(https://cdn.tedjs.org/tedjs/1.2.0/js/2ed.min.js)
```ext{js}[mylib]->mylib.js injs
var 
    myElm = ted.define("myelm",["a","b"]),
    multiple = function(a,b){
        return a * b;
    };

ted.create(myElm,function(a,b){
    this.html( multiple( parseInt(a), parseInt(b) ) );
    this.show();
});

this.multiple = multiple;

@endcode


### تابع ainclude

بر خلاف تابع `include` ، این تابع به فرایند وارد سازی را به صورت نا همزمان اجرا می کند تا تداخلی در روند عادی اجرا برنامه به وجود نیاید

#### ساختار

```javascript
ainclude(

    LibraryName : String ,

    (Optional) DataType : String => (Default : "js")

) : [Object ainclude]
```

تابع `ainclude` شی ای از سه تابع زیر را برمیگرداند:

<br/>

#### :: تابع then
  تابع `then` بعد از اینکه دیتا از مقصد دریافت شد ، اجرا می شود .
  این تابع یک آرگومان ورودی به عنوان مقدار دریافتی دارا می باشد.

  <br/>

##### ساختار  
```javascript
[Object ainclude].then(
    callback : Function(
        data : any
    )
) : [Object ainclude]
```

<br/>

#### :: تابع error

درصورتی که خطایی در دریافت محتوا یا پردازش آن رخ دهد این تابع اجرا می شود و خطای آن به تابع `callback` ارسال می شود

  <br/>
  
##### ساختار  
```javascript
[Object ainclude].error(
    callback : Function(
        error : string
    )
) : [Object ainclude]
```

<br/>

#### :: تابع complete 

در خراخوانی یک کتابخانه ، پس از دریافت و اجرای کتابخانه نیاز می باشد صفحه دوباره رندر گرفته ششود تا عناصر جدید جایگزین شود. تابع `complete` این امکان را فراهم می سازد تا بتوان درخواست های خود را پس از اتمام رندر گیری نیز انجام داد.

  <br/>
  
##### ساختار  
```javascript
[Object ainclude].complete(
    callback : Function(
        data : any
    )
) : [Object ainclude]
```

#### مثال

@code(width:100%,height:300px)

```html
<myelm a="2" b="3"></myelm>
<br/>
<span id="message"></span>
```js
ainclude( "file: mylib.js" )
    .then(function(data){
        tedApi.elm("#message").innerHTML += "Library is Evaluated!";
    })
    .complete(function(data){
        tedApi.elm("#message").innerHTML += "<br/> Page Has Been Rendered!";
    })
    .error(function(err){
        alert(err);
    });

```css
body{
    color:#fff
}
```ext{js}(https://cdn.tedjs.org/tedjs/1.2.0/js/2ed.min.js)
```ext{js}[mylib]->mylib.js injs
var 
    myElm = ted.define("myelm",["a","b"]),
    multiple = function(a,b){
        return a * b;
    };

ted.create(myElm,function(a,b){
    this.html( multiple( parseInt(a), parseInt(b) ) );
    this.show();
});

this.multiple = multiple;

@endcode



## کتابخانه داخلی

بر خلاف کتاب خانه های خارجی ، کتابخانه های از نوع داخلی را می توان علاوه بر اینکه می توان به صورت خارجی استفاده کرد ، به صورت درون خطی ، درون کد های `html` یا کد های جاوا اسکریپت استفاده کرد

در این نوع وارد سازی کتابخانه ، از `ajax` استفاده نمی شود بلکه کد ها به صورت مستقیم دریافت شده و پردازش می شوند.

<br/>

#### مثال

```javascript 
tedApi.export.myLib = function(ted, __GLOBALS__){
    var 
        myElm = ted.define("myelm",["a","b"]),
        multiple = function(a,b){
            return a * b;
        };

    ted.create(myElm,function(a,b){
        this.html( multiple( parseInt(a), parseInt(b) ) );
        this.show();
    });

    this.multiple = multiple;
}
```

> ویژگی `export` زیر مجموعه `tedApi` ، یک ویژگی واکنشگراست که به صورت بالا مقدار تابع ورودی خود را پردازش می کتد و در ویژگی زیرمجموعه خود(`myLib`) ذخیره می نماید.

این روش تنها برای وارد سازی کتابخانه ها کاربرد دارد،بر خلاف دو روش قبل.

<br/>

#### مثال

@code(width:100%,height:300px)

```html
<myelm a="2" b="3"></myelm>
<br/>
<span id="multiple"></span>
```js
tedApi.export.myLib = function(ted, __GLOBALS__){
    var 
        myElm = ted.define("myelm",["a","b"]),
        multiple = function(a,b){
            return a * b;
        };

    ted.create(myElm,function(a,b){
        this.html( multiple( parseInt(a), parseInt(b) ) );
        this.show();
    });

    this.multiple = multiple;
}

var MyLib = tedApi.export.myLib;

tedApi.elm("#multiple").innerHTML = MyLib.multiple(1024, 2048)

```css
body{
    color:#fff
}
```ext{js}(https://cdn.tedjs.org/tedjs/1.2.0/js/2ed.min.js)


@endcode