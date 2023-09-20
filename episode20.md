## MSquare Programming Fullstack Course

### Batch 2

### Episode-_19_ Summary

## Redux toolkit ( recup + tunk action)

### ပြီးခဲ့တဲ့ သင်ခန်းစာမှာလေ့လာခဲ့တဲ့ redux toolkit အကြောင်းကို အနည်းငယ် ပြန်နွှေးကြည့်ရအောင်

- redux-toolkit မှာ အဓိက အနေနဲ့

  - store
  - slice
  - reducer
  - action
  - dispatch
  - hook

- စတာတွေ ရှိပါတယ်

### store

- data တွေ သိမ်းတဲ့နေရာဖြစ်ပါတယ်

### slice

- store ထဲက data တွေကို store အသေးတစ်ခုချင်းစီ ခွဲပြီး သိမ်းတာဖြစ်ပါတယ်

### reducer

- store ထဲက data တွေရဲ့ state ကို manage လုပ်ပေးတဲ့ အရာဖြစ်ပါတယ်

### action

- reducer ကို အလုပ်ခိုင်းပေးတဲ့ function ဖြစ်ပါတယ်

### dispatch

- frontend နဲ့ action ကို ဆက်သွယ်ပေးတဲ့ function ဖြစ်ပါတယ်

### hook

- store ထဲက data တွေကို လှမ်းယူချင်ရင် `useSelecter()` hook ကို သုံးပြီး
- store ထဲက data တွေကို update လုပ်ပေးမယ့် reducer ထဲက action ကို dispatch လုပ်ချင်ရင် `useDispatch()` hook ကို သုံးပေးရပါတယ်

##

### setup redux-toolkit

