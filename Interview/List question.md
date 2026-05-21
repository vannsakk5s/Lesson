1. សំណួរទូទៅ (General Questions)
- សូមណែនាំខ្លួនអ្នក៖ (ឧទាហរណ៍) "ខ្ញុំបាទ/ចាសឈ្មោះ [ឈ្មោះរបស់អ្នក] ជា Backend Developer ដែលមានបទពិសោធន៍ [ចំនួនឆ្នាំ] ឆ្នាំ ក្នុងការបង្កើត API, design database និងការគ្រប់គ្រង logic នៅពីក្រោយ application ធំៗ។"

- ហេតុអ្វីបានជាអ្នកចង់ធ្វើការ position Backend Developer? ព្រោះខ្ញុំចូលចិត្តការដោះស្រាយបញ្ហា (Problem-solving) ការគិតពី Logic ស្មុគស្មាញ ការរៀបចំទិន្នន័យ (Data structure) និងការធ្វើឱ្យ System ដំណើរការបានលឿន សុវត្ថិភាព និងមានស្ថេរភាព។

- តើអ្នកធ្លាប់ធ្វើ project អ្វីខ្លះ? (រៀបរាប់ពី Project ពិតរបស់អ្នក ដូចជា៖ E-commerce web, POS system, ឬ School management mobile app API ល)។

- ក្នុង project មុន តើអ្នកទទួលខុសត្រូវលើផ្នែកអ្វី? ខ្ញុំទទួលខុសត្រូវលើការ Design database architecture, សរសេរ RESTful APIs, បញ្ចូលការទូទាត់ប្រាក់ (Payment integration), និងការ Deploy system ទៅលើ Cloud server។

- តើអ្នកធ្លាប់ធ្វើការជាមួយ Frontend Developer ដែរឬទេ? បាទ/ចាស ធ្លាប់។ ខ្ញុំធ្វើការជិតស្និទ្ធជាមួយពួកគាត់ដោយការផ្តល់ជូន API documentation ច្បាស់លាស់ (ដូចជា Swagger/Postman) និងការពិភាក្សាគ្នាពី Format នៃ Request/Response ទិន្នន័យ។

- តើអ្នកដោះស្រាយ bug យ៉ាងដូចម្តេច? ខ្ញុំពិនិត្យមើល Error log (Console/Log files) -> ប្រើប្រាស់ Debugger ដើម្បីត្រួតពិនិត្យតម្លៃ Variable តាមជំហាននីមួយៗ -> ស្វែងរក root cause -> បង្កើតដំណោះស្រាយ -> រួចធ្វើ Test ម្តងទៀតដើម្បីប្រាកដថាវាមិនប៉ះពាល់ផ្នែកផ្សេង។

- តើអ្នករៀន technology ថ្មីៗដោយរបៀបណា? តាមរយៈការអាន Official Documentation, មើលវីដេអូបង្រៀនលើ YouTube/Udemy, តាមដាន Tech blogs (Medium, Dev.to) និងការសាកល្បងធ្វើ Project តូចៗផ្ទាល់ដៃ (Hands-on projects)។

2. Programming Language & Core Concepts
- តើអ្នកខ្លាំងលើ language អ្វី? (ឆ្លើយតាមភាសាដែលអ្នកចេះច្បាស់ ឧទាហរណ៍៖ "ខ្ញុំខ្លាំងលើ JavaScript/TypeScript ជាមួយ Node.js និង PHP ជាមួយ Laravel")។

- តើ OOP គឺជាអ្វី? OOP (Object-Oriented Programming) គឺជាវិធីសាស្ត្រសរសេរកូដដែលផ្ដោតលើការបង្កើត "Objects" ដែលផ្ទុកទិន្នន័យ (Properties) និងសកម្មភាព (Methods) ដើម្បីតំណាងឱ្យអ្វីមួយក្នុងពិភពពិត។

- សូមពន្យល់ class និង object៖

- Class: គឺជាប្លង់មេ ឬគំរូ (Blueprint/Template) សម្រាប់បង្កើត Object។

- Object: គឺជាអ្វីដែលកើតចេញពី Class នោះ (An instance of a class) មានទិន្នន័យពិតប្រាកដ។

- តើ inheritance គឺជាអ្វី? គឺជារបៀបដែល Class កូន (Child class) អាចទាញយក ឬប្រើប្រាស់ Properties និង Methods ពី Class មេ (Parent class) ដោយមិនបាច់សរសេរឡើងវិញ។

- តើ interface ប្រើសម្រាប់អ្វី? ប្រើសម្រាប់កំណត់ "កិច្ចសន្យា (Contract)" ឬរចនាសម្ព័ន្ធនៃ Methods ដែល Class ផ្សេងៗត្រូវតែយកទៅអនុវត្តតាម (Implement) ប៉ុន្តែវាមិនមាន Logic នៅក្នុងនោះទេ។

