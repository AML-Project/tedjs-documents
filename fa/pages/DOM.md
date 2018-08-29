## توضیحات
توابع DOM ، از جمله توایعی هستند که برای مدیریت بهتر و سریع تر DOM ها طراحی شده اند. این توابع بیشتر نیاز های برنامه نویس را برای مدیریت  و نیاز به استفاده از کتابخانه هایی مانند `jQuery` برای استفاده ابتدایی برطرف می نماید.

!! نکته
!این توابع زیر مجموعه شی `tedApi` می باشند


## تابع ready
تابع `ready` به عنوان یک رویداد عمل می نماید. این تابع یک DOM را به عنوان ورودی میگیرد و زمانی که تحلیلگر عناصر تد جی اس به عنصر مورد نظر برسد ، تابع مشخص شده اجرا می شود.

!!!توجه
! ورودی اول تابع می تواند سلکتور یا DOM باشد ، اما درصورتی که خروجی سلکتور آرایه ای از DOM ها یا نود لیست باشد ، اولین مقدار آن انتخاب می شود.

<br/>

!!نکته
! اشاره گر this در تابع callback ، یک شی از تابع `__GLOBALS__` می باشد


#### مثال
```javascript
tedApi.ready("div", function(){
    this.self.classList.add("active");
});
```
خروجی : __1__

<br/>

#### مثال
```javascript
var divs = tedApi.elm("div");
tedApi.ready(divs, function(){
    this.self.classList.add("active");
});
```
خروجی : __0__


<br/>

#### مثال
```javascript
var divs = tedApi.elm("div");
tedApi.ready(divs[0], function(){
    this.self.classList.add("active");
});
```
خروجی : __1__


#### ساختار
```javascript
tedApi.ready(
 
    selector : (string , HTMLElement) ,

    callback : function
      
) : number
```


## تابع docReady
همانند تابع `ready` عمل می کند با این تفاوت که تابع ورودی تنها زمانی اجرا می شود که `document` آماده شده باشد.

<br/>


#### مثال
```javascript
tedApi.docReady(function(){
    // do somthing that need document be ready
});
```

#### ساختار
```javascript
tedApi.docReady(

    callback : function
      
) : void
```



## تابع elm
تابع `elm` تابعی برای انتخاب انواع DOM نظیر عناصر و تگ ها می باشد که ورودی آن سلکتوری با فرمت `string` می باشد.

همچنین می توان عناصر دلخواه خود را در آن طراحی کنید و به صورت یک شی خروجی بگیرید.

<br/>


