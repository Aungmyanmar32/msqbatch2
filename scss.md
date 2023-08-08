**What is CSS library?**
CSS library ဆိုတာက web-developer တွေ web-app ရေးတဲ့အခါ<br>
**ကုဒ် နည်းနည်းနဲ့ အချိန်တိုတို အတွင်း ထိထိရောက်ရောက် ရေးသားဖန်တီးနိုင်ဖို့ အတွက်** <br>အသင့်ရေးပီးသား pure CSS တွေကို
**သတ်မှတ်ထားတဲ့ အတိုင်း code တိုတိုလေး ရေးပေးရုံနဲ့** ယူသုံးလို့ ရအောင် ပြုလုပ်ပေးတဲ့ အရာပါ။

ဘာလို့  CSS library တွေ သုံးသင့်တာလဲ?<br>
Complex ဖြစ်တဲ့ web-app တွေ ဖန်တီးအခါ CSS အများကြီး ရေးရလေ့ရှိပါတယ်.
code များလေ file ဆိုဒ် ကြီးလေ loading time ကြာလေ ဖြစ်တက်ပါတယ်။
ဖန်တီးသူတွေ အနေနဲ့လဲ အချိန်တွေ အများကြီး ပေးရပါတယ်<br>
**button tag** တစ်ခုကို design လုပ်ဖို့ ရိုးရိုး CSS နဲ့ဆို **properties and Values အများကြီး**နဲ့ CSS ရေးပေးရပါတယ်။
ဒီလို button tag design အတွက် **CSS library** သုံးလိုက်ရင် **သတ်မှတ်ထားတဲ့ class name တစ်ခု ထည့်ပေးလိုက်ရုံ**နဲ့ အဆင်ပြေတာမလို့  pure CSS တွေ ကိုယ်တိုင် **အစအဆုံး ရေးစရာမလိုပဲ မြန်မြန် နဲ့ မှန်မှန် ရနိုင်ပါတယ်**။<br>
CSS library တွေကို အချိန်ကုန်သက်သာတာရယ် အစအဆုံးရေးစရာမလိုတာရယ် ရိုးရိုး CSS ရေးရင် ဖြစ်လာမယ့် ဖိုင်ဆိုဒ်ကို အများကြီး လျော့ချပေးနိုင်တာရယ်  မြန်မြန် နဲ့ မှန်မှန်ရနိုင်တာရယ် စတာတွေကြောင့် အသုံးပြုကြပါတယ်။

## လေ့လာသွားမယ့် CSS library/framework များ

 - SCSS
 - Bootstrap
 - Tailwind

### SCSS
Pre-processor CSS ဖြစ်ပါတယ်
ရိုးရိုး CSS မှာ မရနိုင်တဲ့
 

 - **variable ကြေငြာတာ**
 ```css
 CSS
body {
  font: 100% Helvetica, sans-serif;
  color: #333;
}
```
```css
SCSS
$font-stack: Helvetica, sans-serif;
$primary-color: #333;

body {
  font: 100% $font-stack;
  color: $primary-color;
}
```
 - **Nesting**
 ```css
 CSS
nav ul {
  margin: 0;
  padding: 0;
  list-style: none;
}
nav li {
  display: inline-block;
}
nav a {
  display: block;
  padding: 6px 12px;
  text-decoration: none;
}
```
```scss
SCSS
nav {
  ul {
    margin: 0;
    padding: 0;
    list-style: none;
  }

  li { display: inline-block; }

  a {
    display: block;
    padding: 6px 12px;
    text-decoration: none;
  }
}
```
 - **Mixin (function ဖန်တီးတာ)**
 ```scss
 SCSS
@mixin theme($theme: DarkGray) {
  background: $theme;
  box-shadow: 0 0 1px rgba($theme, .25);
  color: #fff;
}

.info {
  @include theme;
}
.alert {
  @include theme($theme: DarkRed);
}
.success {
  @include theme($theme: DarkGreen);
}
```
 - **Looping** 
 ```scss
 SCSS
$base-color: #036;

@for $i from 1 through 3 {
  ul:nth-child(3n + #{$i}) {
    background-color: lighten($base-color, $i * 5%);
  }
}

```
 - **Conditional check** 
 ```scss
 SCSS
$light-background: #f2ece4;
$light-text: #036;
$dark-background: #6b717f;
$dark-text: #d2e1dd;

@mixin theme-colors($light-theme: true) {
  @if $light-theme {
    background-color: $light-background;
    color: $light-text;
  } @else {
    background-color: $dark-background;
    color: $dark-text;
  }
}

.banner {
  @include theme-colors($light-theme: true);
  body.dark & {
    @include theme-colors($light-theme: false);
  }
}
```

စတဲ့အရာတွေကို
CSS file အထဲမှာပဲ ပြုလုပ်လို့ ရစေပါတယ်။
CSS ကို programing language တစ်ခုကဲ့သို့ ရေးလို့ရအောင် ပြုလုပ်ပေးတာပါ။

