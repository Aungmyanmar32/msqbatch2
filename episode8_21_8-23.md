## MSquare Programing Fullstack Course
### Batch 2
### Episode-*8*  
### Summary 
## Using NodeJS ( fs,http) part-2
### http and server
#### http ဆိုတာ *server( backend)*  နဲ့  *browser(frontend)* ကို ချိတ်ဆက်ပေးတဲ့ နည်းပညာ ဖြစ်ပါတယ်။ node js မှာ http ကို အသုံးပြုပြီး web server တစ်ခုကို ကိုယ်တိုင် တည်ဆောက်လို့ရပါတယ်။

#### server ဆိုတာက browser(frontend) မှာ ပြပေးမယ့် (ပြုလုပ်ချင်တဲ့)အချက်အလက်တွေကို စီမံခန့်ခွဲပေးတဲ့ နေရာ လို့ အလွယ်သတ်မှတ်နိုင်ပါတယ်။ 
ဆာဗာ ကို ၀င်ရောက် အသုံးပြုနိုင်ဖို့အတွက် 
- **လိပ်စာ( `IP address`)**
- **၀င်ပေါက် ( `port`)** လိုအပ်ပါတယ်။
>  **http `IP-address` `:` `port`

    http://127.0.0.1:3000 or http://localhost:3000