- តើ error handling គឺជាអ្វី? គឺជាដំណើរការចាប់ (Catch) និងដោះស្រាយរាល់បញ្ហា ឬ Error ដែលកើតឡើងពេលកូដកំពុង Run ដើម្បីកុំឱ្យ System គាំង (Crash) និងដើម្បីបង្ហាញសារប្រាប់ User ឱ្យបានសមរម្យ។

- តើ synchronous និង asynchronous ខុសគ្នាយ៉ាងដូចម្តេច?

- Synchronous: កូដដំណើរការម្តងមួយជួរតាមលំដាប់ (ជួរទី១ ចប់ ទើបជួរទី២ ដើរ)។ បើមានជួរណាយូរ វាជិះជាន់ (Block) ជួរក្រោយ។

- Asynchronous: កូដអាចដំណើរការការងារដែលចំណាយពេលយូរ (ដូចជាទាញទិន្នន័យពី DB) នៅ Background បាន ដោយមិនរង់ចាំ ឬ Block កូដជួរផ្សេងទៀតឡើយ។

- តើ async/await ប្រើសម្រាប់អ្វី? ប្រើសម្រាប់សរសេរកូដ Asynchronous ឱ្យមើលទៅស្អាត និងងាយស្រួលអាន/យល់ ដូចកូដ Synchronous ដែរ (ជៀសវាងបញ្ហា Callback Hell)។

- តើ variable, function, loop, condition ប្រើសម្រាប់អ្វី?

- Variable: សម្រាប់រក្សាទុកទិន្នន័យបណ្តោះអាសន្ន។

- Function: សម្រាប់ប្រមូលផ្តុំកូដមួយដុំយកទៅប្រើប្រាស់ឡើងវិញបានច្រើនដង។

- Loop: សម្រាប់ដំណើរការកូដដដែលៗតាមលក្ខខណ្ឌកំណត់។

- Condition: សម្រាប់កំណត់ឱ្យកូដដើរ ឬមិនដើរ ផ្អែកលើលក្ខខណ្ឌ ពិត (True) ឬមិនពិត (False)។

3. API និង HTTP
- តើ REST API គឺជាអ្វី? គឺជាស្តង់ដារ ឬស្ថាបត្យកម្ម (Architectural style) នៃការបង្កើត API ដែលប្រើប្រាស់ HTTP Methods ផ្សេងៗដើម្បីធ្វើការជាមួយទិន្នន័យ (Resources)។

- តើ API ប្រើសម្រាប់អ្វី? ប្រើដើម្បីឱ្យ System ពីរ ឬច្រើន (ដូចជា Frontend និង Backend) អាចប្រាស្រ័យទាក់ទង និងផ្លាស់ប្តូរទិន្នន័យគ្នាទៅវិញទៅមកបាន។

- តើ GET, POST, PUT, PATCH, DELETE ខុសគ្នាយ៉ាងដូចម្តេច?

- GET: សម្រាប់ទាញយកទិន្នន័យ (Read)។

- POST: សម្រាប់បង្កើតទិន្នន័យថ្មី (Create)។

- PUT: សម្រាប់កែប្រែទិន្នន័យទាំងមូល (Replace/Update entire resource)។

- PATCH: សម្រាប់កែប្រែទិន្នន័យតែផ្នែកណាមួយ (Partial update)។

- DELETE: សម្រាប់លុបទិន្នន័យ (Delete)។

- តើ HTTP status code ទាំងនេះមានន័យអ្វី?

- 200 OK: ការស្នើសុំជោគជ័យ។

- 201 Created: បង្កើតទិន្នន័យថ្មីជោគជ័យ។

- 400 Bad Request: Request ពី Client មិនត្រឹមត្រូវ (ខុស Format/ខ្វះទិន្នន័យ)។

- 401 Unauthorized: មិនទាន់បាន Login ឬគ្មាន Token។

- 403 Forbidden: បាន Login ហើយ តែគ្មានសិទ្ធិ (Permission) ចូលមើល។

- 404 Not Found: រកមិនឃើញ Resource ឬ URL នោះទេ។

- 500 Internal Server Error: មាន Error កើតឡើងនៅខាង Server (កូដ Backend មានបញ្ហា)។

- តើ request និង response គឺជាអ្វី?

- Request: គឺជាទិន្នន័យ ឬការស្នើសុំដែល Client (Frontend) ផ្ញើទៅកាន់ Server។

- Response: គឺជាលទ្ធផល ឬទិន្នន័យដែល Server (Backend) ផ្ញើត្រឡប់មក Client វិញ។

