## Msqure Programing Fullstack Course
### Episode 2 Summary
Episode 2 မှာတော့ 
- CSS Layout နဲ့ ပတ်သတ်တဲ့ အရာတွေ 
- Javascript basic အချို့ကို
လေ့လာသွားကြပါမယ်။



##
### CSS Flex-Box
Html element တွေကို Layoutချတဲ့ အခါ အသုံးပြုပါတယ်။
Real world web app တွေ ရေးတဲ့အခါ တကယ်ကို အသုံများတဲ့ CSS နည်းပညာပါ။
Flex-box ကို မလေ့လာခင် Html Element တွေ ရဲ့ nesting သဘောသဘာ၀ကို အရင်သိထားရပါမယ်။
``` html
<div class="container"> parent element
     <div>child element
     Div 1
     </div>
     <div>child element
      Div2
     </div>
</div> 
```
Div နှစ်ခု ကို Div တစ်ခုထဲမှာ ထည့်ထားပါတယ်။
Div နှစ်ခုကို ထိန်းထားတဲ့ container ဆိုတဲ့ Div ဟာ သူ့အထဲက Div နှစ်ခုရဲ့ မိဘ parent ဖြစ်ပါတယ်။
အထဲက Div နှစ်ခုဟာ container Div ရဲ့ ကလေး child ဖြစ်ကြပါတယ်။

parent Div ကို display : flex; လို့ CSS property ပေးလိုက်တာနဲ့  သူ့ရဲ့ child Div တွေကို နေရာချ စီမံလို့ ရပါပြီး။

``` html
<div class="container"> parent element
     <div>child element
     Div 1
     </div>
     <div>child element
      Div2
     </div>
</div> 

CSS
   .container {
     display: flex ; 
    }
```
အသုံးများသော Flex -box property များ

    flex-direction : ### ;
    justify-content : ### ;
    align-items : ### ;
    flex-warp : ### ;
