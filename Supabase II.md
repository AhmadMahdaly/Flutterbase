## تنفيذ العمليات الأساسية للـ Authentication:
- وفي فايل `auth_check.dart` لعمل check على التسجيل نضع الكود:
```dart
import 'package:flutter/material.dart';
import 'package:supabase_flutter/supabase_flutter.dart';

class AuthCheck extends StatefulWidget {
  const AuthCheck({super.key});

  @override
  State<AuthCheck> createState() => _AuthCheckState();
}

class _AuthCheckState extends State<AuthCheck> {
  /*Retrieve the current user and assign the value to the _user variable. Notice that
this page sets up a listener on the user's auth state using onAuthStateChange. */
  User? _user;
  @override
  void initState() {
    _getAuth();
    super.initState();
  }

  Future<void> _getAuth() async {
    if (mounted) {
      setState(() {
        _user = supabase.auth.currentUser;
      });
    }

    supabase.auth.onAuthStateChange.listen((event) {
      if (mounted) {
        setState(() {
          _user = event.session?.user;
        });
      }
    });
  }

  final SupabaseClient supabase = Supabase.instance.client;
  @override
  Widget build(BuildContext context) {
    return _user == null ? const OnboardScreen() : const NavigationBarApp();
  }
}
```
- ولإضافة مستخدم جديد (Sign Up):
في `StatefulWidget`
```dart
  final SupabaseClient supabase = Supabase.instance.client;
  final emailController = TextEditingController();
  final passwordController = TextEditingController();
  final userNameController = TextEditingController();
  String? userId;
  String? userName, email, password;

  @override
  void dispose() {
    emailController.dispose();
    passwordController.dispose();
    super.dispose();
  }
```
```dart
CustomTextformfield(
                      text: 'الاسم',
                      controller: userNameController,),
 CustomTextformfield(
                      controller: emailController,
                      text: 'البريد الإلكتروني',
),
         CustomTextformfield(
                      controller: passwordController,
                                         text: 'كلمة المرور',),
```
```dart
  onTap: () async {
                final email = emailController.text;
                final userName = userNameController.text;
             

                  try {
                    final response = await supabase.auth.signUp(
                        email: emailController.text.trim(),
                        password: passwordController.text.trim(),
                        data: {'name': userName});
/// لتسجيل اسم الuser لاظهاره تاليا في التطبيق
                    if (response.user != null) {
                      /// إضافة اسم المستخدم إلى جدول "users" بعد نجاح التسجيل
                      await supabase.from('users').insert({
                        'id': response.user?.id, // ربط المستخدم باستخدام UID
                        'username': userName,
                        'email': email,
                      });
                    }
 userId = response.user!.id;
                    /// showSnackBar 
                   /// Navigator
                     
                   
                  

                  } catch (e) {
                    print(e.toString());
                    /// showSnackBar
                   
                }
              },
```
