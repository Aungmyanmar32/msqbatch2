## MSquare Programming Fullstack Course

### Batch 2

### Episode-_32_ Summary

### 1. Update menu

### 2. Delete Menu

##

### 1. Update menu

- menu တစ်ခုကို update လုပ်တဲ့နေရာမှာ name,price နဲ့ join ထားမယ့် menu categopry တွေကို ပြင်လို့ရအောင် လုပ်မှာဖြစ်ပါတယ်။

```js

// src/pages/backoffice/menus/[id]/index.tsx

import { useAppDispatch, useAppSelector } from "@/store/hooks";
import { deleteMenu, updateMenu } from "@/store/slices/menuSlice";
import { UpdateMenuOptions } from "@/types/menu";
import {
  Box,
  Button,
  Checkbox,
  Chip,
  Dialog,
  DialogActions,
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
import { useRouter } from "next/router";
import { useState } from "react";

const MenuDetail = () => {
  const router = useRouter();
  const menuId = Number(router.query.id);
  const menus = useAppSelector((state) => state.menu.items);
  const menuCategories = useAppSelector((state) => state.menuCategory.items);
  const menuCategoryMenus = useAppSelector(
    (state) => state.menuCategoryMenu.items
  );
  const menu = menus.find((item) => item.id === menuId);
  const menuCategoryMenu = menuCategoryMenus.filter(
    (item) => item.menuId === menuId
  );
  const menuCategoryIds = menuCategoryMenu.map((item) => item.menuCategoryId);
  const [data, setData] = useState<UpdateMenuOptions>();
  const dispatch = useAppDispatch();
  const [open, setOpen] = useState(false);

  if (!menu || !data) return null;

  const handleOnChange = (evt: SelectChangeEvent<number[]>) => {
    const selectedIds = evt.target.value as number[];
    setData({ ...data, id: menuId, menuCategoryIds: selectedIds });
  };

  const handleUpdateMenu = () => {
    dispatch(updateMenu(data));
  };



  return (
    <Box sx={{ display: "flex", flexDirection: "column" }}>

      <TextField
        defaultValue={menu.name}
        sx={{ mb: 2 }}
        onChange={(evt) =>
          setData({ ...data, id: menuId, name: evt.target.value })
        }
      />
      <TextField
        defaultValue={menu.price}
        sx={{ mb: 2 }}
        onChange={(evt) =>
          setData({ ...data, id: menuId, price: Number(evt.target.value) })
        }
      />
      <FormControl fullWidth>
        <InputLabel>Menu Category</InputLabel>
        <Select
          multiple
          value={data.menuCategoryIds}
          label="Menu Category"
          onChange={handleOnChange}
          renderValue={(selectedMenuCategoryIds) => {
            return selectedMenuCategoryIds
              .map((selectedMenuCategoryId) => {
                return menuCategories.find(
                  (item) => item.id === selectedMenuCategoryId
                ) as MenuCategory;
              })
              .map((item) => <Chip label={item.name} sx={{ mr: 1 }} />);
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
              <Checkbox checked={data.menuCategoryIds.includes(item.id)} />
              <ListItemText primary={item.name} />
            </MenuItem>
          ))}
        </Select>
      </FormControl>
      <Button
        variant="contained"
        sx={{ mt: 2, width: "fit-content" }}
        onClick={handleUpdateMenu}
      >
        Update
      </Button>

    </Box>
  );
};

export default MenuDetail;

```

> ရှင်းလင်းချက်

```js
const router = useRouter();
const menuId = Number(router.query.id);
```

- router ကို သုံးပြီး dynamic အဖြစ် ၀င်လာတဲ့ id ကို menuId အဖြစ် သိမ်းလိုက်ပါတယ်

```js
const menus = useAppSelector((state) => state.menu.items);
const menuCategories = useAppSelector((state) => state.menuCategory.items);
const menuCategoryMenus = useAppSelector(
  (state) => state.menuCategoryMenu.items
);
```

- store ထဲက data တွေကို လှမ်းယူထားပါတယ်

```js
const menu = menus.find((item) => item.id === menuId);
const menuCategoryMenu = menuCategoryMenus.filter(
  (item) => item.menuId === menuId
);
const menuCategoryIds = menuCategoryMenu.map((item) => item.menuCategoryId);
```

- သိမ်းထားတဲ့ menuId နဲ့ menu ကိုရှာလိုက်ပါတယ်
- ထပ်ပြီးတော့ အဲ့ဒီmenu နဲ့ join ထားတဲ့ menuCategory တွေကို ရှာပြီး menu-category id သီးသန့် ထုတ်ကာ သိမ်းလိုက်တာပဲဖြစ်ပါတယ်

