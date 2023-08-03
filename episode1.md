## MSquare Programming Fullsatck Developer Course ( Batch 2)
### Episode 1 Summary
##



Episode 1 မှာတော့ 
 - Html အကြောင်း
 - Html Tag အကြောင်:
 - Html Tag တွေရဲ့ Attribute အကြောင်း
 - CSS အကြောင်း
စတာတွေ လေ့လာသွားပါတယ်။

### Html 
Html အကြောင်းမှာတော့
web တွေ စတင်တုန်းက Html သီးသန့် ပဲ သုံးခဲ့ကြကြောင်း၊
စာဖတ်ဖို့ သက်သက်သာ သုံးခဲ့ကြကြောင်း၊
CSS,JS တွေ နောက်ပိုင်းမှ ပေါ်လာတာ ဖြစ်ပါတယ်။
သဘောပြောရရင် 
Html က Naked ဖြစ်နေတဲ့ လူတစ်ယောက်ဆိုပါစို့
 CSS က အ၀တ်အစားဆင် မိတ်ကပ်ပြင်ပေးလိုက်တာပါ။
  JS က လှုပ်ရှားမှု(action)တွေ ထည့်ပေးလိုက်တာပါ။
  Html fileတွေကို chrome/firefox စတဲ့ web browser တွေမှာ ဖွင့်ကြည့်လို့ ရပါတယ်။

