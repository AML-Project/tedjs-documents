# tedjs documents pages


 [tedjs document website](https://doc.tedjs.org) is based on markedown pages.
 the document is a multi language document and the main language is `persian`. 
 if you are intersted , you can help me to translate the pages to other languages.
 
# Document File Structure
```
[Page Container]
├───[Language Folder]
│   ├───setting.json
│   ├───pages
│       ├───[page].md
├───setting.json
```
the `Page Container` is the root folder that contains `language folders`.
each `language folder` like `fa` has a `setting.json` and a `pages` folder.

`pages` folder contains the markdown files that the documents has been written in it.

# Setting.json
this file contains some setting properties. now is just the languages that are exist.

# [Language Folder] -> Setting.json
each language folder has its `setting` . this file has some rules.

* __files__ :  this property is an associative array that contains a list of markdown files details. the `key` of property is the file name with out `.md`.
* __title__ : this property is the title of the documentation site
* __indexTitle__ : on the `index` page of the site , under the logo , the header title will be replaced with this property.
* __indexDesc__ : this property will be replaced with the description area under the header title.

# Markdown Rules
tedjs document website has its own `markdown` version. it has some new operations that just work in the tedjs document website.

> notice: the `h2` will create a section and `h3` will create a block inside the section

## Callouts
Callouts are message boxes , like `success` , `info` , `warning`  and `error`

to use this feature you can use exclamation mark (!) . 

the first line is the determinative line to determine the type of message box and the text front it is the title of the box:

count of exclamation mark | type
-------------------------- | -----------------
1 | success
2 | info
3 | warning
4 | error

the other lines that are start with one exclamation mark will be used as the message of box
### Example
```
! success
! the message line 1
! the message line 2
```
or
```
!! info
! the message line 1
! the message line 2
```
of
```
!!! warning
! the message line 1
! the message line 2
```
or
```
!!!! error
! the message line 1
! the message line 2
```

## JsRun Code Block (PlayGround)
the other 3rd party operation is the code block.

to use this feature , you should first start with this line:
```
@code(width:<width>,height:<height>)
```
> the `<width>` and `<height>` can be any css like measurable units, like `200px` , `20%` and ...

and the of block must be:
```
@endblock
```

the between these you can use formats below to create css , html , js and external files.

each format is a block that its below contents will set as its content until the end of block or the start of other file format

formats |  example
-------------|----------------
\```html | \```html  <br/> \<div>this is a test\</div>
\```css| \```css <br/> body{<br/>color: black;<br/>}
\```js|\```js<br/>var a = 2;
\```ext{`<FileType>`}(`<ExternalUrl>`)[`<TabName>`] -> `<VirtualPath>` `injs`|\```ext(js)[js Library] ->/path/file.js injs <br/> var y = 2

> the `<FileType>` ca be one of `js` , `html` , `css` .

> if you want to attach  an external  library , you should write down its url instead of `<ExternalUrl>`

> `<TabName>` is the name that will show on the tab 

> if you want to use the external data as an accessible virtual file to use `ajax` to get its content you should set the `<VirtualPath>`

> the `injs` attribute is to enable the virtual path to be accessible from other js tabs.

### Example
```
@code(width:100%,height:300px)

```html
<myelm a="2" b="3"></myelm>
<br/>
<span id="Multiple"></span>

```js
var myLib = include( "file: mylib.js" );

tedApi.html( "#Multiple", myLib.multiple(1024,2048) );

```css
body{
    color:#fff
}

```ext{js}(https://cdn.tedjs.org/tedjs/1.2.0/js/2ed.min.js)

```ext{js}[mylib]->mylib.js injs
var 
    myElm = ted.define("myelm",["a","b"]),
    multiple = function(a,b){
        return a * b;
    };

ted.create(myElm,function(a,b){
    this.html( multiple( parseInt(a), parseInt(b) ) );
    this.show();
});

this.multiple = multiple;

@endcode
```
