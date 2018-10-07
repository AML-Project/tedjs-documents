## تابع animGraph
این تابع یکی از توابع اصلی کتابخانه `tedjs` می باشد که از توابع درجه دوم استفاده می کند و یک راه آسان برای ایجاد افکت ها و انیمیشن های دو بعدی را فراهم می سازد.
<br/>
!! نکته
! این تابع ، به صورت کلاس طراحی شده است . می بایست برای استفاده از این کلاس یک <b>نمونه</b> ایجاد نمایید
<br/>

<img src="https://doc.tedjs.org/assets/images/anim.jpg"></img>
<span class="imgDesc">هر قله یا دره در نمودار یک معادله درجه دوم می باشد</span>

<br/>

!!! توجه
! برای ایجاد انیمیشن ، می بایست هر نقطه را به صورت یک آرایه متشکل از دو عضو که به ترتیب از سمت راست به چپ عرض نمودار و ارتفاع نمودار می باشد نمایش داد.

<br/>

#### مثال
```javascript
    [ 
        10 /* The Length */,
        -10 /* The Height */
    ]
```

شروع انیمیشن از نقطه مبدا (0,0) می باشد و رو به جلو بر روی محور X حرکت می نماید

این تابع دارای سه آرگومان ورودی می باشد که ورودی اول آرایه ای از آرایه های دو عضوی نقاط می باشد ، ورودی دوم آن تابع فراخوان در هر لحظه می باشد و آخرین ورودی ، تابع فراخوان انتهایی می باشد که در زمان اتمام انیمیشن اجرا خواهد شد.

<br/>

#### ساختار
```javascript
tedApi.animGraph(
    // form : [ ... , [ Length , Height ] , ...]
    points : Array<Array[2]> ,

    callback : function(x , y , precent) ,
    // optional
    endCallBack : function(x , y , precent)

) : animGraph
```

! بروزرسانی
! از نسخه `1.1.3` فریم ورک ، خروجی توابع زیر ، شی ایجاد شده می باشد


### تابع start
تابع اجرای انیمیشن

پس از ساخت یک نمونه از کلاس ،می توان انیمیشن را به وسیله این تابع اجرا نمود. تابع دارای یک آرگومان ورودی اختیاری نیز می باشد که درصورت ارسال مقدار `true` به آن ، انیمیشن بدون توقف تکرار خواهد شد تا زمانی که آن را متوقف نمایید یا پایان دهید.

<br/>

#### ساختار
```javascript
[animGraph Object].start(
    // optional , default : false
    loop : boolean

) : animGraph
```


### تابع stop
تابع توقف انیمیشن

پس از اجرای این تابع ، انیمیشن متوقف می گردد و مقادیر آن دوباره مقدار دهی می گردد.

<br/>

#### ساختار
```javascript
[animGraph Object].stop() : animGraph
```

### تابع end
تابع پایان دادن اجرای انیمیشن

همانند تابع `stop` عمل می نماید با این تفاوت که در تابع `stop` تابع فراخوان اجرا می گردد اما در این تابع ، تابع فراخوان انتهایی اجرا می گردد. 

<br/>

#### ساختار
```javascript
[animGraph Object].end() : animGraph
```


### تابع pause
تابع توقف موقت انیمیشن

همانند توابع `stop` و `end` عمل می نماید با این تفاوت که انیمیشن دوباره مقدار دهی نمی گردد و می توان آن را از همان نقطه توقف ادامه داد.

<br/>

#### ساختار
```javascript
[animGraph Object].pause() : animGraph
```


### تابع continue
تابع ادامه یا شروع دوباره انیمیشن

درصورتی که انیمیشن متوقف شده باشد یا به صورت موقف متوقف شده باشد ، می توان با این تابع آن را ادامه داد.

<br/>

#### ساختار
```javascript
[animGraph Object].continue() : animGraph
```


### تابع gravity
تنظیم جاذبه بر روی اجرای انیمیشن

! بروزرسانی
! این تابع از نسخه `1.1.3` به فریم ورک اضافه گشته است

انیمیشن به صورت پیش فرض در جاذبه شبیه سازی شده 0 اجرا می شود ، این بدان معنیست که نمایش انیمیشن بر اساس `fps` سیستم کاربر می باشد و با سرعت ثابت نمایش داده می شود.
تابع دارای یک آرگومان ورودی اختیاری است که درصورتی که مقدار `true` به آن ارسال گردد ، جاذبه `9.8` به آن اعمال می شود

<br/>

#### ساختار
```javascript
[animGraph Object].gravity(
    // optional , Default : false
    state : boolean

) : animGraph
```

### تابع precent
پردازش انیمیشن به صورت مرحله ای

! بروزرسانی
! این تابع از نسخه `1.2.0` به فریم ورک اضافه گشته است