### Html Document structure
![enter image description here](https://lecturely.com/assets/ckeditor/imageuploader/uploads/html-cheatsheet-intro.png)
- ***!DOCTYPE html*** ဆိုတာက Html file ဖြစ်ကြောင်း browser ကို ပြောလိုက်တာပါ။
- html document တစ်ခုကို  Html file ဖြစ်ကြောင်း ကြေငြာပြီးရင်
- html ဖိုင်ကို ***.html*** နဲ့ file format ထားသိမ်းပေးရပါမယ်.
- ```<html> ``` နဲ့ စပြီး ```</html>```နဲ့ ပိတ်ပေးရပါမယ်။


### **Html tag**
- Html tag ရေးရင် **အဖွင့် tag** ```<body> ``` နဲ့ **အပိတ် tag** ```</body> ``` ရေးပေးရပါတယ်။
- ကိုယ်ပြချင်တဲ့ အကြောင်းအရာကို ```<body> ``` နဲ့ ```</body> ```ကြားမှာ ရေးပေးရပါမယ်။

### ထူးခြားပြီး အသုံးများသောTag အချို့
```<a> ```<br>
- Document တစ်ခုကနေ နောက် documentတစ်ခုကို link လုပ်ပေး။
- State less ဖြစ်။
- မူလ document ကနေ နောက် documnetတစ်ခုရောက်ရင် မူလကဟာတွေအကုန်ပျောက်သွားပါတယ်။ 
```<a>``` tag ကိုဒီလိုရေးလေ့ရှိပါတယ်။<br>
```<a href="your-link">Google</a>```
*your-link* မှာ ကိုယ်သွားချင်တဲ့ url/file ကိုထည့်ပေးရပါတယ်။
##
```<div> ```<br>
 တကယ်ကို အသုံးများတဲ့ tag ပါ။
 အကန့်ကန့် အပိုင်းခွဲပေးတာပါ။
##
```<img> ```<br>
ပုံများဖော်ပြချင်တဲ့အခါသုံး။
Img tag အတွက် အပိတ် tag ထည့်ပေးရန်မလို **self-closing**။
ရေးနည်း ```<img src="Url" />```
**Url** မှာ ကိုယ်ပြချင်တဲ့ ပုံရဲ့ တည်နေရာ or Link ကိုထည့်ပေးရပါမယ်။
ကိုယ့် ကွန်ပြူတာထဲက ပုံရဲ့ တည်နေရာကို
**relative url** လို့ခေါ်ပြီး အင်တာနက်က ပုံရဲ့ တိုက်ရိုက် link ကို **absolute url** လို့ခေါ်ပါတယ်။
##
```<p> ```<br>
စာပိုဒ်ခွဲတဲ့အခါ အသုံးပြုပါတယ်။
##
```<h> ```<br>
စာတွေကို ခေါင်းစဥ် အဖြစ် ရေးချင်ရင်သုံးပါတယ်။
```<h1>Header1</h1>``` 
# Header1
```<h2>Header2</h2>``` 
## Header2
```<h2>Header3</h2>``` 
### Header3
```<h3>Heade41</h3>``` 
#### Header4
```<h5>Header5</h5>``` 
##### Header5
```<h6>Header6</h6>``` 
###### Header6
##

## Html Attribute
![enter image description here](https://clearlydecoded.com/assets/images/posts/2017-09-04-anatomy-of-html-tag/html-tag-attributes.png)

**Attribute** ဆိုတာ Html Tag တစ်ခုကို အထူးပြုပေးလိုက်တာပါ
**Attribute** တွေမှာ **Name** နဲ့ **Value** ရှိပြီး အလယ်မှာ ***=*** ထည့်ပေးရပါမယ်။
အသုံးများတဲ့ **Attribute** တွေက
**class / id / src / style**  စတာတွေပါပါတယ်။
ထူးခြားချက် အနေနဲ့ **style Attribute** က ****html tag** တိုင်းမှာ သုံးလို့ရပါတယ်**
##
## important Note

 - Html tag(Element) တွေမှာ 
**Inline level Element** နဲ့ **Block level Element**ရှိပါတယ်။
 **Inline level element** ဟာ သူ့ရဲ့ ရှိသလောက် width ပမာဏသာ နေရာယူပြီး ကျန်နေသေးတဲ့ width ကို နောက် Element ကို နေရာထပ်ပေးပါတယ်။
  **Block level element** ဟာ width အပြည့်နေရာယူလိုက်ပြီး နောက် Element ကို လိုင်း အသစ်မှာ နေရာပေးလိုက်ပါတယ်။

## CSS
![enter image description here](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTP6rtBCZj2zcGdG8zPKtO82omPNVdjqJLbwWk_BMPL6dxD9DAQSMw_5Cjsiuz1B3So9N0&usqp=CAU)
**CSS**ဆိုတာ Html Element တွေကို style,design,color,layout ပြုပြင်ဖန်တီးလုပ်ဆောင်ပေးတာပါ။
 1. **Inline CSS**
 2. **Internal CSS**
 3. **External CSS**
  ဆိုပြီး သုံးမျိုးရှိပါတယ်။
##
## 1.Inline CSS
Html tag ထဲမှာ style attribute နဲ့ တစ်ခါထဲ ထည့်ရေးတာပါ။
![enter image description here](https://www.myprograming.com/wp-content/uploads/2021/03/inline-CSS.png)
![enter image description here](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQR-adFhG28vCnoBkyiw8YYqdb616ZOsuF7Gw&usqp=CAU)

## 2.Internal CSS
![enter image description here](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSPtBAkGzLManMm4YkOtzSDSL7mD8-iS48SWA&usqp=CAU)
```html
    <html>
    <head>
      <style>
          p ( Tag selector) {
            color (property) : red (value) 
            }
            
          .hello(class selector) {
            font-size : 20px;
            color : blue
            }
          #hello (Id selector){
          border: 1px solid black;
          backgroud-color : #234;
          text-align : center
          }
            
       </style>
      </head>
      <body>
        <p>Hello</p>
       
        <div class ="hello">Hello Class</div>
       
        <div id="hello">
        Hello ID 
        </div>
      
      </body>
      </html>
```

## 3.External CSS
အပြင်မှာ **style.css** ဖိုင် သက်သက်ရေးပြီး **Html document ထဲ link ချိတ်** အသုံးပြုတာ ပါ။
သူ့ကိုလဲ internal css ကဲ့သို့ပဲ `<head></head>`ကြားမှာ link လုပ်ပေးရပါတယ်။

    <html>
     
     <head>
      <link rel="stylesheet" type="text/css" href="style.css">
     </head>
     
     <body>
     </body>
    
    </html>
##
## Important Note

 - CSS မှာ property တစ်ခုထပ် ပိုရင်  **;** (semi column) ခံပေးရပါမယ်။
 - tag တွေကို နာမည်အတိုင်း ခေါ်သုံးနိုင် 
 div tag ကို select(ခေါ်သုံး ) မယ်ဆိုရင် html documentထဲက ရှိရှိသမျှ div tag တိုင်း သက်ရောက်မှု ရှိပါမယ်။
 - **class** attribute ကို select(ခေါ်သုံး ) မယ်ဆိုရင်   **.** (dot) ရှေ့မှာ ထည့်ပေးရပါမယ်။
 - **id** attribute ကို select(ခေါ်သုံး ) မယ်ဆိုရင်   **#** (sharp) ရှေ့မှာ ထည့်ပေးရပါမယ်။
 - **id** attribute က တစ်ခုကို တစ်ခါပဲ သုံးသင့်ပါတယ်။
 ဆိုလိုတာက `id="koko"` လို့ သုံးပြီးရင် နောက်ထပ် koko ဆိုတဲ့ id ထပ်မသုံးသင့်ပါ။
 
 - လက်တွေ့မှာ ****Inline CSS** နဲ့  **Internal CSS ကို အသုံးမပြုကြပဲ** **External CSS** ကို သာ သုံးကြပါတယ်။**
 - CSS **precedence** ဦးစားပေးစနစ်
    - တူညီတဲ့ selector တစ်ခုကို  ရေးနည်းသုံးနည်းစလုံးမှာproperty တူတူ ကြေငြာထားရင် 
    - **Inline CSS** က ဦးစားပေး **နံပါတ် တစ်**
    - **Internal CSS** က ဦးစားပေး **နံပါတ် နှစ်**
    - **External CSS** က ဦးစားပေး **နံပါတ် သုံ**းပါ
    - p tag တစ်ခုကို **inline css** မှာ  **red** , **Internal css** မှာ  **blue** , **external css** မှာ **yellow** လို့ ကြေငြာထားမယ်ဆိုရင် **precedence** ဦးစားပေးစနစ်အရ **red** ကိုပဲ ပြပေးမှာပါ။

