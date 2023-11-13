## MSquare Programming Fullstack Course

### Batch 2

### Episode-_40_ Summary

##

### 1. Order App (Menu details page)

### 2. Error handling and debugging

##

### 1. Order App (Menu details page)

- order app မှာ menu တစ်ခုကို နှိပ်လိုက်တဲ့ အချိန်မှာ အဲ့ဒီ menu နဲ့ ပတ်သတ်တဲ့ addon category တွေနဲ့ add onတွေကို ရွေးလို့ရအောင် ပြပေးလိုက်မှာဖြစ်ပါတယ်
-

```js
// src/pages/order/menus/[id]/index.tsx

import AddonCategories from "@/components/AddonCategories";
import QuantitySelector from "@/components/QuantitySelector";
import { useAppSelector } from "@/store/hooks";
import { Box } from "@mui/material";
import Image from "next/image";
import { useRouter } from "next/router";
import { useState } from "react";

const MenuDetail = () => {
  const { query, isReady } = useRouter();
  const menuId = Number(query.id);
  const menus = useAppSelector((state) => state.menu.items);
  const menu = menus.find((item) => item.id === menuId);
  const [quantity, setQuantity] = useState(1);
  const [selectedAddonIds, setSelectedAddonIds] = useState<number[]>([]);

  const handleQuantityDecrease = () => {
    const newValue = quantity - 1 === 0 ? 1 : quantity - 1;
    setQuantity(newValue);
  };

  const handleQuantityIncrease = () => {
    const newValue = quantity + 1;
    setQuantity(newValue);
  };

  if (!isReady || !menu) return null;

  return (
    <Box>
      <Box
        sx={{
          display: "flex",
          justifyContent: "center",
          flexDirection: "column",
          p: 4,
        }}
      >
        <Image
          src={menu.assetUrl || "/default-menu.png"}
          alt="menu-image"
          width={150}
          height={150}
          style={{
            borderRadius: "50%",
            margin: "0 auto",
          }}
        />
        <Box
          sx={{
            mt: 5,
            display: "flex",
            flexDirection: "column",
            alignItems: "center",
          }}
        >
          <AddonCategories
            menuId={menuId}
            selectedAddonIds={selectedAddonIds}
            setSelectedAddonIds={setSelectedAddonIds}
          />
          <QuantitySelector
            value={quantity}
            onDecrease={handleQuantityDecrease}
            onIncrease={handleQuantityIncrease}
          />

        </Box>
      </Box>
    </Box>
  );
};

export default MenuDetail;
```

## ရှင်းလင်းချက်

```js
const [quantity, setQuantity] = useState(1);
  const [selectedAddonIds, setSelectedAddonIds] = useState<number[]>([]);
```

- ရွေးထားတဲ့ addon တွေကို သိမ်းဖို့ selectedAddons state တစ်ခုကိုလည်း သတ်မှတ်ထားလိုက်ပါမယ်
- quantity ကို သိမ်းဖို့အတွက်လည်း state တစ်ခု သတ်မှတ်ပေးထားလိုက်ပါတယ်

```js
const handleQuantityDecrease = () => {
  const newValue = quantity - 1 === 0 ? 1 : quantity - 1;
  setQuantity(newValue);
};

const handleQuantityIncrease = () => {
  const newValue = quantity + 1;
  setQuantity(newValue);
};
```

- quantity တွေကို ကိုင်တွယ်မယ့် function တွေဖြစ်ပါတယ်

```js
<Image
  src={menu.assetUrl || "/default-menu.png"}
  alt="menu-image"
  width={150}
  height={150}
  style={{
    borderRadius: "50%",
    margin: "0 auto",
  }}
/>
```

- order လုပ်မယ့် menu ပုံကို ပြပေးထားလိုက်ပါတယ်

```js
<AddonCategories
  menuId={menuId}
  selectedAddonIds={selectedAddonIds}
  setSelectedAddonIds={setSelectedAddonIds}
/>
```