تابع `precent` همانطور که از نامش پیداست ، به صورت درصدی اجرا می شود ، بدین صورت که درصورت نزدیک بودن رینج درصد به هر یک از درصد های مشخص شده که به عنوان آرگومان اول تابع ارسال شده است ، تابع آن اجرا می گردد.

<br/>

!! نکته
! فرایند اجرای این تابع بدین صورت است که به عنوان مثال درصورتی که درصد های 1، 10 ، 100 ارسال شود ، از مقدار 0 تا 1 به تابع درصد 1 ، از مقدار 1 تا 10 به تابع درصد 10 و مابقی به 100 ارسال می گردد

<br/>

#### ساختار
```javascript
[animGraph Object].precent(

    precent : number | Array<number> ,

    callback : function( x , y , precent , currentPercentage )
    
) : animGraph
```


### تابع point
پردازش انیمیشن بر اساس نقاط وارد شده

! بروزرسانی
! این تابع از نسخه `1.2.2` به فریم ورک اضافه گشته است

این تابع تقریبا همانند تابع `precent` می باشد. فرایند در این تابع بر اساس نقاط وارد شده ([x,y]) می باشد . 

به عنوان مثال درصورتی که سه نقطه وارد شده باشد ، سه معادله درجه دوم نیز ایجاد شده است که بر اساس شماره معادله می توان به آن قسمت دسترسی داشت و آن قسمت را پردازش نمود

<br/>

#### ساختار
```javascript
[animGraph Object].point(

    point : number | Array<number> ,

    callback : function( x , y , precent , currentPoint )
    
) : animGraph
```

<br/>

! بروز رسانی
! در نسخه `1.2.2` ، آرگومان های point و precent در دو تابع به همین نام ها ، می توانند آرایه ای از اعداد را نیز دریافت نمایند و مقدار کنونی آن عدد نیز اط طریق آرگومان چهارم تابع فراخوان آن در دسترس می باشد

<hr/>

#### مثال

@code(width:100%,height:300px)

