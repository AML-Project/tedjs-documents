## مقدمه

عناصر در html رکن اساسی زبان می باشند. از طریق عناصر می توان ساختار وب خود را ایجاد نمود و به آن سبک و استیل اضافه کرد.

هر عنصر متشکل از یک نام تگ (tag name) ، تعدادی مشخصه (Attributes) و فرزندان درونی که متشکل از نود های مختلف دیگر می باشد.

## عناصر در تد جی اس

تد جی اس این امکان را می دهد تا بتوان عناصری شخصی سازی شده ای را طراحی نمود و آن ها را پردازش کرد.  طراحی عنصر در تد جی اس بسیار آسان  می باشد و از تمامی جنبه ها به عنصر دسترسی خواهید داشت تا تغییرات لازم را اعمال نمایید.

هر عنصر در تد جی اس دارای یک نام و مجموعه ای از مشخصه های مشخص می باشد. این مشخصه ها محدودیتی در وجود دیگر مشخصه ها ایجاد نمی نماید.

علاوه بر مشخصه ها تابع فراخوانی نیز وجود دارد که پردازش عنصر را بر عهده دارد تا عنصر را به شکل دلخواه تبدیل نماید.

## ایجاد عنصر
به منظور ایجاد یک عنصر ابتدا می بایست از طریق تابع `define` آن را تعریف نماید. سپس مقدار خروجی تابع را به تابع `create` ارسال نمایید و تابع پردازش آن را طراحی کنید.

!! نکته
! تابع `define` یک نمونه ابتدایی از عنصر را به صورت ناپدید (`display:none`) طراحی می نماید .
! پس از فراخوان تابع ، مقداری به عنوان کلید دستیابی (Access Key) آن بازگشت داده می شود.
!
! یک یا چندین کلید دستیابی را می توان به تابع `create` ارسال نمود.

<br/>

! مقدار بازگشتی
! در توابع زیر درصورت عدم موفقیت مقدار `-1` بازگشت داده می شود.

### تابع define
وظیفه این تابع ایجاد یک نمونه اولیه از عنصر مورد نظر می باشد تا بتوان در ادامه آن را طراحی نمود.

تابع دارای دو آرگمان می باشد . آرگمان اول ، نام عنصر مورد نظر می باشد و آرگمان دوم آرایه ای از نام مشخصه های این عنصر می باشد.

<br/>

!! نکته
! درصورت مشخص کردن مشخصه های مخصوص عنصر ، مقادیر آن مشخصه ها به ترتیب مشخص شدن در آرایه ، به تابع پردازش گرد عنصر ارسال خواهند شد

<br/>

#### ساختار
```javascript
ted.define(

    tag_name : string ,

    attributes_name : Array<string>

) : -1 | object
```

### تابع create
پس از تعریف عنصر از طریق تابع `define` ، حال می بایست عنصر را با تابع `create` طراحی نمود. 

این تابع دارای دو آرگمان ورودی می باشد. 

آرگمان اول یک مقدار یا یک آرایه از مقادیر کلید های دستیابی را دریافت می نماید. 
و آرگمان دوم تابع پردازش گر می باشد که فرایند ایجاد عنصر به عهده آن می باشد.

#### ساختار
```javascript
ted.create(

    access_keys : object | Array<object> ,

    callback : function(attributes_value : string ...)

) : -1 | ted
```

### مثال

طراحی DropDown اختصاصی . محتوای درون عنصر ، به عنوان تمپلیت هر سطر استفاده خواهد شد .

مقدار دهی لیست از طریق جاوا اسکریپت خواهد بود. 

رخداد onSelect برای فراخوانی تابعی بعد از انتخاب سطر ها می باشد.

مشخصه `multiselect` به معنای امکان انتخاب چندین سطر می باشد

<br/>

###### جاوا اسکریپت : 
کد زیر درون کتابخانه می بایست اجرا گردد

