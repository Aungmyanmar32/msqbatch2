## MSquare Programming Fullstack Course

### Batch 2

### Episode-_27_ Summary

### 1. Create layout

### 2. Setup pages and components

### 3. Using next auth in backend(getServerSession)

### 4. Create new user

- ပြီးခဲ့တဲ့ သင်ခန်းစာမှာ database ကို setup လုပ်ခဲ့ကြပါတယ်
- ဒီနေ့သင်ခန်းစာမှာတော့ backoffice app အတွက် layout နဲ့ theme တွေကို create လုပ်ကြပါမယ်

### 1. Create layout

- layout ဆိုတာက backoffice app ထဲက ဘယ် route(page)ကိုပဲ ၀င်၀င် ပြပေးမယ့် ပုံစံဖြစ်ပြီး
- theme ဆိုကတော့ app မှာ သုံးမယ့် color တွေကို ရွေးချယ်ပြီး ခေါ်သုံးလို့ရအောင် သတ်မှတ်ပေးထားလိုက်ကို ဆိုလိုတာပါ
- theme အဖြစ်သုံးမယ့် themeဖိုင် နဲ့ layout အတွက် Layout , TopBar , SideBar စတဲ့ component သုံးခုသတ်မှတ်လိုက်ပါမယ်

```js
// src/utils/theme.ts

import { createTheme } from "@mui/material/styles";
export const theme = createTheme({
  palette: {
    primary: {
      main: "#4C4C6D",
    },
    secondary: {
      main: "#FFE194",
    },
    info: {
      main: "#E8F6EF",
    },
    success: {
      main: "#1B9C85",
    },
  },
});
```

- theme.ts မှာတော့ mui createTheme ကို သုံးပြီး `primary` `secondary` `info` `success` စတဲ့color တွေကို သတ်မှတ်ထားလိုက်ပါတယ်

```js
// src/components/TopBar.tsx

import { Box, Button, Typography } from "@mui/material";
import { signOut, useSession } from "next-auth/react";
import Image from "next/image";
import logo from "../assets/logo.png";

const Topbar = () => {
  const { data } = useSession();
  return (
    <Box
      sx={{
        bgcolor: "success.main",
        display: "flex",
        alignItems: "center",
        justifyContent: "space-between",
        px: 2,
      }}
    >
      <Box sx={{ height: 70 }}>
        <Image
          src={logo}
          alt="logo"
          style={{ width: "100%", height: "100%" }}
        />
      </Box>
      <Typography variant="h5" color={"secondary"}>
        Foodie POS
      </Typography>
      {data ? (
        <Box>
          <Button
            variant="contained"
            onClick={() => signOut({ callbackUrl: "/" })}
          >
            Sign out
          </Button>
        </Box>
      ) : (
        <span />
      )}
    </Box>
  );
};

export default Topbar;
```

- Top bar မှာတော့ logo တစ်ခုရယ် Foodie POS ဆိုတဲ့ title တစ်ခုရယ် Sign out ခလုတ်ရယ်ကို ပြပေးထားပါတယ်
- Sign out ခလုတ်ကို ပြပေးတဲ့အခါမှာလည်း user က login ၀င်ထားမှ(session data ရှိမှ) ပြပေးမှာဖြစ်ပြီး data မရှိခဲ့ရင်တော့ empty span tag တစ်ခုပဲ ထည့်ပေးလိုက်မှာဖြစ်ပါတယ်

