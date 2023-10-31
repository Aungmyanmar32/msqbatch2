## MSquare Programming Fullstack Course

### Batch 2

### Episode-_34_ Summary

### 1. Addon CURD

### 2. Table CURD

### 3. Menu-category CURD

##

### 1. Addon CURD

- အရင် သင်ခန်းစာတွေတုန်းကလိုပဲ addon ကို Addon page မှာ item card ကို သုံးပြီးပြပေးလိုက်ပါမယ်

```js
// src/pages/backoffice/addon/index.tsx

import ItemCard from "@/components/ItemCard";
import NewAddon from "@/components/NewAddon";
import { useAppSelector } from "@/store/hooks";
import EggIcon from "@mui/icons-material/Egg";
import { Box, Button } from "@mui/material";
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
      <Box sx={{ display: "flex", flexWrap: "wrap" }}>
        {addons.map((item) => (
          <ItemCard
            icon={<EggIcon />}
            href={`/backoffice/addons/${item.id}`}
            key={item.id}
            title={item.name}
          />
        ))}
      </Box>
      <NewAddon open={open} setOpen={setOpen} />
    </Box>
  );
};

export default AddonsPage;
```

##

### Update and delete addon

- Addon item တစ်ခုခုကို click လိုက်ရင် Dynamic route နဲ့ addon detail page ကို ပို့လိုက်ပြီး update and delete လုပ်လို့ရအောင် လုပ်လိုက်ပါမယ်

```js
// src/pages/backoffice/addons/[id]/index.tsx

import { useAppDispatch, useAppSelector } from "@/store/hooks";
import { deleteAddon, updateAddon } from "@/store/slices/addonSlice";
import { UpdateAddonOptions } from "@/types/addon";
import {
  Box,
  Button,
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
import { AddonCategory } from "@prisma/client";
import { useRouter } from "next/router";
import { useEffect, useState } from "react";

const AddonDetail = () => {
  const router = useRouter();
  const addonId = Number(router.query.id);
  const addons = useAppSelector((state) => state.addon.items);
  const addonCategories = useAppSelector((state) => state.addonCategory.items);
  const addon = addons.find((item) => item.id === addonId);
  const [data, setData] = useState<UpdateAddonOptions>();
  const dispatch = useAppDispatch();
  const [open, setOpen] = useState(false);

  useEffect(() => {
    if (addon) {
      setData({
        id: addon.id,
        name: addon.name,
        price: addon.price,
        addonCategoryId: addon.addonCategoryId,
      });
    }
  }, [addon]);

  if (!addon || !data) return null;

  const handleOnChange = (evt: SelectChangeEvent<number>) => {
    const selectedId = evt.target.value as number;
    setData({ ...data, id: addon.id, addonCategoryId: selectedId });
  };

  const handleUpdateAddon = () => {
    dispatch(updateAddon(data));
  };

  const handleDeleteAddon = () => {
    dispatch(
      deleteAddon({
        id: addon.id,
        onSuccess: () => router.push("/backoffice/addons"),
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
        defaultValue={data.name}
        sx={{ mb: 2 }}
        onChange={(evt) =>
          setData({ ...data, id: addon.id, name: evt.target.value })
        }
      />
      <TextField
        defaultValue={data.price}
        sx={{ mb: 2 }}
        onChange={(evt) =>
          setData({ ...data, id: addon.id, price: Number(evt.target.value) })
        }
      />
      <FormControl fullWidth>
        <InputLabel>Addon Category</InputLabel>
        <Select
          value={data.addonCategoryId}
          label="Addon Category"
          onChange={handleOnChange}
          renderValue={(selectedAddonCategoryId) => {
            return (
              addonCategories.find(
                (item) => item.id === selectedAddonCategoryId
              ) as AddonCategory
            ).name;
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
          {addonCategories.map((item) => (
            <MenuItem key={item.id} value={item.id}>
              <ListItemText primary={item.name} />
            </MenuItem>
          ))}
        </Select>
      </FormControl>
      <Button
        variant="contained"
        sx={{ mt: 2, width: "fit-content" }}
        onClick={handleUpdateAddon}
      >
        Update
      </Button>
      <Dialog open={open} onClose={() => setOpen(false)}>
        <DialogTitle>Confirm delete addon</DialogTitle>
        <DialogContent>
          Are you sure you want to delete this addon?
        </DialogContent>
        <DialogActions>
          <Button onClick={() => setOpen(false)}>Cancel</Button>
          <Button variant="contained" onClick={handleDeleteAddon}>
            Confirm
          </Button>
        </DialogActions>
      </Dialog>
    </Box>
  );
};

export default AddonDetail;
```