- តើ header, body, query params, path params ខុសគ្នាយ៉ាងដូចម្តេច?

- Header: ផ្ទុក Metadata (ព័ត៌មានបន្ថែម) ដូចជា Content-Type ឬ Auth Token។

- Body: ផ្ទុកទិន្នន័យចម្បងដែលត្រូវផ្ញើទៅ (ច្រើនប្រើក្នុង POST, PUT, PATCH) ជាទម្រង់ JSON។

- Query Params: ទិន្នន័យនៅលើ URL ក្រោយសញ្ញា ? (ឧទាហរណ៍៖ /products?search=apple&page=2) ប្រើសម្រាប់ filter/search។

- Path Params: ជាផ្នែកមួយនៃ URL ហ្មង (ឧទាហរណ៍៖ /products/12) លេខ 12 គឺតំណាងឱ្យ ID។

- តើ middleware គឺជាអ្វី? គឺជាកូដ ឬ Function ដែលរត់នៅចន្លោះកណ្តាល ពេលមាន Request ចូលមក មុននឹងទៅដល់ Controller ចម្បង (ច្រើនប្រើសម្រាប់ Check Auth, Log data, ឬ Validate input)។

- តើ rate limiting គឺជាអ្វី? គឺជាបច្ចេកទេសកំណត់ចំនួនដង (Limit) ដែល Client ម្នាក់អាច call API ក្នុងរយៈពេលណាមួយ (ឧទាហរណ៍៖ 100 requests ក្នុង 1 នាទី) ដើម្បីការពារ Server កុំឱ្យហៀរ (Overload) ឬត្រូវគេវាយប្រហារ (DDoS)។

- តើ pagination គឺជាអ្វី? គឺជាការបែងចែកទិន្នន័យជាទំព័រៗ (ឧទាហរណ៍៖ បង្ហាញម្តង ១០ ជួរ) ដើម្បីកុំឱ្យ API ផ្ញើទិន្នន័យទៅច្រើនពេកក្នុងពេលតែមួយ ដែលធ្វើឱ្យដើរយឺត។

- តើអ្នក design API endpoint សម្រាប់ login យ៉ាងដូចម្តេច?

- Method: POST

- URL: /api/v1/auth/login

- Body: { "email": "...", "password": "..." }

4. Database
- តើអ្នកធ្លាប់ប្រើ database អ្វីខ្លះ? (ឆ្លើយតាមបទពិសោធន៍ ឧទាហរណ៍៖ "MySQL សម្រាប់ Relational DB និង MongoDB សម្រាប់ NoSQL DB")។

- តើ SQL និង NoSQL ខុសគ្នាយ៉ាងដូចម្តេច?

- SQL (Relational): រក្សាទុកទិន្នន័យជា Table (ជួរដេក ជួរឈរ) មានរចនាសម្ព័ន្ធច្បាស់លាស់ (Strict schema) និងមាន Relationship ផ្ដោតលើ ACID ច្បាស់លាស់។

- NoSQL (Non-relational): រក្សាទុកជា Document (ដូច JSON), Key-Value, ឬ Graph មិនសូវមាន schema ណែនាំទេ និងងាយស្រួលពង្រីក (Scale horizontally)។

- តើ table, row, column គឺជាអ្វី?

- Table: ធុងផ្ទុកទិន្នន័យនៃប្រធានបទណាមួយ (ឧទាហរណ៍៖ users)។

- Row (Record): ទិន្នន័យមួយជួរពេញរបស់ Object មួយ។

- Column (Field): ជួរឈរដែលកំណត់ប្រភេទព័ត៌មាន (ឧទាហរណ៍៖ name, email)។

- តើ primary key គឺជាអ្វី? គឺជា Column ពិសេសមួយដែលកំណត់អត្តសញ្ញាណទិន្នន័យនីមួយៗក្នុង Table មិនឱ្យជាន់គ្នា (Unique) និងមិនអាចស្មើសូន្យ (Not Null) ដូចជា id។

- តើ foreign key គឺជាអ្វី? គឺជា Column ក្នុង Table មួយ ដែលយក Primary Key ពី Table មួយទៀតមកប្រើ ដើម្បីបង្កើតទំនាក់ទំនង (Relationship) រវាង Table ទាំងពីរ។

- តើ relationship ទាំងនេះមានន័យអ្វី?

- One-to-One: ទិន្នន័យ ១ ក្នុង Table A ទាក់ទងនឹងទិន្នន័យ ១ គត់ក្នុង Table B (ឧទាហរណ៍៖ User ១ មាន Profile ១)។