```js
// src/components/SideBar.tsx

import CategoryIcon from "@mui/icons-material/Category";
import ClassIcon from "@mui/icons-material/Class";
import EggIcon from "@mui/icons-material/Egg";
import LocalDiningIcon from "@mui/icons-material/LocalDining";
import LocalMallIcon from "@mui/icons-material/LocalMall";
import LocationOnIcon from "@mui/icons-material/LocationOn";
import SettingsIcon from "@mui/icons-material/Settings";
import TableBarIcon from "@mui/icons-material/TableBar";
import {
  Box,
  Divider,
  List,
  ListItem,
  ListItemButton,
  ListItemIcon,
  ListItemText,
} from "@mui/material";
import Link from "next/link";

export const sidebarMenuItems = [
  {
    id: 1,
    label: "Orders",
    icon: <LocalMallIcon />,
    route: "/backoffice/orders",
  },
  {
    id: 2,
    label: "Menu Categories",
    icon: <CategoryIcon />,
    route: "/backoffice/menu-categories",
  },
  {
    id: 3,
    label: "Menus",
    icon: <LocalDiningIcon />,
    route: "/backoffice/menus",
  },
  {
    id: 4,
    label: "Addon Categories",
    icon: <ClassIcon />,
    route: "/backoffice/addon-categories",
  },
  {
    id: 5,
    label: "Addons",
    icon: <EggIcon />,
    route: "/backoffice/addons",
  },
  {
    id: 6,
    label: "Tables",
    icon: <TableBarIcon />,
    route: "/backoffice/tables",
  },
  {
    id: 7,
    label: "Locations",
    icon: <LocationOnIcon />,
    route: "/backoffice/locations",
  },
  {
    id: 8,
    label: "Settings",
    icon: <SettingsIcon />,
    route: "/backoffice/settings",
  },
];

const SideBar = () => {
  return (
    <Box
      sx={{
        minWidth: 250,
        backgroundColor: "success.main",
        borderTopRightRadius: "20px",
        minHeight: "100vh",
      }}
    >
      <List sx={{ p: 0 }}>
        {sidebarMenuItems.slice(0, 7).map((item) => (
          <Link
            key={item.id}
            href={item.route}
            style={{ textDecoration: "none", color: "#313131" }}
          >
            <ListItem
              disablePadding
              sx={{ "&.hover": { backgroundColor: "blue" } }}
            >
              <ListItemButton>
                <ListItemIcon sx={{ color: "info.main" }}>
                  {item.icon}
                </ListItemIcon>
                <ListItemText
                  primary={item.label}
                  sx={{ color: "info.main" }}
                />
              </ListItemButton>
            </ListItem>
          </Link>
        ))}
      </List>
      <Divider
        variant={"middle"}
        sx={{ backgroundColor: "secondary.main", mt: 2 }}
      />
      <List>
        {sidebarMenuItems.slice(-1).map((item) => (
          <Link
            key={item.id}
            href={item.route}
            style={{ textDecoration: "none", color: "#313131" }}
          >
            <ListItem disablePadding>
              <ListItemButton>
                <ListItemIcon sx={{ color: "info.main" }}>
                  {item.icon}
                </ListItemIcon>
                <ListItemText
                  primary={item.label}
                  sx={{ color: "info.main" }}
                />
              </ListItemButton>
            </ListItem>
          </Link>
        ))}
      </List>
    </Box>
  );
};

export default SideBar;
```

- Side bar မှာတော့ list အနေနဲ့ ပြမယ့် array တစ်ခုကို သတ်မှတ်လိုက်ပါတယ်
- array item အဖြစ်
  - id
  - label
  - icon
  - route
- စတာတွေပါတဲ့ object တွေကို ထည့်ပေးထားလိုက်ပါတယ်
- အဲ့ဒီ array items တွေကို map လုပ်ပြီး side bar menu အဖြစ် ပြပေးထားပါတယ်
- color တွေ အဖြစ်လဲ theme ထဲမှာ သတ်မှတ်ထားတဲ့ color code တွေကို ထည့်သွင်း အသုံးပြုထားပါတယ်
-