- AsddonCategory component ကို ခေါ်သုံးလိုက်ပြီးပြီး လိုအပ်တဲ့ props တွေကို ထည့်ပေးလိုက်ပါတယ်

```js
<QuantitySelector
  value={quantity}
  onDecrease={handleQuantityDecrease}
  onIncrease={handleQuantityIncrease}
/>
```

- အောက်ဆုံးမှာတော့ quantity ကို updateလုပ်လို့ရမယ့် ခလုတ်တွေကို ထည့်ပေးထားလိုက်တာပဲဖြစ်ပါတယ်

##

- QuantitySelector component တစ်ခု သတ်မှတ်ပေးလိုက်ပါမယ်

```js
import { AddCircle, RemoveCircle } from "@mui/icons-material";
import { Box, IconButton, Typography } from "@mui/material";

interface Props {
  value: number;
  onIncrease: () => void;
  onDecrease: () => void;
}

const QuantitySelector = ({ value, onIncrease, onDecrease }: Props) => {
  return (
    <Box
      sx={{
        display: "flex",
        justifyContent: "space-between",
        alignItems: "center",
        maxWidth: "100px",
        mt: 5,
      }}
    >
      <IconButton color="primary" onClick={onDecrease}>
        <RemoveCircle />
      </IconButton>
      <Typography variant="h5">{value}</Typography>
      <IconButton color="primary" onClick={onIncrease}>
        <AddCircle />
      </IconButton>
    </Box>
  );
};

export default QuantitySelector;
```

##

- AddonCategory component မှာ Addonတွေကို category အလိုက် ပြထားလိုက်ပြီး ရွေးလို့ရအောင် လုပ်လိုက်ပါမယ်

```js
import { useAppSelector } from "@/store/hooks";
import { Box, Chip, Typography } from "@mui/material";
import { Dispatch, SetStateAction } from "react";
import Addons from "./Addons";

interface Props {
  menuId: number;
  selectedAddonIds: number[];
  setSelectedAddonIds: Dispatch<SetStateAction<number[]>>;
}

const AddonCategories = ({
  menuId,
  selectedAddonIds,
  setSelectedAddonIds,
}: Props) => {
  const allMenuAddonCategories = useAppSelector(
    (state) => state.menuAddonCategory.items
  );
  const addonCategoryIds = allMenuAddonCategories
    .filter((item) => item.menuId === menuId)
    .map((item) => item.addonCategoryId);
  const addonCategories = useAppSelector(
    (state) => state.addonCategory.items
  ).filter((item) => addonCategoryIds.includes(item.id));

  return (
    <Box>
      {addonCategories.map((item) => {
        return (
          <Box key={item.id} sx={{ mb: 5 }}>
            <Box
              sx={{
                display: "flex",
                width: "300px",
                justifyContent: "space-between",
              }}
            >
              <Typography variant="h6" sx={{ userSelect: "none" }}>
                {item.name}
              </Typography>
              <Chip label={item.isRequired ? "Required" : "Optional"} />
            </Box>
            <Box sx={{ pl: 1, mt: 2 }}>
              <Addons
                addonCategoryId={item.id}
                selectedAddonIds={selectedAddonIds}
                setSelectedAddonIds={setSelectedAddonIds}
              />
            </Box>
          </Box>
        );
      })}
    </Box>
  );
};

export default AddonCategories;
```

- Addon category တွေ အားလုံးထဲမှ props အနေနဲ့ ၀င်လာတဲ့ menu id တွေ နဲ့ join ထားတဲ့ category တွေ အရင်ရွေးထုတ်လိုက်ပါတယ်
- ရွေးထုတ်ထားတဲ့ addon category တွေကို map လုပ်ပြီး render လုပ်ထားပါတယ်
- render လုပ်တဲ့အချိန်မှာ သက်ဆိုင်ရာ addon တွေကို တစ်ခါတည်းပြနိုင်ဖို့ Addon component ကိုပါ ထည့်ပြီး loop လုပ်ပေးထားပါတယ်
  ##
