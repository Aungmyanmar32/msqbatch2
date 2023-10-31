## MSquare Programming Fullstack Course

### Batch 2

### Episode-_35_ Summary

### 1. Menu-category CURD

### 2. Modify the deletion logic based on the scenario

### 3. Deleting Menus (example )

### 1. Menu-category CURD

- ပြီးခဲ့တဲ့သင်ခန်းစာမှာတော့ menu category တွေကို item card နဲ့ပြပေးခဲ့ကြပါတယ်
- ဆက်ပြီးတော့ create , upadte and deleteလုပ်လို့ရအောင် လုပ်ပါမယ်

```js
// src/components/NewMenuCategory.tsx

import { useAppDispatch } from "@/store/hooks";
import { createMenuCategory } from "@/store/slices/menuCategorySlice";
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
    dispatch(
      createMenuCategory({
        name,
        locationId: Number(selectedLocationId),
        onSuccess,
      })
    );
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

##

### Update and delete Menu-category

- Menu-category item တစ်ခုခုကို click လိုက်ရင် Dynamic route နဲ့ Menu-category detail page ကို ပို့လိုက်ပြီး update and delete လုပ်လို့ရအောင် လုပ်လိုက်ပါမယ်

```js
// src/pages/backoffice/menu-categories/[id]/index.tsx

import { useAppDispatch, useAppSelector } from "@/store/hooks";
import {
  deleteMenuCategory,
  updateMenuCategory,
} from "@/store/slices/menuCategorySlice";
import { UpdateMenuCategoryOptions } from "@/types/menuCategory";
import {
  Box,
  Button,
  Dialog,
  DialogActions,
  DialogContent,
  DialogTitle,
  TextField,
} from "@mui/material";
import { useRouter } from "next/router";
import { useEffect, useState } from "react";