```js
  const [data, setData] = useState<UpdateMenuOptions>();
```

- update လုပ်မယ့် menu data တွေကို သိမ်းဖို့အတွက် data state တစ်ခုကို သတ်မှတ်လိုက်ပါတယ်

```js
  const handleOnChange = (evt: SelectChangeEvent<number[]>) => {
    const selectedIds = evt.target.value as number[];
    setData({ ...data, id: menuId, menuCategoryIds: selectedIds });
  };

  const handleUpdateMenu = () => {
    dispatch(updateMenu(data));
  };
```

- select component မှာ ပြောင်းလဲမှု ရှိရင် run မယ့် function နဲ့ update ကို နှိပ်လိုက်ချိန် run မယ့် function တွေဖြစ်ပါတယ်
- select component မှာ ပြောင်းလဲမှု ရှိရင် run မယ့် function မှာတော့ ရွေးလိုက်တဲ့ item တွေရဲ့ id ကို data state ထဲက menu-category id array အဖြစ် update လုပ်ထားလိုက်ပါတယ်
- update ကို နှိပ်လိုက်ချိန် run မယ့် functionမှာတော့ updateMenu action ကို dispatch လုပ်ထားတာပဲဖြစ်ပါတယ်

```js
      <TextField
        defaultValue={menu.name}
        sx={{ mb: 2 }}
        onChange={(evt) =>
          setData({ ...data, id: menuId, name: evt.target.value })
        }
      />
      <TextField
        defaultValue={menu.price}
        sx={{ mb: 2 }}
        onChange={(evt) =>
          setData({ ...data, id: menuId, price: Number(evt.target.value) })
        }
      />
```

- name နဲ့ price ကို update လုပ်လို့ရမယ့် textfiled တွေ ဖြစ်ပါတယ်

```js
      <FormControl fullWidth>
        <InputLabel>Menu Category</InputLabel>
        <Select
          multiple
          value={data.menuCategoryIds}
          label="Menu Category"
          onChange={handleOnChange}
          renderValue={(selectedMenuCategoryIds) => {
            return selectedMenuCategoryIds
              .map((selectedMenuCategoryId) => {
                return menuCategories.find(
                  (item) => item.id === selectedMenuCategoryId
                ) as MenuCategory;
              })
              .map((item) => <Chip label={item.name} sx={{ mr: 1 }} />);
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
              <Checkbox checked={data.menuCategoryIds.includes(item.id)} />
              <ListItemText primary={item.name} />
            </MenuItem>
          ))}
        </Select>
      </FormControl>
```

- MUI Select componet နဲ့ menu-category တွေ ရွေးလို့ရအောင် လုပ်ပေးထားပါတယ်
- (မှတ်ချက်)။ ။ MUI Select componet အလုပ်လုပ်ပုံကို အရင် သင်ခန်းစာမှာ အသေးစိတ် ဖတ်နိုင်/ကြည့်နိုင်ပါတယ်

```js
<Button
  variant="contained"
  sx={{ mt: 2, width: "fit-content" }}
  onClick={handleUpdateMenu}
>
  Update
</Button>
```

- နောက်ဆုံးမှာတော့ update button တစ်ခု ထည့်ပေးထားပြီး click လိုက်ရင် handleUpdateMenu function ကို runပေးထားလိုက်ပါတယ်

##

### create thunk action and api request

- update button ကို နှိပ်လိုက်တဲ့အချိန်မှာ updateMenu actionကို dispatch လုပ်ခဲ့ပါတယ်
- menuSlice မှာ updateMenu action ကို thunk-action အဖြစ် သတ်မှတ်ပြီး server ကို request ပို့မှာဖြစ်ပါတယ်

```js
// src/store/slices/menuSlice.ts --> updateMenu

export const updateMenu = createAsyncThunk(
  "menu/updateMenu",
  async (options: UpdateMenuOptions, thunkApi) => {
    const { id, name, price, menuCategoryIds, onSuccess, onError } = options;
    try {
      const response = await fetch(`${config.apiBaseUrl}/menu`, {
        method: "PUT",
        headers: { "content-type": "application/json" },
        body: JSON.stringify({ id, name, price, menuCategoryIds }),
      });
      const { menu, menuCategoryMenus } = await response.json();
      thunkApi.dispatch(replaceMenu(menu));
      thunkApi.dispatch(replaceMenuCategoryMenu(menuCategoryMenus));
      onSuccess && onSuccess();
    } catch (err) {
      onError && onError();
    }
  }
);

// reducers {.... } --> replace menu
    replaceMenu: (state, action) => {
      state.items = state.items.map((item) =>
        item.id === action.payload.id ? action.payload : item
      );
    },

```

