## MSquare Programming Fullstack Course

### Batch 2

### Episode-_27_ Summary

### 1. Create next app

### 2. Setup up database

### 3. next auth with custom login page

## ဒီနေ့သင်ခန်းစာမှာတော့ သင်တန်းရဲ့ main project ဖြစ်တဲ့ next foodie app ကို create လုပ်သွားမှာဖြစ်ပါတယ်

### setup project

- create next app

```console
$ npx create-next-app
```

- install mui package

```console
$ npm i @mui/icons-material @mui/material @emotion/styled @emotion/react
```

- install redux-toolkit

```console
$ npm i @reduxjs/toolkit react-redux
```

- install prisma

```console
$ npm i prisma
```

- initialize prisma

```console
$ npx prisma init
```

- install next auth

```console
$ npm i next-auth
```

##

### Create database tables

- ပြီးခဲ့တဲ့ သင်ခန်းစာမှာ ဆွေးနွေးခဲ့ကြတဲ့ data model အတိုင်း database မှာ table တွေ တည်ဆောက်ပေးလိုက်ပါမယ်

```js
// prisma/schema.prisma

// This is your Prisma schema file,
// learn more about it in the docs: https://pris.ly/d/prisma-schema

generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}

model Company {
  id             Int            @id @default(autoincrement())
  name           String
  address        String?
  menuCategories MenuCategory[]
  locations      Location[]
  isArchived     Boolean        @default(false)
  createdAt      DateTime       @default(now())
  updatedAt      DateTime       @updatedAt
}

model Location {
  id                           Int                            @id @default(autoincrement())
  name                         String
  address                      String
  company                      Company                        @relation(fields: [companyId], references: [id])
  companyId                    Int
  disabledLocationMenuCategory DisabledLocationMenuCategory[]
  disabledLocationMenu         DisabledLocationMenu[]
  tables                        Table[]
  isArchived                   Boolean                        @default(false)
  createdAt                    DateTime                       @default(now())
  updatedAt                    DateTime                       @updatedAt
}

model MenuCategory {
  id                           Int                            @id @default(autoincrement())
  name                         String
  company                      Company                        @relation(fields: [companyId], references: [id])
  companyId                    Int
  disabledLocationMenuCategory DisabledLocationMenuCategory[]
  menuCategoryMenu             MenuCategoryMenu[]
  isArchived                   Boolean                        @default(false)
  createdAt                    DateTime                       @default(now())
  updatedAt                    DateTime                       @updatedAt
}

model DisabledLocationMenuCategory {
  id             Int          @id @default(autoincrement())
  location       Location     @relation(fields: [locationId], references: [id])
  locationId     Int
  menuCategory   MenuCategory @relation(fields: [menuCategoryId], references: [id])
  menuCategoryId Int
  isArchived     Boolean      @default(false)
  createdAt      DateTime     @default(now())
  updatedAt      DateTime     @updatedAt
}

model Menu {
  id                   Int                    @id @default(autoincrement())
  name                 String
  price                Int                    @default(0)
  description          String?
  assetUrl             String?
  disabledLocationMenu DisabledLocationMenu[]
  menuCategoryMenu     MenuCategoryMenu[]
  menuAddonCategory    MenuAddonCategory[]
  orderlines            Orderline[]
  isArchived           Boolean                @default(false)
  createdAt            DateTime               @default(now())
  updatedAt            DateTime               @updatedAt
}

model DisabledLocationMenu {
  id         Int      @id @default(autoincrement())
  location   Location @relation(fields: [locationId], references: [id])
  locationId Int
  menu       Menu     @relation(fields: [menuId], references: [id])
  menuId     Int
  isArchived Boolean  @default(false)
  createdAt  DateTime @default(now())
  updatedAt  DateTime @updatedAt
}

model MenuCategoryMenu {
  id             Int          @id @default(autoincrement())
  menuCategory   MenuCategory @relation(fields: [menuCategoryId], references: [id])
  menuCategoryId Int
  menu           Menu         @relation(fields: [menuId], references: [id])
  menuId         Int
  isArchived     Boolean      @default(false)
  createdAt      DateTime     @default(now())
  updatedAt      DateTime     @updatedAt
}

model AddonCategory {
  id                Int                 @id @default(autoincrement())
  name              String
  isRequired        Boolean             @default(true)
  menuAddonCategory MenuAddonCategory[]
  addons            Addon[]
  isArchived        Boolean             @default(false)
  createdAt         DateTime            @default(now())
  updatedAt         DateTime            @updatedAt
}

model MenuAddonCategory {
  id              Int           @id @default(autoincrement())
  menu            Menu          @relation(fields: [menuId], references: [id])
  menuId          Int
  addonCategory   AddonCategory @relation(fields: [addonCategoryId], references: [id])
  addonCategoryId Int
  isArchived      Boolean       @default(false)
  createdAt       DateTime      @default(now())
  updatedAt       DateTime      @updatedAt
}

model Addon {
  id              Int           @id @default(autoincrement())
  name            String
  price           Int           @default(0)
  addonCategory   AddonCategory @relation(fields: [addonCategoryId], references: [id])
  addonCategoryId Int
  orderlines       Orderline[]
  isArchived      Boolean       @default(false)
  createdAt       DateTime      @default(now())
  updatedAt       DateTime      @updatedAt
}

model Order {
  id         Int         @id @default(autoincrement())
  status     ORDERSTATUS
  totalPrice Int
  orderlines Orderline[]
  table      Table       @relation(fields: [tableId], references: [id])
  tableId    Int
  isArchived Boolean     @default(false)
  createdAt  DateTime    @default(now())
  updatedAt  DateTime    @updatedAt
}

model Orderline {
  id         Int      @id @default(autoincrement())
  order      Order    @relation(fields: [orderId], references: [id])
  orderId    Int
  menu       Menu     @relation(fields: [menuId], references: [id])
  menuId     Int
  addon      Addon    @relation(fields: [addonId], references: [id])
  addonId    Int
  quantity   Int
  orderSeq   String
  isArchived Boolean  @default(false)
  createdAt  DateTime @default(now())
  updatedAt  DateTime @updatedAt
}

model Table {
  id         Int      @id @default(autoincrement())
  name       String
  location   Location @relation(fields: [locationId], references: [id])
  locationId Int
  isArchived Boolean  @default(false)
  createdAt  DateTime @default(now())
  updatedAt  DateTime @updatedAt
  orders      Order[]
}

enum ORDERSTATUS {
  ORDERED
  OUTFORDELIVERY
  DELIVERED
  CANCELLED
}
```

