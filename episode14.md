## MSquare Programming Fullstack Course

### Batch 2

### Episode-_14_ Summary

## CURD web-application with react and express(part-1)

### Prepare for request to server(backend) ("POST" method)

- mui text field တွေ နဲ့ menu state တစ်ခုကို Update လုပ်ပြီး log ထုတ်ကြည့်တာကို အရင် သင်ခန်းစာတွေမှာ လေ့လာခဲ့ပြီး ဖြစ်ပါတယ်
- အခုသင်ခန်းစာမှာတော့ အဲ့ဒီ data တွေကို server ဆီ request ပို့ တာရယ်၊
- server မှာ request ကို လက်ခံပြီး ပါလာတဲ့ data တွေကို သိမ်းတာရယ်ကို လေ့လာကြည့်ကြပါမယ်
- အရင်ဆုံး frontend (browser) နဲ့ server (backend Express) ကို ချိတ်ဆက်သုံးလို့ရအောင် cors ဆိုတဲ့ node module တစ်ခုကို install လုပ်ပေးရပါမယ်

```console
$ cd backend/
```

```console
$ npm i cors
```

```console
$ npm i -D @types/cors
```

- browser တွေကနေ security အရ server တစ်ခုကနေ နောက် server တစ်ခုဆီ ဆက်သွယ်တဲ့အခါ တားမြစ်ထားလေ့ရှိပါတယ်
- cors module ကို သုံးပေးလိုက်ခြင်းအားဖြင့် express server ဆီကို အခြားserver တွေကနေ ဆက်သွယ်ခွင့်ရအောင် လုပ်ပေးလိုက်တာပဲ ဖြစ်ပါတယ်

```js
// backend/index.ts

import express from "express";
import cors from "cors";

const app = express();
const PORT = 5000;

app.use(cors());

app.get("/appData", (req, res) => {
  res.send("ok");
});

app.listen(PORT, () => console.log("serveris running at port", PORT));
```

- ခု frontend ဘက်ကနေ request တွေကို server မှာ လက်ခံလို့ရပြီးဖြစ်ပါတယ်။

##

### frontend ကနေ POST method နဲ့ menu state ကို ထည့်ပြီး request ပို့ဖို့ လုပ်ပါမယ်

