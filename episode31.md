## MSquare Programming Fullstack Course

### Batch 2

### Episode-_31_ Summary

### 1. Render all menus with ItemCard component

### 2. Create new menu

### 3. Mui Select component with multi select props

##

### 1. Render all menus with ItemCard component

- Location page မှာလုပ်ခဲ့သလိုပဲ menu page မှာလည်း menu တွေကို ItemCard component ကို သုံးပြီး ပြပေးလိုက်ပါမယ်

```js
import ItemCard from "@/components/ItemCard";
import NewMenu from "@/components/NewMenu";
import { useAppSelector } from "@/store/hooks";
import LocalDiningIcon from "@mui/icons-material/LocalDining";
import { Box, Button } from "@mui/material";
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
      <Box sx={{ display: "flex", flexWrap: "wrap" }}>
        {menus.map((item) => (
          <ItemCard
            href={`/backoffice/menus/${item.id}`}
            key={item.id}
            title={item.name}
            icon={<LocalDiningIcon />}
          />
        ))}
      </Box>
      <NewMenu open={open} setOpen={setOpen} />
    </Box>
  );
};

export default MenusPage;
```

- menus တွေကို ItemCard component နဲ့ ပြထားပြီး နှိပ်လိုက်တဲ့အခါကျရင် dynamic rtoute နဲ့ /bcakoffice/menus/:id ကို ပို့ပေးထားလိုက်ပါတယ်

##

### Create New Menu

- menu တစ်ခု အသစ်လုပ်နိုင်ဖို့ NewMenuComponent မှာ ဆက်ပြီး ပြင်ဆင်ပါမယ်

```js

import { useAppDispatch, useAppSelector } from "@/store/hooks";
import { createMenu } from "@/store/slices/menuSlice";
import { CreateMenuOptions } from "@/types/menu";
import {
  Box,
  Button,
  Checkbox,
  Dialog,
  DialogContent,
  DialogTitle,
  FormControl,
  InputLabel,
  ListItemText,
  MenuItem,
  Select,
  SelectChangeEvent,
  TextField,
} from "@mui/material";
import { MenuCategory } from "@prisma/client";
import { Dispatch, SetStateAction, useState } from "react";

interface Props {
  open: boolean;
  setOpen: Dispatch<SetStateAction<boolean>>;
}

// 1. default value for new menu
const defaultNewMenu = {
  name: "",
  price: 0,
  menuCategoryIds: [],
};

const NewMenu = ({ open, setOpen }: Props) => {
// 2 . new menu state
  const [newMenu, setNewMenu] = useState<CreateMenuOptions>(defaultNewMenu);

// 3. get all menu-category from store
  const menuCategories = useAppSelector((state) => state.menuCategory.items);
  const dispatch = useAppDispatch();

// 4. Selected menu-category function
  const handleOnChange = (evt: SelectChangeEvent<number[]>) => {
    const selectedIds = evt.target.value as number[];
    setNewMenu({ ...newMenu, menuCategoryIds: selectedIds });
  };

// 5. create menu
  const handleCreateMenu = () => {
    dispatch(createMenu({ ...newMenu, onSuccess: () => setOpen(false) }));
  };

  return (
    <Dialog
      open={open}
      onClose={() => {
        setNewMenu(defaultNewMenu);
        setOpen(false);
      }}
    >
      <DialogTitle>Create new menu</DialogTitle>
      <DialogContent
        sx={{ display: "flex", flexDirection: "column", width: 400 }}
      >
      </* Get name and price*/>
        <TextField
          placeholder="Name"
          sx={{ mb: 2 }}
          onChange={(evt) => setNewMenu({ ...newMenu, name: evt.target.value })}
        />
        <TextField
          placeholder="Price"
          sx={{ mb: 2 }}
          onChange={(evt) =>
            setNewMenu({ ...newMenu, price: Number(evt.target.value) })
          }
        />

        </* Get selected menu-category*/>
        <FormControl fullWidth>
          <InputLabel>Menu Category</InputLabel>
          <Select
            multiple
            value={newMenu.menuCategoryIds}
            label="Menu Category"
            onChange={handleOnChange}
            renderValue={(selectedMenuCategoryIds) => {
              return selectedMenuCategoryIds
                .map((selectedMenuCategoryId) => {
                  return menuCategories.find(
                    (item) => item.id === selectedMenuCategoryId
                  ) as MenuCategory;
                })
                .map((item) => item.name)
                .join(", ");
            }}
            MenuProps={{
              PaperProps: {
                style: {
                  maxHeight: 48 * 4.5 + 8,
                  width: 250,
                },
              },
            }}
          >
            {menuCategories.map((item) => (
              <MenuItem key={item.id} value={item.id}>
                <Checkbox checked={newMenu.menuCategoryIds.includes(item.id)} />
                <ListItemText primary={item.name} />
              </MenuItem>
            ))}
          </Select>
        </FormControl>
        <Box sx={{ mt: 3, display: "flex", justifyContent: "flex-end" }}>

          <Button
            variant="contained"
            sx={{ mr: 2 }}
            onClick={() => setOpen(false)}
          >
            Cancel
          </Button>
          <Button
            variant="contained"
            disabled={!newMenu.name || !newMenu.menuCategoryIds.length}
            onClick={handleCreateMenu}
          >
            Confirm
          </Button>
        </Box>
      </DialogContent>
    </Dialog>
  );
};

export default NewMenu;


```

