# আলোর দিশারী ইসলামি সমাজ সংঘ — Membership Management System

মধ্য ইলশা, বাঁশখালী, চট্টগ্রামের একটি অরাজনৈতিক, অলাভজনক সামাজিক সংগঠনের জন্য তৈরি করা সদস্য ব্যবস্থাপনা, চাঁদা/সহায়তা সংগ্রহ ও তহবিল হিসাব রাখার সম্পূর্ণ ওয়েব সিস্টেম। কোনো পেইড হোস্টিং বা ডাটাবেজ ছাড়াই সম্পূর্ণ ফ্রি — GitHub Pages + Google Sheets + Google Apps Script দিয়ে চলে।

**লাইভ সাইট:** `https://nyousuf-cloud.github.io/...`

---

## 📁 ফাইল স্ট্রাকচার

| ফাইল | কাজ | কে ব্যবহার করবে |
|---|---|---|
| `index.html` | পাবলিক হোমপেজ — সংগঠনের পরিচিতি, কার্যক্রম, নোটিশ, কমিটি, যোগাযোগ | সবাই |
| `login.html` | সদস্য লগইন | সদস্য |
| `member-form.html` | নতুন সদস্য নিবন্ধন ফর্ম | নতুন আবেদনকারী |
| `dashboard.html` | সদস্য পোর্টাল — প্রোফাইল, পেমেন্ট হিস্ট্রি, তহবিল হিসাব (view-only), নোটিশ, কমিটি | লগইন করা সদস্য |
| `payment.html` | চাঁদা/সহায়তা প্রদান পেজ | সদস্য ও সাধারণ মানুষ (guest) দুইভাবেই |
| `admin-login.html` | এডমিন লগইন | এডমিন |
| `admin-dashboard.html` | এডমিন প্যানেল — সদস্য অনুমোদন, পেমেন্ট, তহবিল হিসাব, নোটিশ, কমিটি, এডমিন ম্যানেজমেন্ট | এডমিন/সুপার এডমিন |
| `Code.gs` | ব্যাকএন্ড API (Google Apps Script) — Google Sheet-কে ডাটাবেজ হিসেবে ব্যবহার করে | Google Sheet-এর সাথে যুক্ত |

## 🔗 ফাইলগুলো কীভাবে যুক্ত

```
index.html
 ├── "সদস্য লগইন" → login.html → dashboard.html
 │                                   ├── চাঁদা জমা দিন → payment.html (memberPayment)
 │                                   ├── তহবিল হিসাব (view-only)
 │                                   └── Logout → login.html
 ├── "নিবন্ধন করুন" → member-form.html → (Pending) → login.html
 ├── "Admin লগইন" → admin-login.html → admin-dashboard.html
 │                                        ├── সদস্য অনুমোদন/বাতিল
 │                                        ├── পেমেন্ট যোগ/সম্পাদনা/যাচাই
 │                                        ├── তহবিল হিসাব (full access)
 │                                        └── নোটিশ/কমিটি/এডমিন ম্যানেজমেন্ট
 └── পাবলিক পেমেন্ট → payment.html (publicPayment, লগইন ছাড়াই)
```

সব পেজ একই ব্যাকএন্ড (`API_URL`, Google Apps Script Web App) কল করে — `fetch()` দিয়ে JSON POST রিকোয়েস্ট পাঠায়, ব্যাকএন্ড `action` অনুযায়ী কাজ করে JSON রিটার্ন করে।

## ⚙️ Tech Stack

- **Frontend:** Vanilla HTML/CSS/JavaScript (কোনো framework/build step নেই), Font Awesome 6.6, Google Fonts (Noto Sans Bengali)
- **Backend:** Google Apps Script (`Code.gs`)
- **Database:** Google Sheets (প্রতিটা শীট = একটা টেবিল: Members, Payments, Expenses, Notices, Committee, Admins, DueSettings)
- **Hosting:** GitHub Pages (static, ফ্রি)
- **File storage:** Google Drive (সদস্যের ছবি/ডকুমেন্ট আপলোডের জন্য)

## 🚀 Setup (নতুন করে বসাতে চাইলে)

1. একটা নতুন Google Sheet খুলুন।
2. **Extensions → Apps Script** এ গিয়ে `Code.gs`-এর সম্পূর্ণ কোড পেস্ট করুন।
3. `setupSheets()` ফাংশনটা একবার Run করুন (মেনু থেকে ফাংশন সিলেক্ট করে ▶️ চাপুন) — এটা সব শীট ও হেডার নিজে থেকে বানিয়ে দেবে। ডিফল্ট Super Admin তৈরি হবে:
   - Username: `admin`
   - Password: `admin123` ⚠️ **এখনই পরিবর্তন করুন**