### serverကို ထို လိပ်စာနဲ့ ၀င်ပေါက်မှာ အသင့်စောင့်(`listen`) နေပြီး ၀င်လာသမျှ အချက်အလက် ( `request`) တွေကို လက်ခံကာ သက်ဆိုင်ရာ အချက်အလက်များကို ပြန်လည်( `response`) ထုတ်ပေးပါတယ်။
![enter image description here](https://github.com/Aungtat/MSquareFullstackCourseSummary/blob/main/server1.jpg?raw=true)
 ##
 ### Setup web-server ( http server )
 - server.ts ဖိုင်တစ်ခု လုပ်ပါမယ်
 ```js
 import  http  from  "http";
import  fs  from  "fs";

const  PORT  =  5000;

//create server
const server = http.createServer((request, response) => {
     //// your code here 
}

server.listen(PORT, () =>  console.log(" Server Running at PORT -", PORT));
```
> ရှင်းလင်းချက်
```js
 import  http  from  "http";
 import  fs  from  "fs";
```
- fs module နဲ့ http module ကို import လုပ်ထားပါတယ်
```js
const  PORT  =  5000;
```
- server အတွက် port တစ်ခု သတ်မှတ်ထားလိုက်ပါတယ်
```js
const server = http.createServer((request, response) => {
     //// your code here 
}
```
- http module က createServer method ကို သုံးပြီး  http web serverတစ်ခု  create  လုပ်လိုက်ပါတယ်
- createServer method က callback function တစ်ခုကို လက်ခံပြီး 
- အဲ့ဒီ function မှာတော့ **`request`** ( client ကနေ ၀င်လာတဲ့ request )  နဲ့ **`response`** ( server ကနေ ပြန်ထုတ်ပေးတဲ့ response ) ဆိုတဲ့ **parameter နှစ်ခုကို လက်ခံ**မှာဖြစ်ပါတယ်

```js
server.listen(PORT, () =>  console.log(" Server Running at PORT -", PORT));
```
- server ကို port 5000 မှာ listen လုပ်ထားပေးရမှာ ဖြစ်ပါတယ်
- listen method ရဲ့ callback function ထဲမှာတော့ server run နေကြောင်း log တစ်ခု ထုတ်ပေးထားလိုက်တာပါ
- ခု http server ကို run ဖို့ အတွက် `package.json` file မှာ script ကို သွားပြင်ပေးရပါမယ်
```js
// package.json

{
  "name": "nodejs-test",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "ts-node-dev server.ts"//update to server.ts
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "ts-node-dev": "^2.0.0"
  }
}

```
- ခု server ကို npm start နဲ့ run ကြည့်ပါက terminal မှာ အောက်ကပုံလို ပြပေးမှာဖြစ်ပါတယ်
![enter image description here](https://cdn.discordapp.com/attachments/1076154921562411008/1143483069475590164/image.png)
##
### http request method
- http request တွေမှာ အသုံးများတဲ့ METHOD တွေ ကတော့
   - POST 
   - PUT
   - GET
   - DELETE
- ဖြစ်ကြပါတယ်
### method တွေကို စာလုံးအကြီးတွေနဲ့ အထက်ပါအတိုင်း ရေးပေးရမှာဖြစ်ပါတယ်
 - POST  က data တွေ *C*reate လုပ်တဲ့အခါ သုံးလေ့ရှိပါတယ်
  - PUT က data တွေ *U*pdate လုပ်တဲ့အခါ သုံးလေ့ရှိပါတယ်
  - GET က data တွေ *R*ead လုပ်တဲ့အခါ သုံးလေ့ရှိပါတယ်
  - DELETE က data တွေ *D*elete လုပ်တဲ့အခါ သုံးလေ့ရှိပါတယ်

##
### Test http request and response
- node js မှာ http server ကနေ frontend က request တွေကို လက်ခံပြီး  response ပြန်လုပ်တာ စမ်းကြည့်ရအောင်

 ```js
 //server.ts
 
 import  http  from  "http";
import  fs  from  "fs";

const  PORT  =  5000;

//create server
const server = http.createServer((request, response) => {
     const method = request.method
     console.log(method)
     response.write(method)
     response.end()
}

server.listen(PORT, () =>  console.log(" Server Running at PORT -", PORT));
```
- အထက်ပါ http server ကို postman ကို အသုံးပြုပြီး request ပို့ကြည့်ပါမယ်
![enter image description here](https://raw.githubusercontent.com/Aungmyanmar32/msquare-fullstack-m2/main/ep20-1-1.jpg)

- postman က method နေရာမှာ POST ကို ရွေးပေးပါ
- url နေရာမှာ မိမိ web server ဖြစ်တဲ့ `http://localhost:5000` ကို ထည့်ပေးပါ
- ပြီးရင် send ကို နှိပ်ပြီး SERVER ဆီ request ပို့ကြည့်လိုက်တဲ့အခါ server ကပြန်လာတဲ့ reponse ကို output မှာ ပြပေးမှာဖြစ်ပါတယ်။

![enter image description here](https://cdn.discordapp.com/attachments/1076154921562411008/1143488769811550218/image.png)
- postman က method တွေကို ပြောင်းပြီး request ပို့စမ်းကြည့်ပါက ပို့လိုက်တဲ့ request အတိုင်း response ပြန်လာတာကို မြင်ရမှာဖြစ်ပါတယ်
##
### Response html file when request method is "GET"
- frontend ကနေ GET method နဲ့ request ၀င်လာတဲ့အခါ html file တစ်ခု ကို ဖတ်ပြီး response ပြန်ပေးလိုက်တာကို လေ့လာကြည့်ရအောင်။
- အရင်ဆုံး index.html ဖိုင်တစ်ခုလုပ်လိုက်ပါမယ်
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    <h1>Html response ......</h1>
  </body>
</html>
```
- ပြီးရင် http server မှာ request ကို လက်ခံတဲ့အခါ အဲ့ဒီ index.html ဖိုင်ကို ဖတ်ပြီး reponse ပြန်ပေးလိုက်ပါမယ်

 ```js
 //server.ts
 
 import  http  from  "http";
import  fs  from  "fs";

const  PORT  =  5000;

//create server
const server = http.createServer((request, response) => {
     const method = request.method
     console.log(method)
      
    if (method === "GET") {
    //GET method + fs read file-->response html
      fs.readFile("index.html", function (err, data) {
       response.writeHead(200, { "Content-Type": "text/html" });
       response.write(data);
       return response.end();
    });
  }
}

server.listen(PORT, () =>  console.log(" Server Running at PORT -", PORT));
```
- ရှင်းလင်းချက်
```js
  if (method === "GET") {
    //GET method + fs read file-->response html
    fs.readFile("index.html", function (err, data) {
      response.writeHead(200, { "Content-Type": "text/html" });
      response.write(data);
      return response.end();
    });
  }
```
- method က GET ဖြစ်ခဲ့ရင်
- fs readFile method ကို သုံးပြီး index.html ကို ဖတ် လိုက်ပါတယ်
-  `response.writeHead(200, { "Content-Type": "text/html" });` မှာ
   - ပထမ parameter က http status code ဖြစ်ပြီး request ၀င်လာတာကို server မှာ လက်ခံတဲ့ အခြေအနေကို ဖော်ပြပေးတဲ့ code ဖြစ်ပါတယ် 
   - ဒုတိယ parameter ကတော့ response ပြန်ပေးတဲ့ file အမျိုးအစား ကို reponse ကို လက်ခံမယ့် client ( browser ) ကို ပြောပြပေးထားတာဖြစ်ပါတယ်
- `response.write(data);` ကတော့ readFile လုပ်ပြီး ထွက်လာတဲ့ data တွေကို response body ထဲကို write လုပ်လိုက်တာဖြစ်ပါတယ်
- `return response.end();` ဒါကတော့ request ကို လက်ခံပြီး response ပြန်ပေးခြင်း ပြီးဆုံးပြီ ဖြစ်ကြောင်း သတ်မှတ်လိုက်တာပါ
- node js server မှာ response ပြန်တဲ့ လုပ်ငန်းစဥ် ပြီးဆုံးတိုင်း response.end() method ကို မဖြစ်မနေ ထည့်ရေးပေးရမှာဖြစ်ပါတယ်
- ခု postman ကနေ GET method နဲ့ request send ကြည့်ပါမယ်
![enter image description here](https://cdn.discordapp.com/attachments/1076154921562411008/1143494889452085288/image.png)
- html ထဲမှာ ရေးထားတဲ့ h1 tag ကို response ပြန်ပေးတာကို မြင်ရမှာဖြစ်ပါတယ်။
