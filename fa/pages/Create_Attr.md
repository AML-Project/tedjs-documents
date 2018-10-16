## مقدمه

مشخصه ها (Attributes) در عناصر به عنوان داده های جانبی و توصیف گر های آن عنصر عمل می نمایند. هر مشخصه دارای یک نام و یک مقدار می باشد که در قالب string ذخیره می گردد. 

مشخصه در هر یک از عناصر ، به صورت پیش فرض تعریف شده اند تا در هنگام رندر صفحه از آن داده ها درصورت وجود استفاده شود. 

حال می توان از طریق tedjs ، مشخصه های شخصی سازی شده ای را نیز طراحی کرد که پس از اتمام رندرینگ اصلی صفحه ، از طریق پردازش فریم ورک برای عنصر یا عناصر مورد نظر با توجه به عملکرد توصیف شده اجرا گردد.

## مشخصه ها در تد جی اس

مشخصه ها در تد جی اس از طریق نامشان شناسایی می شوند . به دان معنی که نام مشخصه ها می بایست یکتا باشد و همزمان چند مشخصه با یک نمام نمی توان ایجاد کرد.

هر مشخصه علاوه بر نام ، تابع فراخوان و یک آرایه از نام تگ ها (tag name) را دربر دارد.

نام تگ ها برای مشخص نمودن قابلیت اجرای مشخصه بر روی عناصر خاص می باشد. درصورت خالی بودن آرایه ، محدودیتی اعمال نمی گردد.

## ایجاد مشخصه

به منظور ایجاد یک مشخصه تنها کافیست ، تابع 
`createAttr`
 را به مانند زیر درون کتابخانه خود ایجاد نمایید:

<br/>

! مقدار بازگشتی
! درصورتی که عملیات با خطایی روبرو گردد (ورودی هایی با انواع نادرست) مقدار `-1` بازگشت داده می شود و در غیر این صورت فضای نام ted بازگشت می گردد.

<br/>


#### ساختار تابع
```javascript
ted.createAttr(

    attr_name : string ,

    callback : function( value : string ) ,

    tags_filter : Array<string>

) : -1 | ted
```

!! نکته
! مقدار `this` ، در تابع فراخوان یک نمونه از `__GLOBALS__` می باشد که به عنصر جاری اشاره دارد

<br/>

!!! توجه
! مقدار `value` در تابع فراخوان ، مقدار مشخصه می باشد


### مثال

طراحی نمایش امتیاز به صورت ستاره :

<br/>

###### جاوا اسکریپت : 
 مقدار مشخصه می بایست عدد بین 0 تا 100 باشد . عدد به صورت ستاره 5 تا نمایش داده خواهد شد

```javascript
ted.createAttr( "rate-star" , function( value ){

    var
        emptyStar = "&#9734;",
        fillStar = "&#9733;",
        star_count = 5,
        stars = "",
        i = 0;

    if( !tedApi.isNumber(value) ){
        value = 0;
    }
    else{
        value = parseInt( value );
    }

    value = tedApi.scaleNumber(
        {min:0,max:100},//from
        {min:0,max:star_count},//to
        value
    );

    stars = "";

    for( i = 0 ; i < star_count ; i++ ){
        if( i + 1 <= value ){
            stars += fillStar;
        }
        else{
            stars += emptyStar;
        }
    }

    this.html( stars );

} , ["span"] );
```


###### html : 
استفاده از مشخصه

```html
<p>
    امتیاز : <span rate-star="56"></span>
</p>
```

### نمونه قابل اجرا

مثال بالا به صورت زنده


@code(width:100%,height:400px)

```html
<p>
    امتیاز : <span rate-star="56"></span>
</p>
```ext{js}(https://cdn.tedjs.org/tedjs/1.2.2/js/2ed.min.js?675)
```js
tedApi.export.Attr = function(ted){
    ted.createAttr( "rate-star" , function( value ){

        var
            emptyStar = "9734",
            fillStar = "9733",
            star_count = 5,
            stars = "",
            i = 0;

        if( !tedApi.isNumber(value) ){
            value = 0;
        }
        else{
            value = parseInt( value );
        }

        value = tedApi.scaleNumber(
            {min:0,max:100},//from
            {min:0,max:star_count},//to
            value
        );

        stars = "";

        for( i = 0 ; i < star_count ; i++ ){
            if( i + 1 <= value ){
                stars += "&#" + fillStar + ";";
            }
            else{
                stars += "&#" + emptyStar + ";";
            }
        }

        this.html( stars );

    } , ["span"] );
}


```css
p{
    padding: 10px;
    margin: 30px;
    box-sizing: border-box;
    color: #4c4c4c;
    font-family: Vazir;
    direction: rtl;
    text-align: right;
    font-size: 18px;
    background: #fff;
    border-radius: 3px;
}

span{
    color: #9C27B0;
    font-size: 26px;
}

@endcode