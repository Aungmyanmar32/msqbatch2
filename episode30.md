## MSquare Programming Fullstack Course

### Batch 2

### Episode-_30_ Summary

### 1. Show selected location

### 2. Render all location with ItemCard component

### 3. Create new location

##

### 1. Show selected location

- setting page မှာ location တွေရွေးလို့ရအောင်နဲ့ ရွေးလိုက်တဲ့ location ရဲ့ id ကို localStroage မှာ သိမ်းထားလိုက်တာကို ပြီးခဲ့တဲ့သင်ခန်းစာမှာ လေ့လာခဲ့ကြပါတယ်
- ဒါပေမယ့် location တစ်ခုခုကိုရွေးပြီး တခြားpage တွေကို သွားကြည်ပြီးမှ setting page ကို ပြန်၀င်လာတဲ့အခါ ခုနက ရွေးထားတဲ့location ကselect လုပ်တဲ့နေရာမှာ မပြပေးတာကိုမြင်ရမှာဖြစ်ပါတယ်
- ဒီနေ့သင်ခန်းစာမှာတော့ location တစ်ခုခုကိုရွေးထားတယ်ဆိုရင် setting page ကို ဘယ်အချိန် ၀င်လာလာ ရွေးထားတဲ့ location ကို select လုပ်ပြီးသားဖြစ်အောင် လုပ်ကြည့်ပါမယ်