- One-to-Many: ទិន្នន័យ ១ ក្នុង Table A ទាក់ទងនឹងទិន្នន័យច្រើនក្នុង Table B (ឧទាហរណ៍៖ User ១ អាចមាន Orders ច្រើន)។

- Many-to-Many: ទិន្នន័យច្រើនក្នុង Table A ទាក់ទងនឹងទិន្នន័យច្រើនក្នុង Table B (ឧទាហរណ៍៖ Student ច្រើនអាចរៀន Course ច្រើន) -> ត្រូវបង្កើត Table កណ្តាល (Pivot table) មួយដើម្បីភ្ជាប់។

- តើ index គឺជាអ្វី? ហេតុអ្វីត្រូវប្រើ? Index គឺដូចជាលិបិក្រម (មាតិកា) នៅចុងសៀវភៅ។ យើងប្រើវាដើម្បីជួយឱ្យ Database ស្វែងរកទិន្នន័យ (Query) បានលឿនជាងមុន ដោយមិនបាច់ស្កេនមើលទិន្នន័យទាំងមូល (Full table scan)។

- តើ normalization គឺជាអ្វី? គឺជាដំណើរការរៀបចំរចនាសម្ព័ន្ធ Database ដើម្បីកាត់បន្ថយការជាន់គ្នានៃទិន្នន័យ (Data redundancy) និងការពារកុំឱ្យមានភាពមិនស៊ីសង្វាក់គ្នា (Data anomaly)។

- តើ transaction គឺជាអ្វី? គឺជាបណ្តុំនៃ SQL Operations ច្រើនដែលត្រូវរត់ជាមួយគ្នា។ វាត្រូវតែ "ជោគជ័យទាំងអស់ ឬបរាជ័យទាំងអស់ (All or Nothing)" បើមានមួយណា Error វានឹងហៅត្រឡប់មកដើមវិញ (Rollback) ធានាសុវត្ថិភាពទិន្នន័យ (ដូចជាការផ្ទេរប្រាក់)។

- តើ join ប្រើសម្រាប់អ្វី? ប្រើសម្រាប់ទាញយក និងផ្គុំទិន្នន័យចេញពី Table ពីរ ឬច្រើនបញ្ចូលគ្នា ផ្អែកលើ Column ដែលទាក់ទងគ្នា (Foreign key)។

- តើអ្នក optimize slow query យ៉ាងដូចម្តេច?

- ប្រើ EXPLAIN ដើម្បីមើលដំណើរការរបស់ Query។

- បង្កើត Index លើ Column ណាដែលប្រើក្នុង WHERE, JOIN ឬ ORDER BY ញឹកញាប់។

- ចៀសវាងការប្រើ SELECT * (យកតែ Column ណាដែលត្រូវការ)។

- ប្រើប្រាស់ Caching (ដូចជា Redis) សម្រាប់ទិន្នន័យណាដែលដដែលៗ។

5. Authentication និង Security
- តើ authentication និង authorization ខុសគ្នាយ៉ាងដូចម្តេច?

- Authentication (AuthN): ការបញ្ជាក់ថា "តើអ្នកជាអ្នកណា?" (ឧទាហរណ៍៖ Login ដោយប្រើ Email/Password)។

- Authorization (AuthZ): ការបញ្ជាក់ថា "តើអ្នកមានសិទ្ធិធ្វើអ្វីខ្លះ?" (ឧទាហរណ៍៖ Admin អាចលុបបាន តែ User ធម្មតាបានតែមើល)។

- តើ JWT គឺជាអ្វី? JSON Web Token គឺជា String មួយដែលត្រូវបាន Encrypt ប្រើសម្រាប់បញ្ជូនព័ត៌មានរវាង Client និង Server ដោយសុវត្ថិភាព។ ច្រើនប្រើសម្រាប់រក្សាស្ថានភាព Login (Stateless Authentication)។

- តើ access token និង refresh token ខុសគ្នាយ៉ាងដូចម្តេច?

- Access Token: ប្រើសម្រាប់សុំសិទ្ធិ call API ផ្សេងៗ (មានអាយុកាលខ្លី ដូចជា ១៥ នាទី ដើម្បីសុវត្ថិភាព)។

- Refresh Token: ប្រើសម្រាប់សុំ Access Token ថ្មីពេលវាហួសកំណត់ (Expired) ដោយ User មិនបាច់វាយ Password ចូលម្តងទៀតទេ (មានអាយុកាលវែង ដូចជា ៧ ថ្ងៃ)។

- តើ password គួររក្សាទុកក្នុង database ដោយរបៀបណា? មិនត្រូវរក្សាទុកជាអក្សរធម្មតា (Plain text) ដាច់ខាត។ ត្រូវតែធ្វើការ Hash វាជាមួយ Algorithm ខ្លាំងៗ (ដូចជា bcrypt) រួមជាមួយការថែម Salt (String 随机 ផ្ដេសផ្ដាស)។

