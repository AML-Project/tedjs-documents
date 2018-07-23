## توضیحات

توابع صحت سنجی ، از توابع زیر مجموعه شی `tedApi` می باشند که وظیفه بررسی صحت انواع داده مانند متون ، اشیاء ، آرایه ها و ... را بر عهده دارند.

این توابع یک تا چند ورودی را دریافت کرده و اگر تمامی ورودی ها از نظر صحبت توسط تابع مورد نظر تصدیق شوند ، مقدار `true` بازگردانده و در غیر این صورت مقدار `false` بازگردانده می شود.

به لحاظ منطقی صحبت ورودی ها به صورت عملیات منطقی `and` با یکدیگر مقایسه می شوند

## تابع isAlpha

از طریق این تابع می توان تشخیص داد ، ورودی تنها از الفبا تشکیل شده یا خیر. 
> الفبا در اینجا به معنی حروف زیر می باشد
>
> حروف A تا Z و _ و $


#### مثال ها 

مثال 1
```javascript
tedApi.isAlpha("$test_")
// return true
```


مثال 2
```javascript
tedApi.isAlpha("test","$","_","a");  
//return true
```


مثال 3
```javascript
tedApi.isAlpha("test","$","_","a","");  
//return false
```


مثال 4
```javascript
tedApi.isAlpha("test","$","_","a",[],"g");  
//return false
```


مثال 5
```javascript
tedApi.isAlpha();  
//return false
```

#### ساختار
```javascript
tedApi.isAlpha(
 
    arg1 : string ,
    // optional
    arg2 : string ,
    ...
    // optional
    argn : string  
      
) : boolean
```



## تابع isPersianAlpha

این تابع نیز مانند تابع بالا برای تشخیص متون آلفابتیک از باقی متون می باشد. با این تفاوت که این تابع بر خلاف تابع بالا زبان فارسی-عربی را شناسایی می نماید
> این تابع برای تشخصی متون در رینج کد های یونی کد زیر بررسی می نماید
> و از فضای خالی و ... پشتیبانی نمی کند
>
> 0600 تا  06FF

#### مثال ها 

مثال 1
```javascript
tedApi.isPersianAlpha("سلامـبرـشما");
// return true
```


مثال 2
```javascript
tedApi.isPersianAlpha("سلام","تد جی اس");  
//return false
```

مثال 3
```javascript
tedApi.isPersianAlpha();  
//return false
```

#### ساختار
```javascript
tedApi.isPersianAlpha(
 
    arg1 : string ,
    // optional
    arg2 : string ,
    ...
    // optional
    argn : string  
      
) : boolean
```



## تابع isNumber

بررسی عدد بودن ورودی ها چه به صورت رقم و چه به صورت متنی


#### مثال ها 

مثال 1
```javascript
tedApi.isNumber(10.2,"0.0005");
// return true
```


مثال 2
```javascript
tedApi.isNumber(2.5,55.2);  
//return true
```


مثال 3
```javascript
tedApi.isNumber(0,"a");  
//return false
```


مثال 4
```javascript
tedApi.isNumber(0);  
//return false
```


مثال 5
```javascript
tedApi.isNumber();  
//return false
```

#### ساختار
```javascript
tedApi.isNumber(
 
    arg1 : (string , number) ,
    // optional
    arg2 : (string , number) ,
    ...
    // optional
    argn : (string , number)  
      
) : boolean
```



## تابع isPersianNumber

این تابع مشابه تابع بالا عمل می کند با این تفاوت که اعداد فارسی-عربی را شناسایی می نماید.


#### مثال ها 

مثال 1
```javascript
tedApi.isPersianNumber("۰.۰۰۰۰۵")
// return false
```


مثال 2
```javascript
tedApi.isPersianNumber("۱۲۳۴۵۸۷")
//return true
```

#### ساختار
```javascript
tedApi.isPersianNumber(
 
    arg1 : string ,
    // optional
    arg2 : string ,
    ...
    // optional
    argn : string  
      
) : boolean
```


## تابع isInt

ورودی ها را برای تشخیص عدد `integer` می سنجد.


#### مثال ها 

مثال 1
```javascript
tedApi.isInt(22,"45");  
//return true
```


مثال 2
```javascript
tedApi.isInt(1005,0.55);  
//return false
```


مثال 3
```javascript
tedApi.isInt("b");  
//return false
```


