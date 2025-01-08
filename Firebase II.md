# 4/ استخدام Firestore لحفظ وإدارة بيانات المستخدمين:
### Firestore هي قاعدة بيانات NoSQL مقدمة من Firebase، تتيح تخزين البيانات في الوقت الحقيقي ومزامنتها بين الأجهزة.

- افتح Firebase Console واختر مشروعك.
- انقر على Firestore Database من الشريط الجانبي.
- اختر Create Database.
- اختر وضع الأمان:
  `Test mode`: للمشاريع التجريبية (يمكنك التغيير لاحقًا).
  `Locked mode`: للمشاريع الإنتاجية.
- Next، واختر موقع الخادم (اترك الخيار الافتراضي إن لم تكن بحاجة إلى تغيير).
  

## يمكنك كتابة بيانات إلى قاعدة Firestore باستخدام الكود التالي:

```dart
import 'package:cloud_firestore/cloud_firestore.dart';

Future<void> addUser(String userId, String name, String email) async {
  try {
    await FirebaseFirestore.instance.collection('users').doc(userId).set({
      'name': name,
      'email': email,
      'createdAt': FieldValue.serverTimestamp(),
    });
    print('User added successfully');
  } catch (e) {
    print('Error: $e');
  }
}
```
- استخدام الوظيفة:

```dart
ElevatedButton(
  onPressed: () {
    addUser('12345', 'John Doe', 'johndoe@example.com');
  },
  child: Text('Add User'),
),
```

## قراءة بيانات من Firestore:
- لاسترجاع البيانات المخزنة:

```dart
Future<void> getUser(String userId) async {
  try {
    DocumentSnapshot userDoc = await FirebaseFirestore.instance
        .collection('users')
        .doc(userId)
        .get();
    print('User data: ${userDoc.data()}');
  } catch (e) {
    print('Error: $e');
  }
}
```
- استخدام الوظيفة:

```dart
ElevatedButton(
  onPressed: () {
    getUser('12345');
  },
  child: Text('Get User'),
),
```

## تحديث بيانات في Firestore:
- لتحديث بيانات موجودة:

```dart
Future<void> updateUser(String userId, String newName) async {
  try {
    await FirebaseFirestore.instance
        .collection('users')
        .doc(userId)
        .update({'name': newName});
    print('User updated successfully');
  } catch (e) {
    print('Error: $e');
  }
}
```
- استخدام الوظيفة:

```dart
ElevatedButton(
  onPressed: () {
    updateUser('12345', 'Jane Doe');
  },
  child: Text('Update User'),
),
```

## حذف بيانات من Firestore:
- لحذف مستند:

```dart
Future<void> deleteUser(String userId) async {
  try {
    await FirebaseFirestore.instance.collection('users').doc(userId).delete();
    print('User deleted successfully');
  } catch (e) {
    print('Error: $e');
  }
}
```
- استخدام الوظيفة:

```dart
ElevatedButton(
  onPressed: () {
    deleteUser('12345');
  },
  child: Text('Delete User'),
),
```

### ملاحظات إضافية:
- يمكنك مشاهدة التغييرات في قاعدة البيانات في الوقت الحقيقي باستخدام snapshots():

```dart
FirebaseFirestore.instance.collection('users').snapshots().listen((snapshot) {
  for (var doc in snapshot.docs) {
    print(doc.data());
  }
});
```
### يمكن استخدام query لتصفية البيانات:
- مثال: جلب المستخدمين الذين أسماؤهم تبدأ بـ "J":

```dart
FirebaseFirestore.instance
    .collection('users')
    .where('name', isGreaterThanOrEqualTo: 'J')
    .snapshots()
    .listen((snapshot) {
  for (var doc in snapshot.docs) {
    print(doc.data());
  }
});
```