- តើ hashing គឺជាអ្វី? គឺជាការបំប្លែង String មួយ ឱ្យទៅជា String ប្រវែងថេរមួយផ្សេងទៀតដែលមិនអាចបកប្រែត្រឡប់មកវិញបានឡើយ (One-way encryption)។

- តើ bcrypt គឺជាអ្វី? គឺជា Algorithm សម្រាប់ធ្វើ Password Hashing ដែលមានលក្ខណៈយឺត និងប្រើប្រាស់ CPU ច្រើន (Adaptive hashing) ធ្វើឱ្យ Hacker ពិបាកក្នុងខែកាច់បំបែក (Brute-force attack)។

- តើ SQL injection គឺជាអ្វី? តើយើងការពារយ៉ាងដូចម្តេច? គឺជាការវាយប្រហារដែល Hacker បញ្ចូលកូដ SQL ផ្ដេសផ្ដាសតាមរន្ធ Input (ដូចជាប្រឡោះ Search) ដើម្បីលួច ឬបំផ្លាញទិន្នន័យ។ ការការពារ: ត្រូវប្រើប្រាស់ Parameterized Queries ឬ Prepared Statements (ដែលត្រូវបាន handle រួចជាស្រេចក្នុង ORM ភាគច្រើន)។

- តើ CORS គឺជាអ្វី? Cross-Origin Resource Sharing គឺជាយន្តការសុវត្ថិភាពរបស់ Browser ដែលរារាំងមិនឱ្យ Frontend មកពី Domain ផ្សេង (ឧទាហរណ៍៖ domain-a.com) មក call API របស់ Server (domain-b.com) លើកលែងតែ Server អនុញ្ញាត។

- តើ XSS និង CSRF គឺជាអ្វី?

- XSS (Cross-Site Scripting): Hacker បញ្ចូលកូដ JavaScript ទៅក្នុង Web យើងដើម្បីលួច Token របស់ User -> ការពារដោយការ Sanitize input (សម្អាត input)។

- CSRF (Cross-Site Request Forgery): Hacker បោកបញ្ឆោតឱ្យ User ចុច Link មួយដើម្បីផ្ញើ Request មកកាន់ Web យើងទាំងដែល User កំពុង Login ជាប់ -> ការពារដោយប្រើ CSRF Token ឬ SameSite Cookie។

- តើ role-based permission គឺជាអ្វី? គឺជាការគ្រប់គ្រងសិទ្ធិដែលផ្អែកលើតួនាទី (Role) របស់ User (ឧទាហរណ៍៖ Role Admin, Manager, Customer នីមួយៗមានសិទ្ធិ call endpoints ខុសៗគ្នា)។

6. CRUD និង Project Implementation
- តើ CRUD មានអ្វីខ្លះ? C = Create (បង្កើត), R = Read (អាន/ទាញយក), U = Update (កែប្រែ), D = Delete (លុប)។

- តើអ្នកធ្វើ create user API យ៉ាងដូចម្តេច? ទទួល Request -> Validate input (Email, password) -> Check មើលក្រែងលោមាន Email ហ្នឹងហើយ -> Hash password -> រក្សាទុកក្នុង DB -> បញ្ជូន Response 201 Created ត្រឡប់ទៅវិញ។

- តើអ្នកធ្វើ update product API យ៉ាងដូចម្តេច? ទទួល Product ID តាម Path Param -> រកមើលក្នុង DB (បើអត់មានផ្ញើ 404) -> Validate ទិន្នន័យថ្មី -> ប្រើ PATCH ឬ PUT ដើម្បីកែប្រែ -> រក្សាទុក -> ផ្ញើ Response 200 OK។

- តើអ្នក validate input យ៉ាងដូចម្តេច? ខ្ញុំប្រើប្រាស់ Library (ដូចជា Joi/Zod ក្នុង Node.js ឬ Request Validation ក្នុង Laravel) ដើម្បីពិនិត្យមើល Required fields, ប្រភេទជំពូកទិន្នន័យ (String/Number) និង Format (ដូចជា Email, ទូរស័ព្ទ)។

- តើអ្នក handle file upload យ៉ាងដូចម្តេច? ប្រើ Middleware (ដូចជា multer ក្នុង Node.js) ដើម្បីទទួល File -> Validate ទំហំ និងប្រភេទ File (ឧទាហរណ៍៖ តែចំណាំ .jpg, .png) -> យក File ទៅរក្សាទុកលើ Cloud Storage (ដូចជា AWS S3) -> រួចយក URL របស់វាទៅរក្សាទុកក្នុង Database។

