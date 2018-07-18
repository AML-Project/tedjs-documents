## چیست؟

تابع گلوبالز ، مجموعه توابعی است که در `tedApi` به طور معمول وجود دارد و برای مدیریت و انتخاب و حذف `DOM` ها کمک می کنند. این توابع در گلوبالز (`__GLOBALS__`) جمع آوری شده اند و این قابلیت به آن ها داده شده تا بتوان یک مجموعه محلی از آن ها ایجاد کرد. به عنوان مثال تحت اشاره گر `this` در توابع ایجاد عناصر ، این تابع ایجاد شده است

!!! نکته
! این تابع تنها از طریق کتابخانه ها در دسترس می باشد

## مقادیر بازگشتی

این تابع ، به عنوان یک کلاس عمل کرده و توابعی را در دسترس قرار می دهد. 

!! توجه
! تمام این توابع در `tedApi` در دسترس هستند . مستندات این توابع در صفحات دیگر موجود است

### تابع show

تابعی برای نمایش یک عنصر یا مشخص کردن درصد پدیداری عنصر جاری می باشد
#### ساختار
```javascript
[Object Globals].show(
    // optional, default : "show"
    value : string | number

) : number
```

### تابع hide

تابعی برای ناپدید کردن یک عنصر یا مشخص کردن درصد ناپدیداری عنصر جاری می باشد
#### ساختار
```javascript
[Object Globals].hide(
    // optional, default : "show"
    value : string | number

) : number
```

### تابع parent

تابعی برای بدست آوردن والد چند سطح بالاتر از عنصر جاری
#### ساختار
```javascript
[Object Globals].parent(
    // optional, default : 1
    level : number

) : HTMLElement | null
```

### تابع copyAttr

ایجاد کپی از مشخصه های عنصر ورودی بر روی عنصر جاری
#### ساختار
```javascript
[Object Globals].copyAttr(

    fromNode : HTMLNode

) : number
```


### تابع run

یک تابع `eval` می باشد که اشاره `this` در آن به عنصر جاری اشاره می نماید
#### ساختار
```javascript
[Object Globals].run(

    data : string

) : any
```


### تابع random

این تابع دقیقا مانند تابع `random` می نماید
#### ساختار
```javascript
[Object Globals].random(
    // optional , Default : <Random>
    length : number,
    // optional , Default : "string"
    kind : string

) : string | number
```

### تابع toMiliSecond

این تابع دقیقا مانند تابع `toMiliSecond` می نماید
#### ساختار
```javascript
[Object Globals].toMiliSecond(

    time : string

) : string | number
```

### تابع toTime

این تابع دقیقا مانند تابع `secToTime` می نماید
#### ساختار
```javascript
[Object Globals].toTime(

    milliseconds : number

) : tedDate | number
```


### تابع html

گرفتن محتوای عنصر جاری یا تغییر محتوای عنصر جاری با فرمت html
#### ساختار
```javascript
[Object Globals].html(
    // optional
    text : string

) : string
```



### تابع text

گرفتن محتوای متنی عنصر جاری یا تغییر محتوای متنی عنصر جاری
#### ساختار
```javascript
[Object Globals].text(
    // optional
    text : string

) : string
```



### تابع asHtml

گرفتن عنصر جاری به صورت محتوای متنی یا تغییر تمام عنصر
#### ساختار
```javascript
[Object Globals].asHtml(
    // optional
    text : string

) : string
```


### تابع attr

تغییر و گرفت یک مشخصه یا گرفتن تمامی مشخصه های عنصر جاری
#### ساختار
```javascript
[Object Globals].attr(
    // optional
    attrName : string,
    // optional
    attrValue : any

) : number | Object | string
```

### تابع removeAttr

همانند تابع `removeAttr` برای حذف مشخصه های عنصر جاری به کار می رود
#### ساختار
```javascript
[Object Globals].removeAttr(

    attribute : string

) : number
```


### تابع changeWith

