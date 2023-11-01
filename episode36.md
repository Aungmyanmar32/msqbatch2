## MSquare Programming Fullstack Course

### Batch 2

### Episode-_36_ Summary

##

### handle delete in api (backend)

- ပြီးခဲ့တဲ့ သင်ခန်းစာမှာ menu delete လုပ်လို့ရှိရင် အဲ့ဒီ menu နဲ့ ပတ်သတ်တဲ့ addon category တွေကိုပါ ဖျက်သင့်မဖျက်သင့် frontend မှာပဲ စစ်ခဲ့ကြပြီး တစ်ခါတည်း ဖျက်ပေးခဲ့ကြပါတယ်
- ခုသင်ခန်းစာမှာတော့ menu delete လုပ်လို့ရှိရင် အဲ့ဒီ menu နဲ့ ပတ်သတ်တဲ့ addon category တွေကိုပါ ဖျက်သင့်မဖျက်သင့်ကို api မှာပဲ စစ်လိုက်ပြီး database မှာပါ တစ်ခါတည်း update လုပ်ပေးလိုက်မှာဖြစ်ပါတယ်

##

![](https://cdn.discordapp.com/attachments/1146496852087287898/1169278744213602345/image.png?ex=6554d2b0&is=65425db0&hm=c39e1841abad2157bbe89ca51c2b8b9a04448ad3999ad6e3d22ef4751edf1773&)

- နမူနာ ပုံထဲကလို dataတွေ ရှိတယ်ဆိုပါစို့
- menu category တစ်ခု delete လုပ်မယ်ဆိုရင် menuCategoryMenu join tableမှာ delete လုပ်ချင်တဲ့ menu category နဲ့ ပတ်သတ်တဲ့ rows တွေကိုပါ delete လုပ်ပေးရမှာဖြစ်ပြီး
- အဲ့ဒီ menu category နဲ့ ချိတ်ထားတဲ့ menu တွေကိုပါ delete လုပ်သင့်မလုပ်သင့် check ပေးရမှာဖြစ်ပါတယ်
- အလားတူပဲ menuတစ်ခု delete လုပ်မယ်ဆိုရင် menuAddonCategory join tableမှာ delete လုပ်ချင်တဲ့ menu နဲ့ ပတ်သတ်တဲ့ rows တွေကိုပါ delete လုပ်ပေးရမှာဖြစ်ပြီး
- အဲ့ဒီ menu နဲ့ ချိတ်ထားတဲ့ Addon Category တွေကိုပါ delete လုပ်သင့်မလုပ်သင့် check ပေးရမှာဖြစ်ပါတယ်
  ##

### Deleting Menu Category

1. Get all connected Menus.
2. Check if connected Menus should be deleted.
   - If the Menu can be deleted:
     - 2-A. Get all connected Addon Categories.
     - 2-B. Check if connected Addon Categories should be deleted.
       - If the Addon Category can be deleted:
     - 2-C. Also delete connected Addons.
     - Continue to 2-D, 2-E, 2-F.
       - If the Addon Category cannot be deleted:
     - 2-D. Delete all related rows in the MenuAddonCategory join table.
     - 2-E. Delete the current Menu.
     - 2-F. Continue to step 3 and 4.
   - If the Menu cannot be deleted, skip all of step 2.
3. Delete all related rows in the MenuCategoryMenu join table.
4. Delete the current Menu Category.

##

### Deleting Menu

1. Get all connected Addon Categories.
2. Check if connected Addon Categories should be deleted.
   - If the Addon Category can be deleted:
3. Also delete connected Addons.
4. Continue to 5, 6
   - If the Addon Category cannot be deleted:
5. Delete all related rows in the MenuAddonCategory join table.
6. Delete the current Menu.

##

### Checking should delete or not

- menu categoryတစ်ခု ဖျက်လို့ရှိရင် အဲ့ဒီ menu category နဲ့ ချိတ်ထားတဲ့ menu တွေကိုပါ delete လုပ်သင့်မလုပ်သင့် check ပေးရမှာဖြစ်ပါတယ်
- ဖျက်မယ့် menu category နဲ့ ချိတ်ထားတဲ့ menu တွေက တခြား menu category နဲ့ ချိတ်ထားတာရှိခဲ့ရင် ဘာမှ လုပ်ပေးစရာမလိုပါ
- ဖျက်မယ့် menu category နဲ့ ချိတ်ထားတဲ့ menu တွေက တခြား menu category နဲ့ ချိတ်ထားတာမရှိမှ အဲ့ဒီ menu ကိုပါ delete လုပ်ပေးလိုက်ရမှာဖြစ်ပါတယ်

##

### အလားတူပဲ

- menu cတစ်ခု ဖျက်လို့ရှိရင် အဲ့ဒီ menuနဲ့ ချိတ်ထားတဲ့ addon category တွေကိုပါ delete လုပ်သင့်မလုပ်သင့် check ပေးရမှာဖြစ်ပါတယ်
- ဖျက်မယ့် menu နဲ့ ချိတ်ထားတဲ့ addon category တွေက တခြား menu နဲ့ ချိတ်ထားတာရှိခဲ့ရင် ဘာမှ လုပ်ပေးစရာမလိုပါ
- ဖျက်မယ့် menu နဲ့ ချိတ်ထားတဲ့ addon category တွေက တခြား menu နဲ့ ချိတ်ထားတာမရှိမှ အဲ့ဒီ addon category ကိုပါ delete လုပ်ပေးလိုက်ရမှာဖြစ်ပါတယ်

## Delete လုပ်တယ်ဆိုတာ isArchived ရဲ့ value ကို true လုပ်ပေးတာကို ဆိုလိုတာ ဖြစ်ပါတယ်

### Example code

```js
// handle delete menuCategory

//get joined Menu ids
const joinedMenu = await prisma.menuCategoryMenu.findMany({
  where: { menuCategoryId },
});
const menuIds = joinedMenu.map((item) => item.menuId);

// check that menus are can delete
menuIds.forEach(async (id) => {
  const filterMenu = await prisma.menuCategoryMenu.findMany({
    where: { menuId: id, isArchived: false },
  });

  const canDeleteMenu = filterMenu.length === 1 ? true : false;

  //if menu can delete

  if (canDeleteMenu) {
    //find connected addon-category ids
    const connection = await prisma.menuAddonCategory.findMany({
      where: { menuId: id },
    });

    const addonCategoryIds = connection.map((item) => item.addonCategoryId);

    //check can delete addon category
    addonCategoryIds.forEach(async (acid) => {
      const joindedMenu = await prisma.menuAddonCategory.findMany({
        where: { addonCategoryId: acid, isArchived: false },
      });

      const canDeleteAddonCategory = joindedMenu.length === 1 ? true : false;

      //if can delete
      if (canDeleteAddonCategory) {
        await prisma.addonCategory.update({
          data: { isArchived: true },
          where: { id: acid },
        });

        await prisma.addon.updateMany({
          data: { isArchived: true },
          where: { addonCategoryId: acid },
        });
      }

      //delete join table row
      await prisma.menuAddonCategory.updateMany({
        data: { isArchived: true },
        where: { menuId: id },
      });

      //delete menu
      await prisma.menu.update({ data: { isArchived: true }, where: { id } });
    });
  }
});

//delete in join tabel
await prisma.menuCategoryMenu.updateMany({
  data: { isArchived: true },
  where: { menuCategoryId },
});

//delete current menu-category
await prisma.menuCategory.update({
  data: { isArchived: true },
  where: { id: menuCategoryId },
});
```