![](https://cdn.discordapp.com/attachments/1146496852087287898/1149005437769887956/image.png)

> ရှင်းလင်းချက်

```js
//Create menu function
const createMenu = async () => {
  const response = await fetch("http://localhost:5000/menu", {
    method: "POST",
    headers: {
      "content-type": "application/json",
    },
    body: JSON.stringify(menu),
  });
  const dataFromServer = await response.json();
  console.log("dataFromServer: ", dataFromServer);
};
```

- createMenu function တစ်ခု သတ်မှတ်လိုက်ပါတယ်
- function ထဲမှာ /menu route ဆီကို POST method နဲ့ request လုပ်ထားပါတယ်
- menu state ရဲ့ တန်ဖိုးကို json string ပြောင်းပြီး body အဖြစ် ထည့်ပေးထားပါတယ်
- headers မှာတော့ body အနေနဲ့ ပို့ပေးလိုက်တဲ့ data ဟာ json ဖြစ်ကြောင်း server ကို အသိပေးတဲ့အနေနဲ့ content-type ကို သတ်မှတ်ပေးလိုက်တာဖြစ်ပါတယ်
- ဒါမှလဲ request ကို လက်ခံတဲ့server က request body ထဲက dataကို ဖတ်လို့ အဆင်ပြေမှာဖြစ်ပါတယ်
- ကျန်တာတွေကတော့ server ကနေ response ပြန်လာတဲ့ data ကို log ထုတ်ကြည့်ထားတာပဲဖြစ်ပါတယ်
- createMenu function ကို Cretae Button ကို နှိပ်တဲ့အခါ ခေါ်သုံးလိုက်ပါမယ်

![](https://cdn.discordapp.com/attachments/1146496852087287898/1149007369632755743/image.png)

- frontend မှာ create button နှိပ်လိုက်တာနဲ့ server ဆီ POST method နဲ့ request လုပ်ဖို့ ပြင်ဆင်ထားပြီးဖြစ်ပါတယ်

##

### Backend server မှာ အထက်ပါ request ကို လက်ခံပြီး ၀င်လာတဲ့ data တွေကို array တစ်ခုထဲသိမ်းလိုက်မှာဖြစ်ပါတယ်

```js
// backend/index.ts

import express from "express";
import cors from "cors";

const app = express();
const PORT = 5000;

app.use(cors());

//to get request body string data
app.use(express.json());

interface Menu {
  id: number;
  name: string;
  price: number;
  isArchived: boolean;
}

//DEMO menus array
const menus: Menu[] = [];

//use post middle to handle POST request
app.post("/menu", (req, res) => {
  //req.body= { name: "Shan-Kout-Swell", price: 3000 }
  const { name, price } = req.body;
  //create new id
  const newMenuId = menus.length === 0 ? 1 : menus[menus.length - 1].id + 1;

  const isArchived = false;

  //crate new menu object
  const newMenu = { name, price, id: newMenuId, isArchived };
  // const newMenu = {...req.body, id: newMenuId, isArchived: isArchived }

  //add to DEMO menus array
  menus.push(newMenu);

  //response menus array to frontend
  res.send(menus);
});

app.listen(PORT, () => console.log("serveris running at port", PORT));
```

- app.use နဲ့ cors() ကို သုံးထားပေးလိုက်တာမလို့ react server(frontend)ကနေ express server(backend ) ကို ဆက်သွယ်လို့ရအောင် လုပ်ပေးထားပါတယ်
- express.json() middleware ကို သုံးပြီး request ရဲ့ Body နဲ့ ပါလာတဲ့ Json string ကို express server မှာ js လို လက်ခံလို့ရအောင် လုပ်လိုက်ပါတယ်
- `app.post("/menu", (req, res) => {...}`
- post middleware ကို သုံးပြီး /menu route မှာ POST method နဲ့ ၀င်လာတဲ့ request ကို ဖမ်းလိုက်ပါတယ်
- req.body ထဲက name နဲ့ price ကို ထုတ်ယူလိုက်ပါတယ်
- menus array ထဲက item ရှိမရှိပေါ် မူတည်ပြီး

  - item တွေ မရှိခဲ့ရင် newMenuId ကို 1 အဖြစ် သတ်မှတ်လိုက်ပါတယ်
  - item တွေ ရှိခဲ့ရင်တော့ ေနာက်ဆုံး item ရဲ့ id ကို 1 ထပ်ပေါင်းပြီး newMenuId အဖြစ် သတ်မှတ်လိုက်ပါတယ်

- ပြီးရင် အပေါ်က သတ်မှတ်ထားတဲ့ DATA တွေနဲ့ object တစ်ခုလုပ်လိုက်ပြီး menus array ထဲ push ထည့်လိုက်တာပဲဖြစ်ပါတယ်
- အကုန်လုံးပြီးတဲ့အချိန်မှာတော့ frontend ဆီ response ပြန်ပြီး menus array ကို ပါ ထည့်ပေးလိုက်တာပဲဖြစ်ပါတယ်
- request ကို server ဆီမှာ ဖမ်းထားပြီမလို့ menu တစ်ခု create လုပ်ကြည့်ပါမယ်
- terminal နှစ်ခုကို ဖွင့်ထားပြီး express server ရော react server ပါ ပြိုင်တူ start ပေးထားပါမယ်

![](https://cdn.discordapp.com/attachments/1146496852087287898/1149291373086912522/image.png)
![](https://cdn.discordapp.com/attachments/1146496852087287898/1149291452338282496/image.png)

- ပြီးရင် menu route ထဲ ၀င်ပြီး menu name နဲ့ price ကို ထည့်ပြီး create လုပ်လိုက်ပါမယ်။

![](https://cdn.discordapp.com/attachments/1146496852087287898/1149293032726872114/image.png)

- server ကပြန်လာတဲ့ array မှာ create လုပ်လိုက်တဲ့ menu object ပါလာတာကို မြင်ရမှာဖြစ်ပါတယ်

##

### update menu with PUT method

- ရှိပြီးသား data တွေကို update လုပ်ချင်တဲ့အခါ server ဆီ PUT method နဲ့ request လုပ်ပြီး dataတွေကို ပို့ပေးလို့ရပါတယ်
- အထက်ပါ သင်ခန်းစာအတိုင်း လုပ်ထားတဲ့ menu ရဲ့ name ကို ပြင်ချင်တယ် ဆိုပါစို့
- အရင်ဆုံး ပြင်ချင်တဲ့ data ကိုပြင်ဆင်ပြီး server ဆီ request လုပ်ပြီး ပို့ပေးရမှာဖြစ်ပါတယ်
- နမူနာအနေနဲ့ data တစ်ခု သတ်မှတ်ပြီးပဲ စမ်းသပ်ကြည့်ရအောင်

- Menu component မှာ update button တစ်ခု ထပ်လုပ်ပြီး update function ကို အထက်ပါသင်ခန်းစာကလို ခေါ်သုံးပေးလိုက်ပါမယ်

```js
const updateMenu = async () => {
  const menuToUpdate = { id: 1, name: "gin tote", price: 50000 };
  const response = await fetch("http://localhost:5000/menu", {
    method: "PUT",
    headers: {
      "content-type": "application/json",
    },
    body: JSON.stringify(menuToUpdate),
  });
  const dataFromServer = await response.json();
  console.log("dataFromServer: ", dataFromServer);
};
```

```js
<Button variant="contained" sx={{ width: "fit-content" }} onClick={updateMenu}>
  updateMenu
</Button>
```

- id 1 ရှိတဲ့ menus array ထဲက item ရဲ့ name နဲ့ price ကို update လုပ်လိုက်တဲ့ value တွေ နဲ့ အစားထိုးပြီး update လုပ်ပေးမှာဖြစ်ပါတယ်

```js
// backend/ index.js --> put middleware

app.put("/menu", (req, res) => {
  //get update data from req
  const menuToUpdate = req.body;

  //find menu
  const menu = menus.find((menu) => menu.id === menuToUpdate.id);

  if (menu) {
    const newMenus = menus.map((menu) => {
      //founded..
      if (menu.id === menuToUpdate.id) {
        //replace with updated menu and add isArchived
        return { ...menuToUpdate, isArchived: false };
      } else {
        return menu;
      }
    });
    res.send(newMenus);
  } else {
    res.send(menus);
  }
});
```

> ရှင်းလင်းချက်

```
const menu = menus.find((menu) => menu.id === menuToUpdate.id);
```

- backend မှာ request ကို လက်ခံပြီး req body နဲ့ ၀င်လာတဲ့ object ထဲ id နဲ့ မူလ menus array ထဲရှိနေတဲ့ menu item တွေက id နဲ့ တူတဲ့ဟာ ရှိမရှိ ရှာလိုက်ပါတယ်

```
 if (menu) {
    const newMenus = menus.map((menu) => {

      if (menu.id === menuToUpdate.id) {

        return { ...menuToUpdate, isArchived: false };
      }
```

- မူရင်း array ကို map လုပ်ပြီး item တစ်ခုချင်းစီကို loop လုပ်ပါတယ်
- id တူတဲ့ menu ရှိခဲ့ရင်တော့ request နဲ့ ပါလာတဲ့ menu object နဲ့ အစားထိုးလိုက်ပါတယ်
- map လုပ်ပြီး ရလာတဲ့ array ကို newMenus ဆိုပြီး သိမ်းလိုက်ပြီး frontend ဆီကို response ပြန်ပေးလိုက်တာပဲ ဖြစ်ပါတယ်

##

### DELETE method for delete menu (using param and delete middleware)

### What is param??

### params

- params ဆိုတာ request လုပ်တဲ့အခါ သုံးတဲ့ route နောက်က ထည့်ပေးလိုက်တဲ့ အရာကို ဆိုလိုပါတယ်
- နမူနာ

```html
address:port/route/params http://localhost:5000/users/1
```

- express မှာ req.params ဆိုပြီး requset ၀င်လာတဲ့ params ကို ဖမ်းလို့ရပါတယ်။
- နမူနာ

```js
app.delete("/users/:id", (req, res) => {
  console.log(req.params);
  res.end();
});
// output: { id: '1' }
```

- delete middleware နဲ့ users route ကနေ ၀င်လာတဲ့ request ကို ဖမ်းထားပါတယ်
- users route နောက်မှာ : id ဆိုပြီး param ကိုဖမ်းထားပါတယ်
- အောက်မှာတော့ req.params ဆိုပြီး log ထုတ်ကြည့်ထားပါတယ်။
- postman ကနေ http://localhost:5000/users/1 ကို သုံးပြီး delete method ကို ရွေးပြီး request send ကြည့်ပါက
- request နဲ့ အတူပါလာတဲ့ `1` ဆိုတဲ့ params ကို express က route နောက်မှာ ဖမ်းထားတဲ့ id ရဲ့ တန်ဖိုးအဖြစ် သတ်မှတ်လိုက်ပြီး object တစ်ခု အဖြစ် req.params ထဲ ထည့်ပေးလိုက်တာဖြစ်ပါတယ်
- log ထုတ်ထားတာကို terminal ထဲ ကြည့်တဲ့အခါ `{ id : '1'}` ဆိုပြီး ပြပေးနေမှာဖြစ်ပါတယ်။
- ဒီနေရာမှာ သတိပြုရမှာက params ကို number အနေနဲ့ request ပို့လိုက်ပေမယ့် express မှာ ဖမ်းတဲ့အခါ string ဖြစ်သွားပါတယ်။
- ဆိုလိုတာက params ကို ဘယ်data type နဲ့ request ပို့လိုက်လိုက် express က string အနေနဲ့ ပဲ ဖမ်းမှာဖြစ်ပါတယ်။
-

##

### param ကို သုံးပြီး menu တစ်ခုကို delete လုပ်ကြည့်ပါမယ်

- အရင်ဆုံး menu item သုံးခုလောက် create လုပ်ပေးထားလိုက်ပါ။
- Menu component မှာ delete button တစ်ခုလုပ်ပြီး delete လုပ်မယ့် function တစ်ခုသတ်မှတ်ကာ delete button ရဲ့ click event မှာ run ခိုင်းလိုက်ပါမယ်

```js
<Button
  variant="contained"
  sx={{ width: "fit-content", mb: 2 }}
  onClick={deleteMenu}
>
  Delete
</Button>
```

```js
const deleteMenu = async () => {
  const menuIdToDelete = 2;
  const response = await fetch(`http://localhost:5000/menu/${menuIdToDelete}`, {
    method: "DELETE",
  });
  const dataFromServer = await response.json();
  console.log("dataFromServer: ", dataFromServer);
};
```

- delete button ကို နှိပ်လိုက်ချိန်မှာ deleteMenu function ကို ခေါ်လိုက်မှာဖြစ်ပြီး param အနေ နဲ့ **2** ကို route ရဲ့ ေနာက်မှာ ထည့်ပြီး request လုပ်လိုက်တာဖြစ်ပါတယ်
- အဲ့ဒီ request ကို backend မှာ လက်ခံလိုက်ပါမယ်

```js
// backend/index.js --> delete middleware

app.delete("/menu/:id", (req, res) => {
  //get id from param
  const menuIdToDelete = Number(req.params.id);

  // find menu to delete with id param
  const menuToDelete = menus.find((menu) => menu.id === menuIdToDelete);
  if (menuToDelete) {
    //filter out menu to delete
    const remainingMenus = menus.filter((menu) => menu.id !== menuIdToDelete);
    res.send(remainingMenus);
  } else {
    res.send(menus);
  }
});
```

- param အနေနဲ့ ၀င်လာတဲ့ data ကို :id နဲ့ ဖမ်းလိုက်ပြီး number ပြောင်းကာ menuIdToDelete အဖြစ် သတ်မှတ်လိုက်ပါတယ်
- အဲ့ဒီ id ကို သုံးပြီး menus array ကနေ id မတူတဲ့ Items တွေ ရွေးထုတ်ကာ remainingMenus အဖြစ် သိမ်းလိုက်ပြီး frontend ဆီ ပြန်ပို့ပေးလိုက်တာဖြစ်ပါတယ်
- react app မှာ delete button ကို နှိပ်ကြည့်လိုက်ပါက id 2 ဖြစ်တဲ့ menu item ကို ဖြတ်ပြီး ကျန်တဲ့ menu item တွေ response ပြန်ပေးတာကို ြမင်ရမှာဖြစ်ပါတယ်
