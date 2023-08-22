## MSquare Programing Fullstack Course
### Batch 2
### Episode-*7*  
### Summary 
## Using NodeJS ( fs,http)
- အရင်ဆုံး မိမိ project မှာ node js ကို သုံးနိုင်ဖို့ node ကို init လုပ်ပေးရပါမယ်
- terminal မှာ မိမိ project ရှိတဲ့နေရာ ကို အရင်၀င်ပါ.
- ပြီးရင်အောက်က command ထည့်ပြီး enter ခေါက်ပါ
```console
npm init -y
```
- npm ကို သုံးပြီး node js ကို initialize လုပ်လိုက်တာဖြစ်ပါတယ်
- `-y` ကတော့ init လုပ်တဲ့အချိန် ထည့်ပေးရမယ့် info တွေကို default အတိုင်းပဲ  ထားခိုင်းလိုက်တာဖြစ်ပါတယ်
- init လုပ်ပြီးတဲ့အချိန် package.json ဆိုပြီး fileတစ်ခု project folder ထဲ ရောက်လာမှာဖြစ်ပါတယ်
```js
{
  "name": "nodejs-test",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1" 
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
 
}
```

##
### fs ( file system ) module
- nodejs မှာ build in ပါလာပြီးသား module တွေထဲက file တွေကို စီမံခန့်ခွဲနိုင်တဲ့ fs module ကို လေ့လာကြမှာဖြစ်ပါတယ်
- nodejs မှာ  အခြား developer တွေ ဖန်တီးထားတဲ့ package တွေကို npm နဲ့ install လုပ်ပြီး အသုံးပြုလို့ရပါတယ်
- nodejs မှာ ထည့်သွင်း/အသုံးပြုလို့ရတဲ့ package တွေကို node module လို့လဲခေါ်ပါတယ်
- fs ကို လေ့လာတဲ့အခါမှာ typescript နဲ့ ရေးမှာမလို့ typescript ကို  compile လုပ်စရာမလိုပဲ တစ်ခါတည်း run ပေးနိုင်တဲ့ ts-node-dev ဆိုတဲ့ module ကို install လုပ်ပြီး အသုံးပြုပါမယ်။

```console
npm i ts-node-dev
```
- install ပြီးတာနဲ့ project folder ထဲမှာ node_module folder နဲ့ package-lock.json ဆိုပြီး file တစ်ခု တိုးလာမှာဖြစ်ပါတယ်
- ဒါ့အပြင် package.json ထဲမှာလည်း `dependencies` ဆိုတဲ့ object တစ်ခုတိုးလာပြီး ခုနက ထည့်ထားတဲ့ ts-node-dev ကို ပြပေးနေမှာဖြစ်ပါတယ်
- node_modules folder မှာတော့ Dependencies မှာရှိတဲ့ module တွေရဲ့ data တွေကို သိမ်းထားပေးပြီး ဖိုင်တွေ အများကြီး ရှိတက်ပါတယ်
- package-lock.json ကတော့ Dependencies တွေ ချိတ်ဆက်ထားတဲ့ ဖိုင်ဖြစ်ပါတယ်
- node_module folder နဲ့ package-lock.json တွေကို ပြင်ဆင်ခြင်းမပြုလုပ်ပဲ ဒီအတိုင်းပဲ ထားပေးရပါမယ်
##
### Run NodeJS server
- node js server ကို run နိုင်ဖို့ နည်းလမ်းနှစ်မျိုးကို သုံးလို့ရပါတယ်
### node file-name.file-extension
- terminal မှာ `node script.ts` လို့ မိမိrun ချင်တဲ့ ဖိုင်ကို node server နဲ့ တစ်ခါတည်း run နိုင်ပါတယ်

### using script 
- နောက်တစ်နည်းကတော့ package.json file ထဲမှာ script တစ်ခု လုပ်ပြီး command နဲ့ run ခိုင်းတာဖြစ်ပါတယ်။
```json
{
  "name": "nodejs-test",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "ts-node-dev script.ts"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "ts-node-dev": "^2.0.0"
  }
}
```
- script object အောက်မှာ `"start": "ts-node-dev script.ts"` ဆိုတဲ့ script တစ်ခု ထည့်ပြီး ts-node-dev နဲ့ run ချင်တဲ့ ဖိုင်နာမည် ထည့်ပေးလိုက်တာဖြစ်ပါတယ်
- node server ကို run ချင်ရင် တော့ ***`npm start`*** ဆိုပြီး terminal မှာ ရိုက်ထည့်ပေးရမှာဖြစ်ပါတယ်
- node server ကို stop ( kill ) ချင်ရင်တော့ `Ctrl` + `c` ကို တွဲနှိပ်ပေးရမှာဖြစ်ပါတယ်
##
### Using *fs*
- file system module ဆိုတဲ့အတိုင်း file တွေ အသစ်လုပ်/ပြုပြင်/ဖတ်/ဖျက် ( CURD ) လုပ်လို့ရပါတယ်
- script.ts ဖိုင်တစ်ခု လုပ်ပါ
- fs ကို အသုံးပြုနိုင်ရန်အတွက် import လုပ်ပေးရပါမယ်
```js
//script.ts
import  fs  from  "fs";
```

