## MSquare Programming Fullstack Course

### Batch 2

### Episode-_25_ Summary

## Mini e-commerce project (`part-4`)

### 1 . Review Order

### 2 . Cancel Order

##

### dispatch action with function paylaod

- comfirm order ကို နှိပ်လိုက်ချိန်မှာ dispatch လုပ်တဲ့ action ကို ခေါ်တဲ့အချိန် payload parameter အနေနဲ့ data တွေတင် မကပဲ function တွေကိုပါ ထည့်ပေးလိုက်လို့ရပါတယ်
- action မှာ payload ကို လက်ခံတဲ့အခါ ပါလာတဲ့ function တွေကို ဆက်ပြီး run ခိုင်းလို့ရပါတယ်
- ခု payload အနေနဲ့ ထည့်ပေးမယ့် function တွေကို cart page component မှာသတ်မှတ်မှာဖြစ်ပါတယ်

```js
// src/pages/cart/index.ts --->
const onSuccess = (data: any) => {
  console.log(data);
};

const onError = () => {};

const handelComfirm = async () => {
  dispatch(confirmOrder({ payload: cartItems, onSuccess, onError }));
};
```

- cart page component မှာ onSuccess နဲ့ onError ဆိုတဲ့ function နှစ်ခုသတ်မှတ်လိုက်ပြီး confirmOrder action ကို dispatch လုပ်တဲ့အခါ parameter အနေနဲ့ ထည့်ပေးလိုက်တာဖြစ်ပါတယ်
- onSuccess function မှာတော့ data ဆိုတဲ့ parameter တစ်ခုကို လက်ခံထားပြီး log ထုတ်ကြည့်ထားပါတယ်
- confirmOrder action မှာလည်း ထည့်ပေးလိုက်တဲ့ action တွေကို သုံးပြီး ORDER CREATE လုပ်ကာ data တွေ ပြန်ထည့်ပေးလိုက်မှာဖြစ်ပါတယ်
- type folder ထဲမှာလည်း cartType တွေကို update လုပ်ပေးရပါမယ်

```js
// src/types/cartType.ts

import { Product } from "@prisma/client";

export interface CartItem extends Product {
  quantity: number;
}

export interface CartSlice {
  items: CartItem[];
  isLoading: boolean;
  error: Error | null;
}

export interface BaseOption {
  onSuccess?: (data?: any) => void;
  onError?: (data?: any) => void;
}

export interface CreateOrderOptions extends BaseOption {
  payload: CartItem[];
}

export interface CancelOrderOptions extends BaseOption {
  orderId: string;
}
```

- slice ထဲက action ကို UPDATE လုပ်ပါမယ်

```js
// src/store/slices/cartSlice.ts --> confimOrder action

export const confirmOrder = createAsyncThunk(
  "cart/createOrder",
  async (options: CreateOrderOptions, thunkApi) => {
    const { payload, onSuccess, onError } = options;
    try {
      const response = await fetch(`${config.apiBaseUrl}/order`, {
        method: "POST",
        headers: {
          "content-type": "application/json",
        },
        body: JSON.stringify(payload),
      });
      const dataFromServer = await response.json();
      onSuccess && onSuccess(dataFromServer);
    } catch (err) {
      onError && onError(err);
    }
  }
);
```

- ပြီးခဲ့တဲ့ သင်ခန်းစာမှာလုပ်ထားခဲ့တဲ့ confimOrder action ကို အနည်းငယ် ပြင်ရေးလိုက်တာပဲဖြစ်ပါတယ်

```js
async (options: CreateOrderOptions, thunkApi) => {....}
```

- ပထမ parameter နေရာမှာ action ကို dispatch လုပ်ချိန်မှာ ထည့်ပေးလိုက်တဲ့ data ကို options အနေနဲ့ လက်ခံလိုက်ပါတယ်

```js
const { payload, onSuccess, onError } = options;
```