- ပြင်ချင်တဲ့ addon id ပေါ်မူတည်ပြီး name နဲ့ addon category တွေကို update လုပ်လို့ရအောင် လုပ်လိုက်ပြီး update ဖြစ်တဲ့ state ကို updateAddon action ဆီ dispatch လုပ်လိုက်တာပဲဖြစ်ပါတယ်
- အလားတူပဲ delete ကို နှိပ်လိုက်ရင် dialog box နဲ့ delete ကို confirm လုပ်ခိုင်းလိုက်ပြီး deleteAddon action ဆီ dispatch လုပ်လိုက်တာပဲဖြစ်ပါတယ်

##

### Create New addon

- NewAddon component မှာလည်း addon တစ်ခု create လုပ်လို့ရအောင် သတ်မှတ်ပေးလိုက်ပါမယ်

```js
// src/components/NewAddon.tsx

import { useAppDispatch, useAppSelector } from "@/store/hooks";
import { createAddon } from "@/store/slices/addonSlice";
import { CreateAddonOptions } from "@/types/addon";
import {
  Box,
  Button,
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
import { AddonCategory } from "@prisma/client";
import { Dispatch, SetStateAction, useState } from "react";

interface Props {
  open: boolean;
  setOpen: Dispatch<SetStateAction<boolean>>;
}

const defaultNewAddon = {
  name: "",
  price: 0,
  addonCategoryId: undefined,
};

const NewAddon = ({ open, setOpen }: Props) => {
  const [newAddon, setNewAddon] = useState<CreateAddonOptions>(defaultNewAddon);
  const addonCategories = useAppSelector((state) => state.addonCategory.items);
  const dispatch = useAppDispatch();

  const handleOnChange = (evt: SelectChangeEvent<number>) => {
    const selectedId = evt.target.value as number;
    setNewAddon({ ...newAddon, addonCategoryId: selectedId });
  };

  const handleCreateAddon = () => {
    dispatch(createAddon({ ...newAddon, onSuccess: () => setOpen(false) }));
  };
  return (
    <Dialog
      open={open}
      onClose={() => {
        setOpen(false);
        setNewAddon(defaultNewAddon);
      }}
    >
      <DialogTitle>Create new addon</DialogTitle>
      <DialogContent>
        <DialogContent
          sx={{ display: "flex", flexDirection: "column", width: 400 }}
        >
          <TextField
            placeholder="Name"
            sx={{ mb: 2 }}
            onChange={(evt) =>
              setNewAddon({ ...newAddon, name: evt.target.value })
            }
          />
          <TextField
            placeholder="Price"
            sx={{ mb: 2 }}
            onChange={(evt) =>
              setNewAddon({ ...newAddon, price: Number(evt.target.value) })
            }
          />
          <FormControl fullWidth>
            <InputLabel>Addon Category</InputLabel>
            <Select
              value={newAddon.addonCategoryId}
              label="Addon Category"
              onChange={handleOnChange}
              renderValue={(selectedAddonCategoryId) => {
                return (
                  addonCategories.find(
                    (item) => item.id === selectedAddonCategoryId
                  ) as AddonCategory
                ).name;
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
              {addonCategories.map((item) => (
                <MenuItem key={item.id} value={item.id}>
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
              disabled={!newAddon.name || !newAddon.addonCategoryId}
              onClick={handleCreateAddon}
            >
              Confirm
            </Button>
          </Box>
        </DialogContent>
      </DialogContent>
    </Dialog>
  );
};

export default NewAddon;

```

##

### Prepairing in addon slice