- តើអ្នកធ្វើ search និង filter data យ៉ាងដូចម្តេច? ទទួលពាក្យ Search តាមរយៈ Query Params (ឧទាហរណ៍៖ ?q=phone&category=electronics) រួចយកទៅសរសេរ SQL ជាមួយលក្ខខណ្ឌ WHERE category = :category AND name LIKE :q។

- តើអ្នកធ្វើ pagination សម្រាប់ list data យ៉ាងដូចម្តេច? ទទួលយក page និង limit ពី Query params រួចគណនា offset = (page - 1) * limit។ ក្នុង SQL ប្រើបញ្ជា LIMIT {limit} OFFSET {offset} រួច Count ចំនួនសរុប (Total rows) ផ្ញើទៅជាមួយ Response។

- តើអ្នកធ្លាប់ធ្វើ admin dashboard backend ដែរឬទេ? បាទ/ចាស ធ្លាប់។ ភាគច្រើនវាផ្ដោតលើការធ្វើ API សម្រាប់ទាញយករបាយការណ៍ (Analytics), ការធ្វើ CRUD គ្រប់គ្រងលើ User/Product និងការគ្រប់គ្រងប្រព័ន្ធ Log។

- តើអ្នកធ្លាប់ធ្វើ payment integration ដែរឬទេ? (ឆ្លើយតាមការពិត ឧទាហរណ៍៖ "បាទ ធ្លាប់ភ្ជាប់ជាមួយ ABA Payway ឬ Stripe ដោយប្រើប្រាស់ SDK របស់ពួកគេ និងការបង្កើត Webhook ដើម្បីទទួលការបញ្ជាក់ស្ថានភាពបង់ប្រាក់ពីធនាគារ")។

- តើអ្នកធ្លាប់ធ្វើ notification system ដែរឬទេ? ធ្លាប់។ សម្រាប់ Real-time ខ្ញុំប្រើ WebSockets (Socket.io) ឬ Firebase Cloud Messaging (FCM) សម្រាប់ Push Notification ទៅលើទូរស័ព្ទ។

7. System Design Basic
- តើអ្នក design login system យ៉ាងដូចម្តេច? Client ផ្ញើ Email/Password -> Server ផ្ទៀងផ្ទាត់ (Check email រួចប្រើ bcrypt ផ្ទៀងផ្ទាត់ password) -> បើត្រូវ បង្កើត JWT (Access token + Refresh token) ផ្ញើទៅឱ្យ Client រក្សាទុកក្នុង Secure cookie ឬ LocalStorage។

- តើអ្នក design ecommerce backend យ៉ាងដូចម្តេច? ត្រូវបែងចែកជា Tables ចម្បងៗ៖ users, products, carts, orders, order_items, និង payments។ ត្រូវមានប្រព័ន្ធគ្រប់គ្រង Stock (Inventory tracking) និងការប្រើប្រាស់ Database Transaction ពេល User កម្មង់ទិញ ដើម្បីកុំឱ្យ Stock ខុស។

- តើអ្នក design booking system យ៉ាងដូចម្តេច? ត្រូវមាន Table សំខាន់គឺ slots ឬ availability។ ពេលមានការកក់ ត្រូវប្រើប្រាស់ Database Lock (Pessimistic/Optimistic lock) ដើម្បីការពារបញ្ហា Double Booking (មនុស្សពីរនាក់កក់ចំកៅអី ឬម៉ោងតែមួយក្នុងពេលតែមួយ)។

- តើអ្នកធ្វើឲ្យ API fast យ៉ាងដូចម្តេច? ប្រើប្រាស់ Database Indexing, អនុវត្ត Caching (Redis), ប្រើ Pagination, ធ្វើការ Optimize កូដ (ចៀសវាង N+1 query problem), និងប្រើប្រាស់ Asynchronous background jobs។

- តើ caching គឺជាអ្វី? តើ Redis ប្រើសម្រាប់អ្វី? Caching គឺជាការរក្សាទុកទិន្នន័យដែលប្រើប្រាស់ញឹកញាប់នៅកន្លែងដែលទាញយកបានលឿន (Memory/RAM)។ Redis គឺជា In-memory data store ដែលពេញនិយមបំផុតសម្រាប់ធ្វើជា Cache server ព្រោះវាអាន-សរសេរបានលឿនបំផុត (កម្រិត Millisecond)។

- តើ message queue គឺជាអ្វី? តើ background job ប្រើសម្រាប់អ្វី?

