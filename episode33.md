## MSquare Programming Fullstack Course

### Batch 2

### Episode-_33_ Summary

### Addon-category CURD

### 1. Show all addon category with ItemCard(Read)

### 2. Create new Addon-category

### 3. Update Addon-category

### 4. Delete Addon-category

##

### 1. Show all addon category with ItemCard(Read)

- menu page မှာ လုပ်ခဲ့သလိုပဲ addon-category တွေကိုလည်း ItemCard နဲ့ addon-category page မှာ ပြပေးလိုက်ပါမယ်

```js
// src/pages/backoffice/addon-categories/index.tsx

import ItemCard from "@/components/ItemCard";
import NewAddonCategory from "@/components/NewAddonCategory";
import { useAppSelector } from "@/store/hooks";
import ClassIcon from "@mui/icons-material/Class";
import { Box, Button } from "@mui/material";
import { useState } from "react";

const AddonCategoriesPage = () => {
  const [open, setOpen] = useState(false);
  const addonCategories = useAppSelector((state) => state.addonCategory.items);
  return (
    <Box>
      <Box sx={{ display: "flex", justifyContent: "flex-end" }}>
        <Button variant="contained" onClick={() => setOpen(true)}>
          New addon category
        </Button>
      </Box>
      <Box sx={{ display: "flex", flexWrap: "wrap" }}>
        {addonCategories.map((item) => (
          <ItemCard
            icon={<ClassIcon />}
            key={item.id}
            title={item.name}
            href={`/backoffice/addon-categories/${item.id}`}
          />
        ))}
      </Box>
      <NewAddonCategory open={open} setOpen={setOpen} />
    </Box>
  );
};

export default AddonCategoriesPage;
```

- addon-category တွေကို item card နဲ့ ပြပေးထားလိုက်ပြီး itemcard ကိုနှိပ်လိုက်ရင် အဲ့ဒီ card ရဲ့ id နဲ့ dynamic page ကို သွားလိုက်မှာဖြစ်ပါတယ်

##

### 2. Create new Addon-category

- addon category တစ်ခု create လုပ်နိုင်ဖို့ newAddonCategory componet မှာ လိုအပ်တဲ့ data တွေကို ထည့်ပေးလို့ရအောင် လုပ်လိုက်ပါမယ်
- addon category တစ်ခု create လုပ်မယ်ဆိုရင်

  - category အမည် (name)
  - required ဖြစ်မဖြစ် (isRequired)
  - category နဲ့ ချိတ်မယ့် menu တွေ လိုအပ်မှာဖြစ်ပါတယ်

```js
// src/components/NewAddonCategory

import { useAppDispatch, useAppSelector } from "@/store/hooks";
import { createAddonCategory } from "@/store/slices/addonCategorySlice";
import { setOpenSnackbar } from "@/store/slices/snackbarSlice";
import { CreateAddonCategoryOptions } from "@/types/addonCategory";
import {
  Box,
  Button,
  Checkbox,
  Chip,
  Dialog,
  DialogContent,
  DialogTitle,
  FormControl,
  FormControlLabel,
  InputLabel,
  ListItemText,
  MenuItem,
  Select,
  SelectChangeEvent,
  TextField,
} from "@mui/material";
import { Menu } from "@prisma/client";
import { Dispatch, SetStateAction, useState } from "react";

interface Props {
  open: boolean;
  setOpen: Dispatch<SetStateAction<boolean>>;
}

const defaultNewAddonCategory = {
  name: "",
  isRequired: true,
  menuIds: [],
};

const NewAddonCategory = ({ open, setOpen }: Props) => {
  const [newAddonCategory, setNewAddonCategory] =
    useState<CreateAddonCategoryOptions>(defaultNewAddonCategory);
  const menus = useAppSelector((state) => state.menu.items);
  const dispatch = useAppDispatch();

  const handleOnChange = (evt: SelectChangeEvent<number[]>) => {
    const selectedIds = evt.target.value as number[];
    setNewAddonCategory({ ...newAddonCategory, menuIds: selectedIds });
  };

  const handleCreateAddonCategory = () => {
    const isValid =
      newAddonCategory.name && newAddonCategory.menuIds.length > 0;
    if (!isValid) {
      return dispatch(
        setOpenSnackbar({
          message: "Missing required fields",
          severity: "error",
        })
      );
    }
    dispatch(
      createAddonCategory({
        ...newAddonCategory,
        onSuccess: () => {
          setOpen(false);
          dispatch(
            setOpenSnackbar({
              message: "New addon category created succcessfully.",
            })
          );
        },
      })
    );
  };

  return (
    <Dialog
      open={open}
      onClose={() => {
        setNewAddonCategory(defaultNewAddonCategory);
        setOpen(false);
      }}
    >
      <DialogTitle>Create new addon category</DialogTitle>
      <DialogContent sx={{ width: 300 }}>
        <TextField
          onChange={(evt) =>
            setNewAddonCategory({ ...newAddonCategory, name: evt.target.value })
          }
          sx={{ mb: 2, width: "100%" }}
        />
        <FormControl fullWidth>
          <InputLabel>Menus</InputLabel>
          <Select
            multiple
            value={newAddonCategory.menuIds}
            label="Menus"
            onChange={handleOnChange}
            renderValue={(selectedMenuIds) => {
              return selectedMenuIds
                .map((selectedMenuId) => {
                  return menus.find(
                    (item) => item.id === selectedMenuId
                  ) as Menu;
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
            {menus.map((item) => (
              <MenuItem key={item.id} value={item.id}>
                <Checkbox
                  checked={newAddonCategory.menuIds.includes(item.id)}
                />
                <ListItemText primary={item.name} />
              </MenuItem>
            ))}
          </Select>
        </FormControl>
        <FormControlLabel
          control={
            <Checkbox
              defaultChecked={newAddonCategory.isRequired}
              onChange={(evt, value) =>
                setNewAddonCategory({
                  ...newAddonCategory,
                  isRequired: value,
                })
              }
            />
          }
          label="Required"
          sx={{ mt: 1 }}
        />
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
            disabled={
              !newAddonCategory.name || !newAddonCategory.menuIds.length
            }
            onClick={handleCreateAddonCategory}
          >
            Confirm
          </Button>
        </Box>
      </DialogContent>
    </Dialog>
  );
};

export default NewAddonCategory;
```

