## MSquare Programming Fullstack Course

### Batch 2

### Episode-_15_ Summary

## CURD web-application with react and express(part-2)

- Create menu (MUI Dialog Box)
- show menu (MUI Card component)
  ##

#### အရင် သင်ခန်းစာတုန်းကလို Menu page (Menu component) တစ်ခုထဲမှာပဲ create / update / delete လုပ်တာတွေကို စုပြုံ မထားတော့ပဲ menu create လုပ်တဲ့အပိုင်းကို CreateMenu ဆိုတဲ့ component တစ်ခု လုပ်ပြီး သီးသန့်ခွဲထုတ်လိုက်ပါမယ်

```js
// frontend/src/pages/menu/CreateMenu.tsx

import { Box, Dialog } from "@mui/material";

interface Props {
  open: boolean;
  setOpen: (value: boolean) => void;
}

const CreateMenu = ({ open, setOpen }: Props) => {
  return (
    <Box>
      <Dialog open={open} onClose={() => setOpen(false)}>
        <h1>I'm Dialog</h1>
      </Dialog>
    </Box>
  );
};

export default CreateMenu;
```

- open နဲ့ setOpen တို့ကို props အနေနဲ့ လက်ခံထားတဲ့ CreateMenu component တစ်ခု လုပ်ထားပါတယ်
- open ရဲ့ type ကို boolean အဖြစ် သတ်မှတ်ပေးထားပြီး
- setOpen ရဲ့ type တော့ boolean ဖြစ်တဲ့parameter တစ်ခုကို လက်ခံတဲ့ function type အဖြစ် သတ်မှတ်ပေးထားပါတယ်
- Mui က Dialog component ကို return လုပ်ထားပါတယ်
- Dialog component မှာ `open` နဲ့ `onclose` props တွေကို လက်ခံပါတယ်
- `open` props က Dialog box ကို ပြပေးမယ့် state ကို ကိုင်တွယ်မှာဖြစ်ပြီး true ဖြစ်ရင် dialog box ကို ပြပေးပြီး false ဖြစ်ရင် dialog box ကို ပိတ်ထားပေးမှာဖြစ်ပါတယ်
- `onclose` props ကတော့ dialog box အပြင်ဘက်မှာနှိပ်လိုက်တဲ့အခါ run ပေးမယ့် function ဖြစ်ပါတယ်
- အဲ့ဒီ props နှစ်ခုလုံးရဲ့ တန်ဖိုးကို CreateMenu မှာ လက်ခံထားတဲ့ props တွေရဲ့ တန်ဖိုးနဲ့ သတ်မှတ်ပေးထားလိုက်ပါတယ်
- flow ကို မြင်သာအောင် ရှင်းပြရမယ်ဆိုရင် CreateMenu component ကို ခေါ်သုံးမယ့် parent component မှာ open state တစ်ခု(`[open,setOpen]=useState(flase)`) သတ်မှတ်ပြီး အဲ့ဒီ state ကို update လုပ်ပြီး dialog box ကို ဖွင့်/ပိတ် လုပ်ချင်တာဖြစ်ပါတယ်
- flow ကို လက်တွေ့ အသုံးချကြည့်ရအောင်
- Menu componentမှာ CreateMenu component ကို ခေါ်သုံးပြီး dialog box ကို ပြပေးနိုင်အောင် လုပ်ပါမယ်
  ![](https://cdn.discordapp.com/attachments/1146496852087287898/1149767700382490714/image.png)

  - Menu component မှာ open state တစ်ခုသတ်မှတ်ထားပြီး Open Dialog button ကို နှိပ်လိုက်ချိန်မှာ open ကို true လုပ်ပေးလိုက်ပါတယ်
  - CreateMenu component ကို ေခါ်သုံးထားပြီး open နဲ့ setOpen ကို props အနေနဲ့ ထည့်ပေးထားပါတယ်
  - ခု react app မှာ Open dialog ကို နှိပ်ကြည့်ပါက Dialog box ပြပေးမှာဖြစ်ပြီး dialog box အပြင်ဘက်မှာ click လိုက်ပါက dialog box ကို ပိတ်ပေးလိုက်မှာဖြစ်ပါတယ်

![](https://cdn.discordapp.com/attachments/1146496852087287898/1149770768331309136/dialog1.png)

##

### Move create menu function to CreateMenu component's Dialog box

- Menu component ထဲက menu တစ်ခု create လုပ်တဲ့ function ကို CreateMenu component ထဲကို ရွေ့ပြီး menu တစ်ခု create လုပ်တဲ့ အခါ dialog box နဲ့ ပြပေးမှာဖြစ်ပါတယ်

```js
// frontend/src/pages/menu/CreateMenu.tsx

import {
  Box,
  Button,
  Dialog,
  DialogContent,
  DialogTitle,
  TextField,
} from "@mui/material";
import { useState } from "react";

interface Props {
  open: boolean;
  setOpen: (value: boolean) => void;
}

interface Menu {
  name: string;
  price: number;
}

const CreateMenu = ({ open, setOpen }: Props) => {
  const [menu, setMenu] = useState < Menu > { name: "", price: 0 };
  const url = "http://localhost:5000";

  //create menu
  const handleCreateMenu = async () => {
    console.log(menu);
    const response = await fetch(`${url}/menu`, {
      method: "POST",
      headers: { "content-type": "application/json" },
      body: JSON.stringify(menu),
    });
    const data = await response.json();
    console.log(data);
  };

  return (
    <Box>
      <Dialog open={open} onClose={() => setOpen(false)}>
        <DialogTitle>Create Menu</DialogTitle>
        <DialogContent>
          <Box
            sx={{
              display: "flex",
              alignItems: "center",
              flexDirection: "column",
            }}
          >
            <TextField
              sx={{ width: 300, mb: 2 }}
              placeholder="Name"
              onChange={(evt) => setMenu({ ...menu, name: evt.target.value })}
            />
            <TextField
              sx={{ width: 300, mb: 4 }}
              placeholder="Price"
              onChange={(evt) =>
                setMenu({ ...menu, price: Number(evt.target.value) })
              }
            />

            <Button
              variant="contained"
              sx={{ width: "fit-content", mb: 2 }}
              onClick={handleCreateMenu}
            >
              Create menu
            </Button>
          </Box>
        </DialogContent>
      </Dialog>
    </Box>
  );
};

export default CreateMenu;
```

- Dialog content မှာ menu တစ်ခု crate လုပ်တဲ့အခါ လိုတဲ့ input တွေနဲ့ button တွေကို Menu component ကနေ ပြောင်းထည့်ပေးလိုက်ပြီး တခြားလိုအပ်တဲ့ data တွေကို လဲ CreateMenu component ဆီမှာ ေပြာင်းထည့်ထားတာပဲဖြစ်ပါတယ်

- ဒါ့အပြင် menu create လုပ်တဲ့အခါ လိုအပ်တဲ့ handleCreateMenu function နဲ့ menu state ရယ် type တွေ ရယ် ကိုပါ တစ်ခါတည်း ရွေ့ပေးလိုက်ပါတယ်
- ကျနော်တို့ရဲ့ Menu component မှားလည်း button တစ်ခုနဲ့ ခုလိုပဲကျန်မှာဖြစ်ပါတယ်

```js
// src/pages/menu/Menu.tsx

import { Box, Button, Typography } from "@mui/material";
import React, { useEffect, useState } from "react";
import CreateMenu from "./CreateMenu";

const MenuPage = () => {
  const url = "http://localhost:5000";

  const [open, setOpen] = useState(false);

  return (
    <Box>
      <Box
        sx={{
          display: "flex",
          justifyContent: "flex-end",
          mr: 2,
        }}
      >
        {/* button to show CreateMenu's Dialog */}
        <Button variant="contained" onClick={() => setOpen(true)}>
          Create menu
        </Button>
      </Box>

      <CreateMenu open={open} setOpen={setOpen} setMenus={setMenus} />
    </Box>
  );
};

export default MenuPage;
```

- Menu component ကို MenuPage.tsx လို့ နာမည်ပြောင်းလိုက်ပြီး အထဲမှာ သတ်မှတ်ထားတဲ့ component function name ကို လည်း MenuPage လို့ ပြောင်းလဲသတ်မှတ်ပြီး export လုပ်ထားလိုက်ပါတယ်
- Menu component ထဲမှာဆိုရင် create menu button တစ်ခုနဲ့ CreateMenu component ကို သာ return လုပ်ထားပေးပါတယ်
- Cretae Menu Button ကို click လိုက်ရင် CreateMenu component ထဲက dialog ကို ပြပေးမှာဖြစ်ပြီး MENU တစ်ခု create လုပ်လို့ရမှာဖြစ်ပါတယ်
- react app မှာ စမ်းသပ်ကြည့်ပါမယ်

![](https://cdn.discordapp.com/attachments/1146496852087287898/1150648768346198016/image.png)

- menu တစ်ခု ကို create လုပ်ကြည့်ထားပါတယ်
- လိုအပ်တဲ့ data တွေ ထည့်ပြီး create menu butoon ကို နှိပ်လိုပ်ချိန်မှာ MENU တစ်ခု CREATE လုပ်ပေးသွားပေမယ့်dialog box က ဆက်ပြီး ပြပေးနေတာကို မြင်ရမှာပါ။
- menu တစ်ခု ကို create လုပ်ပြီးပြီ ဆိုရင် dialog box ကို ပြန်ပိတ်ပေးရမှာဖြစ်လို့ handleCrateMenu function ထဲမှာ open state ကို false အဖြစ် update လုပ်ပေးရမှာဖြစ်ပါတယ်

```js
// src/pages/menu/CreateMenu.tsx --> handleCreateMenu function

const handleCreateMenu = async () => {
  console.log(menu);
  const response = await fetch("http://localhost:5000/menu", {
    method: "POST",
    headers: { "content-type": "application/json" },
    body: JSON.stringify(menu),
  });
  const data = await response.json();

  setOpen(false); // <-- update open state

  console.log(data);
};
```

##

### abstraction

- ကျနော်တို့ Type တွေ interface တွေ လုပ်တဲ့အခါ props တွေ အတွက် type တွေ လောက်သာ component အထဲမှာ ထည့်ရေးလေ့ရှိပြီး ကျန်တဲ့ type တွေကိုတော့ folder တစ်ခု သပ်သပ်ခွဲရေးလေ့ရှိပြီး export လုပ်ထားကာ လိုအပ်တဲ့ component မှာ import လုပ်သုံးပါတယ်
- type error တက်လာလို့ပဲဖြစ်ဖြစ် update လုပ်ချင်လို့ပဲ ဖြစ်ဖြစ် type folder အောက်မှာ သွားလုပ်ပေးရုံပါပဲ
- ခု menu နဲ့ ပတ်သတ်တဲ့ type တွေကို menuType.ts ဆိုတဲ့ file သီးသန့်ခွဲပြီး types folder အောက်မှာ ထားလိုက်ပါမယ်

![](https://cdn.discordapp.com/attachments/1146496852087287898/1150653614386315355/image.png)

- src / types/menuType.ts ဖိုင်တစ်ခုလုပ်ပြီး Menu interface type တစ်ခုကို သတ်မှတ်ကာ export လုပ်ထားလိုက်ပါတယ်

### Show all menus in UI

- ဆက်ပြီးတော့ server ရှိတဲ့ menu တွေကို လှမ်းယူပြီး UI မှာ ပြပေးနိုင်အောင် လုပ်ပါမယ်

### Get all menus from server ( _GET_ method )

![](https://cdn.discordapp.com/attachments/1146496852087287898/1150651484762030131/image.png)

- menus state တစ်ခုကို သတ်မှတ်လိုက်ပြီး ခုနက သတ်မှတ်ထားတဲ့ Menu type ကို import လုပ်သုံးကာ menus state ရဲ့ type ဟာ Menu object ပါတဲ့ array ဖြစ်ကြောင်း type ပေးထားလိုက်ပါတယ်
- getMenu function ထဲမှာ server ဆီ GET method နဲ့ request လုပ်ထားပြီး response ပြန်လာတဲ့ data တွေကို menus state အဖြစ် update လုပ်ပေးထားပါတယ်
- ခု လုပ်ထားတဲ့ request ကို server မှာ လက်ခံလိုက်ပါမယ်

```js
// backend/index.ts --> GET & POST

//GET method
app.get("/menu", (req, res) => {
  res.send(menus);
});

app.post("/menu", (req, res) => {
  console.log(req.body);
  const { name, price } = req.body;
  const id = menus.length === 0 ? 1 : menus[menus.length - 1].id + 1; //uuid
  const isArchived = false;
  const imgUrl =
    "https://thumbs.dreamstime.com/z/heart-shape-various-vegetables-fruits-healthy-food-concept-isolated-white-background-140287808.jpg?w=768";

  const newMenu = { id, name, price, isArchived, imgUrl };
  // {id: id ,name: name, price:price,isArchived:isArchived,imgUrl:imgUrl}
  menus.push(newMenu);

  res.send(menus);
});
```

- GET method နဲ့ request ၀င်လာရင် server မှာရှိတဲ့ menus array ကိုပဲ response ပြန်ပေးလိုက်တာပဲဖြစ်ပါတယ်
- POST method မှာတော့ imgUrl တစ်ခုကို အသေထည့်ပေးထားပြီး create လုပ်လာတဲ့ menu တွေရဲ့ ပုံအဖြစ် အသုံးပြုမှာ ဖြစ်ပါတယ်
- နောက်ပိုင်းကျရင်တော့ IMGAGE UPLOAD သင်ခန်းစာ ပြီးတဲ့အခါ ကိုယ်ပိုင်ပုံတွေနဲ့ အသုံးပြုမှာဖြစ်ပါတယ်
- server မှာလဲ ပြင်ဆင်လို့ပြီးပြီမို့ frontend က request ပို့မယ့် function ကို ခေါ်ပေးရပါမယ်
- ဒီနေရာမှာ သတိထားရမှာက `getMenu()` လို့ ဒီအတိုင်း ခေါ်လိုက်ရင်

![](https://cdn.discordapp.com/attachments/1146496852087287898/1150659748262322236/image.png)

- getMenu function ထဲမှာ state ကို update လုပ်ထားတဲ့အတွက် component က re-render ပြန်ဖြစ်ပြီး အစကနေ ပြန်run ပါမယ်
- line 47 ရောက်တဲ့အခါ `getMenu()` ကို ခေါ်ထားတာမလို့ state ချိန်ပြီး re-render ပြန်ဖြစ်/ အစက ပြန် run / `getMenu()` တွေ့ / state ချိန်း/ပြန်run /...နဲ့ component ကို render လုပ်လို့မပြီးနိုင်ပဲ infinity rendering ဖြစ်ကာ နောက်ဆုံး computer ပါ ရပ်သွားလောက်တဲ့အထိ ERROR တက်သွားနိုင်ပါတယ်
- အဲ့လို error ကို ဖြေရှင်းနိုင်ဖို့ `getMenu()` ကို useEffect ထဲမှာ ထည့်ခေါ်ပေးရမှာ ဖြစ်ပါတယ်

![](https://cdn.discordapp.com/attachments/1146496852087287898/1150661303619293274/image.png)

- dependency list ကို empty array ပေးထားတာမလို့ useEffect ထဲက function ဟာ ပထမအကြိမ် render ဖြစ်တဲ့ အခါမှသာ run မှာဖြစ်ပြီး state ချိန်းလို့ ပဲဖြစ်ဖြစ် component re-render ဖြစ်တဲ့အခါ အလုပ်လုပ်ပေးမှာ မဟုတ်လို့ error ကို ဖြေရှင်းပြီးသားဖြစ်သွားပါတယ်
- react app မှာ menu သုံးခုလောက် create လုပ်ပြီး Menu page ကို refresh တစ်ခါလုပ်ပေးကြည့်ကာ server ကdata တွေ GET နဲ့ ယူထားတာကို log ထုတ်ကြည့်ပါမယ်
  ![](https://cdn.discordapp.com/attachments/1146496852087287898/1150663396941897850/image.png)

##

### server မှာရှိတဲ့ menu တွေလဲရပြီမလို့ UI မှာ ပြဖို့ ဆက်လုပ်ပါမယ်

- UI မှာ ပြရန်အတွက် MUI က card component ကို သုံးပါမယ်
-

```js
// src/components/MenuCard.tsx

import PaidIcon from "@mui/icons-material/Paid";
import { Box, Card, CardContent, CardMedia, Typography } from "@mui/material";
import { Link } from "react-router-dom";
import { Menu } from "../types/menuType";

interface Props {
  menu: Menu;
}

const MenuCard = ({ menu }: Props) => {
  return (
    <Link
      to={String(menu.id)}
      style={{
        textDecoration: "none",
        marginRight: "15px",
        marginBottom: "20px",
      }}
    >
      <Card sx={{ width: 200, height: 220, pb: 2 }}>
        <CardMedia
          sx={{ height: 140, backgroundSize: "contain" }}
          image={menu.imgUrl || ""}
        />
        <CardContent>
          <Typography
            gutterBottom
            variant="h6"
            sx={{ textAlign: "center", mb: 0 }}
          >
            {menu.name}
          </Typography>
          <Box
            sx={{
              display: "flex",
              justifyContent: "center",
              alignItems: "center",
            }}
          >
            <PaidIcon color="success" />
            <Typography
              gutterBottom
              variant="subtitle1"
              sx={{ mt: 0.8, ml: 0.8 }}
            >
              {menu.price}
            </Typography>
          </Box>
        </CardContent>
      </Card>
    </Link>
  );
};

export default MenuCard;
```

> ရှင်းလင်းချက်

```js
interface Props {
  menu: Menu;
}

const MenuCard = ({ menu }: Props) => {....}
```

- MenuCard component တစ်ခု သတ်မှတ်ထားပြီး menu props ကို လက်ခံထားပါတယ်

```js
<Link
  to={String(menu.id)}
  style={{
    textDecoration: "none",
    marginRight: "15px",
    marginBottom: "20px",
  }}
>
  <Card>.....</Card>
</Link>
```

- React router dom က Link component နဲ့ MUI Card ကို wrap လုပ်ထားပါတယ်
- Link component ရဲ့ to props မှာတော့ menu props ထဲက id ကို route လုပ်ပေးထားပါတယ်

```js
<Card sx={{ width: 200, height: 220, pb: 2 }}>
  <CardMedia
    sx={{ height: 140, backgroundSize: "contain" }}
    image={menu.imgUrl || ""}
  />
  <CardContent>
    <Typography gutterBottom variant="h6" sx={{ textAlign: "center", mb: 0 }}>
      {menu.name}
    </Typography>
    <Box
      sx={{
        display: "flex",
        justifyContent: "center",
        alignItems: "center",
      }}
    >
      <PaidIcon color="success" />
      <Typography gutterBottom variant="subtitle1" sx={{ mt: 0.8, ml: 0.8 }}>
        {menu.price}
      </Typography>
    </Box>
  </CardContent>
</Card>
```

- Card ထဲမှာတော့ props အဖြစ်၀င်လာမယ့် menu ထဲက data တွေကို သုံးပြီး UI မှာ card အဖြစ် ပြပေးဖို့ လုပ်ထားပါတယ်
- MenuCard မှာ ဘာတွေ လုပ်ထားသလဲဆိုရင် props အဖြစ် ၀င်လာမယ့် menu ထဲက data တွေကို သုံးပြီး card လေးတွေ အဖြစ် ပြပေးနိုင်ဖို့ရယ် card တစ်ခုခုကို နှိပ်လိုက်ရင် နှိပ်လိုက်တဲ့ card ရဲ့ id အတိုင်း route တစ်ခုစီကို ပို့ပေးနိုင်ဖို့ရယ် ကို လုပ်ထားတာပဲ ဖြစ်ပါတယ်
- MenuCard ကို MenuPage ထဲမှာ သုံးပြီး server ဆီက menu data တွေကို UI မှာ ပြပေးလိုက်ပါမယ်

![](https://cdn.discordapp.com/attachments/1146496852087287898/1150670012672118854/image.png)

- menus state ကို map လုပ်ပြီး ထွက်လာမယ့် menu item တစ်ခုချင်းစီကို MenuCard ကလက်ခံမယ့် props အဖြစ် ထည့်ပေးလိုက်တာပဲ ဖြစ်ပါတယ်
- react app မှာ refresh လုပ်ကြည့်ပါက create လုပ်ထားတဲ့ menu တွေကို ပြပေးမှာဖြစ်ပါတယ်
  ![](https://cdn.discordapp.com/attachments/1146496852087287898/1150670786844172298/image.png)

- ဒါပေမယ့် menu တစ်ခု create ထပ်လုပ်ကြည့်တဲ့အခါ ချက်ချင်း မပြပေးပဲ page ကို refresh လုပ်ပေးမှသာ ပေါ်လာတာကို ြမင်ရမှာပါ
- menu တစ်ခု create လုပ်လိုက်တာနဲ့ UI မှာ ချက်ချင်းပြပေးနိုင်ဖို့ menus state ကို menu create လုပ်တိုင်း update လုပ်ပေးရမှာဖြစ်ပါတယ်
- MenuPage ကနေ CreateMenu ဆီကို setMenus funtion အား props အနေနဲ့ ပို့လိုက်ပါမယ်
- CreateMenu မှာလည်း menu create လုပ်တိုင်း setMenus ကို ခေါ်ပေးထားလိုက်ပါမယ်

![](https://cdn.discordapp.com/attachments/1146496852087287898/1150672035119374357/image.png)

![](https://cdn.discordapp.com/attachments/1146496852087287898/1150672173200064512/image.png)

- အဲ့ဒါဆို menu create လုပ်လိုက်တိုင်း UI မှာ ချက်ချင်းပြပေးမှာဖြစ်ပါတယ်

##

### Dynamic Route in react

- ခု menu တွေ ပြပေးနိုင်ပေမယ့် menu card တစ်ခုကို နှိပ်ကြည့်လိုက်ရင် route အသစ် တစ်ခုကို သွားပေးမယ့်လည်း ပြောင်းလဲမှုမရှိပဲ console မှာ no route match ... error ပြလာတာကိုတွေ့ရမှာပါ.
- ဥပမာ ။ ။ id 2 ဖြစ်တဲ့ menu ကို နှိပ်လိုက်ရင် url မှာ `http://localhost:3000/menu/2` လို့ routing လုပ်သွားပေမယ့်လည်း UI ကတော့ ဒီအတိုင်းပဲ ရှိနေတာကို ြမင်ရမှာပါ
- ဘာလို့လဲဆိုတော့ `http://localhost:3000/menu/2` ဆိုတဲ့ route မှာ ပြပေးဖို့ component ကို routing system မှာ မသတ်မှတ်ထားလို့ဖြစ်ပါတယ်
- ဒါဆိုရင် AppRouter component မှာ `http://localhost:3000/menu/2` အတွက် ပြပေးမယ့် component တစ်ခုတည်ဆောက်ပြီး routing system မှာ ထည့်ပေးလိုက်ပါမယ်

```js
// src/pages/menu/UpdateMenu.tsx

import React from "react";

const UpdateMenuPage = () => {
  return <h1>Update Menu</h1>;
};

export default UpdateMenuPage;
```

- အဲ့ဒီ UpdateMenu component နဲ့ `http://localhost:3000/menu/2` ကို route လုပ်ပေးလိုက်ပါမယ်

```js
// src/components/appRouter/AppRouter.tsx

import { BrowserRouter, Route, Routes } from "react-router-dom";
import App from "../../App";
import Menu from "../../pages/menu/MenuPage";
import MenuCategory from "../../pages/menuCategory/MenuCategory";
import Layout from "../layout/Layout";
import MenuPage from "../../pages/menu/MenuPage";
import UpdateMenuPage from "../../pages/menu/UpdateMenuPage";

//making routing system
// syntax
{
  /* 

<BrowserRouter>
  <Routes>
    <Route path="route" Component={component-name} />
  </Routes>
</BrowserRouter>

*/
}

const AppRouter = () => {
  return (
    <BrowserRouter>
      <Layout>
        <Routes>
          <Route path="/" Component={App} />
          <Route path="/menu" Component={MenuPage} />
          <Route path="/menuCategory" Component={MenuCategory} />
          <Route path="/menu/2" Component={UpdateMenuPage} />
        </Routes>
      </Layout>
    </BrowserRouter>
  );
};

export default AppRouter;
```

- အခုဆိုရင် id 2 ရှိတဲ့ menu card ကို နှိပ်ကြည့်လိုက်ပါက UpdateMenu component ကို ပြပေးမှာဖြစ်ပါတယ်။

![](https://cdn.discordapp.com/attachments/1146496852087287898/1150675994361995304/image.png)

- ဒါပေမယ့် id 2 မဟုတ်တဲ့ အခြားmenu card တွေကို နှိပ်ကြည့်ပါက ခုနကလို errorထပ်ဖြစ်လာမှာပါ
- menu card id တစ်ခုချင်းစီအတွက် route တွေ တစ်ခုစီ သတ်မှတ်ပေးရမယ့် ပုံစံမျိုး ဖြစ်လာပါတယ်
- အခုလို routing system မှာ id တွေနဲ့ route လုပ်မယ်ဆိုရင် dynamic route ကို အသုံးပြုနိုင်ပါတယ်
- AppRouter မှာ /menu/2 လို့ အသေ မထည့်တော့ပဲ 2 နေရာကို dynamic ဖြစ်အောင် ခုလိုလက်ခံပေးလိုက်ပါမယ်

```js
//before
<Route path="/menu/2" Component={UpdateMenuPage} />

//after
<Route path="/menu/:id" Component={UpdateMenuPage} />
```

- ဆိုလိုတဲ့ သဘောက :id ဆိုတဲ့ နေရာမှာ ဘာပဲ၀င်လာလာ UpdateMenuPage ကို ပြပေးမှာဖြစ်ပါတယ်
- react app မှာ စမ်းကြည့်ပါက ဘယ် menu card ကိုပဲ နှိပ်နှိပ် သက်ဆိုင်ရာ id နဲ့ route တစ်ခုကို သွားလိုက်မှာ ဖြစ်ပြီး UpdateMenuPage ကိုပဲ ပြပေးမှာဖြစ်ပါတယ်
  ##
