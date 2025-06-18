# 🚀 دليل تنصيب البوت على السرفر المحلي

## 📋 المتطلبات الأساسية

### 1. Python 3.8+ 
```bash
python --version
# يجب أن يكون 3.8 أو أحدث
```

### 2. Git
```bash
git --version
```

## 🔧 خطوات التنصيب

### الخطوة 1: تحميل المشروع
```bash
git clone <repository-url>
cd telegram-userbot
```

### الخطوة 2: إنشاء البيئة الافتراضية
```bash
# إنشاء البيئة الافتراضية
python -m venv venv

# تفعيل البيئة الافتراضية
# على Windows:
venv\Scripts\activate
# على Linux/Mac:
source venv/bin/activate
```

### الخطوة 3: تنصيب المكتبات
```bash
pip install -r requirements.txt
# أو
pip install telethon python-dotenv configparser
```

### الخطوة 4: إعداد ملف .env
```bash
# انسخ ملف .env النموذجي
cp .env.example .env
# أو أنشئ ملف .env جديد
```

## 🔑 الحصول على البيانات المطلوبة

### 1. API ID & API Hash
1. اذهب إلى: https://my.telegram.org/apps
2. سجل دخولك برقم هاتفك
3. انقر على "Create new application"
4. املأ البيانات المطلوبة
5. احصل على `API ID` و `API Hash`

### 2. Bot Token (للتحكم)
1. ابحث عن `@BotFather` في التيليجرام
2. أرسل `/newbot`
3. اتبع التعليمات لإنشاء بوت جديد
4. احصل على `Bot Token`

### 3. String Session
```bash
# شغل مولد String Session
python generate_session.py
# اتبع التعليمات لإدخال API ID و API Hash
# ستحصل على String Session
```

### 4. Admin User ID
1. ابحث عن `@userinfobot` في التيليجرام
2. أرسل له أي رسالة
3. احصل على `User ID` الخاص بك

### 5. معرفات القنوات
1. استخدم `@userinfobot` للحصول على معرف القناة
2. أو استخدم `@chatidbot`
3. أضف البوت للقناة وأرسل رسالة

## ⚙️ تكوين ملف .env

### نموذج الإعداد الأساسي:
```env
# بيانات التيليجرام الأساسية
TELEGRAM_API_ID=12345678
TELEGRAM_API_HASH=abcd1234efgh5678ijkl9012
TELEGRAM_BOT_TOKEN=1234567890:ABCdefGHijKLmnoPQRstUVwxYZ
TELEGRAM_STRING_SESSION=1BVtsOKoBu5...طويل جداً
TELEGRAM_ADMIN_USER_ID=123456789

# القنوات
SOURCE_CHAT_ID=-1001234567890
TARGET_CHAT_ID=-1001987654321

# الإعدادات الأساسية
FORWARD_MODE=copy
MESSAGE_DELAY=1
MAX_RETRIES=3
```

## 🚀 تشغيل البوت

### تشغيل البوت الأساسي:
```bash
python main.py
```

### تشغيل بوت التحكم:
```bash
python modern_control_bot.py
```

### تشغيل الاثنين معاً:
```bash
# في terminal منفصل لكل بوت
# Terminal 1:
python main.py

# Terminal 2:
python modern_control_bot.py
```

## 🔍 اختبار التشغيل

### 1. تحقق من الاتصال
- يجب أن تظهر رسالة "Userbot started successfully"
- يجب أن تظهر رسالة "Modern control bot started"

### 2. اختبار التوجيه
- أرسل رسالة في القناة المصدر
- تحقق من وصولها للقناة الهدف

### 3. اختبار بوت التحكم
- ابحث عن البوت في التيليجرام
- أرسل `/start`
- تحقق من ظهور قائمة التحكم

## 🛠️ استكشاف الأخطاء

### مشاكل شائعة:

#### 1. خطأ في API ID/Hash
```
AuthKeyError: Invalid authorization key
```
**الحل:** تحقق من صحة API ID و API Hash

#### 2. خطأ في String Session
```
SessionPasswordNeededError
```
**الحل:** أعد إنشاء String Session باستخدام `generate_session.py`

#### 3. خطأ في معرف القناة
```
ValueError: Could not find the input entity
```
**الحل:** تحقق من صحة معرف القناة وأن البوت عضو فيها

#### 4. خطأ في الصلاحيات
```
ChatAdminRequiredError
```
**الحل:** تأكد من أن الحساب له صلاحية في القناة المصدر

## 📊 مراقبة الأداء

### عرض السجلات:
```bash
# عرض السجلات المباشرة
tail -f userbot.log

# البحث في السجلات
grep "ERROR" userbot.log
```

### إحصائيات الأداء:
- مراقبة معدل الرسائل المرسلة
- تتبع الأخطاء والإعادات
- مراقبة استهلاك الذاكرة

## 🔒 الأمان

### تأمين الملفات:
```bash
# تقييد صلاحيات ملف .env
chmod 600 .env

# إخفاء ملفات البيانات الحساسة
echo ".env" >> .gitignore
echo "*.session" >> .gitignore
echo "session_string.txt" >> .gitignore
```

### النسخ الاحتياطي:
- احتفظ بنسخة آمنة من String Session
- أنشئ نسخة احتياطية من ملف .env
- احفظ إعدادات البوت بانتظام

## 🚀 التشغيل كخدمة

### على Linux باستخدام systemd:

1. إنشاء ملف الخدمة:
```bash
sudo nano /etc/systemd/system/telegram-userbot.service
```

2. محتوى الملف:
```ini
[Unit]
Description=Telegram Userbot
After=network.target

[Service]
Type=simple
User=your-username
WorkingDirectory=/path/to/telegram-userbot
Environment=PATH=/path/to/telegram-userbot/venv/bin
ExecStart=/path/to/telegram-userbot/venv/bin/python main.py
Restart=always
RestartSec=10

[Install]
WantedBy=multi-user.target
```

3. تفعيل الخدمة:
```bash
sudo systemctl daemon-reload
sudo systemctl enable telegram-userbot
sudo systemctl start telegram-userbot
```

## 📞 الدعم

### في حالة وجود مشاكل:
1. تحقق من السجلات أولاً
2. راجع هذا الدليل
3. تأكد من صحة جميع البيانات في .env
4. أعد تشغيل البوت

### ملفات مفيدة للدعم:
- `userbot.log` - سجل البوت الأساسي
- `config.ini` - إعدادات البوت
- `.env` - المتغيرات البيئية (لا تشارك محتواه!)

---

🎉 **مبروك! البوت جاهز للعمل على السرفر المحلي**