- လိုအပ်မယ့် data တွေကို သိမ်းမယ့် state တစ်ခုလုပ်လိုက်ပြီး return ထဲမှာ state ကို update လုပ်လို့ရအောင် textField တွေနဲ့ select component တွေကို menu create လုပ်ခဲ့တုန်းကလိုပဲ ထည့်ပေးထားလိုက်ပါတယ်
- create ကို နှိပ်လိုက်ချိန်မှာတော့ createNewAddonCategory action ကို dispatch လုပ်လိုက်ပါတယ်
- dispatch လုပ်မယ့် action ကို လည်း addonCategorySlice မှာ သတ်မှတ်ပေးလိုက်ပါမယ်

```js
// src/store/slices/addonCategorySlice.ts

import {
  AddonCategorySlice,
  CreateAddonCategoryOptions,
  DeleteAddonCategoryOptions,
  UpdateAddonCategoryOptions,
} from "@/types/addonCategory";
import { config } from "@/utils/config";
import { createAsyncThunk, createSlice } from "@reduxjs/toolkit";
import { addMenuAddonCategories } from "./menuAddonCategorySlice";

const initialState: AddonCategorySlice = {
  items: [],
  isLoading: false,
  error: null,
};

export const createAddonCategory = createAsyncThunk(
  "addonCategory/createAddonCategory",
  async (options: CreateAddonCategoryOptions, thunkApi) => {
    const { name, isRequired, menuIds, onSuccess, onError } = options;
    try {
      const response = await fetch(`${config.apiBaseUrl}/addon-categories`, {
        method: "POST",
        headers: { "content-type": "application/json" },
        body: JSON.stringify({ name, isRequired, menuIds }),
      });
      const { addonCategory, menuAddonCategories } = await response.json();
      thunkApi.dispatch(addAddonCategory(addonCategory));
      thunkApi.dispatch(addMenuAddonCategories(menuAddonCategories));
      onSuccess && onSuccess();
    } catch (err) {
      onError && onError();
    }
  }
);

const addonCategorySlice = createSlice({
  name: "addonCategory",
  initialState,
  reducers: {
    setAddonCategories: (state, action) => {
      state.items = action.payload;
    },

    addAddonCategory: (state, action) => {
      state.items = [...state.items, action.payload];
    },
  },
});

export const { setAddonCategories, addAddonCategory } =
  addonCategorySlice.actions;
export default addonCategorySlice.reducer;
```

- menuSlice မှာလုပ်ခဲ့တုန်းကလိုပဲ tunck action နဲ့ server ဆီ request လုပ်ပြီး ပြန်ရလာတဲ့ data ကို addNewAddonCategory action ရဲ့ payload အဖြစ် ထည့်ပြီး dispatch လုပ်လိုက်တာပဲဖြစ်ပါတယ်
- addon category အသစ် create လုပ်ရင် join table ဖြစ်တဲ့ menuAddonCategory table မှာပါ rows အသစ်တွေ crate လုပ်ပေးလိုက်မှာ ဖြစ်ပြီး
- response ပြန်လာတဲ့ rows တွေကို menuAddonCategorySlice လုပ်ပြီး store ထဲမှာ သိမ်းပေးလိုက်ပါမယ်

```js
// src/store/slices/menuAddonCategorySlice.ts

import { MenuAddonCategorySlice } from "@/types/menuAddonCategory";
import { createSlice } from "@reduxjs/toolkit";

const initialState: MenuAddonCategorySlice = {
  items: [],
  isLoading: false,
  error: null,
};

const menuAddonCategorySlice = createSlice({
  name: "menuAddonCategorySlice",
  initialState,
  reducers: {
    setMenuAddonCategories: (state, action) => {
      state.items = action.payload;
    },
    addMenuAddonCategories: (state, action) => {
      state.items = [...state.items, ...action.payload];
    },

});

export const {
  setMenuAddonCategories,
  addMenuAddonCategories,
} = menuAddonCategorySlice.actions;
export default menuAddonCategorySlice.reducer;

```

