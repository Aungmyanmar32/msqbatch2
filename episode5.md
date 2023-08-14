## MSquare Programing Fullstack Course
### Batch 2
### Episode-*5*  
### Summary 
##
### Typescript

- Typescript ဆိုတာ javascript superset တစ်မျိုးဖြစ်ပါတယ်။
- Typescript သည် စောစောစီးစီး အမှားထောက်လှမ်းခြင်း နှင့်ခိုင်မာသောTyping တို့အပါအဝင် runtime အမှားများကို ရှာဖွေရန် အသုံး၀င်သည်။
- 
##
### Install Typescript in Project
- Project folder တစ်ခုဆောက်ပြီး npm ကို ထည့်းပေးထားပါ
- terminal ထဲ `npm i -D typescript` လို့ ရေးပြီး typescript ကို install လုပ်ပေးပါ။ 
- Typescript ဖိုင် များသည် `.ts` ဖြင့် file extension သတ်မှတ်ပေးရပါသည်( eg: script.ts )
- Typescript ဖြင့် ရေးသားထားသော  ဖိုင်များကို မူလ  language file အဖြစ် သို့ ပြန်ပြောင်းပေးရပါတယ် ( ဥပမာ ။    ။ javascript  ကို  typescript ဖြင့်ရေးထားပြီး `.ts` extension နဲ့ သိမ်းထားတာကို  **typescript မှ javascript** `.js` ဖိုင် အဖြစ်သို့ **tsc ကို သုံးပြီး**  ပြန်ပြောင်းပေးရမှာပါ )
- 

## Typescript Data type
- နောက်ပိုင်း သင်ခန်းစာမှာ typescript ကို ( ts ) လို့ပဲ သုံးသွားပါမယ်
- ts မှာ variable ကြေငြာရင် data-type သတ်မှတ်ပေးရပါတယ်။
- script.ts ဖိုင် တစ်ခုလုပ်ပါမယ်
- ဖိုင်ထဲမှာ variable တွေ လုပ်ကြည့်ပါမယ်
```ts
const  num1: number = 20;

const  userName: string = "aung aung";

const  isOk: boolean = true;
```
- num1 ရဲ့ တန်ဖိုး က number ဖြစ်ကြောင်း သတ်မှတ်ထားလိုက်တာဖြစ်ပါတယ်
- ခုလို သတ်မှတ်လိုက်တာနဲ့ num1 တန်ဖိုးက number data type မှ လွဲပြီး ကျန် string တို့ undefended  တို့  ဖြစ်လာရင် error ပြပေးမှာဖြစ်ပါတယ်။
- သတ်မှတ်ထားတဲ့  data type နဲ့ variable တန်ဖိုးတို့ type တူမှသာ 
error ပျောက်သွားမှာဖြစ်ပါတယ်။
- ကျန်တဲ့ userName / isOk တွေလည်း ထိုနည်းအတိုင်းပါပဲ။

## 
### Typescript complier ( tsc )
- Typescript ဖြင့် ရေးသားထားသော  ဖိုင်များကို မူလ  language file အဖြစ် သို့ ပြန်ပြောင်းပေးရပါတယ် ( ဥပမာ ။    ။ javascript  ကို  typescript ဖြင့်ရေးထားပြီး `.ts` extension နဲ့ သိမ်းထားတာကို  **typescript မှ javascript** `.js` ဖိုင် အဖြစ်သို့ **tsc ကို သုံးပြီး**  ပြန်ပြောင်းပေးရမှာပါ )

```properties
tsc script.ts
```
- ဒီ command  က **script.ts** ဖိုင် ဖတ်ပြီးကို  **typescript ကို javascript အဖြစ် ပြန်ပြောင်း**ကာ **script.js ဖိုင်တစ်ခု create လုပ်**ပေးလိုက်ပါတယ်
- vs code  explorer ထဲမှာ script.js ဖိုင်တစ်ခုတိုးလာတာမြင်ရမှာပါ။
> tsc ကို သုံးမရပါက **npm နဲ့ global install** လုပ်ပေးပါ 
##
### function 
- ts မှာ function သတ်မှတ်တဲ့ အခါ parameter နောက်မှာ return ပြန်မယ့် data type ကို သတ်မှတ်ပေးရပါမယ်
- function မှာ လဲ  return  တစ်ခု ပြန်ပေးရပါမယ်
```ts
const  num1: number = 20;
const  userName: string = "aung aung";
const  isOk: boolean = true;

const  testFunction = ():number  => {
return  num1
}
```

---
### Function with parameter
```ts
const testFunction = (num2: number , num3: number):number => {
 return num2 * num3
}

testFunction(2,12)
```
- function ထဲမှာ parameter လက်ခံရင် အဆိုပါ parameter ကိုပါ  data type သတ်မှတ်ပေးရပါတယ်။
##
### Interface ( typescript object)
-  အထက် က function က parameter မှာ data type  တွေကို Interface ထဲမှာ ကြိုတင်ကြေငြာပြီး အသုံးပြုလို့ရပါတယ်။
```ts
interface Props {
  num2: number;
  num3: number;
}
const testFunction = (params: Props): number => {
  return params.num2 * params.num3;
};

testFunction({ num2: 2, num3: 12 });
```
- parameter ထဲမှာ Props ဆိုတဲ့ object ကို ထည့်ပေးလိုက်ပြီး
- Props object ထဲမှာ ပါလာတဲ့ key တွေကို parameters အဖြစ်ယူသုံးလိုက်တာပါ
- function ကို ခေါ်တဲ့အခါမှာတော့ parameter မှာ object  အနေတဲ့ ပြန်ခေါ်ပေးရပါမယ်
- ---
- testFunction ထဲမှာ params.num2 / num3 ဆိုပြီး ခေါ်သုံးလို့ရသလို object Destructuring လုပ်ပြီးလဲ သုံးလို့ရပါတယ်
```ts
interface Props {
  num2: number;
  num3: number;
}
const testFunction = ({num2,num3}: Props): number => {
  return num2 * num3;
};

testFunction({ num2: 2, num3: 12 });
```
##
### optional ( ? ) 
- parameter တစ်ခုကို interface ထဲမှာ ? ထည့်ရေးပေးခြင်းဖြင့် optional သတ်မှတ်နိုင်ပါတယ်
- optional parameter က function ခေါ်တဲ့အချိန် ထည့်လဲရ မထည့်လဲ အဆင်ပြေအောင် သတ်မှတ်လိုက်တာပါ
- optional သတ်မှတ်လိုတဲ့ parameter name အနောက်မှာ ? ထည့်ပေးရပါမယ်