- Addon component ကို သတ်မှတ်လိုက်ပါမယ်

```js
// src/components/Addon.tsx

import { useAppSelector } from "@/store/hooks";
import {
  Box,
  Checkbox,
  FormControlLabel,
  Radio,
  Typography,
} from "@mui/material";
import { Dispatch, SetStateAction } from "react";

interface Props {
  addonCategoryId: number;
  selectedAddonIds: number[];
  setSelectedAddonIds: Dispatch<SetStateAction<number[]>>;
}

const Addons = ({
  addonCategoryId,
  selectedAddonIds,
  setSelectedAddonIds,
}: Props) => {
  const addonCategory = useAppSelector(
    (state) => state.addonCategory.items
  ).find((item) => item.id === addonCategoryId);
  const addons = useAppSelector((state) => state.addon.items).filter(
    (item) => item.addonCategoryId === addonCategoryId
  );
  if (!addonCategory) return null;
  return (
    <Box>
      {addons.map((addon) => {
        return (
          <Box
            key={addon.id}
            sx={{
              display: "flex",
              justifyContent: "space-between",
              alignItems: "center",
            }}
          >
            <FormControlLabel
              control={
                addonCategory.isRequired ? (
                  <Radio
                    color="success"
                    checked={
                      selectedAddonIds.find((addonId) => addonId === addon.id)
                        ? true
                        : false
                    }
                    onChange={() => {
                      const addonIds = addons.map((item) => item.id);
                      const others = selectedAddonIds.filter(
                        (item) => !addonIds.includes(item)
                      );
                      setSelectedAddonIds([...others, addon.id]);
                    }}
                  />
                ) : (
                  <Checkbox
                    color="success"
                    checked={
                      selectedAddonIds.find((addonId) => addonId === addon.id)
                        ? true
                        : false
                    }
                    onChange={(evt, value) => {
                      if (value) {
                        setSelectedAddonIds([...selectedAddonIds, addon.id]);
                      } else {
                        const selected = selectedAddonIds.filter(
                          (selectedAddonId) => selectedAddonId !== addon.id
                        );
                        setSelectedAddonIds(selected);
                      }
                    }}
                  />
                )
              }
              label={addon.name}
            />
            <Typography sx={{ fontStyle: "italic" }}>{addon.price}</Typography>
          </Box>
        );
      })}
    </Box>
  );
};

export default Addons;
```

## ရှင်းလင်းချက်

```js
const addonCategory = useAppSelector((state) => state.addonCategory.items).find(
  (item) => item.id === addonCategoryId
);
```

- props အနေနဲ့ ၀င်လာတဲ့ addon category id ကို သုံးပြီး valid ဖြစ်တဲ့ category ကို storeထဲကနေ လှမ်းယူလိုက်ပါတယ်

```js
const addons = useAppSelector((state) => state.addon.items).filter(
  (item) => item.addonCategoryId === addonCategoryId
);
```

- props အနေနဲ့ ၀င်လာတဲ့ addon category id ကို သုံးပြီး သူနဲ့ချိတ်ထားတဲ့ addon တွေကို ထပ်ယူလိုက်ပါတယ်

```js
<FormControlLabel
              control={
                addonCategory.isRequired ? (
                  <Radio
                   ............
                   ..............
                   .....................
                  />
                ) : (
                  <Checkbox
                    .............
                    ................
                    ......................
                  />
                )
              }
              label={addon.name}
            />
```

- formControlLabel ကို သုံးပြီး addon တွေကို ပြပေးထားပါတယ်
- reqiured ဖြစ် တဲ့ category တွေက addon တွေကို radio box နဲ့ ပြပေးလိုက်မှာဖြစ်ပြီး
- reqiured မဖြစ် တဲ့ category တွေက addon တွေကို check box နဲ့ ပြပေးလိုက်မှာဖြစ်ပါတယ်