- /api/menu route ကို PUT method နဲ့ request လုပ်လိုက်ပြီး payload အဖြစ် ၀င်လာမယ့် data ကိုပါ ထည့်ပေးလိုက်ပါတယ်
- server ကနေ response ပြန်လာမယ့် menu row ကို replaceMenu action ကို dispatchလုပ်ပြီး parameter အဖြစ် ထည့်ပေးလိုက်သလို menuCategoryMenus rowကို လည်း replaceMenuCategoryMenu action ကို dispatch လုပ်ပြီး ထည့်ပေးလိုက်ပါတယ်
- replaceMenu action မှာတော့ ရှိပြီးသား menu item တွေကို map လုပ်ပြီး ပြင်လိုက်တဲ့ menu id နဲ့ တူတဲ့ item ရောက်တဲ့အချိန် server က response ပြန်လာတဲ့menu row နဲ့ အစားထိုးပေးလိုက်တာပဲဖြစ်ပါတယ်

```js
// src/store/slices/menuCategoryMenuSlice.ts -->replaceMenuCategoryMenu action

   replaceMenuCategoryMenu: (state, action) => {
      const menuId = action.payload[0].menuId; // 5
      const otherMenuCategoryMenu = state.items.filter(
        (item) => item.menuId !== menuId
      );
      state.items = [...otherMenuCategoryMenu, ...action.payload];
    },


```

- replaceMenuCategoryMenu action ထဲမှာတော့ payload အဖြစ်၀င်လာတဲ့ array ထဲက menu id ကို အရင်ထုတ်ယူလိုက်ပြီး
- အဲ့ဒီ menu id နဲ့ မတူတဲ့ item တွေကို filter လုပ်ကာ otherMenuCategoryMenu အဖြစ် သိမ်းလိုက်ပါတယ်
- state ထဲက item array ကို otherMenuCategoryMenu ရော payload array ပါ ပြန်ထည့်ပေးလိုက်ပါတယ်

##

### menuSlice မှာ PUT method နဲ့ request လုပ်ထားတာကို server မှာလက်ခံလိုက်ပါမယ်

```js
// src/pages/api/menu/index.tsx --> PUT method

 else if (method === "PUT") {
    //get data from request
    const { id, name, price, menuCategoryIds } = req.body;

    //data validation
    const isValid =
      id && name && price !== undefined && menuCategoryIds.length > 0;
    if (!isValid) return res.status(400).send("Bad request.");

    //update menu
    const menu = await prisma.menu.update({
      data: { name, price },
      where: { id },
    });



    // create updated data
    const menuCategoryMenusData: { menuId: number; menuCategoryId: number }[] =
      menuCategoryIds.map((item: number) => ({
        menuId: id,
        menuCategoryId: item,
      }));

    // delete old  menuCategoryMenu rows
    await prisma.menuCategoryMenu.deleteMany({ where: { menuId: id } });

    //update menuCategoryMenu with updated data
    const menuCategoryMenus = await prisma.$transaction(
      menuCategoryMenusData.map((item) =>
        prisma.menuCategoryMenu.create({
          data: { menuId: item.menuId, menuCategoryId: item.menuCategoryId },
        })
      )
    );

    //return updated menu and menuCategoryMenus
    return res.status(200).json({ menu, menuCategoryMenus });
  }


```

> ရှင်းလင်းချက်

```js
//get data from request
const { id, name, price, menuCategoryIds } = req.body;

//data validation
const isValid = id && name && price !== undefined && menuCategoryIds.length > 0;
if (!isValid) return res.status(400).send("Bad request.");
```

- လိုအပ်တဲ့dataတွေ ထုတ်ယူလိုက်ပြီး valid ဖြစ်မဖြစ် စစ်ထားပါတယ်

```js
//update menu
const menu = await prisma.menu.update({
  data: { name, price },
  where: { id },
});
```

- menu table ထဲမှာ update လုပ်မယ့် menu id ကို သုံးပြီး ၀င်လာတဲ့ data တွေနဲ့ update လုပ်ပေးလိုက်ပါတယ်

```js
// create updated data
const menuCategoryMenusData: { menuId: number, menuCategoryId: number }[] =
  menuCategoryIds.map((item: number) => ({
    menuId: id,
    menuCategoryId: item,
  }));
```

- request နဲ့ ပါလာတဲ့ id နဲ့ mecategoryIds array ကို သုံးပြီး {menuId,menuCategoryId} တွေပါတဲ့ menuCategoryMenusData array တစ်ခု ဖန်တီးလိုက်ပါတယ်