همان تابع `replaceElement` می باشد که عنصر جاری را با عنصر ورودی تعویض می نماید
#### ساختار
```javascript
[Object Globals].changeWith(

    desiredElement : HTMLElement

) : HTMLElement
```



### تابع remove

همان تابع `remove` برای حذف عنصر و جایگذین کردن آن با عنصر فرضی است.
#### ساختار
```javascript
[Object Globals].remove() : HTMLElement | number
```


### تابع delete

همان تابع `delete` برای حذف عنصر است.
#### ساختار
```javascript
[Object Globals].delete() : number
```

### تابع children

همانند تابع `children` ، تمام نود های فرزند در تمامی صطوح را بازمیگرداند
#### ساختار
```javascript
[Object Globals].children(
    // optional
    filter: string

) : Array | number | null
```


### تابع sons

همانند تابع `sons` ، تمام عناصر فرزند در سطح اول را بازمیگرداند
#### ساختار
```javascript
[Object Globals].sons(
    // optional
    filter: string

) : Array | number | null
```


### تابع removeText

همانند تابع `removeText` ، تمامی نود های متنی مود در عنصر جاری را حذف می نماید
#### ساختار
```javascript
[Object Globals].removeText() : number
```


### تابع css

همانند تابع `css` عمل می نماید و استایل های عنصر جاری را دستخوش تغییر قرار می دهد یا مقادیر مشخصه های استایل ها را بازمیگرداند
#### ساختار
```javascript
[Object Globals].css(

    index : string | object,
    // optional
    value : string | function

) : any
```

### تابع ElementStyle

همانند تابع `ElementStyle` استایل های ورودی را به عنوان استایل کلی برای عنصر جاری ثبت نمی نماید
#### ساختار
```javascript
[Object Globals].css(

    cssDatas: Object

) : number
```

### تابع bind

همانند تابع `bind` برای الصاق ایونت به کار می رود
#### ساختار
```javascript
[Object Globals].bind(

    eventName : string,

    callback : function

) : any
```

### تابع find

همانند تابع `find` ، عنصر ورودی را در عنصر جاری جستجو می نماید
#### ساختار
```javascript
[Object Globals].find(

    targetElement : HTMLNode | string

) : NodeList
```


### تابع append

همانند تابع `append` ، مقدار ورودی را به عنصر جاری اضافه می نماید
#### ساختار
```javascript
[Object Globals].append(

    desireElement : any

) : number
```


### تابع reCompile

همانند تابع `compile` می باشد ، این تابع عنصر جاری را دوباره پردازش می نماید و محتوای جدید را جایگذین می کند.
#### ساختار
```javascript
[Object Globals].reCompile() : number
```


### رفرنس self

امشخصه `self` به عنصر جاری اشاره می کند که می توان از طریق آن  به عنصر جاری دسترسی مستقیم داشت
#### ساختار
```javascript
[Object Globals].self : HTMLNode
```


## نحوه استفاده

برای استفاده از این تابع می بایست تنها عنصر مورد نظر را به آن ارسال کرده و آن را فراخوانی نمایید.

```javascript
var GlobRef = __GLOBALS__(document.body);
```

حال از طریق متغیر `GlobRef` می توانید به توابع بالا دسترسی داشته باشید.

```javascript
GlobRef.run("console.log(this.tagName);");
```

به مثال زیر توجه نمایید

@code(width:100%,height:300px)

```html
<div class="red" id="DivBox">این یک متن تست است</div>
```js
tedApi.export.Global = function(__GLOBALS__){
    var DivBox = tedApi.elm("#DivBox");

    var DivBoxRef = __GLOBALS__(DivBox);

    DivBoxRef.self.classList.add("green");
    DivBoxRef.self.classList.remove("red");

    DivBoxRef.bind("click",function(){
        alert("بر روی عنصر کلیک کردید");
    });
}
```css
body{
    color:#fff
}
.red{
    color:red;
}
.green{
    color:green
}
```ext{js}(https://cdn.tedjs.org/tedjs/1.2.0/js/2ed.min.js)

@endcode