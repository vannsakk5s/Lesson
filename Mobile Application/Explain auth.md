# ការពន្យល់អំពី Authentication System (Flutter)

---

## ផ្នែកទី ១៖ ដំណើរការទូទៅ និងតួនាទីរបស់ File

### ១. Register ដំណើរការយ៉ាងដូចម្តេច?
ក្នុង `register_page.dart` អ្នកប្រើបញ្ចូល៖
- **Name**
- **Email**
- **Password**
- **Confirm Password**

ពេលចុច **Create Account**, function `_handleRegister()` ពិនិត្យ៖
- Name មានយ៉ាងតិច ២ តួអក្សរ។
- Email មានទម្រង់ត្រឹមត្រូវ។
- Password មានយ៉ាងតិច ៦ តួអក្សរ។
- Confirm Password ដូច Password។
- អ្នកប្រើបានយល់ព្រមលើ Terms។

បើត្រឹមត្រូវ វាហៅ៖
```dart
await AuthService.instance.register(name, email, password);
```

`AuthService` បញ្ជូនការងារទៅ `ApiService` ដែលផ្ញើ៖
```http
POST /api/v1/auth/register
Content-Type: application/json

{
  "name": "Sak",
  "email": "sak@example.com",
  "password": "example123"
}
```

> **ចំណាំ:** Confirm Password មិនផ្ញើទៅ API ទេ ព្រោះវាប្រើសម្រាប់ពិនិត្យនៅ Flutter ប៉ុណ្ណោះ។  
> កូដរំពឹងថា API ឆ្លើយតប `201 Created` ជាមួយទិន្នន័យ User។ បន្ទាប់មក Flutter ព្យាយាម Login ដោយស្វ័យប្រវត្តិ ហើយបើកទំព័រ `/main`។

---

### ២. Login ដំណើរការយ៉ាងដូចម្តេច?
ក្នុង `login_page.dart` អ្នកប្រើបញ្ចូល Email និង Password ហើយចុច **Sign In**។  
`_handleLogin()` ពិនិត្យទម្រង់ Email និង Password មិនទទេ រួចហៅ៖
```dart
final response = await AuthService.instance.login(email, password);
```

`ApiService` ផ្ញើ៖
```http
POST /api/v1/auth/login
Content-Type: application/json

{
  "email": "sak@example.com",
  "password": "example123"
}
```

បើ API ឆ្លើយតប `200 OK`, កូដបម្លែង JSON ទៅជា `AuthResponse` ដែលមាន៖
- `access_token`
- `refresh_token`
- `token_type`
- `user`

`AuthService` រក្សាទុក Token និង User ក្នុង memory និង `SharedPreferences` ដើម្បីអាចផ្ទុក session ឡើងវិញពេលបើក App។  
ចូលបានជោគជ័យ វាបង្ហាញ *"Welcome back, Sak!"* ហើយត្រឡប់ទៅទំព័រមុន ប្រសិនបើមាន ឬបើក `/main`។

---

### ៣. File នីមួយៗមានតួនាទីអ្វី?

| File | តួនាទី |
| :--- | :--- |
| `login_page.dart` | Form Login, validation និងបង្ហាញ error |
| `register_page.dart` | Form Register និង auto-login |
| `auth_service.dart` | គ្រប់គ្រង Token, User, session និង Logout |
| `api_service.dart` | ផ្ញើ HTTP request ទៅ Backend |
| `user_model.dart` | បម្លែង JSON ទៅ `UserModel` និង `AuthResponse` |
| `app_routes.dart` | កំណត់ route `/login`, `/register`, `/main` |
| `main.dart` | ផ្ទុក session មុនដំណើរការ App |
| `home.dart` / `profile.dart` | បង្ហាញ User តាមស្ថានភាព Login |

> ក្នុង Home និង Profile មាន `ValueListenableBuilder` ស្ដាប់ `userNotifier`។ ពេល Login ឬ Logout ផ្លាស់ប្ដូរ User នោះ UI update ដោយស្វ័យប្រវត្តិ។

---

