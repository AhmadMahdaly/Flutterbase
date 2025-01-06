# 1/ ما هي Firebase ولماذا نستخدمها؟
## هي منصة (تابعة لجوجل) توفر أدوات وخدمات لتطوير التطبيقات على الويب والجوال.

- إدارة قواعد البيانات (Database) -في الوقت الحقيقي أو NoSQL-.
- المصادقة (Authentication).
- إرسال الإشعارات (Notifications).
- تخزين الملفات مثل الصور والفيديوهات.
- إدارة استضافة التطبيقات (Hosting).
- مراقبة أداء التطبيق (Analytics and Performance Monitoring).

## 2/ إنشاء مشروع Firebase وربطه بـ Flutter
- فتح Firebase Console.
- تسجيل الدخول.
- انقر على "Add Project".
- أدخل اسم المشروع (مثلاً: my_flutter_app).
- إلغي تفعيل Google Analytics إن لم تكن بحاجة له.
- انقر على "Create Project" وانتظر حتى يتم الإنشاء.
- 
### إضافة تطبيق Android:
- انقر على أيقونة Android في لوحة Firebase.
- أدخل التفاصيل التالية:
 - **Android package name**: هو اسم الحزمة لتطبيقك (يمكنك العثور عليه في ملف `android/app/build.gradle` تحت `applicationId`).
 - **App nickname**: (اختياري) اسم التطبيق في Firebase.
 - **SHA-1**: (اختياري الآن، لكنه ضروري لبعض الميزات مثل المصادقة باستخدام Google).
  نفتح Command Prompt على مسار المشروع في فولدر `android` وأنفذ الأمر التالي لتظهر معلومات SHA-1 و SHA-256 ضمن التقرير:
 ```
gradlew signingReport
```
- انقر على "Register App".
- بعد التسجيل، سيتم توليد ملف `google-services.json`. قم بتنزيله وضعه في مسار: `android/app/`
- إعداد ملفات Gradle:
افتح ملف `android/app/build.gradle` وأضف السطر التالي في `plugins`:
```gradle
id 'com.google.gms.google-services' version '4.4.2' apply false
```

- إضافة مكتبات Firebase إلى Flutter في ملف `pubspec.yaml`.

```yaml
dependencies:
  firebase_core: ^2.15.1
  firebase_auth: ^4.7.0
  cloud_firestore: ^5.8.0
```

- تهيئة Firebase في كود التطبيق: افتح ملف `main.dart` وأضف التالي في بداية main():

```dart
import 'package:firebase_core/firebase_core.dart';

void main() async {
  WidgetsFlutterBinding.ensureInitialized();
  Platform.isAndroid
      ? await Firebase.initializeApp(
          options: const FirebaseOptions(
            apiKey: ' // currentKey موجود في ملف جوجل سيرفيس',
            appId: ' // mobileSDKappId موجود في ملف جوجل سيرفيس',
            messagingSenderId: ' // project_number موجود في ملف جوجل سيرفيس',
            projectId: ' // project_id موجود في ملف جوجل سيرفيس',
          ),
        )
      : await Firebase.initializeApp();
  runApp(MyApp());
}
```
