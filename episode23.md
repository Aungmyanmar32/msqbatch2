## MSquare Programming Fullstack Course

### Batch 2

### Episode-_23_ Summary

## Mini e-commerce project (`part-2`)

### 1. Add search product

### 2 . Update Product detail page with `ADD TO CART` button

### 3 . Create cart page and cart slice

### 4 . Quantity and total price

##

### Search product

- product တွေ ရှာလို့ရအောင် home page မှာ search input တစ်ခု တည့်လိုက်ပါမယ်

```js
import ProductCard from "@/components/productCard";
import { useAppDispatch, useAppSelector } from "@/store/hooks";
import { fetchProducts } from "@/store/slices/productSlice";
import { AppBar, Box, TextField } from "@mui/material";
import { Product } from "@prisma/client";
import { ChangeEvent, useEffect, useState } from "react";

export default function Home() {
  const dispatch = useAppDispatch();
  const products = useAppSelector((state) => state.products.items);
  const [filteredProducts, setFilteredProducts] = useState<Product[]>([]);

  useEffect(() => {
    dispatch(fetchProducts());
  }, []);

  const handleSearch = (
    evt: ChangeEvent<HTMLInputElement | HTMLTextAreaElement>
  ) => {
    const searchText = evt.target.value.toLowerCase();
    const searchResult = products.filter((product) =>
      product.title.toLowerCase().includes(searchText)
    );
    setFilteredProducts(searchResult);
  };
  return (
    <Box>
      <Box sx={{ display: "flex", justifyContent: "center" }}>
        <TextField
          sx={{ width: 800, margin: "0 auto", mb: 2 }}
          placeholder="Search products.."
          onChange={handleSearch}
        />
      </Box>
      <Box sx={{ display: "flex", justifyContent: "center", flexWrap: "wrap" }}>
        {filteredProducts.map((product) => (
          <Box key={product.id} sx={{ mr: 5, mb: 3 }}>
            <ProductCard
              title={product.title}
              description={product.description}
              imageUrl={product.imageUrl}
            />
          </Box>
        ))}
      </Box>
    </Box>
  );
}

```

- filteredProducts ဆိုတဲ့ state တစ်ခု သတ်မှတ်ထားပါတယ်
- MUI text field ကို သုံးပြီး input ကနေ ထည့်လိုက်တဲ့ စာက product title ထဲမှာ ပါမပါ စစ်လိုက်ပြီး filteredProducts အဖြစ် update လုပ်လိုက်ပါတယ်
- render လုပ်တဲ့အခါလည်း filteredProducts ကိုပဲ map လုပ်ပြီး ပြပေးထားလိုက်ပါတယ်
- next app ကုိ run ကြည့်တဲ့ အခါ search box ကလွဲပြီး ဘာမှ မပြသေးတာကို ြမင်ရမှာဖြစ်ပါတယ်
- filteredProducts ရဲ့ တန်ဖိုးက ပထမအကြိမ် ၀င်လာလာချင်းမှာ empty array ဖြစ်နေတာမို့လို့ပါ
- useEffect ကို သုံးပြီး update လုပ်ပေးလိုက်ပါမယ်