- migrate db

```console
$ npx prisma migrate dev --name init
```

- create config

```js
// src/utils/config/index.ts
interface Config {
  apiBaseUrl: string;
}

export const config: Config = {
  apiBaseUrl: process.env.NEXT_PUBLIC_API_BASE_URL || "",
};
```

- create db client

```js
// src/utils/db/index.ts

import { PrismaClient } from "@prisma/client";

export const prisma = new PrismaClient();
```

##

### Setup next auth with custom signin/out page

- setup `[...nextauth].ts`

```js
import { config } from "@/utils/config";
import NextAuth from "next-auth";
import GoogleProvider from "next-auth/providers/google";

export const authOptions = {
  providers: [
    GoogleProvider({
      clientId: config.googleClientId,
      clientSecret: config.googleClientSecret,
    }),
  ],
  pages: {
    signIn: "/auth/signin",
    signOut: "/auth/signout",
  },
};

export default NextAuth(authOptions);
```

- authOptions object မှာ provider အနေနဲ့ GoogleProvider , pages အနေနဲ့ custom signin/out page နှစ်ခုကို route လုပ်ပြီး သတ်မှတ်ထားလိုက်ပါတယ်
- အဲ့ဒီpage နှစ်ခုကို route လုပ်ထားတဲ့အတိုင်း folder လုပ်ပြီး သတ်မှတ်လိုက်ပါမယ်

```js
// src/pages/auth/signin/index.tsx

import { Box, Button } from "@mui/material";
import { signIn } from "next-auth/react";

const SignIn = () => {
  return (
    <Box>
      <Button
        variant="contained"
        onClick={() => signIn("google", { callbackUrl: "/" })}
      >
        Sign in
      </Button>
      <h1>Custom sign in page</h1>
    </Box>
  );
};

export default SignIn;
```

- signin page မှာတော့ button တစ်ခု ထည့်ပေးထားပြီး click လုပ်လိုက်ရင် next-auth က signin function ကို ခေါ်လိုက်ပါတယ်
- signin function မှာ ပထမ parameter အဖြစ် "google"ကို ထည့်ပေးရမှာဖြစ်ပြီး
- ဒုတိယparameter အဖြစ် Sign in ၀င်လို့ပြီးရင် ပို့ပေးလိုက်မယ့် callback url ကို ထည့်ပေးရမှာဖြစ်ပါတယ်

```js
// src/pages/auth/signout/index.tsx

import { Box } from "@mui/material";

const SignOut = () => {
  return (
    <Box>
      <h1>Custom sign out page</h1>
    </Box>
  );
};

export default SignOut;
```

- signout page မှာတော့ Custom sign out page ဆိုတဲ့ h1 tag တစ်ခုပဲ render လုပ်ပေးထားတာပဲဖြစ်ပါတယ်
- home page မှာ next auth ရဲ့ session ရှိမရှိစစ်ပြီး session မရှိရင် sign in page ကို auto ရောက်ပြီး login ၀င်ခိုင်းလိုက်မှာဖြစ်ပါတယ်

```js
// src/pages.index.tsx

import { Box, Button, Typography } from "@mui/material";
import { signIn, signOut, useSession } from "next-auth/react";

export default function Home() {
  const { data: session } = useSession();
  if (!session) {
    return (
      <Box>
        <Typography>Not signed in</Typography>
        <Button variant="contained" onClick={() => signIn()}>
          Sign in
        </Button>
      </Box>
    );
  }
  return (
    <Box>
      <h1>Signed in with: {session.user?.email}</h1>
      <Button variant="contained" onClick={() => signOut()}>
        Sign out
      </Button>
    </Box>
  );
}
```

- session ရှိနေရင်တော့ sign out လုပ်လို့ရမယ့် button တစ်ခု ကို ပြပေးထားမှာဖြစ်ပါတယ်
- next authကို သုံးနိုင်ဖို့ \_app.tsx မှာ session provider နဲ့ wrap လုပ်ပေးရမှာဖြစ်ပါတယ်

```js
// src/pages/_app.tsx

import Layout from "@/components/layout/Layout";
import { SessionProvider } from "next-auth/react";
import type { AppProps } from "next/app";
import { Provider } from "react-redux";

export default function App({ Component, pageProps }: AppProps) {
  return (
    <SessionProvider>
      <Component {...pageProps} />
    </SessionProvider>
  );
}
```

- ခု next app ကို run ပြီး localhost:3000 ကို သွားကြည့်ပါက
- login မ၀င်ရသေးရင် sign in page ကို routing လုပ်သွားမှာဖြစ်ပြီး
- sign in လုပ်လိုက်ပါက homne page ကို ပြန်ရောက်လာတာကို ြမင်ရမှာဖြစ်ပါတယ်