##

### Update and Delete addon category

- addon-category တစ်ခုချင်းစီအတွက် dynamic route သတ်မှတ်လိုက်ပြီး Update and Delete လုပ်လို့ရအောင် လုပ်ပါမယ်

```js
// src/pages/addon-categories/[id]/index.tsx

import { useAppDispatch, useAppSelector } from "@/store/hooks";
import {
  deleteAddonCategory,
  updateAddonCategory,
} from "@/store/slices/addonCategorySlice";
import { setOpenSnackbar } from "@/store/slices/snackbarSlice";
import { UpdateAddonCategoryOptions } from "@/types/addonCategory";
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
  FormControlLabel,
  InputLabel,
  ListItemText,
  MenuItem,
  Select,
  SelectChangeEvent,
  TextField,
} from "@mui/material";
import { Menu } from "@prisma/client";
import { useRouter } from "next/router";
import { useEffect, useState } from "react";

const AddonCategoryDetail = () => {
  const router = useRouter();

//get id
  const addonCategoryId = Number(router.query.id);

//get data from store
  const addonCategories = useAppSelector((state) => state.addonCategory.items);
  const menuAddonCategories = useAppSelector(
    (state) => state.menuAddonCategory.items
  );
  const menus = useAppSelector((state) => state.menu.items);

  //find current addon-category
  const addonCategory = addonCategories.find(
    (item) => item.id === addonCategoryId
  );

  //get joined menu ids
  const currentMenuAddonCategories = menuAddonCategories.filter(
    (item) => item.addonCategoryId === addonCategoryId
  );
  const menuIds = currentMenuAddonCategories.map((item) => item.menuId);

  //delete dialog state
  const [open, setOpen] = useState(false);

  //updated addon-category state
  const [data, setData] = useState<UpdateAddonCategoryOptions>();

  const dispatch = useAppDispatch();

//update data state with current addon category and joined menu ids
  useEffect(() => {
    if (addonCategory) {
      setData({ ...addonCategory, menuIds });
    }
  }, [addonCategory]);

  if (!addonCategory || !data) return null;



  const handleUpdateAddonCategory = () => {
    const isValid = data.name && data.menuIds.length > 0;
    if (!isValid) return;
    dispatch(
      updateAddonCategory({
        ...data,
      })
    );
  };

  const handleOnChange = (evt: SelectChangeEvent<number[]>) => {
    const selectedIds = evt.target.value as number[];
    setData({ ...data, id: addonCategoryId, menuIds: selectedIds });
  };

  const handleDeleteAddonCategory = () => {
    dispatch(
      deleteAddonCategory({
        id: addonCategoryId,
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
        defaultValue={addonCategory.name}
        sx={{ mb: 2 }}
        onChange={(evt) => setData({ ...data, name: evt.target.value })}
      />
      <FormControl fullWidth sx={{ my: 1 }}>
        <InputLabel>Menus</InputLabel>
        <Select
          multiple
          value={data.menuIds}
          label="Menus"
          onChange={handleOnChange}
          renderValue={(selectedMenuIds) => {
            return selectedMenuIds
              .map((selectedMenuId) => {
                return menus.find((item) => item.id === selectedMenuId) as Menu;
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
          {menus.map((item) => (
            <MenuItem key={item.id} value={item.id}>
              <Checkbox checked={data.menuIds.includes(item.id)} />
              <ListItemText primary={item.name} />
            </MenuItem>
          ))}
        </Select>
      </FormControl>

      <FormControlLabel
        control={
          <Checkbox
            defaultChecked={addonCategory.isRequired}
            onChange={(evt, value) => setData({ ...data, isRequired: value })}
          />
        }
        label="Required"
        sx={{ mb: 4 }}
      />

      <Button
        variant="contained"
        sx={{ width: "fit-content" }}
        onClick={handleUpdateAddonCategory}
      >
        Update
      </Button>
      <Dialog open={open} onClose={() => setOpen(false)}>
        <DialogTitle>Confirm delete addon category</DialogTitle>
        <DialogContent>
          Are you sure you want to delete this addon category?
        </DialogContent>
        <DialogActions>
          <Button onClick={() => setOpen(false)}>Cancel</Button>
          <Button variant="contained" onClick={handleDeleteAddonCategory}>
            Confirm
          </Button>
        </DialogActions>
      </Dialog>
    </Box>
  );
};

export default AddonCategoryDetail;
```

> ရှင်းလင်းချက်

```js
//get id
const addonCategoryId = Number(router.query.id);

//get data from store
const addonCategories = useAppSelector((state) => state.addonCategory.items);
const menuAddonCategories = useAppSelector(
  (state) => state.menuAddonCategory.items
);
const menus = useAppSelector((state) => state.menu.items);
```

