## تابع bind
event ها جزئ جدایی نا پذیر از جاوا اسکریپت به شمار می آیند که از طریق آن می توان بر روی `DOM` های مختلف ، رخداد های متفاوتی را نیز ایجاد کرد مانند کلیک ، حرکت موس و...

در `tedjs` نیز این تابع وجود دارد که کاربا آن کمی راحت تر شده و رخداد های جدیدی نیز در آن اضافه شده است.

!! نکته
! تابع `bind` از توابع `tedApi` می باشد

<br/>

!! نکته
! خروجی تابع درصورت موفقیت عددی به عنوان آیدی رخداد و در غیر این صورت `0` خواهد بود

<br/>

!!! توجه
! اشاره گر `this` در تابع فراخوان ارسالی به تابع ، یک شی از `__GLOBALS__` می باشد.

<br/>

علاوه بر رخداد های موجود در جاوا اسکریپت مانند `click` ، `mouseenter`  و ... 
رخداد های مخصوص `tedjs` نیز وجود دارند. 

### رخداد attrchange
توسط این رخداد خواهید توانست ، هر مشخصه دلخواهی از عنصر یا عناصر مورد نظر خود را شنود نمایید. درصورت تغییر در مشخصه ، تابع فراخوان اجرا خواهد شد.

این رخداد همراه با نام یک مشخصه نوشته می شود که با `:` از یک دیگر جدا می گردند. مانند : `attrchange:value`

#### مثال
```javascript
tedApi.bind("a", "attrchange:href", function(_new, old) {
    console.log(_new, old, this)
});
```

@code(width:100%,height:300px)

```html
<div id="box" back="#000">
    Data Box
</div>

<div id="ChoosColorBox">
    یک رنگ انتخاب نمایید : 
    <input type="color" id="ChoosColor"/>
</div>
```js

tedApi.bind("div#box","attrchange:back",function(_new,_old){
    var
        backColor = _new,
        textColor = invertColor(_new);

    this.css({
        "background-color":_new,
        color:textColor
    });
});

tedApi.attr("div#box","back","#000");

//================================
//         Pick Color
//================================

tedApi.bind("#ChoosColor","change",function(){
    tedApi.attr("div#box","back",this.self.value);
});

//=================================
//   Inverting Color Functions
//=================================

function invertColor(hex) {
    if (hex.indexOf('#') === 0) {
        hex = hex.slice(1);
    }
    if (hex.length === 3) {
        hex = hex[0] + hex[0] + hex[1] + hex[1] + hex[2] + hex[2];
    }
    if (hex.length !== 6) {
        throw new Error('Invalid HEX color.');
    }
    var r = parseInt(hex.slice(0, 2), 16),
        g = parseInt(hex.slice(2, 4), 16),
        b = parseInt(hex.slice(4, 6), 16);
    r = (255 - r).toString(16);
    g = (255 - g).toString(16);
    b = (255 - b).toString(16);
    return "#" + padZero(r) + padZero(g) + padZero(b);
}

function padZero(str, len) {
    len = len || 2;
    var zeros = new Array(len).join('0');
    return (zeros + str).slice(-len);
}

