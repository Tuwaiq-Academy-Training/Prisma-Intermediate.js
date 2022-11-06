محتوى الملف:
- العلاقات 
- أنواع العلاقات


## العلاقات

#### العلاقة هي اتصال بين نموذجين في مخطط Prisma. على سبيل المثال ، هناك علاقة رأس بأطراف بين المستخدم والمنشور لأن مستخدم واحد يمكن أن يكون لديه العديد من منشورات المدونة.

#### يحدد مخطط Prisma التالي علاقة رأس بأطراف بين نموذج User و Post. يتم تمييز الحقول المتضمنة في تحديد العلاقة:

    model User {
      id    Int    @id @default(autoincrement())
      posts Post[]
    }

    model Post {
      id       Int  @id @default(autoincrement())
      author   User @relation(fields: [authorId], references: [id])
      authorId Int // relation scalar field  (used in the `@relation` attribute above)
    }

على مستوى Prisma ، تتكون علاقة المستخدم / المنشور من:

- مجالان للعلاقة: المؤلف والمشاركات. تحدد حقول العلاقة الاتصالات بين النماذج على مستوى Prisma ولا توجد في قاعدة البيانات. تُستخدم هذه الحقول لإنشاء عميل Prisma.
- حقل معرف المؤلف القياسي ، والذي تتم الإشارة إليه بواسطة السمةrelation. هذا الحقل موجود في قاعدة البيانات - إنه المفتاح الخارجي الذي يربط Post والمستخدم.

على مستوى Prisma ، يتم دائمًا تمثيل الاتصال بين نموذجين بحقل علاقة على كل جانب من جوانب العلاقة.

#### قواعد البيانات العلائقية

يحدد الرسم التخطيطي لعلاقة الكيان التالي نفس علاقة one-to-many بين جدول المستخدم و Post في قاعدة بيانات علائقية:

![image](https://www.prisma.io/docs/static/e83a6a5933258930b5e6b7bc6f1bf839/e548f/one-to-many.png)

#### في SQL ، يمكنك استخدام مفتاح خارجي لإنشاء علاقة بين جدولين. يتم تخزين المفاتيح الخارجية على جانب واحد من العلاقة. يتكون مثالنا من:

- عمود مفتاح خارجي في جدول النشر يسمى authorId.
- عمود المفتاح الأساسي في جدول المستخدم المسمى id. يشير عمود authorId في جدول Post إلى عمود المعرف في جدول المستخدم.

في مخطط Prisma ، يتم تمثيل علاقة المفتاح الخارجي / المفتاح الأساسي بواسطة السمةrelation في حقل المؤلف:

      author     User        @relation(fields: [authorId], references: [id]) 


## أنواع العلاقات

- ال One-to-one (وتسمى أيضًا علاقة 1-1)
- ال One-to-many (وتسمى أيضًا علاقة 1-n)
- ال Many-to-many (وتسمى أيضًا علاقة m-n)

#### يتضمن مخطط Prisma التالي كل نوع من العلاقات:



ا ![image](https://user-images.githubusercontent.com/92247967/200191773-5b94c20c-99f8-4994-85a5-d7b04be4b598.png)

#### يمثل الرسم التخطيطي لعلاقة الكيان التالي قاعدة البيانات التي تتوافق مع نموذج مخطط Prisma:

![image](https://www.prisma.io/docs/static/80c7aa384ca962faf5be1ee91f89aa7e/e548f/sample-schema.png)

## مجالات العلاقة

- يجب أن تحتوي كل علاقة على حقلي علاقة بالضبط ، واحد في كل نموذج. في حالة العلاقات 1-1 و 1-n ، يلزم وجود حقل عددي للعلاقة إضافي يتم ربطه بأحد حقلي العلاقة في السمةrelation. مقياس العلاقة هذا هو التمثيل المباشر للمفتاح الخارجي في قاعدة البيانات الأساسية.

#### ضع في اعتبارك هذه النماذج:

       model User {
         id      Int      @id @default(autoincrement())
         posts   Post[]
         profile Profile?
       }

       model Profile {
         id     Int  @id @default(autoincrement())
         user   User @relation(fields: [userId], references: [id])
         userId Int  @unique // relation scalar field (used in the `@relation` attribute above)
       }

       model Post {
         id         Int        @id @default(autoincrement())
         author     User       @relation(fields: [authorId], references: [id])
         authorId   Int // relation scalar field  (used in the `@relation` attribute above)
         categories Category[]
       }

       model Category {
         id    Int    @id @default(autoincrement())
         posts Post[]
       }
