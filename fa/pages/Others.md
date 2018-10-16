## توابع درونی
توابع درونی ، مجموعه ای از توابع می باشند که در کلاس های سیستمی ایجاد یا override شده اند.

### تابع Object.each
`new` : تابع پیمایش بر روی داده های اشیا ، آرایه ها ، متون و هر object دیگر

<br/>

#### ساختار
```javascript
[Any Object].each(

    callback : function( key : string )

) : any
```

!!! توجه
! درصورتی که مقدار `return` تابع `callback` به غیر از `undefined` باشد ، همان مقدار ، برای خروجی تابع `each` نیز در نظر گرفته می شود و از تابع خارج می گردد.

### تابع Object.count
`new` : نشان دهنده تعداد اعضای یک شی . این مشخصه همانند مشخصه `length` عمل می نماید ، با این تفاوت که در تمامی اشیا ، تعداد کل متد یا مشخصه های شی را نمایش می دهد.


<br/>

#### ساختار
```javascript
[Any Object].count : number
```

### تابع Object.equal
`new` : مقایسه دو شی از لحاظ مشخصه ها و متد های موجود در آن ها

<br/>

#### ساختار
```javascript
[Any Object].equal(

    other : object

) : boolean
```

### تابع Object.isEmpty
`new` : بررسی خالی بودن یک شی از نظر وجود هرگونه مشخصه یا متدی در آن

<br/>

#### ساختار
```javascript
[Any Object].isEmpty( ) : boolean
```

### تابع Object.copy
`new` : رونوشت (copy) کردن داده های شی ورودی تابع بر روی شی مورد نظر

<br/>

#### ساختار
```javascript
[Any Object].copy(

    from_obj : object

) : any
```

### تابع Object.push
`new` : اضافه کردن یک مشخصه یا متد به شی کنونی.

!! نکته
! درصورت وجود نام (key) مشابه ، عددی به صورت صعودی به نام جدید اضافه می گردد.

<br/>

#### ساختار
```javascript
[Any Object].push(

    data : object

) : any
```

<br/>

#### مثال
```javascript
var test = { a : 1 };
test.push({
    a : 2,
    b : 3
});
// returns {a: 1, a2: 2, b: 3}
```

### تابع Array.toString
`override` : تبدیل آرایه به صورت string در قالب آرایه - اضافه شدن آرگومان ورودی تعیین کننده

<br/>

#### ساختار
```javascript
[Array].toString(

    array_format : boolean

) : string
```

<br/>

#### مثال
```javascript
var test = [ 1 , 2 , 3 ];

test.toString() // returns "1,2,3"

test.toString(true) // returns "[1,2,3]"
```

### تابع Array.indexOf
`override` : علاوه بر یافتن یک متن در یک Array ، می توان از طریق RegExp نیز جستجو نمود

<br/>

#### ساختار
```javascript
[Array].indexOf(

    pattern : string | RegExp,

    start : number

) : number
```

### تابع String.capital
`new` : بزرگ کردن (Capitalization) حرف ابتدایی متن

<br/>

#### ساختار
```javascript
[String].capital( ) : string
```


### تابع String.escape
`new` : escape کردن کاراکتر های " , ' , \ و /

<br/>

#### ساختار
```javascript
[String].escape( ) : string
```

### تابع String.replace
`override` : اضافه شدن قابلیت جایگذاری چندین مقدار به صورت همزمان

<br/>

#### ساختار
```javascript
[String].replace( 

    pattern : string | RegExp | Array<string | RegExp>,

    replace_value : any

) : string
```

#### مثال
```javascript
var test = "this is a test value";

test.replace( [ "this" , /(test|value)/g ] , function(value){
    return value.capital();
});
//returns "This is a Test Value"
```

## توابع parser
### تابع parseHTML
تبدیل کدهای html در قالب string به DOM

#### ساختار
```javascript
tedApi.parseHTML(

    data : string

) : HTMLDocument
```

### تابع parseXML
تبدیل کدهای XML در قالب string به XMLDocument

#### ساختار
```javascript
tedApi.parseXML(

    data : string

) : XMLDocument
```

### تابع parseJSON
تبدیل کدهای JSON در قالب string  به Object. این تابع درصورت عدم وجود تابع درونی JSON ، از تابع خود برای تبدیل استفاده می نماید

#### ساختار
```javascript
tedApi.parseJSON(

    data : string

) : object
```

## توابع serialize
این توابع برخلاف توابع parser عمل می نمایند

### تابع objectToString
تابع objectToString ، هر شی ای را با هر مقداری تبدیل به قالب متنی اش با همان فرمت می نماید

#### ساختار
```javascript
tedApi.objectToString(

    data : any

) : string
```

#### مثال
```javascript
tedApi.objectToString(
    {
        a: [{
                b:function(){}
            }]
    }
); 
//returns "{"a":[{"b":function(){}}]}"
```

### تابع serialize
تابع serialize ، تقریبا عملکردی همانند تابع بالا دارد ، با این تفاوت که کارکرد متفاوتی دارند. این تابع ، اشیا را برای استفاده در URL آماده می نماید.


#### ساختار
```javascript
tedApi.serialize(

    data : any

) : string
```

#### مثال
```javascript
tedApi.serialize(
    {
        a : 1, 
        b : "hello" , 
        c : [ 1, 2, "lang:fa" ]
    }
); 
//returns "a=1&b=hello&c[0]=1&c[1]=2&c[2]=lang%3Afa"
```


## توابع کار با زمان
### تابع secToTime
تبدیل ثانیه به تاریخ و ساعت