> ရှင်းလင်းချက်

```js
// 1. default value for new menu
const defaultNewMenu = {
  name: "",
  price: 0,
  menuCategoryIds: [],
};
```

- name , price , menuCategoryId array တွေ ပါတဲ့ default variable တစ်ခုသတ်မှတ်ထားပါတယ်

```js
// 2 . new menu state
const [newMenu, setNewMenu] = useState < CreateMenuOptions > defaultNewMenu;
```

- အပေါ်မှာ သတ်မှတ်ထားတဲ့ default value ကို newMenu ရဲ့ မူလတန်ဖို့အဖြစ်သတ်မှတ်ပြီး state တစ်ခု လုပ်ထားလိုက်ပါတယ်

```js
// 3. get all menu-category from store
const menuCategories = useAppSelector((state) => state.menuCategory.items);
```

- store ထဲရှိ menu category တွေ အကုန်လှမ်းယူထားလိုက်ပါတယ်

```js

// 4. Selected menu-category function
  const handleOnChange = (evt: SelectChangeEvent<number[]>) => {
    const selectedIds = evt.target.value as number[];
    setNewMenu({ ...newMenu, menuCategoryIds: selectedIds });
  };
```

- menu-category တစ်ခုခုကို ရွေးလိုက်ချိန်မှာ run ပေးမယ့် function ဖြစ်ပါတယ်
- ဒီ function ကို return ထဲမှာ render လုပ်ထားတဲ့ MUI select component နဲ့ တွဲပြီး ေအာက်မှာ ရှင်းပြထားပါမယ်

```js
// 5. create menu
const handleCreateMenu = () => {
  dispatch(createMenu({ ...newMenu, onSuccess: () => setOpen(false) }));
};
```

- ဒါကတော့ create buttom ကို click လိုက်ချိန်မှာ run ပေးမယ့် function ဖြစ်ပါတယ်
- createMenu action ကို dispatch လုပ်ထားပြီး dialog box ကို ပြန်ပိတ်ထားတာပဲ ဖြစ်ပါတယ်

```js
 </* Get name and price*/>
        <TextField
          placeholder="Name"
          sx={{ mb: 2 }}
          onChange={(evt) => setNewMenu({ ...newMenu, name: evt.target.value })}
        />
        <TextField
          placeholder="Price"
          sx={{ mb: 2 }}
          onChange={(evt) =>
            setNewMenu({ ...newMenu, price: Number(evt.target.value) })
          }
        />

```

- return လုပ်ထားတဲ့အထဲမှာတော့ new menu အတွက် NAME နဲ့ PRICE ကို input လုပ်လို့ရမယ့် text field တွေ ထည့်ပေးထားပြီး newMenu state ကို update လုပ်ပေးထားတာပဲဖြစ်ပါတယ်

```js
 </* Get selected menu-category*/>
        <FormControl fullWidth>
          <InputLabel>Menu Category</InputLabel>
          <Select
            multiple
            value={newMenu.menuCategoryIds}
            >
            .......
            ......
            ......
          </Select>
        </FormControl>

```

- menu-category တစ်ခုထပ်ပိုပြီးရွေးလို့ရမယ့် Multi select component ကိုလည်း return မှာ ထည့်ပေးထားတယ်
- သူ့ရဲ့ အလုပ်လုပ်ပုံကို onChange function နဲ့ တွဲပြီး အောက်မှာ ရှင်းပြပေးထားပါတယ်

