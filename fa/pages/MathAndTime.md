## توابع ریاضی و عددی

توابع کار با اعداد و ریاضیاتی ، به دلیل وجود ضرورت این دست توابع در کتابخانه برای استفاده در تابع انیمیشن تعبیه شده اند. 

### کلاس equation

تابع `equation` ، یک تابع ریاضیاتی می باشد که خروجی آن بک تابع درجه دوم است. این تابع بیس اصلی تابع انیمیشن می باشد.

این تابع سه ورودی به عنوان نقطه بر روی صفحه دریافت می کند که شامل `x` و `y` می باشد ، سپس  اطلاعات را پردازش و معادله درجه دوم و معادله سقوط آزاد بدون استحکاک آن را ایجاد می نماید. خروجی هر یک از معادله ها به ازایه هر ورودی قابل محاسبه می باشد.

<br/>

! برای مطالعه
! فرمول تابع درجه دوم
!<span class="Math">
!y = Ax<sup>2</sup> + Bx + C
!</span>

!<br/>

! معادله سقوط آزاد بدون استحکاک
!<span class="Math">
!y = ( ( v<sub>0</sub> &times; sin( &Theta;<sub>0</sub> ) ) &times; t ) - ( (1/2) &times; g  &times; t<sup>2</sup> )
!</span>

<br/>

!!! توجه
! tedApi.equation  به عنوان یک کلاس تعبیه شده است پس می بایست برای استفاده ، شی ای از آن ایجاد شود.

به عنوان مثال شی ای به صورت زیر ایجاد می کنیم:

<img src="/assets/images/eq1.png"></img>
<span class="imgDesc">سه نقطه روی نمودار برای ایجاد معادله</span>

``` javascript
// point is like [ x , y ]
var eq = new tedApi.equation(
    [ 1 , 5 ], // point 1
    [ -5 , 7 ], // point 2
    [ 0 , 10 ] // point 3
)
```

#### توابع کلاس


##### :: تابع answer
از طریق این تابع می توان خروجی تابع درجه دوم ایجاد شده را در یک نقطه (`x`) بدست آورد.

<br/>

###### مثال
```javascript
eq.answer( 4 ); //return -21.199998
```

<br/>

###### مثال
```javascript
eq.answer( -5 ); //return 7
```

###### ساختار
```javascript
eq.answer(

    x : number

) : number
```

##### :: تابع inverse
این تابع برخلاف تابع `answer` عمل می نماید. مقدار `y` را دریافت و مقدار `x` را محاسبه می نماید

!! نکته
! در این تابع ، از مقدار دلتا (Δ) ، قدرمطلق گرفته می شود. به همین خاطر ، این تابع برای همه مقادیر پاسخ دارد. 

<br/>

!!! توجه
! درصورتی که پاسخ جزو اعداد مختلط باشد ، `x` های خروجی ، دارای `y` متفاوتی از `y` ورودی خواهند داشت. 
!
! درصورتی که مقدار دلتا (Δ) برابر با صفر شود، مقدار خروجی `x` ها یکی خواهد بود

<br/>

###### مثال
```javascript
eq.inverse( 0 ); //return Points {x1: -6.1105503, x2: 1.7534076}
```

<br/>

###### ساختار
```javascript
eq.inverse(

    y : number

) : number
```

##### :: تابع SEquat
این تابع ، سریع معادله درجه دوم را در قالب `string` ایجاد می نماید.

<br/>

###### مثال
```javascript
eq.SEquat(); //return "y = -0.9333333x^2 - 4.0666664x + 10"
```

<br/>

###### ساختار
```javascript
eq.SEquat() : number
```

##### :: تابع time
این تابع از فرمول معادله سقوط آزاد استفاده می نماید. از طریق این تابع می توانید در هر `x` ، زمانی که سپری می شود تا به `y` آن برسد را (بر حسب ثانیه) بدست می آورد.

!! نکته
! مقدار `0` ، زاویه پرتاب در ثانیه صفر می باشد

<br/>

###### مثال
```javascript
eq.time(-100); //return 0 // the first second has x = -100
```