```html
<div id="Box">
    <div id="AnimBox">
        <div id="TextBrack">
            <span id="before"></span>
            <p><span>Hello World</span></p>
            <span id="after"></span>
        </div>

        <div id="Buttons">
            <button type="button" onclick="RunAnim(1);">اجرا با جاذبه</button> 
            <button type="button" onclick="RunAnim(0);">اجرا بدون جاذبه</button>
        </div>
    </div>

    <div id="ShowChart">
        <canvas id="canv"></canvas>
    </div>
</div>
```ext{js}(https://cdn.tedjs.org/tedjs/1.2.2/js/2ed.min.js?675)
```ext{js}
var c;
var wid;
var ctx;
var len;
var height_ = 0;
var minH = 0;

function InitDraw(points){
    c = tedApi.elm("#canv");
    wid = parseInt(tedApi.css(c, "width")) - 10;

    c.width = parseInt(tedApi.css(c, "width"));
    c.height = parseInt(tedApi.css(c, "height"));

    len = 0;
    for(var i in points){
        len += points[i][0];
        height_ = height_ < points[i][1] ? points[i][1] : height_;
        minH = minH > points[i][1] ? points[i][1] : minH;
    }
    ctx = c.getContext("2d");
    ctx.clearRect(0, 0, c.width, c.height);
}

function DrawPoint(x,y){
    ctx.beginPath();
    ctx.arc(14 + tedApi.scaleNumber({min:0,max:len},{min:0,max:wid - 16},x), ((tedApi.scaleNumber({min:minH,max:height_},{min:(c.height)-20 ,max:20},y))), 2, 0, 2 * Math.PI);
    ctx.fill();
}

```js
var TextBrack = tedApi.elm("#TextBrack");
var points = [
    [30,10],
    [60,200],
    [90,400],
    [120,600],
    [150,800],
    [50,-20],
    [50,40],
    [50,-100],
    [100,700]
];

var anim = new tedApi.animGraph(points,function(x,y,precent){
    DrawPoint(x,y);
});

anim
    .point([1,2,3,4,5],function(x,y,percent,point){
        if(y < (point)*50){
            tedApi.css(TextBrack,"opacity","1");
        }
        else{
            tedApi.css(TextBrack,"opacity","0");
        }
    })
    .point([6,7,8,9],function(x,y,per,point){
        if( x >= (30+60+90+120+150) && x <= (30+60+90+120+150+50+50+50+40) ){
            var width = tedApi.scaleNumber({min:-100,max:700},{min:0,max:80},y);
            tedApi.css(TextBrack,"width",width+"%");
            tedApi.css("#before,#after","width","6px");
        }
    });

anim
    .precent(90,function(){
        tedApi.css(TextBrack,{
            "-webkit-transform": "scale3d(1, 1, 1)",
            "transform": "scale3d(1, 1, 1)"
        });
    })
    .precent([91,92],function(){
        tedApi.css(TextBrack,{
            "-webkit-transform": "scale3d(.9, .9, .9) rotate3d(0, 0, 1, -3deg)",
            "transform": "scale3d(.9, .9, .9) rotate3d(0, 0, 1, -3deg)"
        });
    })
    .precent([93,95,97,99],function(){
        tedApi.css(TextBrack,{
            "-webkit-transform": "scale3d(1.1, 1.1, 1.1) rotate3d(0, 0, 1, 3deg)",
            "transform": "scale3d(1.1, 1.1, 1.1) rotate3d(0, 0, 1, 3deg)"
        });
    })
    .precent([94,96,98],function(){
        tedApi.css(TextBrack,{
            "-webkit-transform": "scale3d(1.1, 1.1, 1.1) rotate3d(0, 0, 1, -3deg)",
            "transform": "scale3d(1.1, 1.1, 1.1) rotate3d(0, 0, 1, -3deg)"
        });
    })
    .precent(100,function(){
        tedApi.css(TextBrack,{
            "-webkit-transform": "scale3d(1, 1, 1)",
            "transform": "scale3d(1, 1, 1)"
        });
    });


function RunAnim(gravity){
    InitDraw(points);

    tedApi.css(TextBrack,"width","0%");
    tedApi.css("#before,#after","width","0px");

    anim.end();
    anim.gravity(gravity).start();
}

RunAnim(1);

```css
#Buttons{
    display: block;
    position: relative;
    text-align: center;
    width: 100%;
}

#Buttons button{
    color: #7af22b;
    background: transparent;
    font-family: Vazir;
    font-size: 15px;
    font-weight: bolder;
    border: solid 1px;
    margin: 0px 6px;
    cursor: pointer;
    padding: 2px 10px;
}

#Buttons button:hover{
    color: #000;
    background: #7af22b;
}

#canv{
    width: 100%;
    height: 100%;
}

#Box{
    width: 100%;
    height: 100%;
    box-sizing: border-box;
    position: relative;
    display:flex;
    border-radius: 5px;
    overflow:hidden
}

#TextBrack{
    color:#28ff30;
    position: relative;
    width: 0%;
    text-align: center;
    margin: 0 auto;
    top: 50%;
    transform: translateY(-50%);
    -moz-transform: translateY(-50%);
    -webkit-transform: translateY(-50%);
    -ms-transform: translateY(-50%);
    -o-transform: translateY(-50%);
    display: flex;

    transition:transform 0.1s;
    -webkit-transition:transform 0.1s;
    -moz-transition:transform 0.1s;
    -ms-transition:transform 0.1s;
    -o-transition:transform 0.1s;
}


#TextBrack #before,
#TextBrack #after{
    position: absolute;
    top: 0;
    border: 2px solid;
    width: 0px;
    height: 100%;
    transition:width 0.08s;
    -webkit-transition:width 0.08s;
    -moz-transition:width 0.08s;
    -ms-transition:width 0.08s;
    -o-transition:width 0.08s;
}

#TextBrack #after{
    right: -6px;
    border-left: 0px;
}

#TextBrack #before{
    left: -6px;
    border-right: 0px;
}

#TextBrack p{
    overflow: hidden;
    height: 25px;
    font-size: 25px;
    position:relative;
    margin: 0px auto;
}

#TextBrack p span{
    width: 320px;
    position: relative;
    display: block;
    margin: 0 auto;
    left: 50%;
    transform: translateX(-50%);
    -webkit-transform: translateX(-50%);
    -moz-transform: translateX(-50%);
    -ms-transform: translateX(-50%);
    -o-transform: translateX(-50%);
}


#AnimBox{
    width:50%;
    height: 100%;
    background: #000;
}
#ShowChart{
    width:50%;
    height: 100%;
    background: #fff;
}

@endcode

## تابع onFrame

این تابع یک تابع Asynchrone می باشد که از طریق آن می توان متناسب با فریم ریت سیستم ، تغییرات را نمایش داد.

تابع یک تابع فراخوان دریافت می نماید و به توجه به فریم ریت (به طور پیش فرض 60) تابع را یک بار اجرا می نماید.

<br/>

#### مثال
```javascript
tedApi.onFrame(function(){
 
    /* ... */
 
});
```

<br/>

#### ساختار
```javascript
tedApi.onFrame(

    callback : function
    
) : void
```

## تابع onFrameInterval

همانند تابع بالا عمل می نماید با این تفاوت که به صورت بی انتها تکرار خواهد شد.

<br/>

#### مثال
```javascript
var __Date = new Date();

tedApi.onFrameInterval(function(){
 
    var now = new Date();
    var fps = 1000 / (now - __Date);
    __Date = now;

    console.log(fps + " fps");
 
});
```

<br/>

#### ساختار
```javascript
tedApi.onFrameInterval(

    callback : function
    
) : void
```