![](https://cdn.discordapp.com/attachments/1153197808900395062/1164259749848686725/image.png?ex=65429063&is=65301b63&hm=545d83c6ba61a0cceaaba803352c30976b10e0f90dc7c05edab21b11b0ea7c8c&)

```js
const SettingsPage = () => {
  const locations = useAppSelector((state) => state.location.items);
  const [locationId, setLocationId] = useState<number>();

  useEffect(() => {
    if (locations.length) {
      const selectedLocationId = localStorage.getItem("selectedLocationId");
      if (selectedLocationId) {
        setLocationId(Number(selectedLocationId));
      } else {
        const firstLocationId = locations[0].id;
        setLocationId(Number(firstLocationId));
        localStorage.setItem("selectedLocationId", String(firstLocationId));
      }
    }
  }, [locations]);

  const handleLocationChange = (evt: SelectChangeEvent<number>) => {
    localStorage.setItem("selectedLocationId", String(evt.target.value));
    setLocationId(Number(evt.target.value));
  };

  if (!locationId) return null;

  return (......)
```

> ရှင်းလင်းချက် ။ ။

```js
useEffect( ()=>{
    ......
},[locations])

```

- react useEffect hook ကို သုံးထားလိုက်ပြီး locations array ပြောင်းလဲမှုရှိတိုင်း useEffect ထဲက function ကို run ခိုင်းထားပါတယ်

```js
 if (locations.length) {
      const selectedLocationId = localStorage.getItem("selectedLocationId");
      if (selectedLocationId) {
        setLocationId(Number(selectedLocationId));
      } else {.....}
```

- useEffect function ထဲမှာတော့ locations ထဲမှာ item တွေရှိမရှိစစ်လိုက်ပါတယ်
- ရှိခဲ့ရင် localStroage ထဲမှာသိမ်းထားတဲ့ location id ကို selectedLocationId အဖြစ် ယူလိုက်ပါတယ်
- ရလာတဲ့ selectedLocationId က တန်ဖိုးတစ်ခုခုရှိခဲ့မယ်ဆိုရင် locationId state ကို selectedLocationId ရဲ့ တန်ဖိုးနဲ့ update လုပ်ပေးလိုက်ပါတယ်

```js
if (locations.length) {
  ........
} else {
        const firstLocationId = locations[0].id;
        setLocationId(Number(firstLocationId));
        localStorage.setItem("selectedLocationId", String(firstLocationId));
}
```

- selectedLocationId က တန်ဖိုးတစ်ခုခု မရှိခဲ့ရင်တော့ locations array ထဲက ပထမဆုံး item ရဲ့ id ကို locationId state တန်ဖိုးအဖြစ် update လုပ်ပေးလိုက်သလို localStroage မှာလည်း သိမ်းပေးထားလိုက်တာ ဖြစ်ပါတယ်
- useEffect ထဲက code တွေအားလုံကို ပြန်ပြီး ခြုံပြောရရင် local stroage မှာ သိမ်းထားတဲ့ location id ရှိရင် locationId state အဖြစ် သတ်မှတ်လိုက်မှာဖြစ်ပြီး
- local stroage မှာ သိမ်းထားတာမရှိဘူးဆိုရင် store ထဲက ယူထားတဲ့ location array ရဲ့ ပထမဆုံး item က id ကို သုံးပြီး state ကို update လုပ်လိုက်မှာဖြစ်ပါတယ်

```js
const handleLocationChange = (evt: SelectChangeEvent<number>) => {
  localStorage.setItem("selectedLocationId", String(evt.target.value));
  setLocationId(Number(evt.target.value));
};
```

- location တစ်ခုခုကို ရွေးလိုက်တဲ့အခါ run မယ့် function ထဲမှာတော့ ရွေးလိုက်တဲ့ location id ကို state ရဲ့ တန်ဖိုးအဖြစ် update လုပ်လိုက်သလို local stroage မှာလည်း သိမ်းလိုက်မှာဖြစ်ပါတယ်

```js
  if (!locationId) return null;

  return (......)
```

- setting page မှာ location ရွေးလို့ရတဲ့ element တွေ return မလုပ်ခင်လေးမှာ locationId state ကို စစ်လိုက်ပြီး falsy ဖြစ်ခဲ့ရင် null ကို retrun ပြန်လိုက်ပြီး useEffect ထဲက code တွေ run ပေးလိုက်မှာဖြစ်ပါတယ်

### locastroage မှာ location id တစ်ခုခုမရှိခဲ့ရင် useEffect ကို သုံးပြီး location id ကို ထည့်ပေးလိုက်မှာဖြစ်လို့ နောက်တစ်ကြိမ် render လုပ်တဲ့ အချိန်ကျရင် locationId state က တန်ဖိုးတစ်ခုခုရှိလာမှာဖြစ်ပြီး return ထဲက code တွေက render လုပ်ပေးမှာဖြစ်ပါတယ်

- ခု foodie app မှာ စမ်းကြည့်လိုက်ပါက setting page မှာ location တစ်ခုခုကို ရွေးပြီး ပြန်ထွက် ပြန်၀င် လုပ်ကြည့်ရင် ရွေးထားတဲ့ location ကို select လုပ်ပြီးသား ပြပေးတာကိုတွေ့ရမှာဖြစ်ပါတယ်

![](https://cdn.discordapp.com/attachments/1153197808900395062/1164266546835759155/image.png?ex=654296b7&is=653021b7&hm=f0920df5cf9ac977f497b3629870fdf17277183b1f684231179d218f484c29ae&)

##

### 2. Render all location with ItemCard component

- location page မှာလည်း store ထဲမှာရှိတဲ့ location ကို item card နဲ့ ပြပေးလိုက်ပါမယ်
- အရင်ဆုံး ItemCard component ကို create လုပ်လိုက်ပါမယ်

```js
// src/components/ItemCard.tsx

import { Paper, Typography } from "@mui/material";
import Link from "next/link";
import { ReactNode } from "react";

interface Props {
  icon: ReactNode;
  title: string;
  href?: string;
  subtitle?: string;
}

const ItemCard = ({ icon, title, href, subtitle }: Props) => {
  if (href) {
    return (
      <Link href={href} style={{ textDecoration: "none", color: "#000000" }}>
        <Paper
          elevation={2}
          sx={{
            width: 170,
            height: 170,
            p: 2,
            display: "flex",
            flexDirection: "column",
            justifyContent: "center",
            alignItems: "center",
            m: 2,
          }}
        >
          {icon}
          <Typography sx={{ color: "#4C4C6D", fontWeight: "700" }}>
            {title}
          </Typography>
          {subtitle && (
            <Typography sx={{ color: "#4C4C6D", fontSize: 14 }}>
              {subtitle}
            </Typography>
          )}
        </Paper>
      </Link>
    );
  }

  return (
    <Paper
      elevation={2}
      sx={{
        width: 170,
        height: 170,
        p: 2,
        display: "flex",
        flexDirection: "column",
        justifyContent: "center",
        alignItems: "center",
        m: 2,
      }}
    >
      {icon}
      <Typography sx={{ color: "#4C4C6D", fontWeight: "700" }}>
        {title}
      </Typography>
      {subtitle && (
        <Typography sx={{ color: "#4C4C6D", fontSize: 14 }}>
          {subtitle}
        </Typography>
      )}
    </Paper>
  );
};

export default ItemCard;
```

- ItemCard မှာတော့ **icon, title, href, subtitle ဆိုတဲ့ props** တွေ လက်ခံထားပြီး return လုပ်တဲ့အခါ ပြန်ပြီး render လုပ်ပေးထားလိုက်ပါတယ်
- အဲ့ဒီ ItemCard component ကို location page မှာ ခေါ်သုံးပြီး locationတွေကို ပြပေးလိုက်မှာပဲဖြစ်ပါတယ်

```js
// src/pages/backoffice/locations/index.tsx

import ItemCard from "@/components/ItemCard";
import NewLocation from "@/components/NewLocation";
import { useAppSelector } from "@/store/hooks";
import LocationOnIcon from "@mui/icons-material/LocationOn";
import { Box, Button } from "@mui/material";
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
      <Box sx={{ display: "flex", flexWrap: "wrap" }}>
        {locations.map((item) => (
          <ItemCard key={item.id} icon={<LocationOnIcon />} title={item.name} />
        ))}
      </Box>
      <NewLocation open={open} setOpen={setOpen} />
    </Box>
  );
};

export default LocationsPage;
```

- location တွေကို map လုပ်လိုက်ပြီး Itemcard ကို props တွေအဖြစ် icon ကို LocationIcon / titleကို Location ရဲ့ name တွေ နဲ့ ထည့်ပေးပြီး render လုပ်ပေးထားလိုက်တာပဲ ဖြစ်ပါတယ်

![](https://cdn.discordapp.com/attachments/1153197808900395062/1164270791240720476/image.png?ex=65429aab&is=653025ab&hm=c1a2f22e7bb394c60c83d8a85b7113aaed43e2eb0404d3b900f6af67fc6d7229&)

##

### 3. Create new location

- ဆက်ပြီးတော့ location တစ်ခုကို create လုပ်လို့ရအောင် ဆက်ပြီး လုပ်ကြပါမယ်
- NewLocation componet မှာ location တစ်ခု create လုပ်ရာမှာ လိုအပ်တဲ့ အချက်အလက်တွေကို mui textfield နဲ့ input ရယူပြီး server ဆီ request လုပ်ကာ database မှာ သိမ်းပြီး ပြန်ပြပေးမှာဖြစ်ပါတယ်

```js
// src/components/NewLocation.tsx

import { useAppDispatch } from "@/store/hooks";
import { createNewLocation } from "@/store/slices/locationSlice";
import {
  Box,
  Button,
  Dialog,
  DialogContent,
  DialogTitle,
  TextField,
} from "@mui/material";
import { Dispatch, SetStateAction, useState } from "react";

interface Props {
  open: boolean;
  setOpen: Dispatch<SetStateAction<boolean>>;
}

const NewLocation = ({ open, setOpen }: Props) => {
  const [newLocation, setNewLocation] = useState({ name: "", address: "" });
  const dispatch = useAppDispatch();

  return (
    <Dialog
      open={open}
      onClose={() => setOpen(false)}
      sx={{
        "& .MuiDialog-container": {
          "& .MuiPaper-root": {
            width: "100%",
            maxWidth: "400px", // Set your width here
          },
        },
      }}
    >
      <DialogTitle>Create new location</DialogTitle>
      <DialogContent>
        <Box sx={{ display: "flex", flexDirection: "column" }}>
          <TextField
            placeholder="Name"
            sx={{ mb: 2 }}
            onChange={(evt) =>
              setNewLocation({ ...newLocation, name: evt.target.value })
            }
          />
          <TextField
            placeholder="Address"
            onChange={(evt) =>
              setNewLocation({ ...newLocation, address: evt.target.value })
            }
          />
          <Box sx={{ display: "flex", justifyContent: "flex-end", mt: 3 }}>
            <Button
              variant="contained"
              sx={{ width: "fit-content", mr: 2 }}
              onClick={() => setOpen(false)}
            >
              Cancel
            </Button>
            <Button
              disabled={newLocation.name && newLocation.address ? false : true}
              variant="contained"
              sx={{ width: "fit-content" }}
              onClick={() => {
                dispatch(
                  createNewLocation({
                    ...newLocation,
                    onSuccess: () => setOpen(false),
                  })
                );
              }}
            >
              Confirm
            </Button>
          </Box>
        </Box>
      </DialogContent>
    </Dialog>
  );
};

export default NewLocation;
```

- newLocation state တစ်ခုကို သတ်မှတ်ထားပြီး
- newLocation stateအတွက် name နဲ့ address ကို Mui text field နဲ့ လက်ခံယူထားလိုက်ပြီး update လုပ်ထားပါတယ်
- cancel နဲ့ comfirm button နှစ်ခုလုပ်ထားပြီး
  - cancel ကို နှိပ်လိုက်ရင် dialog box ကိုပိတ်ပေး လိုက်မှာဖြစ်ပြီး
  - comfrim လုပ်လိုက်ရင်တော့ location storeထဲက createNewLocation actionကို dispatch လုပ်ပေးလိုက်မှာဖြစ်ပြီး newlocation state နဲ့ onSuccess function ကို payload အဖြစ်ထည့်ပေးလိုက်တာပဲဖြစ်ပါတယ်
- location slice ထဲမှာ createNewLocation action ကို သတ်မှတ်ပြီး server ဆီကို request လုပ်မှာဖြစ်ပါတယ်

```js
// src/store/slices/locationSlice.ts

import { CreateNewLocationOptions, LocationSlice } from "@/types/location";
import { config } from "@/utils/config";
import { createAsyncThunk, createSlice } from "@reduxjs/toolkit";

const initialState: LocationSlice = {
  items: [],
  isLoading: false,
  error: null,
};

export const createNewLocation = createAsyncThunk(
  "location/createNewLocation",
  async (options: CreateNewLocationOptions, thunkApi) => {
    const { name, address, onSuccess, onError } = options;
    try {
      const response = await fetch(`${config.apiBaseUrl}/location`, {
        method: "POST",
        headers: { "content-type": "application/json" },
        body: JSON.stringify({ name, address }),
      });
      const createdLocation = await response.json();
      thunkApi.dispatch(addLocation(createdLocation));
      onSuccess && onSuccess();
    } catch (err) {
      onError && onError();
    }
  }
);

const locationSlice = createSlice({
  name: "location",
  initialState,
  reducers: {
    setLocations: (state, action) => {
      state.items = action.payload;
    },
    addLocation: (state, action) => {
      state.items = [...state.items, action.payload];
    },
  },
});

export const { setLocations, addLocation } = locationSlice.actions;
export default locationSlice.reducer;
```

- createNewLocation thunk-action တစ်ခု လုပ်လိုက်ပြီး `/api/location` route ကို post method နဲ့ request လုပ်လိုက်ပါတယ်
- ပြန်ရလာတဲ့response ကို addLocation actionမှာ dispatch လုပ်ပြီး payload အဖြစ် ထည့်ပေးလိုက်ပါတယ်
- addLocation action မှာတော့ ၀င်လာတဲ့ payload ကို items array မှာ ထပ်ပေါင်းထည့်လိုက်တာပဲဖြစ်ပါတယ်
- `/api/location` routeမှာ POST request ကို လက်ခံပြီး database မှာ location တစ်ခု အသစ်လုပ်ပေးလိုက်မှာဖြစ်ပါတယ်

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
  const method = req.method;
  if (method === "POST") {
    const session = await getServerSession(req, res, authOptions);
    if (!session) return res.status(401).send("Unauthorized.");
    const user = session.user; // next-auth
    const email = user?.email as string;
    const dbUser = await prisma.user.findUnique({ where: { email } });
    if (!dbUser) return res.status(401).send("Unauthorized.");
    const companyId = dbUser.companyId;
    const { name, address } = req.body;
    // data validation
    const isValid = name && address;
    if (!isValid) return res.status(400).send("Bad request.");
    const createdLocation = await prisma.location.create({
      data: { name, address, companyId },
    });
    return res.status(200).json(createdLocation);
  }
  res.status(405).send("Method now allowed.");
}

```

- signin လုပ်ထားတဲ့ user ကို db မှာအရင်ရှာလိုက်ပြီး ရလာတဲ့ user data ထဲက company id ကိုသုံးပြီး req body ထဲက name နဲ့ address ကို ပေါင်းကာ location အသစ်တစ်ခုကို database မှာ create လုပ်လိုက်တာပဲဖြစ်ပါတယ်
- create လုပ်လိုက်လို့ prisma က return လုပ်ပေးတဲ့ row ကို response ပြန်လုပ်လိုက်တာပဲဖြစ်ပါတယ်