```js
<Box sx={{ mt: 3, display: "flex", justifyContent: "flex-end" }}>
  <Button variant="contained" sx={{ mr: 2 }} onClick={() => setOpen(false)}>
    Cancel
  </Button>
  <Button
    variant="contained"
    disabled={!newMenu.name || !newMenu.menuCategoryIds.length}
    onClick={handleCreateMenu}
  >
    Confirm
  </Button>
</Box>
```

- နောက်ဆုံးမှာတော့ cancel ခလုတ်နဲ့ create ခလုတ်တွေကို ပြပေးထားပြီး
- cancel က dialog ပိတ်ပေးမယ့် ခလုတ်ဖြစ်ပြီး
- create က menu တစ်ခုကို create မယ့် ACTION ကို dispatch လုပ်ပေးမယ့် ခလုတ်ဖြစ်ပါတယ်

##

### Mui multi select component

- MUI select component uကို setting page ကို လုပ်ခဲ့တုန်းက location ရွေးလို့ရအောင် သုံးခဲ့ကြပါတယ်

```js
const handleLocationChange = (evt: SelectChangeEvent<number>) => {
  localStorage.setItem("selectedLocationId", String(evt.target.value));
  setLocationId(Number(evt.target.value));
};

if (!locationId) return null;

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
          <MenuItem key={item.id} value={item.id}>
            {item.name}
          </MenuItem>
        ))}
      </Select>
    </FormControl>
  </Box>
);
```

- MUI select component မှာ **internal-value** နဲ့ **external-value** ဆိုပြီး value နှစ်ခုရှိပါတယ်
- Selcet Component ထဲမှာ props အဖြစ် သုံးထားတဲ့

```js
        <Select
          *value*={locationId}
          label="Location"
          onChange={handleLocationChange}
        >

```

- value( locationId ) ကို internal value အဖြစ် သတ်မှတ်ပါတယ်

##

- select ရဲ့ UI မှာပြပေးတဲ့ value (Sanchaung) ကိုတော့ external-value လို့ သတ်မှတ်ပါတယ်

