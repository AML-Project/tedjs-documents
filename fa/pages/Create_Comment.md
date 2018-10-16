## مقدمه
کامنت ها یا توضیحات (Comments) در کد ، به منظور تسهیل در فرایند دوباره خوانی کد و مستند کردن کد در تمامی زبان ها ایجاد می گردد. 

HTML نیز مانند دیگر زبان ها ، این چنین امکانی را درون خود گنجانده است. در این زبان ، کامنت ها به صورت یک نود (Node) ساخته می شوند و مانند باقی نود ها در این زبان در ساختار درختی داکیومنت (DOM) وارد می گردد. 

در برخی موارد نود های کامنت مانند برخی زبان های دیگر تنها به منظور مستند کردن کد مورد استفاده قرار نمی گیرند ، بلکه در رابطه با ارتباط کد با سیستم کامپایلر و یا موتور رندر نیز می توانند به کار روند.
مانند کامنت های تشخیص نوع مرورگر برای استفاده از کدهای خاص آن.

## نود کامنت در تد جی اس
در تد جی اس اما ، این نود ها می توانند فرایند های پیچیده تری را نیز بر عهده گیرند. از جمله این فرایند ها ، می توان به تشخیص سطوح دسترسی برای بارگذاری یک داده ، تشخیص مرورگر به صورت پیشرفته ، ایجاد تولتیپ های عناصر ، ذخیره و بازیابی داده ها و...  اشاره نمود.

نکته ای که در مورد نود های کامنت وجود دارد این است که ، این نود ها غیر قابل نمایش هستند و به منظور استفاده در رابطه با فرایند هایی که نیاز به نمایش ندارند کاربردی خواهند بود.

## ایجاد نود کامنت
به منظور ایجاد یک نود توضیحات می بایست از تابع `createComment` استفاده نمایید. این تابع می بایست درون کتابخانه استفاده گردد.
این تابع یک الگو از نوع Regular Expression چه با فرمت string و چه با فرمت RegExp دریافت می نماید. در هریک از نودهای کامنتی که این الگو یافت شود ، پردازش گر آن را اجرا می نماید.

<br/>

! مقدار بازگشتی
! درصورتی که عملیات با خطایی روبرو گردد (ورودی هایی با انواع نادرست) مقدار `-1` بازگشت داده می شود و در غیر این صورت فضای نام ted بازگشت می گردد.

<br/>

#### ساختار تابع
```javascript
ted.createComment(

    comment_algo : string | RegExp ,

    callback : function( matches : string ... )

) : -1 | ted
```

!! نکته
! مقدار `this` در تابع فراخوان ، به نود توضیحات جاری اشاره دارد

<br/>

!!! توجه
! در هر نود کامنت می توان چندین الگو از نود های کامنت طراحی شده استفاده نمود و هر نود کامنت محدود به یک کامنت طراحی شده نمی باشد

<br/>

### مثال
طراحی validator برای بررسی نوع داده input :

<br/>

###### جاوااسکریپت:
دو نوع نود کامنت طراحی شده است. یکی برای اعتبار سنجی ورودی و دیگری برای نمایش پیام های موفقیت و خطا

```javascript
//validator
ted.createComment( /\@validate\s*\((email|mobile)\)/ , function( whole , type ){
    var input = this.nextElementSibling;
    var Validate;

    if(input.tagName.toLowerCase() !== "input"){
        return;
    }

    if(type == "email"){
        tedApi.attr(input,"placeholder","Please Enter Your Email...");
        
        Validate = function(current){
            return tedApi.isEmail( current.self.value );
        };
    }
    else{
        tedApi.attr(input,"placeholder","Please Enter Your Mobile...");

        Validate = function(current){
            return mobile_validate( current.self.value );
        };
    }

    tedApi.bind(input,"input",function(){
        var val = Validate(this);
        if( this.self.value.trim() === "" ){
            this.self.classList.remove("success");
            this.self.classList.remove("error");
        }
        else if(val){
            this.self.classList.add("success");
            this.self.classList.remove("error");
        }
        else{
            this.self.classList.remove("success");
            this.self.classList.add("error");
        }
    });

});

// show success message
ted.createComment( "\\@state_success\\s*\\((.+?)\\)" , function( whole , message ){
    var input = this.nextElementSibling;
    var messageBox = tedApi.elm("<p style='display:none'></p>");

    if(input.tagName.toLowerCase() !== "input"){
        return;
    }

    tedApi.insert(messageBox).after(input);

    messageBox.innerHTML = message;

    tedApi.bind(input,"attrchange:class",function(){
        if( this.self.classList.contains("success") ){
            tedApi.css(messageBox,{
                display:"block"
            });
        }
        else{
            tedApi.css(messageBox,{
                display:"none"
            });
        }
    });
});

// show error message
ted.createComment( /\@state_error\s*\((.+?)\)/ , function( whole , message ){
    var input = this.nextElementSibling;
    var messageBox = tedApi.elm("<p style='display:none'></p>");

    if(input.tagName.toLowerCase() !== "input"){
        return;
    }

    tedApi.insert(messageBox).after(input);

    messageBox.innerHTML = message;

    tedApi.bind(input,"attrchange:class",function(){
        if( this.self.classList.contains("error") ){
            tedApi.css(messageBox,{
                display:"block"
            });
        }
        else{
            tedApi.css(messageBox,{
                display:"none"
            });
        }
    });
});
```

