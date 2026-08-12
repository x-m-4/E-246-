# E-246 System

بوت Discord متكامل مبني باستخدام Node.js و Discord.js، مع لوحة تحكم Web
وMongoDB ودعم تشغيل البوت على عدة سيرفرات مع فصل بيانات كل سيرفر.

## المميزات

-   Discord.js v14.
-   Slash Commands وPrefix Commands.
-   دعم عدة Discord Guilds في نفس العملية.
-   نشر الأوامر لكل Guild بدون مسح أوامر السيرفرات الأخرى عند إعادة
    التشغيل.
-   MongoDB لتخزين بيانات البوت.
-   عزل إعدادات وبيانات السيرفرات باستخدام `guildId` عند الحاجة.
-   لوحة تحكم Web مبنية بـ Express/EJS.
-   نظام Tickets.
-   Moderation وProtection.
-   Levels وRank.
-   Auto Reply وAuto Line وميزات أتمتة أخرى.
-   Temp Voice.
-   Suggestions وStarboard.
-   Captcha وReaction Roles.
-   Backups محلية مع Webhook اختياري.
-   PM2 للتشغيل المستمر.
-   دعم Lavalink اختياري لميزات الموسيقى.

> **تنبيه:** هذا المشروع يحتاج إعداد Discord Developer Portal وMongoDB
> وملف `.env` قبل التشغيل.

------------------------------------------------------------------------

## المتطلبات

-   Node.js 18 أو أحدث، ويفضل إصدار LTS حديث.
-   npm.
-   MongoDB.
-   حساب Discord Bot من Discord Developer Portal.
-   على Termux: الحزم الأساسية مثل `git` و`unzip` عند استخدام Git/ZIP.

------------------------------------------------------------------------

## إعداد Discord Bot

من Discord Developer Portal:

1.  أنشئ Application جديد.
2.  أنشئ Bot.
3.  انسخ Bot Token وضعه في `.env`.
4.  فعّل Gateway Intents المطلوبة للمشروع، خصوصًا:
    -   Server Members Intent
    -   Message Content Intent
    -   Presence Intent إذا كانت الميزات المستخدمة تحتاجه.
5.  ادعُ البوت إلى السيرفرات المطلوبة بالصلاحيات المناسبة.

لا تضع Token أو Client Secret داخل GitHub.

------------------------------------------------------------------------

## إعداد MongoDB

أنشئ قاعدة بيانات MongoDB ثم استخدم رابط الاتصال في:

``` env
MONGODB_URI=mongodb://username:password@host:port/database?options
```

يمكن استخدام MongoDB Atlas أو MongoDB مستضاف على خادمك.

يفضل أن يكون لكل بيئة تشغيل قاعدة بيانات منفصلة، أو على الأقل صلاحيات
MongoDB مقيدة للمستخدم الخاص بالبوت.

------------------------------------------------------------------------

## ملف `.env`

انسخ المثال:

``` bash
cp .env.example .env
```

ثم عدّل القيم:

``` env
DISCORD_TOKEN=ضع_توكن_البوت_هنا
CLIENT_ID=ضع_معرف_التطبيق_هنا
CLIENT_SECRET=ضع_Client_Secret_هنا
GUILD_ID=ضع_معرف_السيرفر_الأساسي_هنا
OWNER_ID=ضع_معرف_المالك_هنا

CALLBACK_URL=http://localhost:3000/auth/callback
PORT=3000

USER_TOKEN=

DISABLE_DASHBOARD=false

LAVALINK_NODES=

MONGODB_URI=mongodb://username:password@host:port/database?options

BACKUP_WEBHOOK_URL=
```

### ملاحظة مهمة عن `GUILD_ID`

`GUILD_ID` يستخدمه المشروع في بعض عمليات النشر/الإعداد الأساسية.

المشروع يدعم العمل على أكثر من Guild، ولا ينبغي وضع بيانات عدة سيرفرات
داخل متغير واحد. بيانات السيرفرات التي يجب أن تكون مستقلة يتم ربطها بـ
`guildId` داخل قاعدة البيانات.

------------------------------------------------------------------------

## تشغيل محلي

بعد تجهيز `.env`:

``` bash
npm install
npm start
```

أو:

``` bash
node index.js
```

إذا بدأ البوت بنجاح، ستظهر رسائل في اللوج توضح تحميل الأوامر والاتصال بـ
MongoDB وتسجيل الدخول إلى Discord.

------------------------------------------------------------------------