const MenuCategoryDetail = () => {
  const router = useRouter();
  const menuCategoryId = Number(router.query.id);
  const menuCategories = useAppSelector((state) => state.menuCategory.items);
  const menuCategory = menuCategories.find(
    (item) => item.id === menuCategoryId
  );
  const [data, setData] = useState<UpdateMenuCategoryOptions>();
  const dispatch = useAppDispatch();
  const [open, setOpen] = useState(false);

  useEffect(() => {
    if (menuCategory) {
      setData({ ...menuCategory });
    }
  }, [menuCategory]);

  if (!menuCategory || !data) return null;

  const handleUpdateMenuCategory = () => {
    dispatch(updateMenuCategory(data));
  };

  const handleDeleteMenuCategory = () => {
    dispatch(
      deleteMenuCategory({
        id: menuCategoryId,
        onSuccess: () => router.push("/backoffice/menu-categories"),
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
        defaultValue={menuCategory.name}
        sx={{ mb: 2 }}
        onChange={(evt) =>
          setData({ ...data, id: menuCategoryId, name: evt.target.value })
        }
      />
      <Button
        variant="contained"
        sx={{ mt: 2, width: "fit-content" }}
        onClick={handleUpdateMenuCategory}
      >
        Update
      </Button>
      <Dialog open={open} onClose={() => setOpen(false)}>
        <DialogTitle>Confirm delete menu category</DialogTitle>
        <DialogContent>
          Are you sure you want to delete this menu category?
        </DialogContent>
        <DialogActions>
          <Button onClick={() => setOpen(false)}>Cancel</Button>
          <Button variant="contained" onClick={handleDeleteMenuCategory}>
            Confirm
          </Button>
        </DialogActions>
      </Dialog>
    </Box>
  );
};

export default MenuCategoryDetail;
```

- ပြင်ချင်တဲ့ MenuCategory id ပေါ်မူတည်ပြီး nameကို update လုပ်လို့ရအောင် လုပ်လိုက်ပြီး update ဖြစ်တဲ့ state ကို updateMenuCategory action ဆီ dispatch လုပ်လိုက်တာပဲဖြစ်ပါတယ်
- အလားတူပဲ delete ကို နှိပ်လိုက်ရင် dialog box နဲ့ delete ကို confirm လုပ်ခိုင်းလိုက်ပြီး deleteMenuCategory action ဆီ dispatch လုပ်လိုက်တာပဲဖြစ်ပါတယ်

##

### Prepairing in MenuCategory slice

- MenuCategory slice မှာလည်း action တွေ သတ်မှတ်ပြီး server request လုပ်ကာ store ကိုလည်း update လုပ်ပေးလိုက်မှာ ဖြစ်ပါတယ်

```js
// src/store/slices/menuCategorySlice.ts

import {
  CreateMenuCategoryOptions,
  DeleteMenuCategoryOptions,
  MenuCategorySlice,
  UpdateMenuCategoryOptions,
} from "@/types/menuCategory";
import { config } from "@/utils/config";
import { MenuCategory } from "@prisma/client";
import { PayloadAction, createAsyncThunk, createSlice } from "@reduxjs/toolkit";
import { removeMenuCategoryMenu } from "./menuCategoryMenuSlice";

const initialState: MenuCategorySlice = {
  items: [],
  isLoading: false,
  error: null,
};

export const createMenuCategory = createAsyncThunk(
  "menuCategory/createMenuCategory",
  async (options: CreateMenuCategoryOptions, thunkApi) => {
    const { name, locationId, onSuccess, onError } = options;
    try {
      const response = await fetch(`${config.apiBaseUrl}/menu-categories`, {
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

export const updateMenuCategory = createAsyncThunk(
  "menuCategory/updateMenuCategory",
  async (options: UpdateMenuCategoryOptions, thunkApi) => {
    const { id, name, onSuccess, onError } = options;
    try {
      const response = await fetch(`${config.apiBaseUrl}/menu-categories`, {
        method: "PUT",
        headers: { "content-type": "application/json" },
        body: JSON.stringify({ id, name }),
      });
      const { menuCategory } = await response.json();
      thunkApi.dispatch(replaceMenuCategory(menuCategory));
      onSuccess && onSuccess();
    } catch (err) {
      onError && onError();
    }
  }
);

export const deleteMenuCategory = createAsyncThunk(
  "menuCategory/deleteMenuCategory",
  async (options: DeleteMenuCategoryOptions, thunkApi) => {
    const { id, onSuccess, onError } = options;
    try {
      await fetch(`${config.apiBaseUrl}/menu-categories?id=${id}`, {
        method: "DELETE",
      });
      thunkApi.dispatch(removeMenuCategory({ id }));
      thunkApi.dispatch(removeMenuCategoryMenu({ menuCategoryId: id }));
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
    addMenuCategory: (state, action) => {
      state.items = [...state.items, action.payload];
    },
    replaceMenuCategory: (state, action: PayloadAction<MenuCategory>) => {
      state.items = state.items.map((item) =>
        item.id === action.payload.id ? action.payload : item
      );
    },
    removeMenuCategory: (state, action: PayloadAction<{ id: number }>) => {
      state.items = state.items.filter((item) => item.id !== action.payload.id);
    },
  },
});

export const {
  setMenuCategories,
  addMenuCategory,
  replaceMenuCategory,
  removeMenuCategory,
} = menuCategorySlice.actions;
export default menuCategorySlice.reducer;
```

##

### api မှာလည်း request တွေကို လက်ခံပြီး database တွေကို update လုပ်ကာ response ပြန်ပေးလိုက်မှာဖြစ်ပါတယ်

```js
// src/pages/api/menu-categories/index.ts

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
    // Data validation
    const { name, locationId } = req.body;
    const isValid = name && locationId;
    if (!isValid) return res.status(400).send("Bad request.");
    const location = await prisma.location.findFirst({
      where: { id: locationId },
    });
    if (!location) return res.status(400).send("Bad request.");
    const menuCategory = await prisma.menuCategory.create({
      data: { name, companyId: location.companyId },
    });
    return res.status(200).json(menuCategory);
  } else if (method === "PUT") {
    const { id, name } = req.body;
    const isValid = id && name;
    if (!isValid) return res.status(400).send("Bad request.");
    const exist = await prisma.menuCategory.findFirst({
      where: { id },
    });
    if (!exist) return res.status(400).send("Bad request.");
    const menuCategory = await prisma.menuCategory.update({
      data: { name },
      where: { id },
    });
    return res.status(200).json({ menuCategory });
  } else if (method === "DELETE") {
    const menuCategoryId = Number(req.query.id);
    const menuCategory = await prisma.menuCategory.findFirst({
      where: { id: menuCategoryId },
    });
    if (!menuCategory) return res.status(400).send("Bad request.");
    /*
    const menuCategoryMenus = await prisma.menuCategoryMenu.findMany({
      where: { menuCategoryId },
    });
    const menuIds = menuCategoryMenus.map((item) => item.menuId);
    menuIds.forEach(async (menuId: number) => {
      const menuCategoryMenus = await prisma.menuCategoryMenu.findMany({
        where: { menuId, isArchived: false },
      });
      if (menuCategoryMenus.length === 1) {
        // one menu is connected to only one menu category
        await prisma.menuCategoryMenu.updateMany({
          data: { isArchived: true },
          where: { menuCategoryId, menuId },
        });
        // menuAddonCategory
        await prisma.menuAddonCategory.updateMany({
          data: { isArchived: true },
          where: {
            menuId,
          },
        });
      } else {
        // one menu is connected to many menu cateogories
        await prisma.menuCategoryMenu.updateMany({
          data: { isArchived: true },
          where: { menuCategoryId },
        });
      }
    });*/
    await prisma.menuCategory.update({
      data: { isArchived: true },
      where: { id: menuCategoryId },
    });
    return res.status(200).send("Deleted.");
  }
  res.status(405).send("Method now allowed.");
}
```

##

### Delete Menu (modify code)

- အရင် သင်ခန်းစာတွေမှာ menu တစ်ခု delete လုပ်တဲ့အခါ menu table ထဲမှာပဲ isArchived true လုပ်ခဲ့ကြပါတယ်
- menu ကို delete လုပ်လိုက်ချိန်မှာ menuAddonCategory table ထဲမှာ ရှိတဲ့ join ထားတဲ့ rows တွေကို isArchived true လုပ်ပေးရမှာဖြစ်ပါတယ်

```js
// src/pages/api/menus/index.ts --> DELETE method

 else if (method === "DELETE") {
    const menuId = Number(req.query.id);
    const menu = await prisma.menu.findFirst({ where: { id: menuId } });
    if (!menu) return res.status(400).send("Bad request.");
    await prisma.menuAddonCategory.updateMany({
      data: { isArchived: true },
      where: { menuId },
    });
    const menuAddonCategoriesRow = await prisma.menuAddonCategory.findMany({
      where: { menuId },
    });
    const addonCategoryIds = menuAddonCategoriesRow.map(
      (item) => item.addonCategoryId
    );

    await prisma.menu.update({
      data: { isArchived: true },
      where: { id: menuId },
    });
    return res.status(200).send("Deleted.");
  }
```

### Menu တစ်ခုကို delete လုပ်တဲ့အချိန်မှာ အဲ့ဒီ menu နဲ့ ချိတ်ထားတဲ့ addon-category တွေကိုပါ isArchived လုပ်ပေးရမှာဖြစ်ပါတယ်

- ဒီနေရာမှာ အခြေအနေ နှစ်ခု ရှိနိုင်ပါတယ်

### အခြေအနေ တစ်

- အဲ့ဒီ menu နဲ့ ချိတ်ထားတဲ့ addon-category ဟာ တစ်ခြား ဘယ်menu တွေနဲ့မှ join ထားခြင်း မရှိဘူးဆိုရင် အဲ့ဒီ menu ရော ချိတ်ထားတဲ့ addon-category ကိုပါ isArchived true လုပ်ပေးလိုက်မှာဖြစ်ပါတယ်

### အခြေအနေ နှစ်

- အဲ့ဒီ menu နဲ့ ချိတ်ထားတဲ့ addon-category ဟာ တစ်ခြား menu တွေနဲ့ join ထားတာရှိခဲ့ရင် အဲ့ဒီ menu ကိုပဲ isArchived true လုပ်ပေးလိုက်မှာဖြစ်ပြီး ချိတ်ထားတဲ့ addon-category ကိုတော့ ဘာမှ မလုပ်ပဲ ထားပေးရမှာ ဖြစ်ပါတယ်

### menu delete လုပ်တဲ့ action ကို dispatch လုပ်ပြီးတဲ့အချိန်မှာ စစ်ပေးလိုက်ရမှာ ဖြစ်ပြီး ရလာတဲ့ အခြေအနေ ပေါ်မူတည်ကာ addon-category ကို ကိုင်တွယ်ပေးရမှာဖြစ်ပါတယ်

```js
// src/pages/backoffice/menus/[id]/index.tsx--> handleDelete function

const handleDeleteMenu = () => {
  dispatch(
    deleteMenu({
      id: menuId,
      onSuccess: () => {
        menuAddonCategories
          .filter((item) => item.menuId === menuId)
          .map((item) => item.addonCategoryId)
          .forEach((addonCategoryId) => {
            const entries = menuAddonCategories.filter(
              (item) => item.addonCategoryId === addonCategoryId
            );
            if (entries.length === 1) {
              const menuAddonCategoryId = entries[0].id;
              dispatch(removeAddonCategory({ id: addonCategoryId }));
              dispatch(
                removeMenuAddonCategoryById({ id: menuAddonCategoryId })
              );
            }
          });
      },
    })
  );
};
```

- menu တစ်ခု delete လုပ်လိုက်တိုင်း အဲ့ဒီmenuနဲ့ ချိတ်ထားတဲ့ addon-category တွေကို တခြား menu နဲ့ ချိတ်ထားတာမရှိခဲ့ရင် addon-category ကိုပါ isArchived true လုပ်ပေးလိုက်တာပဲဖြစ်ပါတယ်