SCSSကို သုံးဖို့ အတွက် NPM (**N**ode **P**ackage **M**anager) ကနေ install and setup လုပ်ပြီးမှ အသုံးပြုနိုင်ပါတယ်။

### Bootstrap
Components တွေ အများကြီးကို ဖန်တီးထားပေးပြီး သက်ဆိုင်ရာ စကားလုံးတွေကို class attribute မှာ ထည့်ပေးလိုက်ရုံနဲ့ styling လုပ်ပေးပါတယ်။
Drop down Button တစ်ခုကို bootstrap နဲ့ ဖန်တီးထားတာပါ
![enter image description here](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRAgm-uJdJtCrUckwlt6qi4uO9de9Begaqy8Q&usqp=CAU)
```html
    <!DOCTYPE html>
    <html>
    <head>
      <meta name="viewport" content="width=device-width, initial-scale=1">
      <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css">
      <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.3.1/jquery.min.js"></script>
      <script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js"></script>
    </head>
    <body>
     
    <div class="container">
      <h2>Bootstrap Dropdowns Example</h2>
      <div class="dropdown">
        <button class="btn btn-primary dropdown-toggle" type="button" data-toggle="dropdown">Dropdown Example
        <span class="caret"></span></button>
        <ul class="dropdown-menu">
          <li><a href="#">HTML</a></li>
          <li><a href="#">CSS</a></li>
          <li><a href="#">JavaScript</a></li>
        </ul>
      </div>
    </div>
     
    </body>
    </html>
```
 Bootstrap ကိုတော့ ကုမ္ပဏီ တစ်ခု ရုံးတစ်ခု အတွက်ပဲ ဖန်တီးထားတဲ့ Internal-facing website/web-app တွေမှာပဲ သုံးလေ့ရှိကြပါတယ်။
 brand,design စတာတွေ ဦးစားပေးတဲ့ User-facing web-app တွေမှာ သုံးလေ့မရှိကြပါဘူး။
 Bootstarp ကို သုံးဖို့ အတွက် NPM (**N**ode **P**ackage **M**anager) ကနေ install and setup လုပ်ပြီးမှ အသုံးပြုနိုင်သလို
 CDN (content Delivery Network) link ချိတ်ဆက်ပြီးလဲ အသုံပြုနိုင်ပါတယ်။ Bootstarp CSS file တစ်ခုလုံး download ပြီး external CSS ဖိုင်ကဲ့သို့ ချိတ်ဆက်ပြီးလဲ အသုံးပြုနိုင်ပါတယ်။
   
   ### Tailwind
   Bootstrap ကဲ့သို့ Components တွေ အများကြီးကို မဖန်တီးထားပဲ
   utility first CSS အမျိုးအစား ဖြစ်ပါတယ်။
   CSS properties တွေ ကို internal CSS ရေးနည်းကဲ့သို့ class attribute ရဲ့ value မှာ သတ်မှတ်ထားတဲ့အတိုင်း ရေးလိုက်ရုံဖြင့် styling လုပ်လို့ရအောင် ဖန်တီးထားတာပါ။
   
```html
    <div class="container"></div>
    pure CSS
      .container {
        display : flex;
        justify-content : space-between;
        }
        
    Tailwind CSS
     <div class="flex justify-between"></div>
```         
### Important Note

 - CSS library/framework များသုံးပြီး ရေးထားတဲ့ CSS တွေကို browser တွေက မဖတ်နိုင်/နားမလည်နိုင်ပါဘူး။ CSS library/framework တွေကပဲ save လိုက်တာနဲ့ ရိုးရိုး CSSအဖြစ် ပြန်ပြောင်းပြီးမှ သိမ်း ပေးတာမလို့ browser မှာ run နိုင်ခြင်း ဖြစ်ပါတယ်။
 - CSS library/framework ကို CDN link ချိတ်တဲ့အခါ CSS file သာမက သက်ဆိုင်ရာ JavaScript file link လဲ ချိတ်ပေးရပါမယ်။ ဒါမှပဲ  library/framework ကောင်းကောင်း အလုပ်လုပ်မှာပါ။
 
 ### Q & A
 Q. project တစ်ခုမှာ မတူညီတဲ့ CSS library/framework သုံးလေ့ရှိပါသလား?
 - A. သုံးလေ့မရှိပါ။ တစ်ခုသာ သုံးပါတယ်။

Q. ဘာလို့ Tailwind က inline ဖြစ်နေရတာလဲ?
- A. inline က component အသေးတွေများတဲ့အခါ သင့်တော်တာမလို့ပါ။ Content switching အတွက် အကူအညီဖြစ်စေပါတယ် ။ 
 
 Q. Tailwind need class name?
 - A. No. Tailwind Doesn't need any Class or Id.

Q .Tailwind ကို external အနေနဲ့ သုံးလို့ရသလား?
- A. Yes, But not recommend.

Q. Bootstarp ကို pure CSS နဲ့ တွဲသုံးလို့ရနိုင်မလား?
- A. Yes. For customize by using over-write.
# Homework
## သက်ဆိုင်ရာ Documentations တွေဖတ်ပြီး CSS library တွေ စမ်းသုံးကြည့်ပါ