```js
<Radio
  color="success"
  checked={
    selectedAddonIds.find((addonId) => addonId === addon.id) ? true : false
  }
  onChange={() => {
    const addonIds = addons.map((item) => item.id);
    const others = selectedAddonIds.filter((item) => !addonIds.includes(item));
    setSelectedAddonIds([...others, addon.id]);
  }}
/>
```

- radio box နဲ့ ပြထားတဲ့ addon တွေက required ဖြစ်တာမလို့ တစ်ခု ကို မဖြစ်မနေ ရွေးပေးရမှာဖြစ်သလို တစ်ခုပဲ ရွေးလို့ရအောင် သတ်မှတ်ပေးရမှာဖြစ်ပါတယ်
- addon တစ်ခုခု ကို ရွေးလိုက်တိုင်း
  - လက်ရှိ loop လုပ်နေတဲ့ addon category နဲ့ ချိတ်ထားတဲ့ addon id တွေ အရင်ယူလိုက်ပြီး
  - selectedAddonIds မှာ ရှိနေတဲ့ itemတွေထဲကနေ လက်ရှိ loop လုပ်နေတဲ့ addon category နဲ့ ချိတ်ထားတဲ့ addon id တွေကို ဖယ်ထုတ်ပြီး ကျန်တာတွေကို others အနေနဲ့ ပြန်သိမ်းလိုက်ပါတယ်
  - others နဲ့ ရွေးလိုက်တဲ့ addon ကို ပြန်ပေါင်းလိုက်ပြီး selectedAddonIds အနေနဲ့ update လုပ်ပေးလိုက်တာပဲ ဖြစ်ပါတယ်

```js
                <Checkbox
                    color="success"
                    checked={
                      selectedAddonIds.find((addonId) => addonId === addon.id)
                        ? true
                        : false
                    }
                    onChange={(evt, value) => {
                      if (value) {
                        setSelectedAddonIds([...selectedAddonIds, addon.id]);
                      } else {
                        const selected = selectedAddonIds.filter(
                          (selectedAddonId) => selectedAddonId !== addon.id
                        );
                        setSelectedAddonIds(selected);
                      }
                    }}
                  />
                )
              }
              label={addon.name}
            />
```

- required မဟုတ်တဲ့ addon တွေကျတော့ selected လုပ်ထားရင် ဖြုတ်ပေးလိုက်ပြီး select လုပ်လိုက်ရင်တော့ setSelectedAddonIds ထဲ ထည့်ပေးလိုက်တာပဲဖြစ်ပါတယ်

```js
<Typography sx={{ fontStyle: "italic" }}>{addon.price}</Typography>
```

- ဒါကတော့ ေစျးနှုန်းတွေ ပြပေးလိုက်တာပဲဖြစ်ပါတယ်

### အချုပ်ပြောရရင် **required ဖြစ်တဲ့ addon တွေဆို category တစ်ခုအောက်မှာ addon တစ်ခုသာ select လုပ်လို့ရမှာဖြစ်ပြီး** _required မဟုတ်ရင်တော့ တစ်ခုထပ်မက ရွေးလို့ရမှာဖြစ်ပါတယ်_

> ဒီအပိုင်း record ကို အချိန်ပေးပြီးကြည့်ကာ ဖြည်းဖြည်းချင်း စဥ်းစားကြည့်ကြဖို့ တိုက်တွန်းချင်ပါတယ်
> order ရဲ့ logic အဓိက အပိုင်းလည်းဖြစ်တာမလို့ နားလည်အောင် ပုံနဲ့ ဆွဲပြီး လုပ်တာပဲဖြစ်ဖြစ် တစ်ဆင့်ချင်းစီ ရေးထားတဲ့ code နဲ့ result ကို ချိတ်ဆက်တွေးတောပြီး လေ့ကျင့်ကြပါခင်ဗျာ
