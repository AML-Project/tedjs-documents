## مقدمه
نود های متنی پس از عناصر ، پرکاربردترین نود ها در html می باشد. 

این نود ها دارای نشانه خاصی نمی باشند ، تنها متون طبیعی (plain text) ها هستند که به صورت عادی در بین عناصر و دیگر نود ها نوشته می شوند. 

موتور رندر HTML ، متون عادی را به صورت مجزا در یک نود در DOM ذخیره می نماید.

## نود متنی در تد جی اس
در تد جی اس ، نود های متنی همانند نود های توضیحات پردازش می گردند و از طریق پترن قابل شناسایی و انتخاب هستند. تد جی اس با در اختیار قرار دادن امکانات متفاوتی ، روند تغییر و یا بازیابی اطلاعات از نود های متنی را تسهیل کرده است تا بتوان کنترل دو چندان بر کد های html داشت. 

از جمله کاربرد های نود های متنی می توان به ایجاد تمپلیت ها برای اجرای دستوراتی چون حلقه ها ، شرت ها ، ... و یا بزایابی اطلاعات از متغیر ها و نمایش آن ها ، تغییر اعداد انگلیسی به فارسی بدون استفاده از عناصر اضافه و ... اشاره نمود

## ایجاد نود متنی
به منظور ایجاد یک نود متنی می بایست از تابع `createTextNode` استفاده نمایید. این تابع می بایست درون کتابخانه استفاده گردد. این تابع یک الگو از نوع Regular Expression چه با فرمت string و چه با فرمت RegExp دریافت می نماید. در هر یک از نودهای متنی که این الگو یافت شود ، پردازش گر آن را اجرا می نماید.

<br/>

! مقدار بازگشتی
! درصورتی که عملیات با خطایی روبرو گردد (ورودی هایی با انواع نادرست) مقدار `-1` بازگشت داده می شود و در غیر این صورت فضای نام ted بازگشت می گردد.

<br/>

#### ساختار تابع
```javascript
ted.createTextNode(

    text_algo : string | RegExp ,

    callback : function( matches : string ... )

) : -1 | ted
```

!! نکته
! مقدار `this` در تابع فراخوان ، به نود متنی جاری اشاره دارد

<br/>

!!! توجه
! در هر نود متنی می توان چندین الگو از نود های متنی طراحی شده استفاده نمود و هر textNode محدود به یک نودمتنی طراحی شده نمی باشد

<br/>

### تابع between
تابع `between` ، یک تابع از فضای نام `tedApi` می باشد که وظیفه دارد نود های بین دو نود start و end را مشخص کرده و برگرداند.

<br/>

#### ساختار
```javascript
tedApi.between(

    start : Node | string ,

    end : Node | string

) : Array<Node> | null
```
! مقدار بازگشتی
! مقدار بازگشتی تابع ، آرایه ای از نود ها می باشد . در صورت عدم موفقیت و یا وجود خطا ، مقدار null بازگشت داده می شود.

<br/>

### تابع splitTextNode
تابع `splitTextNode` وظیفه دارد تا نود متنی مشخصی را به وسیله یک جدا کننده به سه قسمت تقسیم نماید. نود قبل از جدا کننده ، نود جدا کننده و نود پس از جداکننده.

<br/>

#### ساختار
```javascript
tedApi.splitTextNode(

    textNode : TextNode ,

    seperator : string

) : boolean
```
! مقدار بازگشتی
! درصورت موفقیت `true` و در غیر این صورت `false` بازگشت داده می شود.

<br/>

### مثال
ایجاد تمپلیت حلقه برای تکرار عناصر

<br/>

##### جاوااسکریپت :
طراحی نود `@for` . روند اجرا اینگونه خواهد بود که حلقه `for` دارای یک نود انتهایی به صورت `@endfor` می باشد که می توان محتوای درونش را تشخیص داد.

<br/>

###### :: فضای ذخیره سازی :
```javascript
var ForStorage = {
    datas:[]
}
```


<br/>

###### :: طراحی فرمان مقدار دهی اولیه :
```javascript
ted.createTextNode( 
    /\@for\s+([A-Za-z_][A-Za-z_$0-9]*)\s+from\s+(\d+)\s+to\s+(\d+)\s+step\s+(\d+)/ ,
    function(whole, variable, from, to, step){

        tedApi.splitTextNode( this , whole );

        ForStorage[ "for" ] = this;

        ForStorage[ "from" ] = parseInt( from );

        ForStorage[ "var" ] = variable;
        ForStorage[ "to" ] = parseInt( to );
        ForStorage[ "step" ] = parseInt( step );
    }
);
```


<br/>

###### :: طراحی بخش دسترسی به متغیر ها و داده حلقه :
```javascript
ted.createTextNode(
    /\@F\{(.+?)\}/ ,
    function(whole, data){
        tedApi.splitTextNode( this , whole );

        var pattern = new RegExp("([^A-Za-z0-9]+|^)("+ForStorage["var"]+")([^A-Za-z0-9]+|$)");

        while(pattern.test(data)){
            data = data.replace(pattern,function(w,m1,m2,m3){
                return m1+ "this" +m3;
            });
        }

        ForStorage["datas"].push({
            node : new RegExp(whole.replace(/(\W)/g,"\\$&"),"g"),
            text : data
        });
    }
);
```


