## MSquare Programming Fullstack Course

### Batch 2

### Episode-_22_ Summary

## Mini e-commerce project (`part-1`)

1. Layout + Data modelling
2. Project nextjs init
3. Install packages
4. Redux toolkit setup, nextjs api
5. Display product

##

### Layout and Data modelling

- project တစ်ခု မစခင်မှာ ပြသမယ့် layout နဲ့ data တွေ စီမံခန့်ခွဲရမယ့် အပိုင်းကို အရင် စဥ်းစားပြီး plan ချရမှာဖြစ်ပါတယ်
- ခု သင်ခန်းစာမှာ layout အနေနဲ့

  - product အားလုံးပြမယ့် page (Home page)
  - product တစ်ခုခုကို နှိပ်လိုက်ရင် info တွေပြပြီး add to cart လုပ်လို့ရမယ့်page(product page)
  - order လုပ်ထားတဲ့ product တွေကို ပြန်ကြည့်လို့ရပြီး order comfirm လုပ်လို့ရမယ့် page (cart page)
  - comfirm ဖြစ်ပြီး order ထဲမှာ ပါတဲ့ items တွေ နဲ့ total price တွေ status တွေ ပြန်ပြပေးတဲ့ page (order review page)

- စတာတွေ လိုအပ်ပါမယ်

![](https://cdn.discordapp.com/attachments/1153197808900395062/1156653804603850913/image.png?ex=6515c14c&is=65146fcc&hm=3807907990255ac09b6963a2a99e55341eacfc4cc0065008511a7a269c2d3a2f&)

##

### data modelling အနေနဲ့ database မှာ table တွေ create လုပ်တာနဲ့ table တွေ ချိတ်ဆက်တဲ့အပိုင်းကို Model လုပ်ရမှာဖြစ်ပါတယ်

- အခု လုပ်မယ့် project မှာ
  - product တွေ သိမ်းထားမယ့် table
  - order line တွေ သိမ်းထားမယ့် table
  - order တွေ သိမ်းထားမယ့် table
  - order status
- စတာတွေ လိုအပ်မှာဖြစ်ပါတယ်

```js
// This is your Prisma schema file,
// learn more about it in the docs: https://pris.ly/d/prisma-schema

generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}

model Product {
  id          Int         @id @default(autoincrement())
  title       String
  description String
  price       Int
  imageUrl    String?
  Orderline   Orderline[]
}

model Order {
  id         Int         @id @default(autoincrement())
  status     OrderStatus
  totalPrice Int
  Orderline  Orderline[]
}

model Orderline {
  id        Int     @id @default(autoincrement())
  order     Order   @relation(fields: [orderId], references: [id])
  orderId   Int
  product   Product @relation(fields: [productId], references: [id])
  productId Int
  quantity Int
  isArchived Boolean @default(false)
}

enum OrderStatus {
  ORDERED
  OUTFORDELIVERY
  DELIVERED
  CANCELLED
}

```

- ထူးခြားချက် အနေနဲ့ order status ကို enum နဲ့ လုပ်ထားတာဖြစ်ပါတယ်
- enum နဲ့ Order status တွေကို ပုံသေသတ်မှတ်ထားလိုက်တာဖြစ်တယ်
- order table ထဲမှာ status အနေနဲ့ OrderStatus ကို သုံးထားပါတယ်
- `OrderStatus` ထဲမှာ ထည့်ထားတဲ့ data type တွေထဲက
  - ORDERED
  - OUTFORDELIVERY
  - DELIVERED
  - CANCELLED
- တစ်ခုခုကိုပဲ သတ်မှတ်ပေးရမယ်လို့ ဆိုလိုတာဖြစ်ပါတယ်

### [More about enum data type](https://www.w3schools.com/typescript/typescript_enums.php)

##

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

```js
// .env

DATABASE_URL="postgresql://postgres:1234@localhost:5432/mini-commerce?schema=public"
NEXT_PUBLIC_API_BASE_URL=http://localhost:3000/api

```

> Note. replace with your db password at `1234`

```js
// prisma/schema.prisma


generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}

model Product {
  id          Int         @id @default(autoincrement())
  title       String
  description String
  price       Int
  imageUrl    String?
  Orderline   Orderline[]
}

model Order {
  id         Int         @id @default(autoincrement())
  status     OrderStatus
  totalPrice Int
  Orderline  Orderline[]
}

model Orderline {
  id        Int     @id @default(autoincrement())
  order     Order   @relation(fields: [orderId], references: [id])
  orderId   Int
  product   Product @relation(fields: [productId], references: [id])
  productId Int
  quantity Int
  isArchived Boolean @default(false)
}

enum OrderStatus {
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

### Redux store setup

![](https://cdn.discordapp.com/attachments/1153197808900395062/1156668033331372054/image.png?ex=6515ce8c&is=65147d0c&hm=dafb224d6217caf156bc11d8d32889c3531534deeb07f6fa62ab282b569b8072&)

```js
// src/store/slices/productSlice.ts

import { ProductSlice } from "@/types/product";
import { config } from "@/utils/config";
import { createAsyncThunk, createSlice } from "@reduxjs/toolkit";

const initialState: ProductSlice = {
  items: [],
  isLoading: false,
  error: null,
};

export const fetchProducts = createAsyncThunk(
  "product/fetchProducts",
  async (_, thunkApi) => {
    const response = await fetch(`${config.apiBaseUrl}/products`);
    const products = await response.json();
    thunkApi.dispatch(setProducts(products));
  }
);

const productSlice = createSlice({
  name: "product",
  initialState,
  reducers: {
    setProducts: (state, action) => {
      state.items = action.payload;
    },
  },
});

export const { setProducts } = productSlice.actions;
export default productSlice.reducer;
```

```js
// src/store/index.ts

import { configureStore } from "@reduxjs/toolkit";
import productReducer from "./slices/productSlice";

export const store = configureStore({
  reducer: {
    products: productReducer,
  },
});

// Infer the `RootState` and `AppDispatch` types from the store itself
export type RootState = ReturnType<typeof store.getState>;
// Inferred type: {posts: PostsState, comments: CommentsState, users: UsersState}
export type AppDispatch = typeof store.dispatch;
```

```js
// src/store/hook.ts

import type { TypedUseSelectorHook } from "react-redux";
import { useDispatch, useSelector } from "react-redux";
import type { AppDispatch, RootState } from "./";

// Use throughout your app instead of plain `useDispatch` and `useSelector`
export const useAppDispatch: () => AppDispatch = useDispatch;
export const useAppSelector: TypedUseSelectorHook<RootState> = useSelector;
```