- Background Job: គឺការយកការងារណាដែលចំណាយពេលយូរ (ដូចជាផ្ញើ Email, បង្កើត PDF) ចេញពីដំណើរការចម្បងរបស់ API ដើម្បីកុំឱ្យ User រង់ចាំ។

- Message Queue: (ដូចជា RabbitMQ, BullMQ) គឺជាធុងផ្ទុកជួរការងារ (Queue) ទាំងនោះ ដើម្បីឱ្យ Worker យកទៅដំណើរការម្តងមួយៗនៅ Background។

- តើ microservices និង monolith ខុសគ្នាយ៉ាងដូចម្តេច?

- Monolith: ប្រព័ន្ធទាំងមូល (គ្រប់ Features) នៅក្នុង Project តែមួយ កូដតែមួយដុំ ងាយស្រួលធ្វើដំបូងៗ តែពិបាកពង្រីកពេលធំ។

- Microservices: បំបែកប្រព័ន្ធធំមួយ ទៅជា Service តូចៗដាច់ពីគ្នា (ឧទាហរណ៍៖ Auth service, Order service) ដែលដើរលើ Server ផ្សេងគ្នា ងាយស្រួល Scale តែស្មុគស្មាញក្នុងការរៀបចំ។

- បើមាន users ច្រើន call API ក្នុងពេលតែមួយ តើអ្នក handle យ៉ាងដូចម្តេច? ខ្ញុំនឹងប្រើប្រាស់ Load Balancer ដើម្បីបែងចែក Traffic ទៅកាន់ Server ច្រើន, ប្រើប្រាស់ Horizontal Scaling (ថែម Server), ប្រើ Caching ឱ្យបានច្រើន និងអនុវត្ត Rate Limiting។

8. Git, Docker, Deployment
- តើ Git គឺជាអ្វី? គឺជា Version Control System ដែលប្រើសម្រាប់តាមដានរាល់ការផ្លាស់ប្តូរកូដ និងជួយឱ្យ Developer ច្រើននាក់អាចធ្វើការលើកូដតែមួយជាមួយគ្នាបានយ៉ាងរលូន។

- តើ GitHub, GitLab, Bitbucket ប្រើសម្រាប់អ្វី? គឺជា Platform លើ Cloud សម្រាប់ផ្ទុក Git Repositories (កូដរបស់យើង) និងផ្តល់ Tools បន្ថែមសម្រាប់ធ្វើការជាក្រុម ដូចជា Pull Request, Code review, និង CI/CD។

- តើ branch គឺជាអ្វី? គឺជាខ្សែបន្ទាត់នៃការអភិវឌ្ឍកូដដាច់ដោយឡែកមួយ (Isolated environment) ដែលអនុញ្ញាតឱ្យយើងសរសេរ Feature ថ្មីដោយមិនប៉ះពាល់ដល់កូដចម្បងដែលកំពុងដំណើរការ (main/master)។

- តើ git pull និង git fetch ខុសគ្នាយ៉ាងដូចម្តេច?

- Git fetch: ទៅទាញយកព័ត៌មានប្រែប្រួលថ្មីៗពី Cloud មក ប៉ុន្តែមិនទាន់យកមកបូកបញ្ចូល (Merge) ក្នុងកូដដែលយើងកំពុងសរសេរទេ។

- Git pull: គឺស្មើនឹង git fetch រួចធ្វើ git merge ភ្លាមៗ (ទាញមកផង ផ្គុំចូលកូដយើងផ្ទាល់តែម្តង)។

- តើ merge conflict គឺជាអ្វី? តើអ្នក resolve យ៉ាងដូចម្តេច? វាកើតឡើងនៅពេល Developer ២ នាក់ កែប្រែកូដចំជួរតែមួយនៅលើ File តែមួយ រួចព្យាយាមយកមកបញ្ចូលគ្នា។ ការដោះស្រាយ: ខ្ញុំបើក Code Editor (ដូចជា VS Code) មើលកូដទាំងពីរផ្នែក រួចពិភាក្សាជាមួយសហការីដើម្បីជ្រើសរើសយកកូដដែលត្រឹមត្រូវ រួចធ្វើការ Commit ម្តងទៀត។

- តើ Docker គឺជាអ្វី? គឺជា Tool មួយដែលជួយខ្ចប់ Application របស់យើង រួមទាំង Environment និង Dependencies ទាំងអស់របស់វា ទៅជាធុងមួយហៅថា Container ដែលធ្វើឱ្យវារត់បានដូចគ្នាបេះបិទនៅគ្រប់ទីកន្លែង (លើម៉ាស៊ីន Developer ឬលើ Production Server)។