- addon slice မှာလည်း action တွေ သတ်မှတ်ပြီး server request လုပ်ကာ store ကိုလည်း update လုပ်ပေးလိုက်မှာ ဖြစ်ပါတယ်

```js
// src/store/slices/addonSlice.ts

import {
  AddonSlice,
  CreateAddonOptions,
  DeleteAddonOptions,
  UpdateAddonOptions,
} from "@/types/addon";
import { config } from "@/utils/config";
import { Addon } from "@prisma/client";
import { PayloadAction, createAsyncThunk, createSlice } from "@reduxjs/toolkit";

const initialState: AddonSlice = {
  items: [],
  isLoading: false,
  error: null,
};

export const createAddon = createAsyncThunk(
  "addon/createAddon",
  async (options: CreateAddonOptions, thunkApi) => {
    const { name, price, addonCategoryId, onSuccess, onError } = options;
    try {
      const response = await fetch(`${config.apiBaseUrl}/addons`, {
        method: "POST",
        headers: { "content-type": "application/json" },
        body: JSON.stringify({ name, price, addonCategoryId }),
      });
      const { addon } = await response.json();
      thunkApi.dispatch(addAddon(addon));
      onSuccess && onSuccess();
    } catch (err) {
      onError && onError();
    }
  }
);

export const updateAddon = createAsyncThunk(
  "addon/updateAddon",
  async (options: UpdateAddonOptions, thunkApi) => {
    const { id, name, price, addonCategoryId, onSuccess, onError } = options;
    try {
      const response = await fetch(`${config.apiBaseUrl}/addons`, {
        method: "PUT",
        headers: { "content-type": "application/json" },
        body: JSON.stringify({ id, name, price, addonCategoryId }),
      });
      const { addon } = await response.json();
      thunkApi.dispatch(replaceAddon(addon));
      onSuccess && onSuccess();
    } catch (err) {
      onError && onError();
    }
  }
);

export const deleteAddon = createAsyncThunk(
  "addon/deleteAddon",
  async (options: DeleteAddonOptions, thunkApi) => {
    const { id, onSuccess, onError } = options;
    try {
      await fetch(`${config.apiBaseUrl}/addons?id=${id}`, {
        method: "DELETE",
      });
      thunkApi.dispatch(removeAddon({ id }));
      onSuccess && onSuccess();
    } catch (err) {
      onError && onError();
    }
  }
);

const addonSlice = createSlice({
  name: "addon",
  initialState,
  reducers: {
    setAddons: (state, action) => {
      state.items = action.payload;
    },
    replaceAddon: (state, action: PayloadAction<Addon>) => {
      state.items = state.items.map((item) =>
        item.id === action.payload.id ? action.payload : item
      );
    },
    addAddon: (state, action: PayloadAction<Addon>) => {
      state.items = [...state.items, action.payload];
    },
    removeAddon: (state, action: PayloadAction<{ id: number }>) => {
      state.items = state.items.filter((item) => item.id !== action.payload.id);
    },
  },
});

export const { setAddons, replaceAddon, addAddon, removeAddon } =
  addonSlice.actions;
export default addonSlice.reducer;
```

###

- api မှာလည်း request တွေကို လက်ခံပြီး database တွေကို update လုပ်ကာ response ပြန်ပေးလိုက်မှာဖြစ်ပါတယ်

```js
// src/pages/api/addons/index.ts

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
    const { name, price, addonCategoryId } = req.body;
    const isValid = name && price !== undefined && addonCategoryId;
    if (!isValid) return res.status(400).send("Bad request.");
    const addon = await prisma.addon.create({
      data: { name, price, addonCategoryId },
    });
    return res.status(200).json({ addon });
  } else if (method === "PUT") {
    const { id, name, price, addonCategoryId } = req.body;
    const isValid = id && name && price !== undefined && addonCategoryId;
    if (!isValid) return res.status(400).send("Bad request.");
    const exist = await prisma.addon.findFirst({ where: { id } });
    if (!exist) return res.status(400).send("Bad request.");
    const addon = await prisma.addon.update({
      data: { name, price, addonCategoryId },
      where: { id },
    });
    return res.status(200).json({ addon });
  } else if (method === "DELETE") {
    const addonId = Number(req.query.id);
    const addon = await prisma.addon.findFirst({
      where: { id: addonId },
    });
    if (!addon) return res.status(400).send("Bad request.");
    await prisma.addon.update({
      data: { isArchived: true },
      where: { id: addonId },
    });
    return res.status(200).send("Deleted.");
  }
  res.status(405).send("Method now allowed.");
}
```