![](https://cdn.discordapp.com/attachments/1146496852087287898/1158272897505296425/image.png?ex=651ba532&is=651a53b2&hm=454be050a430eb11f8329c9e24bb01a82d364679a94dba4b6a9f9f146734aeaf&)

- next app မှာ စမ်းသပ်ကြည့်ပါက products တွေ ပြပေးတာကို မြင်ရမှာဖြစ်ပြီး search box ထဲမှာ laptop လို့ ရိုက်ရှာကြည့်လိုက်ရင် title မှာ laptop ပါတဲ့ product တွေပဲ ပြပေးမှာဖြစ်ပါတယ်
- ဆက်ပြီး product တစ်ခုခုကို နှိပ်လိုက်ရင် အဲ့ဒီ product ရဲ့ detail ကို ပြပေးတဲ့ page ကို route လုပ်ပေးလို့ရအောင် Product card ကို Next Link နဲ့ wrap လုပ်ပေးလိုက်ပါမယ်
  ![](https://cdn.discordapp.com/attachments/1146496852087287898/1158278698550689802/image.png?ex=651baa99&is=651a5919&hm=963a13b72e507907d264cd380f5d084aa17b2f40fc0c4a176d6acb9dc4fcca19&)

- အပေါ်မှာ route လုပ်ထားတာကိုပြပေးနိုင်ဖို့ dynamic page တစ်ခု လုပ်လိုက်ပါမယ်
  ![](https://cdn.discordapp.com/attachments/1146496852087287898/1158280176975749171/image.png?ex=651babf9&is=651a5a79&hm=02ae0d09700ae04dfc86c2c548f978ca721d1fd5c2673381aeaca99475bd29a9&)

```js
// src/pages/product/[id]

import { useAppDispatch, useAppSelector } from "@/store/hooks";
import { Box, Button, Typography } from "@mui/material";
import { useRouter } from "next/router";

const ProductDetailPage = () => {
  const router = useRouter();
  const productId = Number(router.query.id);
  const products = useAppSelector((state) => state.products.items);
  const product = products.find((product) => product.id === productId);
  const dispatch = useAppDispatch();

  if (!product) return null;

  return (
    <Box sx={{ display: "flex", justifyContent: "center" }}>
      <Box sx={{ maxWidth: 900 }}>
        <Box sx={{ display: "flex", justifyContent: "center" }}>
          <Box
            sx={{
              display: "flex",
              alignItems: "center",
              maxWidth: 900,
            }}
          >
            <Box>
              <img src={product?.imageUrl || ""} width={500} />
            </Box>
            <Box sx={{ ml: 5 }}>
              <Typography variant="h4">{product?.title}</Typography>
              <Typography sx={{ my: 2 }}>{product?.description}</Typography>
              <Typography variant="h5">$ {product?.price}</Typography>
            </Box>
          </Box>
        </Box>
        <Box sx={{ display: "flex", justifyContent: "flex-end", mt: 2 }}>
          <Button variant="contained">Add to cart</Button>
        </Box>
      </Box>
    </Box>
  );
};

export default ProductDetailPage;
```

- product တစ်ခုခုကို နှိပ်ကြည့်လိုက်ပါက ခုလို ပြပေးမှာဖြစ်ပါတယ်

![](https://cdn.discordapp.com/attachments/1146496852087287898/1158280641918554202/image.png?ex=651bac68&is=651a5ae8&hm=2d2e1e54cd7677e31ef1d5afe96ab03ed7ed3b4f3c9dec322d85c94542571840&)

- ADD TO CART button ကို နှိပ်လိုက်တဲ့အချိန် cart ထဲမှာ product ကို သိမ်းလိုက်ပြီး Home page ပြန်ထွက်သွားလို့ရအောင် ဆက်လုပ်လိုက်ပါမယ်
- cart ထဲမှာ product တွေ သိမ်းနိုင်ဖို့ card slice တစ်ခုကို redux store ထဲမှာ သတ်မှတ်လိုက်ပါမယ်

```js
// src/store/slice/cartSlice.ts

import { CartSlice } from "@/types/cart";
import { createSlice } from "@reduxjs/toolkit";

const initialState: CartSlice = {
  items: [],
  isLoading: false,
  error: null,
};

const cartSlice = createSlice({
  name: "cart",
  initialState,
  reducers: {
    addToCart: (state, action) => {
      state.items = [...state.items, action.payload];
    },
  },
});

export const { addToCart } = cartSlice.actions;
export default cartSlice.reducer;
```

```js
// src/types/cart.ts

import { Product } from "@prisma/client";

interface CartItem extends Product {
  quantity: number;
}

export interface CartSlice {
  items: CartItem[];
  isLoading: boolean;
  error: Error | null;
}
```

```js
// src/store/index.ts

import { configureStore } from "@reduxjs/toolkit";
import cartReducer from "./slices/cartSlice";
import productReducer from "./slices/productSlice";

export const store = configureStore({
  reducer: {
    products: productReducer,
    cart: cartReducer,
  },
});

// Infer the `RootState` and `AppDispatch` types from the store itself
export type RootState = ReturnType<typeof store.getState>;
// Inferred type: {posts: PostsState, comments: CommentsState, users: UsersState}
export type AppDispatch = typeof store.dispatch;
```

- cart slice တစ်ခု create လုပ်ထားပြီး items တွေ အနေနဲ့ product tpye တွေ အကုန် extends လုပ်ပြီး quantityကို ထပ်ထည့်ထားတဲ့ array အဖြစ် သတ်မှတ်ထားလိုက်ပါတယ်
- addToCart action မှာတော့ မူလ array ကို copy လုပ်ပြီး ၀င်လာတဲ့ payload ကို ထပ်ထည့်ထားလိုက်ပါတယ်
- ADD TO CART ကို နှိပ်လိုက်ချိန်မှာ အပေါ်က action ကို dispatch လုပ်လိုက်မှာဖြစ်ပါတယ်

![](https://cdn.discordapp.com/attachments/1146496852087287898/1158283696944726016/image.png?ex=651baf41&is=651a5dc1&hm=489f51e1e881a0ef0e21d8d8f8033a040aaca4213b458fbd079ccebaacddd502&)

- onClick မှာ addToCart action ကို dispatch လုပ်လိုက်ပြီး payload အနေနဲ့ လက်ရှိproduct နဲ့အတူ quantity ကိုပါ ထည့်ပေးလိုက်ပါတယ်
- next router ကို သုံးပြီး router.push နဲ့ home page ကို route လုပ်ပေးထားပါတယ်
- ခု product တစ်ခု ADD TO CART လုပ်ကြည့်ပါက cart store မှာ update ဖြစ်ပြီး Home page ကို ပြန်ရောက်သွားမှာဖြစ်ပါတယ်

![](https://cdn.discordapp.com/attachments/1146496852087287898/1158284976178081862/image.png?ex=651bb072&is=651a5ef2&hm=9f7eed14de5d55ebd79c61e9acd02a224a38510c6ab825afab4558d780d45986&)

- cart store ထဲမှာရှိတဲ့ item အရေအတွက်ကို home page မှာ ပြပေးနိုင်အောင် လုပ်လိုက်ပါမယ်

![](https://cdn.discordapp.com/attachments/1146496852087287898/1158286748921634846/image.png?ex=651bb218&is=651a6098&hm=5239434e48a00c719bfc6a5ba404287e67b89f10ccba89c0545fa2c103b87c90&)

- store ထဲက cartitems တွေ လှမ်းယူလိုက်ပြီး အရေအတွက် ကို ShoopingCartIcon ဘေးမှာ ပြပေးလိုက်တာပဲ ဖြစ်ပါတယ်

- product တစ်ခုကို ၀င်ပြီး ADD ကြည့်လိုက်ပါက homepage ကို ပြန်ရောက်လာတဲ့အခါ cart icon ဘေးမှာ 1 လို့ ပြပေးနေမှာဖြစ်ပါတယ်

-cart ထဲ ထည့်ထားတဲ့ product အားလုံးကို ပြပေးနိုင်ဖို့ cart page တစ်ခု create ဆက်လုပ်ပါမယ်

```js
// src/pages/cart/index.tsx

import { useAppDispatch, useAppSelector } from "@/store/hooks";
import AddCircleOutlineIcon from "@mui/icons-material/AddCircleOutline";
import RemoveCircleOutlineIcon from "@mui/icons-material/RemoveCircleOutline";
import { Box, Button, Typography } from "@mui/material";

const Cart = () => {
  const cartItems = useAppSelector((state) => state.cart.items);
  const dispatch = useAppDispatch();

  const getCartTotalPrice = () => {
    let totalPrice = 0;
    cartItems.forEach((item) => (totalPrice += item.price));
    return totalPrice;
  };

  return (
    <Box>
      <Box sx={{ display: "flex", justifyContent: "center" }}>
        {cartItems.length ? (
          <Box>
            {cartItems.map((item) => (
              <Box
                sx={{
                  display: "flex",
                  alignItems: "center",
                  justifyContent: "space-between",
                  mt: 5,
                }}
              >
                <Box>
                  <Box sx={{ display: "flex", alignItems: "center" }}>
                    <img src={item.imageUrl || ""} width={150} />
                    <Box sx={{ ml: 3 }}>
                      <Typography variant="h6">{item.title}</Typography>
                      <Typography variant="h6">{item.price}</Typography>
                    </Box>
                  </Box>
                </Box>
                <Box sx={{ ml: 5 }}>
                  <Box sx={{ display: "flex", alignItems: "center" }}>
                    <RemoveCircleOutlineIcon
                      sx={{ fontSize: 40, color: "green", cursor: "pointer" }}
                    />
                    <Typography sx={{ mx: 2 }} variant="h4">
                      {item.quantity}
                    </Typography>
                    <AddCircleOutlineIcon
                      sx={{ fontSize: 40, color: "green", cursor: "pointer" }}
                    />
                  </Box>
                </Box>
              </Box>
            ))}
          </Box>
        ) : (
          <Typography variant="h1">Empty cart</Typography>
        )}
      </Box>
      {cartItems.length > 0 && (
        <Box sx={{ display: "flex", justifyContent: "center" }}>
          <Box
            sx={{
              display: "flex",
              flexDirection: "column",
              justifyContent: "center",
              alignItems: "center",
              mt: 5,
            }}
          >
            <Typography variant="h3">
              Total price: {getCartTotalPrice()}
            </Typography>
            <Button variant="contained" sx={{ width: "fit-content", my: 3 }}>
              Confirm order
            </Button>
          </Box>
        </Box>
      )}
    </Box>
  );
};

export default Cart;
```

- cart ထဲရှိတဲ့ item တွေကို ပြပေးထားပြီး total price ကို တွက်ပေးထားလိုက်ပါတယ်
- product တွေရဲ့ quantity ကို တိုး/လျော့ လုပ်လို့ရအောင် button နှစ်ခုနဲ့ quantity ကို ဘေးမှာ ပြပေးထားလိုက်ပါတယ်
- `+` ကို နှိပ်လိုက်ရင် ၁ တိုးပေးသွားမှာဖြစ်ပြီး `-` ကိုနှိပ်ရင်တော့ ၁ လျော့ ပေးမှာဖြစ်ပါတယ်
- quantity က 0 ဖြစ်ပြီး ဆိုရင်တော့ item ကို cart ထဲကနေ ဖယ်ထုတ်ပေးလိုက်မှာဖြစ်ပါတယ်
- quantity ကို update လုပ်နိုင်ဖို့ cartSlice ထဲမှာ action တစ်ခု create လုပ်လိုက်ပါမယ်

```js
// src/sotre/slice/cartSlice.ts --> reducers

updateQuantity: (state, action) => {
        const quantity = action.payload.quantity;
        if (quantity === 0) {
          state.items = state.items.filter(
            (item) => item.id !== action.payload.id
          );
        } else {
          state.items = state.items.map((item) =>
            item.id === action.payload.id
              ? { ...item, quantity: action.payload.quantity }
              : item
          );
        }
      },

```

- payload အနေနဲ့ ၀င်လာတဲ့ product ရဲ့ quantity က 0 ဖြစ်ခဲ့ရင် အဲဒီ product ကို ဖယ်ထုတ်ပြီး cart ထဲမှာ ကျန်တဲ့ product ကိုပဲ ပြန်ထည့်ပေးလိုက်တာဖြစ်ပါတယ်
- မဟုတ်ရင်တော့ ၀င်ထာတဲ့ product id ကို စစ် ပြီး quantity ကို update လုပ်ပေးလိုက်တာပဲ ဖြစ်ပါတယ်
- cart page ထဲက + / - ခလုတ်တွေမှာ အထက်ပါ actionကို dispatch လုပ်ပြီး cart store ကို update လုပ်မှာဖြစ်ပါတယ်

![](https://cdn.discordapp.com/attachments/1146496852087287898/1158293616800837722/image.png?ex=651bb87e&is=651a66fe&hm=daa270803709310e31e0e304e056bb1194540e91df4a8d770a33cf5ba564fdc4&)

- increase နဲ့ decrase function နှစ်ခု လုပ်လိုက်ပြီး item id နဲ့ quantity ကို parameter အဖြစ် ထည့်ပြီး updateQuantity action ကို dispatch လုပ်ပေးလိုက်တာဖြစ်ပါတယ်
- ခု next app မှာ products တွေ cart ထဲထည့်ပြီး cart page မှာ quantity တွေ update လုပ်လို့ရပြီး ဖြစ်ပါတယ်
- quantity တွေ update ဖြစ်လာပေမယ့် total price က update မဖြစ်နေသေးတာမလို့ ဆက်ပြီး လုပ်ရပါမယ်
- getTotalPrice function မှာ total price ကို quantity နဲ့ မြှောက် ပေးလိုက်ရင် ရပြီး ဖြစ်ပါတယ်

```js
const getCartTotalPrice = () => {
  let totalPrice = 0;
  cartItems.forEach((item) => (totalPrice += item.price * item.quantity));
  return totalPrice;
};
```

##

### [All source code HERE!! ( github repo )](https://github.com/msquarefdc/mini-commerce/tree/main)