#### ساختار
```javascript
tedApi.secToTime(

    seconds : number

) : tedDate
```

#### مثال
```javascript
tedApi.secToTime(123456789);
//returns {"string":"21:33:9","year":3,"month":10,"day":29,"hour":21,"minute":33,"second":9}
```

### تابع toMiliSecond
تبدیل میلی ثانیه ، ثانیه ، دقیقه  و ساعت به ثانیه

#### ساختار
```javascript
tedApi.toMiliSecond(

    data : string

) : number
```

#### مثال
```javascript
tedApi.toMiliSecond("2h");
//returns 7200000
```

!راهنما
! فرمت داده ارسالی به تابع می بایست به صورت زیر باشد:
! عدد + ( `ms` یا `s` یا `m` یا `h` )
!
!مانند : 25m  
!(25 دقیقه)

### تابع sleep
همانند دیگر زبان ها ، تابع sleep ، فرایند پردازش را برای مدت زمانی مشخص متوقف می نماید. واحد زمان میلی ثانیه (milisecond) می باشد


#### ساختار
```javascript
tedApi.sleep(

    seconds : number

) : boolean
```


## توابع کار با متون

### تابع toPersianNumber
تبدیل اعداد انگلیسی دروین متن (string) به فارسی

#### ساختار
```javascript
tedApi.toPersianNumber(

    data : string

) : string
```


#### مثال
```javascript
tedApi.toPersianNumber("امروز 24 ام مهرماه 1397")
// returns "امروز ۲۴ ام مهرماه ۱۳۹۷"
```



### تابع toUnicode
تبدیل حروف Unicode به ساختار نشانه ای آن


#### ساختار
```javascript
tedApi.toUnicode(

    data : string

) : string
```


#### مثال
```javascript
tedApi.toUnicode("سلام دنیا")
// returns "\u0633\u0644\u0627\u0645 \u062f\u0646\u06cc\u0627"
```

### تابع trim
حذف حروف اضافه و ناپیدا و فضا های خالی - عملکردی مشابه تابع درونی


#### ساختار
```javascript
tedApi.trim(

    data : string

) : string
```


## متفرقه

### تابع ElementStyle
ایجاد استایل برای عناصر مختلف با فرمت css

#### ساختار
```javascript
tedApi.ElementStyle(

    cssDatas : object

) : 0 | 1
```


#### مثال
```javascript
tedApi.ElementStyle({
    "div": {
        background: "#000"
    },
    "div:hover": {
        background: "#fff"
    }
});  // return 1
```


#### مثال
```javascript
tedApi.ElementStyle({
    "#id:before": {
        transition: "all 0.5s",
        "min-width":"100px"
    },
    "td:nth-child(2)": {
        width: "calc(100% - 100px)"
    }
});  // return 1
```


#### مثال
```javascript
tedApi.ElementStyle({
    "#id:before": [],
    "div": {
        width: "20px"
    }
});  // return 0
```


### تابع browser
مشخص نمودن نام مرورگر

#### ساختار
```javascript
tedApi.browser( ) : 'opera' | 'firefox' | 'safari' | 'ie' | 'edge' | 'chrome'
```

### تابع callByParam
فراخوانی تابع و ارسال پارامتر بر اساس نام آرگمان های تابع

#### ساختار
```javascript
tedApi.callByParam( 

    callback : function,

    this_refrence : any,

    parameters : object

) : any
```

#### مثال
```javascript
function foo(bar1,bar2,bar3){
    if(bar1) return this + " bar1!";
    if(bar2) return this + " bar2!";
    if(bar3) return this + " bar3!";
    return this + " anonymous!"
}

tedApi.callByParam(
    foo,
    "Hello",
    {
        bar2:234
    }
) // returns "Hello bar2!"
```

### تابع constObj
ایجاد شی ای ثابت (const) که مقادیرش قابل تغییر نمی باشند.

#### ساختار
```javascript
tedApi.constObj( 

    content : object,

    name : string

) : [<name> Object]
```

#### مثال
```javascript
tedApi.constObj({
      a:1,
      b:"2",
      c:function(){return 3;}
},"Test") 
// returns : Test {a: 1, b: "2", c: ƒ}
```

### تابع passwordCheck
بررسی قدرت رمز عبور از 0 تا 10

#### ساختار
```javascript
tedApi.passwordCheck( 

    password : string

) : number
```

### تابع cursor
تابع موس (cursor) مختصات کنونی موس را در صفحه مشخص می نماید.

این تابع مختصات موس نسبت به صفحه (client) ، عنصری که بر روی آن قرار دارد (pointed) را و همچنین عنصری که به آن اشاره می کند را مشخص می نماید.

#### ساختار
```javascript
tedApi.cursor(  ) : {
    client  : {
        // مختصات موس نسبت به کادر صفحه درحال نمایش
        client   : { top : number , left : number },
        // مختصات موس نسبت به کل ارتفاع و عرض داکیومنت وبسایت
        document : { top : number , left : number },
        // مختصات صفحه نسبت به رزولوشن صفحه نمایش
        screen   : { top : number , left : number }
    },
    pointed : {
        left : number,
        top  : number
    },
    target : HTMLElement
}
```


### تابع offset
مختصات عنصر مورد نظر در صفحه

#### ساختار
```javascript
tedApi.offset( 

    element : HTMLElement

) : { top : number , left : number}
```


### تابع run
عملکردی مشابه `eval` دارد.

#### ساختار
```javascript
tedApi.run( 

    evaluation_data : string

) : any
```