- options ထဲကမှ payload dataနဲ့ function တွေကို ပြန်ထုတ်ယူလိုက်ပါတယ်

```js
 try {
      const response = await fetch(`${config.apiBaseUrl}/order`, {
        method: "POST",
        headers: {
          "content-type": "application/json",
        },
        body: JSON.stringify(payload),
      });
      const dataFromServer = await response.json();
      onSuccess && onSuccess(dataFromServer);
    }
```

- serverဆီ request လုပ်လိုက်ပြီး payload ကို request body အနေနဲ့ ပြန်ထည့်ပေးလိုက်ပါတယ်
- response ြန်လာတဲ့ data ကို dataFromServer အဖြစ် သိမ်းလိုက်ပါတယ်
- onSuccess ဆိုတဲ့ function ကို run လိုက်ပြီး parameter အနေနဲ့ ထည့်ပေးထားတာပဲဖြစ်ပါတယ်

```js
catch (err) {
      onError && onError(err);
    }
```

- error ဖြစ်ခဲ့ရင်တော့ onError function ကို run လိုက်တာပဲ ဖြစ်ပါတယ်

- ခု next app မှာ productတွေ add ပြီး comfirm order လုပ်ကြည့်ပါက onSuccess funtion မှာ ၀င်လာတဲ့ data တွေကို ထုတ်ပေးမှာဖြစ်ပါတယ်

