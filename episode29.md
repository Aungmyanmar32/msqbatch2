## MSquare Programming Fullstack Course

### Batch 2

### Episode-_29_ Summary

### 1. Create default data( if new user)

### 2. Create store(slice) for data

### 3. Get all data(if existing user)

### 4. Show all data from store

### 5. set current location and Create New Menu Category

##

### 1. Create default data( if new user)

- backoffice app ထဲကို user တစ်ယောက် Login ၀င်လာပြီးဆိုရင် ရှိပြီးသား user လား user အသစ်လား အရင် စစ်ခဲ့ပါတယ်။
- user အသစ်ဆိုရင်တော့ user က app ကို အဆင်ပြေပြေ သုံးလို့ရအောင် default data တွေကို create လုပ်ပေးလိုက်မှာဖြစ်ပါတယ်

- အရင်ဆုံး layout component မှာ fetch လုပ်ခဲ့တာကို appSlice မှာ thunk action တစ်ခုအဖြစ် သတ်မှတ်ပြီး dispatch လုပ် လို့ရအောင် ပြန်ရေးလိုက်ပါမယ်

```js
// src/store/slices/appSlice.ts

import { AppSlice, GetAppDataOptions } from "@/types/app";
import { config } from "@/utils/config";
import { createAsyncThunk, createSlice } from "@reduxjs/toolkit";

const initialState: AppSlice = {
  init: false,
  isLoading: false,
  error: null,
};

export const fetchAppData = createAsyncThunk(
  "app/fetchAppData",
  async (options: GetAppDataOptions, thunkApi) => {
    const { onSuccess, onError } = options;
    try {
      const response = await fetch(`${config.apiBaseUrl}/app`);
      const appData = await response.json();
      console.log(appData);

      onSuccess && onSuccess();

      thunkApi.dispatch(setInit(true));
    } catch (err) {
      onError && onError();
    }
  }
);

const appSlice = createSlice({
  name: "app",
  initialState,
  reducers: {
    setInit: (state, action) => {
      state.init = action.payload;
    },
  },
});

export const { setInit } = appSlice.actions;
export default appSlice.reducer;
```

```js
// src/types/app.ts

export interface AppSlice {
  init: boolean;
  isLoading: boolean;
  error: Error | null;
}

export interface BaseOptions {
  onSuccess?: (data?: any) => void;
  onError?: (data?: any) => void;
}

export interface GetAppDataOptions extends BaseOptions {}
```

- app slice ရဲ့ item မှာ initail state အဖြစ် init ဆိုတဲ့ boolean တစ်ခုပဲ ထည့်ပေးထားပြီး action အဖြစ် init ရဲ့ state ကို ကိုင်တွယ် မယ့် setInit တစ်ခုပဲ လုပ်ထားလိုက်ပါတယ်
- fetchData thunk action ကို dispatch လုပ်တဲ့အခါ server ဆီ request လုပ်လိုက်ပြီး ပြန်လာတဲ့ response ကို log ထုတ်ထားပါတယ်
- ပြီးတော့ setInit action ကို dispatch လုပ်လိုက်ပြီး init state ကို true လုပ်လိုက်ပါတယ်
- app slice ကို store ထဲမှာ ထည့်ပေးလိုက်ပါမယ်

```js
// src/store/index.ts

import { configureStore } from "@reduxjs/toolkit";
import menuReducer from "./slices/menuSlice";
import appReducer from "./slices/appSlice";

export const store = configureStore({
  reducer: {
    menu: menuReducer,
    app: appReducer,
  },
});

// Infer the `RootState` and `AppDispatch` types from the store itself
export type RootState = ReturnType<typeof store.getState>;
// Inferred type: {posts: PostsState, comments: CommentsState, users: UsersState}
export type AppDispatch = typeof store.dispatch;
```

- fetchData thunk action ကို layout component ကနေ dispatch လုပ်ပြီး data တွေ လှမ်းယူမှာဖြစ်ပါတယ်

```js
import { useAppDispatch, useAppSelector } from "@/store/hooks";
import { fetchAppData } from "@/store/slices/appSlice";
import { Box } from "@mui/material";
import { useSession } from "next-auth/react";
import { ReactNode, useEffect } from "react";
import SideBar from "./Sidebar";
import Topbar from "./Topbar";

interface Props {
  children: ReactNode;
}

const Layout = ({ children }: Props) => {
  const { data: session } = useSession();
  const dispatch = useAppDispatch();
  const { init } = useAppSelector((state) => state.app);

  useEffect(() => {
    if (session && !init) {
      dispatch(fetchAppData({}));
    }
  }, [session]);

  return (
    <Box>
      <Topbar />
      <Box sx={{ display: "flex", position: "relative", zIndex: 5, flex: 1 }}>
        {session && <SideBar />}
        <Box sx={{ p: 3, width: "100%", height: "100%" }}>{children}</Box>
      </Box>
    </Box>
  );
};

export default Layout;
```

##

- user table မှာ company id ကို FK အနေနဲ့ သုံးပြီး relation လုပ်လိုက်ပါမယ်

