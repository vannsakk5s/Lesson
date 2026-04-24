### មេរៀនទី២ មូលដ្ឋានគ្រឹះនៃ Dart (Variables, Data Types, Operators)

**១. គោលបំណងនៃមេរៀន**

នៅក្នុងមេរៀននេះ នឹងឲ្យអ្នកយល់ពី Variables, Data Types, Operators

នៅក្នុងមេរៀននេះ នឹងបង្ហាញឲ្យអ្នកមានសមត្ថភាពដូចខាងក្រោម៖
- កំណត់ពីអ្វីទៅដែលហៅថា អថេរ ឬ អញ្ញាត (Variables)
- ស្គាល់ពី ប្រភេទទិន្នន័យ (Data Types)
- ការប្រើប្រាស់ Operators

**២. អថេរ (Variable)**

Variable គឺជាតំណាងមួយដែលត្រូវបានបម្រុងទុកនូវទិន្នន័យបណ្ដោះអាសន្នក្នុងតំបន់ Memory។ យើងអាចផ្ទុកទិន្នន័យទាំងនោះនៅក្នុង អថេរ ឬ អញ្ញាត ដើម្បីយកទិន្នន័យទាំងនោះទៅធ្វើការគណនា ឬធ្វើការបង្ហាញផ្សេងៗ។

ខាងក្រោមនេះជាទម្រង់នៃការប្រកាសអថេរ Variable ៖

ទី ១ ៖ ទម្រង់ទូទៅ

```dart
Data_type variableName;
```

ទី ២ ៖ ដើម្បីផ្តល់តម្លៃដំបូងទៅឲ្យ variable

```dart
Data_type variableName = value;
```

ទី ៣ ៖ នៅពេលមាន variable ជាច្រើនមានប្រភេទទិន្នន័យដូចគ្នា

```dart
Data_type variableName1, variableName2,........ variableNameN;
```

- `Data_type` : ប្រភេទនៃទិន្នន័យដូចជា int, bool, string .....
- `variableName` : ឈ្មោះរបស់ variable
- `value` : តំលៃដែលត្រូវផ្ដល់ទៅឲ្យ variable អាចជា លេខ ឬ អក្សរ

**ឧទាហរណ៍**
- `int a;`
- `int a=5;`
- `int a, b, c;`

**ច្បាប់ក្នុងការដាក់ឈ្មោះ Variable ៖**
- អាចប្រើប្រាស់ អក្សរ លេខ និង និមិត្តសញ្ញា Underscore ( _ )
- ហាមផ្ដើមដោយលេខ ឧ៖ `int 2Num;`
- ហាមដកឃ្លា ឧ៖ `int User Name;`
- ហាមប្រើជាន់ keyword ឧ៖ `int if;`, `double for;`
- ហាមប្រើជាមួយនឹងសញ្ញាពិសេសផ្សេងៗ ដូចជា៖ `# ? ! & | % @.......`

Keyword គឺជាពាក្យបញ្ជាប្រាប់ទៅ System ឲ្យធ្វើអ្វីមួយ។ វាជាពាក្យដែលមានស្រាប់ និងត្រូវបានគេប្រើប្រាស់ក្នុងភាសា Dart។ ខាងក្រោមជា keyword មួយចំនួនដែលត្រូវបានគេប្រើប្រាស់ក្នុងភាសា Dart ៖

`int`, `double`, `num`, `bool`, `string`, `if`, `else`, `switch`, `while`, `break`, `continue`, `enum`, `return`, .........

**៣. ប្រភេទទិន្នន័យ (Data Types)**

**៣.១. Boolean**

Boolean Type ត្រូវបានគេប្រកាសដោយប្រើ keyword : `bool` ត្រូវបានកំណត់ដើម្បីផ្ទុកតម្លៃតែពីរប្រភេទប៉ុណ្ណោះគឺ ពិត (True) រឺ មិនពិត (False)។

**ឧទាហរណ៍**

```dart
void main() {
  bool status = false;
  bool isActive = true;
  
  if (isActive) {
    print('Hello, World!');
  }
}
```

**៣.២. Integer (int)**

Integer Type គឺជាប្រភេទទិន្នន័យមួយបែប ដែលប្រើប្រាស់សម្រាប់ការរក្សាទុកទិន្នន័យជាចំនួនគត់។

**ឧទាហរណ៍**