- addon-category id နဲ့ storeထဲက လိုအပ်တဲ့ dataတွေကို လှမ်းယူလိုက်ပါတယ်

```js
//find current addon-category
const addonCategory = addonCategories.find(
  (item) => item.id === addonCategoryId
);

//get joined menu ids
const currentMenuAddonCategories = menuAddonCategories.filter(
  (item) => item.addonCategoryId === addonCategoryId
);
const menuIds = currentMenuAddonCategories.map((item) => item.menuId);
```

- addon-category id ကို သုံးပြီး update လုပ်မယ့် addon-category နဲ့ သူနဲ့ joinထားတဲ့ Menu ids တွေကို ရှာလိုက်ပါတယ်

```js
  //delete dialog state
  const [open, setOpen] = useState(false);

  //updated addon-category state
  const [data, setData] = useState<UpdateAddonCategoryOptions>();
```

- delete dialog box ကို ထိန်းချုပ်မယ့် open state နဲ့ update လုပ်မယ့် category အချက်အလက်တွေ သိမ်းထားမယ့် data state နှစ်ခုကို သတ်မှတ်လိုက်တာပါ

```js
  const handleUpdateAddonCategory = () => {
    const isValid = data.name && data.menuIds.length > 0;
    if (!isValid) return;
    dispatch(
      updateAddonCategory({
        ...data,
      })
    );
  };

  const handleOnChange = (evt: SelectChangeEvent<number[]>) => {
    const selectedIds = evt.target.value as number[];
    setData({ ...data, id: addonCategoryId, menuIds: selectedIds });
  };

  const handleDeleteAddonCategory = () => {
    dispatch(
      deleteAddonCategory({
        id: addonCategoryId,
      })
    );
  };
```

- **update comfirm** လုပ်လိုက်ရင် run ပေးမယ့် function ရယ် **catagory ကို ပြင်တဲ့အချိန်** သုံးမယ့် onChange function ရယ့် **delete confirm** လုပ်ရင် runမယ့် function ဆိုပြီး သတ်မှတ်ထားလိုက်တာဖြစ်ပါတယ်
- **update comfirm** လုပ်လိုက်ရင် run ပေးမယ့် function မှာတော့ လိုအပ်တဲ့dataတွေ validation လုပ်လိုက်ပြီး updateAddonCategory action dispatch လုပ်လိုက်ကာ data state ကို payload အဖြစ် ထည့်ပေးထားပါတယ်
- **catagory ကို ပြင်တဲ့အချိန်** သုံးမယ့် onChange function မှာတော့ data state ကို update လုပ်ထားပါတယ်
- **delete confirm** လုပ်ရင် runမယ့် function မှာတော့ deleteAddonCategory action dispatch လုပ်ထားပြီး addon-category id ကို payload အဖြစ် ထည့်ပေးလိုက်ပါတယ်

```js
<Box sx={{ display: "flex", justifyContent: "flex-end", mb: 2 }}>
  <Button variant="outlined" color="error" onClick={() => setOpen(true)}>
    Delete
  </Button>
</Box>
```

- delete button တစ်ခုကို ထည့်ပေးထားပြီး click လိုက်ရင် delete dialog box ကို ပြပေးမှာဖြစ်ပါတယ်

```js
<TextField
        defaultValue={addonCategory.name}
        sx={{ mb: 2 }}
        onChange={(evt) => setData({ ...data, name: evt.target.value })}
      />
      <FormControl fullWidth sx={{ my: 1 }}>
        <InputLabel>Menus</InputLabel>
        <Select
          multiple
          value={data.menuIds}
          label="Menus"
          onChange={handleOnChange}
          renderValue={(selectedMenuIds) => {
            return selectedMenuIds
              .map((selectedMenuId) => {
                return menus.find((item) => item.id === selectedMenuId) as Menu;
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
          {menus.map((item) => (
            <MenuItem key={item.id} value={item.id}>
              <Checkbox checked={data.menuIds.includes(item.id)} />
              <ListItemText primary={item.name} />
            </MenuItem>
          ))}
        </Select>
      </FormControl>

```

- name နဲ့ menu တွေ ပြင်ဖို့ textfield နဲ့ selector ထည့်ပေးထားပါတယ်

```js
<FormControlLabel
  control={
    <Checkbox
      defaultChecked={addonCategory.isRequired}
      onChange={(evt, value) => setData({ ...data, isRequired: value })}
    />
  }
  label="Required"
  sx={{ mb: 4 }}
/>
```