```js
model User {
  id         Int      @id @default(autoincrement())
  email      String   @unique
  name       String?
  companyId  Int
  company    Company  @relation(fields: [companyId], references: [id])
  isArchived Boolean  @default(false)
  createdAt  DateTime @default(now())
  updatedAt  DateTime @updatedAt
}
```

- app slice ထဲကနေ ပို့ထားတဲ့ request လက်ခံတဲ့ api function မှာ user ကို new user ဟုတ်မဟုတ် စစ်လိုက်ပြီး New user ဆိုရင် defualt data တွေ ပါ တစ်ခါတည်း လုပ်ပေးလိုက်မှာဖြစ်ပါတယ်

```js
// src/pages/api/app/index.ts

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
  const dbUser = await prisma.user.findUnique({ where: { email } });
  if (!dbUser) {
    // 1. create new company for user and assign user to it.
    const newCompanyName = "Ah Wa Sarr";
    const newCompanyAddress = "Hintada Street 21, Sanchaung, Yangon";
    const company = await prisma.company.create({
      data: { name: newCompanyName, address: newCompanyAddress },
    });

    // 2. create new user
    await prisma.user.create({
      data: { name, email, companyId: company.id },
    });

    // 3. create new menu category
    const newMenuCategoryName = "Default menu category";
    const menuCategory = await prisma.menuCategory.create({
      data: { name: newMenuCategoryName, companyId: company.id },
    });

    // 4. create new menu
    const newMenuName = "Default menu";
    const menu = await prisma.menu.create({
      data: { name: newMenuName, price: 1000 },
    });

    // 5. create new row in MenuCategoryMenu
    const menuCategoryMenu = await prisma.menuCategoryMenu.create({
      data: { menuCategoryId: menuCategory.id, menuId: menu.id },
    });

    // 6. create new addon category
    const newAddonCategoryName = "Default addon category";
    const addonCategory = await prisma.addonCategory.create({
      data: { name: newAddonCategoryName },
    });

    // 7. create new row in MenuAddonCategory
    const menuAddonCategory = await prisma.menuAddonCategory.create({
      data: { menuId: menu.id, addonCategoryId: addonCategory.id },
    });

    // 8. create new addon
    const newAddonNameOne = "Default addon 1";
    const newAddonNameTwo = "Default addon 2";
    const newAddonNameThree = "Default addon 3";
    const newAddonsData = [
      { name: newAddonNameOne, addonCategoryId: addonCategory.id },
      { name: newAddonNameTwo, addonCategoryId: addonCategory.id },
      { name: newAddonNameThree, addonCategoryId: addonCategory.id },
    ];
    const addons = await prisma.$transaction(
      newAddonsData.map((addon) => prisma.addon.create({ data: addon }))
    );

    // 9. create new location
    const newLocationName = "Sanchaung";
    const location = await prisma.location.create({
      data: {
        name: newLocationName,
        address: newCompanyAddress,
        companyId: company.id,
      },
    });

    // 10. create new table
    const newTableName = "Default table";
    const table = await prisma.table.create({
      data: { name: newTableName, locationId: location.id },
    });

    return res.status(200).json({
      menuCategory,
      menu,
      menuCategoryMenu,
      addonCategory,
      menuAddonCategory,
      addons,
      location,
      table,
    });
  } else {
    res.send(dbUser);
  }
}


```

> ရှင်းလင်းချက်

```js
  if (!dbUser) {
    // 1. create new company for user and assign user to it.
    const newCompanyName = "Ah Wa Sarr";
    const newCompanyAddress = "Hintada Street 21, Sanchaung, Yangon";
    const company = await prisma.company.create({
      data: { name: newCompanyName, address: newCompanyAddress },
    });
```

- user အသစ်နဲ့login ၀င်လာပြီးဆိုတာနဲ့ company တစ်ခု create လုပ်ပေးလိုက်ပြီး company ဆိုပြီး သိမ်းထားလိုက်ပါတယ်

```js
// 2. create new user
await prisma.user.create({
  data: { name, email, companyId: company.id },
});
```

- သိမ်းထားတဲ့ company id ကို သုံးပြီး login ၀င်လာတဲ့ user ကို database မှာ create လုပ်လိုက်ပါတယ်

```js
// 3. create new menu category
const newMenuCategoryName = "Default menu category";
const menuCategory = await prisma.menuCategory.create({
  data: { name: newMenuCategoryName, companyId: company.id },
});
```

- Default menu category တစ်ခု လုပ်ကာ သိမ်းထားတဲ့ company id နဲ့ ချိတ်ပေးလိုက်ပါတယ်

```js
// 4. create new menu
const newMenuName = "Default menu";
const menu = await prisma.menu.create({
  data: { name: newMenuName, price: 1000 },
});
```

- Default menu တစ်ခု လုပ်လိုက်ပါတယ်

```js
// 5. create new row in MenuCategoryMenu
const menuCategoryMenu = await prisma.menuCategoryMenu.create({
  data: { menuCategoryId: menuCategory.id, menuId: menu.id },
});
```

- Default menu ကို Default menu category နဲ့ join table မှာ ချိတ်ပေးလိုက်ပါတယ်