```dart
void main() {
  int age = 20;
  int temperature = -10;
}
```

**៣.៣. Double**

Double Type គឺជាប្រភេទទិន្នន័យមួយបែប ដែលប្រើប្រាស់សម្រាប់ការរក្សាទុកទិន្នន័យជា ចំនួនទស្សភាគ ។

**ឧទាហរណ៍**

```dart
void main() {
  double pi = 3.14;
  double height = 1.75;
}
```

**៣.៤. num**

num Type គឺជាប្រភេទទិន្នន័យមួយបែប ដែលប្រើប្រាស់សម្រាប់ការរក្សាទុកទិន្នន័យជា លេខទូទៅ (ចំនួនគត់ ឬ ចំនួនទស្សភាគ) ។

**ឧទាហរណ៍**

```dart
void main() {
  num value = 10;
  value = 12.5;
}
```

**៣.៥. String**

String គឺជាប្រភេទទិន្នន័យមួយបែប ដែលប្រើប្រាស់សម្រាប់ការរក្សាទុកទិន្នន័យជាតំលៃអក្សរ។

**ឧទាហរណ៍**

```dart
void main() {
  String value = 'Dart Programming';
  print('Welcome to ${value}');
}
```

**៤. ប្រភេទអថេរ (var, final, const, late, dynamic)**

- `var` : អនុញ្ញាតឲ្យ Dart កំណត់ប្រភេទទិន្នន័យ ដោយស្វ័យប្រវត្តិ

```dart
var age = 20;
age = 18;
age = 'Twenty'; (Error : មិនអាចប្រែទៅ String)
```

- `final` : អាចផ្ដល់តម្លៃបានតែម្ដង (មិនអាចផ្លាស់ប្ដូរ)

```dart
final createAt = DateTime.now();
createAt = DateTime(2025); (មិនអាចផ្លាស់ប្ដូរបាន)
```

- `const` : តម្លៃថេរដែលកំណត់នៅពេល compile-time

```dart
const pi = 3.14;
const now = DateTime.now(); (មិនបាន មិនមែន Compile-time)
```

- `late` : ផ្ដល់តម្លៃក្រោយពេលប្រកាស (lazy initialization)

```dart
late String token;
token = getToken();
print(token);
```

- `dynamic` : អាចផ្លាស់ប្ដូរប្រភេទទិន្នន័យនៅពេលក្រោយបាន

```dart
dynamic value = 10;
value = 'Ten';
print(value);
```

**៥. ប្រមាណវិធី (Operators)**

Operators គឺជានិមិត្តសញ្ញា(symbol) ដែលប្រាប់ទៅ system ឲ្យធ្វើការគណនាអ្វីមួយ ឬ រៀបចំចាត់ចែង ធ្វើចំណាត់ថ្នាក់ លើប្រភេទទិន្នន័យ។ Operators ត្រូវបានបែងចែកដូចខាងក្រោម៖

**៥.១. Arithmetic Operator**

គឺជាសញ្ញា ឬ ប្រមាណវិធី ដែលគេប្រើប្រាស់សម្រាប់ការគណនា ។

- `+` : បូក Addition
- `-` : ដក Subtraction
- `*` : គុណ Multiply
- `/` : ចែក Division
- `%` : ចែករកសំណល់ (Modulus)

**៥.២. Comparison Operator**

- `==` : ស្មើ Equal to
- `!=` : មិនស្មើ Not equal to
- `<` : តូចជាង Less than
- `<=` : តូចជាង ឬស្មើ Less than or equal to
- `>` : ធំជាង Greater than
- `>=` : ធំជាង ឬ ស្មើ Greater than or equal to

**៥.៣. Logical Operator**

- `&&` : ឈ្នាប់នឹង Condition AND
- `||` : ឈ្នាប់ឬ Condition OR
- `!` : មិន Condition NOT

**៥.៤. Ternary Operator**

Ternary operator ជួយសរសេរលក្ខខណ្ឌឲ្យខ្លី។

**syntax:** `condition ? valueIfTrue : valueIfFalse`

**ឧទាហរណ៍**

```dart
void main() {
  // Arithmetic Operator
  int a = 10, b = 3;
  print(a + b);

  // Comparison Operator
  bool status1 = (a <= b);
  print(status1);

  // Logical Operator
  bool status3 = (a > 0 && a > b);
  print(status3);
}
```