# QR Pay App (Android + Kotlin)

Ye app user ko text/UPI ID/link daalne deti hai, **₹1 payment seedhe aapke UPI ID par**
(koi gateway/API key ke bina — direct PhonePe/GPay/Paytm se) lene ke baad QR code generate
karti hai, aur usse save/share karne deti hai.

## Kaise kaam karta hai
1. User text/link daalta hai.
2. "₹1 Pay Karke QR Banayein" dabata hai.
3. Phone mein installed UPI apps (PhonePe, GPay, Paytm, etc.) ki list khulti hai.
4. User payment karta hai — paisa **seedha aapke UPI ID** (jo aapne set kiya hai) mein jaata hai.
5. Payment success hone par app QR code generate kar deti hai (ZXing library se, locally,
   koi internet call ki zaroorat nahi).
6. QR ko gallery mein save ya kisi ko share kiya ja sakta hai.

## 📱 Sirf Phone Se APK Banana (Computer ke bina)

Agar aapke paas computer nahi hai, to GitHub (free website) ka use karke cloud mein
apne aap APK build ho jayegi. Sab kuch phone ke browser (Chrome) se ho jayega:

1. **GitHub account banayein**: phone browser mein https://github.com par jaake free
   account sign up karein.

2. **Naya repository banayein**: GitHub par upar-right "+" icon → "New repository" →
   koi bhi naam dein (jaise `qr-pay-app`) → **Private** ya Public, jo chahein → "Create repository".

3. **Files upload karein**: Repository page par "Add file" → "Upload files" dabayein.
   Phone ke file manager se `QRPayApp` folder (jo ZIP se extract kiya tha) ke **andar ke
   saare files aur folders select karke** upload karein (folder structure preserve rehta
   hai). Sab upload hone ke baad neeche "Commit changes" dabayein.
   - Agar phone browser folder upload mein dikkat kare, to Chrome browser use karein
     (Desktop mode on karke) — usmein folder drag-drop/upload better kaam karta hai.

4. **Build apne aap shuru ho jayega**: Maine project mein already ek automatic build
   file (`.github/workflows/build.yml`) daal di hai. Jaise hi aap files upload karenge,
   GitHub khud APK banana shuru kar dega.

5. **APK download karein**: Repository ke top mein "Actions" tab par jaayein → sabse upar
   wali (latest) build par click karein → thoda neeche scroll karke "Artifacts" section
   mein "app-debug-apk" milega → usse tap karke download karein (ye ek ZIP hoga, usme
   andar `app-debug.apk` hoga).

6. **APK install karein**: ZIP se APK nikal ke phone mein tap karein. Agar "Install
   blocked" ka message aaye, to Settings mein "Install from unknown sources" allow
   kar dein us specific app (jaise Chrome/Files) ke liye.

⏱️ Build hone mein 3-5 minute lagte hain. Agar build "fail" (❌ laal cross) dikhaye, to
mujhe screenshot bhej dein, main error dekh kar fix kar dunga.

---

## Setup Steps (Computer/Android Studio ke saath — agar aage kabhi available ho)

### 1. Apna UPI ID set karein
`app/src/main/java/com/example/qrpayapp/MainActivity.kt` file kholein aur ye lines dhundhein:
```kotlin
private val MY_UPI_ID = "6304179481@fam"
private val MY_NAME = "QR Pay App"
```
Yahan apni actual UPI ID aur naam daal dein.

### 2. Amount change karna ho to
```kotlin
private val AMOUNT_RUPEES = "1.00"
```
Isko rupees mein likhein (₹2 ke liye "2.00", ₹5 ke liye "5.00", etc.)

### 3. Android Studio mein open karein
- Android Studio (latest stable, 2023.x ya naya) install karein.
- "Open" karke is poore `QRPayApp` folder ko select karein.
- Gradle sync hone dein (internet chahiye, dependencies download hongi).

### 4. Build & Run
- Real phone par test karein (emulator mein UPI apps generally install nahi hoti).
- Phone mein koi UPI app (PhonePe/GPay/Paytm) already installed honi chahiye.

## ⚠️ Zaroori baatein (production se pehle padhein)

1. **UPI response reliability**: Kuch UPI apps payment ke baad "Status=SUCCESS" wala proper
   response wapas nahi bhejti (ye Android UPI intent standard ki known limitation hai — har
   app isko sahi se implement nahi karti). Isliye occasionally aisा ho sakta hai ki payment
   ho gaya ho par app ko pata na chale. Agar aisa baar-baar ho, to real production app ke liye
   ek proper payment gateway (Razorpay/Cashfree/PayU) use karna zyada reliable hota hai, jisme
   server-side se confirm hota hai ki paisa aaya ya nahi.

2. **No verification against fraud**: Ye simple version sirf UPI app ke response par bharosa
   karti hai. Koi bhi cheez server par verify nahi hoti. Chhote/personal use ke liye theek hai,
   lekin agar bahut saare unknown users se paise lene hain to isse fraud ka risk thoda zyada hai.

3. **Play Store balance**: Google Play ka apna wallet/balance sirf Play Store ki purchases
   (apps, in-app items) ke liye use hota hai — usse UPI ID par ya kisi third-party app mein
   transfer karna Google technically allow nahi karta. Isliye ye feature is app mein add
   nahi kiya gaya.

4. **App icon**: Filhaal ek simple placeholder icon diya gaya hai
   (`app/src/main/res/drawable/ic_launcher.xml`). Chahen to Android Studio ke
   "Image Asset Studio" (right-click res folder → New → Image Asset) se apna khud ka
   professional icon bana sakte hain.

## Folder Structure
```
QRPayApp/
├── app/
│   ├── build.gradle
│   └── src/main/
│       ├── AndroidManifest.xml
│       ├── java/com/example/qrpayapp/MainActivity.kt
│       └── res/
│           ├── layout/activity_main.xml
│           ├── values/ (strings, colors, themes)
│           └── drawable/ic_launcher.xml
├── build.gradle
├── settings.gradle
└── gradle.properties
```
