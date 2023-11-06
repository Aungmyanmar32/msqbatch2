## MSquare Programming Fullstack Course

### Batch 2

### Episode-_37_ Summary

## Image uploading (_multer,aws-s3,multer-s3,file-drop-zone_)

- ဒီနေ့သင်ခန်းစာမှာတော့ digital ocean space မှာ image upload လုပ်တဲ့အကြောင်းကို လေ့လာသွားကြပါမယ်
- အရင်ဆုံး လိုအပ်တဲ့ package တွေကို install လုပ်ပေးလိုက်ပါမယ်

```console
$ npm i multer multer-s3 @aws-sdk react-dropzone
```

```console
$ npm i -D @types/multer @types/multer-s3
```

- multer/s3 ကတော့ file upload တဲ့အခါ အသုံးပြုလို့ရတဲ့ module တစ်ခုဖြစ်ပါတယ်
- react-dropzone ကိုတော့ file upload တဲ့ UI မှာ drap n drop လုပ်လို့ရအောင် လုပ်ပေးတဲ့ဟာဖြစ်ပါတယ်
- ract dropzone ကို သုံးပြီး image upload input UI တစ်ခု သတ်မှတ်လိုက်ပါမယ်

```js
// src/components/FileDropZone.tsx

import { Box } from "@mui/material";
import { useDropzone } from "react-dropzone";

interface Props {
  onFileSelected: (acceptedFiles: File[]) => void;
}

const FileDropZone = ({ onFileSelected }: Props) => {
  const onDrop = (acceptedFiles: File[]) => {
    onFileSelected(acceptedFiles);
  };

  const { getRootProps, getInputProps, isDragActive } = useDropzone({
    onDrop,
  });

  return (
    <Box
      {...getRootProps()}
      sx={{
        borderRadius: 4,
        border: "3px dotted lightgray",
        textAlign: "center",
        p: 1,
        cursor: "pointer",
      }}
    >
      <input {...getInputProps()} />
      {isDragActive ? (
        <p>Drop the files here ...</p>
      ) : (
        <p>Drag drop some files here, or click to select files</p>
      )}
    </Box>
  );
};

export default FileDropZone;
```

- react dropzone က useDropzone hook ကို သုံးပြီး file input component တစ်ခု သတ်မှတ်လိုက်တာပဲဖြစ်ပါတယ်

- အဲ့ဒီ FileDropZone component ကို menu create လုပ်တဲ့ NewMenu component မှာ သုံးလိုက်ပါမယ်