### ៤. Token ប្រើសម្រាប់អ្វី?
ពេលទាញ Profile កូដផ្ញើ Access Token ក្នុង header៖
```http
GET /api/v1/auth/me
Authorization: Bearer <access_token>
```

Backend អាចប្រើ Token នេះដើម្បីកំណត់ថា request ជារបស់អ្នកប្រើណា។  
ពេល Logout, `AuthService.logout()` លុប Token និង User ចេញពី memory និង `SharedPreferences`។

#### ចំណុចដែលកូដបច្ចុប្បន្នមាន៖
- **Remember me** មាន checkbox ប៉ុន្តែមិនទាន់ភ្ជាប់ជាមួយការរក្សាទុក session ទេ (Login តែងតែរក្សាទុក session)។
- មាន function `refreshToken()` ប៉ុន្តែមិនទាន់ឃើញលំហូរ auth ហៅវាដោយស្វ័យប្រវត្តិពេល Token ផុតកំណត់។
- បើ Register ជោគជ័យ តែ auto-login បរាជ័យ កូដនៅតែបើក `/main`។
- Repository នេះបង្ហាញផ្នែក Flutter។ ការពិនិត្យ Password, ការរក្សាទុកក្នុង Database និងការបង្កើត Token នៅ Backend មិនអាចបញ្ជាក់ពីកូដនេះបានទេ។

---

## ផ្នែកទី ២៖ តើទិន្នន័យរក្សាទុកនៅទីណា? (Where is it stored?)

ក្នុង Project របស់អ្នក Token និងព័ត៌មាន User រក្សាទុកនៅលើឧបករណ៍ដែលដំណើរការ App ដោយប្រើ **`SharedPreferences`**។

| Key | ទិន្នន័យរក្សាទុក |
| :--- | :--- |
| `auth_access_token` | Access Token |
| `auth_refresh_token` | Refresh Token |
| `auth_user_data` | User ជា JSON៖ `id`, `name`, `email`, `role`, `created_at` |

កូដរក្សាទុកនៅក្នុង `auth_service.dart`៖
```dart
final prefs = await SharedPreferences.getInstance();

await prefs.setString('auth_access_token', response.accessToken);
await prefs.setString('auth_refresh_token', response.refreshToken);
await prefs.setString(
  'auth_user_data',
  jsonEncode(response.user.toJson()),
);
```

- **Android/iOS:** រក្សាទុកក្នុង local storage របស់ App លើទូរស័ព្ទ។
- **Web:** រក្សាទុកក្នុង `localStorage` របស់ Browser។
- **Password:** កូដ Flutter នេះមិនរក្សាទុក Password ទេ។
- ពេលបិទហើយបើក App វិញ `init()` អានទិន្នន័យនេះឡើងវិញ។ ពេល Logout វាលុប key ទាំងបី។
- **ចំណែក គណនីដែលបង្កើតពេល Register:** គឺផ្ញើទៅ Backend ដើម្បីរក្សាទុក (ឈ្មោះ Database និង Table ត្រូវមើលកូដ Backend ទើបដឹង)។

---

## ផ្នែកទី ៣៖ ដំណើរការជំហានម្តងមួយៗ (Step by Step Flow)

លំហូរ Register → Login → រក្សាទុក Session → Profile → Logout តាមកូដ Project៖

### ១. ពេលបើក App
ក្នុង `main.dart`៖
```dart
WidgetsFlutterBinding.ensureInitialized();
await AuthService.instance.init();
```

`init()` ក្នុង `auth_service.dart` អានទិន្នន័យដែលធ្លាប់រក្សាទុក៖
```dart
final prefs = await SharedPreferences.getInstance();

_accessToken = prefs.getString('auth_access_token');
_refreshToken = prefs.getString('auth_refresh_token');

final userJsonStr = prefs.getString('auth_user_data');
```
បើមាន User JSON វាបម្លែងទៅ `UserModel` ហើយ update `userNotifier`។ បើមាន Access Token វាព្យាយាមទាញ Profile ថ្មីពី API។

---

### ២. អ្នកប្រើបើក Register
Route `/register` បើក `RegisterPage`។  
អ្នកប្រើបញ្ចូល៖
- **Name:** Sak
- **Email:** sak@example.com
- **Password:** example123
- **Confirm Password:** example123