- POST method နဲ့ request ၀င်လာရင် addon တစ်ခုကို database မှာ create လုပ်လိုက်ပြီး response ပြန်ပေးလိုက်တာပဲဖြစ်ပါတယ်
- PUT method နဲ့ request ၀င်လာရင် addon id ပေါ် မူတည်ပြီး database မှာ update လုပ်လိုက်ပြီး response ပြန်ပေးလိုက်တာပဲဖြစ်ပါတယ်
- DELETE method နဲ့ request ၀င်လာရင် တော့ addon id ပေါ် မူတည်ပြီး database မှာ isArchived true လုပ်ပေးလိုက်တာပဲဖြစ်ပါတယ်

### 2. Table CURD

- Table ကို Table page မှာ item card ကို သုံးပြီးပြပေးလိုက်ပါမယ်

```js
//src/pages/backoffice/tables/index.tsx

import ItemCard from "@/components/ItemCard";
import NewTable from "@/components/NewTable";
import { useAppSelector } from "@/store/hooks";
import TableBarIcon from "@mui/icons-material/TableBar";
import { Box, Button } from "@mui/material";
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
      <Box sx={{ display: "flex", flexWrap: "wrap" }}>
        {tables.map((item) => (
          <ItemCard
            href={`/backoffice/tables/${item.id}`}
            icon={<TableBarIcon />}
            key={item.id}
            title={item.name}
          />
        ))}
      </Box>
      <NewTable open={open} setOpen={setOpen} />
    </Box>
  );
};

export default TablesPage;
```

- ### Update and delete Table
- Table item တစ်ခုခုကို click လိုက်ရင် Dynamic route နဲ့ Table detail page ကို ပို့လိုက်ပြီး update and delete လုပ်လို့ရအောင် လုပ်လိုက်ပါမယ်

```js
// src/pages/backoffice/tables/[id]/index.tsx

import { useAppDispatch, useAppSelector } from "@/store/hooks";
import { deleteTable, updateTable } from "@/store/slices/tableSlice";
import { UpdateTableOptions } from "@/types/table";
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

const TableDetail = () => {
  const router = useRouter();
  const tableId = Number(router.query.id);
  const tables = useAppSelector((state) => state.table.items);
  const table = tables.find((item) => item.id === tableId);
  const [data, setData] = useState<UpdateTableOptions>();
  const dispatch = useAppDispatch();
  const [open, setOpen] = useState(false);

  useEffect(() => {
    if (table) {
      setData({
        id: table.id,
        name: table.name,
        locationId: table.locationId,
      });
    }
  }, [table]);

  if (!table || !data) return null;

  const handleUpdateTable = () => {
    dispatch(updateTable(data));
  };

  const handleDeleteTable = () => {
    dispatch(
      deleteTable({
        id: table.id,
        onSuccess: () => router.push("/backoffice/tables"),
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
        defaultValue={data.name}
        sx={{ mb: 2 }}
        onChange={(evt) =>
          setData({ ...data, id: table.id, name: evt.target.value })
        }
      />
      <Button
        variant="contained"
        sx={{ mt: 2, width: "fit-content" }}
        onClick={handleUpdateTable}
      >
        Update
      </Button>
      <Dialog open={open} onClose={() => setOpen(false)}>
        <DialogTitle>Confirm delete table</DialogTitle>
        <DialogContent>
          Are you sure you want to delete this table?
        </DialogContent>
        <DialogActions>
          <Button onClick={() => setOpen(false)}>Cancel</Button>
          <Button variant="contained" onClick={handleDeleteTable}>
            Confirm
          </Button>
        </DialogActions>
      </Dialog>
    </Box>
  );
};

export default TableDetail;
```

