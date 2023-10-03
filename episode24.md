## MSquare Programming Fullstack Course

### Batch 2

### Episode-_23_ Summary

## Mini e-commerce project (`part-3`)

### 1 . Comfirm Order

### 2 . Create Order in database

##

![](https://cdn.discordapp.com/attachments/1146496852087287898/1158647799588593684/image.png?ex=651d025a&is=651bb0da&hm=dda595a9fa864d57c9fcb92230d5e3f222d17c5352372a66ad7f5e66fa99bad1&)

- CONFIRM ORDER လုပ်လိုက်ချိန်မှာ cart ထဲက Item တွေနဲ့ quantity ကို သုံးပြီး order တစ်ခု crate လုပ်လို့ရအောင် ဆက်လုပ်ကြပါမယ်

- cartSlice ထဲမှာ async action တစ်ခုလုပ်ပြီး order confirm လုပ်လိုက်တာနဲ့ server request လုပ်ပြီး database မှာ order data တွေ သွားသိမ်းလိုက်မှာဖြစ်ပါတယ်

```js
// src/store/slices/cartSlice--> async thunk action

export const confirmOrder = createAsyncThunk(
  "cart/confirmOrder",
  async (payload: CartItem[], thunkApi) => {
    const response = await fetch(`${config.apiBaseUrl}/order`, {
      method: "POST",
      headers: {
        "content-type": "application/json",
      },
      body: JSON.stringify(payload),
    });
  }
);
```

- /order route ကို request လုပ်ပြီး cartitems တွေ ပို့ပေးလိုက်တာဖြစ်ပါတယ်
- api folder အောက်မှာ order route တစ်ခုလုပ်ပြီး data တွေ လက်ခံကာ database မှာ သိမ်းမှာဖြစ်ပါတယ်

```js
// src/pages/api/order/index.ts

// Next.js API route support: https://nextjs.org/docs/api-routes/introduction
import { CartItem } from "@/types/cart";
import { prisma } from "@/utils/db";
import { OrderStatus, Product } from "@prisma/client";
import type { NextApiRequest, NextApiResponse } from "next";

export default async function handler(
  req: NextApiRequest,
  res: NextApiResponse
) {
  const method = req.method;
  if (method === "POST") {
    const cartItems = req.body as CartItem[];

    const cartItemIds = cartItems.map((item) => item.id);

    const products = await prisma.product.findMany({
      where: { id: { in: cartItemIds } },
    });

    const getProductPriceWithQuantity = (item: CartItem) => {
      const product = products.find(
        (product) => product.id === item.id
      ) as Product;
      return product.price * item.quantity;
    };

    let totalPrice = 0;

    cartItems.forEach((item) => {
      const price = getProductPriceWithQuantity(item);
      totalPrice += price;
    });

//create order
    const createdOrder = await prisma.order.create({
      data: { status: OrderStatus.ORDERED, totalPrice: totalPrice },
    });

    const orderId = createdOrder.id;

//create order lines
    cartItems.forEach(async (item) => {
      const data = { orderId, productId: item.id, quantity: item.quantity };
      await prisma.orderline.create({ data });
    });

    return res.status(200).json({ orderId, status: OrderStatus.ORDERED });
  }
  res.status(405).send("Method not allowed.");
}

```

> ရှင်းလင်းချက်

```js
export default async function handler(
  req: NextApiRequest,
  res: NextApiResponse
) {
  const method = req.method;
  if (method === "POST") {.........}
}
```

- next api function တစ်ခု သတ်မှတ်ထားပြီး POST method ကို လက်ခံထားလိုက်ပါတယ်

```js
const cartItems = req.body as CartItem[];

const cartItemIds = cartItems.map((item) => item.id);
```

- request body ထဲက cartItems တွေကို လက်ခံလိုက်ပြီး item တွေရဲ့ id တွေကို cartItemIds array အဖြစ် သိမ်းလိုက်ပါတယ်

```js
const products = await prisma.product.findMany({
  where: { id: { in: cartItemIds } },
});
```

- သိမ်းထားတဲ့ cartItemIds array ကို သုံးပြီး database ထဲက product table ကနေ product တွေကိုပြန်ရှာလိုက်ပါတယ်

### Never trust client principle

- frontend ကနေပို့လာတဲ့ items(product) တွေကို သုံးပြီး database ထဲမှာ order အနေနဲ့ သိမ်းလို့ရတာကို ဘာကြောင့် product တွေ database ကနေပြန်ရှာနေရတာလဲ?
- အဓိက ကတော့ total price ကို ပြန်တွက်ချင်လို့ဖြစ်ပါတယ်
- client ဘက်ကနေ data ( in this case "**price**") တွေကို changes လုပ်ပြီး server ဆီကို ပို့လာနိုင်တဲ့အတွက် စိတ်ချရတဲ့ database ထဲက data တွေနဲ့ ပဲ အလုပ်လုပ်ရမှာ ဖြစ်ပါတယ်
- အောက်မှာတော့ ပြန်ရှာထားတဲ့ product နဲ့ client ကပို့လာတဲ့ items တွေက quantity နဲ့ total price ကို ပြန်တွက် ထားတာပဲ ဖြစ်ပါတယ်

```js
    const getProductPriceWithQuantity = (item: CartItem) => {
      const product = products.find(
        (product) => product.id === item.id
      ) as Product;
      return product.price * item.quantity;
    };

    let totalPrice = 0;

    cartItems.forEach((item) => {
      const price = getProductPriceWithQuantity(item);
      totalPrice += price;
    });
```

- getProductPriceWithQuantity function မှာတော့ parameter တစ်ခု လက်ခံထားပြီး အထက်မှာရှာထားတဲ့ products array ကနေ product တစ်ခုချင်းစီကို ပြန်ထုတ်လိုက်ကာ return တန်ဖိုးအနေနဲ့ ထုတ်ထားတဲ့ product ရဲ့ price ကို parameter ကနေ ပါလာမယ့် quantity နဲ့ မြှောက်ပေးထားတာဖြစ်ပါတယ်
- နောက်ထပ် totalPrice တစ်ခု သတ်မှတ်လိုက်ပြီး
- request နေ ၀င်လာတဲ့ cartItems တွေကို loop လုပ်ကာ totalPrice ကို update လုပ်ပေးထားလိုက်တာပဲဖြစ်ပါတယ်

```js
const createdOrder = await prisma.order.create({
  data: { status: OrderStatus.ORDERED, totalPrice: totalPrice },
});

const orderId = createdOrder.id;
```

- Order status နဲ့ total price ကို သုံးပြီး database က order table order row တစ်ခု create လုပ်လိုက်ပါတယ်
- create လုပ်ပြီးရလာတဲ့ row က id ကို orderId အဖြစ် သိမ်းလိုက်ပါတယ်

```js
cartItems.forEach(async (item) => {
  const data = { orderId, productId: item.id, quantity: item.quantity };
  await prisma.orderline.create({ data });
});
```

- orderId ရယ် လက်ရှိ loop လုပ်နေတဲ့ item id/quantity တွေကို သုံးပြီး cart item တစ်ခုချင်းစီအတွက် order line တစ်ခုစီကို orderline table ထဲမှာ create လုပ်လိုက်တာပဲဖြစ်ပါတယ်

```js
return res.status(200).json({ orderId, status: OrderStatus.ORDERED });
```

- နောက်ဆုံးအနေနဲ့ orderId နဲ့ order status ကို response ပြန်ပေးလိုက်တာပဲဖြစ်ပါတယ်

##

### cart page က confirm ခလုတ်ကို နှိပ်လိုက်တဲ့အခါ slice ထဲက thunk action ကို dispatch လုပ်ပြီး confirmation page ကို routing လုပ်ပေးလိုက်မှာဖြစ်ပါတယ်

![](https://cdn.discordapp.com/attachments/1146496852087287898/1158664030769123379/image.png?ex=651d1177&is=651bbff7&hm=323a85a3dc74da1f7bfb8ad5d2805bd6533bc6cdf6b2830f9e0ca0ba8f716159&)

- route လုပ်ထားတဲ့ confirmation မှာ ပြပေးနိုင်ဖို့ confirmation page တစ်ခု ကို လုပ်လိုက်ပါမယ်

```js
// src/pages/confirmation/index.tsx

import React from "react";

const ConfirmationPage = () => {
  return <h1>ConfirmationPage</h1>;
};

export default ConfirmationPage;
```

- လောလောဆယ် မှာတော့ h1 tag တစ်ခုပဲ ပြပေးထားပါတယ်
- နောက် သင်ခန်းစာတွေမှာတော့ order id နဲ့ status တွေကို ပြပေးနိုင်အောင် ဆက်လုပ်သွားမှာဖြစ်ပါတယ်