- required ဖြစ်မဖြစ်ကို update လုပ်နိုင်ဖို့ MUI checkbox (formControlLable)ကို သုံးလိုက်ပြီး
- addonCategory ထဲက isRequired ရဲ့ တန်ဖိုးကို default အနေနဲ့ ထည့်ပေးထားပါတယ်
- addonCategory ထဲက isRequired ရဲ့ တန်ဖိုး true ဖြစ်ရင် default အနေနဲ့ အမှန်ခြစ်ပေးထားမှာဖြစ်ပြီး FLASE ဖြစ်ရင်တော့ အမှန်ခြစ်ဖြုတ်ပေးထားမှာဖြစ်ပါတယ်
- checkbox မှာ အမှန်ခြစ်ကို ဖြုတ်လိုက်ရင် value က false / အမှန်ခြစ်ကိုပေးထားရင် value က true ကို ရလာမှာဖြစ်ပါတယ်
- ရလာတဲ့ valueကို data state ထဲက isRequired တန်ဖိုးအဖြစ် ပြန်သတ်မှတ်ပေးလိုက်တာပဲဖြစ်ပါတယ်

```js
<Button
  variant="contained"
  sx={{ width: "fit-content" }}
  onClick={handleUpdateAddonCategory}
>
  Update
</Button>
```

- update button ကို ထည့်ပေးထားပြီး handleUpdateAddonCategory ကို နှိပ်လိုက်ရင် run လိုက်မှာ ဖြစ်ပါတယ်

```js
<Dialog open={open} onClose={() => setOpen(false)}>
  <DialogTitle>Confirm delete addon category</DialogTitle>
  <DialogContent>
    Are you sure you want to delete this addon category?
  </DialogContent>
  <DialogActions>
    <Button onClick={() => setOpen(false)}>Cancel</Button>
    <Button variant="contained" onClick={handleDeleteAddonCategory}>
      Confirm
    </Button>
  </DialogActions>
</Dialog>
```

- delete dialog box ဖြစ်ပြီး confrim လုပ်လိုက်ရင် handleDeleteAddonCategory ကို ေခါ်ပေးမှာဖြစ်ပါတယ်

##

### addon-category slice မှာ action တွေကို သတ်မှတ်ပြီး server ဆီrequest လုပ်လိုက်ပါမယ်

```js
import {
  AddonCategorySlice,
  CreateAddonCategoryOptions,
  DeleteAddonCategoryOptions,
  UpdateAddonCategoryOptions,
} from "@/types/addonCategory";
import { config } from "@/utils/config";
import { createAsyncThunk, createSlice } from "@reduxjs/toolkit";
import {
  addMenuAddonCategories,
  replaceMenuAddonCategory,
} from "./menuAddonCategorySlice";

const initialState: AddonCategorySlice = {
  items: [],
  isLoading: false,
  error: null,
};

export const createAddonCategory = createAsyncThunk(
  "addonCategory/createAddonCategory",
  async (options: CreateAddonCategoryOptions, thunkApi) => {
    const { name, isRequired, menuIds, onSuccess, onError } = options;
    try {
      const response = await fetch(`${config.apiBaseUrl}/addon-categories`, {
        method: "POST",
        headers: { "content-type": "application/json" },
        body: JSON.stringify({ name, isRequired, menuIds }),
      });
      const { addonCategory, menuAddonCategories } = await response.json();
      thunkApi.dispatch(addAddonCategory(addonCategory));
      thunkApi.dispatch(addMenuAddonCategories(menuAddonCategories));
      onSuccess && onSuccess();
    } catch (err) {
      onError && onError();
    }
  }
);

export const deleteAddonCategory = createAsyncThunk(
  "addonCategory/deleteAddonCategory",
  async (options: DeleteAddonCategoryOptions, thunkApi) => {
    const { id, onSuccess, onError } = options;
    try {
      await fetch(`${config.apiBaseUrl}/addon-categories?id=${id}`, {
        method: "DELETE",
      });
      thunkApi.dispatch(removeAddonCategory({ id }));
      onSuccess && onSuccess();
    } catch (err) {
      onError && onError();
    }
  }
);

export const updateAddonCategory = createAsyncThunk(
  "addonCategory/updateAddonCategory",
  async (options: UpdateAddonCategoryOptions, thunkApi) => {
    const { id, name, isRequired, menuIds, onSuccess, onError } = options;
    try {
      const response = await fetch(`${config.apiBaseUrl}/addon-categories`, {
        method: "PUT",
        headers: { "content-type": "application/json" },
        body: JSON.stringify({ id, name, isRequired, menuIds }),
      });
      const { addonCategory, menuAddonCategories } = await response.json();
      thunkApi.dispatch(replaceAddonCategory(addonCategory));
      thunkApi.dispatch(replaceMenuAddonCategory(menuAddonCategories));
      onSuccess && onSuccess();
    } catch (err) {
      onError && onError();
    }
  }
);

const addonCategorySlice = createSlice({
  name: "addonCategory",
  initialState,
  reducers: {
    setAddonCategories: (state, action) => {
      state.items = action.payload;
    },
    replaceAddonCategory: (state, action) => {
      state.items = state.items.map((item) =>
        item.id === action.payload.id ? action.payload : item
      );
    },
    removeAddonCategory: (state, action) => {
      state.items = state.items.filter((item) => item.id !== action.payload.id);
    },
    addAddonCategory: (state, action) => {
      state.items = [...state.items, action.payload];
    },
  },
});

export const {
  setAddonCategories,
  replaceAddonCategory,
  removeAddonCategory,
  addAddonCategory,
} = addonCategorySlice.actions;
export default addonCategorySlice.reducer;
```