<br/>

! پیاده سازی
! پیاده سازی تابع `mobile_validate` :
! ```javascript
!/**
! * @arg {string} mobile
! */
!function mobile_validate(mobile){
!    var mob_pat = /^09\d{9}$/;
!    if( 
!        !( (mob_pat).test(mobile) )
!    ){
!        return false;
!    }
!    
!    return true;
!}
! ```

<br/>

###### HTML :
استفاده از نود
```html
<div>
    <!-- 
    * @validate (mobile)
    + @state_error( شماره همراه وارد شده صحیح نمی باشد )
    - @state_success( شماره همراه وارد شده صحیح می باشد )
    -->
    <input type="text"/>
</div>
```

<br/>

### نمونه قابل اجرا

مثال بالا به صورت زنده


@code(width:100%,height:400px)

```html
<div>
    <!-- 
    * @validate (email)
    + @state_error( ایمیل نامعتبر می باشد )
    - @state_success( ایمیل صحیح می باشد )
    -->
    <input type="text"/>
</div>
```ext{js}(https://cdn.tedjs.org/tedjs/1.2.2/js/2ed.min.js?675)
```ext{js}[other]
function mobile_validate(mobile){
    var mob_pat = /^09\d{9}$/;
    if( 
        !( (mob_pat).test(mobile) )
    ){
        return false;
    }
    
    return true;
}
```js
tedApi.export.Attr = function(ted){
    //validator
    ted.createComment( /\@validate\s*\((email|mobile)\)/ , function( whole , type ){
        var input = this.nextElementSibling;
        var Validate;

        if(input.tagName.toLowerCase() !== "input"){
            return;
        }

        if(type == "email"){
            tedApi.attr(input,"placeholder","Please Enter Your Email...");
            
            Validate = function(current){
                return tedApi.isEmail( current.self.value );
            };
        }
        else{
            tedApi.attr(input,"placeholder","Please Enter Your Mobile...");

            Validate = function(current){
                return mobile_validate( current.self.value );
            };
        }

        tedApi.bind(input,"input",function(){
            var val = Validate(this);
            if( this.self.value.trim() === "" ){
                this.self.classList.remove("success");
                this.self.classList.remove("error");
            }
            else if(val){
                this.self.classList.add("success");
                this.self.classList.remove("error");
            }
            else{
                this.self.classList.remove("success");
                this.self.classList.add("error");
            }
        });

    });

    // show success message
    ted.createComment( "\\@state_success\\s*\\((.+?)\\)" , function( whole , message ){
        var input = this.nextElementSibling;
        var messageBox = tedApi.elm("<p style='display:none'></p>");

        if(input.tagName.toLowerCase() !== "input"){
            return;
        }

        tedApi.insert(messageBox).after(input);

        messageBox.innerHTML = message;

        tedApi.bind(input,"attrchange:class",function(){
            if( this.self.classList.contains("success") ){
                tedApi.css(messageBox,{
                    display:"block"
                });
            }
            else{
                tedApi.css(messageBox,{
                    display:"none"
                });
            }
        });
    });

    // show error message
    ted.createComment( /\@state_error\s*\((.+?)\)/ , function( whole , message ){
        var input = this.nextElementSibling;
        var messageBox = tedApi.elm("<p style='display:none'></p>");

        if(input.tagName.toLowerCase() !== "input"){
            return;
        }

        tedApi.insert(messageBox).after(input);

        messageBox.innerHTML = message;

        tedApi.bind(input,"attrchange:class",function(){
            if( this.self.classList.contains("error") ){
                tedApi.css(messageBox,{
                    display:"block"
                });
            }
            else{
                tedApi.css(messageBox,{
                    display:"none"
                });
            }
        });
    });
}


```css
p{
    position: absolute;
    width: 100%;
    color: #fff;
    font-family: Vazir;
    margin:0px;
}
div{
    height: 100%;
    width: 100%;
    position: relative;
    text-align:center
}
input{
    background: #fff;
    border: solid 1px #3e63ff;
    border-radius: 3px;
    padding: 5px 10px;
    box-shadow: 0 0 20px #3e63ff;
    color: #3F51B5;
    top: 50%;
    transform:translateY(-50%);
    -webkit-transform:translateY(-50%);
    -moz-transform:translateY(-50%);
    -ms-transform:translateY(-50%);
    -o-transform:translateY(-50%);
    position: relative;
    outline:none;
    transition:all 0.2s;
    -webkit-transition:all 0.2s;
    -moz-transition:all 0.2s;
    -ms-transition:all 0.2s;
    -o-transition:all 0.2s;
}
.success{
    border: solid 1px #8BC34A;
    box-shadow: 0 0 20px #8BC34A;
    color: #8BC34A;
}
.error{
    border: solid 1px #E91E63;
    box-shadow: 0 0 20px #E91E63;
    color: #E91E63;
}

@endcode