1. create slice
   ![](https://cdn.discordapp.com/attachments/1153197808900395062/1153385720648585386/image.png)

![](https://cdn.discordapp.com/attachments/1153197808900395062/1153385823136391272/image.png)

2. create store and import all slice(s) to store

![](https://cdn.discordapp.com/attachments/1153197808900395062/1153384839223980032/image.png)

3. create custom hook

```js
// store / hooks.ts

import type { TypedUseSelectorHook } from "react-redux";

import { useDispatch, useSelector } from "react-redux";

import type { AppDispatch, RootState } from "./";

// Use throughout your app instead of plain `useDispatch` and `useSelector`

export const useAppDispatch: () => AppDispatch = useDispatch;

export const useAppSelector: TypedUseSelectorHook<RootState> = useSelector;
```

##

### Tunk (async action)

- next js က component တွေက UI မှာပြပေးတဲ့ အလုပ်ကို အဓိကထားပြီး ရေးပေးလေ့ရှိကြပါတယ်
- server ဆီ request လုပ်တဲ့ function တွေကို store ထဲ သက်ဆိုင်ရာ slice တွေဆီကနေပဲ action တွေ အနေနဲ့ သတ်မှတ်ပေးထားလေ့ရှိပြီး component တွေကနေ dispatch လုပ်ကာ သုံးလေ့ရှိကြပါတယ်
- async ဖြစ်တဲ့ action တွေကို slice တွေထဲက ပုံမှန် action တွေရှိတဲ့ reducers object ထဲမှာ သတ်မှတ်ပေးလို့မရပဲ အပြင်မှာ `createAsyncThunk()` method နဲ့ သတ်မှတ်ပေးရမှာဖြစ်ပါတယ်

### syntax

`const function-name = createAsyncThunk("action-name" , async( payload , tunkAPI)=>{...})`

### example

```js
const createData = createAsyncThunk(
  "data/createData",
  async (payload, thunkAPI) => {
    const response = await fetch("url", {
      method: "POST",
      body: JSON.stringyfy(payload),
    });
    const data = await response.json();
    thunkAPI.dispatch(action - name(data));
  }
);
```

- `createAsyncThunk()` method မှာ parameter နှစ်ခုလက်ခံပါတယ်
- ပထမ parameter ကတော့ action နာမည်ဖြစ်ပြီး
- ဒုတိယ parameter ကတော့ async function ဖြစ်ပါတယ်
- အဲ့ဒီ async function မှာလည်း parameter နှစ်ခု လက်ခံပါတယ်
  - ပထမ parameter ကတော့ ဒီ action ကို ခေါ်သုံးတဲ့အခါ parameter အနေနဲ့ ထည့်ပေးလိုက်တဲ့ data ဖြစ်ပြီး
  - ဒုတိယ parameter ကတော့ `createAsyncThunk()` method က လာမယ့် tunkAPI ဖြစ်ပါတယ်
  - tunkAPI ထဲက dispatch ကို သုံးပြီး အခြား action ကို ထပ်ခေါ်ပြီး dispatch လုပ်ပေးလို့ရပါတယ်
    > Important Note
- createSlice က လက်ခံတဲ့ reducers ထဲမှာ pure action (async မဟုတ်တဲ့ action )တွေသာ သတ်မှတ်လို့ရပြီး async ဖြစ်တဲ့ action တွေကိုတော့ အထက်ပါ `createAsyncThunk()` နဲ့ သီးခြားသတ်မှတ်ပေးရမှာဖြစ်ပါတယ်
- ခု menu တစ်ခု create လုပ်တဲ့ အခါမှာ server ဆီ "POST" method နဲ့ request ပို့တဲ့ fetch function ကို menu slice ထဲမှာ action အဖြစ် သတ်မှတ်တာကို အထက်က နည်းလမ်းကို သုံးပြီး ရေးကြည့်လိုက်ပါမယ်

```js
// src/store/slices/menuSlice.ts

import config from "@/config";
import { CreateMenuPayload, MenuState, UpdateMenuPayload } from "@/types/menu";
import { createAsyncThunk, createSlice } from "@reduxjs/toolkit";

const initialState: MenuState = {
  items: [],
  isLoading: false,
  error: null,
};

//thunk action
export const createMenu = createAsyncThunk(
  "menu/createMenu",
  async (payload: CreateMenuPayload, thunkApi) => {
    const response = await fetch(`${config.apiBaseUrl}/menu`, {
      method: "POST",
      headers: {
        "content-type": "application/json",
      },
      body: JSON.stringify(payload),
    });
    const menus = await response.json();
    thunkApi.dispatch(setMenus(menus));
  }
);

export const menuSlice = createSlice({
  name: "menu",
  initialState,
  reducers: {
    setMenus: (state, action) => {
      state.items = action.payload;
    },
  },
});

export const { setMenus } = menuSlice.actions;
export default menuSlice.reducer;
```

> ရှင်းလင်းချက်

- async ဖြစ်တဲ့ createMenu ဆိုတဲ့ action ကို slice ထဲမှာ ထည့်မရေးပဲ အပြင်မှာ createAsyncThunk ကို သုံးပြီး သတ်မှတ်ထားလိုက်ပါတယ်

```js
export const createMenu = createAsyncThunk(
  "menu/createMenu",
  async (payload: CreateMenuPayload, thunkApi) => {
    const response = await fetch(`${config.apiBaseUrl}/menu`, {
      method: "POST",
      headers: {
        "content-type": "application/json",
      },
      body: JSON.stringify(payload),
    });
    const menus = await response.json();
    thunkApi.dispatch(setMenus(menus));
  }
);
```

- createMenu action ထဲမှာတော့ createAsyncThunk ကို သုံးလိုက်ပြီး
- action name အဖြစ် `menu/createMenu` လို့ ပေးလိုက်ပါတယ်
- ဒုတိယ parameter ဖြစ်တဲ့ async action မှာတော့ menu တစ်ခု create လုပ်တဲ့အချိန်မှာ request လုပ်တဲ့ fetch ကို အသုံးပြုထားပါတယ်
- server ဆီ POST method နဲ့ request လုပ်ထားပြီး
- body အနေနဲ့ parameter အဖြစ် ၀င်လာမယ့် payload ကို ထည့်ပေးထားလိုက်ပါတယ်
- server က ပြန်လာတဲ့ response ကို menus အဖြစ် သတ်မှတ်ပြီး သိမ်းလိုက်ပါတယ်
- `thunkApi.dispatch(setMenus(menus))` thunkApi.dispatch နဲ့ setMenus action ကို dispatch လုပ်လိုက်ပြီး ခုနက သိမ်းထာတဲ့ menus ကို parameter အဖြစ် ထည့်ပေးလိုက်ပါတယ်
- အထက်ပါ flow ကို ပြန်ရှင်းပြရရင် create လုပ်လိုက်တဲ့ menu ကို server ဆီက menus array ထဲ ထည့်လိုက်ပြီး response ပြန်လိုက်တာပါ
- ပြန်ရလာတဲ့ response data ကို store ထဲက menus state အဖြစ် update လုပ်ပေးလိုက်တာပဲဖြစ်ပါတယ်
- createMenu component မှာလည်း server ဆီ fetch လုပ်စရာမလိုတော့ပဲ createMenu action ကိုသာ dispatch လုပ်ပေးလိုက်ရင် အရင် သင်ခန်းစာတုန်းကလိုပဲ menu တစ်ခုကို create လုပ်လို့ရပြီးဖြစ်ပါတယ်

![](https://cdn.discordapp.com/attachments/1153197808900395062/1154113409764315246/image.png)

```js
const handleCreateMenu = async () => {
  dispatch(createMenu(newMenu));
  setNewMenu(defaultNewMenu);
  //close dialog box
  setOpen(false);
};
```

- create button ကို နှိပ်လိုက်ချိန်မှာ menu slice ထဲက createMenu action ကို dispatch လုပ်လိုက်ပြီး newMenu state ကို parameter အနေနဲ့ ထည့်ပေးလိုက်တာပဲ ဖြစ်ပါတယ်

##

### menu category မှာလည်း အထက်ပါအတိုင်းပဲ ဆက်လုပ်လိုက်ပါမယ်

```js
// src/store/slices/menuCategorySlice.ts

import config from "@/config";
import {
  CreateMenuCategoryPayload,
  MenuCategoryState,
  UpdateMenuCategoryPayload,
} from "@/types/menuCategory";
import { createAsyncThunk, createSlice } from "@reduxjs/toolkit";

const initialState: MenuCategoryState = {
  items: [],
  isLoading: false,
  error: null,
};

export const createMenuCategory = createAsyncThunk(
  "menuCategory/createMenuCategory",
  async (payload: CreateMenuCategoryPayload, thunkApi) => {
    const response = await fetch(`${config.apiBaseUrl}/menu-category`, {
      method: "POST",
      headers: {
        "content-type": "application/json",
      },
      body: JSON.stringify(payload),
    });
    const menuCategories = await response.json();
    thunkApi.dispatch(setMenuCategories(menuCategories));
  }
);

export const menuCategorySlice = createSlice({
  name: "menuCategory",
  initialState,
  reducers: {
    setMenuCategories: (state, action) => {
      state.items = action.payload;
    },
  },
});

export const { setMenuCategories } = menuCategorySlice.actions;
export default menuCategorySlice.reducer;
```

![](https://cdn.discordapp.com/attachments/1153197808900395062/1154116395693514862/image.png)

##

### Update Menu component (dynamic route)

- UI မှာ ပြပေးနေတဲ့ menu တစ်ခုခုကို နှိပ်လိုက်ရင် အဲ့ဒီ menu id ကို သုံးပြီး route တစ်ခုစီ သွားထားတာကို [id] ဆိုတဲ့ folder နဲ့ dynamic route ကို လက်ခံပြီး UI မှာပြဖို့ component တစ်ခု လုပ်ထားတာကို ပြီးခဲ့တဲ့ သင်ခန်းစာမှာ လေ့လာခဲ့ကြပါတယ်

- ခု အဲ့ဒီ component မှာ နှိပ်လိုက်တဲ့ menu ရဲ့ name ကို update လုပ်နိုင်အောင် ဆက်လုပ်ကြပါမယ်

```js
// src/pages/backoffice/menu/[id]/index.tsx

import BackofficeLayout from "@/components/backofficeLayout";
import { useAppDispatch, useAppSelector } from "@/store/hooks";
import { updateMenu } from "@/store/slices/menuSlice";
import { Box, Button, TextField } from "@mui/material";
import { useRouter } from "next/router";
import { useState } from "react";

const UpdateMenu = () => {
  const [name, setName] = useState < string > "";
  const menus = useAppSelector((state) => state.menu.items);
  const router = useRouter();
  const menuId = Number(router.query.id);
  const menu = menus.find((item) => item.id === menuId);
  const dispatch = useAppDispatch();

  if (!menu) return null;

  return (
    <BackofficeLayout>
      <Box sx={{ display: "flex", flexDirection: "column" }}>
        <TextField
          id="name"
          label="Name"
          variant="outlined"
          defaultValue={menu?.name}
          sx={{ width: "fit-content" }}
          onChange={(evt) => setName(evt.target.value)}
        />
        <Button
          variant="contained"
          sx={{ width: "fit-content", mt: 2 }}
          onClick={() => dispatch(updateMenu({ id: menuId, name }))}
        >
          Update
        </Button>
      </Box>
    </BackofficeLayout>
  );
};

export default UpdateMenu;
```

`const [name, setName] = useState<string>("");`
</br>
- name state တစ်ခု သတ်မှတ်ထားပါတယ်
 </br></br>
  `const menus = useAppSelector((state) => state.menu.items);`
  </br>
- store ထဲမှာရှိတဲ့ menu တွေကို လှမ်းယူလိုက်ပါတယ်

  </br></br>
  `const router = useRouter();`
  </br>
- next js useRouter ကို သုံးပြီး router တစ်ခု သတ်မှတ်လိုက်ပါတယ်

  </br></br>
  `const menuId = Number(router.query.id);`
  </br>
- param အနေနဲ့ ၀င်လာတဲ့ menu id ကို လှမ်းယူလိုက်ပါတယ်
</br></br>
`const menu = menus.find((item) => item.id === menuId);`
</br>
- store ထဲက ယူထားတဲ့ menu တွေထဲမှာ လှမ်းယူထားတဲ့ id နဲ့ တူတဲ့ MENU item ကို ရှာထုတ်လိုက်တာပါ
  </br></br>
  `const dispatch = useAppDispatch();`
  </br>
- action တွေ ကို dispatch လုပ်ဖို့ useAppDIspatch ကို အသုံးပြုထားပါတယ်
</br></br>
  `if (!menu) return null;`
</br>
- အပေါ်မှာ ရှာလိုက်တဲ့ id နဲ့ menu ကို မတွေ့ခဲ့ရင် ဒီ component မှာ ဘာမှ render မလုပ်ခိုင်းလိုက်ပါဘူး
- ရှာလို့တွေ့ခဲ့မှ အောက်က retrun ထဲရှိ code တွေကို render လုပ်မှာဖြစ်ပါတယ်

```js
<TextField
          id="name"
          label="Name"
          variant="outlined"
          defaultValue={menu?.name}
          sx={{ width: "fit-content" }}
          onChange={(evt) => setName(evt.target
          value)}
        />
```

- ရှာလိုက်တဲ့ menu ထဲက name ကို textfilde ထဲမှာပြပေးထားလိုက်ပြီး တစ်ခုခု ပြင်လိုက်တိုင်း name state အဖြစ် update လုပ်ပေးလိုက်ပါတယ်

```js
<Button
  variant="contained"
  sx={{ width: "fit-content", mt: 2 }}
  onClick={() => dispatch(updateMenu({ id: menuId, name }))}
>
  Update
</Button>
```

- update ဆိုတဲ့ button တစ်ခု လုပ်ထားပြီး နှိပ်လိုက်တိုင်း updateMenu ဆိုတဲ့ action ကို dispatch လုပ်ထားလိုက်ပါတယ်
- updateMenu ဆိုတဲ့ action ကို menu slice ထဲမှာ သတ်မှတ်လိုက်ပါမယ်

![](https://cdn.discordapp.com/attachments/1153197808900395062/1154123908711194686/image.png)

- updateMenu action ကို thunk action အဖြစ် သတ်မှတ်ထားပြီး လောလောဆယ်တော့ log ပဲ ထုတ်ကြည့်ထားပါတယ်
- အခု menu တွေ create လုပ်ကြည့်ပြီး UI မှာပြပေးလာတဲ့ menu ထဲက တစ်ခုခုကို နှိပ်ကြည့်လိုက်ပါက အဲ့ဒီ menu နာမည်ကို ပြင်ကြည့်လို့ရမှာဖြစ်ပြီး update နှိပ်လိုက်ရင်တော့ ပြင်ထားတဲ့ name နဲ့ အဲ့ဒီ menu ရဲ့ id ကို log ထုတ်ပေးမှာဖြစ်ပါတယ်