مثال 5
```javascript
tedApi.isInt();  
//return false
```

#### ساختار
```javascript
tedApi.isInt(
 
    arg1 : (string , number) ,
    // optional
    arg2 : (string , number) ,
    ...
    // optional
    argn : (string , number)  
      
) : boolean
```


## تابع isInt

ورودی ها را برای تشخیص عدد `float point` می سنجد.


#### مثال ها 

مثال 1
```javascript
tedApi.isFloat(10.2,"0.0005");  
//return true
```


مثال 2
```javascript
tedApi.isFloat(0,55.2);  
//return false
```


مثال 3
```javascript
tedApi.isFloat("1","8");  
//return false
```


#### ساختار
```javascript
tedApi.isFloat(
 
    arg1 : (string , number) ,
    // optional
    arg2 : (string , number) ,
    ...
    // optional
    argn : (string , number)  
      
) : boolean
```


## تابع isJustArray

تابعی برای بررسی نوع داده آرایه از نوع `[Object Array]`


#### مثال ها 

مثال 1
```javascript
tedApi.isJustArray([]);  
//return true
```


مثال 2
```javascript
tedApi.isJustArray([1,2,""],[function(){},{}]);  
//return true
```


مثال 3
```javascript
tedApi.isJustArray([], [Object NodeList] );  
//return false
```

مثال 4
```javascript
tedApi.isJustArray({});  
//return false
```


#### ساختار
```javascript
tedApi.isJustArray(
 
    arg1 : Array  ,
    // optional
    arg2 : Array  ,
    ...
    // optional
    argn : Array  
      
) : boolean
```



## تابع isNodeList

تابعی برای بررسی نوع داده آرایه از نوع `[Object NodeList]`


#### مثال ها 

مثال 1
```javascript
tedApi.isNodeList( document.querySelectorAll("div") );  
//return true
```


مثال 2
```javascript
tedApi.isNodeList("",[34,[]],3);  
//return false
```


مثال 3
```javascript
tedApi.isNodeList( tedApi.elm("div") );  
//return true
```

مثال 4
```javascript
tedApi.isNodeList( tedApi.elm("#someID") );  
//return false
```


#### ساختار
```javascript
tedApi.isJustArray(
 
    arg1 : NodeList  ,
    // optional
    arg2 : NodeList  ,
    ...
    // optional
    argn : NodeList  
      
) : boolean
```


## تابع isArray

این تابع یک مجموعه از دو تابع بالا می باشد که دو نوع آرایه و نود لیست را بررسی می نماید


#### مثال ها 

مثال 1
```javascript
tedApi.isArray([1,2,3, [Object NodeList] ]);  
//return true
```


مثال 2
```javascript
tedApi.isArray( [Object NodeList] );  
//return true
```


مثال 3
```javascript
tedApi.isArray([Object NodeList],[{},1,2,"test"]);  
//return true
```

مثال 4
```javascript
tedApi.isArray({});  
//return false
```


#### ساختار
```javascript
tedApi.isArray(
 
    arg1 : Array  ,
    // optional
    arg2 : Array  ,
    ...
    // optional
    argn : Array  
      
) : boolean
```


## تابع isString

تابع `isString` نوع داده متنی را جدای از حروف داخلش بررسی می نماید


#### مثال ها 

مثال 1
```javascript
tedApi.isString([].toString());  
//return true
```


مثال 2
```javascript
tedApi.isString({});  
//return false
```


مثال 3
```javascript
tedApi.isString("a","b","c","سلام");  
//return true
```

مثال 4
```javascript
tedApi.isString("a",{},[]);  
//return false
```


#### ساختار
```javascript
tedApi.isString(
 
    arg1 : string  ,
    // optional
    arg2 : string  ,
    ...
    // optional
    argn : string  
      
) : boolean
```



## تابع isObject

بررسی نوع داده شی (object) یا `[Object object]`


#### مثال ها 

مثال 1
```javascript
tedApi.isObject([]);  
//return false
```


مثال 2
```javascript
tedApi.isObject({});  
//return true
```


مثال 3
```javascript
tedApi.isObject("a",{},{});  
//return false
```

مثال 4
```javascript
tedApi.isObject(null);  
//return false
```


#### ساختار
```javascript
tedApi.isObject(
 
    arg1 : object  ,
    // optional
    arg2 : object  ,
    ...
    // optional
    argn : object  
      
) : boolean
```



