### មេរៀនទី១ ការចាប់ផ្តើមជាមួយភាសា Dart
- គោលបំណងនៃមេរៀន
- តើអ្វីទៅជា Dart?
- ការតម្លើងកម្មវិធី សរសេរកូដ IDE (Setting Up Your Development Environment)
- កម្មវិធីដំបូងរបស់ Dart

**១. គោលបំណងនៃមេរៀន**
- យល់អំពីមូលដ្ឋានគ្រឹះនៃភាសា Dart និងការដំឡើងកម្មវិធី ។
- ការប្រើប្រាស់ Variable, Data Types, Operator, Control Statements ។
- ការប្រើប្រាស់ Functions ដើម្បីរៀបចំកូដ ។
- ការប្រើប្រាស់ Collections ដូចជា List, Set, Map ដើម្បីគ្រប់គ្រងទិន្នន័យ ។
- យល់អំពីគោលការណ៍ Object-Oriented Programming (OOP) និង Null Safety ។
- ស្វែងយល់អំពី Asynchronous programming
- ការប្រើប្រាស់ Console Input/Output (I/O)

**២. តើអ្វីទៅជា Dart?**

Dart គឺជាភាសាសរសេរកម្មវិធី (Programming Language) ដែលបង្កើតដោយក្រុមហ៊ុន Google ។ វាត្រូវបានរចនាឡើងដើម្បីអនុញ្ញាតសម្រាប់ការអភិវឌ្ឍកម្មវិធីដែលមានប្រសិទ្ធភាពខ្ពស់ ជាពិសេសសម្រាប់ Mobile Application, Web, និង Desktop Application ជាដើម ។

ភាសា Dart មានលក្ខណៈសាមញ្ញ ងាយយល់ និងមានទម្រង់សរសេរដូចនឹងភាសា C, Java, និង JavaScript ។ ដូច្នេះអ្នកដែលធ្លាប់ស្គាល់ភាសាទាំងនោះ អាចរៀន Dart បានលឿន ។

**លក្ខណៈពិសេសនៃភាសា Dart៖**
- មានសមត្ថភាព compiled ទៅជា native code ដើម្បីដំណើរការលឿន។
- មាន JIT Compiler (Just-In-Time) : ក្នុងអំឡុងពេលអភិវឌ្ឍន៍ (development), Dart អនុញ្ញាតឱ្យអ្នក Hot Reload ដោយមិនចាំបាច់ run app ឡើងវិញទាំងអស់។
- អាចប្រើសម្រាប់ Flutter Framework ដើម្បីបង្កើត App ដើរលើ Android និង iOS ដោយ code តែមួយ។
- មាន Null safety ដែលជួយការពារការកើតបញ្ហាពេលប្រើតម្លៃ null។
- មាន Strong typing system ដែលធ្វើឱ្យកម្មវិធីមានសុវត្ថិភាពនិងងាយរកកំហុស។
- អាចដំណើរការទៅលើ Browser (ជាកូដ JavaScript បន្ទាប់ពីបម្លែង) ឬនៅលើ server side។

**៣. ការដំឡើងកម្មវិធី សរសេរកូដ IDE (Setting Up Your Development Environment)**

**៣.១. ការដំឡើង Dart SDK**
Dart SDK មាន Compiler, Tool, និង Library ដើម្បីអនុញ្ញាតឲ្យអ្នកដំណើរការកូដ Dart។
- ចូលទៅកាន់គេហទំព័រផ្លូវការ៖
  - https://dart.dev/get-dart
- ជ្រើសយកប្រព័ន្ធប្រតិបត្តិការ (Windows, macOS, ឬ Linux) របស់អ្នក។

- បន្ទាប់ពីតំឡើងរួច, បើក Terminal ឬ CMD ហើយវាយ៖
  - `dart --version`
- ប្រសិនបើចេញដូចខាងក្រោមមានន័យថា តំឡើងបានជោគជ័យ៖
  - `Dart SDK version: 3.9.2 (stable)`

**៣.២. ការតម្លើង IDE (Visual Studio Code)**

Visual Studio Code (VS Code) ជា editor ដែលពេញនិយម និងអាចប្រើសម្រាប់ Dart យ៉ាងងាយស្រួល។ បន្ទាប់ពីដំឡើង៖
- បើក VS Code
- ចូលទៅផ្នែក Extensions (Ctrl+Shift+X)
- ស្វែងរក និងតម្លើង៖
  - Dart
  - Flutter (បើអ្នកចង់អភិវឌ្ឍ Flutter)

**៣.៣. ការបង្កើត Dart Project**

បង្កើត Project ថ្មី
- ប្រើ Command ដើម្បីបង្កើត project ថ្មី៖
  - `dart create my_app`
- Dart នឹងបង្កើត Folder និង File ដូចជា៖
```text
my_app/
┣ bin/
┃ ┗ my_app.dart
┣ pubspec.yaml
┗ README.md
```

**៤. កម្មវិធីដំបូងរបស់ Dart**
- សរសេរ Program ក្នុង `my_app.dart`

```dart
void main() {
  print('Hello, World!');
  print('Welcome to Dart Programming');
}
```

- Run កម្មវិធី (In Terminal) : `dart run`