```js
// delete old  menuCategoryMenu rows
await prisma.menuCategoryMenu.deleteMany({ where: { menuId: id } });
```

- menuCategoryMenu join table မှာ update မလုပ်ခင် အရင်ဆုံး ရှိနေတဲ့ rowsတွေထဲကမှ လက်ရှိ update လုပ်မယ့် menu id ပါတဲ့ rowတွေကို အရင်ဖျက်လိုက်ပါတယ်

```js
//update menuCategoryMenu with updated data
const menuCategoryMenus = await prisma.$transaction(
  menuCategoryMenusData.map((item) =>
    prisma.menuCategoryMenu.create({
      data: { menuId: item.menuId, menuCategoryId: item.menuCategoryId },
    })
  )
);
```

- menuCategoryMenusData ကိုသုံးပြီး menuCategoryMenu table မှာ row တွေ create လုပ်လိုက်ပါတယ်

```js
//return updated menu and menuCategoryMenus
return res.status(200).json({ menu, menuCategoryMenus });
```

- နောက်ဆုံးမှာတော့ update လုပ်ထားတဲ့ menu နဲ့ create လုပ်ထားတဲ့ menuCategoryMenus row တွေကို reponse ပြန်ပေးလိုက်တာပဲဖြစ်ပါတယ်

##

### Delete Menu