```javascript
var DrDown = ted.define( "my-list", ["multiselect"] );

ted.create( DrDown , function( multiSelect ){
    var 
        template = this.html(),
        option_start = '<div class="myListOption">',
        option_end = '</div>',
        select_title = tedApi.elm(
            '<div class="myListSelect"><span class="Arrow">&#x25BC;</span><span class="Text"></span></div>'
        ),
        i,
        src = this.self.src,
        elm = "",
        that = this,
        ListToggle = function(){
            that.self.classList.toggle("active");
            var arrow = tedApi.find(select_title,".Arrow")[0];
            if(that.self.classList.contains("active")){
                arrow.innerHTML = "&#x25B2;";
            }
            else{
                arrow.innerHTML = "&#x25BC;";
            }
        };

    if(!tedApi.isUndefined(multiSelect)){
        multiSelect = true;
    }
    else{
        multiSelect = false;
    }

    this.html("");

    tedApi.find(select_title,".Text")[0].innerHTML = "انتخاب نشده";

    if( !tedApi.isJustArray(this.self.src) ){
        throw "The 'my-list' element has no data to load!";
    }

    for( i = 0 ; i < src.length ; i++ ){
        elm += 
            '<div class="myListData" data-row="'+i+'">' + 
                '<div class="myListContent">' + set_template_data( template , src[i] ) + '</div>' +
            '</div>';
    }

    elm = tedApi.elm(
        option_start +
        elm +
        option_end
    );

    // event on clicking menu to open/close list
    tedApi.bind(select_title,"click",ListToggle);
    
    // event : select and set value
    tedApi.bind( tedApi.find(elm,".myListData") , "click" , function(){
        if(!multiSelect){
            var all = tedApi.find(elm,".myListData.active");
            for(var j = 0; j < all.length ; j++){
                all[j].classList.remove("active");
            }
        }

        this.self.classList.toggle("active");

        if(!multiSelect){
            ListToggle();
        }

        var 
            all = tedApi.find(elm,".myListData.active"),
            name = [],
            rows = [],
            data = [];

        for(var j = 0; j < all.length ; j++){
            rows.push( tedApi.attr( all[j] , "data-row" ) );
            data.push( src[ tedApi.attr( all[j] , "data-row" ) ] );
            name.push( src[ tedApi.attr( all[j] , "data-row" ) ].title );
        }

        if(tedApi.isFunction(that.self.onSelect)){
            that.self.onSelect.call(that,data,rows);
        }

        tedApi.find(select_title,".Text")[0].innerHTML = name.join(" و ");
    });

    this.append(select_title);
    this.append(elm);

    // make elelemt visible
    this.show();
});
```

!! نکته
! در طراحی عناصر باید توجه شود که به صورت پیشفرض ، عنصر به صورت ناپدیدی (`display:none`) ایجاد می شود ، به همین دلیل می بایست در زمان اتمام فرایند پردازش عنصر ، آن را با تابع (`this.show()`) به نمایش درآورید

<br/>

! پیاده سازی
! پیاده سازی تابع `set_template_data` :
! ```javascript
!/**
! * @arg {string} template
! * @arg {object} obj
! */
!function set_template_data( template , obj ){
!   for( var i in obj ){
!       template = template.replace( "{{" + i + "}}" , obj[i] );
!   }
!   return template;
!}
! ```

<br/>



###### html : 

```html
<my-list id="myList" multiselect>

    <h5>{{title}}</h5>
    <p>
        <small>{{desc}}</small> | <a href="{{link}}">{{link_name}}</a>
    </p>

</my-list>

<script>
    tedApi.elm("#myList").src = [
        { 
            title : "تیتر اول",
            desc : "توضیحات اول",
            link : "#",
            link_name : "کلیک کنید"
        },
        { 
            title : "تیتر دوم",
            desc : "توضیحات دوم",
            link : "#",
            link_name : "کلیک کنید"
        },
        { 
            title : "تیتر سوم",
            desc : "توضیحات سوم",
            link : "#",
            link_name : "کلیک کنید"
        },
        { 
            title : "تیتر چهارم",
            desc : "توضیحات چهارم",
            link : "#",
            link_name : "کلیک کنید"
        }
    ];

    tedApi.elm("#myList").onSelect = function( datas , row_numbers ){
        /* ... */
    }
</script>
```


### نمونه قابل اجرا

مثال بالا به صورت زنده


@code(width:100%,height:400px)