- ပြင်ချင်တဲ့ Table id ပေါ်မူတည်ပြီး update လုပ်လို့ရအောင် လုပ်လိုက်ပြီး update ဖြစ်တဲ့ state ကို updateTable action ဆီ dispatch လုပ်လိုက်တာပဲဖြစ်ပါတယ်
- အလားတူပဲ delete ကို နှိပ်လိုက်ရင် dialog box နဲ့ delete ကို confirm လုပ်ခိုင်းလိုက်ပြီး deleteTable action ဆီ dispatch လုပ်လိုက်တာပဲဖြစ်ပါတယ်

### Create New Table

- NewTable component မှာလည်း Table တစ်ခု create လုပ်လို့ရအောင် သတ်မှတ်ပေးလိုက်ပါမယ်

```js
src / components / NewTable.tsx;

import { useAppDispatch } from "@/store/hooks";
import { createTable } from "@/store/slices/tableSlice";
import { CreateTableOptions } from "@/types/table";
import {
  Box,
  Button,
  Dialog,
  DialogContent,
  DialogTitle,
  TextField,
} from "@mui/material";
import { Dispatch, SetStateAction, useEffect, useState } from "react";

interface Props {
  open: boolean;
  setOpen: Dispatch<SetStateAction<boolean>>;
}

const defaultNewMenu = {
  name: "",
  locationId: undefined,
};

const NewTable = ({ open, setOpen }: Props) => {
  const [newTable, setNewTable] =
    useState < CreateTableOptions > defaultNewMenu;
  const dispatch = useAppDispatch();

  useEffect(() => {
    setNewTable({
      ...newTable,
      locationId: Number(localStorage.getItem("selectedLocationId")),
    });
  }, []);

  const handleCreateMenu = () => {
    dispatch(createTable({ ...newTable, onSuccess: () => setOpen(false) }));
  };

  return (
    <Dialog
      open={open}
      onClose={() => {
        setOpen(false);
        setNewTable(defaultNewMenu);
      }}
    >
      <DialogTitle>Create new table</DialogTitle>
      <DialogContent
        sx={{ display: "flex", flexDirection: "column", width: 400 }}
      >
        <TextField
          placeholder="Name"
          sx={{ mb: 2 }}
          onChange={(evt) =>
            setNewTable({ ...newTable, name: evt.target.value })
          }
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
            disabled={!newTable.name}
            onClick={handleCreateMenu}
          >
            Confirm
          </Button>
        </Box>
      </DialogContent>
    </Dialog>
  );
};

export default NewTable;
```

##

### Prepairing in Table slice

- Table slice မှာလည်း action တွေ သတ်မှတ်ပြီး server request လုပ်ကာ store ကိုလည်း update လုပ်ပေးလိုက်မှာ ဖြစ်ပါတယ်

```js
// src/store/slices/tableSlice.ts

import {
  CreateTableOptions,
  DeleteTableOptions,
  TableSlice,
  UpdateTableOptions,
} from "@/types/table";
import { config } from "@/utils/config";
import { Table } from "@prisma/client";
import { PayloadAction, createAsyncThunk, createSlice } from "@reduxjs/toolkit";

const initialState: TableSlice = {
  items: [],
  isLoading: false,
  error: null,
};

export const createTable = createAsyncThunk(
  "table/createTable",
  async (options: CreateTableOptions, thunkApi) => {
    const { name, locationId, onSuccess, onError } = options;
    try {
      const response = await fetch(`${config.apiBaseUrl}/tables`, {
        method: "POST",
        headers: { "content-type": "application/json" },
        body: JSON.stringify({ name, locationId }),
      });
      const { table } = await response.json();
      thunkApi.dispatch(addTable(table));
      onSuccess && onSuccess();
    } catch (err) {
      onError && onError();
    }
  }
);

export const updateTable = createAsyncThunk(
  "table/updateTable",
  async (options: UpdateTableOptions, thunkApi) => {
    const { id, name, onSuccess, onError } = options;
    try {
      const response = await fetch(`${config.apiBaseUrl}/tables`, {
        method: "PUT",
        headers: { "content-type": "application/json" },
        body: JSON.stringify({ id, name }),
      });
      const { table } = await response.json();
      thunkApi.dispatch(replaceTable(table));
      onSuccess && onSuccess();
    } catch (err) {
      onError && onError();
    }
  }
);

export const deleteTable = createAsyncThunk(
  "table/deleteTable",
  async (options: DeleteTableOptions, thunkApi) => {
    const { id, onSuccess, onError } = options;
    try {
      await fetch(`${config.apiBaseUrl}/tables?id=${id}`, {
        method: "DELETE",
      });
      thunkApi.dispatch(removeTable({ id }));
      onSuccess && onSuccess();
    } catch (err) {
      onError && onError();
    }
  }
);

const tableSlice = createSlice({
  name: "table",
  initialState,
  reducers: {
    setTables: (state, action) => {
      state.items = action.payload;
    },
    replaceTable: (state, action: PayloadAction<Table>) => {
      state.items = state.items.map((item) =>
        item.id === action.payload.id ? action.payload : item
      );
    },
    addTable: (state, action: PayloadAction<Table>) => {
      state.items = [...state.items, action.payload];
    },
    removeTable: (state, action: PayloadAction<{ id: number }>) => {
      state.items = state.items.filter((item) => item.id !== action.payload.id);
    },
  },
});

export const { setTables, replaceTable, removeTable, addTable } =
  tableSlice.actions;
export default tableSlice.reducer;
```