###### :: طراحی فرمان انتهای حلقه :
```javascript
ted.createTextNode(
    /\@endfor/ ,
    function(whole, data){
        
        if(tedApi.isUndefined(ForStorage["for"])){
            return;
        }

        var 
            _temp = tedApi.elm("<div></div>"),
            _temp2 = tedApi.elm("<div></div>"),
            replace = tedApi.elm("<span style='display:none'></span>"),
            tmpReplace = replace,
            i,tempStr = "",sons;

        tedApi.splitTextNode( this , whole ); 

        var between = tedApi.between(ForStorage["for"],this);

        for(i=0;i < between.length;i++){
            tedApi.append(_temp,between[i]);
        }

        //have refrence to that place
        tedApi.insert(replace).after(ForStorage["for"]);

        _temp = _temp.innerHTML;

        tedApi.delete([this,ForStorage["for"]]);

        for(;ForStorage.from < ForStorage.to; ForStorage.from+=ForStorage.step ){
            tempStr = _temp;
            for(i=0;i < ForStorage.datas.length;i++){
                tempStr = tempStr.replace( 
                    ForStorage.datas[i].node , 
                    tedApi.run.call(ForStorage.from,"return "+ForStorage.datas[i].text) 
                );
            }
            tedApi.html(_temp2,tempStr);
            sons = _temp2.childNodes;
            while(sons.length){
                var Temp_ = sons[0];
                tedApi.insert(sons[0]).after(tmpReplace);
                tmpReplace = Temp_;
            }
        }

        tedApi.delete(replace);

        ForStorage = {
            datas:[]
        }
        
    }
);
```

<br/>


! پیاده سازی
! پیاده سازی تابع `Factorial` :
! ```javascript
!/**
! * @arg {number} n
! */
!var f = [];
!function Factorial(n) {
!  if (n == 0 || n == 1)
!    return 1;
!  if (f[n] > 0)
!    return f[n];
!  return f[n] = Factorial(n-1) * n;
!}
! ```

<br/>


###### HTML :
استفاده از نود
```html
<div>
    @for i from 0 to 10 step 2
        <p>@F{i}! = @F{Factorial(i)} </p>
    @endfor
</div>
```

<br/>

### نمونه قابل اجرا


مثال بالا به صورت زنده


@code(width:100%,height:400px)

```html
<div>
    @for i from 0 to 10 step 2
        <p>@F{i}! = @F{Factorial(i)} </p>
    @endfor
</div>
```ext{js}(https://cdn.tedjs.org/tedjs/1.2.2/js/2ed.min.js?675)
```ext{js}[other]
var f = [];
function Factorial(n) {
  if (n == 0 || n == 1)
    return 1;
  if (f[n] > 0)
    return f[n];
  return f[n] = Factorial(n-1) * n;
}
```js
tedApi.export.Attr = function(ted){
    var ForStorage = {
        datas:[]
    }

    ted.createTextNode( 
        /\@for\s+([A-Za-z_][A-Za-z_$0-9]*)\s+from\s+(\d+)\s+to\s+(\d+)\s+step\s+(\d+)/ ,
        function(whole, variable, from, to, step){

            tedApi.splitTextNode( this , whole );

            ForStorage[ "for" ] = this;

            ForStorage[ "from" ] = parseInt( from );

            ForStorage[ "var" ] = variable;
            ForStorage[ "to" ] = parseInt( to );
            ForStorage[ "step" ] = parseInt( step );
        }
    );

    ted.createTextNode(
        /\@F\{(.+?)\}/ ,
        function(whole, data){
            tedApi.splitTextNode( this , whole );

            var pattern = new RegExp("([^A-Za-z0-9]+|^)("+ForStorage["var"]+")([^A-Za-z0-9]+|$)");

            while(pattern.test(data)){
                data = data.replace(pattern,function(w,m1,m2,m3){
                    return m1+ "this" +m3;
                });
            }

            ForStorage["datas"].push({
                node : new RegExp(whole.replace(/(\W)/g,"\\$&"),"g"),
                text : data
            });
        }
    );

    ted.createTextNode(
        /\@endfor/ ,
        function(whole, data){
            
            if(tedApi.isUndefined(ForStorage["for"])){
                return;
            }

            var 
                _temp = tedApi.elm("<div></div>"),
                _temp2 = tedApi.elm("<div></div>"),
                replace = tedApi.elm("<span style='display:none'></span>"),
                tmpReplace = replace,
                i,tempStr = "",sons;

            tedApi.splitTextNode( this , whole ); 

            var between = tedApi.between(ForStorage["for"],this);

            for(i=0;i < between.length;i++){
                tedApi.append(_temp,between[i]);
            }

            //have refrence to that place
            tedApi.insert(replace).after(ForStorage["for"]);

            _temp = _temp.innerHTML;

            tedApi.delete([this,ForStorage["for"]]);

            for(;ForStorage.from < ForStorage.to; ForStorage.from+=ForStorage.step ){
                tempStr = _temp;
                for(i=0;i < ForStorage.datas.length;i++){
                    tempStr = tempStr.replace( 
                        ForStorage.datas[i].node , 
                        tedApi.run.call(ForStorage.from,"return "+ForStorage.datas[i].text) 
                    );
                }
                tedApi.html(_temp2,tempStr);
                sons = _temp2.childNodes;
                while(sons.length){
                    var Temp_ = sons[0];
                    tedApi.insert(sons[0]).after(tmpReplace);
                    tmpReplace = Temp_;
                }
            }

            tedApi.delete(replace);

            ForStorage = {
                datas:[]
            }
            
        }
    );
}
```css
div{
    color:#fff
}

@endcode