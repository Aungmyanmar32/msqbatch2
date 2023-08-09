## MSquare Programming Fullsatck Developer Course ( Batch 2)
### Episode 3 Summary
##
- callback function
- object and array
- usefull built-in method ( array)
   - length
   - indexof
  - find
  - filter
  - map
  - reduce
- logical operators
  -  `||` 
  - `&&` 
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
  ### Array
  object ကဲ့သို့ data တွေ valueတွေ အများကြီးပါတဲ့ variable တစ်ခုလိုပါပဲ<br>
   #### Declare Syntax

 `const` `array-name` = `['A','B','C','D','E','F' `] ;

ခေါ် တဲ့အခါကျရင် object မှာကဲ့သို့ (**Dot** Notation) (**Bracket** Notation) တွေ မသုံးပဲ index နဲ့ အသုံးပြုရမှာပါ။
### Index သတ်မှတ်နည်း
![enter image description here](https://miro.medium.com/max/640/1*hLp5vDQse-YUOpvgI9hfDw.webp)

 #### Usage syntax
 ‌`array-name`[`index`];
 #### example
 
```js
    const myLetter =['A','B','C','D','E','F' ] ;
     const getLetter = myLetter[0];
     console.log(getLetter);
     //'A'
     const getLetter2 = myLetter[4];
     console.log(getLetter2);
     //'E'
```
### Important Note

- **Array ** မှာ ရှိတဲ့ items တွေ ဘယ်လောက်ပါလဲ သိချင်ရင် **`.length`** ကို သုံးပါတယ်။     
     - **`index` ကို** စရေရင် **0** နဲ့ စပြီး , **`length` ကို 1** နဲ့ စပါတယ်။
     - array တစ်ခုရဲ့ **နောက်ဆုံး`index`** ကို လိုချင်ရင် **‌array.length -1** နဲ့ ရယူနိုင်ပါတယ်။
     
###
```js
    const myLetter =['A','B','C','D','E','F' ] ; 
     //   index       0   1   2   3   4   5
     //   length      1   2   3   4   5   6
    const getLength = myLetter.length; 
    const lastIndex =myLetter.length - 1 ;
    console.log(getLength);
    console.log(lastIndex) 
    //6
    //5 
```
  
  
##
### Object in Array
#### syntax
```js
    const myUser = [
                    "Bo Bo",
                    { 
                      name : "ko ko",
                      age : 35
                     },
                      "27"
                    ];
```   
```js 
    // to get ko ko and his age
    const userName = myUser[1].name;
    const userAge = myUser[1].age;
    console.log(userName + " is " +userAge + " years old");
    // ko ko is 35 years old
    
    //get Bo Bo and his age
    const userName2 = myUser[0];
    const userAge2 = myUser[2];
    console.log(userName2 + " is " +userAge2 + " years old");
    // Bo Bo is 27 years old
    
 ```                     
   ##
    ### Callback function
 Function တစ်ခုထဲ နောက် function တစ်ခုကို parameter အဖြစ် ထည့်ပေးလိုက်ရင် ထို အထည့်ခံ function ကို callback function လို့ ခေါ် ပါတယ်။<br>
 **ပထမ function အလုပ်လုပ်ပြီးမှ နောက် functionတစ်ခုကို အလုပ်ခိုင်းချင်တဲ့အခါ callback အနေနဲ့ သုံးလေ့ရှိပါတယ်။**
 
 #### Example..1
 
```js
    const functionOne = (parameter) => {
     console.log("I am not callback");
     parameter();
     };
     
     const functionTwo = ()=>{
      console.log("I am callback");
      };
       functionOne(functionTwo);
      //functionOneအရင်လုပ်ပါ(functionOneပြီးမှ functionTwoနောက်ကလုပ်ပါ=callback)
      
      //output 1: I am not callback
      //output 2: I am callback
```
#### Example..2
```js
    const functionOne = (parameter)=> { 
       console.log("I am not callback");
       parameter(1,2); 
       }; 
     const functionTwo = (a,b)=>{ 
        console.log(a+b); 
       }; 
       functionOne(functionTwo);
       //output 1: "I am not callback"
       //output 2: 3
```
##

 - **callback function  ခေါ်** တဲ့ အခါ  သိမ်းထားတဲ့ **variable name ကိုသာ ရေး**ပေးရမည်။<br>
 `functionOne(functionTwo)` **YES**<br>
 ~~`functionOne(functionTwo())`~~ **NO**
##

##
### Built-in method
- javascript မှာ အသင့်လုပ်ပြီးသား method တွေ ပါရှိပါတယ်
- method ဆိုတာကလဲ function ပဲ ဖြစ်တာမလို့ သတ်မှတ်စရာမလိုပဲ တိုက်ရိုက်ခေါ်သုံးလို့ရပါတယ်
- array မှာ သုံးလို့ရမယ့် အသုံးများတဲ့ method တွေကို ပြန်လေ့လာကြည့်ကြပါမယ်
- method တွေကို သုံးတဲ့အခါ ဘာကို return ပြန်ထုတ်ပေးသလဲဆိုတာကို သိထားဖို့က အရေးကြီးပါတယ်
##
### `array.find()` method
- array တစ်ခုထဲရှိ item ( element) တွေထဲက item တစ်ခုကို ရှာချင်တဲ့အခါ သုံးလေ့ရှိပါတယ်
- မူရင်းarray ရှိ item များကို loop လုပ်ကာ **သတ်မှတ်ချက်နှင့်အညီ စစ်ဆေးပြီး မှန်ကန်မှုရှိတဲ့( true) item တစ်ခုကိုသာ ‌**ရွေးချယ်ပြီး  ပြန်ထုတ်ပေးသည်။<br>**မူရင်း array ရှိ item များ အားလုံး ပြောင်းလဲခြင်းမရှိပါ**
### syntax
```js
array.find(callback-function)
```
### Example
```js
const users = [
    {
     name: "aung",
     age: 30
    },
     {
     name: "thura",
     age: 30
    },
     {
     name: "ezio",
     age: 30
    },
]

const findInArray = users.find((item) => item.name === "thura" )
console.log(findInArray )
//log  { name: "thura", age: 30 }
```
- find method နဲ့ ရှာတဲ့အခါ callback function မှာ စစ်ထားတဲ့ codition က true ထုတ်ပေးတဲ့ item ကို return လုပ်ပေးမှာဖြစ်ပါတယ်
- callback function မှာ စစ်ထားတဲ့ codition က true ထုတ်ပေးတဲ့ item ကို တွေ့ပြီး ဆိုရင် array ထဲရှိ ကျန် item တွေကို loop ထပ်မလုပ်တော့ပဲ ထွက်သွားလိုက်မှာဖြစ်ပါတယ်
- ဆိုလိုတာက find method ကို သုံးလို့ရှိရင် callback မှာ return true ဖြစ်နိုင်တဲ့ item တွေ အများရှိနေပေမယ့် ပထဆုံး true ဖြစ်တဲ့ item ကနေပဲ loop ကို ရပ်နားလိုက်မှာဖြစ်ပါတယ်
- အထက်ပါ users array မှာ age 30 ဖြစ်တဲ့ user ကို ရှာမယ်ဆိုပါစို့
- user အကုန်လုံး က age 30 ဖြစ်တာမလို့ အကုန်လုံးကို return လုပ်ပေးမှာမဟုတ်ပဲ ပထမ item ဖြစ်တဲ့ `{ name: "aung", age: 30 }` ကို သာ return လုပ်ပေးမှာဖြစ်ပါတယ်။
### Example
```js
const users = [
    {
     name: "aung",
     age: 30
    },
     {
     name: "thura",
     age: 30
    },
     {
     name: "ezio",
     age: 30
    },
]

const findInArray2 = users.find((item) => item.age === 30 )
console.log(findInArray2 )
//log  { name: "aung", age: 30 }
```
- callback function မှာ စစ်ထားတဲ့ codition က true ထုတ်ပေးတဲ့ item မရှိခဲ့ရင်တော့ undefined ကို return ပြန်ပေးမှာဖြစ်ပါတယ်
- ### Example
```js
const users = [
    {
     name: "aung",
     age: 30
    },
     {
     name: "thura",
     age: 30
    },
     {
     name: "ezio",
     age: 30
    },
]

const findInArray2 = users.find((item) => item.name === "okar")
console.log(findInArray2 )
//log  undefined
```

 ### `array.filter()` method
 မူရင်းarray ရှိ item များကို loop လုပ်ကာ **သတ်မှတ်ချက်နှင့်အညီ စစ်ဆေးပြီး မှန်ကန်မှုရှိတဲ့( true) item များကိုသာ ‌**ရွေးချယ်ပြီး **array  အသစ်တစ်ခု** ပြန်ထုတ်ပေးသည်။<br>**မူရင်း array ရှိ item များ အားလုံး ပြောင်းလဲခြင်းမရှိပါ**
 #### syntax
 `array`.`filter`(`callback function`);

```js
const  arryItem = [
{
 name :  "Aung AUng",
 age :  34
},
{
 name :  "Ko Ko",
 age :  24
},
{
 name :  "Su Su",
 age :  "18"
},
{
 name :  "Bo Bo",
 age :  23
},
{
 name :  "Mg Mg",
 age :  26
}
];

const  youngPeople = arryItem.filter((user)=> {
 return  user.age < 25;
 //အသက် ၂၅ အောက်ကိုသာ ‌စစ်ထုတ်ပေးပါ
 });

console.log(youngPeople);

// (3) [{…}, {…}, {…}]
// 0: {name: 'Ko Ko', age: 24}
// 1: {name: 'Su Su', age: '18'}
// 2: {name: 'Bo Bo', age: 23}
```
##


### array.map() method
**မူရင်း array ရှိ item များ အားလုံးကို loopလုပ်**ပေးပြီး **array အသစ်တစ်ခု** ထုတ်ပေးသည်။<br>**မူရင်း array ရှိ item များ အားလုံး ပြောင်းလဲခြင်းမရှိပါ**
#### Syntax
`array`.`map`(`callback-function`) ;<br>

`array`.`map`((`parameter`)`=>` `{`
do something
`}` 
`)` ;<br>
#### short-hand
`array`.`map`((`parameter`)`=> `do something`) `;<br>
#### Example..1
```js
    const itemsArr = [1,2,3,4,5];
    const newArr =itemsArray.map((element) => element + 1);
      console.log(newArr);
      //output : (5) [2, 3, 4, 5, 6]
      //auto return in short-hand syntax
```
#### Example..2
```js
    const itemsArr = [1,2,3,4,5];
    const newArr = itemsArr.map((element) =>{
     const newItem = element+1;
     return newItem;
     });
     console.log(newArr);
     //(5) [2, 3, 4, 5, 6]
     
     //if no return
     const newArr = itemsArr.map((element) =>{
     const newItem = element+1;
     });
     console.log(newArr)
     //(5) [undefined,undefined,undefined,undefined,undefined]
 ```    
 ##
 အပေါ် က filter method ထဲက လို ၂၅ အောက်တွေကိုပဲ စစ်ထုတ်ပြီး သူတို့ နာမည်ပဲ လိုချင်တယ် ဆိုပါစို့။
```js
const  arryItem = [
{
 name :  "Aung AUng",
 age :  34
},
{
 name :  "Ko Ko",
 age :  24
},
{
 name :  "Su Su",
 age :  "18"
},
{
 name :  "Bo Bo",
 age :  23
},
{
 name :  "Mg Mg",
 age :  26
}
];

//method 1
//filter people under 25
 const  youngPeople = arryItem.filter((user)=>user.age < 25);

//get name of under 25
 const getName = youngPeople.map((people,index) =>people.name);
 console.log(getName);
//(3) ['Ko Ko', 'Su Su', 'Bo Bo']
 
 //////
 //method 2
 // အပေါ်ကလို တစ်ခုစီခွဲမရေးပဲ တစ်ခါတည်း တွဲရေးလိုက်လို့လဲရပါတယ်
 const getYoungPeopleName =  arryItem.filter((user)=>user.age < 25).map((people) =>people.name);
  console.log(getYoungPeopleName);
//(3) ['Ko Ko', 'Su Su', 'Bo Bo']

```
 `array.map((element, index) => run this);` <br>
`map()` /`filter()` / `find()` method မှာ **callback function** က **parameter နှစ်ခု** လက်ခံလို့ရပြီး<br>**ပထမ parameter** က **element** ကို ခေါ် မှာ ဖြစ်ပြီး <br>**ဒုတိယ parameter**က ထို **element ရဲ့ index** တန်ဖိုး ဖြစ်ပါတယ်။ <br>နမူနာ ပြထားတဲ့ code မှာတော့ <br>`youngPeople.map((people,index) =>people.name);` <br>index ကို parameter အဖြစ်သာ ထည့်ထားပြီး callback function မှာ ဘာလုပ်ဆောင်ချက်မှ မပေးထားပါဘူး။<br>
 ဒီလိုလေး စမ်းကြည့်ပါ
```js
const getName = youngPeople.map((people,index) => index + " "+people.name);
 console.log(getName);     
 //output : (3) ['0 Ko Ko', '1 Su Su', '2 Bo Bo']
 ```
    
 **index နေရာမှာ တခြားစာလုံးဖြစ်လဲ index တန်ဖိုးကိုပဲ ထုတ်ပေးမှာပါ** 
```js
const getname2 = youngPeople.map((people,spiderMan) => spiderMan+ " " + people.name); 
console.log(getName2); 
//output : (3) ['0 Ko Ko', '1 Su Su', '2 Bo Bo']
```
##
### Logical Operators
<table border="0"><tbody><tr><th>Operator</th>
				<th>Description</th>
				<th>Example</th>
			</tr><tr><td><code>&amp;&amp;</code></td>
				<td><strong>Logical AND</strong>: နှစ်ဖက်လုံးမှန်ရင် true ။<br>တစ်ဖက်ပဲမှန် (သို့) နှစ်ဖက်လုံးမှားရင် false</td>
				<td><code>x &amp;&amp; y</code></td>
			</tr><tr><td><code>||</code></td>
				<td><strong>Logical OR</strong>: နှစ်ဖက်လုံးမှန် (သို့) တစ်ဖက်ပဲမှန် true ။ <br> နှစ်ဖက်လုံးမှား false</td>
				<td><code>x || y</code></td>
			</tr><tr><td><code>!</code></td>
				<td><strong>Logical NOT</strong>:  ပြောင်းပြန်စစ်ပေးတာပါ
				<br> !( 1 === 1 )  flase 
				<br> ! ( 1 === 2 ) true</td>
				<td><code>!( x = y)</code></td>
			</tr></tbody></table>

## 