```css
#box{
    width: 200px;
    height: 138px;
    margin: 0 auto;
    position: relative;
    box-shadow: 0 0 20px 3px rgba(0, 0, 0, 0.26);
    text-align: center;
    line-height: 138px;
}
#ChoosColorBox{
    direction: rtl;
    color: #fff;
    text-align: center;
    padding: 21px 0px;
}
```ext{js}(https://cdn.tedjs.org/tedjs/1.2.1/js/2ed.min.js?234)
@endcode

### رخداد inchange
این رویداد ، حالت کلی تری از رویداد `inchange` می باشد که حالت های دیگری همچون اضافه کردن ، حذف کردن یک عنصر و یا تغییر یک `TextNode` را شامل می شود و می توان تغییرات آن را تحت نظر قرار داد.

<br/>

!!! توجه
! این رخداد نیز همانند رخداد قبل ، از `:` به عنوان جداکننده به منظور اعمال فیلتر استفاده می نماید با این تفاوت که فیلتر ، __اختیاری__  است و همچنین می بایست __نام تگ__ نود دلخواه را وارد کرد
!
! همانند `inchange:div`

<br/>

حالات مختلف رخداد:

##### حالت کنترل مشخصه ها (attr)
در این حالت ، همانند رخداد قبل ، نظارت بر روی شاخصه های عنصر صورت میگیرد. تفاوت با حالت قبل در این است که ، در رخداد قبل ، نظارت تنها بر روی یک شاخصه صورت میگیرد ، اما در این حالت ، نظارت بر روی تمامی مشخصه هاست  که داده آن دارای نام مشخصه ، مقدار جدید آن و مقدار قبلی آن می باشد.

<br/>

!!! توجه
! این حالت ، مشخصه تمامی عناصر داخلی و یا خود عنصر جاری را تحت پوشش قرار می دهد ، اما عنصر تغییر یافته در آن مشخص نمی شود

<br/>

! بروزرسانی
! در نسخه `1.2.1` فریم ورک ، عنصر تغییر یافته از طریق مشخصه `target` ، زیر مجموعه مشخصه `value` در دسترس خواهد بود

<br/>

##### حالت اضافه شدن نود (add)
در این حالت ، نظارت بر روی اضافه شدن نود های جدید درون عنصر جاری است . مقدار در دسترس آرایه ای از نود های اضافه شده خواهند بود.

<br/>

##### حالت حذف شدن نود (remove)
همانند حالت `add` عمل می نماید ، با این تفاوت که بر روی حذف نود ها نظارت می نماید و مقدار در دسترس ، آرایه ای از نود های حذف شده خواهند بود

<br/>

##### حالت ویرایش متون (char)
این حالت مختص نود های متنی طراحی شده است و بر روی آن ها نظارت می نماید. در صورتی که بر روی نود های متنی درون عنصر جاری ، تغییری ایجاد گردر ، رخداد با حالت جاری اجرا خواهد شد و مقدار در دسترس ، نود متنی خواهد بود

<br/>

!! نکته
! توجه شود که حالات بالا به عنوان یک مشخصه در یک شی به عنوان پارامتر اول تابع فراخوان رخداد ارسال می گردد

<br/>


#### مثال
```javascript
tedApi.bind("body", "inchange:div", function(data) {
    /* 
        data => {
            type : <One Of The types [attr,add,remove,char]>,
            value : <if type == [attr , add , remove]>
            target : <if type == [char]>
        } 
    */
});
```

@code(width:100%,height:300px)

```html
<div id="box">
    <div id="content"></div>
    <div id="console"></div>
</div>

<div id="DataEditBox">
    کد html خود را وارد نمایید : 
    <input type="input" id="insertHTML" value="<div>div 1</div>"/>
    <button type="button" id="add">اضافه کردن به عنوان html</button>
    <button type="button" id="addText">اضافه کردن به عنوان متن</button>
    <button type="button" id="remove"> حذف کردن کردن آخرین نود</button>
</div>
```js

tedApi.bind("div#content","inchange",function(data){

    if(data.type == "char"){
        addConsole(
            "یک نود متنی اضافه یا حذف شد و یا تغییر پیدا کرد با مقدار: "+ 
            encodeHTML(data.target.nodeValue) 
        );
    }
    else if(data.type == "attr"){
        addConsole(
            "مشخصه "+
            data.value.name+
            " در عنصر "+
            data.value.target.tagName+
            " با مقدار "+
            data.value.old+
            " به مقدار "+
            data.value.new+
            " تغییر پیدا کرد"
        );
    }
    else if(data.type == "add"){
        for(var i = 0;i< data.value.length ; i++){
            addConsole(
                (data.value[i].tagName?"عنصر ":"نود متنی ")+
                (data.value[i].tagName || encodeHTML(data.value[i].nodeValue) )+
                " اضافه شد"
            );
        }
    }
    else if(data.type == "remove"){
        for(var i = 0;i< data.value.length ; i++){
            addConsole("عنصر "+data.value[i].tagName+" حذف شد");
        }
    }
    
});

tedApi.bind("#add","click",function(){
    tedApi.append("#content", tedApi.elm("#insertHTML").value );
});

tedApi.bind("#addText","click",function(){
    tedApi.append(
        "#content", 
        document.createTextNode(tedApi.elm("#insertHTML").value) 
    );
});

tedApi.bind("#remove","click",function(){
    tedApi.delete( tedApi.elm("#content").lastChild );
});


//========================
//      functions
//========================

function addConsole(data){
    tedApi.append("#console","<p>"+data+"</p>")
}


function encodeHTML(str) {
	var buf = [];
	
	for (var i=str.length-1;i>=0;i--) {
		buf.unshift(['&#', str[i].charCodeAt(), ';'].join(''));
	}
	
	return buf.join('');
}


```css
#box{
    width: 80%;
    height: 138px;
    margin: 0 auto;
    position: relative;
    border: solid 1px #fff;
    text-align: center;
    display:flex;
}
#DataEditBox{
    text-align: center;
    direction: rtl;
    color: #fff;
    margin: 17px 0px;
}
#content{
    width: 50%;
    height: inherit;
    color:#fff
}
#console{
    width: 50%;
    height: inherit;
    background:#fff;
    color:#000;
    overflow-y:auto;
}

#console p{
    direction: rtl;
    text-align: right;
    padding: 11px 14px;
    border-bottom: solid 1px #eee;
    margin: 0px;
    box-sizing: border-box;
}

```ext{js}(https://cdn.tedjs.org/tedjs/1.2.1/js/2ed.min.js?234)
@endcode

<br/>

! خروجی
! خروجی این تابع از نوع __عدد__ می باشد که در صورت عدم موفقیت مقدار __0__ و در غیر این صورت یک عدد که نمایانگر آیدی آن رخداد می باشد ، بازگردانده می شود

<br/>

#### ساختار

```javascript
tedApi.bind(

    element : string | HTMLElement | NodeList ,

    eventName : string ,

    callback : function

) : number
```


## تابع unbind
این تابع هماطور که از نامش برمی آید ، وظیفه غیر فعال کردن رخداد های ثبت شده برای عناصر را دارد. هر رخداد دارای یک نام رخداد و یک آی دی می باشد که از طریق این دو مشخصه می توان رخداد های یک نود را حذف کرد و برخلاف توابع خود جاوا اسکریپت نیازی به ارسال تابع رخداد نمی باشد. 

<br/>

!! نکته
! درصورتی که نام رخداد ارسال شود ، تمامی رخداد های آن دسته حذف خواهند شد . درصورت ارسال آی دی رخداد ، تنها رخداد مربوطه حذف خواهد شد و درصورتی که هیچ چیز ارسال نشود تمامی رخداد های نود مربوطه حذف خواهند شد.

<br/>


#### مثال
```javascript
tedApi.unbind("body") // remove all 
```
<br/>

#### مثال
```javascript
tedApi.unbind("body","click") // remove all click events
```
<br/>

#### مثال
```javascript
tedApi.unbind("body", <Number> ) // remove the specified id
```
<br/>


! خروجی
! تعداد رخداد های حذف شده

<br/>

#### ساختار

```javascript
tedApi.bind(

    element : string | HTMLElement | NodeList ,

    // optional
    event_Or_ID : string | number ,

) : number
```