- តើ environment variable គឺជាអ្វី? គឺជាតម្លៃអថេរដែលរក្សាទុកនៅក្រៅកូដ (ច្រើននៅក្នុង File .env) ប្រើសម្រាប់ផ្ទុកព័ត៌មានសម្ងាត់ ឬការកំណត់ដែលប្រែប្រួលតាម Server (ដូចជា DB_PASSWORD, API_KEY, PORT) ដើម្បីសុវត្ថិភាព។

- តើ CI/CD គឺជាអ្វី?

- CI (Continuous Integration): ការធ្វើតេស្ត (Automated Testing) និង build កូដដោយស្វ័យប្រវត្តិនរាល់ពេលមានការ push កូដថ្មី។

- CD (Continuous Deployment): ការយកកូដដែលតេស្តជាប់នោះ ទៅ deploy លើ Server ផ្ទាល់ដោយស្វ័យប្រវត្តិតែម្តង។

- តើអ្នកធ្លាប់ deploy backend ទៅ server ដែរឬទេ? ធ្លាប់ (រៀបរាប់តាមការពិត ដូចជា៖ Deploy ទៅកាន់ AWS (EC2), DigitalOcean, Heroku, ឬ Shared Hosting ដោយប្រើ Docker ឬ Git)។

9. Coding Test (សរសេរជា JavaScript/Node.js)
- ខាងក្រោមនេះជាកូដគំរូខ្លីៗ និងត្រឹមត្រូវដែលអាចយកទៅបង្ហាញពេលគេឱ្យធ្វើតេស្ត៖

១. Reverse String
```javascript
function reverseString(str) {
    return str.split('').reverse().join('');
}
// ឧទាហរណ៍: reverseString("hello") -> "olleh"
```

២. រក Duplicate Number ក្នុង Array
```javascript
function findDuplicates(arr) {
    let seen = new Set();
    let duplicates = new Set();
    for (let num of arr) {
        if (seen.has(num)) {
            duplicates.add(num);
        } else {
            seen.add(num);
        }
    }
    return Array.from(duplicates);
}
// ឧទាហរណ៍: findDuplicates([1, 2, 3, 2, 4, 1]) -> [2, 1]
```

៣. Count Word Frequency (រាប់ចំនួនពាក្យជាន់គ្នា)
```javascript
function countWords(str) {
    let words = str.toLowerCase().split(/\s+/);
    let frequency = {};
    for (let word of words) {
        frequency[word] = (frequency[word] || 0) + 1;
    }
    return frequency;
}
```

៤. API សម្រាប់ User Login (Express.js)
```javascript
app.post('/api/v1/auth/login', async (req, res) => {
    const { email, password } = req.body;
    if (!email || !password) return res.status(400).json({ message: "Missing fields" });

    const user = await db.findUserByEmail(email);
    if (!user) return res.status(401).json({ message: "Invalid credentials" });

    const isMatch = await bcrypt.compare(password, user.password);
    if (!isMatch) return res.status(401).json({ message: "Invalid credentials" });

    const token = jwt.sign({ id: user.id }, process.env.JWT_SECRET, { expiresIn: '1h' });
    return res.status(200).json({ token });
});
```

៥. CRUD API សម្រាប់ Product (Express.js គំរូខ្លះៗ)
```javascript
// CREATE
app.post('/products', async (req, res) => {
    const newProduct = await db.insertProduct(req.body);
    res.status(201).json(newProduct);
});
// READ ALL
app.get('/products', async (req, res) => {
    const products = await db.getAllProducts();
    res.status(200).json(products);
});
```

៦. SQL Query: Get Users ទាំងអស់
```sql
SELECT * FROM users;
```

៧. SQL Query: Join Users table និង Orders table
```sql
SELECT users.id, users.name, orders.order_date, orders.total_amount
FROM users
INNER JOIN orders ON users.id = orders.user_id;
```

៨. SQL Query: រក Monthly Sales Report
```sql
SELECT DATE_FORMAT(order_date, '%Y-%m') AS month, SUM(total_amount) AS total_sales
FROM orders
WHERE status = 'completed'
GROUP BY month
ORDER BY month DESC;
```

៩. Function សម្រាប់ Pagination Logic
```javascript
function getPaginationParams(query) {
    const page = parseInt(query.page) || 1;
    const limit = parseInt(query.limit) || 10;
    const offset = (page - 1) * limit;
    return { limit, offset };
}
```

១០. Validation សម្រាប់ Email និង Password (Regex ងាយៗ)
```javascript
function validateInput(email, password) {
    const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
    const isEmailValid = emailRegex.test(email);
    
    // លក្ខខណ្ឌ៖ Password ត្រូវមានប្រវែងយ៉ាងតិច 8 ខ្ទង់
    const isPasswordValid = password && password.length >= 8; 
    
    return isEmailValid && isPasswordValid;
}
```
