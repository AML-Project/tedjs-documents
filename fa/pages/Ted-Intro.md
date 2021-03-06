## مقدمه
فریم ورک تد جی اس (2ed) متشکل از دو قسمت اصلی می باشد. یکی فضای نام `tedApi` که از ابتدا در باره این فضای نام توضیحاتی داده شد و در قسمت های بعد نیز توابع انتهایی آن گفته می شود و دیگری `ted` که هسته اصلی این فریم ورک می  باشد. این هسته ، فرایند ایجاد و مدیریت عناصر ، شاخصه ها (Attributes) ، نود های متنی و نود های توضیحات را بر عهده دارد و آن را تسهیل می نماید.

در این قسمت با بخشی از توابع کمکی مانند توابع آماده ساز و رخداد های پردازش و همچنین توابع پردازشی و بازگردانی آشنا می شوید.

!!! توجه
! فضای نام `ted` تنها از طریق کتابخانه ها در دسترس خواهد بود. برای آشنایی بیشتر با کتابخانه ها به [بارگذاری کتابخانه ها](https://doc.tedjs.org/fa/page/includeLibrary) 
! مراجعه نمایید

## توابع آماده ساز

توابع آماده ساز یا (ready functions) دسته از توابع هستند که از طریق آن می توان پس از تکمیل فرایند رندرینگ و پردازش صفحه ، به عناصر ایجاد شده در DOM دسترسی داشت.

توابع زیر همگی یک فرایند را با تفاوت های کوچکی اجرا می نمایند:

### تابع eachReady
این تابع رخدادهای زمانی (setInterval) ایجاد می کند . رخداد زمانی بر اساس مدل طراحی زبان جاوا اسکریپت پس از اتمام فرایندهای همزمان (synchronous) به اجرا در خواهد آمد.

هر عنصر در یک رخداد زمانی در دسترس خواهد بود که به آن معناست ، پردازش به صورت ناهمزمان (asynchronous) خواهد بود.

!! نکته
! `this` در تابع فراخوان ، به عنصر جاری اشاره دارد

<br/>

#### ساختار
```javascript
ted.eachReady(

    callback : function

) : void
```

### تابع ready
این تابع همانند تابع `eachReady` عمل می نماید با این تفاوت که ، بررسی عناصر در یک خط اجرای همزمان اجرا خواهد شد.

!! نکته
! `this` در تابع فراخوان ، به عنصر جاری اشاره دارد

<br/>

#### ساختار
```javascript
ted.ready(

    callback : function

) : void
```

### تابع nodeReady
تابع `nodeReady` عمل کرد متفاوت تری نسبت به توابع بالا دارد. این تابع برخلاف توابع بالا تمامی نودها را از جمله نود های متنی و نود های توضیحات بررسی می نماید.

!! نکته
! در تابع فراخوان (callback) ، مقدار `this` نود جاری خواهد بود

<br/>

!! نکته
! تابع فراخوان (callback) دارای دو آرگمان ورودی می باشد. اولین آرگمان شماره ترتیب نود و دومین آرگمان ، درصد پیشرفت بررسی از تعداد کل نود ها می باشد

<br/>


!! نکته
! تابع فراخوان انتهایی (endCallback) پس از اتمام فرایند بررسی اجرا خواهد شد

<br/>

#### ساختار
```javascript
ted.nodeReady(

    callback : function( current_number : number , current_percentage : number ) ,

    endCallback : function

) : void
```

### تابع docReady
عملکردی یکسان با `tedApi.docReady` دارد. ([رجوع شود به توضیحات](https://doc.tedjs.org/fa/page/DOM#sec3))


## رخداد های پردازش
رخداد های پردازش ، مجموعه ای توابع می باشند که می توان مشخص نمود ، در هر یک از حالت های پردازش نود زیر چه عملیاتی انجام شود. 

### رخداد beforeCompile

این رخداد عملیات های درخواستی را قبل از شروع به پردازش یک نود اجرا می نماید ، در زمانی که بر روی نود تغییری انجام نشده و داده ای خوانده نشده است.

<br/>

!!! توجه
! تابع فراخوان دارای یک آرگمان ورودی است که از نوع boolean می باشد . این مقدار یک مقدار اختیاری است که از طریق تابع compile قابل ارسال می باشد.
!
! مقدار `this` به نود جاری اشاره دارد

<br/>

#### ساختار
```javascript
ted.beforeCompile(

    callback : function( option : boolean )

) : void
```


### رخداد beforeElementRun

این رخداد نیز همانند beforeCompile ، قبل از شروع به پردازش اجرا می شود با این تفاوت که تنها پیش از پردازش __خود عنصر__ اجرا می شود. 
پردازش عناصر مرحله ای انجام میگیرد. ابتدا پردازش مشخصه ها (Attributes) و سپس پردازش خود عنصر و این رخداد پس از پردازش مشخصه ها انجام می پذیرد.

<br/>

!!! توجه
! تابع فراخوان دارای یک آرگمان ورودی است که از نوع boolean می باشد . این مقدار یک مقدار اختیاری است که از طریق تابع compile قابل ارسال می باشد.
!
! مقدار `this` به نود جاری اشاره دارد

<br/>

#### ساختار
```javascript
ted.beforeElementRun(

    callback : function( option : boolean )

) : void
```


### رخداد afterCompile

رخداد afterCompile درست برخلاف رویداد های بالا ، پس از اتمام فرایند پردازش __نود__ اجرا می گردد.

<br/>

!!! توجه
! تابع فراخوان دارای یک آرگمان ورودی است که از نوع boolean می باشد . این مقدار یک مقدار اختیاری است که از طریق تابع compile قابل ارسال می باشد.
!
! مقدار `this` به نود جاری اشاره دارد

<br/>

#### ساختار
```javascript
ted.afterCompile(

    callback : function( option : boolean )

) : void
```

### رخداد EndCompile

رخداد EndCompile همانطور که از نامش پیداست پس از اتمام فرایند پردازش اجرا می گردد. 

<br/>

!!! توجه
! این رخداد تنها در اجرای نخست پردازش و در هر بار بارگذاری یک کتابخانه اجرا می گردد که در انتهای اجرای کامپایل اجزای کتابخانه فرایند اجرا می گردد.

<br/>

#### ساختار
```javascript
ted.EndCompile(

    callback : function

) : void
```

### رخداد onCompile

رخداد onCompile بر خلاف رخداد های قبل ، در فضای نام `tedApi` قرار دارد . این رخداد قبل از شروع فرایند پردازش نود اجرا می گردد.

<br/>

!! نکته
! تابع فراخوان دو آرگمان دریافت می نماید. اولین آرگمان شماره ترتیب نود و دومین آرگمان درصد پیشرفت پردازش می باشد
!
! مقدار `this` به نمونه از `__GLOBALS__` اشاره دارد که عنصر جاری را در بر دارد

<br/>

!!! توجه
! این رخداد تنها در اجرای نخست پردازش و در هر بار بارگذاری یک کتابخانه اجرا می گردد که در انتهای اجرای کامپایل اجزای کتابخانه فرایند اجرا می گردد.

<br/>

#### ساختار
```javascript
tedApi.onCompile(

    callback : function( node_number , current_percentage )

) : void
```

## توابع پردازشی

### تابع پشتیبان گیری
تابع پشتیبان گیر وظیفه دارد تا تمامی اطلاعات نود یک پشتیبان تهیه نماید تا در مواقع لزوم ، نود را به حالت ابتدایی بازگرداند. این مقادیر پشتیبان درون ساختار DOM خود نود ذخیره می گردد.

مانند عناصر که مقدار innerHTML و خصوصیت های عنصر ذخیره می گردد و یا نود های متنی یا نود های توضیحات که مقدار درون آن ذخیره می گردد.

<br/>

!!! توجه
! پس از یک بار پشتیبان گیری از هر نود ، دیگر قادر به پشتیبان گیری نخواهید بود و به اصطلاح پشتیبان گیری `lock` شده است. 
!
! برای رفع lock می بایست مقدار `true` را به عنوان آرگمان دوم تابع ارسال نمایید

<br/>

! خروجی
! درصورت موفقیت `1` و در غیر این صورت `0` بازگردانده می شود

<br/>

#### ساختار
```javascript
ted.backup(

    node : HTMLNode ,
    //optional , Default : false
    unlock : bolean

) : number
```

### تابع بازگردانی

پس از پشتیبان گیری از نود ، حال می توان درصورت ناموفق بودن تغییرات بر روی نود ، آن را به حالت ابتدایی بازگرداند. این فرایند محدودیت ندارد.

<br/>

! خروجی
! درصورت موفقیت `1` و در غیر این صورت `0` بازگردانده می شود

<br/>

#### ساختار
```javascript
ted.returnBack(

    node : HTMLNode

) : number
```

### تابع پردازش

تابع پردازش وظیفه اصلی پردازش را بر عهده دارد. این تابع با نام `compile` ، در هر دو فضای نام `ted` و `tedApi` وجود دارد. 

تفاوت این دو در این است که تابع موجود در فضای نام `ted` تنها بر روی یک سطح از نود پردازش انجام می دهد و اما تابع موجود در فضای نام `tedApi` بر روی عنصر و فرزندانش پردازش را انجام می دهد.

<br/>

!! نکته
! تابع پردازش در فضای نام `ted` آرگمان اختیاری نیز دارد که همان آرگمان اختیاری boolean می باشد که به رخداد ها ارسال می گردد.

<br/>

#### ساختار تابع در `ted`
```javascript
ted.compile(

    node : HTMLNode ,
    //optional , Default : false
    option : boolean

) : void
```

<br/>


#### ساختار تابع در `tedApi`
```javascript
tedApi.compile(

    node : string | HTMLElement

) : number
```


! خروجی
! درصورت موفقیت `1` و در غیر این صورت `0` بازگردانده می شود