```js
// 6. create new addon category
const newAddonCategoryName = "Default addon category";
const addonCategory = await prisma.addonCategory.create({
  data: { name: newAddonCategoryName },
});
```

- addon category တစ်ခု create လုပ်လိုက်ပါတယ်

```js
// 7. create new row in MenuAddonCategory
const menuAddonCategory = await prisma.menuAddonCategory.create({
  data: { menuId: menu.id, addonCategoryId: addonCategory.id },
});
```

- Default menu ကို Default addon category နဲ့ join table မှာ ချိတ်ပေးလိုက်ပါတယ်

```js
// 8. create new addon
const newAddonNameOne = "Default addon 1";
const newAddonNameTwo = "Default addon 2";
const newAddonNameThree = "Default addon 3";
const newAddonsData = [
  { name: newAddonNameOne, addonCategoryId: addonCategory.id },
  { name: newAddonNameTwo, addonCategoryId: addonCategory.id },
  { name: newAddonNameThree, addonCategoryId: addonCategory.id },
];
const addons = await prisma.$transaction(
  newAddonsData.map((addon) => prisma.addon.create({ data: addon }))
);
```

- Default addon သုံးခု လုပ်လိုက်ပြီး ခနက addon category နဲ့ ချိတ်ပေးထားလိုက်ပါတယ်

```js
// 9. create new location
const newLocationName = "Sanchaung";
const location = await prisma.location.create({
  data: {
    name: newLocationName,
    address: newCompanyAddress,
    companyId: company.id,
  },
});
```

- location တစ်ခု create လုပ်လိုက်ပြီး အထက်မှာ သိမ်းထားတဲ့ company id နဲ့ ချိတ်ပေးလိုက်ပါတယ်

```js
// 10. create new table
const newTableName = "Default table";
const table = await prisma.table.create({
  data: { name: newTableName, locationId: location.id },
});
```

- table တစ်ခုပါ create လုပ်ပေးလိုက်ပြီး အထက်ပါ location နဲ့ ချိတ်ပေးထားလိုက်ပါတယ်

- အပေါ်ကရေးထားတဲ့ code တွေကို ခြုံပြီးရှင်းပြရရင်တော့ user အသစ် တစ်ယောက် signin ၀င်လာရင် အဲ့ဒီuser အတွက်
  - company အသစ်
  - menu categoryအသစ်
  - locationအသစ်
  - menuအသစ်
  - addon categoryအသစ်
  - addonsအသစ်
  - table အသစ်
- တွေ အားလုံးကို default အနေနဲ့ လုပ်ပေးလိုက်တာပဲဖြစ်ပါတယ်

```js
return res.status(200).json({
  menuCategory,
  menu,
  menuCategoryMenu,
  addonCategory,
  menuAddonCategory,
  addons,
  location,
  table,
});
```

- နောက်ဆုံးမှာတော့ လုပ်ပေးထားတဲ့ data တွေကို response ပြန်ပေးလိုက်တာပဲဖြစ်ပါတယ်
- ခု user အသစ်တစ်ယောက် signin ၀င်စမ်းကြည့်ပါက default data တွေကို ပြန်ပို့ပေးလာတာကို ြမင်ရမှာဖြစ်ပါတယ်

