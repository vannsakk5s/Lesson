### មេរៀនទី៣ Control Flow Statements

**១. គោលបំណងនៃមេរៀន**

Control Statements ត្រូវបានគេប្រើសម្រាប់ត្រួតពិនិត្យទៅលើដំណើរការរបស់ Program នៅពេលដែល Program កំពុងដំណើរការ។ ជាទូទៅការត្រួតពិនិត្យលក្ខខណ្ឌនៅក្នុង Program មួយគឺយើងចង់ដឹងពីសកម្មភាពដែលវាបានកើតឡើង។ នៅក្នុងការត្រួតពិនិត្យលក្ខខណ្ឌ មានលទ្ធផល តែពីរប្រភេទគត់ គឺ លក្ខខណ្ឌពិត (TRUE) និងលក្ខខណ្ឌមិនពិត (FALSE) ។

នៅក្នុងមេរៀននេះ នឹងបង្ហាញអោយអ្នកមានសមត្ថភាពដូចខាងក្រោម៖
- កំណត់ការប្រើប្រាស់លក្ខខណ្ឌ if else Statements
- កំណត់ការប្រើប្រាស់ Switch
- កំណត់ការប្រើប្រាស់ Continue, Break
- កំណត់ការប្រើប្រាស់ Loop (for, for-in, while, do while)

**២. If-Else Statement**

**២.១. if Condition**

if នឹងត្រូវត្រួតពិនិត្យលក្ខខណ្ឌរបស់ expression បើ expression ពិតនោះវានឹងអនុវត្ត Statement(s) របស់វា តែបើមិនពិតទេនោះ វានឹងមិនអនុវត្ត Statement(s) នោះទេ ។

ទម្រង់ទូទៅរបស់ if Statements:

```dart
if ( expression )
   Statement;
```

ឬក៏

```dart
if ( expression )
{
   Statement 1;
   ------------
   Statement N;
}
```

**២.២. if ... else Condition**

if នឹងត្រូវត្រួតពិនិត្យលក្ខខណ្ឌរបស់ expression បើ expression ពិតនោះវានឹងអនុវត្ត Statement(s) របស់វា តែបើមិនពិតទេនោះ វានឹងអនុវត្ត Statement(s) របស់ else ។

ទម្រង់ទូទៅរបស់ if ... else statements:

```dart
if ( expression )
   Statement;
else
   Statement;
```

ឬក៏

```dart
if ( expression )
{
   Statement 1;
   ------------
   Statement N;
}
else
{
   Statement 1;
   ------------
   Statement N;
}
```

**ចំណាំ៖** បើសិនជា expression ពិតនោះវានឹងដំណើរការនូវ Statement(s) ដែលស្ថិតនៅក្រោមការគ្រប់គ្រងរបស់ if រួចហើយវានឹងរំលងទៅ Outer Statement(s)។ ផ្ទុយទៅវិញបើសិនជា expression មិនពិតនោះវានឹងដំណើរការនូវ Statement(s) ដែលស្ថិតនៅក្រោមការគ្រប់គ្រងរបស់ else រួចហើយវានឹងរំលងទៅ Outer Statement(s) ។

**២.៣. if ... else if Condition**

if នឹងត្រូវត្រួតពិនិត្យលក្ខខណ្ឌរបស់ expression បើ expression ពិតនោះវានឹងអនុវត្ត Statement(s) របស់វា រួចចាកចេញពី if Statement តែម្ដង។ តែបើមិនពិតទេនោះ វានឹងត្រូវពិនិត្យលក្ខខណ្ឌរបស់ expression បន្ទាប់ បើលក្ខខណ្ឌពិតនោះវានឹងអនុវត្ត Statement(s) របស់វា រួចចាកចេញពី if Statement តែម្ដង។ តែបើនៅតែមិនពិតទៀតនោះវានឹងត្រួតពិនិត្យលក្ខខណ្ឌ របស់ expression បន្តបន្ទាប់រហូតដល់ចប់ បើពុំមានលក្ខខណ្ឌរបស់ expression ណាមួយពិតទេ នោះ វានឹងអនុវត្ត Statement(s) របស់ else ។