#### مثال
```javascript
tedApi.elm("div","p,#elmId");
```
خروجی : __[ NodeList(nth) , NodeList(nth) , HTMLElement#elmId ]__


<br/>

#### مثال
```javascript
tedApi.elm("div > p + a[class~='lang']");
```
خروجی : __درصورتی که بیشتر ار یک عنصر یافت شده باشد یک ارایه برمیگردد در غیر این صورت همان عنصر__


<br/>

#### مثال
```javascript
tedApi.elm("<div/>",'<a href="#link">test</a>');
```
خروجی : __[ div, a ]__

<br/>

#### مثال
```javascript
tedApi.elm("<!-- Test Comment -->");
```
خروجی : __<\!-- Test Comment -->__

<br/>

#### مثال
```javascript
tedApi.elm(document.getElementById("id"));
```
خروجی : __[Object HTMLElement]__

#### ساختار
```javascript
tedApi.elm(
 
    arg1 : (string , HTMLElement) ,
    //optional
    arg2 : (string , HTMLElement) ,

    ...
    //optional
    argn : (string , HTMLElement)  
      
) : (Array<NodeList>[] , Array<HTMLElement>[] , HTMLElement , null)
```



## تابع find
تابع `find` همانند تابع `elm` عمل می کند با این تفاوت که برخلاف تابع قبل که تنها در قسمت `document` جستجو می کند ، درون عنصری که به تابع گفته می شود ، جستجو می کند و عنصر یا عناصر مورد نظر را بازمیگرداند

<br/>

#### مثال
```javascript
tedApi.find("body","div#elmId");
```
خروجی : <span style="direction:ltr;display:inline-block;">__NodeList[ div ]__</span>

<br/>


#### مثال
```javascript
tedApi.find(document.body,"div");
```
خروجی : <span style="direction:ltr;display:inline-block;">__NodeList[ ... ]__</span>

<br/>


#### مثال
```javascript
tedApi.find("<p><a href='#'>this is a <span>link</span></a></p>", "span" );
```
خروجی : <span style="direction:ltr;display:inline-block;">__NodeList[ span ]__</span>

<br/>

#### مثال
```javascript
tedApi.find(document,"*"); 
```
خروجی : <span style="direction:ltr;display:inline-block;">__NodeList[ ... ]__</span>

<br/>

#### ساختار
```javascript
tedApi.find(
 
    sourceElement : (HTMLElement , string) ,
 
    targetElement : string
      
) : NodeList
```



## تابع append
انتقال عناصر از محلی به محل دیگر در شی DOM. تابع `append` همانند تابع مشابه خود در جاوا اسکریپت عمل می نماید. 

!!! توجه
! تابع append عنصر مورد نظر را به مکان جدید مشخص شده اش __منتقل__ می نماید . به این معنی که از مکان قبلیش حذف می شود.

<br/>

!! فرایند
!این تابع ، ورودی دوم را درصورتی که یک DOM باشد به درون ورودی اول خود (درصورتی که DOM باشد) منتقل می نماید

<br/>

#### مثال
```javascript
tedApi.append("#imagebox","#image");
```
خروجی : __1__

<br/>


#### مثال
```javascript
tedApi.append("body","<a href=\"#\">New Link</a>");
```
خروجی : __1__

<br/>


#### مثال
```javascript
tedApi.append(tedApi.elm("#Container"),document.getElementById("#div"));
```
خروجی : __1__

<br/>

#### ساختار
```javascript
tedApi.append(
 
    destinationElement : (HTMLElement , string) ,
 
    desireElement : (HTMLElement , string) 
      
) : number
```

## تابع insert
این تابع همانند تابع `append` عمل می نماید با این تفاوت که می توانید مشخص نمایید که در کجای فضای عنصر مقصد قرار بگیرد.
به عنوان مثال ابدا ، آخر ، قبل و یا بعد از عنصر

تابع `insert` پس از اجرا شی ای متشکل از 4 تابع باز میگرداند که وظیفه رسیدگی به وظایف گفته شده را دارند.

```javascript
var insert = tedApi.insert("<span>this is a test</span>");
```

#### ساختار
```javascript
tedApi.insert(
 
    element : (HTMLElement , string)
      
) : Insert
```

!! نکته
! خروجی توابع 4 گانه ، بولین (`true` یا `false`) می باشد

### تابع first
انتقال عنصر مشخص شده به ابتدای عنصری که در این تابع مشخص می شود.
```javascript
insert.first("body")
```

#### ساختار
```javascript
tedApi.insert(...).first(
 
    element : (HTMLElement , string)
      
) : boolean
```

### تابع end
انتقال عنصر مشخص شده به انتهای عنصری که در این تابع مشخص می شود.
```javascript
insert.end(document.body)
```

#### ساختار
```javascript
tedApi.insert(...).end(
 
    element : (HTMLElement , string)
      
) : boolean
```

### تابع before
انتقال عنصر مشخص شده به قبل از عنصری که در این تابع مشخص می شود.
```javascript
insert.before(tedApi.elm("#maintext"))
```

#### ساختار
```javascript
tedApi.insert(...).before(
 
    element : (HTMLElement , string)
      
) : boolean
```

### تابع after
انتقال عنصر مشخص شده به بعد از عنصری که در این تابع مشخص می شود.
```javascript
insert.after(tedApi.elm("#maintext"))
```

#### ساختار
```javascript
tedApi.insert(...).after(
 
    element : (HTMLElement , string)
      
) : boolean
```


## تابع children
وظیفه این تابع بازگرداند ن تمامی Node های فرزند در تمامی سطوح و به ترتیب می باشد.

!! نکته
! این تابع دارای دو ورودی می باشد که ورودی دوم اختیاریست. 
! درصورتی که می خواهید تنها عناصر فرزند از یک دسته مانند `div` ها یا `a` ها را بدست آورید ، تنها نام تگ را در ورودی دوم وارد نمایید

<br/>

!!! توجه
! درصورتی که ورودی اول سلکتور(در قالب متن) باشد اما عنصری سلکت نشود یا شی باشد اما انواعی از عنصر ، تکست نود یا کامنت نود نباشد ، مقدار `-1` و در غیر این صورت مقدار `null` بازگشت داده خواهد شد.
! درصورتی که خطا های بالا رخ ندهد ، آرایه از فرزندان بازگشت داده می شود

<br/>

#### مثال
```javascript
tedApi.children("body");
```
خروجی : <span style="direction:ltr;display:inline-block;">__Array[...]__</span>

<br/>


#### مثال
```javascript
tedApi.children({});
```
خروجی : __-1__

<br/>


#### مثال
```javascript
tedApi.children("-NotASelector");
```
خروجی : __-1__

<br/>

#### مثال
```javascript
tedApi.children(56565);
```
خروجی : __null__

<br/>

#### مثال
```javascript
tedApi.children("body","div");
```
خروجی : <span style="direction:ltr;display:inline-block;">__Array[..div..]__</span>

<br/>

#### ساختار
```javascript
tedApi.children(
 
    element : (HTMLElement , string) ,
    // optional
    tagName : string
      
) : number | null | Array
```

## تابع sons
این تابع همانند تابع `children` عمل می نماید با این تفاوت که تنها فرزندان سطح اول را بازمیگرداند

!!! توجه
! بر خلاف تابع `children` ،  این تابع TextNode ها و CommentNode ها را بازنمی گرداند.

<br/>


#### مثال
```javascript
tedApi.sons("body");
```
خروجی : <span style="direction:ltr;display:inline-block;">__Array[...]__</span>

<br/>


#### مثال
```javascript
tedApi.sons({});
```
خروجی : __-1__

<br/>


#### مثال
```javascript
tedApi.sons("-NotASelector");
```
خروجی : __-1__

<br/>

#### مثال
```javascript
tedApi.sons(56565);
```
خروجی : __null__

<br/>

#### مثال
```javascript
tedApi.sons("body","div");
```
خروجی : <span style="direction:ltr;display:inline-block;">__Array[..div..]__</span>

<br/>

#### ساختار
```javascript
tedApi.sons(
 
    element : (HTMLElement , string) ,
    // optional
    tagName : string
      
) : number | null | Array
```

## تابع parent

تابع `parent` وظیفه دارد تا نود والد ، نود در حال استفاده را تا هر سطحی بازگرداند.

مقدار بازگشتی این تابع درصورتی که خطایی رخ دهد `null` خواهد بود در غیر این صورت نود والد را بازمیگرداند

#### مثال
درصورتی که عناصر به این صورت طراحی شده باشند:
```html
<parent-level3>
  <parent-level2>
   <parent-level1>
      <element id="element"/>
   </parent-level1>
  </parent-level2>
</parent-level3>
```

آن گاه توابع زیر هر یک یکی از سطوح بالا را بازمیگرداند:
```javascript
tedApi.parent("#element") // parent-level1

tedApi.parent("#element",1) // parent-level1

tedApi.parent("#element",2) // parent-level2

tedApi.parent("#element",3) // parent-level3

```

#### ساختار
```javascript
tedApi.parent(
 
    element : (HTMLElement , string) ,
    // optional , Default: 1
    level : number
      
) : null | HTMLElement
```

## تابع replaceElement
از طریق این تابع می تواند عناصر مختلف را جایگزین یک دیگر نمایید بدون از دست دادن رفرنس به آن ها

!! فرایند
! این تابع ورودی اول خود را جایگزین عنصری که در ورودی دوم آمده است می کند.

#### مثال
```javascript
tedApi.replaceElement("div#new","#old");
```
خروجی : <span style="direction:ltr;display:inline-block;">__div#new__</span>

<br/>

#### ساختار
```javascript
tedApi.replaceElement(
 
    desiredElement  : (HTMLElement , string) ,

    destinationElement  : (HTMLElement , string)
      
) : HTMLElement
```

## توابع حذف عنصر

### تابع delete
این تابع عنصر یا مجموعه عناصری را در ورودی دریافت می نماید و آن ها را حذف می کند. در صورتی که موفقیت آمیز باشد 1 در غیر این صورت 0 باز میگرداند

#### مثال
```javascript
tedApi.delete("p");
```
خروجی : <span style="direction:ltr;display:inline-block;">__1__</span>

<br/>

#### مثال
```javascript
tedApi.delete("#id");
```
خروجی : <span style="direction:ltr;display:inline-block;">__1__</span>
<br/>
#### مثال
```javascript
tedApi.delete(document.querySelectorAll("div"));
```
خروجی : <span style="direction:ltr;display:inline-block;">__1__</span>

<br/>
#### مثال
```javascript
tedApi.delete(";d");
```
خروجی : <span style="direction:ltr;display:inline-block;">__0__</span>

<br/>

#### ساختار
```javascript
tedApi.delete(
 
    element  : (HTMLElement , string) 
      
) : number
```

### تابع remove
همانند تابع `delete` عمل می نماید . یک عنصر ورودی خود را حذف می نماید با این تفاوت که آن هارا با عنصری رندم جایگزین می نماید تا ترتیب عناصر به هم نریزد

درصورتی که عنصر حذف و جایگزین شود ، عنصر رندم بازگشت داده مشود

#### مثال
```javascript
tedApi.remove("p");
```
خروجی : <span style="direction:ltr;display:inline-block;">__0__</span>

<br/>

#### مثال
```javascript
tedApi.remove("#id");
```
خروجی : <span style="direction:ltr;display:inline-block;">__Removed-Elm_[RandomNumber]__</span>



#### ساختار
```javascript
tedApi.remove(
 
    element  : (HTMLElement , string) 
      
) : number | HTMLElement
```



## توابع کار با محتوا

### تابع asHtml

در مواقعی که نیاز باشد کلیت عنصر مورد نظر را به صورت متنی مشاهده کنید یا یک کپی از آن را ذخیره کنید ، می تواند از خاصیت `outerHTML` جاوا اسکریپت استفاده نمایید. این خاصیت از طریق تابع `asHtml` نیز در دسترس می باشد. علاوه بر مشاهده می توانید عنصر را با یک عنصر جدید به صورت متنی جایگزین کنید.

!!! توجه
! در صورتی که ورودی اول(سلکتور) مجموعه ای از عناصر باشد یا سلکتوری باشد که مجموعه ای از عناصر را برگرداند ، خروجی تابع `undefined` خواهد بود.

<br/>

#### مثال
```javascript
tedApi.asHtml("div");
```
خروجی : __undefined__

<br/>

#### مثال
```javascript
tedApi.asHtml("body");
```
خروجی : __[HTML ELEMENT CONTENT AS STRING]__

<br/>

#### مثال
```javascript
tedApi.asHtml("div#id","<span>span</span>");
```
خروجی : __&lt;span&gt;span&lt;/span&gt;__

<br/>

#### ساختار
```javascript
tedApi.asHtml(
 
    element : (HTMLElement , string) ,
    // optional
    content : string
      
) : string
```


### تابع html
تابع `html` به فرقیپ کوچکی دقیقا همانند تابع `asHtml` عمل می نماید. این تفاوت در این است که بر خلاف تابع `asHtml` ، این تابع مقدار `innerHTML` عنصر مورد نظر را بارمیگرداند


<br/>

#### مثال
```javascript
tedApi.html("div");
```
خروجی : __undefined__

<br/>

#### مثال
```javascript
tedApi.html("body");
```
خروجی : __[HTML ELEMENT CONTENT AS STRING]__

<br/>

#### مثال
```javascript
tedApi.html("div#id","<span>span</span>");
```
خروجی : __&lt;span&gt;span&lt;/span&gt;__

<br/>

#### ساختار
```javascript
tedApi.html(
 
    element : (HTMLElement , string) ,
    // optional
    content : string
      
) : string
```

### تابع text
عملکرید مشابه دو بتا `html`  و `asHtml` دارد با این تفاوت که ، این تابع تنها مقدار داخلی عناصر که textNode هستند را برمیگرداند


<br/>

#### مثال
```javascript
tedApi.text("div");
```
خروجی : __undefined__

<br/>

#### مثال
```javascript
tedApi.text("body");
```
خروجی : __[HTML ELEMENT CONTENT AS STRING]__

<br/>

#### مثال
```javascript
tedApi.text("div#id","sample text");
```
خروجی : __sample text__

<br/>

#### ساختار
```javascript
tedApi.text(
 
    element : (HTMLElement , string) ,
    // optional
    content : string
      
) : string
```



### تابع removeText
با اجرای این تابع بر روی عنصر خود ، می توان تمامی نود های متنی درون آن را حذف کرد.

!! نکته
! درصورتی که عملیات با موفقیت انجام شود مقدار 1 ، در غیر این صورت مقدار 0 بازمیگردد

<br/>

#### مثال
```javascript
tedApi.removeText("div#id");
```
خروجی : __1__

<br/>

#### مثال
```javascript
tedApi.removeText("div");
```
خروجی : __0__

<br/>

#### ساختار
```javascript
tedApi.removeText(
 
    targetElement : (HTMLElement , string) ,
      
) : number
```



## توابع شاخصه ها
مشخصه ها یا `Attributes` یکی از بخش های مهم در `html` می باشد که دیتاهای مختلفی را در خود ذخیره می کند .

 به صورتی می توان به عنوان رابطی داده ای بین `html` و `javascript` از آن ها استفاده کرد

### تابع attr

این تابع 3 فرایند مختلف را انجام می دهد.  ایجاد ، ویرایش و بازگرداندن تمامی مشخصه های عنصر مشخص شده.

#### مثال
```javascript
tedApi.attr("#container");
```
خروجی: __یک شی از تمامی مشخصه های عنصر را بازمیگرداند__

<br/>

#### مثال
```javascript
tedApi.attr("#container","class");
```
خروجی: __محتوای مشخصه `class` بازگردانده می شود__

<br/>

#### مثال
ویرایش یا ایجاد یک مشخصه
```javascript
tedApi.attr("#container","data-value","12");
```
خروجی: __مقدار 1 به عنوان موفقیت و 0 به عنوان عدم موفقیت__

<br/>

#### ساختار
```javascript
tedApi.attr(
 
    targetElement : (HTMLElement , string) ,
    //optional
    attribute_name : string,
    //optional
    attribute_value : any
      
) : number | object | string
```

!!نکته
! درصورتی که نود مشخص شده یک عنصر نباشد مقدار `-1` بازگردانده می شود


### تابع hasAttr

این تابع برای بررسی وجود یا عدم وجود یک مشخصه در یک عنصر استفاده می شود

#### مثال
```javascript
tedApi.hasAttr("#container","data-value");
```
خروجی: __به جدول زیر مراجعه نمایید__

<br/>

#### خروجی های تابع

مقدار خروجی | توضیح 
-----|---------------------------
1    |مشخصه در عنصر وجود __دارد__
0    |مشخصه در عنصر وجود __ندارد__
-1   |نام مشخصه به فرمت string نمی باشد
-2   |نام مشخصه undefined می باشد
-3   |نود ورودی عنصر نمی باشد
-4   |عنصر ورودی undefined می باشد

<br/>

#### ساختار
```javascript
tedApi.hasAttr(
 
    targetElement : (HTMLElement , string) ,

    attribute_name : string,
      
) : number
```

### تابع copyAttr

برخی موارد نیاز است در طراحی عناصر ، مشخصه های عنصر طراحی شده را بر روی عنصر جایگزینش داشته باشیم. از طریق این تابع می توان مشخصه های یک عنصر را بر روی عنصری دیگر کپی کرد

#### مثال
```javascript
tedApi.copyAttr("#container","#div");
```
<br/>

#### خروجی
درصورتی که خطایی در عنصر های ورودی یا عملیات کپی مشخصه ها رخ دهد ، مقدار __0__ و در غیر این صورت مقدار __1__ بازگردانده می شود

<br/>

#### ساختار
```javascript
tedApi.copyAttr(
 
    fromElement : (HTMLElement , string) ,

    toElement : (HTMLElement , string),
      
) : number
```

### تابع removeAttr
تابع `removeAttr` ، مشخصه ها را از عناصر حذف می نماید

!! نکته
! درصورت موفقیت حذف ، مقدار `1` و در غیر این صورت مقدار `0` را بازمیگرداند .


#### مثال
```javascript
tedApi.removeAttr(".elements","data-id");
```
<br/>

#### ساختار
```javascript
tedApi.removeAttr(
 
    elements  : (HTMLElement , string , Array<HTMLElement>[]) ,

    attribute : string,
      
) : number
```

## توابع کار با css

### تابع cssSelect

درصورت نیاز به گرفتن مقدار یکی مشخصه های استایل یا شبه سلکتور یا شبه سلکتور های استایل یک عنصر ، می توان از این تابع استفاده کرد


#### مثال
```javascript
tedApi.cssSelect( tedApi.elm("body") ,"width");
```
<br/>


#### مثال
```javascript
tedApi.cssSelect("#element","content",":before");
```
<br/>

#### ساختار
```javascript
tedApi.cssSelect(
 
    elements  : (HTMLElement , string) ,

    index : string,
    //optional
    pseudo : string
      
) : string
```

### تابع cssEdit

از طریق این تابع می توانید استایل های عناصر را ویرایش کنید یا درصورت عدم وجود ایجاد نمایید.

!! نکته
! درصورت موفقیت ، مقدار `1` و در غیر این صورت مقدار `0` را بازمیگرداند .

#### مثال
```javascript
tedApi.cssEdit( tedApi.elm("#id") ,"width" , "10px");
```
<br/>

#### ساختار
```javascript
tedApi.cssEdit(
 
    elements : HTMLElement ,

    index : string ,

    value : string
      
) : number
```

### تابع cssEditFunc

درصورتی که نیاز داشته باشید مقادیر استایل را به صورت داینامیک تغییر دهید می توان از توابع برای اعمال تغییر استفاده نمود.

!! نکته
! مقدار خروجی تابع درصورتی که ورودی ها اشتباه باشند ، `-1` می باشد در غیر این صورت ، درصورتی که عملیات با موفقیت انجام شود `1` و در غیر این صورت مقدار `0` بازگشت داده می شود.

<br/>

!!! توجه
! تابع ورودی یک آرگیومنت به عنوان محتوای مشخصه دارد و مقدار بازگشتی تابع نیز به عنوان مقدار جدید ثبت خواهد شد

#### مثال
```javascript
tedApi.cssEditFunc( tedApi.elm("#id") ,"width" , function(val){
    return val/2;
});
```
<br/>

#### ساختار
```javascript
tedApi.cssEditFunc(
 
    elements : HTMLElement ,

    index : string ,

    callback : function
      
) : number
```

### تابع css

تابع `css` یک مجموعه از توابع بالاست که تمامی موارد را در یک تابع قرار داده است. این تابع ایجاد ، ویرایش و سلکت را دربر دارد.

! مزیت
! برخلاف توابع بالا که تنها یک عنصر را دریافت می کرده اند، این تابع می تواند عملیات های ذکر شده را بر روی آرایه ای از عناصر نیز اعمال کند

<br/>

!!! توجه
! خروجی این تابع بر اساس نوع استافده از آن ، خروجی یکی از توابع بالا می باشد.

<br/>

#### عملیات انتخاب

##### مثال
```javascript
tedApi.css( tedApi.elm("body") , "width" );// return string
```
<br/>

##### مثال
```javascript
tedApi.css( "div" , "width" ); // return [string...] if multiple divs are exist
```
<br/>

#### عملیات ویرایش یا اضافه کردن

##### مثال
```javascript
tedApi.css( tedApi.elm("body") , "width" , "500px" );
```
<br/>

##### مثال
```javascript
tedApi.css( "div" , {
    width:"500px"
});
```
<br/>

#### عملیات ویرایش با تابع

##### مثال
```javascript
tedApi.css( tedApi.elm("body") , "width" , function(val){
    return val/2;
});
```
<br/>

##### مثال
```javascript
tedApi.css( "div" , {
    width:function(val){
        return val/2;
    }
});
```
<br/>


#### ساختار
```javascript
tedApi.css(
 
    elements : (HTMLElement , string) ,

    index : (string , object) ,

    value : (string , function)
      
) : number | undefined | string
```


### تابع show و hide

این دو تابع برای نمایش یا مخفی کردن یک عنصر به کار می روند

!!نکته
! این دو تابع دارای دو آرگیومنت ورودی می باشند که آرگیومنت اول ، عنصر مورد نظر و آرگیومنت دوم درصد وضوح بسته به تابع می باشد.
!
! به عنوان مثال در تابع show ، درصد وضوح هرچه به `1` نزدیک تر باشد ، عنصر واضح تر است
!
! در تابع hide بر خلاف تابع show ، هرچه درصد وضوح به `1` نزدیک تر باشد ، وضوح عنصر پایین تر می آید
!
! بازه وضوح بین `0` و `1` می باشد

<br/>



##### مثال
```javascript
tedApi.[show or hide]( "#content" , "hide");

tedApi.[show or hide]( "#content" , "show");

tedApi.[show or hide]( "#content" , 0.5);
```
<br/>

#### ساختار تابع show
```javascript
tedApi.show(
 
    elements : (HTMLElement , string) ,
    //optional , default: show
    opacity : (string , number) ,
      
) : number
```


#### ساختار تابع hide
```javascript
tedApi.hide(
 
    elements : (HTMLElement , string) ,
    //optional , default: hide
    opacity : (string , number) ,
      
) : number
```

!!! توجه
! خروجی توابع درصورت موفقیت `1` و در غیر این صورت `0` می باشد

<br/>

!!نکته
! مقادیر `hide` و `show` به عنوان وضوح ، `display` عنصر را به ترتیب `none` و `block` قرار می دهد