## تابع isUndefined

وظیفه دارد تا بررسی کند ، ورودی هایش تماما `undefined` باشند


#### مثال ها 

مثال 1
```javascript
tedApi.isUndefined(undefined);  
//return true
```


مثال 2
```javascript
var UnsetVariable;

tedApi.isUndefined(UnsetVariable);  
//return true
```


مثال 3
```javascript
tedApi.isUndefined("a",{},{});  
//return false
```

مثال 4
```javascript
tedApi.isUndefined(null);  
//return false
```


#### ساختار
```javascript
tedApi.isUndefined(
 
    arg1 : any  ,
    // optional
    arg2 : any  ,
    ...
    // optional
    argn : any  
      
) : boolean
```



## تابع isFunction

بررسی می کند ورودی هایش تابع باشند از نوع `Function`


#### مثال ها 

مثال 1
```javascript
tedApi.isFunction(function(){});  
//return true
```


مثال 2
```javascript
tedApi.isFunction( (function(){return 1;})()  );  
//return false
```


مثال 3
```javascript
tedApi.isFunction(1,"asd",[]);  
//return false
```

مثال 4
```javascript
tedApi.isFunction( tedApi.isFunction );  
//return true
```


#### ساختار
```javascript
tedApi.isFunction(
 
    arg1 : Function  ,
    // optional
    arg2 : Function  ,
    ...
    // optional
    argn : Function  
      
) : boolean
```



## تابع isNull

بررسی می کند ورودی ها همگی `null` باشند


#### مثال ها 

مثال 1
```javascript
tedApi.isNull({});  
//return false
```


مثال 2
```javascript
tedApi.isNull(null);  
//return true
```


مثال 3
```javascript
tedApi.isNull([],null,{});  
//return false
```

مثال 4
```javascript
tedApi.isNull( (function(){return null;})() );  
//return true
```


#### ساختار
```javascript
tedApi.isNull(
 
    arg1 : any  ,
    // optional
    arg2 : any  ,
    ...
    // optional
    argn : any  
      
) : boolean
```


## تابع isElement

بررسی می کند ورودی ها همگی `[Object HTMLElement]` باشند


#### مثال ها 

مثال 1
```javascript
tedApi.isElement([]);  
//return false
```


مثال 2
```javascript
tedApi.isElement(null, tedApi.elm("body") );  
//return false
```


مثال 3
```javascript
tedApi.isElement( tedApi.elm("div#id") , tedApi.elm("body") );  
//return true
```

مثال 4
```javascript
tedApi.isElement(1,"a",null,{});  
//return false
```


#### ساختار
```javascript
tedApi.isElement(
 
    arg1 : HTMLElement  ,
    // optional
    arg2 : HTMLElement  ,
    ...
    // optional
    argn : HTMLElement   
      
) : boolean
```


## تابع isTag

بررسی می کند ورودی ها همگی `[Object HTMLElement]` باشند و همچنین به صورت بلاکی نبوده و یک `tag`داشته باشد


#### مثال ها 

مثال 1
```javascript
tedApi.isTag({},[]);  
//return true
```


مثال 2
```javascript
tedApi.isTag( tedApi.elm("img#id") );  
//return true
```


مثال 3
```javascript
tedApi.isTag( tedApi.elm("img") );  
//return false
```

مثال 4
```javascript
tedApi.isTag( tedApi.elm("div#id") );  
//return false
```


#### ساختار
```javascript
tedApi.isTag(
 
    arg1 : HTMLElement  ,
    // optional
    arg2 : HTMLElement  ,
    ...
    // optional
    argn : HTMLElement   
      
) : boolean
```


## تابع isTag

بررسی می کند ورودی ها همگی `[Object HTMLElement]` باشند و همچنین به صورت بلاکی نبوده و یک `tag`داشته باشد


#### مثال ها 

مثال 1
```javascript
tedApi.isTag({},[]);  
//return true
```


مثال 2
```javascript
tedApi.isTag( tedApi.elm("img#id") );  
//return true
```


مثال 3
```javascript
tedApi.isTag( tedApi.elm("img") );  
//return false
```

مثال 4
```javascript
tedApi.isTag( tedApi.elm("div#id") );  
//return false
```


