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
-