- menu တစ်ခုကို delete လုပ်နိုင်ဖို့အတွက် /menus/[id]/index.ts မှာ delete button နဲ့ dialog box တစ်ခုထည့်ပေးလိုက်ပါမယ်
  ![](https://cdn.discordapp.com/attachments/1153197808900395062/1166424905798139904/image.png?ex=654a70d8&is=6537fbd8&hm=b062ed97d6a3a31121a715448f782fb8234510bcba1905a2671672ef2fa964b5&)

```js
// src/pages/backoffice/menus/[id]/index.ts

import { useAppDispatch, useAppSelector } from "@/store/hooks";
import { deleteMenu, updateMenu } from "@/store/slices/menuSlice";
import { UpdateMenuOptions } from "@/types/menu";
import {
  Box,
  Button,
  Checkbox,
  Chip,
  Dialog,
  DialogActions,
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
import { useRouter } from "next/router";
import { useState } from "react";

const MenuDetail = () => {
  const router = useRouter();
  const menuId = Number(router.query.id);
  const menus = useAppSelector((state) => state.menu.items);
  const menuCategories = useAppSelector((state) => state.menuCategory.items);
  const menuCategoryMenus = useAppSelector(
    (state) => state.menuCategoryMenu.items
  );
  const menu = menus.find((item) => item.id === menuId);
  const menuCategoryMenu = menuCategoryMenus.filter(
    (item) => item.menuId === menuId
  );
  const menuCategoryIds = menuCategoryMenu.map((item) => item.menuCategoryId);
  const [data, setData] = useState<UpdateMenuOptions>();
  const dispatch = useAppDispatch();
  const [open, setOpen] = useState(false);

  if (!menu || !data) return null;

  const handleOnChange = (evt: SelectChangeEvent<number[]>) => {
    const selectedIds = evt.target.value as number[];
    setData({ ...data, id: menuId, menuCategoryIds: selectedIds });
  };

  const handleUpdateMenu = () => {
    dispatch(updateMenu(data));
  };

  const handleDeleteMenu = () => {
    dispatch(
      deleteMenu({
        id: menuId,
        onSuccess: () => router.push("/backoffice/menus"),
      })
    );
  };

  return (
    <Box sx={{ display: "flex", flexDirection: "column" }}>
      <Box sx={{ display: "flex", justifyContent: "flex-end", mb: 2 }}>
        <Button variant="outlined" color="error" onClick={() => setOpen(true)}>
          Delete
        </Button>
      </Box>
      <TextField
        defaultValue={menu.name}
        sx={{ mb: 2 }}
        onChange={(evt) =>
          setData({ ...data, id: menuId, name: evt.target.value })
        }
      />
      <TextField
        defaultValue={menu.price}
        sx={{ mb: 2 }}
        onChange={(evt) =>
          setData({ ...data, id: menuId, price: Number(evt.target.value) })
        }
      />
      <FormControl fullWidth>
        <InputLabel>Menu Category</InputLabel>
        <Select
          multiple
          value={data.menuCategoryIds}
          label="Menu Category"
          onChange={handleOnChange}
          renderValue={(selectedMenuCategoryIds) => {
            return selectedMenuCategoryIds
              .map((selectedMenuCategoryId) => {
                return menuCategories.find(
                  (item) => item.id === selectedMenuCategoryId
                ) as MenuCategory;
              })
              .map((item) => <Chip label={item.name} sx={{ mr: 1 }} />);
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
              <Checkbox checked={data.menuCategoryIds.includes(item.id)} />
              <ListItemText primary={item.name} />
            </MenuItem>
          ))}
        </Select>
      </FormControl>
      <Button
        variant="contained"
        sx={{ mt: 2, width: "fit-content" }}
        onClick={handleUpdateMenu}
      >
        Update
      </Button>
      <Dialog open={open} onClose={() => setOpen(false)}>
        <DialogTitle>Confirm delete menu</DialogTitle>
        <DialogContent>
          Are you sure you want to delete this menu?
        </DialogContent>
        <DialogActions>
          <Button onClick={() => setOpen(false)}>Cancel</Button>
          <Button variant="contained" onClick={handleDeleteMenu}>
            Confirm
          </Button>
        </DialogActions>
      </Dialog>
    </Box>
  );
};

export default MenuDetail;

```

- delete button ကို နှိပ်လိုက်ရင် dialog box လေး ပြပေးမှာဖြစ်ပြီး
- dialog box ထဲမှာတော့ cancel နဲ့ confirm ခလုတ်နှစ်ခု ထည့်ပေးထားပါတယ်
- cancel ဆိုရင် dialog ကိုပဲ ပြန်ပိတ်ပေးလိုက်ပြီး
- confirm လုပ်ခဲ့ရင်တော့ deleteMenu action ကို dispatch လုပ်လိုက်မှာဖြစ်ပါတယ်
  ##

### menuSlice ထဲမှာ deleteMenu action ကို သတ်မှတ်လိုက်ပါမယ်

```js

// src/store/slices/menuSLice.ts

//deleteMenu (thunk action)
export const deleteMenu = createAsyncThunk(
  "menu/deleteMenu",
  async (options: DeleteMenuOptions, thunkApi) => {
    const { id, onSuccess, onError } = options;
    try {
      await fetch(`${config.apiBaseUrl}/menu?id=${id}`, {
        method: "DELETE",
      });
      thunkApi.dispatch(removeMenu({ id }));
      onSuccess && onSuccess();
    } catch (err) {
      onError && onError();
    }
  }
);

//removeMenu(reducers{....})
    removeMenu: (state, action) => {
      state.items = state.items.filter((item) => item.id !== action.payload.id);
    },
```

- deleteMenu (thunk action) မှာ /api/menu route ဆီ DELETE method နဲ့ request လုပ်ထားပြီး payload အနေနဲ့ ၀င်လာတဲ့ menu id ကို query string အနေနဲ့ ထည့်ပေးထားလိုက်ပါတယ်
- ပြီးရင် removeMenu action dispatch လုပ်လိုက်ပြီး payload အနေနဲ့ ၀င်လာတဲ့ menu id ကိုပဲ parameter အဖြစ် ထည့်ပေးလိုက်ပါတယ်
- removeMenu action မှာတော့ ရလာတဲ့ menu id ရှိတဲ့ item ကို ဖယ်ထုတ်လိုက်တာပဲဖြစ်ပါတယ်

##

### /api/menu route ဆီ DELETE method နဲ့ ၀င်လာတဲ့ request ကို api မှာ လက်ခံလိုက်ပါမယ်

```js

// src/pages/api/menu/index.ts --> DELETE method

else if (method === "DELETE") {
    const menuId = Number(req.query.id);
    const menu = await prisma.menu.findFirst({ where: { id: menuId } });
    if (!menu) return res.status(400).send("Bad request.");
    await prisma.menu.update({
      data: { isArchived: true },
      where: { id: menuId },
    });
    return res.status(200).send("Deleted.");
  }

```

- query အနေနဲ့ ၀င်လာတဲ့ menu id ကိုသုံးပြီး menu table ထဲမှာ အဲဒီ id ရှိတဲ့ row ထဲးက isArchived ရဲ့ တန်ဖိုးကို true အဖြစ် update လုပ်ပေးလိုက်တာပဲဖြစ်ပါတယ်

## /api/app route မှာ menu data တွေ ယူတဲ့အခါမှာလည်း isArchived false ဖြစ်တဲ့ menu တွေကိုပဲ လှမ်းယူလိုက်ပါမယ်

![](https://cdn.discordapp.com/attachments/1153197808900395062/1166430016616284231/image.png?ex=654a759b&is=6538009b&hm=4ea3ae183c2dbc8f76231944299396c65dc0bc3f5cf22e8fc8611c9bf640ef87&)

##

### Menu CURD ပြီးပါပြီး