#### ساختار
```javascript
tedApi.isTag(
 
    arg1 : HTMLElement  ,
    // optional
    arg2 : HTMLElement  ,
    ...
    // optional
    argn : HTMLElement   
      
) : boolean
```


## تابع isTextNode

بررسی می کند داده های ورودی از نوع `[Object TextNode]` باشند


#### مثال ها 

مثال 1
```javascript
tedApi.isTextNode( tedApi.children("head")[0] );   // if tedApi.children("head")[0] be a textNode
//return true
```


#### ساختار
```javascript
tedApi.isTextNode(
 
    arg1 : TexNode  ,
    // optional
    arg2 : TexNode  ,
    ...
    // optional
    argn : TexNode   
      
) : boolean
```


## تابع isCommentNode

بررسی می کند داده های ورودی از نوع `[Object CommentNode]` باشند


#### مثال ها 

مثال 1
```javascript
tedApi.isCommentNode( tedApi.children("head")[0] );   // if tedApi.children("head")[0] be a CommentNode
//return true
```


#### ساختار
```javascript
tedApi.isCommentNode(
 
    arg1 : CommentNode  ,
    // optional
    arg2 : CommentNode  ,
    ...
    // optional
    argn : CommentNode   
      
) : boolean
```




## تابع isNode

بررسی می کند ورودی ها همگی نود های HTML باشند


#### مثال ها 

مثال 1
```javascript
tedApi.isNode([]);  
//return false
```


مثال 2
```javascript
tedApi.isNode( tedApi.elm("img#id") );  
//return true
```


مثال 3
```javascript
tedApi.isNode( tedApi.elm("div#id") , tedApi.elm("body") );  
//return true
```

مثال 4
```javascript
tedApi.isNode( tedApi.elm("div") );  
//return false
```


#### ساختار
```javascript
tedApi.isNode(
 
    arg1 : HTMLNode  ,
    // optional
    arg2 : HTMLNode  ,
    ...
    // optional
    argn : HTMLNode   
      
) : boolean
```


## تابع isBool

بررسی می کند مقادیر ورودی از نوع داده `boolean` باشد


#### مثال ها 

مثال 1
```javascript
tedApi.isBool(true,false);  
//return true
```


مثال 2
```javascript
tedApi.isBool(!0);  
//return true
```


مثال 3
```javascript
tedApi.isBool(1,"",[],{});  
//return false
```

مثال 4
```javascript
tedApi.isBool( null , function(){} , true  );  
//return false
```

مثال 5
```javascript
tedApi.isBool(!"a");  
//return true
```


#### ساختار
```javascript
tedApi.isBool(
 
    arg1 : boolean  ,
    // optional
    arg2 : boolean  ,
    ...
    // optional
    argn : boolean   
      
) : boolean
```


## تابع isUrl

بررسی می کند فرمت متون ورودی تابع ، آدرس اینترنتی باشند


#### مثال ها 

مثال 1
```javascript
tedApi.isUrl("http://localhost");  
//return true
```


مثال 2
```javascript
tedApi.isUrl("ftp://localhost");  
//return true
```


مثال 3
```javascript
tedApi.isUrl("https://domain.com/page/file.format");  
//return true
```

مثال 4
```javascript
tedApi.isUrl("www.domain.com");  
//return true
```

مثال 5
```javascript
tedApi.isUrl("goog..le.com");  
//return false
```


#### ساختار
```javascript
tedApi.isUrl(
 
    arg1 : string  ,
    // optional
    arg2 : string  ,
    ...
    // optional
    argn : string   
      
) : boolean
```


## تابع isEmail

بررسی می کند ريا، متون ورودی تابع با فرمت آدرس ایمیل وارد شده باشد


#### مثال ها 

مثال 1
```javascript
tedApi.isEmail("name@domain.com");  
//return true
```


مثال 2
```javascript
tedApi.isEmail("na*me@domain.com");  
//return false
```


مثال 3
```javascript
tedApi.isEmail("name@domain.co.com");  
//return true
```

مثال 4
```javascript
tedApi.isEmail("n@ame@domain.com");  
//return false
```

مثال 5
```javascript
tedApi.isEmail("name@domain");  
//return false
```


#### ساختار
```javascript
tedApi.isEmail(
 
    arg1 : string  ,
    // optional
    arg2 : string  ,
    ...
    // optional
    argn : string   
      
) : boolean
```