```js
// src/components/layout.tsx

import { config } from "@/utils/config";
import { Box } from "@mui/material";
import { useSession } from "next-auth/react";
import { ReactNode, useEffect } from "react";
import SideBar from "./Sidebar";
import Topbar from "./Topbar";

interface Props {
  children: ReactNode;
}

const Layout = ({ children }: Props) => {
  const { data } = useSession();

  useEffect(() => {
    if (data) {
      fetchData();
    }
  }, [data]);

  const fetchData = async () => {
    const response = await fetch(`${config.apiBaseUrl}/app`);
    const dataFromServer = await response.json();
    console.log(dataFromServer);
  };

  return (
    <Box>
      <Topbar />
      <Box sx={{ display: "flex", position: "relative", zIndex: 5, flex: 1 }}>
        {data && <SideBar />}
        <Box sx={{ p: 3, width: "100%", height: "100%" }}>{children}</Box>
      </Box>
    </Box>
  );
};

export default Layout;
```

- layout မှာတော့ useEffect နဲ့ /api/app route ဆီ data က update ဖြစ်တိုင်း fetch လုပ်ထားပြီး serverဆီ request ပို့ထားပါတယ်
- အောက်မှာတော့ top bar, side bar နဲ့ children propsကို render လုပ်ပြီး ပြပေးထားတာပဲဖြစ်ပါတယ်

### theme နဲ့ layout ကို component တိုင်းမှာ သုံးလို့ရအောင် \_app.tsx မှာ wrap လုပ်ပေးရမှာဖြစ်ပါတယ်

```js
// src/pages/_app.tsx

import Layout from "@/components/Layout";
import { store } from "@/store";
import { theme } from "@/utils/theme";
import { ThemeProvider } from "@emotion/react";
import { SessionProvider } from "next-auth/react";
import type { AppProps } from "next/app";
import { Provider } from "react-redux";
import "../styles/global.css";

export default function App({ Component, pageProps }: AppProps) {
  return (
    //next-auth
    // store
    // theme
    // layout
    <SessionProvider>
      <Provider store={store}>
        <ThemeProvider theme={theme}>
          <Layout>
            <Component {...pageProps} />
          </Layout>
        </ThemeProvider>
      </Provider>
    </SessionProvider>
  );
}
```

- home page မှာလည်း app ထဲ၀င်လာရင် signin လုပ်ထား မထားကို စစ်ပြီး
- sign in လုပ်ထားရင် /backoffice/orders ကို routing လုပ်ပေးလိုက်မှာဖြစ်ပြီး
- sign in မလုပ်ထားရင်တော့ google နဲ့ log in ၀င်ဖို့ ခလုတ်တစ်ခုကို ပြပေးထားလိုက်ပါမယ်

```js
// src/pages/index.tsx

import { Box, Button } from "@mui/material";
import { signIn, useSession } from "next-auth/react";
import { useRouter } from "next/router";

export default function Home() {
  const { data } = useSession();
  const router = useRouter();
  if (!data) {
    return (
      <Box
        sx={{
          display: "flex",
          justifyContent: "center",
          alignItems: "center",
          height: "80vh",
        }}
      >
        <Button
          variant="contained"
          onClick={() => signIn("google", { callbackUrl: "/" })}
        >
          Sign in
        </Button>
      </Box>
    );
  } else {
    router.push("/backoffice/orders");
  }
}
```

- sign in လုပ်ပြီးချိန်မှာ ပြပေးနိုင်ဖို့ /backoffice/orders အတွက် page တစ်ခု လုပ်လိုက်ပါမယ်

```js
// src/pages/backoffice/orders/index.ts

const OrdersPage = () => {
  return <h1>Orders page</h1>;
};

export default OrdersPage;
```

- ခု next app ကို run ကြည့်ပါက အောက်ကပုံလို ပြပေးနေမှာဖြစ်ပါတယ်