![](https://cdn.discordapp.com/attachments/1153197808900395062/1166062628855095417/image.png?ex=65491f73&is=6536aa73&hm=6830b38db909f0fa797250b946ad53609785723f32525f0c5bafd0d7c0374bc4&)

- ခု သင်ခန်းစာမှာ သုံးမယ့် Select component က တစ်ခုထပ်ပိုပြီး ရွေးလို့ရမယ့် multi select component ဖြစ်ပါတယ်
- နောက်ထပ် component တစ်ခုကို ထပ်သုံးရမှာ မဟုတ်ပဲ mui select component ကိုပဲ multiple ဆိုတဲ့ prop ကို သုံးပြီး လုပ်သွားမှာပဲ ဖြစ်ပါတယ်

```js
// new menu state
const defaultNewMenu = {
  name: "",
  price: 0,
  menuCategoryIds: [],
};

  const [newMenu, setNewMenu] = useState<CreateMenuOptions>(defaultNewMenu);

//////////////////

//  Selected menu-category function
  const handleOnChange = (evt: SelectChangeEvent<number[]>) => {
    const selectedIds = evt.target.value as number[];
    setNewMenu({ ...newMenu, menuCategoryIds: selectedIds });
  };

  /////////////////

 <FormControl fullWidth>
          <InputLabel>Menu Category</InputLabel>
          <Select
            multiple
            value={newMenu.menuCategoryIds}
            label="Menu Category"
            onChange={handleOnChange}
            renderValue={(selectedMenuCategoryIds) => {
              return selectedMenuCategoryIds
                .map((selectedMenuCategoryId) => {
                  return menuCategories.find(
                    (item) => item.id === selectedMenuCategoryId
                  ) as MenuCategory;
                })
                .map((item) => item.name)
                .join(", ");
            }}
            MenuProps={{
              PaperProps: {
                style: {
                  maxHeight: 48 * 4.5 + 8,
                  width: 250,
                },
              },
            }}
          >
            {menuCategories.map((item) => (
              <MenuItem key={item.id} value={item.id}>
                <Checkbox checked={newMenu.menuCategoryIds.includes(item.id)} />
                <ListItemText primary={item.name} />
              </MenuItem>
            ))}
          </Select>
        </FormControl>
```

> ရှင်းလင်းချက်

```js
       <FormControl fullWidth>
          <InputLabel>Menu Category</InputLabel>
          <Select
            multiple
            value={newMenu.menuCategoryIds}
            label="Menu Category"
            onChange={handleOnChange}
            ...............
            .............
            ........
```

- select component မှာ multiple ဆိုတဲ့ prop ကို ထည့်ပေးထားပြီး internal-value အနေနဲ့ new menu state ထဲက menuCategoryIds array ကို ထည့်ပေးထားပါတယ်
- multiple ဖြစ်သွားပြီးမလို့ array ကို value အဖြစ် လက်ခံမှာဖြစ်သလို တစ်ခုခုပြောင်းလဲလိုက်ရင် လဲ (onChange) array ကို event.taeget.value အဖြစ် ထုတ်ပေးမှာဖြစ်ပါတယ်

```js
//  Selected menu-category function
  const handleOnChange = (evt: SelectChangeEvent<number[]>) => {
    const selectedIds = evt.target.value as number[];
    setNewMenu({ ...newMenu, menuCategoryIds: selectedIds });
  };
```

- handleOnChange function မှာလည်း ထုတ်ပေးလိုက်တဲ့ array ကို selectedIds ဆိုတဲ့ array အဖြစ်နဲ့ သိမ်းလိုက်ပြီး new menu state ထဲက menucategoryIds array ကို သိမ်းထားတဲ့selectedIds ဆိုတဲ့ array နဲ့ update လုပ်ပေးလိုက်တာပဲဖြစ်ပါတယ်

```js
            renderValue={(selectedMenuCategoryIds) => {
              return selectedMenuCategoryIds
                .map((selectedMenuCategoryId) => {
                  return menuCategories.find(
                    (item) => item.id === selectedMenuCategoryId
                  ) as MenuCategory;
                })
                .map((item) => item.name)
                .join(", ");
            }}
```

- external-value အဖြစ် ပြပေးမယ့် function ဖြစ်ပါတယ်
- renderValue prop ကို ေခါ်သုံးလိုက်ရင် select component က inetrnal-value ကို parameter တစ်ခုထည့်ပေးလိုက်မှာဖြစ်ပါတယ်
- ရလာတဲ့ internal-value ကို map လုပ်ပြီး store ထဲကနေ လှမ်းယူထားတဲ့ menu category တွေ အားလုံးထဲမှာ သွားရှာလိုက်ပါတယ်
- ရှာလို့တွေ့လာတဲ့ menu category array ထဲက name တွေ ကို join လုပ်ပြီး external-value အနေနဲ့ UI မှာပြပေးလိုက်တာပဲဖြစ်ပါတယ်

```js
            MenuProps={{
              PaperProps: {
                style: {
                  maxHeight: 48 * 4.5 + 8,
                  width: 250,
                },
              },
            }}
          >
            {menuCategories.map((item) => (
              <MenuItem key={item.id} value={item.id}>
                <Checkbox checked={newMenu.menuCategoryIds.includes(item.id)} />
                <ListItemText primary={item.name} />
              </MenuItem>
            ))}
```

- MenuProps ဆိုတာက အောက်မှာ MAP လုပ်ပြီး select လုပ်လို့ရအောင်ပြပေးမယ့် MenuItem ကို CSS ပေးထားတာပါ
- အောက်မှတော့ menu category တွေ အကုန်လုံးကို map လုပ်ပြီး ပြပေးထားပါတယ်
- select လုပ်ထား/မထား သိရအောင် check box နဲ့ တွဲပြထားပြီး newMenu ထဲက menuCategoryIds array ထဲမှာ ရှိတဲ့item တွေကို check လုပ်ပေးထားလိုက်တာပဲဖြစ်ပါတယ်
- ![](https://cdn.discordapp.com/attachments/1153197808900395062/1166330114389323916/image.png?ex=654a1890&is=6537a390&hm=cfec0c6658a0a635adbba7c6e11cd14e7279046a6e322e8be7f319bfa418a150&)

##

- အစပိုင်းမှာ ရှင်ပြခဲ့တဲ့အတိုင်း confirm ကို နှိပ်လိုက်ချိန်မှာ createMenu ဆိုတဲ့ action ကို dispatch လုပ်ထားပါတယ်
- အဲ့ဒီ action ကို menuSlice ထဲမှာ သတ်မှတ်လိုက်ပါမယ်

```js
// src/store/slices/menuSlice.ts -->

export const createMenu = createAsyncThunk(
  "menu/createMenu",
  async (options: CreateMenuOptions, thunkApi) => {
    const { name, price, menuCategoryIds, onSuccess, onError } = options;
    try {
      const response = await fetch(`${config.apiBaseUrl}/menu`, {
        method: "POST",
        headers: { "content-type": "application/json" },
        body: JSON.stringify({ name, price, menuCategoryIds }),
      });
      const { menu, menuCategoryMenus } = await response.json();
      thunkApi.dispatch(addMenu(menu));
      thunkApi.dispatch(addMenuCategoryMenu(menuCategoryMenus));
      onSuccess && onSuccess();
    } catch (err) {
      onError && onError();
    }
  }
);

/////////

// action
    addMenu: (state, action) => {
      state.items = [...state.items, action.payload];
    },


```

- createMenu action မှာတော့ payload ကို body အနေနဲ့ သုံးပြီး /api/menu route ဆီ POST method နဲ့ request လုပ်ထားပါတယ်
- server က response ပြန်လာတဲ့ data တွေကို သက်ဆိုင်ရာ action တွေမှာ parameter အဖြစ် ထည့်ပြီး dispatch ထပ်လုပ်ထားလိုက်ပါတယ်
- thunk action မှာ dispatch လုပ်ထားတဲ့ addMenuCategoryMenu action ကိုလည်း menuCategoryMenuSlice ထဲမှာလည်း သတ်မှတ်ထားလိုက်ပါမယ်

```js
// src/store/slices/menuCategoryMenuSlice .ts

import { MenuCategoryMenuSlice } from "@/types/menuCategoryMenu";
import { createSlice } from "@reduxjs/toolkit";

const initialState: MenuCategoryMenuSlice = {
  items: [],
  isLoading: false,
  error: null,
};

const menuCategoryMenuSlice = createSlice({
  name: "menuCategoryMenuSlice",
  initialState,
  reducers: {
    setMenuCategoryMenus: (state, action) => {
      state.items = action.payload;
    },
    addMenuCategoryMenu: (state, action) => {
      state.items = [...state.items, ...action.payload];
    },
  },
});

export const { setMenuCategoryMenus, addMenuCategoryMenu } =
  menuCategoryMenuSlice.actions;
export default menuCategoryMenuSlice.reducer;
```

##

### server မှာ request ကို လက်ခံပြီး response ပြန်ပါမယ်

```js
// src/pages/api/menu/index.ts

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
  if (!session) return res.status(401).send("Unauthorized");
  const method = req.method;
  if (method === "POST") {
    const { name, price, menuCategoryIds } = req.body;
    const isValid = name && price !== undefined && menuCategoryIds.length > 0;
    if (!isValid) return res.status(400).send("Bad request.");
    const menu = await prisma.menu.create({ data: { name, price } });
    const newMenuCategoryMenu: { menuCategoryId: number, menuId: number }[] =
      menuCategoryIds.map((item: number) => ({
        menuCategoryId: item,
        menuId: menu.id,
      }));
    const menuCategoryMenus = await prisma.$transaction(
      newMenuCategoryMenu.map((item) =>
        prisma.menuCategoryMenu.create({
          data: { menuCategoryId: item.menuCategoryId, menuId: item.menuId },
        })
      )
    );
    return res.status(200).json({ menu, menuCategoryMenus });
  }
  res.status(405).send("Method not allowed.");
}
```

- server မှာတော့ data validation လုပ်ပြီး menu တစ်ခုကို create အရင်လုပ်လိုက်ပါတယ်
- ရလာတဲ့ menu row ထဲက id ကို သုံးပြီး req.body ထဲက menuCategory id တွေနဲ့ ေပါင်းကာ menuCategoryMenus row တွေကို menuCategoryMenus table မှာ ထပ်ပြီး create လုပ်လိုက်ပါတယ်
- နောက်ဆုံးမှာတော့ create လုပ်ထားတဲ့ menu နဲ့ menuCategoryMenus တွေကို response ပြန်ပေးလိုက်တာပဲဖြစ်ပါတယ်