![](https://cdn.discordapp.com/attachments/1146496852087287898/1159187195278663730/image.png?ex=651ef8b4&is=651da734&hm=0bd8cc4fc00a7383f578240a792d899e075d5887226efb535a6b1cbf1b14f134&)

### confirm လုပ်လိုက်တဲ့အချိန်မှာ confirmation page ကို route လုပ်လိုက်ပြီး ရလာတဲ့ data တွေကိုပါ query string အနေနဲ့ ပို့ပေးလိုက်မှာဖြစ်ပါတယ်

##

### what is quary string?

![](https://static.semrush.com/blog/uploads/media/ca/37/ca3737d8edb5cf079aaf1f11ae01d286/mMREmiwXSrryVyv8IxbVFgje7ICFhfqWnca7W0db36KjX35vzLTnStkFynEd9NBoXXH-JYKCm2deskxgLo_vxzEvl-qLpVOgtwc78rhFI2Rm1pBK-j2SwMKWz0CXU42STjzUVcF1vaRTFbw_6wvH_5A.png)

- htpps//www.domain.com/page ဆိုတာ route တစ်ခုဖြစ်ပြီး ? နောက်မှာ ထည့်ပေးလိုက်တာတွေက query string ဖြစ်ပါတယ်
- query string တစ်ခုမှာ key နဲ့ value ရှိကြပါတယ်
  `htpps//www.domain.com/page?productId=12`
- အပေါ်က နမူနာမှာ `productId=12` က query string တစ်ခုဖြစ်ပြီး query ထဲက `productId` လို့ ယူလိုက်ရင် `"12"` ဆိုပြီး ထွက်လာမှာဖြစ်ပါတယ်
- query string တစ်ခုထပ်ပိုပြီး ထည့်ချင်ရင်တော့ & နဲ့ ဆက်ထည့်ပေးလေ့ရှိပါတယ်
- `htpps//www.domain.com/page?productId=12&oderId=3`

##

### confirm လုပ်လိုက်တဲ့အချိန်မှာ confirmation page ကို route လုပ်လိုက်ပြီး ရလာတဲ့ data တွေကိုပါ query string အနေနဲ့ ပို့ပေးလိုက်မှာဖြစ်ပါတယ်

```js
// src/pages/cart/index.tsx --> onSuccess function

const onSuccess = (data: any) => {
  router.push(`/confirmation?orderId=${data.orderId}&status=${data.status}`);
};
```

- confirm လုပ်လိုက်တဲ့အချိန်မှာ confirmation page ကို route လုပ်လိုက်ပြီး ရလာတဲ့ dataတွေ ထဲက orderId နဲ့ status ကို qurey အဖြစ် ထည့်ပေးလိုက်တာဖြစ်ပါတယ်
- confirmation pageမှာတော့ query ထဲက dataတွေကို သုံးပြီး order ကို ပြန်ပြပေးမှာဖြစ်ပါတယ်

```js
// src/pages/confirmation/index.tsx

import { useAppDispatch } from "@/store/hooks";
import { Alert, Box, Button, Snackbar, Typography } from "@mui/material";
import { useRouter } from "next/router";
import { useState } from "react";

const OrderConfirmation = () => {
  const [open, setOpen] = useState(false);
  const router = useRouter();
  const orderId = router.query.orderId as string;
  const status = router.query.status as string;
  const dispatch = useAppDispatch();

  return (
    <Box
      sx={{
        width: "100%",
        height: "95vh",
        justifyContent: "center",
        display: "flex",
        alignItems: "center",
        flexDirection: "column",
      }}
    >
      <Typography variant="h4">Order: {orderId}</Typography>
      <Typography variant="h6">Status: {status}</Typography>
      <Box sx={{ mt: 2 }}>
        <Button variant="contained">Cancel order</Button>
      </Box>
    </Box>
  );
};

export default OrderConfirmation;

```

- query ထဲက order id နဲ့ status ကို ထုတ်ယူလိုက်ပြီး `cancel order` botton နဲ့ အတူ render လုပ်ကာ ပြပေးထားလိုက်တာပဲဖြစ်ပါတယ်

![](https://cdn.discordapp.com/attachments/1146496852087287898/1159192909917204541/image.png?ex=651efe06&is=651dac86&hm=a12812c7fbd9a5c9334cafea51b2be5ea5ec3742492fc100091eed3f6eec69f8&)

##

### ဆက်ပြီး order cancel လုပ်လို့ရမယ့် action တစ်ခုကို cancel order ခလုတ်ကို နှိပ်လိုက်ချိန်မှာ dispatch လုပ်လိုက်မှာဖြစ်ပါတယ်

![](https://cdn.discordapp.com/attachments/1146496852087287898/1159195095325741086/image.png?ex=651f000f&is=651dae8f&hm=31526b4a3b0e7b9acca7c24bc0230a6a02f5fd3b1af1b8c9e8dfe691214b815c&)

- cancel ကို နှိပ်လိုက်ချိန်မှာ cancelOrder ဆိုတဲ့ action ကို dispatch လုပ်လိုက်တာပဲဖြစ်ပါတယ်
- cancelOrder ဆိုတဲ့ action ကို cartSlice.ts ထဲမှာ သတ်မှတ်ပါမယ်

```js
// src/store/slices/cartSlice.ts ---> add new action

export const cancelOrder = createAsyncThunk(
  "cart/cancelOrder",
  async (options: CancelOrderOptions, thunkApi) => {
    const { orderId, onSuccess, onError } = options;
    try {
      await fetch(`${config.apiBaseUrl}/order/${orderId}`, {
        method: "DELETE",
      });
      onSuccess && onSuccess();
    } catch (err) {
      onError && onError(err);
    }
  }
);
```

- comfirmOrder action ကလိုပဲ ၀င်လာတဲ့ options payload ထဲက orderId, onSuccess, onError တွေကို ထုတ်ယူထားလိုက်ပါတယ်
- /order/:id ဆိုတဲ့ dynamic route နဲ့ DELETE method အနေဖြင့် request လုပ်လိုက်ပါတယ်
- ပြီးရင် onSuccess function ကို run လိုက်တာပဲ ဖြစ်ပါတယ်
- api server မှာ /order/:id ဆိုတဲ့ dynamic route နဲ့ request ကို လက်ခံပေးရမှာဖြစ်ပါတယ်

![](https://cdn.discordapp.com/attachments/1146496852087287898/1159199743583461496/image.png?ex=651f0463&is=651db2e3&hm=4680613f0ebc0ea2c61aea553811aafdc48a9b854575f350b4039158f157bfa8&)

```js
// src/pages/api/order/[id].ts

import { prisma } from "@/utils/db";
import type { NextApiRequest, NextApiResponse } from "next";

export default async function handler(
  req: NextApiRequest,
  res: NextApiResponse
) {
  const method = req.method;
  console.log(method);

  if (method === "DELETE") {
    const query = req.query;

    const orderId = Number(query.id);
    if (!orderId) return res.status(400).send("Bad request.");

    const isFound = await prisma.order.findFirst({ where: { id: orderId } });
    if (!isFound) return res.status(400).send("Bad request.");

    await prisma.orderline.deleteMany({ where: { orderId } });
    await prisma.order.deleteMany({ where: { id: orderId } });

    return res.status(200).send("OK");
  }
  res.status(405).send("Method not allowed.");
}
```

> ရှင်းလင်းချက်

```js
if (method === "DELETE") {
    const query = req.query;
```

- delete method ဖြစ်ခဲ့ရင် request object ထဲက query ( dynamic အနေနဲ့ ၀င်လာတဲ့ id)ကို လှမ်းယူလိုက်ပါတယ်
-

```js
const orderId = Number(query.id);
if (!orderId) return res.status(400).send("Bad request.");
```

- ၀င်လာတဲ့ query ကို number ဟုတ်မဟုတ်စစ်လိုက်ပြီး number မဟုတ်ခဲ့ရင် bad request ကို response ပြန်ပြီး return လုပ်ပေးလိုက်ပါတယ်
-

```js
const isFound = await prisma.order.findFirst({ where: { id: orderId } });
if (!isFound) return res.status(400).send("Bad request.");
```

- ဆက်ပြီး ရလာတဲ့ query ထဲက id က database ထဲရှိ order id နဲ့ တူတာရှိမရှိ ထပ်စစ်လိုက်ပြီး တူတာမရှိရင် bad request ကို response ပြန်ပြီး return လုပ်ပေးလိုက်ပါတယ်
-

```js
await prisma.orderline.deleteMany({ where: { orderId } });
await prisma.order.deleteMany({ where: { id: orderId } });
```

- data တွေ မှန်မမှန် စစ်ပြီးတဲ့နောက် database ထဲရှိ orderLine table ထဲရှိ row(data) တွေအနက်မှ ၀င်လာတဲ့ order id တွေရှိတဲ့ rows တွေကို အရင် delete လုပ်လိုက်တာဖြစ်ပါတယ်
- ေနာက်ထပ်လဲ order table ထဲမှ row တွေထဲက ၀င်လာတဲ့ id နဲ့ တူတဲ့ row ကို deleteထပ်လုပ်လိုက်တာပဲဖြစ်ပါတယ်
-

```js
return res.status(200).send("OK");
```

- နောက်ဆုံးမှာတော့ order cancel လုပ်တာအဆင်ပြေကြောင်း ok response ပြန်လိုက်တာပဲဖြစ်ပါတယ်
- response ပြန်တာကို user တွေ မမြင်ရတာမလို့ MUI SnackBar component ကို သုံးပြီး ပြပေးလိုက်ပါမယ်

![](https://cdn.discordapp.com/attachments/1146496852087287898/1159204332483379200/image.png?ex=651f08a9&is=651db729&hm=be92eecc61f6c016ec7cb1b29f2c554c3870d3bbff87b9bfe44fd9f8c7f4dae7&)

- MUI Snackbar component ကို သုံးလိုက်ပြီ
  - open props အဖြစ် open state
  - onClose props အဖြစ် setOpen function
  - anchorOrigin props အဖြစ် snackbar position
  - message props အဖြစ် order has been cancelled
- ဆိုပြီး ထည့်ပေးလိုက်ပါတယ်
- onSuccess function မှာလည်း open state ကို true အဖြစ် update လုပ်ပေးထားလိုက်ပါတယ်
- ခု order တစ်ခု comfirm လုပ်ပြီး cancel ပြန်လုပ်ကြည့်ပါက cancel လုပ်ပေးကြောင်း message တစ်ခုပြပေးပြီးမှ home ကို ပြန်ထွက်သွားမှာဖြစ်ပါတယ်

![](https://cdn.discordapp.com/attachments/1146496852087287898/1159206080899985419/image.png?ex=65302dca&is=651db8ca&hm=65a0c916d8fd6b4545715da364573c95d3757f36b0ad0f8da6729deab80b306e&)

##

## Next Auth

### Authentication for Next.js

- next js မှာ log in / out နဲ့ Authentication အတွက် next-auth ဆိုတဲ့ package တစ်ခုကို အသုံးပြုလို့ ရပါတယ်
- ခု သင်ခန်းစာမှာတော့ next auth ကို သုံးပြီး google account နဲ့ log in/out လုပ်လို့ရအောင် လေ့လာသွားကြပါမယ်
- အရင်ဆုံး မိမိ project မှာ next-auth ကို ထည့်ပါ။

```console
npm i next-auth
```

- ပြီးရင် google developer website မှာ credentail ကုဒ်တွေ create သွားလုပ်ပါမယ်
  [https://console.developers.google.com/apis/credentials](https://console.developers.google.com/apis/credentials)
- project တစ်ခု create လုပ်ပေးပါ

![enter image description here](https://cdn.discordapp.com/attachments/1076154921562411008/1130345485283106836/image.png)

- ပြီးရင် create credentail --> 0auth client ID ကို နှိပ်ပါ

![enter image description here](https://cdn.discordapp.com/attachments/1076154921562411008/1130345907590795284/image.png)

- Configure consent screen ကို နှိပ်ပါ
- ![enter image description here](https://cdn.discordapp.com/attachments/1076154921562411008/1130346027526918145/image.png)

- app name နဲ့ email တွေ ဖြည့်ပြီး save ပါ

![enter image description here](https://cdn.discordapp.com/attachments/1076154921562411008/1130346368276373555/image.png)

- ပြီးရင် credential ကို ပြန်သွားပြီး credentail တစ်ခု ကို အောက်ပါအတိုင်း create လုပ်လိုက်ပါ

![enter image description here](https://cdn.discordapp.com/attachments/1076154921562411008/1130349115268677732/image.png)

- Application type မှာ Web application ကို ရွေးပါ
- ![enter image description here](https://cdn.discordapp.com/attachments/1076154921562411008/1130347199142834227/image.png)

- Authorized redirect URIs မှာ `http://localhost:3000/api/auth/callback/google` ကို Add ပေးပါ
  ![enter image description here](https://cdn.discordapp.com/attachments/1076154921562411008/1130347263932235786/image.png)
- create နှိပ်လိုက်ပါက box တစ်ခု ပေါ်လာမှာဖြစ်ပါတယ်

![enter image description here](https://cdn.discordapp.com/attachments/1076154921562411008/1130347374594752582/image.png)

- Client ID နဲ့ secret key တွေကို copy လုပ်ထားပေးပါ

##

### Setup next auth in project

- အရင်ဆုံး copy လုပ်ထားတဲ့ Client ID နဲ့ secret key တွေကို .env မှာ သိမ်းပြီး config ကနေ သုံးလို့ရအောင် သတ်မှတ်ပါမယ်

```js
// .env
# Environment variables declared in this file are automatically made available to Prisma.

# See the documentation for more detail: https://pris.ly/d/prisma-schema#accessing-environment-variables-from-the-schema



# Prisma supports the native connection string format for PostgreSQL, MySQL, SQLite, SQL Server, MongoDB and CockroachDB.

# See the documentation for all the connection string options: https://pris.ly/d/connection-strings



DATABASE_URL="postgresql://postgres:1234@localhost:5432/mydb?schema=public"



NEXT_PUBLIC_API_BASE_URL=http://localhost:3000/api



GOOGLE_CLIENT_ID=762948475479-fh816tgk5vjimgkp23qr53ilp1c25np9.apps.googleusercontent.com

GOOGLE_CLIENT_SECRET=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
```

```js
//config/config.ts

interface Config {
  apiBaseUrl: string;
  googleClientId: string;
  googleClientSecret: string;
}

export const config: Config = {
  apiBaseUrl: process.env.NEXT_PUBLIC_API_BASE_URL || "",
  googleClientId: process.env.GOOGLE_CLIENT_ID || "",
  googleClientSecret: process.env.GOOGLE_CLIENT_SECRET || "",
};
```

- `pages--> api --> auth folder` တစ်ခုလုပ်ပြီး `[...nextauth].ts` ဆိုတဲ့ file တစ်ခု လုပ်ပေးပါ
- folder နဲ့ file နာမည်တွေကို အပေါ်မှာ ရေးထားတဲ့ အတိုင်း အတိအကျ လုပ်ပေးရပါမယ်
- `[...nextauth].ts` ထဲမှာ Google provider နဲ့ setup လုပ်ပါမယ်

![](https://cdn.discordapp.com/attachments/1146496852087287898/1159207846873923625/image.png?ex=65302f6f&is=651dba6f&hm=5003d3cbfd020cd5cce7234f8af38b21d2027919ecfa5f92bcbf32cf4ab99c30&)

```js
import { config } from "@/config";
import NextAuth from "next-auth";
import GoogleProvider from "next-auth/providers/google";

export const authOptions = {
  // Configure one or more authentication providers
  providers: [
    GoogleProvider({
      clientId: config.googleClientId,
      clientSecret: config.googleClientSecret,
    }),
  ],
};

export default NextAuth(authOptions);
```

-ပြီးရင် next auth ကို သုံးနိုင်ဖို့ \_app.tsx မှာ `<SessionProvider>` နဲ့ wrap လုပ်ပေးရမှာဖြစ်ပါတယ်
![](https://cdn.discordapp.com/attachments/1146496852087287898/1159209035309010964/image.png?ex=6530308b&is=651dbb8b&hm=36fd3ff1458a74d4e83a192ed44dabaf8ec657e6e82296a73d9d68463f00e722&)

### Next app ကို google account နဲ့ login လုပ်လို့ရအောင် ပြင်ဆင်ထားပြီးပါပြီး

- ခု နမူနာ component တစ်ခုလုပ်ပြီး login/out စမ်းကြည့်ပါမယ်

```js
// src/pages/auth/index.tsx

import { Box, Button } from "@mui/material";
import { signIn, signOut, useSession } from "next-auth/react";
import { useState } from "react";

const AuthPage = () => {
  const { data: session } = useSession();
  const [type, setType] = useState("password");
  if (session) {
    return (
      <>
        Signed in as {session?.user?.email} <br />
        <button onClick={() => signOut()}>Sign out</button>
        <Box>
          <input type={type} />
          <Button
            variant="contained"
            onClick={() => {
              const newType = type === "password" ? "text" : "password";
              setType(newType);
            }}
          >
            Eye
          </Button>
        </Box>
      </>
    );
  }
  return (
    <>
      Not signed in <br />
      <button onClick={() => signIn()}>Sign in</button>
    </>
  );
};

export default AuthPage;
```

- next auth က useSession hook ကို သုံးပြီး ရလာတဲ့ data တွေကို session အဖြစ် သိမ်းလိုက်ပါတယ်
- session data မရှိခဲ့ရင် Signin ၀င်ခိုင်းမှာဖြစ်ပြီး
- ရှိခဲ့ရင် singin လုပ်ထားတဲ့အချက်အလက်တွေ ပြပေးကာ signout လုပ်လို့ရမယ့် button တစ်ခုလဲ ထည့်ပေးလိုက်မှာဖြစ်ပါတယ်