ទម្រង់ទូទៅរបស់ if ... else if Statements:

```dart
if ( expression )
   Statement;
else if ( expression )
   Statement;
```

**៣. Switch Statement**

switch គឺត្រួតពិនិត្យគ្រប់តម្លៃថេរ នៃ case ទាំងអស់ ប្រសិនបើមានលក្ខខណ្ឌ case ណាមួយពិតនោះវានឹងអនុវត្តនូវ Statement(s) របស់ case នោះភ្លាម។

ខាងក្រោមគឺទម្រង់ទូទៅរបស់ switch Statements:

```dart
switch ( variable ){
   case constant 1: Statement(s); break;
   case constant 2: Statement(s); break;
   -------------------------------------
   default: Statement(s); break;
}
```

**ចំណាំ:**
- switch ខុសពី if ត្រង់ switch អាចត្រួតពិនិត្យបានតែចំពោះលក្ខខណ្ឌស្មើ រីឯ if វិញគឺអាចត្រួតពិនិត្យចំពោះ relational ឬក៏ logical expression ។
- variable ត្រូវតែមានប្រភេទជាចំនួនគត់ ឬក៏ជាតួអក្សរ (character) ។
- មិនអាចមានតម្លៃរបស់ case ពីរស្មើគ្នាក្នុង switch តែមួយ ។
- break; វាត្រូវបានប្រើដើម្បីចាកចេញពី case នីមួយៗ។ បើមិនប្រើ break; ទេនោះបន្ទាប់ពីបាន អនុវត្ត Statement(s) របស់ case ណាមួយរួចហើយ វានឹងបន្តអនុវត្ត Statement(s) របស់ case បន្តបន្ទាប់ទៀត។
- default: ត្រូវបានអនុវត្តនៅពេលដែល ពុំមានលក្ខខណ្ឌរបស់ case ណាមួយពិត។

**៤. Loop Statements**

នៅក្នុងការសរសេរកម្មវិធីដើម្បីដោះស្រាយបញ្ហាអ្វីមួយ ជួនកាលគេជួបប្រទះលក្ខខណ្ឌ មួយតែមួយដង ជួនកាលគេជួបប្រទះលក្ខខណ្ឌដដែលច្រើនដង ។ ហើយលក្ខខណ្ឌដដែលៗ ច្រើនដងនេះត្រូវបានគេហៅថា Loops ។ Loops គឺជាការធ្វើសកម្មភាពដដែលរហូតជួបលក្ខខណ្ឌ ណាមួយទើបវាបញ្ចប់សកម្មភាពរបស់វា ។

នៅក្នុង Dart Programming មាន Loops ៤ គឺ: for, for-in, while, do-while

**៤.២. for loop**

ទម្រង់ទូទៅរបស់ for loop:

```dart
for ( initialization; expression ; increment ){
   Statement 1;
   ------------
   Statement N;
}
```

**ឧទាហរណ៍**

```dart
void main() {
   for( int i = 1; i <= 5; i++ ){
      print('Value : ${i}');
   }
}
```

**៤.៣. for-in loop**

ទម្រង់ទូទៅរបស់ for-in loop:

```dart
for( var in expression )
{
   Statement 1;
   ------------
   Statement N;
}
```

**ឧទាហរណ៍**

```dart
void main() {
   var data = [1, 2, 3, 4, 5];
   for( var item in data )
   {
      print('Value : ${item}');
   }
}
```

**៤.៤. while loop**

ទម្រង់ទូទៅរបស់ while loop:

```dart
while( expression )
{
   Statement1;
   ------------
   StatementN;
}
```

**ឧទាហរណ៍**

```dart
void main() {
   int value = 1;
   while( value <= 5 )
   {
      print( 'Value : ${value}' );
      value++;
   }
}
```

**៤.៥. do-while loop**

ទម្រង់ទូទៅរបស់ do...while loop:

```dart
do{
   Statement1;
   ------------
   StatementN;
} while ( expression );
```

**ឧទាហរណ៍**

```dart
void main() {
   int value = 1;
   do{
      print( 'Value : ${value}' );
      value++;
   } while( value <= 5 );
}
```