![](https://cdn.discordapp.com/attachments/1146496852087287898/1170940886201421964/image.png?ex=655adeae&is=654869ae&hm=8c965abbafa2528aeca0337d53379c589da7e43409753feb23440a8cc600cf14&)

```js
 const [menuImage, setMenuImage] = useState<File>();
 /*
..................................
..................................
 */

  const onFileSelected = (files: File[]) => {
    setMenuImage(files[0]);
  };

 /*
..................................
..................................
 */

  <Box sx={{ mt: 2 }}>
          <FileDropZone onFileSelected={onFileSelected} />
          {menuImage && (
            <Chip
              sx={{ mt: 2 }}
              label={menuImage.name}
              onDelete={() => setMenuImage(undefined)}
            />
          )}
    </Box>
```

- menuImage ဆိုတဲ့ state တစ်ခုသတ်မှတ်လိုက်ပြီး onFileSelected function မှာ update လုပ်ထားလိုက်ပါတယ်
- return ထဲမှာတော့ FileDropZone component ကို သုံးလိုက်ပြီး အပေါ်မှာ သတ်မှတ်ထားတဲ့onFileSelected function ကို props အနေနဲ့ ထည့်ပေးလိုက်ပါတယ်
- ဖိုင်တစ်ခုခု ရွေးလိုက်ပြီးဆိုရင် ရွေးလိုက်တဲ့ဖိုင်ကို လည်း mui chip နဲ့ အောက်မှာ ပြပေးထားလိုက်ပါတယ်
  ![](https://cdn.discordapp.com/attachments/1146496852087287898/1170943786097061928/image.png?ex=655ae161&is=65486c61&hm=07a4f7db4717a3e5ffcb81cc035501a3b77eb0d896a44c21ca80737417ba7cbd&)

##

- handleCreateMenu function မှာလည်း formData တစ်ခုတည်ဆောက်ပြီး file upload လုပ်လို့ရအောင် သတ်မှတ်လိုက်ပါမယ်

```js
// src/components/NewMenu.tsx ---> handleCreateMenu ()=>{...}

const handleCreateMenu = async () => {
  const newMenuPayload = { ...newMenu };
  if (menuImage) {
    const formData = new FormData();
    formData.append("files", menuImage);
    const response = await fetch(`${config.apiBaseUrl}/assets`, {
      method: "POST",
      body: formData,
    });
    const { assetUrl } = await response.json();
    newMenuPayload.assetUrl = assetUrl;
  }
  dispatch(createMenu({ ...newMenuPayload, onSuccess: () => setOpen(false) }));
};
```

- newImage မှာ value တစ်ခုခုရှိခဲ့ရင် formData တစ်ခုကို create လုပ်လိုက်ပြီး files ဆိုတဲ့ name နဲ့ append လုပ်ပေးလိုက်ပါတယ်
- အဲ့ဒီ newImage appendလုပ်ထားတဲ့ formDataကို /api/assets route ဆီ POST method နဲ့ request လုပ်လိုက်ပြီး body အနေနဲ့ ထည့်ပေးလိုက်ပါတယ်
- apiမှာတော့ digital ocean space ဆီ upload သွားလုပ်ပေးမှာဖြစ်ပြီး url တစ်ခုကို response ပြန်လာမှာဖြစ်ပါတယ်
- ပြန်လာတဲ့ response ကို newMenuရဲ့ assetUrl အဖြစ် သတ်မှတ်ပေးလိုက်ပါတယ်
- ပြီးရင် newMenu state တစ်ခုလုံးကို createMenu action dispatch လုပ်တဲ့အချိန် ပြန်ထည့်ပေးလိုက်တာပဲဖြစ်ပါတယ်

##

### File upload to digital ocean space

- digital ocean space ဆိုတာက file upload လုပ်လို့ရတဲ့ cloud server တစ်ခုဖြစ်ပါတယ်
- ကျနော်တို့ရဲ့ database မှာ ပုံတွေကို တိုက်ရိုက်သိမ်းထားမှာ မဟုတ်ပဲ digital ocean space server မှာ upload လုပ်ပြီးမှ ရလာတဲ့ url ကိုပဲ သိမ်းမှာဖြစ်ပါတယ်
- digital ocean space မှာ file upload လုပ်နိုင်ဖို့ ပြင်ဆင်ပါမယ်
- အရင်ဆုံး fileupload.ts ဖိုင်တစ်ခု သတ်မှတ်လိုက်ပါမယ်

```js
// src/utils/fileUpload.ts

import { S3Client } from "@aws-sdk/client-s3";
import multer from "multer";
import multerS3 from "multer-s3";
import { config } from "./config";

const s3Client = new S3Client({
  endpoint: config.spaceEndpoint,
  region: "sgp1",
  credentials: {
    accessKeyId: config.spaceAccessKeyId,
    secretAccessKey: config.spaceSecretAccessKey,
  },
});

export const fileUpload = multer({
  storage: multerS3({
    s3: s3Client,
    bucket: "msquarefdc",
    acl: "public-read",
    key: (request, file, cb) => {
      cb(null, `foodie-pos/msquarefdc/${Date.now()}_${file.originalname}`);
    },
  }),
}).array("files", 1);
```

- aws-sdk ကနေ s3Client တစ်ခု သတ်မှတ်လိုက်ပြီး ကျနော်တို့ သုံးမယ့် digital ocean space (DOS) server နဲ့ ချိတ်ဆက်ပေးလိုက်ပါတယ်
- ထည့်ပေးရမယ့် endpoint,accessKeyId,secretAccessKey တွေကို .env ဖိုင်ထဲမှာ သတ်မှတ်ပေးရပါမယ် (လိုအပ်တဲ့ key တွေကို Aungmyanmar Discord မှာ လာယူပေးကြပါခင်ဗျာ)
- နောက်ပြီးတော့ multer ကို သုံးပြီး file ကို သိမ်းမယ့် storage တစ်ခု သတ်မှတ်လိုက်ပါတယ်
- s3 အဖြစ် အပေါ်မှာ သတ်မှတ်ထားတဲ့ client ကို သုံးမှာဖြစ်ပြီး
- bucket အနေနဲ့ DOS မှာ သတ်မှတ်ထားတဲ့ bucket nameကို ထည့်ပေးရမှာဖြစ်ပါတယ်
- acl publice read ဆိုတာကလူတိုင်းကြည့်လို့ရအောင် access ပေးထားလိုက်တာဖြစ်ပါတယ်
- key ဆိုတက file သိမ်းမယ့် နေရာကို သတ်မှတ်ပေးလိုက်တာဖြစ်ပါတယ်
-

```js
`foodie-pos/msquarefdc/${Date.now()}_${file.originalname}`;
```

- msquarefdc ဆိုတဲ့နေရာမှာ မိမိနာမည်ကို space မပါပဲ အစားထိုးပေးရမှာဖြစ်ပါတယ်(`foodie-pos/ko-aung/${Date.now()}_${file.originalname}`)

```js
array("files", 1);
```

- array("files", 1) ဆိုတာက file တစ်ခုပဲလက်ခံမယ်လို့ သတ်မှတ်လိုက်တာပါ

- အဲ့ဒီ fileUpload ကို သုံးပြီး /api/assets route မှာ frontend က ပို့လာတဲ့ request ကို လက်ခံပြီး DOS မှာ upload လုပ်ပေးလိုက်ပါမယ်

```js
// src/pages/api/assets/index.ts

// Next.js API route support: https://nextjs.org/docs/api-routes/introduction
import { fileUpload } from "@/utils/fileUpload";
import { Request, Response } from "express";

export const config = {
  api: {
    bodyParser: false,
  },
};

export default function handler(req: Request, res: Response) {
  try {
    fileUpload(req, res, (error) => {
      if (error) {
        return res.status(500).send("Internal Server Error");
      }
      const files = req.files as Express.MulterS3.File[];
      const file = files[0];
      const assetUrl = file.location;
      res.status(200).json({ assetUrl });
    });
  } catch (err) {
    res.status(500).send("Internal Server Error");
  }
}
```

##

- ခုဆို menu တစ်ခု create လုပ်တိုင်း ပုံတွေပါ ပါလာပြီမို့ Menu page မှာ menu တွေကို ပြပေးတဲ့အခါ item card ကို မသုံးတော့ပဲ MenuCard ကို ပြောင်းသုံးလိုက်ပါမယ်

```js
// src/components/MenuCard.tsx

import PaidIcon from "@mui/icons-material/Paid";
import { Box, Card, CardContent, CardMedia, Typography } from "@mui/material";
import { Menu } from "@prisma/client";
import Link from "next/link";

interface Props {
  menu: Menu;
  href: string | object;
}

const MenuCard = ({ menu, href }: Props) => {
  return (
    <Link
      key={menu.id}
      href={href}
      style={{
        textDecoration: "none",
        marginRight: "15px",
        marginBottom: "20px",
      }}
    >
      <Card sx={{ width: 200, height: 220, pb: 2 }}>
        <CardMedia
          sx={{ height: 140, objectFit: "contain" }}
          image={menu.assetUrl || ""}
          component={"div"}
        />
        <CardContent>
          <Typography gutterBottom variant="h6" sx={{ mb: 0 }}>
            {menu.name}
          </Typography>
          <Box
            sx={{
              display: "flex",
              justifyContent: "flex-start",
              alignItems: "center",
            }}
          >
            <PaidIcon color="success" />
            <Typography
              gutterBottom
              variant="subtitle1"
              sx={{ mt: 0.8, ml: 0.4, fontWeight: "bold", fontStyle: "italic" }}
            >
              {menu.price}
            </Typography>
          </Box>
        </CardContent>
      </Card>
    </Link>
  );
};

export default MenuCard;
```

![](https://cdn.discordapp.com/attachments/1146496852087287898/1170953182520823890/image.png?ex=655aea21&is=65487521&hm=979d82d5def72fce547ca4854c90c5996589a554b09feada12a0aa34c8c09b66&)

##

### database မှာ menu create လုပ်တဲ့နေရာမှာလည်း assetUrl ကိုပါ ထည့်ပေးလိုက်ပါမယ်

![](https://cdn.discordapp.com/attachments/1146496852087287898/1170953808109641818/image.png?ex=655aeab7&is=654875b7&hm=42614bba14d9278f6b772922b848f866a59b7b9aeb42b1520e1a29047f42de06&)
![](https://cdn.discordapp.com/attachments/1146496852087287898/1170953964821422160/image.png?ex=655aeadc&is=654875dc&hm=f55d696c5cc6c7e08427f324922ebaffc0b98c287f681e690266ca71972c5f5e&)