# تشغيل المشروع على Termux

## الطريقة الأولى: من GitHub

ثبت المتطلبات:

``` bash
pkg update
pkg install git nodejs unzip -y
```

استنسخ المشروع:

``` bash
git clone https://github.com/USERNAME/REPOSITORY.git
cd REPOSITORY
```

ثم:

``` bash
npm install
```

أنشئ `.env`:

``` bash
cp .env.example .env
nano .env
```

ضع بيانات Discord وMongoDB ثم احفظ الملف.

شغل البوت:

``` bash
npm start
```

------------------------------------------------------------------------

## الطريقة الثانية: رفع ZIP إلى الهاتف ثم فك الضغط في Termux

إذا كان ملف ZIP موجودًا في مجلد Download على الهاتف:

``` bash
termux-setup-storage
```

اسمح لـ Termux بالوصول إلى الملفات.

اذهب إلى Downloads:

``` bash
cd ~/storage/downloads
```

اعرض الملفات:

``` bash
ls
```

إذا كان الملف مثل:

``` text
E-246-main-multiguild-safe.zip
```

فك الضغط:

``` bash
unzip E-246-main-multiguild-safe.zip
```

ادخل للمشروع:

``` bash
cd E-246-main
```

ثبت الحزم:

``` bash
npm install
```

أنشئ `.env`:

``` bash
cp .env.example .env
nano .env
```

ثم شغل:

``` bash
npm start
```

------------------------------------------------------------------------

# تشغيل البوت باستخدام PM2 على Termux

PM2 مناسب لتشغيل البوت في الخلفية وإعادة تشغيله عند حدوث Crash.

ثبته:

``` bash
npm install -g pm2
```

من مجلد المشروع:

``` bash
pm2 start ecosystem.config.js
```

اعرض الحالة:

``` bash
pm2 status
```

اعرض اللوج:

``` bash
pm2 logs e-246
```

أو:

``` bash
pm2 logs e-246 --lines 100
```

إعادة التشغيل:

``` bash
pm2 restart e-246
```

إيقاف:

``` bash
pm2 stop e-246
```

حذف العملية من PM2:

``` bash
pm2 delete e-246
```

------------------------------------------------------------------------

# رفع المشروع إلى GitHub

## 1. أنشئ Repository جديد

أنشئ مستودعًا جديدًا على GitHub، ثم من داخل مجلد المشروع:

``` bash
git init
git add .
git commit -m "Initial release"
git branch -M main
git remote add origin https://github.com/USERNAME/REPOSITORY.git
git push -u origin main
```

استبدل:

``` text
USERNAME
REPOSITORY
```

ببيانات مستودعك.

## 2. تأكد من عدم رفع الأسرار

قبل `git add .` تأكد أن `.env` غير موجود ضمن الملفات المتتبعة:

``` bash
git status
```

ويجب أن يكون `.env` ضمن `.gitignore`.

إذا كان `.env` قد تم رفعه سابقًا إلى GitHub، لا يكفي حذفه فقط؛ قم بتغيير
Token وClient Secret وأي بيانات سرية فورًا.

------------------------------------------------------------------------

# تنزيل المشروع من GitHub على Termux

بعد رفع المشروع:

``` bash
cd ~
git clone https://github.com/USERNAME/REPOSITORY.git
cd REPOSITORY
npm install
cp .env.example .env
nano .env
pm2 start ecosystem.config.js
```

------------------------------------------------------------------------

# تحديث المشروع لاحقًا

إذا كنت تستخدم Git:

``` bash
cd ~/REPOSITORY
git pull
npm install
pm2 restart e-246
```

ثم افحص اللوج:

``` bash
pm2 logs e-246 --lines 100
```

------------------------------------------------------------------------

# MongoDB وفصل بيانات السيرفرات

المشروع مصمم ليعمل مع أكثر من Discord Guild.

القاعدة الأساسية هي أن أي بيانات مرتبطة بالسيرفر يجب أن تحتوي على
`guildId` أو تكون مرتبطة به في الاستعلام/المفتاح المناسب.

مثال منطقي:

``` text
Guild A
 ├── settings
 ├── tickets
 ├── levels
 ├── protection
 └── autoreplies

Guild B
 ├── settings
 ├── tickets
 ├── levels
 ├── protection
 └── autoreplies
```

لذلك لا يجب استخدام إعدادات Guild A عند معالجة Guild B.

كذلك بيانات العضو داخل نظام السيرفرات يجب أن تراعي:

``` text
guildId + userId
```

بدل الاعتماد على `userId` وحده عندما تكون القيمة خاصة بالسيرفر.

------------------------------------------------------------------------

# Slash Commands

عند بدء البوت، يتم تحميل الأوامر ثم نشرها على الـGuilds التي يعمل عليها
النظام.

لا ينبغي أن يظهر في اللوج:

``` text
[Deploy] Set 0 guild commands
```

لـGuild تحتوي على أوامر المشروع.

إذا ظهرت هذه الرسالة بعد تعديل الكود أو بعد تحديث المشروع، أوقف البوت
وافحص إعدادات النشر قبل تكرار Restart.

------------------------------------------------------------------------

# النسخ الاحتياطي

يمكن للمشروع إنشاء نسخ احتياطية لبيانات قاعدة البيانات محليًا.

إذا أردت إرسال النسخ الاحتياطية إلى Discord، ضع Webhook في:

``` env
BACKUP_WEBHOOK_URL=ضع_الرابط_هنا
```

هذا المتغير اختياري.

لا ترفع Webhook URL إلى GitHub.

------------------------------------------------------------------------

# Dashboard

إذا كانت لوحة التحكم مفعلة، يستخدم المشروع:

``` env
PORT=3000
CALLBACK_URL=http://localhost:3000/auth/callback
```

عند تشغيله محليًا ستكون اللوحة متاحة عادة على:

``` text
http://localhost:3000
```

إذا كان البوت مستضافًا على خادم عام، يجب ضبط `CALLBACK_URL` على عنوان
OAuth الصحيح واستخدام HTTPS في بيئة الإنتاج.

------------------------------------------------------------------------

# Lavalink

ميزات الموسيقى تحتاج Lavalink Node صالحًا.

إذا لم تستخدم الموسيقى، يمكن ترك:

``` env
LAVALINK_NODES=
```

وستظهر رسالة تحذير بأن نظام الموسيقى غير متاح.

------------------------------------------------------------------------

# استكشاف الأخطاء

## البوت لا يبدأ

افحص:

``` bash
pm2 logs e-246 --lines 100
```

وتأكد من:

-   صحة `DISCORD_TOKEN`.
-   صحة `MONGODB_URI`.
-   وجود `.env`.
-   تثبيت `npm install`.
-   توافق إصدار Node.js.

## الأوامر لا تظهر

تحقق من اللوج:

``` bash
pm2 logs e-246 --lines 100
```

ابحث عن:

``` text
Loaded ... slash commands
```

ثم رسائل:

``` text
[Deploy]
```

وتأكد من أن البوت لديه صلاحية استخدام أوامر التطبيقات في السيرفر.

## MongoDB لا يتصل

راجع:

``` env
MONGODB_URI=
```

وتأكد من:

-   اسم المستخدم وكلمة المرور.
-   السماح بعنوان IP للخادم في MongoDB Atlas إذا كنت تستخدمه.
-   اسم قاعدة البيانات.
-   اتصال الإنترنت.

## البوت يتوقف بعد الخروج من Termux

استخدم PM2:

``` bash
pm2 start ecosystem.config.js
```

ثم:

``` bash
pm2 status
```

> ملاحظة: بعض إصدارات Android/Termux قد توقف العمليات في الخلفية بسبب
> Battery Optimization. قد تحتاج إلى استثناء Termux من تحسين البطارية
> حسب جهازك.

------------------------------------------------------------------------

# الأمان

لا ترفع أيًا من التالي إلى GitHub:

``` text
.env
DISCORD_TOKEN
CLIENT_SECRET
USER_TOKEN
MONGODB_URI
BACKUP_WEBHOOK_URL
LAVALINK credentials
أي Session أو Cookie أو API Key
```

ملف `.gitignore` الموجود في المشروع مخصص للمساعدة في منع رفع الملفات
الحساسة.

إذا تسرب Bot Token أو Client Secret، قم بتغييره من Discord Developer
Portal فورًا.

------------------------------------------------------------------------

# الترخيص

المشروع يحتوي على `LICENSE` من نوع ISC حسب إعدادات المشروع الحالية.

راجع الترخيص قبل إعادة توزيع المشروع أو استخدام أجزاء منه في مشروع
تجاري.

------------------------------------------------------------------------

## الدعم والمساهمة

للمساهمة:

``` bash
git checkout -b feature/my-change
git add .
git commit -m "Add my change"
git push origin feature/my-change
```

ثم افتح Pull Request على GitHub.