![enter image description here](https://byteiota.com/wp-content/uploads/2020/11/flex-direction.png)
![enter image description here](https://miro.medium.com/max/434/1*iigDGiNFBOUVJQ_07C1B2g.png)
![enter image description here](data:image/png;base64,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)
![enter image description here](https://samanthaming.gumlet.io/flexbox30/10-flex-wrap.jpg.gz)
##
    justify-content : center;
    align-items : conter

![enter image description here](http://2.bp.blogspot.com/-wSizB5Yw-EU/UCzPr_RVTzI/AAAAAAAAElg/a2BecXgMjSI/s1600/flexbox_layout.png)

### Important Note

 - margin /padding စတဲ့ CSS properties တွေဟာ Element တစ်ခုကို နေရာချ တဲ့ အခါ အသုံးပြုပြီး Layout များ လုပ်တဲ့အခါမှာတော့ flex-box ကို အသုံးပြုဖို့ ပိုပြီးသင့်တော်ပါတယ်။
 - display inline ဖြစ်တဲ့ element တွေကို width and height ပေးချင်ရင်  display : inline-block ဒါမှမဟုတ် display: block; အရင်ပြောင်းပေးရပါမယ်။
 - -child  DIv တွေကို စီမံချင်ရင် သူတို့တွေကို ထိန်းထားတဲ့ parent Div မှာ display : flex ;  ပေးပြီး flex-box properties တွေကို parent Div ရဲ့ CSS အထဲမှာပဲ စီမံရပါမယ်။
 - ပုံ နဲ့ စာ ကို center ကျကျ ထားချင်ရင် `vertical-align : middle ;` ကို သုံးနိုင်ပါတယ်။
##
### Javascript basic
Javascript အခြေခံလေးတွေကို ပြန်နွှေးပေးသွားမှာပါ။
ဒီနေ့သင်တာတွေ နားမလည်သေးဘူးဆိုရင် Msqure Youtube Channel မှာ အသေးစိတ်ရှင်းပြထားတွေကို သေချာလေး ပြန်ကြည့်ကြစေလိုပါတယ်။

JavaScript ရဲ့

 - Data-type
 - Variable
 - Scope
 - Operator
 - conditional statement
 စတာတွေကို လေ့လာသွားကြမှာပါ။


### JavaScript Data Types
<table border="0"><tbody><tr><td>Data Types</td>
				<td>ရှင်းလင်းချက်</td>
				<td>Example</td>
			</tr><tr><td><code>String</code></td>
				<td>စကားလုံး/စကားစု</td>
				<td><code>'hello'</code>, <code>"hello world!"</code> etc</td>
			</tr><tr><td><code>Number</code></td>
				<td>နံပါတ်များ</td>
				<td><code>3</code>, <code>3.234</code>, <code> etc.</td>
			</tr><tr><td><code>BigInt</code></td>
				<td>an integer with arbitrary precision</td>
				<td><code>900719925124740999</code> , <code></code> etc.</td>
			</tr><tr><td><code>Boolean</code></td>
				<td>မှန်သည် (သို့) မှားသည် </td>
				<td><code>true</code> and <code>false</code></td>
			</tr><tr><td><code>undefined</code></td>
				<td>မသတ်မှတ်ရသေးသော</td>
				<td><code>let a;</code></td>
			</tr><tr><td><code>null</code></td>
				<td>တန်ဖိုး Null ဖြစ်နေသော</td>
				<td><code>let a = null;</code></td>
			</tr><tr><td><code>Symbol</code></td>
				<td>သင်္ကေတ</td>
				<td><code>let value = Symbol('hello');</code></td>
			</tr><tr><td><code>Object</code></td>
				<td>အချက်အလက်များ စုစည်းထားသော</td>
				<td><code>let student = { };</code></td>
			</tr></tbody></table>



### Note 

 - `string` data-type သည် 
 -**Single quotes**:  `'Hello'`
-**Double quotes**:  `"Hello"`
-- **Backticks**:  `` `Hello` `` စသည် တို့ တစ်ခုခုဖြင့် ရေးသားပေးရသည်။ Single/Double quotes, backticks မပါသော text သည် `string` မဟုတ်ပေ။ ထို့ပြင် `"6"` `"209"` စသည်တို့သည် Double quotes ဖြင့် ရေးထားသောကြောင့်  `Number` data-type မဟုတ်ပဲ `string` data type ဖြစ်သည်။

- အထက်တွင် ဖော်ပြထားသောdata types  ထဲတွင် `object` data-type တစ်ခုမှလွဲပြီး ကျန် အားလုံးသည် **Primitive Data Type** (ထပ်ခွဲ၍ မရနိုင်သော) ဖြစ်ပြီး   `object` data-type မှာ  **Non-Primitive Data Type** (ထပ်ခွဲ၍ ရ) ဖြစ်သည်။

###  Variable
Variable ကြေငြာရန် var , let , const စသည့် keyword များ အသုံးပြုပေးရသည်။
**Syntax**
`keyword`  `Variable's name` = `Value` ;

    var num1 = 4 ; 
    let userName =  "Aung Myanmar" ;
    const age = 30 ;
### note

 - **var နှင့်  let**  သည် တစ်ခါကြေငြာပြီး နောက် **ပြုပြင်ပြောင်းလဲ၍ ရ**ပါသည်။  
 - **const** ကို အသုံးပြုပြီး ကြေငြာထားရင်ေတာ့ **ထပ်မံပြောင်းလဲ၍ မရနိုင်**ပါ။
 
  `keyword`  `Variable's name` = `Value` ; (**console,log result**) 

example : 1 ( **var** )
   
    var userName = "ko ko" ; ( ko ko )
      userName = "Mg Mg" ; ( Mg Mg )

 example : 2  ( **let** )

     var num1 = 5
      const num2= 8;
       let num3 = 2 ; ( 2 )
       num3 = num1 + num2 ; ( 13 )

example : 3 ( **const** )

    var num5 = 7 :
    const num6 = 3 ; ( 3 )
    let num7 = 19 ;
    num6 = num5 + num7 ; ( Error ) 

  
  

### Important Note

 - Javascript ကကြိုတင်သတ်မှတ်အသုံးပြုထာသည့် keyword များကို 
   variable name (or) function name အဖြစ် သုံးစွဲခွင့်မရှိပါ။
 ##
 ### Object 
 Object ဆိုတာ data တွေ valueတွေ အများကြီးပါတဲ့ variable တစ်ခုလိုပါပဲ။
 
 #### Declare Syntax

 `const` `object-name` = { 
 `property-key` : `"property-value"` `,`
 `key` : `value`
 } ;

![enter image description here](https://miro.medium.com/max/856/0*7EptITWWPaI9BoxL.png)
#### Usage syntax
`object-name`.`key` **(Dot** Notation)
`object-name`[`"key"`] (**Bracket** Notation)
#### example

    const user = {
                  name : "Aung Aung",
                  age : 23,
                  address : "Yangon"
                  }
    //dot notation
    const getUserName = user.name;
    console.log(getUserName);
    //"Aung Aung"
    
    // bracket notation
     const getUserAge = user["age"];
     console.log(getUserAge);
     //23
### Important Note

 - object ထဲမှာ property တစ်ခုထပ်ပိုရင် နောက် object မ‌ရေးခင် ကော်မာ **`,`** ထည့် ပေးရပါမယ်။ <br>
 -Example
 

 

       const myObj = {
                       name : "aung aung" ,
                       age : 27
                      }

 - object သိမ်းမယ့် **variable ကြေငြာရင်**  `let` / `var` အစား **`const`** **ကိုသာ အသုံးပြုသင့်**ပါတယ်။
##
 JavaScript Function အကြောင်း လေ့လာသွားကြမှာပါ။၊<br>
 `function` တွေကို အချိန် နဲ့ နေရယူတာကို လျော့နည်းစေရန်၊ တူညီတဲ့လုပ်ဆောင်ချက်များကို ထပ်ခါထပ်ခါ ရေးစရာမလိုပဲ  တစ်ခါသတ်မှတ်ထားရုံနဲ့ ပြန်လည်ပြီး အကြိမ်ကြိမ် အသုံးပြုနိုင်ရန်၊ စတာတွေအတွက် အသုံးပြုကြပါတယ်
![enter image description here](https://res.cloudinary.com/practicaldev/image/fetch/s--pClJgvrv--/c_limit,f_auto,fl_progressive,q_auto,w_880/https://dev-to-uploads.s3.amazonaws.com/i/mt2jlra7jd5gdgl8up8y.png)


#### JavaScript မှာ function ကို အသုံးပြုမယ်ဆိုရင်

 1. function ကို အရင် သတ်မှတ် ပေးရပါမယ် (define, declare)
 2. ထို သတ်မှတ်ထားတဲ့ function ကို အသုံးပြုရန် / အလုပ်လုပ်စေရန် function ကို ပြန်လည်ခေါ်ယူ ပေးရပါမည်။(call)
  
  ##
  ### Function တစ်ခု သတ်မှတ်ခြင်း (define, declare)
  - ပထမနည်းလမ်း
syntax
      ``function-keyword`` ``functionနာမည်`` ( ``parameter`` ){
       ``function body ;``
       } ;

   code

     function helloWorld(){
        console.log("Hello world");
        };

- **ES6 arrow Function** (antonymous အမည်မဲ့)
syntax
   `(parameter)` `=>` `{`
   `function body;`
   `}`

code
   

     () => {
        console.log (" Hello world");
        };

 **ES6** function ကို variable တစ်ခုထဲတွင် သိမ်းထားလေ့ရှိပါတယ်။
code

    const sayHello = () =>{
    console.log ( " Hello ")
    } ;
         
##
### Function ကို ခေါ် ယူအသုံးပြုခြင်း
အထက်မှာ ဖော်ပြခဲ့သလို function တွေကို သတ်မှတ်လိုက်ရုံ နဲ့ ဘာ လုပ်ဆောင်ချက်မျှ
ပြုလုပ်ပေးမည်မဟုတ်ပါ။<br>
သတ်မှတ်ထားတဲ့ **function Name** (or) **သိမ်းထားတဲ့ variable Name** ကို **ခေါ််ပေးမှသာ** function က အလုပ်လုပ်မည်ဖြစ်သည်။

    
      function helloWorld(){
        console.log("Hello world");
      };
       helloWorld();//call by function name
      
      //Hello world

**ES6**
   

       const sayHello = () =>{
         console.log ( " Hello ")
       } ;
            sayHello();//call by variable name
            
            // Hello
##
### **Function with parameter**
 - function name နောက်က လက်သဲကွင်း ထဲမှာ parameter ကို ထည့် သွင်းကာ define လုပ်နိုင်သည်။
 - ထို function ကို ခေါ် သုံးသောအခါ နောက်က လက်သဲကွင်း ထဲမှာ parameter ရဲ့ တန်ဖိုးကို ထည့် သွင်း
 ပြီး အသုံးပြုနိုင်ပါသည်။

code

    //example - 1
    
    function sayHi(name) {
    console.log ( "Hi" + " ," + name);
    };
    
    sayHi("Mg Mg");
    // Hi ,Mg Mg
    
    sayHi ("Su Su");
    // Hi ,Su Su
    
    example -2
    
    function numberPlus(a,b){
    console.log ( a + b );
    };
    numberPlus(2,5);
    // 7
    numberPlus(80,160);
    // 240

 
 ### ES6
 

    //example - 1
    
     const helloUser = (name) =>{
        console.log ( "Hello" + " ," + name);
        };
        
     helloUser("Mg Mg");
    // Hello ,Mg Mg
    
     helloUser("Su Su");
    // Hi ,Su Su
    
    //example -2
    
    const multipleNumber = (a,b)=>{
      console.log ( a * b );
    };
    
     multipleNumber(2,5);
    // 10
    
     multipleNumber(80,160);
    // 12800
##
### Return
 - function များ၏ လုပ်ဆောင်ချက်**ရလဒ် များကို ရယူအသုံးပြုနိုင်ရန်** return ဖြင့် ထုတ်ပြန်ပေးရသည်။
မှတ်ချက်။ ။ **console.log သည်** ရလဒ်ကို အသုံးပြုနိုင်ရန် ထုတ်ပေးခြင်း မဟုတ်ပါ။ <br>
**console ထဲတွင် log တစ်ခုအနေနဲ့ ပြသပေးရုံသာ** ဖြစ်သည်။
 
 - function များကို **return မပေးထားရင် Undefined** ဟူသော တန်ဖိုး
   ပြန်ထုတ်ပေးသည်။

 
  

     const toCheck = (a,b) => {
             return (a + b );
             }
             toCheck( 3, 4);
             //return 7

### Try this ....

    const cake = 5;
    const juice = 3;
    const orange = 1;
    const apple = 4;
    const dragon = 4000;
    const discountPrice = 1;
    let normalPrice;
    let toPay;
    
      const totalPrice = ( price1 , price2)=> {
       normalPrice = price1 + price2 ;
       return normalPrice;
     }
     totalPrice( cake , juice);
    
   
    const haveToPay = ( a , b) => {
     toPay = a - b ;
     return toPay ;
    };
    haveToPay(normalPrice,discountPrice);
    
    console.log ( "You Have to Pay " + toPay + "$"+" "+ "Total");

       
**totalPrice** function ခေါ််တဲ့အခါ  **parameter** တွေမှာ အခြားတန်ဖိုးတွေ ( orange , apple, dragon) အစားထိုးပြီး **console မှာ run** ကြည့်ပါ။
  ##
 
