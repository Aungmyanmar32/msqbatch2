## MSquare Programming Fullstack Course

### Batch 2

### Episode-_38_ Summary

### create and upload QR code for tables

### Disable menu-category in locaton

### Setup Layout for Order App and Backoffice App

##

### create and upload QR code for tables

- table တစ်ခု create လုပ်လိုက်ရင် QR image တစ်ခု create လုပ်ပေးပြီး တစ်ခါတည်း Digital ocean မှာ upload လုပ်လိုက်မှာဖြစ်ပါတယ်

```console
$ npm i qrcode
```

- ပြီးရင် FileUpload.ts မှာ qrcode create and upload လုပ်ဖို့ ပြင်ဆင်လိုက်ပါမယ်

```js
export const generateLinkForQRCode = (companyId: number, tableId: number) => {
  return `${config.orderAppUrl}?companyId=${companyId}&tableId=${tableId}`;
};

export const qrCodeImageUpload = async (companyId: number, tableId: number) => {
  try {
    const qrImageData = await QRCode.toDataURL(
      generateLinkForQRCode(companyId, tableId)
    );
    const input = {
      Bucket: "msquarefdc",
      Key: `foodie-pos/msquarefdc/qrcode/companyId-${companyId}-tableId-${tableId}.png`,
      ACL: "public-read",
      Body: Buffer.from(
        qrImageData.replace(/^data:image\/\w+;base64,/, ""),
        "base64"
      ),
    };
    // @ts-ignore
    const command = new PutObjectCommand(input);
    await s3Client.send(command);
  } catch (err) {
    console.error(err);
  }
};

export const getQrCodeUrl = (companyId: number, tableId: number) => {
  return `https://msquarefdc.sgp1.cdn.digitaloceanspaces.com/foodie-pos/msquarefdc/qrcode/companyId-${companyId}-tableId-${tableId}.png`;
};
```

> ရှင်းလင်းချက်

```js
export const generateLinkForQRCode = (companyId: number, tableId: number) => {
  return `${config.orderAppUrl}?companyId=${companyId}&tableId=${tableId}`;
};
```

- user က create table လုပ်လိုက်ရင် order app url ရယ် အဲ့ဒီ table နဲ့ location id ရယ်ကို တွဲပြီး url တစ်ခုကို သတ်မှတ်လိုက်ပါတယ်

```js

```

```js
export const getQrCodeUrl = (companyId: number, tableId: number) => {
  return `https://msquarefdc.sgp1.cdn.digitaloceanspaces.com/foodie-pos/msquarefdc/qrcode/companyId-${companyId}-tableId-${tableId}.png`;
};
```

```js
export const qrCodeImageUpload = async (companyId: number, tableId: number) => {
  try {
    const qrImageData = await QRCode.toDataURL(
      generateLinkForQRCode(companyId, tableId)
    );
    const input = {
      Bucket: "msquarefdc",
      Key: `foodie-pos/msquarefdc/qrcode/companyId-${companyId}-tableId-${tableId}.png`,
      ACL: "public-read",
      Body: Buffer.from(
        qrImageData.replace(/^data:image\/\w+;base64,/, ""),
        "base64"
      ),
    };
    // @ts-ignore
    const command = new PutObjectCommand(input);
    await s3Client.send(command);
  } catch (err) {
    console.error(err);
  }
};
```

```js
export const qrCodeImageUpload = async (companyId: number, tableId: number) => {
  try {
    const qrImageData = await QRCode.toDataURL(
      generateLinkForQRCode(companyId, tableId)
    );
```

- qrcode module ကို သုံးပြီး အပေါ်မှာသတ်မှတ်ထားတဲ့ url ကို qr image အနေနဲ့ တည်ဆောက်လိုက်ပါတယ်

```js

```

```js

```

```js

```

```js

```

### Disable menu-category in locaton

### Setup Layout for Order App and Backoffice App
