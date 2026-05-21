# Software Development Life Cycle (SDLC)

Software Development Life Cycle (SDLC) នេះហើយគឺជាឈ្មោះផ្លូវការ និងពេញនិយមបំផុតដែលគេប្រើប្រាស់នៅក្នុងវិស័យ IT និង Software Engineering (ចំណែកពាក្យ Developer Life Cycle ដែលយើងនិយាយមុននេះ គឺសំដៅលើដំណើរការតែមួយនេះឯង)។

ដើម្បីឱ្យអ្នកកាន់តែងាយស្រួលយល់ និងអាចយកទៅអនុវត្ត ឬត្រៀមសម្រាប់ការពិភាក្សាការងារ (Technical Discussions) ខ្ញុំសូមសង្ខេប SDLC នេះជាលក្ខណៈ Visual Workflow តាមលំដាប់លំដោយជាក់ស្តែងដូចខាងក្រោម៖

## ដំណើរការជាក់ស្តែងនៃ SDLC (The 6 core phases)

1. **Requirement Gathering & Analysis** (ដំណាក់កាលប្រមូលទិន្នន័យ): ប្រមូលរាល់មុខងារដែលចង់បាន (System Requirements) ពី Client ឬ Stakeholders រួចចងក្រងវាទៅជាឯកសារមួយហៅថា SRS (Software Requirement Specification)។
2. **System Design** (ដំណាក់កាលរចនាប្រព័ន្ធ): យកឯកសារ SRS មកបំបែកជាប្លង់បច្ចេកទេស។ រៀបចំ Database Schema (MySQL, PostgreSQL), ឌីហ្សាញ Architecture (Monolith ឬ Microservices) និងគូររូបរាង UI/UX (Figma)។
3. **Coding / Implementation** (ដំណាក់កាលសរសេរកូដ): Developers ចាប់ផ្តើមសរសេរកូដជាក់ស្តែង។ បង្កើត API ជាមួយ Backend (NestJS, .NET), រៀបចំ Frontend (Angular, React) និងភ្ជាប់ប្រព័ន្ធផ្សេងៗចូលគ្នា។
4. **Testing & Quality Assurance** (ដំណាក់កាលពិនិត្យគុណភាព): កូដដែលសរសេររួច ត្រូវហុចទៅឱ្យក្រុម QA/Testers ដើម្បីរាវរក Bugs។ គេធ្វើការ Test ទាំងមុខងារ (Functional Testing) និងល្បឿនរបស់ប្រព័ន្ធ (Performance Testing)។
5. **Deployment** (ដំណាក់កាលដាក់ឱ្យដំណើរការ): បោះ (Deploy) គម្រោងដែលគ្មាន Bug ទៅកាន់ Production Server (ដូចជា AWS EC2, Cloud Hosting) ឬរៀបចំប្រព័ន្ធ CI/CD ដើម្បីកុម្ម៉ង់ឱ្យវាដំណើរការដោយស្វ័យប្រវត្តិ។
6. **Maintenance & Evolution** (ដំណាក់កាលថែទាំ): តាមដានមើលដំណើរការប្រព័ន្ធ (Monitoring) ជួសជុលបញ្ហាដែលកើតឡើងយថាហេតុ និងត្រៀមលក្ខណៈ Update បន្ថែមមុខងារថ្មីៗទៅតាមការចង់បានរបស់អ្នកប្រើប្រាស់។

## ហេតុអ្វីបានជា SDLC មានសារៈសំខាន់ខ្លាំង?

គម្រោង Software ភាគច្រើនដែលដួលរលំ ឬយឺតយ៉ាវជាងផែនការ មិនមែនមកពី Developers អន់មិនចេះសរសេរកូដនោះទេ គឺមកពី ការខ្វះខាតការអនុវត្តតាម SDLC ត្រឹមត្រូវ។

ការមាន SDLC ច្បាស់លាស់ជួយឱ្យ៖

- **កាត់បន្ថយការចំណាយ (Reduce Costs):** ការរកឃើញ Bug ក្នុងវគ្គ Testing ឬ Design គឺចំណាយលុយ និងពេលតិចជាងការទៅតាមកែពេល App ដាក់ឱ្យគេប្រើប្រាស់រួច (Live)។
- **ការងារមានតម្លាភាព (Transparency):** គ្រប់គ្នាក្នុង Team (ចាប់តាំងពី Project Manager, Designers, Developers ដល់ QA) ដឹងថាខ្លួនត្រូវធ្វើអ្វី និងស្ថិតក្នុងដំណាក់កាលណា។
- **គុណភាពខ្ពស់ (High Quality):** ធានាថា Product ចុងក្រោយដែលចេញទៅ គឺចំទៅនឹងអ្វីដែល Client ចង់បាន និងមាន Standard ត្រឹមត្រូវ។