4. **Deploy → New deployment → Web app**
   - Execute as: **Me**
   - Who has access: **Anyone**
5. Deploy করার পর যে URL পাবেন, সেটা সব HTML ফাইলে `API_URL` ভ্যারিয়েবলে বসান (প্রতিটা ফাইলের `<script>` অংশে খুঁজে পাবেন)।
6. ৭টা HTML ফাইল GitHub রিপোতে আপলোড করে GitHub Pages চালু করুন।

কোনো পরিবর্তন (Code.gs-এ) করার পর অবশ্যই **Deploy → Manage deployments → ✏️ → New version → Deploy** করতে হবে, নাহলে পরিবর্তন লাইভ হবে না।

## ✨ প্রধান ফিচার

- সদস্য নিবন্ধন, ছবি/ডকুমেন্ট আপলোড, Admin অনুমোদন/বাতিল ওয়ার্কফ্লো
- Role-based Admin permission সিস্টেম (Super Admin আলাদা আলাদা এডমিনকে নির্দিষ্ট অনুমতি দিতে পারে)
- একাধিক ক্যাটাগরির চাঁদা/সহায়তা (মাসিক চাঁদা, ইফতার, বৃক্ষরোপণ, ঈদ সহায়তা, ইত্যাদি)
- সদস্য/পাবলিক নিজে পেমেন্ট সাবমিট করতে পারে (status: **Pending Verification**) → Admin যাচাই করে **Paid** করে দেয়
- তহবিল হিসাব (Fund Ledger): Admin-এর জন্য পূর্ণ এক্সেস (আয়/ব্যয় যোগ, সম্পাদনা), সদস্যের জন্য শুধু দেখার অধিকার (view-only)
- নোটিশ বোর্ড ও কার্যনির্বাহী কমিটি তথ্য
- পাসওয়ার্ড SHA-256 হ্যাশ করে সংরক্ষণ
- মোবাইল-রেসপনসিভ ডিজাইন (সব পেজ)
- হোমপেজে "আমাদের কার্যক্রম" অটো-স্লাইডার (ছবিসহ)
- WhatsApp ফ্লোটিং চ্যাট বাটন

## 🖼️ কার্যক্রম সেকশনে ছবি যুক্ত করা

`index.html`-এর "আমাদের কার্যক্রম" স্লাইডারে ছবি বসাতে GitHub রিপোতে `images/` ফোল্ডার বানিয়ে নিচের নামে ছবি আপলোড করুন — কোড বদলানোর দরকার নেই, নাম মিললেই ছবি অটো দেখাবে:

```
images/activity-corona.jpg
images/activity-eid.jpg
images/activity-iftar.jpg
images/activity-tree.jpg
images/activity-sports.jpg
images/activity-social.jpg
```

ছবি না দেওয়া পর্যন্ত প্রতিটা স্লাইডে সংশ্লিষ্ট আইকন দেখাবে, ভাঙা ছবি দেখাবে না।

## 🔐 নিরাপত্তা সংক্রান্ত নোট

- এটা একটা static site + no-cost backend — খুব উচ্চ-সুরক্ষা প্রয়োজনীয় (যেমন ব্যাংকিং) সিস্টেমের জন্য উপযুক্ত না, তবে একটা কমিউনিটি সংগঠনের জন্য যথেষ্ট।
- Admin/Member লগইন স্ট্যাটাস ব্রাউজারের `localStorage`-এ রাখা হয় — শেয়ার করা কম্পিউটারে লগআউট করে বের হওয়া জরুরি।
- Apps Script Deploy করার সময় "Who has access: Anyone" রাখা আবশ্যক (নাহলে ফ্রন্টএন্ড থেকে API কল করা যাবে না), কিন্তু এর মানে URL জানলে কেউ সরাসরি API-ও কল করতে পারবে — তাই ডিফল্ট Admin পাসওয়ার্ড অবশ্যই বদলে নিন।

## 📞 যোগাযোগ

- ঠিকানা: মধ্য ইলশা, বাঁশখালী, চট্টগ্রাম
- ফোন/WhatsApp: 01870932446
- ইমেইল: inasbinyousuf@gmail.com
- Facebook: https://www.facebook.com/alordishari.org.2021

---
*আলোর দিশারী ইসলামি সমাজ সংঘ — একটি অরাজনৈতিক, অলাভজনক ও সামাজিক সংগঠন। ২০২১ সালে করোনাকালীন সময়ে প্রতিষ্ঠিত।*