- deleteAddonCategory action ထဲမှာတော့ /api/addon-category route ဆီ delete method နဲ့ request လုပ်လိုက်ပြီး addon-category id ကို query နဲ့ ထည့်ပေးလိုက်ပါတယ်
-

```js
export const deleteAddonCategory = createAsyncThunk(
  "addonCategory/deleteAddonCategory",
  async (options: DeleteAddonCategoryOptions, thunkApi) => {
    const { id, onSuccess, onError } = options;
    try {
      await fetch(`${config.apiBaseUrl}/addon-categories?id=${id}`, {
        method: "DELETE",
      });
      thunkApi.dispatch(removeAddonCategory({ id }));
      onSuccess && onSuccess();
    } catch (err) {
      onError && onError();
    }
  }
);
```

- ပြီးရင် removeAddonCategory action ကို dispatchထပ်လုပ်ပြီး addon-category id ကိုပဲ payload အဖြစ် ထည့်ပေးလိုက်ပါတယ်

##

```js
export const updateAddonCategory = createAsyncThunk(
  "addonCategory/updateAddonCategory",
  async (options: UpdateAddonCategoryOptions, thunkApi) => {
    const { id, name, isRequired, menuIds, onSuccess, onError } = options;
    try {
      const response = await fetch(`${config.apiBaseUrl}/addon-categories`, {
        method: "PUT",
        headers: { "content-type": "application/json" },
        body: JSON.stringify({ id, name, isRequired, menuIds }),
      });
      const { addonCategory, menuAddonCategories } = await response.json();
      thunkApi.dispatch(replaceAddonCategory(addonCategory));
      thunkApi.dispatch(replaceMenuAddonCategory(menuAddonCategories));
      onSuccess && onSuccess();
    } catch (err) {
      onError && onError();
    }
  }
);
```

- updateAddonCategory action မှာတော့ /api/addon-categories route ဆီ PUT method နဲ့ request လုပ်ထားပြီး payload အနေနဲ့ ၀င်လာတဲ့ data ကို request body အဖြစ် ထည့်ပေးထားလိုက်ပါတယ်
- server ကနေပြန်လာတဲ့ response dataတွေကို replaceAddonCategory နဲ့ replaceMenuAddonCategory action တွေကို ခေါ်ပြီး payload အဖြစ် ပြန်ထည့်ပေးလိုက်ပါတယ်

```js
    replaceAddonCategory: (state, action) => {
      state.items = state.items.map((item) =>
        item.id === action.payload.id ? action.payload : item
      );
    },
```

- replace action တွေ ထဲမှာတော့ id တူတဲ့ item တွေ နေရာမှာ server က response လာတဲ့ ပြင်ထားပြီးသားrow နဲ့ replace လုပ်ထားလိုက်တာပဲ ဖြစ်ပါတယ်

##

### Setup api

- addonCategory slice ကနေ လုပ်ထားတဲ့ request တွေကို api မှာ လက်ခံပြီး database မှာ CURD လုပ်ကာ response ပြန်ပေးလိုက်မှာဖြစ်ပါတယ်

```js
// /src/pages/api/addon-categories/index.ts
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
    const { name, isRequired = true, menuIds } = req.body;
    const isValid = name && menuIds.length > 0;
    if (!isValid) return res.status(400).send("Bad request.");

    //create new addon category
    const addonCategory = await prisma.addonCategory.create({
      data: { name, isRequired },
    });

    //create new join data
    const newMenuAddonCategory: { menuId: number, addonCategoryId: number }[] =
      menuIds.map((item: number) => ({
        menuId: item,
        addonCategoryId: addonCategory.id,
      }));

    //create new row in menuAddonCategory
    const menuAddonCategories = await prisma.$transaction(
      newMenuAddonCategory.map((item) =>
        prisma.menuAddonCategory.create({
          data: { menuId: item.menuId, addonCategoryId: item.addonCategoryId },
        })
      )
    );
    return res.status(200).json({ addonCategory, menuAddonCategories });
  } else if (method === "PUT") {
    //data validation
    const { id, name, isRequired, menuIds } = req.body;
    const isValid =
      id && name && isRequired !== undefined && menuIds.length > 0;
    if (!isValid) return res.status(400).send("Bad request.");

    //update addonCategory
    const addonCategory = await prisma.addonCategory.update({
      data: { name, isRequired },
      where: { id },
    });

    // delete old row
    await prisma.menuAddonCategory.deleteMany({
      where: { addonCategoryId: id },
    });

    // create new join data
    const menuAddonCategoryData: { addonCategoryId: number, menuId: number }[] =
      menuIds.map((item: number) => ({
        addonCategoryId: id,
        menuId: item,
      }));

    //update menuAddonCategory table
    const menuAddonCategories = await prisma.$transaction(
      menuAddonCategoryData.map((item) =>
        prisma.menuAddonCategory.create({
          data: item,
        })
      )
    );
    return res.status(200).json({ addonCategory, menuAddonCategories });
  } else if (method === "DELETE") {
    const addonCategoryId = Number(req.query.id);

    //find addoncategory to delete
    const addonCategory = await prisma.addonCategory.findFirst({
      where: { id: addonCategoryId },
    });
    if (!addonCategory) return res.status(400).send("Bad request.");
    await prisma.addonCategory.update({
      data: { isArchived: true },
      where: { id: addonCategoryId },
    });
    return res.status(200).send("Deleted.");
  }
  res.status(405).send("Method now allowed.");
}
```