##

- api မှာလည်း request တွေကို လက်ခံပြီး database တွေကို update လုပ်ကာ response ပြန်ပေးလိုက်မှာဖြစ်ပါတယ်

```js
src / pages / api / tables / index.ts;

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
    const { name, locationId } = req.body;
    const isValid = name && locationId;
    if (!isValid) return res.status(400).send("Bad request.");
    const table = await prisma.table.create({
      data: { name, locationId },
    });
    return res.status(200).json({ table });
  } else if (method === "PUT") {
    const { id, name } = req.body;
    const isValid = id && name;
    if (!isValid) return res.status(400).send("Bad request.");
    const exist = await prisma.table.findFirst({ where: { id } });
    if (!exist) return res.status(400).send("Bad request.");
    const table = await prisma.table.update({
      data: { name },
      where: { id },
    });
    return res.status(200).json({ table });
  } else if (method === "DELETE") {
    const tableId = Number(req.query.id);
    const table = await prisma.table.findFirst({
      where: { id: tableId },
    });
    if (!table) return res.status(400).send("Bad request.");
    await prisma.table.update({
      data: { isArchived: true },
      where: { id: tableId },
    });
    return res.status(200).send("Deleted.");
  }
  res.status(405).send("Method now allowed.");
}
```

- POST method နဲ့ request ၀င်လာရင် Table တစ်ခုကို database မှာ create လုပ်လိုက်ပြီး response ပြန်ပေးလိုက်တာပဲဖြစ်ပါတယ်
- PUT method နဲ့ request ၀င်လာရင် Table id ပေါ် မူတည်ပြီး database မှာ update လုပ်လိုက်ပြီး response ပြန်ပေးလိုက်တာပဲဖြစ်ပါတယ်
- DELETE method နဲ့ request ၀င်လာရင် တော့ Table id ပေါ် မူတည်ပြီး database မှာ isArchived true လုပ်ပေးလိုက်တာပဲဖြစ်ပါတယ်

##

### Menu-category CURD

- Menu-category ကို Addon page မှာ item card ကို သုံးပြီးပြပေးလိုက်ပါမယ်

```js
// src/pages/backoffice/menu-categories/index.tsx

import ItemCard from "@/components/ItemCard";
import NewMenuCategory from "@/components/NewMenuCategory";
import { useAppSelector } from "@/store/hooks";
import CategoryIcon from "@mui/icons-material/Category";
import { Box, Button } from "@mui/material";
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
      <Box sx={{ display: "flex", flexWrap: "wrap" }}>
        {menuCategories.map((item) => (
          <ItemCard
            icon={<CategoryIcon />}
            href={`/backoffice/menu-categories/${item.id}`}
            key={item.id}
            title={item.name}
          />
        ))}
      </Box>
      <NewMenuCategory open={open} setOpen={setOpen} />
    </Box>
  );
};

export default MenuCategoriesPage;
```