```ts
interface Props {
  num2: number;
  num3?: number;
  
}
const testFunction = ({num2,num3}: Props): number => {
  if(num3){
   return num2 * num3;
  }
   return num2;
 
};

testFunction({ num2: 2 });
```
- function ကို ခေါ်တဲ့ အခါ num2 (parameter ) တစ်ခုပဲ ထည့်ပေးလိုက်ပေမယ့်လည်း error မပြတာ မြင်ရမှာဖြစ်ပါတယ်
- num3 က optional လို့ သတ်မှတ်ထားတဲ့ အတွက် ဖြစ်ပါတယ်။
##
### Use object as parameter
```ts
interface Props {
  num2: number;
  num3?: number;
  user: { name: string; age: number; email: string };
}
const testFunction = ({ num2, num3, user }: Props): number => {
  if (num3) {
    return num2 * num3;
  } 
    console.log(user.email);
    return num2;
  
};

testFunction({
  num2: 2,
  user: { name: "Aung", age: 27, email: "aung@web.de" },
});
```
- user object တစ်ခုကို Props interface ထဲမှာ data type သတ်မှတ်ထားလိုက်ပြီး testFunction ထဲ parameter အနေနဲ့ Props object တစ်ခုလုံး ယူသုံးထားပါတယ်
- testFunction ခေါ်တဲ့အခါ user parameter က  object မလို့ အထက်ကအတိုင်း ခေါ်ပေးရမှာ ဖြစ်ပါတယ်။
##

### Use interface in other interface
- အထက်က လေ့လာခဲ့တဲ့ နမူနာလို interface ထဲ မှာ user object တစ်ခုလုံးကို မထည့်ပဲ သီးသန့် interface သပ်သပ်လုပ်ပြီး Props interface မှာ ပြန်ခေါ်သုံးလို့ရပါတယ်။
```ts
interface User {
  name: string;
  age: number;
  email: string;
}
interface Props {
  num2: number;
  num3?: number;
  user: User;
}
const testFunction = ({ num2, num3, user }: Props): number => {
  if (num3) {
    return num2 * num3;
  } 
    console.log(user.email);
    return num2;
  
};

testFunction({
  num2: 2,
  user: { name: "Aung", age: 27, email: "aung@web.de" },
});
```
##
### Array parameter
```ts
interface User {
  name: string;
  age: number;
  email: string;
}
interface Props {
  num2: number;
  num3?: number;
  users: User[];
}
const testFunction = ({ num2, num3, users }: Props): number => {
  if (num3) {
    return num2 * num3;
  }
  const userEmail = users.map((user) => {
    return user.email;
  });
  console.log(userEmail);
  return num2;
};

testFunction({
  num2: 2,
  users: [
    { name: "Aung", age: 27, email: "aung@web.de" },
    { name: "KoKo", age: 27, email: "koko@web.de" },
  ],
});
```
- Props interface ထဲမှာ User interface ကို ခေါ်သုံးတဲ့အခါ `[ ]` နဲ့ array   အဖြစ် သတ်မှတ်လိုက်တာပါ။
- testFunction ထဲမှာ map () ကို သုံးပြီး user တွေရဲ့ email များကို log ထုတ်ထားပါတယ်။
- testFunction ကို ခေါ်တဲ့အခါ users parameter မှာ array အနေနဲ့ ထည့်ခေါ်ပေးရမှာပါ။
## 
### function parameter
```ts
interface User {
  name: string;
  age: number;
  email: string;
}

interface Props {
  num2: number;
  num3?: number;
  users: User[];
  fetchData: (url: string) => void;
}

const fetchData = (url: string) => {
  console.log(url);
};

const testFunction = ({ num2, num3, users,fetchData }: Props): number => {
  if (num3) {
    return num2 * num3;
  }

  const userEmail = users.map((user) => {
    return user.email;
  });

  console.log(userEmail);

  fetchData("http://localhost:3000");

  return num2;
};

testFunction({
  num2: 2,
  users: [
    { name: "Aung", age: 27, email: "aung@web.de" },
    { name: "KoKo", age: 27, email: "koko@web.de" },
  ],
  fetchData,
});

```
- fetchData ဆိုတဲ့ function ကို Props interface ထဲ ထည့်ပေးထားပါတယ်
- testFunction  မှာ Props interface ကို parameter အဖြစ်လက်ခံထားပြီး Props ထဲမှ value တွေကို လှမ်းယူထားပါတယ်
- testFunction ကို ခေါ်တဲ့အခါ fetchData parameter ရဲ့  တန်ဖိုးက fetchData ဖြစ်ပါတယ်လို့ ပေးထားလိုက်တာပါ  ။`fetchData : fetchData`  ကို  `fetchData` လို့ပဲ ရေးလို့ရတာကို သတိပြုပါ။
##