> ရှင်းလင်းချက်

```js
if (method === "POST") {
  // Data validation
  const { name, isRequired = true, menuIds } = req.body;
  const isValid = name && menuIds.length > 0;
  if (!isValid) return res.status(400).send("Bad request.");

  //create new addon category
  const addonCategory = await prisma.addonCategory.create({
    data: { name, isRequired },
  });

  //create new join data
  const newMenuAddonCategory: { menuId: number, addonCategoryId: number }[] =
    menuIds.map((item: number) => ({
      menuId: item,
      addonCategoryId: addonCategory.id,
    }));

  //create new row in menuAddonCategory
  const menuAddonCategories = await prisma.$transaction(
    newMenuAddonCategory.map((item) =>
      prisma.menuAddonCategory.create({
        data: { menuId: item.menuId, addonCategoryId: item.addonCategoryId },
      })
    )
  );
  return res.status(200).json({ addonCategory, menuAddonCategories });
}
```

- POST request မှာတော့ ၀င်လာတဲ့ data တွေကို သုံးပြီး addon category နဲ့ menuAddonCategory table တွေမှာ row အသစ်တွေ create လုပ်ပြီး response ပြန်ပေးလိုက်တာပဲ ဖြစ်ပါတယ်

```js
else if (method === "PUT") {
   //data validation
    const { id, name, isRequired, menuIds } = req.body;
    const isValid =
      id && name && isRequired !== undefined && menuIds.length > 0;
    if (!isValid) return res.status(400).send("Bad request.");

    //update addonCategory
    const addonCategory = await prisma.addonCategory.update({
      data: { name, isRequired },
      where: { id },
    });

    // delete old row
    await prisma.menuAddonCategory.deleteMany({
      where: { addonCategoryId: id },
    });

    // create new join data
    const menuAddonCategoryData: { addonCategoryId: number; menuId: number }[] =
      menuIds.map((item: number) => ({
        addonCategoryId: id,
        menuId: item,
      }));

   //update menuAddonCategory table
    const menuAddonCategories = await prisma.$transaction(
      menuAddonCategoryData.map((item) =>
        prisma.menuAddonCategory.create({
          data: item,
        })
      )
    );
    return res.status(200).json({ addonCategory, menuAddonCategories });
  }
```

- PUT request မှာတော့ update လုပ်မယ့် category id ပေါ်မူတည်ပြီး ၀င်လာတဲ့ data တွေနဲ့ update လုပ်ပေးလိုက်တာပဲဖြစ်ပါတယ်
- join table မှာ update လုပ်မယ့် category id ရှိတဲ့ rows တွေ အရင်ဖျက်ပြီးမှ request ထဲက ပါလာတဲ့ data တွေနဲ့ row အသစ်တွေ ပြန်လုပ်ပေးလိုက်တာပဲ ဖြစ်ပါတယ်

```js
else if (method === "DELETE") {

    const addonCategoryId = Number(req.query.id);

    //find addoncategory to delete
    const addonCategory = await prisma.addonCategory.findFirst({
      where: { id: addonCategoryId },
    });
    if (!addonCategory) return res.status(400).send("Bad request.");
    await prisma.addonCategory.update({
      data: { isArchived: true },
      where: { id: addonCategoryId },
    });
    return res.status(200).send("Deleted.");
  }
```

- DELETE request မှာတော့ qurey အနေနဲ့ ထည့်ပေးလိုက်တဲ့ id ရှိတဲ့ addon-category ရဲ့ isArchived တန်ဖိုးကို true အဖြစ် update လုပ်ပေးလိုက်တာပဲဖြစ်ပါတယ်

##

### အရင် သင်ခန်းစာတွေမှာ menuAddonCategory ကို response ပြန်ထားပေမယ့် appslice မှာ လက်မခံရသေးလို့ လက်ခံပေးရပါမယ်