`TextEditingController` ទទួលអត្ថបទពី field នីមួយៗ។

---

### ៣. ចុច Create Account
Function `_handleRegister()` ពិនិត្យ Form៖
```dart
if (!_formKey.currentState!.validate()) return;
```

វាពិនិត្យ៖
- Name យ៉ាងតិច ២ តួអក្សរ
- Email មានទម្រង់ត្រឹមត្រូវ
- Password យ៉ាងតិច ៦ តួអក្សរ
- Confirm Password ត្រូវគ្នា
- បានយល់ព្រមលើ Terms

បើមិនត្រឹមត្រូវ វាបង្ហាញ error ហើយមិនផ្ញើ request ទេ។

---

### ៤. Register Page ហៅ AuthService
```dart
await AuthService.instance.register(
  name,
  email,
  password,
);
```

ក្នុង `auth_service.dart`៖
```dart
Future<UserModel> register(
  String name,
  String email,
  String password,
) async {
  return await ApiService.authRegister(name, email, password);
}
```
`AuthService` បញ្ជូនការងារទៅ `ApiService`។

---

### ៥. ApiService ផ្ញើ Register ទៅ Backend
Endpoint កំណត់ក្នុង `app_config.dart`៖
```http
POST /api/v1/auth/register
```

Request body៖
```json
{
  "name": "Sak",
  "email": "sak@example.com",
  "password": "example123"
}
```

កូដផ្ញើ៖
```dart
final body = jsonEncode({
  'name': name,
  'email': email,
  'password': password,
});

final response = await http.post(
  Uri.parse(AppConfig.authRegisterEndpoint),
  headers: _defaultHeaders,
  body: body,
);
```

> Confirm Password មិនផ្ញើទៅ Backend ទេ។  
> កូដ Flutter រំពឹង status `201` និង User JSON។ ការរក្សាទុកគណនីក្នុង Database គឺជាការងាររបស់ Backend ដែលមិនមានក្នុង repository នេះ។

---

### ៦. Register ជោគជ័យ → ព្យាយាម Auto-login
ក្នុង `_handleRegister()`៖
```dart
await AuthService.instance.register(name, email, password);

try {
  await AuthService.instance.login(email, password);
} catch (_) {
  // Register ជោគជ័យ ទោះ auto-login បរាជ័យ
}
```

បន្ទាប់មកវាបើក `/main` និងលុប route មុនៗ៖
```dart
Navigator.pushNamedAndRemoveUntil(
  context,
  AppRoutes.main,
  (route) => false,
);
```
> **ចំណាំ:** កូដបច្ចុប្បន្នបើក `/main` ទោះ auto-login បរាជ័យក៏ដោយ។

---

### ៧. បើអ្នកប្រើ Login ដោយខ្លួនឯង
អ្នកប្រើបើក `/login` បញ្ចូល Email និង Password ហើយចុច **Sign In**។  
ក្នុង `_handleLogin()`៖
```dart
if (!_formKey.currentState!.validate()) return;

final email = _emailController.text.trim();
final password = _passwordController.text;

final response = await AuthService.instance.login(email, password);
```
វាពិនិត្យ Email និង Password មិនទទេ មុនហៅ API។

---

### ៨. ApiService ផ្ញើ Login
```http
POST /api/v1/auth/login
```

Body៖
```json
{
  "email": "sak@example.com",
  "password": "example123"
}
```

បើ API ឆ្លើយ status `200`, JSON ត្រូវបានបម្លែងទៅ `AuthResponse`៖
```dart
return AuthResponse.fromJson(data);
```

Model នេះរំពឹងទិន្នន័យ៖
- `access_token`
- `refresh_token`
- `token_type`
- `user`

បើ API ឆ្លើយ error កូដបង្កើត `ApiException` ហើយ Login Page បង្ហាញសារ error។

---

### ៩. AuthService រក្សាទុកក្នុង Memory
បន្ទាប់ពី Login ជោគជ័យ៖
```dart
_accessToken = response.accessToken;
_refreshToken = response.refreshToken;
_currentUser = response.user;

userNotifier.value = _currentUser;
```

