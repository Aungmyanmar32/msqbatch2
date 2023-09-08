## MSquare Programming Fullstack Course

### Batch 2

### Episode-_15_ Summary

## CURD web-application with react and express(part-2)

- Create menu (MUI Dialog Box)
- show menu (MUI Card component)
  ##

### အရင် သင်ခန်းစာတုန်းကလို Menu page (Menu component) တစ်ခုထဲမှာပဲ create / update / delete လုပ်တာတွေကို စုပြုံ မထားတော့ပဲ menu create လုပ်တဲ့အပိုင်းကို CreateMenu ဆိုတဲ့ component တစ်ခု လုပ်ပြီး သီးသန့်ခွဲထုတ်လိုက်ပါမယ်

```js
// frontend/src/pages/menu/CreateMenu.tsx

import { Box, Dialog } from "@mui/material";

interface Props {
  open: boolean;
  setOpen: (value: boolean) => void;
}

const CreateMenu = ({ open, setOpen }: Props) => {
  return (
    <Box>
      <Dialog open={open} onClose={() => setOpen(false)}>
        <h1>I'm Dialog</h1>
      </Dialog>
    </Box>
  );
};

export default CreateMenu;
```

- open နဲ့ setOpen တို့ကို props အနေနဲ့ လက်ခံထားတဲ့ CreateMenu component တစ်ခု လုပ်ထားပါတယ်
- open ရဲ့ type ကို boolean အဖြစ် သတ်မှတ်ပေးထားပြီး
- setOpen ရဲ့ type တော့ boolean ဖြစ်တဲ့parameter တစ်ခုကို လက်ခံတဲ့ function type အဖြစ် သတ်မှတ်ပေးထားပါတယ်
- Mui က Dialog component ကို return လုပ်ထားပါတယ်
- Dialog component မှာ `open` နဲ့ `onclose` props တွေကို လက်ခံပါတယ်
- `open` props က Dialog box ကို ပြပေးမယ့် state ကို ကိုင်တွယ်မှာဖြစ်ပြီး true ဖြစ်ရင် dialog box ကို ပြပေးပြီး false ဖြစ်ရင် dialog box ကို ပိတ်ထားပေးမှာဖြစ်ပါတယ်
- `onclose` props ကတော့ dialog box အပြင်ဘက်မှာနှိပ်လိုက်တဲ့အခါ run ပေးမယ့် function ဖြစ်ပါတယ်
- အဲ့ဒီ props နှစ်ခုလုံးရဲ့ တန်ဖိုးကို CreateMenu မှာ လက်ခံထားတဲ့ props တွေရဲ့ တန်ဖိုးနဲ့ သတ်မှတ်ပေးထားလိုက်ပါတယ်
- flow ကို မြင်သာအောင် ရှင်းပြရမယ်ဆိုရင် CreateMenu component ကို ခေါ်သုံးမယ့် parent component မှာ open state တစ်ခု(`[open,setOpen]=useState(flase)`) သတ်မှတ်ပြီး အဲ့ဒီ state ကို update လုပ်ပြီး dialog box ကို ဖွင့်/ပိတ် လုပ်ချင်တာဖြစ်ပါတယ်
- flow ကို လက်တွေ့ အသုံးချကြည့်ရအောင်
- Menu componentမှာ CreateMenu component ကို ခေါ်သုံးပြီး dialog box ကို ပြပေးနိုင်အောင် လုပ်ပါမယ်
  ![](https://cdn.discordapp.com/attachments/1146496852087287898/1149767700382490714/image.png)

  - Menu component မှာ open state တစ်ခုသတ်မှတ်ထားပြီး Open Dialog button ကို နှိပ်လိုက်ချိန်မှာ open ကို true လုပ်ပေးလိုက်ပါတယ်
  - CreateMenu component ကို ေခါ်သုံးထားပြီး open နဲ့ setOpen ကို props အနေနဲ့ ထည့်ပေးထားပါတယ်
  - ခု react app မှာ Open dialog ကို နှိပ်ကြည့်ပါက Dialog box ပြပေးမှာဖြစ်ပြီး dialog box အပြင်ဘက်မှာ click လိုက်ပါက dialog box ကို ပိတ်ပေးလိုက်မှာဖြစ်ပါတယ်

![](https://cdn.discordapp.com/attachments/1146496852087287898/1149770768331309136/dialog1.png)

##

### Move create menu function to CreateMenu component's Dialog box

- Menu component ထဲက menu တစ်ခု create လုပ်တဲ့ function ကို CreateMenu component ထဲကို ရွေ့ပြီး menu တစ်ခု create လုပ်တဲ့ အခါ dialog box နဲ့ ပြပေးမှာဖြစ်ပါတယ်

## To be containue....

## ဆက်ရန်....