![](https://cdn.discordapp.com/attachments/1146496852087287898/1167023565317607486/image.png?ex=654c9e64&is=653a2964&hm=cadf6d1931d2033445d64707f742c4e431028bbb05a747a7531609d8614dfea7&)

- နောက်ပြီး delete လုပ်ထားတဲ့ addon-category တွေ မပါသွားရအောင် isArchived `false` ဖြစ်တဲ့ဟာတွေပဲ လှမ်းယူပေးရမှာဖြစ်ပါတယ်

![](https://cdn.discordapp.com/attachments/1146496852087287898/1167024380023410739/image.png?ex=654c9f26&is=653a2a26&hm=43ca7cee87c72636e3b5f95424221f315520671269e67f2e6c41adbfa6d9566b&)

##

### Using SnackBar component

- create / update / delete တွေ လုပ်တဲ့အခါ လုပ်ငန်းစဥ် ပြီးမြောက်ကြောင်း toast alert (snack) လေး ပြပေးလိုက်ချင်းအားဖြင့် UI/UX ကို ပိုပြီး အဆင်ပြေစေပါတယ်
- snackbar component နဲ့ snackBarSlice တွေကို သတ်မှတ်လိုက်ပါမယ်

```js
// src/types/snackerbar.ts

type SnackbarSeverity = "success" | "error";

export interface SnackbarSlice {
  message: string | null;
  autoHideDuration: number;
  open: boolean;
  severity: SnackbarSeverity;
}
```

```js
import { SnackbarSlice } from "@/types/snackbar";
import { createSlice } from "@reduxjs/toolkit";

const initialState: SnackbarSlice = {
  open: false,
  message: null,
  autoHideDuration: 5000,
  severity: "success",
};

const snackbarSlice = createSlice({
  name: "snackbar",
  initialState,
  reducers: {
    setOpenSnackbar: (state, action) => {
      const { autoHideDuration, message, severity } = action.payload;
      state.open = true;
      state.message = message;
      state.autoHideDuration = autoHideDuration;
      state.severity = severity;
    },
    resetSnackbar: (state) => {
      state.open = false;
    },
  },
});

export const { setOpenSnackbar, resetSnackbar } = snackbarSlice.actions;
export default snackbarSlice.reducer;
```

- slice တစ်ခု လုပ်ထားပြီး
- setOpenSnackbar action မှာတော့ open value ကို true အဖြစ် သတ်မှတ်ထားပြီး ကျန်တဲ့ value တွေကိုတော့ payload ထဲက data တွေနဲ့ Update လုပ်ထားပါတယ်
- resetSnackbar action မှာတော့ open value ကို false အဖြစ် ြပန်ထားပေးလိုက်ပါတယ်

```js
// src/components/Snackbar.tsx

import { useAppDispatch, useAppSelector } from "@/store/hooks";
import { resetSnackbar } from "@/store/slices/snackbarSlice";
import { Alert, Snackbar as MuiSnackBar } from "@mui/material";

const Snackbar = () => {
  const { open, severity, message, autoHideDuration } = useAppSelector(
    (state) => state.snackBar
  );

  setTimeout(() => dispatch(resetSnackbar()), autoHideDuration);
  return (
    <MuiSnackBar
      open={open}
      autoHideDuration={autoHideDuration}
      severity={severity}
      anchorOrigin={{ vertical: "bottom", horizontal: "right" }}
    >
      <Alert severity={severity} sx={{ width: "100%" }}>
        {message}
      </Alert>
    </MuiSnackBar>
  );
};

export default Snackbar;
```

- Snackbar component မှာတော့ store ထဲက dataတွေကို ယူလိုက် MUI snacker မှာ ထည့်ပေးလိုက်တာပဲဖြစ်ပါတယ်
- open props က snackbar အဖွင့်အပိတ် အတွက်
- autoHideDuration က အလိုအလျောက်ပိတ်ပေးမယ့်အချိန်
- serverity က icon (success/error)
- anchorOrigin က snackbar ကို ပြပေးမယ့် position ဖြစ်ပါတယ်
- snackbar component render လုပ်တဲ့အခါ setTimeoutနဲ့ ပြန်ပိတ်ပေးမယ့် action ကို ေခါ်ပေးလိုက်ပါတယ်

- Snackbar component ကို addon-category crate လုပ်တဲ့ new addon မှာ သုံးကြည့်လိုက်ပါမယ်

```js
// src/components/NewAddonCategory.tsx --> handleCreateAddonCategory
const handleCreateAddonCategory = () => {
  const isValid = newAddonCategory.name && newAddonCategory.menuIds.length > 0;
  if (!isValid) {
    return dispatch(
      setOpenSnackbar({
        message: "Missing required fields",
        severity: "error",
        open: true,
        autoHideDuration: 3000,
      })
    );
  }
  dispatch(
    createAddonCategory({
      ...newAddonCategory,
      onSuccess: () => {
        setOpen(false);
        dispatch(
          setOpenSnackbar({
            message: "New addon category created succcessfully.",
            severity: "success",
            open: true,
            autoHideDuration: 3000,
          })
        );
      },
    })
  );
};
```

```js
- addon-category create လုပ်ကြည့်ပါက data valid မဖြစ်ရင် error message ပြပေးမှာဖြစ်ပြီး created ဖြစ်ခဲ့ရင်တော့ success message ပြပေးမှာဖြစ်ပါတယ်
- ပြန်ပိတ်မယ့်အချိန်က 3 second သတ်မှတ်ပေးထားတာမလို့ 3sec ကြာရင် autoပြန်ပိတ်ပေးသွားမှာဖြစ်ပါတယ်
```
