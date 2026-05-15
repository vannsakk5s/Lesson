# ឯកសារជំនួយ Jinja (Jinja Cheat Sheet)

## កន្សោមមូលដ្ឋាន (Basic Expressions)

- `{{ "Hello World" }}`: បង្ហាញ Hello World
- `{{ foo }}`: បង្ហាញតម្លៃនៃអថេរ `foo`
- `{{ 2 + 3 }}`: បង្ហាញលេខ `5`
- `{{ 5 > 10 }}`: បង្ហាញ `False`

## លក្ខខណ្ឌ (If Statement)

```jinja
{% if hour > 5 and hour < 12 %}
Good Morning!
{% elif number >= 12 and hour < 17 %}
Good Afternoon!
{% elif number >= 17 %}
Good Evening!
{% endif %}
```

## រង្វិលជុំ (For Loop)

```jinja
{% for number in 10 %}
{{ number }}
{% endfor %}
```

### រង្វិលជុំជាមួយលក្ខខណ្ឌ (For loop with conditions)

```jinja
{% for item in states if item.attributes.emulated_hue_name|length > 0-%}
{%- if loop.first %}{% elif loop.last %}, and
{% else %}, {% endif -%}
{{item.attributes.emulated_hue_name|title}}
{%- endfor -%}.
```

*នៅពេលដំណើរការរង្វិលជុំ ពេលខ្លះយើងចង់បន្ថែមតក្កវិជ្ជាបន្ថែមដោយផ្អែកលើរង្វាស់រង្វិលជុំ (loop counter) ។ នេះជាឧទាហរណ៍មួយ។*

## ការគ្រប់គ្រងអត្ថបទ (String Manipulations)

- `{{ "Hello" ~ " World" }}`: បង្ហាញ "Hello World" (ការតភ្ជាប់អក្សរ)
- `{{ "Hello World" | lower }}`: បង្ហាញ "hello world"
- `{{ "hello world" | title }}`: បង្ហាញ "Hello World"
- `{{ "hello world" | upper }}`: បង្ហាញ "HELLO WORLD"
- `{{ "hello world".split(' ')[0] }}`: បំបែកដោយសញ្ញា ' ' និងបង្ហាញ "hello"
- `{{ "hello" | replace("he", "she") }}`: ជំនួស "he" ដោយ "she" និងបង្ហាញ "shello"
- `{{ "hello" | length }}`: បង្ហាញចំនួនតួអក្សរ ដែលស្មើនឹង 5
- `{{ "hello" == "hello" }}`: ប្រៀបធៀបអក្សរ; ប្រសិនបើស្មើគ្នា វាផ្តល់លទ្ធផល True
- `{{ "Hello world!"[:4] }}`: យកផ្នែកនៃអត្ថបទ។ ផ្តល់លទ្ធផល "Hell", តួអក្សរ 4 ដំបូង
- `{{ "HELLO"[2:4] }}`: យកផ្នែកនៃអត្ថបទ។ ផ្តល់លទ្ធផល "LL"
- `{{ "HELLO"[:-3] }}`: យកផ្នែកនៃអត្ថបទដោយលុប 3 តួអក្សរចុងក្រោយចេញ។ ផ្តល់លទ្ធផល "HE"

## កាលបរិច្ឆេទ និងពេលវេលា (Date and Time)

- `{{ now() }}`: បង្ហាញតម្លៃកាលបរិច្ឆេទ និងពេលវេលាបច្ចុប្បន្ន
- `{{ now().strftime("%m-%d-%Y") }}`: បង្ហាញ "07-26-2017" ដោយបំប្លែងតម្លៃកាលបរិច្ឆេទទៅជាទម្រង់ដែលបានបញ្ជាក់
- `{{ as_timestamp(now()) | timestamp_custom("%m-%d-%Y", true) }}`: បង្ហាញ "07-26-2017" ដោយបំប្លែងតម្លៃកាលបរិច្ឆេទទៅជាទម្រង់ដែលបានបញ្ជាក់
- `{{ now().strftime("%m") }}`: បង្ហាញតម្លៃខែ។ ឧទាហរណ៍៖ 07

*សម្រាប់ទម្រង់កាលបរិច្ឆេទ និងពេលវេលាបន្ថែម សូមចូលទៅកាន់ strftime.org ។*

## ការបំប្លែងលេខ (Number Conversions)

- `{{ 10.5 }}`: បង្ហាញលេខដូចដើម
- `{{ 10.500482737 | int }}`: បំប្លែងចំនួនទសភាគ (float) ទៅជាចំនួនគត់ (integer)
- `{{ 10.6 | round }}`: បង្គត់ទៅចំនួនទសភាគដែលជិតបំផុត
- `{{ 10.4 | round }}`: បង្គត់ទៅចំនួនទសភាគដែលជិតបំផុត
- `{{ 10 | float }}`: បំប្លែងចំនួនគត់ទៅជាចំនួនទសភាគ (float)
- `{{ '%0x' % 255 }}`: បំប្លែងលេខគោលដប់ទៅជាតម្លៃ hex (គោលដប់ប្រាំមួយ)
- `{{ "%0x" | format(some_numeric_value | int) }}`: បំប្លែងតម្លៃលេខទៅជាតម្លៃ hex
- `{{ '%0.2f' % 24.12345 }}`: បង្ហាញតែពីរខ្ទង់ប៉ុណ្ណោះបន្ទាប់ពីក្បៀស
- `{{ "${:.2f}".format(15.45) }}`: រៀបចំទម្រង់ជារូបិយប័ណ្ណ

## ម៉ាក្រូមូលដ្ឋាន (Basic Macros)

```jinja
{% macro sayHello() -%}
Hello!
{%- endmacro %}
```

ហៅម៉ាក្រូមកប្រើតាមរយៈ៖

`{{ sayHello() }}`

## ចៃដន្យ (Random)

- `{{ ["hello", "hi there!", "howdy!", "hey there!"] | random }}`

## ការដោះស្រាយបញ្ហា (Troubleshooting)

ត្រូវបំប្លែងទៅជាប្រភេទផ្ទុកទិន្នន័យ (data type) ត្រឹមត្រូវជានិច្ច មុនពេលប្រើប្រាស់ filter ណាមួយ។

ឧទាហរណ៍៖

```jinja
{{ set value = "123" }}
{{ value > 100 }}
```

ដោយសារតម្លៃនេះជាប្រភេទអក្សរ (string) សូមប្រាកដថាអ្នកបំប្លែងវាទៅជាចំនួនគត់ (integer) ជាមុនសិន៖

```jinja
{{ (value | int) > 123 }}
```