<br/>

###### ساختار
```javascript
eq.time(

    x : number ,
    // optional , Default = 20
    init_velocity : number

) : number
```

##### :: مشخصه points
این مشخصه یک شی  متشکل از سه مشخصه `a` ، `b` و `c` بازمیگرداند.

این مقادیر ، در فرمول سری درجه دوم گذارده شده است. 

<br/>

###### ساختار
```javascript
eq.points.a : number
eq.points.b : number
eq.points.c : number
```

<hr/>


###### ساختار کلاس
```javascript
tedApi.equation(

    point1 : [ x : number , y : number ] ,

    point2 : [ x : number , y : number ] ,

    point3 : [ x : number , y : number ]

) : equation
```

### تابع closeNumber

تابع `close Number` ، نزدیکترین عدد به ورودی را از آرایه که مشخص می شود ، انتخاب می کند. 

<br/>

###### مثال
```javascript
tedApi.closeNumber(
    [ 1 , 5 , 11 , 25 , 105 , 10 , 4 , 80 ] , 
    8
); //return 10
```

<br/>

###### ساختار
```javascript
tedApi.closeNumber(

    numbers_list : Array<number> ,

    data : number

) : number
```

### تابع findAngel

از طریق این تابع می توان زاویه بین دو خط AB و BC یا سه نقطه A , B, C را به واحد رادیان بدست آورد.

<br/>

###### مثال
```javascript
tedApi.findAngel( {x:0,y:0}, {x:0,y:10}, {x:10,y:0} ); 
// return  0.7853981633974484(radians) => 45 Degree
```

<br/>

###### ساختار
```javascript
tedApi.findAngel(

    A : { x : number , y : number } ,

    B : { x : number , y : number } ,

    C : { x : number , y : number }

) : number
```

### تابع floatPoint

کاهش تعداد اعداد بعد اعشار فرایندی است که توسط تابع `Number.toFixed` تعبیه شده در جاوا اسکریپت انجام پذیر است.

 تفاوت این تابع با تابع `tedApi.floatPoint` در این است که در تابع `floatPoint` ، خروجی به صورت عددی می باشد و رقم آخر درصورت __لزوم__ گرد می شود ، در غیر این صورت خود آن عدد نوشته می شود

 <br/>

###### مثال
```javascript
tedApi.floatPoint( 1.35644987755e-25 , 26 ); 
// returns 1.3e-25
```

<br/>

###### ساختار
```javascript
tedApi.floatPoint(

    theNumber : number ,
    // optional , Default = 1
    floatPointPos : number

) : number
```

### تابع scaleNumber
در برخی اوقات نیاز است تا عددی در یک بازه را به به عدد هم ترازش در بازه ای دیگر تبدیل نماییم. برای این کار کافیست از تابع `tedApi.scaleNumber` استفاده کنیم. این تابع اعداد متناظر را در بازه های مختلف به یک دیگر تبدیل می کند.

###### مثال
```javascript
tedApi.scaleNumber( 
    {min : 0, max : 1920},
    {min : 800, max : 1024},
    1700
); 
// returns 998.3333333333334
```

<br/>

###### مثال
```javascript
tedApi.scaleNumber( 
    {min : 0, max : 100},
    {min : 0, max : 20},
    68
); 
// returns 13.6
```

<br/>

###### ساختار
```javascript
tedApi.scaleNumber(

    from_range : { min : number , max : number } ,

    to_range : { min : number , max : number } ,

    the_number : number

) : number
```

### تابع random
تابع رندم برای ایجاد متون و اعداد به صورت رندم مورد استفاده قرار میگیرد. خروجی این تابع سه نوع داده رندم می باشد. عدد رندم ، حروف رندم و متن مخلوط حرف و عدد به صورت رندم.

!! نکته
! تابع رندم دو ورودی دریافت می نماید. ورودی اول طول داده خروجی و ورودی دوم نوع داده رندم شده. درصورتی که طول داده مشخص نشود ، یک طول به صورت رندم انتخاب می شود.

<br/>