```html
<my-list id="myList" multiselect>

    <h5> {{title}} </h5>
    <p>
        <small> {{desc}} </small> | 
        <a href="{{link}}"> {{link_name}} </a>
    </p>

</my-list>

<p class="output"> مقدار کنونی: <span id="Current"></span>  </p>

<script>
    tedApi.elm("#myList").src = [
        { 
            title : "تیتر اول",
            desc : "توضیحات اول",
            link : "#",
            link_name : "کلیک کنید"
        },
        { 
            title : "تیتر دوم",
            desc : "توضیحات دوم",
            link : "#",
            link_name : "کلیک کنید"
        },
        { 
            title : "تیتر سوم",
            desc : "توضیحات سوم",
            link : "#",
            link_name : "کلیک کنید"
        },
        { 
            title : "تیتر چهارم",
            desc : "توضیحات چهارم",
            link : "#",
            link_name : "کلیک کنید"
        }
    ];

    tedApi.elm("#myList").onSelect = function( datas , row_numbers ){
        var names = [];
        for(var i=0;i<datas.length;i++){
            names.push(datas[i].title);
        }
        tedApi.elm("#Current").innerHTML = names.join(" و ");
    }
</script>
```ext{js}(https://cdn.tedjs.org/tedjs/1.2.2/js/2ed.min.js?675)
```ext{js}

function set_template_data( template , obj ){
   for( var i in obj ){
       template = template.replace( "{{" + i + "}}" , obj[i] );
   }
   return template;
}

```js
tedApi.export.Attr = function(ted){
    var DrDown = ted.define( "my-list", ["multiselect"] );

    ted.create( DrDown , function( multiSelect ){

        var 
            template = this.html(),
            option_start = '<div class="myListOption">',
            option_end = '</div>',
            select_title = tedApi.elm(
                '<div class="myListSelect"><span class="Arrow">&#x25BC;</span><span class="Text"></span></div>'
            ),
            i,
            src = this.self.src,
            elm = "",
            that = this,
            ListToggle = function(){
                that.self.classList.toggle("active");
                var arrow = tedApi.find(select_title,".Arrow")[0];
                if(that.self.classList.contains("active")){
                    arrow.innerHTML = "&#x25B2;";
                }
                else{
                    arrow.innerHTML = "&#x25BC;";
                }
            }

        if(!tedApi.isUndefined(multiSelect)){
            multiSelect = true;
        }
        else{
            multiSelect = false;
        }

        this.html("");

        tedApi.find(select_title,".Text")[0].innerHTML = "انتخاب نشده";

        if( !tedApi.isJustArray(this.self.src) ){
            throw "The 'my-list' element has no data to load!";
        }

        for( i = 0 ; i < src.length ; i++ ){
            elm += 
                '<div class="myListData" data-row="'+i+'">' + 
                    '<div class="myListContent">' + set_template_data( template , src[i] ) + '</div>' +
                '</div>';
        }

        elm = tedApi.elm(
            option_start +
            elm +
            option_end
        );

        // event on clicking menu to open/close list
        tedApi.bind(select_title,"click",ListToggle);
        
        // event : select and set value
        tedApi.bind( tedApi.find(elm,".myListData") , "click" , function(){
            if(!multiSelect){
                var all = tedApi.find(elm,".myListData.active");
                for(var j = 0; j < all.length ; j++){
                    all[j].classList.remove("active");
                }
            }

            this.self.classList.toggle("active");

            if(!multiSelect){
                ListToggle();
            }

            var all = tedApi.find(elm,".myListData.active");
            var name = [];
            var rows = [];
            var data = [];
            for(var j = 0; j < all.length ; j++){
                rows.push( tedApi.attr( all[j] , "data-row" ) );
                data.push( src[ tedApi.attr( all[j] , "data-row" ) ] );
                name.push( src[ tedApi.attr( all[j] , "data-row" ) ].title );
            }

            if(tedApi.isFunction(that.self.onSelect)){
                that.self.onSelect.call(that,data,rows);
            }

            tedApi.find(select_title,".Text")[0].innerHTML = name.join(" و ");

        });

        this.append(select_title);
        this.append(elm);

        this.show();

    });
}

```ext{css}[my-Css]->mystyle.css
my-list h5{
    font-family: Vazir;
    font-size: 12px;
    margin: 5px 0px;
    color: #570158;
}

my-list p {
    margin: 3px 0px;
}
my-list small {
    font-family: Vazir;
    font-size: 11px;
    color: #b0b0b0;
}
my-list a {
    text-decoration: none;
    font-family: Vazir;
    font-size: 11px;
    color: #3F51B5;
}

```css
.output{
    position: absolute;
    text-align: center;
    top: 50%;
    color: #fff;
    width: 50%;
    padding: 0px;
    margin: 0 auto;
    left: 0px;
    right: 0px;
    font-family: Vazir;
    font-size: 19px;
    direction: rtl;
}
my-list{
    width: 200px;
    height: 35px;
    margin: 30px auto;
    position: relative;
    background: #fff;
    border-radius: 3px;
    overflow: hidden;
    margin-bottom:0px;
    font-family:Vazir;
    z-index: 99;
}

my-list.active{
    height: auto;
}

my-list .myListSelect{
    height: 35px;
    border-bottom: solid 2px #3e63ff;
    display: flex;
    cursor: pointer;
}


my-list .myListSelect .Arrow{
    width: 35px;
    line-height: 35px;
    font-size: 13px;
    position: relative;
    text-align: center;
    color: #c1c1c1;
}

my-list .myListSelect .Text{
    text-align: right;
    text-indent: 17px;
    width: 100%;
    direction: rtl;
    line-height: 35px;
    color: #3e63ff;
    overflow:hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
}

my-list .myListOption{
    position: relative;
    max-height: 195px;
    margin: 0px;
    padding: 0px;
    overflow-y: auto;
}

my-list .myListOption .myListData{
    width: 100%;
    min-height: 50px;
    display: flex;
    direction: rtl;
    border-bottom: solid 1px #bbb;
    cursor: pointer;
}

my-list .myListOption .myListData.active,
my-list .myListOption .myListData:hover{
    background: #e8e8e8;
}

my-list .myListOption .myListData .myListContent{
    width: 100%;
    padding: 5px 12px;
    box-sizing: border-box;
}

@endcode