### create file 
- syntax
```js
fs.writeFile("file-name.file-extension","content",function(err) {})
```
- example
```js
//script.ts
import  fs  from  "fs";

fs.writeFile("hello.txt", "Hello content!", function (err) {
  if (err) throw err;
  console.log("Saved!");
})
```
- npm start နဲ့ run ကြည့်လိုက်ပါက hello.txt ဆိုတဲ့ file တစ်ခု လုပ်ပေးမှာဖြစ်ပြီး terminal ထဲမှာ saved! ဆိုပြီး log တစ်ခု ထုတ်ပေးမှာဖြစ်ပါတယ်
> important note
> Nodejs မှာ console.log ထုတ်တဲ့အခါ browser console မှာ ပြပေးမှာမဟုတ်ပဲ backend console ဖြစ်တဲ့ terminal မှာ ပြပေးတာကို သတိပြုပါ

- ဆက်ပြီးတော့ fs module ရဲ့ အခြားmethod တွေကို စမ်းကြည့်ရအောင်

### update file 
- syntax
```js
fs.appendFile("file-name.file-extension","New content",function(err) {})
fs.writeFile("file-name.file-extension","content",function(err) {})
```
- example
```js
//script.ts
import  fs  from  "fs";

fs.appendFile("hello.txt", " This is my new content.", function (err) {
  if (err) throw err;
  console.log("Updated!");
});

fs.writeFile("hello.txt", "my replace context", function (err) {
  if (err) throw err;
  console.log("Saved!");
})
```
### read file 
- syntax
```js
fs.readFile("file-name.file-extension", function(err, data) {} )
```
- example
```js
//script.ts
import  fs  from  "fs";

fs.readFile("hello.txt", function (err, data) {
  if (err) throw err;
  console.log(data);//output buffer
});

fs.readFile("hello.txt", "utf8" ,function (err, data) {
  if (err) throw err;
  console.log(data);//output text
});

```
### delete file 
- syntax
```js
fs.unlink("file-name.file-extension", function(err) {} )
```
- example
```js
//script.ts
import  fs  from  "fs";

fs.unlink("hello.txt", function (err) {
  if (err) throw err;
  console.log("deleted");
});
```
##
### folder managment with fs
- fs module က file တင်မဟုတ်ပဲ folder တွေကိုလဲ စီမံခန့်ခွဲလို့ ရပါတယ်
###  create folder 
- syntax
```js
fs.mkdir("folder-name", function(err) {} )
```
- example
```js
//script.ts
import  fs  from  "fs";

fs.mkdir("hello", function (err) {
  if (err) throw err;
  console.log("Saved!");
});
```
###  rename folder 
- syntax
```js
fs.rename("old folder-name","new folder-name", function(err) {} )
```
- example
```js
//script.ts
import  fs  from  "fs";

fs.rename("hello", "world", function (err) {
  // if (err) throw err;
  console.log("Updated!");
});

```
###  read file in folder
- syntax
```js
fs.readdir("folder-name", function(err,files) {} )
```
- example
```js
//script.ts
import  fs  from  "fs";

fs.readdir("hello", (err, files) => {
  if (err) console.log(err);
  else {
    files.forEach((file) => {
      console.log(file);
    });
  }
});
```
###  delete folder 
- syntax
```js
fs.rmdir("folder-name", function(err) {} )
```
- example
```js
//script.ts
import  fs  from  "fs";

fs.rmdir("hello", (err) => {
  if (err) throw err;
  console.log("deleted");
});

```
### nodejs က http module ကို သုံးပြီး web server တစ်ခု တည်ဆောက်နည်း နဲ့ request လက်ခံနည်း/reponse send နည်းတွေအတွက် summary ကို နောက်သင်ခန်းစာတွင် ပေါင်းပြီး ကြည့်ရှုနိုင်ပါသည်
