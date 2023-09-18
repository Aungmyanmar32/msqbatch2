## MSquare Programming Fullstack Course

### Batch 2

### Episode-_18_ Summary

##

## `NextJS`

- environment
- API Request

##

## NextJS မှာ development နဲ့ product ဆိုပြီး environment နှစ်မျိုးရှိတယ်ဆိုတာ အရင် သင်ခန်းစာမှာ ရှင်းပြခဲ့ပါတယ်

- development လုပ်နေတဲ့ အချိန်မှာ သုံးတဲ့ endpoint( http://loaclhost:3000 ) ဟာ local မှာ သုံးလို့ အဆင်ပြေနေပေမယ့် production ( internet ပေါ်တင်လိုက်တဲ့အခါ) ရောက်တဲ့အခါ http://loaclhost:300 ဆိုတာ ရှိမှာမဟုတ်ပဲ app ကို hosting လုပ်ထားတဲ့ endpoint ( eg: https//my-app.vercel.app ) ကို သုံးပေးမှ အလုပ်လုပ်မှာဖြစ်ပါတယ်
- environment ပေါ် မူတည်ပြီး ပြောင်းလဲအသုံဲးပြုပေးရမယ့် value တွေကို .env ဆိုတဲ့ file ထဲမှာ သိမ်းထားပြီး လိုအပ်တဲ့ component တွေမှာ ခေါ်သုံးပေးလို့ရပါတယ်

## example

- အရင်သင်ခန်းစာတွေမှာ backend ဆီ request လုပ်တဲ့အချိန် api url(endpoint ) ကို http://localhost:5000/ ဆိုပြီး အသေသတ်မှတ်ပြီး သုံးခဲ့ကြပါတယ်

```js
const response = await fetch("http://localhost:5000/");
```

- environment မတူတဲ့အခါလည်း အဆင်ပြေအောင် အပေါ်ကလို အသေမထည့်တော့ပဲ .env ဖိုင်ကို သုံးပြီး အခုလို သတ်မှတ်ပေးလိုက်လို့ရပါတယ်

```
  // .env

  API_BASE_URL = http://localhost:5000/

```

```js
const apiBaseUrl = process.env.API_BASE_URL;
const response = await fetch("apiBaseUrl");
```

- အခု သတ်မှတ်ထားတဲ့ .env ဟာ development မှာ သုံးတဲ့ environment ဖြစ်ပြီး production မှာကျရင်တော့ ခုသတ်မှတ်ထားတဲ့ .env ကို ပို့ပေးမှာ မဟုတ်ပဲ production environment သက်သက် သတ်မှတ်ပြီး သုံးပေးလိုက်ရင် အဆင်ပြေသွားမှာဖြစ်ပါတယ်
- production မှာ environment သတ်မှတ်နည်းကို နောက်သင်ခန်းစာတွေမှာ လေ့လာကြပါမယ်
- next foodie pos app မှာလည်း environment အတွက် setup လုပ်ပြီး သုံးပေးလိုက်ပါမယ်။

### အရင်ဆုံး .env ဖိုင်တစ်ခု create လုပ်လိုက်ပါမယ်

![](https://cdn.discordapp.com/attachments/1146496852087287898/1152236496166866994/image.png)

- ပြီးရင် နမူနာမှာ ပြထားသလို တိုက်ရိုက်မသုံးပဲ config file တစ်ခုနဲ့ သတ်မှတ်လိုက်ပါမယ်
- ![](https://cdn.discordapp.com/attachments/1146496852087287898/1152240169932566669/image.png)

- ခုဆိုရင် backend ဆီ request လုပ်တဲ့အခါ config.apiBaseUrl ကို url အနေနဲ့ import လုပ်ပြီး သုံးလို့ရပြီးဖြစ်ပါတယ်

```js
import {config} from "../config/config"

.....
.....
const response = await fetch(config.apiBaseUrl);
....
```

##

### ADD layout component

- next js ရဲ့ backoffice page တိုင်းမှာ သုံးဖို့ layout component တစ်ခု သတ်မှတ်ပါမယ်
- အရင် react မှာ သုံးတဲ့တဲ့ Layout component ကိုပဲ copy ကူးပြီး Backoffice.tsx အနေနဲ့ သိမ်းလိုက်မှာဖြစ်ပါတယ်

```js
// src/components/BackofficeLayout.tsx

import CategoryIcon from "@mui/icons-material/Category";
import RestaurantMenuIcon from "@mui/icons-material/RestaurantMenu";
import {
  Box,
  ListItemButton,
  ListItemIcon,
  ListItemText,
  Typography,
} from "@mui/material";
import Link from "next/link";
import { ReactNode } from "react";

interface Props {
  children: ReactNode;
}

const BackofficeLayout = ({ children }: Props) => {
  return (
    <Box>
      {/* app bar */}
      <Box
        sx={{
          display: "flex",
          justifyContent: "center",
          alignItems: "center",
          height: 80,
          bgcolor: "#4C4C6D",
        }}
      >
        <Typography variant="h4" sx={{ color: "#E8F6EF" }}>
          Foodie POS - Backoffice
        </Typography>
      </Box>

      {/* side bar */}
      <Box sx={{ display: "flex", minHeight: "100vh" }}>
        <Box sx={{ width: 300, bgcolor: "#1B9C85", borderTopRightRadius: 20 }}>
          <Link href={"/backoffice/menu"} style={{ textDecoration: "none" }}>
            <ListItemButton>
              <ListItemIcon>
                <RestaurantMenuIcon sx={{ color: "#E8F6EF", fontSize: 30 }} />
              </ListItemIcon>
              <ListItemText
                primary={
                  <Typography sx={{ fontSize: 20, color: "#E8F6EF" }}>
                    Menu
                  </Typography>
                }
              />
            </ListItemButton>
          </Link>
          <Link
            href={"/backoffice/menuCategory"}
            style={{ textDecoration: "none" }}
          >
            <ListItemButton>
              <ListItemIcon>
                <CategoryIcon sx={{ color: "#E8F6EF", fontSize: 30 }} />
              </ListItemIcon>
              <ListItemText
                primary={
                  <Typography sx={{ fontSize: 20, color: "#E8F6EF" }}>
                    Menu Category
                  </Typography>
                }
              />
            </ListItemButton>
          </Link>
        </Box>

        {/* children component */}
        <Box sx={{ width: "100%", pl: 3, pt: 3 }}>{children}</Box>
      </Box>
    </Box>
  );
};

export default BackofficeLayout;
```

- ပြီးရင် menu page မှာ wrap လုပ်ပေးလိုက်ပါမယ်
- next app ကနေ /backoffice/menu route ကို ၀င်ကြည့်လိုက်ရင် layout နဲ့ ပြပေးနေတာကို မြင်ရမှာပါ

```js
// src/pages/backoffice/menu/index.tsx

import BackofficeLayout from "@/components/BackofficeLayout";
import React from "react";

const Menu = () => {
  return (
    <BackofficeLayout>
      <div>
        <h1>Menu</h1>
      </div>
    </BackofficeLayout>
  );
};

export default Menu;
```

![](https://cdn.discordapp.com/attachments/1146496852087287898/1152245893676933150/image.png)

##

## ဆက်ပြီး react app မှာ လုပ်ခဲ့သလိုမျိုးပဲ menu တစ်ခု create လုပ်လို့ရအောင် ပြင်ဆင်ပါမယ်

- layout လိုပဲ react app ထဲက menu component နဲ့ create menu component တွေကို copy လုပ်ပြီး next js project ထဲကို ပြောင်းထည့်လိုက်ပါမယ်

```js
// src/pages/backoffice/menu/index.tsx

import BackofficeLayout from "@/components/backofficeLayout";
import MenuCard from "@/components/menuCard/MenuCard";
import config from "@/config";
import { Menu } from "@/types/menu";
import { Box, Button } from "@mui/material";
import { useEffect, useState } from "react";
import CreateMenu from "../../../components/createMenu/CreateMenu";

const MenuPage = () => {
  const [menus, setMenus] = useState<Menu[]>([]);
  const [open, setOpen] = useState<boolean>(false);

  // call fetchMenus function once at first rendering
  useEffect(() => {
    //fetchMenus();
  }, []);

  //fetch menus from server
  const fetchMenus = async () => {
    const response = await fetch(`${config.apiBaseUrl}/menu`);
    const menus = await response.json();
    setMenus(menus);
  };

  return (
    <BackofficeLayout>
      <Box sx={{ mr: 2 }}>
        <Box
          sx={{
            display: "flex",
            justifyContent: "flex-end",
          }}
        >
          {/* button to show CreateMenu's Dialog */}
          <Button variant="contained" onClick={() => setOpen(true)}>
            Create menu
          </Button>
        </Box>

        {/* render CreateMenu Component */}
        <CreateMenu open={open} setOpen={setOpen} setMenus={setMenus} />

        <Box sx={{ display: "flex", flexWrap: "wrap" }}>
          {/* display menu with MenuCard */}
          {menus.map((menu) => (
            <MenuCard key={menu.id} menu={menu} />
          ))}
        </Box>
      </Box>
    </BackofficeLayout>
  );
};

export default MenuPage;
```

- react app က menu page component ကိုပဲ ပြန်ကူးထည့်လိုက်တာဖြစ်ပါတယ်
- အောက်မှာတော့ MenuCard.tsx နဲ့ CreateMenu.tsx component တွေကို component folder အောက်မှာ ကူးထည့်ထားလိုက်တာပါ။

```js
// src/components/menuCard/MenuCard.tsx

import PaidIcon from "@mui/icons-material/Paid";
import { Box, Card, CardContent, CardMedia, Typography } from "@mui/material";
import Link from "next/link";
import { Menu } from "../../types/menu";

interface Props {
  menu: Menu;
}

const MenuCard = ({ menu }: Props) => {
  return (
    <Link
      href={`menu/${String(menu.id)}`}
      style={{
        textDecoration: "none",
        marginRight: "15px",
        marginBottom: "20px",
      }}
    >
      <Card sx={{ width: 200, height: 220, pb: 2 }}>
        <CardMedia
          sx={{ height: 140, backgroundSize: "contain" }}
          image={menu.assetUrl || ""}
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

```js
// src/components/createMenu/CreateMenu.tsx

import config from "@/config";
import { CreateMenuPayload, Menu } from "@/types/menu";
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
  setMenus: (menus: Menu[]) => void;
}

const defaultNewMenu = {
  name: "",
  price: 0,
  assetUrl: "",
};
const CreateMenu = ({ open, setOpen, setMenus }: Props) => {
  const [newMenu, setNewMenu] = useState < CreateMenuPayload > defaultNewMenu;

  //Create menu function
  const createMenu = async () => {
    const response = await fetch(`${config.apiBaseUrl}/menu`, {
      method: "POST",
      headers: {
        "content-type": "application/json",
      },
      body: JSON.stringify(newMenu),
    });
    const menus = await response.json();

    //update menus
    setMenus(menus);
    setNewMenu(defaultNewMenu);

    //close dialog box
    setOpen(false);
  };

  //update menu function
  const updateMenu = async () => {
    const menuToUpdate = { id: 3, name: "gin tote", price: 50000 };
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

  //delete menu function
  const deleteMenu = async () => {
    const menuIdToDelete = [1, 2, 3];
    const response = await fetch(
      `http://localhost:5000/menu/${menuIdToDelete}`,
      {
        method: "DELETE",
      }
    );
    const dataFromServer = await response.json();
    console.log("dataFromServer: ", dataFromServer);
  };

  //use function on change name
  const handleNameUpdate = (
    evt: React.ChangeEvent<HTMLInputElement | HTMLTextAreaElement>
  ) => {
    setNewMenu({ price: newMenu.price, name: evt.target.value });
  };

  return (
    <Dialog open={open} onClose={() => setOpen(false)}>
      <DialogTitle>Create menu</DialogTitle>
      <DialogContent>
        <Box
          sx={{
            display: "flex",
            alignItems: "center",
            flexDirection: "column",
          }}
        >
          <TextField
            defaultValue={newMenu.name}
            sx={{ width: 300, mb: 2 }}
            placeholder="Name"
            onChange={handleNameUpdate}
          />
          <TextField
            defaultValue={newMenu.price}
            sx={{ width: 300, mb: 4 }}
            placeholder="Price"
            onChange={(evt) =>
              setNewMenu({
                name: newMenu.name,
                price: Number(evt.target.value),
              })
            }
          />
          <Button
            variant="contained"
            sx={{ width: "fit-content" }}
            onClick={createMenu}
          >
            Create
          </Button>
        </Box>
      </DialogContent>
    </Dialog>
  );
};

export default CreateMenu;
```

- အရင် react app က code တွေကိုပဲ ပြန်ကူးထည့်ထားဖြစ်ပြီး ပြောင်းထားတာဆိုလို့ import လုပ်တဲ့ နေရာတွေ update လုပ်ထားတာပဲ ရှိပါတယ်
- လိုအပ်တဲ့ MUI package တွေကို next app project မှာ install လုပ်ပေးရမှာဖြစ်ပါတယ်

```console
$ npm install @mui/icons-material @mui/material @emotion/styled @emotion/react
```

- ခု NEXT APP မှာ MENU CREATE လုပ်ကြည့်နိုင်ဖို့ api folder အောက်မှာ backend code တွေ ထည့်ပေးရမှာဖြစ်ပါတယ်

```js
// src/pages/api/menu/index.ts

import type { NextApiRequest, NextApiResponse } from "next";

interface Menu {
  id: number;
  name: string;
  price: number;
  assetUrl?: string;
  isArchived: boolean;
}

//DEMO menus array
let menus: Menu[] = [];

export default function handler(req: NextApiRequest, res: NextApiResponse) {
  if (req.method === "POST") {
    const isValid = req.body.name;
    if (!isValid) return res.status(400).send("Bad request. Missing required");
    const newMenuId = menus.length === 0 ? 1 : menus[menus.length - 1].id + 1;
    const isArchived = false;

    //crate new menu object
    const newMenu = {
      ...req.body,
      id: newMenuId,
      assetUrl:
        "https://thumbs.dreamstime.com/b/heart-shape-various-vegetables-fruits-healthy-food-concept-isolated-white-background-140287808.jpg",
      isArchived,
    };

    menus.push(newMenu);
    return res.send(menus);
  } else if (req.method === "GET") {
    return res.send(menus);
  }
  res.status(405).send("Invalid method");
}
```

- api folder အောက်မှာ menu folder တစ်ခုထပ်လုပ်ပြီး index.ts ဖိုင် လုပ်လိုက်ပါတယ်
- next js ရဲ့ handler function ကို သုံးပြီး request တွေကို လက်ခံထားပါတယ်
- အရင် express server မှာ သုံးခဲ့တဲ့ logic အတိုင်းပဲ menu တစ်ခု create လုပ်ပြီး menus array ထဲကို push လုပ်ကာ response ပြန်ပေးလိုက်တာပဲဖြစ်ပါတယ်
- ထူးခြားချက်အနေနဲ့ data validation ကို စစ်ထားပါတယ်

```js
const isValid = req.body.name;
if (!isValid) return res.status(400).send("Bad request. Missing required");
```

- request body ထဲမှာ menu name ပါမပါကို စစ်ထားလိုက်ပြီး
- မပါလာခဲ့ရင် "Bad request. Missing required" လို့ 400 code နဲ့ response ပြန်လိုက်ပြီး retrunလုပ်ထားပါတယ်
- ဆိုလိုတာက လိုအပ်တဲ့အချက်အလက်တွေ မပါလာရင် "Bad request. Missing required" ကိုပဲ response ပြန်လိုက်ပြီး အောက်က ကုဒ်တွေ ဆက်မrun ခိုင်းတာပဲ ဖြစ်ပါတယ်
- "GET" method နဲ့ request ၀င်လာရင်တော့ menus array ကိုပဲ response ပြန်ထားလိုက်တာဖြစ်ပါတယ်

##

### Dynamic Route

- react မှာလိုပဲ `http://localhost:3000/menu/2` လို့ routing လုပ်လာရင် dynamic route နဲ့ လက်ခံပြီး component တစ်ခု ပြပေးလိုက်သလိုမျိုး ကို အရင် သင်ခန်းစာမှာ လေ့လာခဲ့ကြပါတယ်
- NestJS မှာ dinamic route ကို အခုလို create လုပ်လို့ရပါတယ်
- ![](https://cdn.discordapp.com/attachments/1153197808900395062/1153365204235718686/image.png)

```js
// src/pages/backoffice/menu/[id]/index.tsx

import BackofficeLayout from "@/components/backofficeLayout";
import { useRouter } from "next/router";

const UpdateMenu = () => {
  const router = useRouter();
  const menuId = router.query.id;
  return (
    <BackofficeLayout>
      <h1>Menu id to update: {menuId}</h1>
    </BackofficeLayout>
  );
};

export default UpdateMenu;
```

- menu folder အောက်မှာ dynamic route ကို လက်ခံတဲ့ [id] ဆိုတဲ့ folder တစ်ခု လုပ်ထားလိုက်ပါတယ်
- folder name ကို **[id] လို့ပဲ အတိအကျ ထားပေး**ရမှာဖြစ်ပြီး
  `http:localhost:3000/backoffice/menu/id` id နေရာမှာ ဘာ နံပါတ်ပဲ ၀င်လာလာ [id] folder ထဲက index.tsx ကို render လုပ်ပေးမှာဖြစ်ပါတယ်

- [id] folder ထဲက index.tsx ထဲမှာတော့ next/router ကို သုံးပြီး router တစ်ခု သတ်မှတ်ကာ oင်လာမယ့် id တွေကို router.query.id နဲ့ လှမ်းယူလိုက်ပြီး အောက်မှာ ပြပေးထားတာပဲဖြစ်ပါတယ်

- အားလုံးပြင်ဆင်ပြီးပြီ ဆိုရင် next app ကို စ run ပြီး menu page ထဲ၀င်ကာ menu တွေ create လုပ်ကြည့်ပါက react app မှာလိုပဲ ပြပေးတာကို ြမင်ရမှာပါ
- create လုပ်ထားတဲ့ menu ကို နှိပ်ကြည့်လိုက်ပါက dynamic route ကို လက်ခံထားတဲ့ UpdateMenu component ဆီ ရောက်သွားမှာဖြစ်ပြီး ၀င်လာတဲ့ params ကို menu id အနေနဲ့ ပြပေးနေမှာဖြစ်ပါတယ်

##

### အထက်ပါအတိုင်းပဲ menu category အတွက် ဆက်ပြီး လုပ်ကြည့်လိုက်ကြပါမယ်

```js
// src/pages/backoffice/menu-category/index.tsx

import BackofficeLayout from "@/components/backofficeLayout";
import ItemCard from "@/components/itemCard/ItemCard";
import CreateMenuCategory from "@/components/menuCategory/CreateMenuCategory";
import { MenuCategory } from "@/types/menuCategory";
import CategoryIcon from "@mui/icons-material/Category";
import { Box, Button } from "@mui/material";
import { useState } from "react";

const MenuCategoryPage = () => {
  const [menuCategories, setMenuCategories] = useState<MenuCategory[]>([]);
  const [open, setOpen] = useState<boolean>(false);
  return (
    <BackofficeLayout>
      <Box sx={{ mr: 2 }}>
        <Box
          sx={{
            display: "flex",
            justifyContent: "flex-end",
          }}
        >
          {/* button to show CreateMenu's Dialog */}
          <Button variant="contained" onClick={() => setOpen(true)}>
            Create menu category
          </Button>
        </Box>

        {/* render CreateMenuCategory Component */}
        <CreateMenuCategory
          open={open}
          setOpen={setOpen}
          setMenuCategories={setMenuCategories}
        />

        <Box sx={{ display: "flex", flexWrap: "wrap" }}>
          {/* display menu with MenuCard */}
          {menuCategories.map((menuCategory) => (
            <ItemCard
              href={`/backoffice/menu-category/${menuCategory.id}`}
              icon={<CategoryIcon />}
              key={menuCategory.id}
              title={menuCategory.name}
            />
          ))}
        </Box>
      </Box>
    </BackofficeLayout>
  );
};

export default MenuCategoryPage;
```

- menu အတွက် လုပ်ထားတဲ့ code တွေကို ပဲ ပြန်ကူးယူထားတာဖြစ်ပြီး
- menu category တွေ ကို ပြတဲ့နေရာမှာ menu card ကို မသုံးပဲ ItemCard component ကို အသုံးပြုထားလိုက်ပါတယ်

```js
// src/components/itemCard/ItemCard.tsx

import { Paper, Typography } from "@mui/material";
import Link from "next/link";
import { ReactNode } from "react";

interface Props {
  icon: ReactNode;
  title: string;
  href?: string;
  subtitle?: string;
}

const ItemCard = ({ icon, title, href, subtitle }: Props) => {
  if (href) {
    return (
      <Link href={href} style={{ textDecoration: "none", color: "#000000" }}>
        <Paper
          elevation={2}
          sx={{
            width: 170,
            height: 170,
            p: 2,
            display: "flex",
            flexDirection: "column",
            justifyContent: "center",
            alignItems: "center",
            m: 2,
          }}
        >
          {icon}
          <Typography sx={{ color: "#4C4C6D", fontWeight: "700" }}>
            {title}
          </Typography>
          {subtitle && (
            <Typography sx={{ color: "#4C4C6D", fontSize: 14 }}>
              {subtitle}
            </Typography>
          )}
        </Paper>
      </Link>
    );
  }

  return (
    <Paper
      elevation={2}
      sx={{
        width: 170,
        height: 170,
        p: 2,
        display: "flex",
        flexDirection: "column",
        justifyContent: "center",
        alignItems: "center",
        m: 2,
      }}
    >
      {icon}
      <Typography sx={{ color: "#4C4C6D", fontWeight: "700" }}>
        {title}
      </Typography>
      {subtitle && (
        <Typography sx={{ color: "#4C4C6D", fontSize: 14 }}>
          {subtitle}
        </Typography>
      )}
    </Paper>
  );
};

export default ItemCard;
```

- MUI Paper component ကို သုံးပြီး menu category တွေကို ပြပေးထားတာဖြစ်ပါတယ်
- `icon`, `title`, `href`, `subtitle` ဆိုတဲ့ parameter လေးခုလက်ခံထားပြီး အောက်မှာ အသုံးပြုထားပါတယ်
- nextJS Link compoent ကို သုံးပြီး menu category တစ်ခုခုကို နှိပ်လိုက်ရင် href အဖြစ် ၀င်လာမယ့် route ကို သွားခိုင်းလိုက်တာဖြစ်ပါတယ်
- ကျန်တဲ့ parameter တွေကို လည်းအောက်မှာ သူ့နေရာနဲ့သူ သုံးပြီး menu category တွေကို ပြပေးထားတာပဲဖြစ်ပါတယ်
- MenuCategoryPage မှာလည်း ItemCard component ကို ခေါ်သုံးတဲ့အခါ href parameter အနေနဲ့ menucategory id နဲ့ dynamic route တစ်ခု အဖြစ် ထည့်ပေးထားတာဖြစ်ပါတယ်
- menu အတွက် လုပ်ခဲ့သလိုပဲ menu category အတွက်လဲ create component တစ်ခုနဲ့ dynamic route အနေနဲ့ ၀င်လာရင် ပြပေးနိုင်ဖို့ component တစ်ခု လုပ်လိုက်ပါမယ်
  ![](https://cdn.discordapp.com/attachments/1153197808900395062/1153374328637894718/image.png)

```js
// src/pages/backoffice/menu-category/[id]/index.tsx

import BackofficeLayout from "@/components/backofficeLayout";
import { useRouter } from "next/router";

const UpdateMenuCategory = () => {
  const router = useRouter();
  const menuCategoryId = router.query.id;
  return (
    <BackofficeLayout>
      <h1>Menu category id to update: {menuCategoryId}</h1>
    </BackofficeLayout>
  );
};

export default UpdateMenuCategory;
```

![](https://cdn.discordapp.com/attachments/1153197808900395062/1153374972211908758/image.png)

```js
// src/components/menuCategory/CreateMenuCategory.tsx

// CreateMenu Component with MUI Dialog box

import config from "@/config";
import { CreateMenuCategoryPayload, MenuCategory } from "@/types/menuCategory";
import {
  Box,
  Button,
  Dialog,
  DialogContent,
  DialogTitle,
  FormControlLabel,
  Switch,
  TextField,
} from "@mui/material";
import { useState } from "react";

interface Props {
  open: boolean;
  setOpen: (value: boolean) => void;
  setMenuCategories: (menuCategories: MenuCategory[]) => void;
}

const defaultNewMenuCategory = {
  name: "",
  isAvailable: true,
};

const CreateMenuCategory = ({ open, setOpen, setMenuCategories }: Props) => {
  const [newMenuCategory, setNewMenuCategory] =
    useState < CreateMenuCategoryPayload > defaultNewMenuCategory;

  //Create menu category function
  const createMenuCategory = async () => {
    const response = await fetch(`${config.apiBaseUrl}/menu-category`, {
      method: "POST",
      headers: {
        "content-type": "application/json",
      },
      body: JSON.stringify(newMenuCategory),
    });
    const menuCategories = await response.json();

    //update menus
    setMenuCategories(menuCategories);
    setNewMenuCategory(defaultNewMenuCategory);

    //close dialog box
    setOpen(false);
  };

  //use function on change name
  const handleNameUpdate = (
    evt: React.ChangeEvent<HTMLInputElement | HTMLTextAreaElement>
  ) => {
    setNewMenuCategory({
      isAvailable: newMenuCategory.isAvailable,
      name: evt.target.value,
    });
  };

  return (
    <Dialog open={open} onClose={() => setOpen(false)}>
      <DialogTitle>Create menu category</DialogTitle>
      <DialogContent>
        <Box
          sx={{
            display: "flex",
            flexDirection: "column",
          }}
        >
          <TextField
            defaultValue={newMenuCategory.name}
            sx={{ width: 300, mb: 2 }}
            placeholder="Name"
            onChange={handleNameUpdate}
          />
          <FormControlLabel
            control={
              <Switch
                defaultChecked={newMenuCategory.isAvailable}
                onChange={(evt, value) =>
                  setNewMenuCategory({
                    name: newMenuCategory.name,
                    isAvailable: value,
                  })
                }
              />
            }
            label="Available"
            sx={{ mb: 2 }}
          />
          <Box sx={{ display: "flex", justifyContent: "center" }}>
            <Button
              variant="contained"
              sx={{ width: "fit-content" }}
              onClick={createMenuCategory}
            >
              Create
            </Button>
          </Box>
        </Box>
      </DialogContent>
    </Dialog>
  );
};

export default CreateMenuCategory;
```

- code တွေက menu အတွက် လုပ်ခဲ့တုန်းကလိုပဲ အတူတူပဲမို့လို့ ရှင်းမပြတော့ပါဘူး
- အခု api folder မှာ menu category အတွက် menu တုန်းကလိုပဲ request/response လုပ်မှာဖြစ်ပါတယ်

![](https://cdn.discordapp.com/attachments/1153197808900395062/1153375894266716261/image.png)

- code တွေက menu အတွက် လုပ်ခဲ့တုန်းကလိုပဲ logic အတူတူပဲဖြစ်ပါတယ်
- ခုဆိုရင် menu category အတွက် လဲ create and render လုပ်ပေးလို့ရပြီ ဖြစ်ပါတယ်
