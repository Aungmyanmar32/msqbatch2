## MSquare Programming Fullstack Course

### Batch 2

### Episode-_13_ Summary

- routing
- AppRouter
- Server side routing and client side routing
- a tag and link component
- Layout component
- children props

##

### create react router

- react မှာ routing လုပ်တယ်ဆိုတာက page တစ်ခုကနေ နောက်page တစ်ခုကို သွားနိုင်ဖို့ လမ်းကြောင်းသတ်မှတ်ပေးတာဖြစ်ပါတယ်
- react မှာ router system တစ်ခုလုပ်ဖို့ react-router-dom ဆိုတဲ့ package တစ်ခုကို install လုပ်ပေးရပါမယ်

```console
$ npm i react-router-dom
```

- အရင်ဆုံး src folder အောက်မှာ pages folder တစ်ခု တည်ဆောက်ပြီး Menu.tsx နဲ့ MenuCaotegory.tsx file နှစ်ခုလုပ်လိုက်ပါမယ်

```js
// src/pages/menu/Menu.tsx

import { Box, Typography } from "@mui/material";
import React from "react";

const Menu = () => {
  return (
    <Box>
      <Typography variant="h1"> Menu page</Typography>
    </Box>
  );
};

export default Menu;
```

```js
// src/pages/menuCategory/MenuCategory.tsx

import { Box, Typography } from "@mui/material";
import React from "react";

const MenuCategory = () => {
  return (
    <Box>
      <Typography variant="h1"> MenuCategory page</Typography>
    </Box>
  );
};

export default MenuCategory;
```

- App.tsx မှာလည်း အောက်ကလို ပြင်ရေးလိုက်ပါမယ်

```js
// src/App.tsx

import { Box, Typography } from "@mui/material";
import React from "react";

const App = () => {
  return (
    <Box>
      <Typography variant="h1"> Home page</Typography>
    </Box>
  );
};

export default App;
```

- react router dom ကို သုံးပြီး AppRouter.tsx component တစ်ခုကို create ပြီး routing system တစ်ခု တည်ဆောက်လိုက်ပါမယ်

```js
// src/components/appRouter/AppRouter.tsx

import { BrowserRouter, Route, Routes } from "react-router-dom";
import App from "../../App";
import Menu from "../../pages/menu/Menu";
import MenuCategory from "../../pages/menuCategory/MenuCategory";

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
      <Routes>
        <Route path="/" Component={App} />
        <Route path="/menu" Component={Menu} />
        <Route path="/menuCategory" Component={MenuCategory} />
      </Routes>
    </BrowserRouter>
  );
};

export default AppRouter;
```

- "/ " နဲ့ ၀င်လာရင် App component ကို render လုပ်ခိုင်းထားတာဖြစ်ပါတယ်
- ထိုနည်းတူပဲ "/menu" route နဲ့ ၀င်လာရင် Menu component
- "/menuCategory" route နဲ့ ၀င်လာရင် MenuCategory component ကို render လုပ်ပေးဖို့ routing သတ်မှတ်ပေးထားတာဖြစ်ပါတယ်
- အဲဒီ routing system ကို main file ဖြစ်တဲ့ index.tsx မှာ render လုပ်ပေးရမှာဖြစ်ပါတယ်။

```js
// src/index.tsx

import ReactDOM from "react-dom/client";
import "./index.css";

import AppRouter from "./components/appRouter/AppRouter";

const root = ReactDOM.createRoot(
  document.getElementById("root") as HTMLElement
);
root.render(<AppRouter />);

```

##

### server side routing VS client side routing

- react မှာ route တစ်ခုခုထဲ၀င်လာရင် network request လုပ်ပြီး လိုအပ်တဲ့ ဖိုင်တွေ download လုပ်ကာ သက်ဆိုင်ရာ component ကို render လုပ်ပေးမှာဖြစ်ပါတယ်
- react app ကို start လိုက်ပြီး browser မှာ ၀င်ကြည့်ပါက root route ကို ၀င်လာတဲ့အချိန်မှာ App component ကို ပြပေးနေတာကို မြင်ရမှာဖြစ်ပါတယ်

