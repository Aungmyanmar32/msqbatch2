## MSquare Programming Fullstack Course

### Batch 2

### Episode-_48_ Summary

### SQL query

##

- database မှာ အဓိကအားဖြင့် SQL database နဲ့ No SQL database ဆိုပြီး နှစ်မျိုးရှိပါတယ်

![enter image description here](https://raw.githubusercontent.com/Aungmyanmar32/msquare-m4/main/database1.png)

- ဒီနေ့ သင်ခန်းစာမှာတော့ SQL db(DataBase) အကြောင်း လေ့လာသွားကြပါမယ်
- Sql database ကို အသုံးပြုဖို့ အတွက် sql language နဲ့ ရေးသားပေးရပါတယ်
- sql command တစ်ခု ပြီးတိုင်း **`;`** နဲ့ ပိတ်ပေးရပါမယ်

##

## Useful sql command

| command           | Usage                                                  | meaing                                |
| ----------------- | ------------------------------------------------------ | ------------------------------------- |
| `\l`              | `\l`                                                   | List all database                     |
| `CREATE DATABASE` | `CREATE DATABASE your_db_name;`                        | Create new database                   |
| `\q`              | `\q`                                                   | Qiut from current                     |
| `\c`              | `\c`                                                   | show current database                 |
| `\c`              | `\c your_db_name`                                      | switch and connect to your db         |
| `\dt`             | `\dt`                                                  | show table details in current db      |
| `\d+`             | `\d+ table-name`                                       | show table columns details            |
| `CREATE TABLE`    | `CREATE TABLE IF NOT EXISTS table_name(table items);`  | create table                          |
| `DROP DATABASE`   | `DROP DATABASE db_name;`                               | delete database                       |
| `INSERT INTO `    | `INSERT INTO table_name (items name) VALUES (values);` | Insert row into table                 |
| `SELECT `         | `SELECT * FROM table-name;`                            | select all from table                 |
| `WHERE`           | `WHERE gender = 'male';`                               | use to ckeck a specified condition.   |
| `AND` `OR` `NOT`  | `WHERE gender = 'male' AND age ='20';`                 | use to ckeck more than one condition. |

##

### https://www.sqlteaching.com/ မှာ sql ကို နဲနဲ စမ်းရေးကြည့်ပါမယ်။

- ပထမ သင်ခန်းစာမှာတော့ table ထဲက အကုန်လုံးကို select လုပ်ခိုင်းတာမလို့
  `SELECT * FROM family_members;`
  ဆိုပြီး ရေးထည့်ပေးလိုက်ပါတယ်။
  ![enter image description here](https://raw.githubusercontent.com/Aungmyanmar32/msquare-m5-summaries/main/Screenshot%202023-04-30%20142137.png)

- sql command box ထဲမှာ `SELECT * FROM family_members;` ရေးထည့်ပြီး Run SQL လုပ်လိုက်တဲ့အခါ Congrats....ဆိုပြီး next lesson ပြလာရင် အဖြေ မှန်တာဖြစ်ပါတယ်။
- next lesson ကို နှိပ်ပြီး လုပ်ခိုင်းတာတွေ ဆက်လုပ်ကြည့်ပါမယ်
  https://www.sqlteaching.com/#!select_columns
- Can you return just the name and species columns?
- name နဲ့ species column က data တွေပဲ return လုပ်ခိုင်းတာမလို့

```sql
SELECT name,species  FROM family_members
```

- ထည့်ပေးလိုက်ရင် အဆင်ပြေသွားမှာဖြစ်ပါတယ်
- family_members table ထဲက nname နဲ့ species column အောက်က data တွေပဲ Select လုပ်လိုက်တာဖြစ်ပါတယ်။

##

lesson -3 https://www.sqlteaching.com/#!where_equals

- Can you run a query that returns all of the rows that refer to dogs?

```sql
SELECT * FROM family_members WHERE species = 'dog';
```

##

lesson-4 https://www.sqlteaching.com/#!where_greater_than

- Can you run return all rows of family members whose num_books_read is greater than 190?

```sql
SELECT * FROM family_members WHERE num_books_read >190;
```

##

lesson -5 https://www.sqlteaching.com/#!where_greater_than_or_equal

- Can you return all rows in **family_members** where num_books_read is a value greater or equal to 180?

```sql
SELECT * FROM family_members WHERE num_books_read >= 180;
```

##

## JOIN in SQL

![enter image description here](https://learnsql.com/blog/learn-and-practice-sql-joins/2.png)

## how to join tables

### Syntax

```sql
select * from table_1
join_method table_2 on table_2.FK = table_1.PK
```

### Usage

```sql
select users.name from users
inner join photos on photos.user_id = users.id
```

- အခု join တွေကို လက်တွေ့ စမ်းသပ်ကြည့်ပါမယ်။

### inner join

https://www.sqlteaching.com/#!joins

> Can you use an inner join to pair each character name with the actor
> who plays them?  
> Select the columns:  
> **character**._name_,  
> **character_actor**._actor_name_

- inner join ကို သုံးပြီး **character** နာမည်နဲ့ **actor** နာမည်ကို select လုပ်ပြီး ပြခိုင်းတာဖြစ်ပါတယ်။

```sql
SELECT character.name,character_actor.actor_name
FROM character
INNER JOIN character_actor
ON character_actor.character_id = character.id
```

-
- character table ထဲက id နဲ့ character_actor table ထဲက character_id တူတဲ့ အပေါ် မူတည်ပြီး inner join နဲ့ join လိုက်ပြီး
- character table ထဲက name နဲ့ character_actor table ထဲက actor name ကို select လုပ်လိုက်တာဖြစ်ပါတယ်။

### multiple join

https://www.sqlteaching.com/#!multiple_joins

- အပေါ်က link ရဲ့ lesson မှာတော့
  > Can you use two joins to pair each character name with the actor who plays them? Select the columns: **character**._name_, **actor**._name_
- join နှစ်ခါ လုပ်ပြီး actor name နဲ့ ဇာတ်ကောင် ကို တွဲပေးရမှာဖြစ်ပါတယ်။

```sql
SELECT character.name,actor.name
FROM character
INNER JOIN character_actor
ON 	character.id = character_actor.character_id
INNER JOIN actor
ON character_actor.actor_id = actor.id
```

- character_actor ထဲက character_id နဲ့ character ထဲက id တူတဲ့ဟာကို အရင်စစ်ပြီး join လိုက်ပါတယ်
- ဆက်ပြီး actor ထဲက id နဲ့ character_actor ထဲ က actor_id တူတဲ့ဟာကို ထပ်စစ်ပြီး join ပါတယ်
- အဲ့ဒီ join တွေထဲက character.name နဲ့ actor.name ကို ရွေးထုတ်လိုက်တာဖြစ်ပါတယ်

##

### join multiple with where

https://www.sqlteaching.com/#!joins_with_where

> Can you return a list of characters and TV shows that are not named "Willow Rosenberg" and not in the show "How I Met Your Mother"?

- Willow Rosenberg ဆိုတဲ့ ဇာတ်ကောင်မပါတဲ့character နဲ့ How I Met Your Mother ဆိုတဲ့ show မပါတဲ့ Show ကို ပြခိုင်းတာဖြစ်ပါတယ်။

```sql
SELECT character.name,tv_show.name
FROM character
INNER JOIN character_tv_show
ON character.id = character_tv_show.character_id
INNER JOIN tv_show
ON character_tv_show.tv_show_id = tv_show.id
WHERE character.name != 'Willow Rosenberg'
AND tv_show.name != 'How I Met Your Mother'
```

- character_tv_show ထဲက character_id နဲ့ characterထဲက id တူတဲ့ဟာကို အရင်join ပါတယ်
- ထပ်ပြီး character_tv_show ထဲက tv_show_id နဲ့ tv_showထဲက id တူတဲ့ဟာကိုလည်း ထပ်join ပါတယ်။
- အဲ့ဒီ join လိုက်တဲ့ အထဲကမှ Willow Rosenberg ဆိုတဲ့ character name နဲ့ How I Met Your Mother ဆိုတဲ့ show မပါတဲ့ ဟာတွေကို ရွေးထုတ်လိုက်တာဖြစ်ပါတယ်

##

### Left join

https://www.sqlteaching.com/#!left_joins

> Can you use left joins to match **character names** with the **actors** that play them?

- Left join ကို သုံးပြီး ဘယ် actor က ဘယ်လို ဇာတ်ကောင်နေရာ သရုပ်ဆောင်တယ်ဆိုတာကို join လုပ်ခိုင်းတာဖြစ်ပါတယ်။

```sql
SELECT character.name,actor.name
FROM character
LEFT JOIN character_actor
ON character.id = character_actor.character_id
LEFT JOIN actor
ON  character_actor.actor_id = actor.id
```

- character_actor ထဲက character_id နဲ့ character ထဲက id တူတဲ့ဟာကို အရင်စစ်ပြီး join လိုက်ပါတယ်
- ဆက်ပြီး actor ထဲက id နဲ့ character_actor ထဲ က actor_id တူတဲ့ဟာကို ထပ်စစ်ပြီး join ပါတယ်
- အဲ့ဒီ join တွေထဲက character.name နဲ့ actor.name ကို ရွေးထုတ်လိုက်တာဖြစ်ပါတယ်
- **_left join နဲ့ inner join နဲ့ မတူတာက_**
- **inner join** က table နှစ်ခု **join တဲ့ အချက်ပေါ် မူတည်ပြီး table နှစ်ခုထဲက မှ သက်ဆိုင်တဲ့ data တွေကိုပဲ** ပြပေးတာဖြစ်ပြီး
- **left join** ကတော့ **ပထမ table(left) ရှိ select လုပ်ထားတဲ့ column ထဲက data တွေကို အကုန်ပြပေးမှာဖြစ်ပြီး** join တဲ့ အချက်ပေါ် မူတည်ကာ ဒုတိယ table က data ကို ပြပေးမှာဖြစ်ပါတယ်။ ပ**ထမ table ရှိ data နဲ့ သက်ဆိုင်တဲ့ data တွေ ဒုတိယ table မှာ မရှိပါက တန်ဖိုး null အဖြစ်ပြပေးမှာဖြစ်ပါတယ်**။

##

### Using pg (example)

- express server တစ်ခုကို backend folder ထဲမှာ သတ်မှတ်လိုက်ပါ
- express server ကနေ databse ( postgres) ကို ချိတ်ဆက်ဖို့ **_`pg`_** ဆိုတဲ့ node module ကို အသုံးပြုရပါမယ်။

```console

$ npm i pg
$ npm i -D @types/pg
```

- backend folder အောက်မှာ src folder တစ်ခု ဆောက်ပါမယ်။
- src folder ထဲမှာ db folder တစ်ခု ထပ်လုပ်ပါမယ်။
- db folder ထဲမှာမှ db.ts ဆိုတဲ့ ဖိုင်တစ်ခုလုပ်ပါမယ်။

```sql
project/backend/src/db/db.ts
```

```js
// db.ts
import { Pool } from "pg";

export const db = new Pool({
  host: "localhost",
  port: 5432,
  user: "postgres",
  password: "test",
  database: "foodie_pos_db",
  max: 20,
  idleTimeoutMillis: 30000,
  connectionTimeoutMillis: 2000,
});
```

- password နေရာမှာ မိမိ postgres ရဲ့ password ကို ထည်ပေးပါ။
- databaseနေရာမှာ ခုနက create လုပ်လိုက်တဲ့ database name ကို ထည့်ပေးပါ။
- server.ts မှာ request နဲ့အတူပါလာတဲ့ data တွေကို database မှာ သိမ်းဖို့ လုပ်ပါမယ်။

```js
import cors from "cors";
import express from "express";
import { db } from "./src/db/db";
const app = express();
const port = 5006;
app.use(cors());
app.use(express.json());

app.post("/auth/register", async (req, res) => {
  const { name, email, password } = req.body;
  if (!name || !email || !password) return res.sendStatus(400);

  const text =
    "INSERT INTO users(name, email, password) VALUES($1, $2, $3) RETURNING *";
  const values = [name, email, password];

  const result = await db.query(text, values);

  const userInfo = result.rows[0];

  res.send(userInfo);
});

app.listen(port, () => console.log(`Example app listening on port ${port}!`));
```

> ရှင်းလင်းချက်

    if (!name || !email || !password) return res.sendStatus(400);

- တကယ်လို့ request နဲ့ ၀င်လာတဲ့ data တွေ မပြည့်စုံခဲ့ရင် Http status code 400 ( Bad request ) ကို response ပြန်ပေးလိုက်တာဖြစ်ပါတယ်။

```js
const text =
  "INSERT INTO users(name, email, password) VALUES($1, $2, $3) RETURNING *";
const values = [name, email, password];

const result = await db.query(text, values);
res.send(result.rows[0]);
```

- text ဆိုတဲ့ variable မှာ sql query ( code) တွေ ထည့်သိမ်းထားတာဖြစ်ပါတယ်
- **INSERT INTO users(name, email, password)** ဆိုတာက users table ထဲက name, email, password ဆိုတဲ့ column တွေ မှာ ထည့်မှာဖြစ်ပါတယ်
- နောက်က values နေရာမှာ ထည့်မယ့် data တွေကို ရေးပေးရမှာဖြစ်ပါတယ်။
- name ရဲ့ တန်ဖိုးက $1 / eamil ရဲ့ တန်ဖိုးက$2 / password ရဲ့ တန်ဖိုးက$3 ဆိုပြီး ထည့်လိုက်တာဖြစ်ပါတယ်။
- $1 $2 $3 နေရာမှာ အောက်က ကြေငြာထားတဲ့ values array ထဲက တန်ဖိုးတွေ အစဥ်တိုင်း ၀င်လာမှာဖြစ်ပါတယ်

```js
const values = [name, email, password];
// values = [$1:name,$2:email,$3:password]
```

- တစ်ခါတည်း မရေးထည့်ပဲ $1 $2 $3 ဆိုပြီး value array ထဲကနေ တစ်ဆင့်ပြန်ခေါ်သုံးတာကို parameterize query လို့ ခေါ်ပါတယ်။
- မသမာသူများက မလိုလားအပ်တဲ့ sql query တွေကို request ကနေ တစ်ဆင့် ပေးပို့လာမယ့် problem ကနေ safe ဖြစ်အောင်လို့ အခုလို အသုံးပြုပေးရပါတယ်။

`const result = await db.query(text, values)`;

- db.ts ထဲက export လုပ်ထားတဲ့ db ကို အသုံးပြုထားပါတယ်။
- db.query method ကို သုံးပြီး parameter အနေနဲ့ text နဲ့ values တွေကို ထည့်ပေးလိုက်ပါတယ်။
- အထက်မှာ ရှင်းပြခဲ့သလိုပဲ database ထဲက users table မှာ သွားသိမ်းပေးမှာဖြစ်ပါတယ်
- db.query က promise ကို return ပြန်တာမလို့ async await ကို အသုံးပြုပေးရမှာဖြစ်ပါတယ်။

- sql query မှာ `RETURNING *` ဆိုပြီး လုပ်ထားမလို့ result ထဲက index 0 ဖြစ်တဲ့ rows တစ်ခုကိုပဲ response ပြန်လိုက်တာဖြစ်ပါတယ်
- ခု app မှာ user info တွေ ဖြည့်ပြီး register လုပ်ကြည့်ပါက ထည့်လိုက်တဲ့ အချက်အလက်အတိုင်း log ထုတ်ပေးတာကို မြင်ရမှာ ဖြစ်ပါတယ်။
- တစ်ခု ရှိတာက email တူတဲ့ user တွေကို Register လုပ်ကြည့်ပါက error တက်ပြီး backend server ပါ သေသွားတာတဲ့ error ကို ကြုံရမှာဖြစ်ပါတယ်
- အဆိုပါ error ကို try catch နဲ့ ဖြေရှင်းပါမယ်

```js
app.post("/auth/register", async (req, res) => {
  const { name, email, password } = req.body;
  if (!name || !email || !password) return res.sendStatus(400);

  const text =
    "INSERT INTO users(name, email, password) VALUES($1, $2, $3) RETURNING *";
  const values = [name, email, password];

  try {
    const result = await db.query(text, values);
    const userInfo = result.rows[0];
    delete userInfo.password;
    res.send(userInfo);
  } catch (error) {
    console.log(error);
    res.sendStatus(500);
  }
});
```

- error ဖြစ်ခဲ့ရင် status 500 ကို server error အနေနဲ့ ပို့ပေးလိုက်တာဖြစ်ပါတယ်။
- အခု ဆိုရင် email တူနေတဲ့ users တွေ ထည့်စမ်းကြည့်ရင်တောင် ဆာဗာက မသေတော့ပဲ error ကို ပြပေးမှာဖြစ်ပါတယ်။