![](https://cdn.discordapp.com/attachments/1153197808900395062/1163528021282660352/image.png?ex=653fe6e9&is=652d71e9&hm=05e1647dfaa51757c7675a7907998e69edfa3aa415e217076186c3fd836bce8f&)

##

### 2. Create store(slice) for data

- response ပြန်လာတဲ့ dataတွေကို သက်ဆိုင်ရာ slice တစ်ခုစီ လုပ်လိုက်ပြီး storeမှာ သိမ်းလိုက်မှာဖြစ်ပါတယ်

![](https://cdn.discordapp.com/attachments/1153197808900395062/1163529713076158505/image.png?ex=653fe87c&is=652d737c&hm=5126be8210cd9679ac577e0cb8dd4720016e9fcf284f791234f801a4d6759458&)

- slice တွေ ထဲမှာတော့ အောက်ပါအတိုင်း action တစ်ခုပါတဲ့ slice ပဲ လုပ်လိုက်မှာဖြစ်ပါတယ်

```js
import { *slice-type* } from "@/types/location";
import { createSlice } from "@reduxjs/toolkit";

const initialState: *slice-type* = {
  items: [],
  isLoading: false,
  error: null,
};

const *slice-name* = createSlice({
  name: "*slice-name*",
  initialState,
  reducers: {
    set*slice-name*: (state, action) => {
      state.items = action.payload;
    },
  },
});

export const { set*slice-name* } = *slice-name*.actions;
export default *slice-name*.reducer;
```

exmaple

```js
// locationSlice.ts

import { LocationSlice } from "@/types/location";
import { createSlice } from "@reduxjs/toolkit";

const initialState: LocationSlice = {
  items: [],
  isLoading: false,
  error: null,
};

const locationSlice = createSlice({
  name: "location",
  initialState,
  reducers: {
    setLocations: (state, action) => {
      state.items = action.payload;
    },
  },
});

export const { setLocations } = locationSlice.actions;
export default locationSlice.reducer;
```

- သက်ဆိုင်ရာslice တွေ အတွက် type တွေကိုလည်း types folder ထဲမှာ သတ်မှတ်ပေးရပါမယ်

![](https://cdn.discordapp.com/attachments/1153197808900395062/1163531566341951609/image.png?ex=653fea36&is=652d7536&hm=63fb75d4c7c219251d3e168357c39ffc53196f77eadf63c7ac0b3d4daec539a1&)

- slice တွေ types တွေ လုပ်ပြီးရင် store ကို update လုပ်ပေးလိုက်ပါမယ်

```js
// src/store/index.ts

import { configureStore } from "@reduxjs/toolkit";
import addonCategoryReducer from "./slices/addonCategorySlice";
import addonReducer from "./slices/addonSlice";
import appReducer from "./slices/appSlice";
import locationReducer from "./slices/locationSlice";
import menuCategoryReducer from "./slices/menuCategorySlice";
import menuReducer from "./slices/menuSlice";
import tableReducer from "./slices/tableSlice";

export const store = configureStore({
  reducer: {
    app: appReducer,
    location: locationReducer,
    menuCategory: menuCategoryReducer,
    menu: menuReducer,
    addonCategory: addonCategoryReducer,
    addon: addonReducer,
    table: tableReducer,
  },
});

// Infer the `RootState` and `AppDispatch` types from the store itself
export type RootState = ReturnType<typeof store.getState>;
// Inferred type: {posts: PostsState, comments: CommentsState, users: UsersState}
export type AppDispatch = typeof store.dispatch;
```

- ပြီးရင်တော့ appSlice ထဲမှာ response ပြန်လာတဲ့data တွေကို သက်ဆိုင်ရာ slice တွေက set action ကို dispatch လုပ်ပြီး store ထဲမှာ သိမ်းလိုက်မှာဖြစ်ပါတယ်

```js
// src/store/slices/appSlice.ts

import { AppSlice, GetAppDataOptions } from "@/types/app";
import { config } from "@/utils/config";
import { createAsyncThunk, createSlice } from "@reduxjs/toolkit";
import { setAddonCategories } from "./addonCategorySlice";
import { setAddons } from "./addonSlice";
import { setLocations } from "./locationSlice";
import { setMenuCategories } from "./menuCategorySlice";
import { setMenus } from "./menuSlice";
import { setTables } from "./tableSlice";

const initialState: AppSlice = {
  init: false,
  isLoading: false,
  error: null,
};

export const fetchAppData = createAsyncThunk(
  "app/fetchAppData",
  async (options: GetAppDataOptions, thunkApi) => {
    const { onSuccess, onError } = options;
    try {
      const response = await fetch(`${config.apiBaseUrl}/app`);
      const appData = await response.json();
      const {
        locations,
        menuCategories,
        menus,
        addonCategories,
        addons,
        tables,
      } = appData;
      thunkApi.dispatch(setInit(true));
      thunkApi.dispatch(setLocations(locations));
      thunkApi.dispatch(setMenuCategories(menuCategories));
      thunkApi.dispatch(setMenus(menus));
      thunkApi.dispatch(setAddonCategories(addonCategories));
      thunkApi.dispatch(setAddons(addons));
      thunkApi.dispatch(setTables(tables));
      onSuccess && onSuccess();
    } catch (err) {
      onError && onError();
    }
  }
);

const appSlice = createSlice({
  name: "app",
  initialState,
  reducers: {
    setInit: (state, action) => {
      state.init = action.payload;
    },
  },
});

export const { setInit } = appSlice.actions;
export default appSlice.reducer;
```

- user အသစ်နဲ့ signin ၀င်ကြည့်ပါက store ထဲမှာ default data တွေရောက်လာတာကို မြင်ရမှာဖြစ်ပါတယ်

![](https://cdn.discordapp.com/attachments/1153197808900395062/1163544695658516581/image.png?ex=653ff671&is=652d8171&hm=ffbc9713565c4df565e904e8f28e0660562a8b0b8071855a5b90f4e4b7a543b7&)

##

### 3. Get all data(if existing user)

- တကယ်လို့ user က dabase ထဲမှာ ရှိပြီးသားuser ဆိုရင်တော့ အဲ့ဒီuser နဲ့ ချိတ်ထားတဲ့ company id ကို သုံးပြီး သက်ဆိုင်ရာdataတွေကို databse ကနေလှမ်းယူလိုက်မှာဖြစ်ပါတယ်

```js
// src/pages/api/app/index.ts --> else {....}

if(!dbUser){
    ......
    ......
}else {
    // 1. get company id from current user
    const companyId = dbUser.companyId;

    // 3.find locations
    const locations = await prisma.location.findMany({ where: { companyId } });
    const locationIds = locations.map((item) => item.id);

    // 3. find menucategories
    const menuCategories = await prisma.menuCategory.findMany({
      where: { companyId },
    });
    const menuCategoryIds = menuCategories.map((item) => item.id);

    // 4. find menu
    const menuCategoryMenu = await prisma.menuCategoryMenu.findMany({
      where: { menuCategoryId: { in: menuCategoryIds } },
    });
    const menuIds = menuCategoryMenu.map((item) => item.menuId);
    const menus = await prisma.menu.findMany({
      where: { id: { in: menuIds } },
    });

    // 5. find addon category
    const menuAddonCategory = await prisma.menuAddonCategory.findMany({
      where: { menuId: { in: menuIds } },
    });
    const addonCategoryIds = menuAddonCategory.map(
      (item) => item.addonCategoryId
    );
    const addonCategories = await prisma.addonCategory.findMany({
      where: { id: { in: addonCategoryIds } },
    });

    // 6. find addons
    const addons = await prisma.addon.findMany({
      where: { addonCategoryId: { in: addonCategoryIds } },
    });

    // 7. find table
    const tables = await prisma.table.findMany({
      where: { locationId: { in: locationIds } },
    });

    // 8. response all founded data
    return res.status(200).json({
      locations,
      menuCategories,
      menus,
      addonCategories,
      addons,
      tables,
    });
  }
```

1. လက်ရှိuser ထဲက company id ကို ယူလိုက်ပါတယ်
2. ရလာတဲ့ comapny id ကို သုံးပြီး company အောက်မှာရှိတဲ့ location တွေ ရှာလိုက်ပါတယ်
3. ရလာတဲ့ comapny id ကို သုံးပြီး company အောက်မှာရှိတဲ့ menu category တွေ ရှာလိုက်ပါတယ်
4. ရလာတဲ့ menu category id တွေ ကို သုံးပြီး menuCategoryMenu join table ထဲမှာရှိတဲ့ menu id တွေကို အရင်ယူလိုက်ပြီ menu တွေကို menu table မှာ ပြန်ရှာလိုက်ပါတယ်
5. ရလာတဲ့ menu id တွေ ကို သုံးပြီး menuAddonCategory join table ထဲမှာရှိတဲ့ AddonCategory id တွေကို အရင်ယူလိုက်ပြီ AddonCategory တွေကို AddonCategory table မှာ ပြန်ရှာလိုက်ပါတယ်
6. addon table မှာ ရလာတဲ့ addon category id တွေနဲ့ ချိတ်ထားတဲ့ addon တွေကို ထပ်ရှာလိုက်ပါတယ်
7. location id တွေ ပေါ်မူတည်ပြီး table တွေကို ထပ်ရှာပါတယ်
8. နောက်ဆုံးမှာတော့ ရှာတွေ့ထားတဲ့ data အားလုံးကို response ပြန်ပေးလိုက်တာပဲဖြစ်ပါတယ်

- ခုဆိုရင် user အသစ်ပဲဖြစ်ဖြစ် အဟောင်းပဲဖြစ်ဖြစ် signin ၀င်လာတာနဲ့ သက်ဆိုင်ရာdataတွေကို store မှာ အဆင်သင့် သိမ်းထားလို့ရပြီး ဖြစ်ပါတယ်

##

### 4. Show all data from store

- store ထဲမှာသိမ်းထားတဲ data တွေကို သက်ဆိုင်ရာ page တွေမှာ ပြနိုင်အောင် ဆက်လုပ်လိုက်ပါမယ်

```js
// src/pages/backoffice/addon-categories/index.tsx
import NewAddonCategory from "@/components/NewAddonCategory";
import { useAppSelector } from "@/store/hooks";
import { Box, Button, Typography } from "@mui/material";
import { useState } from "react";

const AddonCategoriesPage = () => {
  const [open, setOpen] = useState(false);
  const addonCategories = useAppSelector((state) => state.addonCategory.items);
  return (
    <Box>
      <Box sx={{ display: "flex", justifyContent: "flex-end" }}>
        <Button
          variant="contained"
          onClick={() => setOpen(true)}
          sx={{ width: 400 }}
        >
          New addon category
        </Button>
      </Box>
      {addonCategories.map((item) => (
        <Typography key={item.id}>{item.name}</Typography>
      ))}
      <NewAddonCategory open={open} setOpen={setOpen} />
    </Box>
  );
};

export default AddonCategoriesPage;
```

```js
// src/pages/backoffice/addons/index.tsx

import NewAddon from "@/components/NewAddon";
import { useAppSelector } from "@/store/hooks";
import { Box, Button, Typography } from "@mui/material";
import { useState } from "react";

const AddonsPage = () => {
  const [open, setOpen] = useState(false);
  const addons = useAppSelector((state) => state.addon.items);
  return (
    <Box>
      <Box sx={{ display: "flex", justifyContent: "flex-end" }}>
        <Button variant="contained" onClick={() => setOpen(true)}>
          New addon
        </Button>
      </Box>
      {addons.map((item) => (
        <Typography key={item.id}>{item.name}</Typography>
      ))}
      <NewAddon open={open} setOpen={setOpen} />
    </Box>
  );
};

export default AddonsPage;
```

```js
// src/pages/backoffice/menu-categories/index.tsx

import NewMenuCategory from "@/components/NewMenuCategory";
import { useAppSelector } from "@/store/hooks";
import { Box, Button, Typography } from "@mui/material";
import { useState } from "react";

const MenuCategoriesPage = () => {
  const [open, setOpen] = useState(false);
  const menuCategories = useAppSelector((state) => state.menuCategory.items);
  return (
    <Box>
      <Box sx={{ display: "flex", justifyContent: "flex-end" }}>
        <Button variant="contained" onClick={() => setOpen(true)}>
          New menu category
        </Button>
      </Box>
      <Box>
        {menuCategories.map((item) => (
          <Typography key={item.id}>{item.name}</Typography>
        ))}
      </Box>
      <NewMenuCategory open={open} setOpen={setOpen} />
    </Box>
  );
};

export default MenuCategoriesPage;
```

```js
// src/pages/backoffice/menus/index.tsx

import NewMenu from "@/components/NewMenu";
import { useAppSelector } from "@/store/hooks";
import { Box, Button, Typography } from "@mui/material";
import { useState } from "react";

const MenusPage = () => {
  const [open, setOpen] = useState(false);
  const menus = useAppSelector((state) => state.menu.items);
  return (
    <Box>
      <Box sx={{ display: "flex", justifyContent: "flex-end" }}>
        <Button variant="contained" onClick={() => setOpen(true)}>
          New menu
        </Button>
      </Box>
      {menus.map((item) => (
        <Typography key={item.id}>{item.name}</Typography>
      ))}
      <NewMenu open={open} setOpen={setOpen} />
    </Box>
  );
};

export default MenusPage;
```

```js
// src/pages/backoffice/locations/index.tsx

import NewLocation from "@/components/NewLocation";
import { useAppSelector } from "@/store/hooks";
import { Box, Button, Typography } from "@mui/material";
import { useState } from "react";

const LocationsPage = () => {
  const [open, setOpen] = useState(false);
  const locations = useAppSelector((state) => state.location.items);
  return (
    <Box>
      <Box sx={{ display: "flex", justifyContent: "flex-end" }}>
        <Button variant="contained" onClick={() => setOpen(true)}>
          New location
        </Button>
      </Box>
      {locations.map((item) => (
        <Typography key={item.id}>{item.name}</Typography>
      ))}
      <NewLocation open={open} setOpen={setOpen} />
    </Box>
  );
};

export default LocationsPage;
```

```js
// src/pages/backoffice/tables/index.tsx
import NewTable from "@/components/NewTable";
import { useAppSelector } from "@/store/hooks";
import { Box, Button, Typography } from "@mui/material";
import { useState } from "react";

const TablesPage = () => {
  const [open, setOpen] = useState(false);
  const tables = useAppSelector((state) => state.table.items);
  return (
    <Box>
      <Box sx={{ display: "flex", justifyContent: "flex-end" }}>
        <Button variant="contained" onClick={() => setOpen(true)}>
          New table
        </Button>
      </Box>
      {tables.map((item) => (
        <Typography key={item.id}>{item.name}</Typography>
      ))}
      <NewTable open={open} setOpen={setOpen} />
    </Box>
  );
};

export default TablesPage;
```

- ခု ဆိုရင်တော့ store ထဲက data တွေကို သက်ဆိုင်ရာpage တွေမှာ ပြပေးထားလိုက်ပြီးပဲ ဖြစ်ပါတယ်

##

### 5. set current location and Create New Menu Category

- setting page ထဲမှာတော့ current location ပြောင်းလို့ရမယ့် setting တစ်ခုကို သတ်မှတ်မှာဖြစ်ပါတယ်

```js
// src/pages/backoffice/settings/index.tsx

import { useAppSelector } from "@/store/hooks";
import {
  Box,
  FormControl,
  InputLabel,
  MenuItem,
  Select,
  SelectChangeEvent,
} from "@mui/material";
import { useState } from "react";

const SettingsPage = () => {
  const locations = useAppSelector((state) => state.location.items);
  const [locationId, setLocationId] = useState<number>();

  const handleLocationChange = (evt: SelectChangeEvent<number>) => {
    localStorage.setItem("selectedLocationId", String(evt.target.value));
  };

  return (
    <Box>
      <FormControl fullWidth>
        <InputLabel>Location</InputLabel>
        <Select
          value={locationId}
          label="Location"
          onChange={handleLocationChange}
        >
          {locations.map((item) => (
            <MenuItem value={item.id}>{item.name}</MenuItem>
          ))}
        </Select>
      </FormControl>
    </Box>
  );
};

export default SettingsPage;
```

- mui က selector ကို သုံးပြီး location တွေ ရွေးလို့ရအောင် လုပ်ထားလိုက်ပါတယ်
- location တစ်ခုခုကို ရွေးလိုက်တိုင်း selectedLocationId ဆိုတဲ့ key နဲ့ localStroage မှာ သိမ်းထားလိုက်တာပဲဖြစ်ပါတယ်

![](https://cdn.discordapp.com/attachments/1153197808900395062/1163683832801808444/image.png?ex=65407805&is=652e0305&hm=92a656c7156b88204b58d6704bf1ef9f136f1f019a5d45f187e6f460be231743&)

##

### create new item (menu category)

- ဆက်ပြီး menu category တစ်ခုကို create လုပ်ကြည့်ပါမယ်
- menu category page မှာ new menu category ကို နှိပ်လိုက်ရင် dialog box နဲ့ ပြထားပေးတာကို အရင် သင်ခန်းစာမှာ လုပ်ခဲ့ကြပါတယ်
- ခု သင်ခန်းစာမှာတော့ mui text field နဲ့ button တွေ ထပ်ထည့်ပြီး အသစ်ဖန်တီးလို့ရအောင် ဆက်လုပ်သွားပါမယ်

```js
// src/components/NewMenuCategory.tsx

import { useAppDispatch } from "@/store/hooks";

import {
  Box,
  Button,
  Dialog,
  DialogActions,
  DialogContent,
  DialogTitle,
  TextField,
} from "@mui/material";
import { Dispatch, SetStateAction, useState } from "react";

interface Props {
  open: boolean;
  setOpen: Dispatch<SetStateAction<boolean>>;
}

const NewMenuCategory = ({ open, setOpen }: Props) => {
  const [name, setName] = useState("");
  const dispatch = useAppDispatch();

  const onSuccess = () => {
    setOpen(false);
  };

  const handleCreateMenuCategory = () => {
    const selectedLocationId = localStorage.getItem("selectedLocationId");
    console.log("name:", name, "location id:", selectedLocationId);
  };

  return (
    <Dialog open={open} onClose={() => setOpen(false)}>
      <DialogTitle>Create new menu category</DialogTitle>
      <DialogContent>
        <Box sx={{ mt: 1 }}>
          <TextField
            label="Name"
            variant="outlined"
            autoFocus
            sx={{ width: "100%" }}
            onChange={(evt) => setName(evt.target.value)}
          />
        </Box>
      </DialogContent>
      <DialogActions sx={{ mr: 1, mb: 1 }}>
        <Button variant="contained" onClick={() => setOpen(false)}>
          Cancel
        </Button>

        <Button
          variant="contained"
          disabled={!name}
          onClick={handleCreateMenuCategory}
        >
          Confirm
        </Button>
      </DialogActions>
    </Dialog>
  );
};

export default NewMenuCategory;
```

- name state တစ်ခု လုပ်ထားပြီး dialog content မှာ ထည့်ပေးထားတဲ့ text field(input) မှာ တစ်ခုခု ပြောင်းလိုက်တိုင်း name state ကိုပါ update လုပ်ပေးထားလိုက်ပါတယ်
- dialog action မှာေတာ့ button နှစ်ခု ထည့်ပေးထားလိုက်ပြီး
  - cancel button ကို နှိပ်လိုက်ရင်တော့ dialog box ကို ပြန်ပိတ်ထားပေးလိုက်ပါတယ်
  - confirm button ကိုတော့ လိုအပ်တဲ့ data (name) ရှိမှ နှိပ်ခွင့်ပေးထားပြီး `handleCreateMenuCategory` function ကို ခေါ်ပေးထားပါတယ်
  - `handleCreateMenuCategory` function မှာတော့ localStroage မှာ သိမ်းထားတဲ့ location id ကို လှမ်းယူလိုက်ပြီး name နဲ့အတူ log ထုတ်ကြည့်ထားပါတယ်
- next foodie app ကို run ပြီး menu category တစ်ခုကို create လုပ်ကြည့်လိုက်ပါမယ်
-

![](https://cdn.discordapp.com/attachments/1153197808900395062/1163691322528518175/image.png?ex=65407eff&is=652e09ff&hm=361eaaf430e6cb3a3031e32391f2058906702540712346db453df847f1a50c8c&)

##

### ရလာတဲ့ name နဲ့ location id ကို သုံးပြီး api server ကနေတစ်ဆင့် database မှာ menu category တစ်ခုကို create လုပ်ပေးရမှာဖြစ်ပါတယ်

- အရင်ဆုံး menu category slice မှာ thunk action တစ်ခုကို သတ်မှတ်ပြီး server ဆီ post method နဲ့ request လုပ်လိုက်ပါမယ်

```js
import { MenuSlice } from "@/types/menu";
import { CreateMenuCategoryOptions } from "@/types/menuCategory";
import { config } from "@/utils/config";
import { createAsyncThunk, createSlice } from "@reduxjs/toolkit";

const initialState: MenuSlice = {
  items: [],
  isLoading: false,
  error: null,
};

//create thunk-action
export const createMenuCategory = createAsyncThunk(
  "menuCategory/createMenuCategory",
  async (options: CreateMenuCategoryOptions, thunkApi) => {
    const { name, locationId, onSuccess, onError } = options;
    try {
      const response = await fetch(`${config.apiBaseUrl}/menu-category`, {
        method: "POST",
        headers: { "content-type": "application/json" },
        body: JSON.stringify({ name, locationId }),
      });
      const menuCategory = await response.json();
      thunkApi.dispatch(addMenuCategory(menuCategory));
      onSuccess && onSuccess();
    } catch (err) {
      onError && onError();
    }
  }
);

const menuCategorySlice = createSlice({
  name: "menuCategory",
  initialState,
  reducers: {
    setMenuCategories: (state, action) => {
      state.items = action.payload;
    },

    // add new item(action)
    addMenuCategory: (state, action) => {
      state.items = [...state.items, action.payload];
    },
  },
});

export const { setMenuCategories, addMenuCategory } = menuCategorySlice.actions;
export default menuCategorySlice.reducer;
```

- `/api/menu-category ` route ကို POST method နဲ့ request လုပ်လိုက်ပြီး bodyအနေနဲ့ action ကို dispatch လုပ်တဲ့အချိန် ၀၀င်လာမယ့် parameterကို ထည့်ပေးထားလိုက်ပါတယ်
- အောက်မှာလည်း addMenuCategory action တစ်ခုကို သတ်မှတ်ထားပြီး
- create menu category thunk မှာ ပြန်ပြီး dispatch လုပ်ကာ server ကနေ response ပြန်လာတဲ့ dataကို parameter အဖြစ် ထည့်ပေးလိုက်ပါတယ်
- addMenuCategory action မှာတော့ ရလာတဲ့ payload ကို မူလ array ထဲမှာ ထပ်ပေါင်းထည့်လိုက်တာပဲ ဖြစ်ပါတယ်
- ခြုံပြီးပြောရရင် **server ဆီကို** name နဲ့ location id ကို ပို့ပြီး **request လုပ်လိုက်ပြီး** serverဆီက **ပြန်လာတဲ့ dataကို storeထဲရှိ menu category array ထဲ ပေါင်းထည့်**ပေးလိုက်တာပဲဖြစ်ပါတယ်
- အထက်ပါ createMenuCategory thunk-action ကို NewMenuCategory component က confirm button ကို click လိုက်ချိန်မှာ dispatch လုပ်လိုက်ပါမယ်

![](https://cdn.discordapp.com/attachments/1153197808900395062/1163695745543720980/image.png?ex=6540831e&is=652e0e1e&hm=30d920db21739f3d8cf85bd014cec220796ee4b6341a4e7d81d2879710ddd579&)

- frontend ကနေ request အတွက် ပြင်ဆင်ပြီးပြီးမို့ api(backend) မှာ အဲ့ဒီrequset ကို လက်ခံလိုက်ပါမယ်

```js
// src/pages/api/menu-category/index.ts

import { prisma } from "@/utils/db";
import type { NextApiRequest, NextApiResponse } from "next";
import { getServerSession } from "next-auth";
import { authOptions } from "../auth/[...nextauth]";

export default async function handler(
  req: NextApiRequest,
  res: NextApiResponse
) {
  // 1. check user login
  const session = await getServerSession(req, res, authOptions);
  if (!session) return res.status(401).send("Unauthorized");

  const method = req.method;

  if (method === "POST") {
    // 2. Data validation
    const { name, locationId } = req.body;
    const isValid = name && locationId;
    if (!isValid) return res.status(400).send("Bad request.");

    // 3. get current location
    const location = await prisma.location.findFirst({
      where: { id: locationId },
    });
    if (!location) return res.status(400).send("Bad request.");

    // 4. create new menu category
    const menuCategory = await prisma.menuCategory.create({
      data: { name, companyId: location.companyId },
    });

    // 5. response
    return res.status(200).json(menuCategory);
  }
  res.status(405).send("Method now allowed.");
}
```

1. User login ၀င်ထားတာကို စစ်လိုက်ပါတယ်
2. Request နဲ့ အတူပါလာတဲ့ dataတွေကို မှန် မမှန် ထပ်စစ်ပါတယ်
3. dataတွေ မှန်ခဲ့တယ်ဆိုရင် dataထဲက location id ကို သုံးပြီး location ကို db ကနေ ရှာလိုက်ပါတယ်
4. menuCategory တစ်ခုကို create လုပ်လိုက်ပြီး name ကို req.body ထဲက name , companyId ကို location ထဲက company id အဖြစ် သတ်မှတ်ပေးလိုက်ပါတယ်
5. ေနာက်ဆုံးမှာတေ့ာ အသစ်create လုပ်လိုက်တဲ့ menu category ကို ပဲ response ပြန်ပေးလိုက်တာပဲဖြစ်ပါတယ်

### foodie app ထဲမှာ menu category တစ်ခု create လုပ်ကြည့်လိုက်ပါမယ်

![](https://cdn.discordapp.com/attachments/1153197808900395062/1163699536066269214/image.png?ex=654086a5&is=652e11a5&hm=3ef825ca47ef903e5dfcaf277a53aceb49a811ea26686d0e49f334e84fec7968&)

- Hot Dish ဆိုတဲ့ category တစ်ခုကို create လုပ်ကြည့်လိုက်တဲ့အခါ menu category page မှာ ချက်ချင်း ပြပေးသလို storeထဲမှာလည်း update ဖြစ်သွားတာကို မြင်ရမှာဖြစ်ပါတယ်
