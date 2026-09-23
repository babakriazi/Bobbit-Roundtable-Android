# Bobbit Roundtable — Android (Capacitor)

اپلیکیشن اندروید «میزگرد بابیت» — چت با مدل‌های هوش مصنوعی و برگزاری کنفرانس چند-مدلی (Multi-Agent Conference).

این پروژه نسخه اندرویدی (بر پایه Capacitor) از برنامه دسکتاپ Bobbit Roundtable است. رابط کاربری کامل به صورت وب (HTML/JS) در پوشه `www/` پیاده‌سازی شده و Capacitor آن را داخل یک اپ اندروید بسته‌بندی می‌کند.

## ساختار پروژه

```
├── package.json                      # تنظیمات npm و وابستگی‌های Capacitor
├── capacitor.config.json             # پیکربندی Capacitor (appId, webDir و ...)
├── www/
│   └── index.html                    # رابط کاربری کامل اپ (چت، کنفرانس، تنظیمات)
└── .github/
    └── workflows/
        └── build-apk.yml             # GitHub Action برای ساخت خودکار APK
```

## ✅ روش ۱: ساخت APK با GitHub Actions (پیشنهادی — بدون نیاز به نصب چیزی روی سیستم شما)

1. **فایل zip را باز کنید** و محتویات آن را در یک پوشه داشته باشید.
2. وارد [github.com](https://github.com) شوید و روی **New repository** کلیک کنید.
3. نام مخزن را مثلاً `bobbit-roundtable-android` بگذارید و **Create repository** را بزنید.
4. در صفحه مخزن روی **uploading an existing file** کلیک کنید (یا از تب Add file → Upload files).
5. **تمام فایل‌ها و پوشه‌ها** (package.json، capacitor.config.json، پوشه www، پوشه .github) را بکشید و رها کنید.
   - توجه: پوشه `.github` با درگ‌اندراپ در برخی مرورگرها آپلود نمی‌شود؛ اگر چنین شد، فایل workflow را از طریق **Add file → Create new file** با مسیر `.github/workflows/build-apk.yml` بسازید و محتوای فایل را paste کنید.
6. پایین صفحه یک **Commit message** بنویسید (مثلاً `Initial commit`) و **Commit changes** را بزنید.
7. به تب **Actions** بروید. ورک‌فلو **Build Android APK** به صورت خودکار اجرا می‌شود (چند دقیقه طول می‌کشد).
8. پس از اتمام (تیک سبز ✓)، روی همان اجرا کلیک کنید و در بخش **Artifacts** فایل `bobbit-roundtable-debug-apk` را دانلود کنید.
9. فایل zip دانلودشده را باز کنید؛ داخل آن فایل `app-debug.apk` قرار دارد و می‌توانید آن را روی گوشی نصب کنید.

> نکته: به Actions دسترسی بدهید اگر خطا گرفتید: Settings → Actions → General → Workflow permissions → **Read and write permissions**.

## ✅ روش ۲: آپلود با Git از طریق خط فرمان

```bash
# داخل پوشه‌ای که فایل‌های پروژه را باز کرده‌اید:
git init
git add .
git commit -m "Initial commit: Bobbit Roundtable Android"

# مخزن خود را در گیت‌هاب بسازید (خالی، بدون README) سپس:
git branch -M main
git remote add origin https://github.com/USERNAME/bobbit-roundtable-android.git
git push -u origin main
```

پس از push، به تب **Actions** در گیت‌هاب بروید تا APK ساخته و به عنوان Artifact آماده دانلود شود.

## روش ۳: ساخت APK به صورت محلی (اختیاری)

پیش‌نیازها: Node.js 18+، JDK 17، Android Studio (یا Android SDK).

```bash
npm install
npx cap add android     # فقط بار اول
npx cap sync android
cd android && ./gradlew assembleDebug
# خروجی: android/app/build/outputs/apk/debug/app-debug.apk
```

## استفاده از اپ

- در تب **Settings**، کلید API و Base URL سرویس سازگار با OpenAI (مثلاً `https://llm.simplellms.com/v1`) را وارد و ذخیره کنید.
- دکمه **Fetch from Server** لیست مدل‌ها را از سرور می‌گیرد؛ با **Test All** وضعیت هر مدل را بررسی کنید.
- در تب **Conference** برای هر مدل یک نقش (Role) تعریف کنید و موضوع را ارسال کنید تا مدل‌ها به ترتیب صحبت کنند.
- اگر مدل خطاب مستقیم شود، بقیه با `[SKIP]` نوبت خود را رد می‌کنند.
- تنظیمات و کلید API در `localStorage` دستگاه ذخیره می‌شود (روی سرور ارسال نمی‌شود).

## امنیت

- کلید API شما فقط روی دستگاه خودتان ذخیره می‌شود و در این مخزن قرار نمی‌گیرد.
- هرگز فایل `config` محلی یا کلید API را commit نکنید.

## مجوز

MIT