![](https://github.com/Aungmyanmar32/msqbatch2/blob/main/image.png?raw=true)

##

### 2. Setup pages and components

- side bar မှာ ရှိတဲ့ item တွေကို နှိပ်လိုက်ရင် သက်ဆိုင်ရာ page တွေကို routed လုပ်ပြီး ပြပေးနိုင်ဖို့ pages folder တွေကို လုပ်လိုက်ပါမယ်

![](https://cdn.discordapp.com/attachments/1153197808900395062/1161728607400509650/image.png?ex=65395b13&is=6526e613&hm=2ead04e5fac845bb253021fe3119e456c768264ad623dc811f2f9a23a205a7a3&)

- backoffice folder အောက်မှာ page folder (route)တွေလုပ်ပေးလိုက်ပါတယ်
- page folder တစ်ခုချင်းစီမှာ index.tsx ဖိုင်တွေ ရှိမှာဖြစ်ပြီး လောလောဆယ်တော့ page-name အတိုင်းပဲ h1 tag နဲ့ return လုပ်ပေးထားလိုက်ပါမယ်

```js
import React from 'react'

const Page-Name = () => {
  return (
    <h1>Page-Name</h1>
  )
}

export default Page-Name;
```

### example

- menu folder အောက်က index.tsx မှာ ခုလိုမျိုးလေး ပြပေးပေးမှာဖြစ်ပါတယ်

```js
import React from "react";

const MenuPage = () => {
  return <h1>Menu-Page</h1>;
};

export default MenuPage;
```

- ကျန်တဲ့ page တွေကို လည်း အထက်က နမူနာ အတိုင်း ပြထားပေးလိုက်မှာဖြစ်ပါတယ်

##

### 3. Using next auth in backend(getServerSession)

- သင်ခန်းစာအရှေ့ပိုင်း က layout လုပ်တဲ့နေရာမှာ /api/app ဆီကို request လုပ်ထားတဲ့တာကို မှတ်မိကြမယ် ထင်ပါတယ်
- အဲ့ဒီ request ကို လက်ခံဖို့ api folder ထဲမှာ app folder တစ်ခု လုပ်ပြီး ပြင်ဆင်လိုက်ပါမယ်

```js
// src/pages/api/app/index.ts

// Next.js API route support: https://nextjs.org/docs/api-routes/introduction
import { prisma } from "@/utils/db";
import type { NextApiRequest, NextApiResponse } from "next";
import { getServerSession } from "next-auth";
import { authOptions } from "../auth/[...nextauth]";

export default async function handler(
  req: NextApiRequest,
  res: NextApiResponse
) {
  const session = await getServerSession(req, res, authOptions);
  if (!session) return res.status(401).send("Unauthorized.");
  const user = session.user;
  const name = user?.name as string;
  const email = user?.email as string;
  console.log("User-data :", name, email);

  res.status(200).send(user);
}


```

- next auth ရဲ့ getServerSession ကို သုံးပြီး login ၀င်ထားခြင်းရှိမရှိ စစ်လိုက်ပါတယ်
- session မရှိခဲ့ရင် 401 Unauthorized ကို ပဲ response ပြန်လိုက်ပြီး return လုပ်လိုက်ပါတယ်
- session ရှိခဲ့ရင်တော့ session ထဲက user object ကို ယူလိုက်ပြီး user name နဲ့ email ကို log ထုတ်ကြည့်ထားပါတယ်
- getServerSession ကို သုံးတဲ့အချိန်မှာ parameter သုံးခု ထည့်ပေးရမှာဖြစ်ပါတယ်
- နောက်ပြီး .env ဖိုင်ထဲမှာလည်း next auth url နဲ့ secert ကို ထည့်ထားပေးရပါမယ်

```js
NEXT_PUBLIC_API_BASE_URL= http://localhost:3000/api

GOOGLE_CLIENT_ID = 762948475479-fh816tgk5vjimgkp23qr53ilp1c25np9.apps.googleusercontent.com
GOOGLE_CLIENT_SECRET=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

DATABASE_URL="postgresql://postgres:1234@localhost:5432/mydb4654?schema=public"

NEXT_AUTH_URL=http://localhost:3000
NEXTAUTH_SECRET=jFRxhoOEjWY1m4Qc
```

- ခု request ကို လက်ခံဖို့ ြပင်ဆင်ပြီးပြီမို့ next app မှာ sign in ပြန်လုပ်ကြည့်လိုက်ပါက terminal မှာ user name နဲ့ email ကို log ထုတ်ပေးတာကို ြမင်ရမှာဖြစ်ပါတယ်

![](https://cdn.discordapp.com/attachments/1153197808900395062/1161735046823153684/image.png?ex=65396113&is=6526ec13&hm=ccc8731422ea0ef8ae16d46676b7429134cc6e377baabf0f515d7518e40494cc&)

- ရလာတဲ့ email နဲ့ user name ကို သုံးပြီး user အသစ်တစ်ယောက် ကို data base မှာ create လုပ်ပြီး သိမ်းလိုက်မှာဖြစ်ပါတယ်

##

### 4. Create new user

- user တွေ သိမ်းထားဖို့ database မှာ user table တစ်ခု လုပ်ပေးရမှာဖြစ်ပါတယ်
- prisma မှာ user model တစ်ခု ထပ်ထည့်လိုက်ပြီး migrate လုပ်လိုက်ပါမယ်

![](https://cdn.discordapp.com/attachments/1153197808900395062/1161738994086842378/image.png?ex=653964c0&is=6526efc0&hm=0fdcae3adce3ecc78f09c3d7f743fda6622fec97a51099e97d780fe4dd6fcbff&)

- email field ကို unique အဖြစ် သတ်မှတ်ပေးထားတာမလို့ emial တူတဲ့ user တစ်ယောက်ထပ်ပိုပြီး သိမ်းလို့မရအောင် လုပ်ထားလိုက်တာဖြစ်ပါတယ်
- ဆက်ပြီးတော့ ခုနက request ကို လက်ခံတဲ့ route မှာ log ထုတ်ထားတဲ့ user data ကို သုံးပြီး user တစ်ယောက် create လုပ်လိုက်မှာဖြစ်ပါတယ်

```js
// Next.js API route support: https://nextjs.org/docs/api-routes/introduction
import { prisma } from "@/utils/db";
import type { NextApiRequest, NextApiResponse } from "next";
import { getServerSession } from "next-auth";
import { authOptions } from "../auth/[...nextauth]";

export default async function handler(
  req: NextApiRequest,
  res: NextApiResponse
) {
  const session = await getServerSession(req, res, authOptions);
  if (!session) return res.status(401).send("Unauthorized.");
  const user = session.user;
  const name = user?.name as string;
  const email = user?.email as string;
  console.log("User-data :", name, email);

  // check user exist or not
  const dbUser = await prisma.user.findUnique({ where: { email } });

  //if new user
  if (!dbUser) {
    const newUser = await prisma.user.create({ data: { name, email } });
    return res.status(200).json(newUser);
  }

// existing user
  res.status(200).send(user);
}

```

- အရင်ဆုံး user tabel ထဲက data တွေထဲမှာ ခု ၀င်လာတဲ့ email နဲ့ user ကို ရှာကြည့်လိုက်ပါတယ်
- ရှာမတွေ့ခဲ့ရင်တော့ user အသစ် ဖြစ်တာမလို့ database က user tabel ထဲမှာ user အသစ် တစ်ယောက် create လုပ်ပေးလိုက်ပါတယ်
- ရှာလို့တွေ့ခဲ့ရင်တော့ ရှိပြီးသား user မလို့ create မလုပ်တော့ပဲ response ပဲ လုပ်ပေးလိုက်တာဖြစ်ပါတယ်
- ခုတစ်ခါ next app မှာ ပြန်ပြီး sign in ၀င်ကြည့်ပါက database က user table ထဲမှာ user တစ်ယောက် တိုးလာမှာဖြစ်ပြီး အဲ့ဒီ user ကိုပဲ(email တူ) ပြန်ပြီး sign in လုပ်ကြည့်ပါက user ရှိပြီးသားမလို့ database မှာ user ထပ်မတိုးလာတာကို တွေ့ရမှာဖြစ်ပါတယ်

![](https://cdn.discordapp.com/attachments/1153197808900395062/1161742145724633218/image.png?ex=653967af&is=6526f2af&hm=b09b35a0eb28ef1e67a5cb9777b2645996e89226eb1820490b25543bd89741e6&)