![](https://cdn.discordapp.com/attachments/1146496852087287898/1148299595386998794/image.png)

- ပထမ အကြိမ် routing လုပ်တာက **server ဆီ request လုပ်ပြီး file တွေ download လုပ်တာမလို့ server side routing** လို့ သတ်မှတ်ရပါမယ်

- browser ကနေ route လုပ်ပြီး တိုက်ရိုက်၀င်ကြည့်ရင်လဲ server side routing ဖြစ်ပါမယ်

![](https://cdn.discordapp.com/attachments/1146496852087287898/1148299739696214016/image.png)
![](https://cdn.discordapp.com/attachments/1146496852087287898/1148300980035788900/image.png)

- နောက်ထပ် `<a>` tag ကို သုံးပြီး href နဲ့ link လုပ်လိုက်ရင်လဲ server side routing ဖြစ်ပါမယ်

```js
// src/App.tsx

import { Box, Typography } from "@mui/material";
import React from "react";

const App = () => {
  return (
    <Box
      sx={{ display: "flex", flexDirection: "column", alignItems: "center" }}
    >
      <Typography variant="h1"> Home page</Typography>
      <br />
      <a href="/menu">Goto Menu Page</a>
      <br />
      <br />
      <a href="/menuCategory">Goto Menu Category Page</a>
    </Box>
  );
};

export default App;
```

![](https://cdn.discordapp.com/attachments/1146496852087287898/1148303205298606241/image.png)

- Home page အောက်က a tag တွေနဲ့ သွားကြည့်ပါက အထက်ကလိုပဲ သက်ဆိုင်ရာ page တွေဆီ ရောက်သွားမှာဖြစ်ပြီး network rquest လုပ်ကာ file တွေ download လုပ်ယူပြီး ပြပေးတာကို မြင်ရမှာဖြစ်ပါတယ်
- server side routing တွေက resource အသုံးပြုတာများပြီး တစ်ခါ၀င်တိုင်း network request နဲ့ download လုပ်နေတာမလို့ အချိန်အနည်းငယ် ပိုကြာနိုင်ပါတယ်
- client side routing မှာတော့ route တစ်ခါလုပ်တိုင်း network request မလိုပဲ ရှိပြီးသားဖိုင်တွေနဲ့ပဲ route လုပ်ပေးတာမလို့ အထက်ပါ ပြဿနာကို ဖြေရှင်းပေးနိုင်ပါတယ်
- react-router-dom က `<Link>` component ကို သုံးပြီး routing တွေကို client side routing အဖြစ် သတ်မှတ်ပေးနိုင်ပါတယ်
- `<Link>` component မှာ `to` ဆိုတဲ့ props မှာ routing လုပ်ချင်တဲ့ route ကို ထည့်ပေးရမှာဖြစ်ပါတယ်

```js
// src/App.tsx

import { Box, Typography } from "@mui/material";
import React from "react";
import { Link } from "react-router-dom";

const App = () => {
  return (
    <Box
      sx={{ display: "flex", flexDirection: "column", alignItems: "center" }}
    >
      <Typography variant="h1"> Home page</Typography>
      <br />
      <a href="/menu">Goto Menu Page</a>
      <br />
      <br />
      <a href="/menuCategory">Goto Menu Category Page</a>
      <br />
      <br />
      <br />
      <br />
      <Link to={"/menu"}>GO to Menu by Link</Link>
      <br />
      <Link to={"/menuCategory"}>Go to MenuCategory by Link</Link>
    </Box>
  );
};

export default App;
```

![](https://cdn.discordapp.com/attachments/1146496852087287898/1148304506308808825/image.png)

- GO to Menu by Link နဲ့ menu route ကို သွားကြည့်ပါက network rquest ထပ်မလုပ်ပဲ တိုက်ရိုက်ရောက်သွားတာကို မြင်ရမှာဖြစ်ပါတယ်

![](https://cdn.discordapp.com/attachments/1146496852087287898/1148304591780319353/image.png)

##

### Layout

- react component တွေကို Layout ချပြီး လုပ်ဖို့အတွက် Layout.tsx component တစ်ခုလုပ်ပါမယ်
- Layout ထဲမှာ app bar တွေ နဲ့ side bar တွေပါ ထည့်ချင်တာမလို့ mui component တွေကို သုံးပြီး လုပ်လိုက်ပါမယ်

```js
// src/components/layout/Layout.tsx

import CategoryIcon from "@mui/icons-material/Category";
import RestaurantMenuIcon from "@mui/icons-material/RestaurantMenu";
import {
  Box,
  ListItemButton,
  ListItemIcon,
  ListItemText,
  Typography,
} from "@mui/material";
import { ReactNode } from "react";
import { Link } from "react-router-dom";

interface Props {
  children: ReactNode;
}

const Layout = ({ children }: Props) => {
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
          Foodie App
        </Typography>
      </Box>

      {/* side bar */}
      <Box sx={{ display: "flex", minHeight: "100vh" }}>
        <Box sx={{ width: 300, bgcolor: "#1B9C85", borderTopRightRadius: 20 }}>
          <Link to={"/menu"} style={{ textDecoration: "none" }}>
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
          <Link to={"/menuCategory"} style={{ textDecoration: "none" }}>
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

export default Layout;
```

- ကျနော်တို့က ဘယ် component ပဲ render လုပ်လုပ် ခုလုပ်ထားတဲ့ layout နဲ့ တွဲပြီး ပြချင်တာမလို့ Layout component မှာ children ဆိုတဲ့ props တစ်ခု လက်ခံထားလိုက်ပြီး return နောက်ဆုံးနားလေးမှာ children ဆိုတဲ့ props ကို ပြန်ပြီး render လုပ်ထားလိုက်ပါတယ်
- **Layout component နဲ့ wrap လုပ်ခံထားရတဲ့ မည်သည့်component မဆို Layout component မှာ children props အနေနဲ့ ၀င်လာ**မှာဖြစ်ပါတယ်
- အဲ့လို props အနေနဲ့ ၀င်လာတဲ့ component တွေကို Layout component မှာ ပြန်ပြီး render လုပ်ပေးထားတာပဲဖြစ်ပါတယ်
- နမူနာ အနေနဲ့ App component ကို Laypout component နဲ့ wrap လုပ်ပြီး ပြကြည့်ပါမယ်

```js
// src/App.tsx

import { Box, Typography } from "@mui/material";
import React from "react";
import { Link } from "react-router-dom";
import Layout from "./components/layout/Layout";

const App = () => {
  return (
    <Layout>
      <Box
        sx={{ display: "flex", flexDirection: "column", alignItems: "center" }}
      >
        <Typography variant="h1"> Home page</Typography>
      </Box>
    </Layout>
  );
};

export default App;
```

-** wrap လုပ်တယ် ဆိုတာက App component မှာ return ပြန်ထားတဲ့ JSX element ကို `<Layout>..... </Loyout>` နှစ်ခုထဲ ထည့်ပေးလိုက်တာပဲဖြစ်ပါတယ်**

- react app ထဲမှာ "/ " route ကို ၀င်ကြည့်လိုက်ရင် ခုလိုပြပေးနေမှာဖြစ်ပါတယ်

![](https://cdn.discordapp.com/attachments/1146496852087287898/1148308833811316756/image.png)

- ဒါပေမယ့် ဘေးနားက ခလုတ်လေးတွေကိုနှိပ်ပြီး menu နဲ့ menuCategory route တွေ ၀င်ကြည့်တဲ့အခါ အဲ့ဒီ component တွေကို Layout နဲ့ warp မလုပ်ထားလို့ layout မပါပဲ ပြပေးနေမှာဖြစ်ပါတယ်
- ဘယ် route ထဲပဲ ၀င်၀င် layout နဲ့ ပြစေချင်တယ် ဆိုရင်**routing system လုပ်ထားတဲ့ AppRouter component မှာ wrap လုပ်ပေးလိုက်ရင် ရှိရှိသမျှ component တွေ အကုန် wrap လုပ်ပြီးသားဖြစ်ပြီမလို့ component တစ်ခုချင်းစီမှာ လိုက်ပြီး Layout နဲ့ wrap လုပ်စရာမလို**တော့ပါဘူး
- တစ်ခုသတိထားရမှာက routing လုပ်ထားတဲ့ component မှာ layout နဲ့ wrap လုပ်မယ်ဆို**BrowserRouter အောက်ကနေ စပြီး wrap လုပ်ပေး**ရမှာဖြစ်ပါတယ်

```js

```

![](https://cdn.discordapp.com/attachments/1146496852087287898/1148310833089216652/image.png)

- routing component မှာ Layout နဲ့ wrap လုပ်ပြီးပြီ မလို့ App component မှာ warp လုပ်ထားတာကို ပြန်ဖျက်လိုက်ပါမယ်

![](https://cdn.discordapp.com/attachments/1146496852087287898/1148311616459395132/image.png)

- ခုဆိုရင် react app မှာ menu နဲ့ menuCategory route တွေမှာလည်း layout နဲ့ ပြပေးတာကို မြင်ရမှာဖြစ်ပါတယ်။
- ![](https://cdn.discordapp.com/attachments/1146496852087287898/1148311824563974255/image.png)
  ![](https://cdn.discordapp.com/attachments/1146496852087287898/1148312040239267912/image.png)