- **Memory** គឺទិន្នន័យដែល App កំពុងកាន់ពេលដំណើរការ (RAM)។
- `userNotifier` ជូនដំណឹងទៅ Home និង Profile ឱ្យបង្ហាញព័ត៌មាន User ថ្មី។

---

### ១០. រក្សាទុកលើឧបករណ៍ (Local Storage)
កូដរក្សាទុកក្នុង `SharedPreferences` ផងដែរ៖
```dart
final prefs = await SharedPreferences.getInstance();

await prefs.setString(
  'auth_access_token',
  response.accessToken,
);

await prefs.setString(
  'auth_refresh_token',
  response.refreshToken,
);

await prefs.setString(
  'auth_user_data',
  jsonEncode(response.user.toJson()),
);
```

| ទិន្នន័យ | ទីតាំង |
| :--- | :--- |
| **Token និង User កំពុងប្រើ** | Memory របស់ App |
| **Token និង User សម្រាប់បើក App វិញ** | `SharedPreferences` លើឧបករណ៍ |
| **Password** | មិនរក្សាទុកក្នុង Flutter code នេះទេ |
| **គណនីដែល Register** | Backend ទទួល (ត្រូវមើលកូដ Backend ដើម្បីដឹង Database) |

---

### ១១. Home និង Profile Update
ទំព័រទាំងនេះស្ដាប់ `userNotifier`៖
```dart
ValueListenableBuilder<UserModel?>(
  valueListenable: AuthService.instance.userNotifier,
  builder: (context, user, _) {
    return Text(user?.name ?? 'Guest');
  },
)
```

របៀបដែលវាដំណើរការ៖
- **មាន User** → បង្ហាញឈ្មោះ និងប៊ូតុង Logout។
- **User ជា null** → បង្ហាញស្ថានភាព Guest និងប៊ូតុង Sign In។

---

### ១២. ទាញ Profile ដោយប្រើ Token
`refreshUserProfile()` ហៅ៖
```dart
await ApiService.getProfile(_accessToken!);
```

Request៖
```http
GET /api/v1/auth/me
Authorization: Bearer <access_token>
```

បើទាញបាន វា update User ក្នុង memory, `userNotifier` និង `SharedPreferences`។  
Access Token ប្រើជាភស្តុតាងសម្រាប់ Backend កំណត់អត្តសញ្ញាណអ្នកប្រើ។

---

### ១៣. បិទ App ហើយបើកវិញ
1. Memory របស់ App ត្រូវបានសម្អាតឡើងវិញ ប៉ុន្តែទិន្នន័យដែលបានរក្សាទុកក្នុង `SharedPreferences` នៅតែអាចអានបាន។
2. `main.dart` ហៅ `init()` ម្ដងទៀត ដើម្បីផ្ទុក Token និង User ពី `SharedPreferences` ចូល memory វិញ។
3. **ចំណាំ:** កូដបច្ចុប្បន្នមិនទាន់ភ្ជាប់ការបន្ត Token (`refreshToken`) ដោយស្វ័យប្រវត្តិទេ ដូច្នេះការមាន Token ដែលបានរក្សាទុកមិនធានាថាវានៅមានសុពលភាពឡើយ។

---

### ១៤. ពេលចុច Logout
Profile ហៅ៖
```dart
await AuthService.instance.logout();
```

វាសម្អាត memory៖
```dart
_accessToken = null;
_refreshToken = null;
_currentUser = null;
userNotifier.value = null;
```

ហើយលុបទិន្នន័យលើឧបករណ៍៖
```dart
await prefs.remove('auth_access_token');
await prefs.remove('auth_refresh_token');
await prefs.remove('auth_user_data');
```

- Home និង Profile update ត្រឡប់ទៅស្ថានភាព **Guest** ដោយស្វ័យប្រវត្តិ។
- Logout មិនលុបគណនីក្នុង Database ទេ ហើយកូដនេះមិនផ្ញើ request ទៅ Backend ដើម្បីដកហូត Token ទេ។