###### مثال
```javascript
tedApi.random(); // returns 3254 | a random number with random length
```

<br/>

###### مثال
```javascript
tedApi.random(10); // returns 1628244557 | a random number with the length 10
```

<br/>

###### مثال
```javascript
tedApi.random(15,"number"); // returns 1628244557 | a random number with the length 15
```

<br/>

###### مثال
```javascript
tedApi.random(20,"alpha"); 
// returns "PMUMJcdXNqPMWCUOeDfa" | a random alphabetic text with the length 20
```

<br/>

###### مثال
```javascript
tedApi.random(15,"hash"); 
// returns "641hv34W69Br6p0" | a random number and alphabetic text with the length 15
```
<br/>

###### ساختار
```javascript
tedApi.random(
    // Default = [random]
    length : number ,
    // optional , Default = "number"
    type : string ,

) : number
```


## توابع کار با زمان

### تابع toMiliSecond
در برخی موارد که نیاز است تا از زمان به صورت میلی ثانیه استفاده نماییم ، میتوان واحد های زمانی مانند ثانیه ، ساعت ،دقیقه را به میلی ثانیه و به صورت عدد خروجی دهد.

!! نکته
! این تابع یک ورودی از نوع `string` دریافت می نماید. درصورتی که ورودی از سه نوع میلی ثانیه، ثانیه ، دقیقه و ساعت باشد ، مقدار آن به میلی ثانیه بازگشت داده می شود در غیر این صورت مقدار `null` برمیگردد. درصورتی که ورودی از نوع `string` نباشد ، همان ورودی به خروجی می رود.


<br/>

###### مثال
```javascript
tedApi.toMiliSecond("5ms"); // returns 5 
```
<br/>

###### مثال
```javascript
tedApi.toMiliSecond("5s"); // returns 5000 
```
<br/>

###### مثال
```javascript
tedApi.toMiliSecond("5m"); // returns 300000 
```
<br/>

###### مثال
```javascript
tedApi.toMiliSecond("5h"); // returns 18000000 
```
<br/>


###### مثال
```javascript
tedApi.toMiliSecond("5d"); // returns null 
```
<br/>


###### مثال
```javascript
tedApi.toMiliSecond([]); // returns [] 
```
<br/>

###### ساختار
```javascript
tedApi.toMiliSecond(

    time : string

) : number
```

### تابع secToTime

از طریق این تابع می توان ثانیه را به تاریخ تبدیل کرد. بدین معنی که به عنوان مثال `315360000` ثانیه ده سال می شود.

!! نکته
! خروجی این تابع شی ای می باشد متشکل از 7 مشخصه که ثانیه ، دقیقه ، ساعت ، روز ، ماه ، سال و یک رشته از ساعت را مشخص می نماید.

<br/>

!!! توجه
! درصورتی که ورودی به صورت عدد نباشد مقدار `-1` بازگردانده می شود

###### مثال
```javascript
tedApi.secToTime(315360000);
// returns {
//              string:"0:0:0",
//              year:10,
//              month:0,
//              day:0,
//              hour:0,
//              minute:0,
//              second:0
//          }
```
<br/>


###### مثال
```javascript
tedApi.secToTime(123456789);
// returns {
//              string:"21:33:9",
//              year:3,
//              month:10,
//              day:29,
//              hour:21,
//              minute:33,
//              second:9
//          }
```
<br/>

###### ساختار
```javascript
tedApi.secToTime(

    time : number

) : tedDate
```

### تابع sleep

از طریق این تابع می توانید برای مدتی اجرای کد را به تعویق بیاندازید.

!! نکته
! درصورتی که ورودی تابع مقدار عددی نداشته باشد ، مقدار `false` در غیر این صورت مقدار `true` بازگشت داده می شود.

<br/>

!!! توجه
! مقدار ورودی میلی ثانیه و به صورت عدد می بایست در نظر گرفته شود

<br/>


###### مثال
```javascript
console.log(1);
tedApi.sleep(5000);
console.log(2);
```
<br/>

###### ساختار
```javascript
tedApi.sleep(

    time : number

) : boolean
```
