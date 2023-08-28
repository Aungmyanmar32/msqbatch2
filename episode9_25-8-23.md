## MSquare Programing Fullstack Course
### Batch 2
### Episode-*9*  Summary
- tsconfig.json
- file upload ( nodeJs + chat gpt )
##
### tsconfig.json
- nodeJS server တစ်ခုကို တည်ဆောက်တဲ့အခါ ts ကို အသုံးပြုပြီး ရေးတဲ့အချိန် error ပြနေတာတွေ ၊ suggestion မရတာတွေကို ဖြေရှင်းဖို့ tsconfig.json ကို အသုံးပြုနိုင်ပါတယ်။
- မိမိ project ထဲမှာ tsconfig.json ဖိုင်တစ်ခုလုပ်ပြီး အောက်ပါ code တွေကို ထည့်ပေးလိုက်ရင် အဆင်ပြေပါပြီး
- 
![enter image description here](https://cdn.discordapp.com/attachments/1076154921562411008/1145571140719366215/image.png)

```js
// tsconfig.json
{

  "compilerOptions": {

    "esModuleInterop": true

  }

}
```
## File upload with nodeJS + chat GPT 
- ဆက်ပြီး chat gpt ကို သုံးပြီး nodejs နဲ့ file upload လို့ ရမယ့် project တစ်ခု ရေးကြည့်ကြပါမယ်
- https://chat.openai.com/ မှာ register လုပ်လိုက်ပါ
- file upload လုပ်လို့ရမယ့် http server တစ်ခု တည်ဆောက်ခိုင်းပြီး postman ကနေ request လုပ်ကြည့်ပါ။
### example
```
// step 1
create http server with file upload using Node.js without any external libraries
```
![enter image description here](https://cdn.discordapp.com/attachments/1076154921562411008/1145595370651533382/image.png)
```js
//step 2
how to send request from postman
```
##
### request object
- request တစ်ခု လုပ်လိုက်တဲ့ အခါ server မှာ requset လက်ခံတဲ့အခါ request object တစ်ခု ရလာမှာဖြစ်ပါတယ်
- server မှာ request ကို console log ထုတ်ပြီး ကြည့်နိုင်ပါတယ်
### request object မှာ dataတွေ အများကြီး ပါလာတာကို မြင်ရမှာ ဖြစ်ပါတယ်
- အဲ့ဒီ data တွေထဲကမှ အသုံးပြုမှုများတဲ့ ဟာတွေကတော့
  - request.headers
  - request.body
  - request.method
  - request.url
  - request.on
- စတာတွေ ဖြစ်ပါတယ်
### request.headers
- request.headers မှာတော့ request body မှာ ပါလာတဲ့ data နဲ့ သက်ဆိုင်တဲ့ information တွေကို request လက်ခံတဲ့ server ကို အသိပေးထားတာဖြစ်ပါတယ်
### example
![enter image description here](https://www.soapui.org/soapui/media/images/functional_testing/headers/raw_new_1.png)
- Content-Type မှာဆိုရင် request body မှာထည့်ပေးလိုက်တဲ့ content အမျိုးအစား က text ဖိုင်အမျိုးအစားဖြစ်ကြောင်း server ဆီကို ပြောပြထားတာဖြစ်ပါတယ်။
##
### request.body
- request.body ထဲမှာတော့ server ဆီ ပို့ချင်တဲ့ data ကို ထည့်ပေးထားတာဖြစ်ပါတယ်
##
### request.method
- ၀င်လာတဲ့ request method ကို လှမ်းယူလို့ရပါတယ်
##
### request.url
- ၀င်လာတဲ့ request url ( route ) ကို လှမ်းယူလို့ရပါတယ်
##
### request.on
- request.on ကတော့ request body ထဲက data ကို လက်ခံတဲ့အချိန်မှာ အသုံးပြုတာဖြစ်ပါတယ်
