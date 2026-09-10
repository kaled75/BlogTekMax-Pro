# خطة تنفيذ إصلاحات قالب BlogTekMax Pro (النقاط الـ 7)

تهدف هذه الخطة إلى معالجة المشاكل السبع المحددة بالترتيب، كنقاط منفصلة ومستقلة، مع الحفاظ الكامل على التصميم الحالي (الألوان، التخطيط، والـ layout) ودون كسر بنية القالب أو وظائف إعلانات المقال وميزات لوحة التخصيص.

---

## User Review Required

> [!IMPORTANT]
> **ترتيب التنفيذ وسلامة القالب:**
> سيتم تنفيذ كل نقطة كتعديل منفصل مع تدقيق آلي لصحة بنية الـ XML (`ElementTree`) واختبار المتصفح للتأكد من عدم حدوث أي تراجع بصري أو وظيفي.

---

## Proposed Changes

### 1. [عاجل] تحويل قسم الـ Hero Slider والمقالات الرائجة (Trending) إلى البناء السيرفري المباشر عبر `b:loop`
* **المشكلة الحالية**: يعتمد قسم الـ Hero Slider في الصفحة الرئيسية على ودجت `FeaturedPost` (الذي يوفر مقالاً واحداً فقط)، بينما المقالان 2 و 3 في القائمة الجانبية يتم جلبهما عبر JavaScript (`fetch` من `/feeds/posts/default?alt=json`) مع `style='display:none;'` مؤقتاً مما يؤخر ظهور المحتوى ويضر بـ LCP.
* **الحل**:
  1. تحويل ودجت قسم السلايدر في `featured-hero-section` إلى ودجت يوفر مصفوفة المشاركات `data:posts` (مثل ودجت `PopularPosts` أو ودجت المشاركات متعدد المقالات).
  2. استخدام `<b:loop values='data:posts' var='post' index='i'>` لبناء:
     * المقال الرئيسي الأول (`i == 0`): `hero-main-card` بالصورة والعنوان والتصنيف والكاتب والتاريخ مباشرة من سيرفر بلوجر بدون `display:none`.
     * المقالان الثاني والثالث (`i == 1` و `i == 2`): `hero-sub-card` داخل `hero-side-stack` بالصورة والعنوان والتاريخ مباشرة من الـ HTML الأولي.
  3. في قسم الـ Trending (`PopularPosts2`): الودجت يحتوي بالفعل على `b:loop values='data:posts'`؛ سنقوم بحذف دالة الـ JavaScript `fetch('/feeds/posts/default?alt=json&start-index=4&max-results=3')` والاعتماد بنسبة 100% على التوليد السيرفري الفوري من بلوجر.
  4. حذف توابع JS fetch من سكربت التغذية لرفع سرعة التحميل الأولي (FCP و LCP).

---

### 2. [عاجل] إضافة وسوم Open Graph و Twitter Card داخل `<head>`
* **المشكلة الحالية**: يفتقر القالب لوسوم بيانات المشاركة الاجتماعية المتوافقة مع فيسبوك وتويتر/إكس وتيليجرام وواتساب.
* **الحل**:
  إضافة وسوم `<meta>` ديناميكية داخل `<head>` قبل `</head>`:
  * في صفحات المقالات (`data:view.isSingleItem`):
    * `og:title` = `data:view.title.escaped`
    * `og:description` = `data:view.description.escaped ?: data:blog.metaDescription`
    * `og:image` = `data:view.featuredImage` (مع fallback لصورة افتراضية عالية الدقة)
    * `og:url` = `data:view.url.canonical`
    * `og:type` = `article`
    * `twitter:card` = `summary_large_image`
    * `twitter:title`, `twitter:description`, `twitter:image`
  * في الصفحة الرئيسية والأرشيف:
    * `og:title` = `data:blog.title`
    * `og:description` = `data:blog.metaDescription ?: data:blog.title`
    * `og:url` = `data:blog.canonicalHomepageUrl ?: data:blog.homepageUrl`
    * `og:type` = `website`
    * `og:image` = صورة الشعار / صورة الهوية الافتراضية.

---

### 3. [مهم] تصحيح ازدواجية وسم `H1`
* **المشكلة الحالية**: يحتوي القالب على وسمي `<h1>`: الأول في عنوان/شعار الموقع بالهيدر داخل `<b:includable id='title'>`، والثاني في عنوان المقال الرئيسي `.article-headline`.
* **الحل**:
  1. في `<b:includable id='title'>`: تغيير `<h1>` إلى `<div class='site-title-heading'>`.
  2. في CSS: تحديث أي قاعدة تنسيق خاصة بـ `.header-brand h1` أو `.site-header h1` لتشمل `.site-title-heading` مع الحفاظ التام والكامل على نفس المظهر وحجم الخط والألوان.
  3. إبقاء وسم `<h1>` حصرياً وفقط لعنوان المقال داخل صفحة المقال (`.article-headline`).

---

### 4. [مهم] تحسين تحميل خطوط Google Fonts (تحميل الخط الافتراضي فقط في `<head>`)
* **المشكلة الحالية**: يحمل رابط Google Fonts خمس عائلات خطوط (Cairo, Tajawal, IBM Plex Sans, Noto Kufi, Noto Sans) دفعة واحدة في `<head>`.
* **الحل**:
  1. تعديل رابط `<link>` في `<head>` ليحمل **فقط الخط الافتراضي Cairo** (بوزني 400 و 700 مع `display=swap`).
  2. إضافة كود JavaScript خفيف وذكي في الفوتر: يفحص قيمة `--font-choice` المختارة من قِبل المستخدم في لوحة التخصيص:
     * إذا كانت القيمة `2px` (Tajawal): يقوم تلقائياً بحقن رابط خط Tajawal.
     * إذا كانت القيمة `3px` (IBM Plex): يقوم بحقن رابط خط IBM Plex Sans Arabic.
     * إذا كانت القيمة `4px` (Noto Kufi): يقوم بحقن رابط خط Noto Kufi Arabic.
     * إذا كانت القيمة `5px` (Noto Sans): يقوم بحقن رابط خط Noto Sans Arabic.
     * إذا كانت القيمة الافتراضية `1px` (Cairo): لا يحمل أي شيء إضافي.
  3. هذا يضمن تحميل خط واحد فقط في جميع الأوقات لتقليل حجم الطلبات إلى أقصى حد.

---

### 5. [متوسط] ربط السمة `alt` في الصور بعنوان المقال ديناميكياً
* **المشكلة الحالية**: توجد سمات `alt=''` فارغة في صور المحتوى الرئيسية.
* **الحل**:
  تحديث الصور في قوالب بلوجر التالية بربط السمة بعنوان المقال:
  * `hero-main-img` -> `expr:alt='data:post.title'`
  * `article-featured-img` -> `expr:alt='data:post.title'`
  * `stream-thumb` -> `expr:alt='data:post.title'`
  * `trending-thumb` -> `expr:alt='data:post.title'`
  * `editors-pick-thumb` -> `expr:alt='data:post.title'`
  * في قوالب الجافاسكريبت إن وُجدت -> `alt="${title}"`

---

### 6. [بسيط] إزالة التكرار لـ CSS `.test-ad-box` ونقله إلى `<b:skin>`
* **المشكلة الحالية**: يوجد 8 بلوكات `<style>` متطابقة مكررة داخل كل ودجت إعلاني (`HTML201` إلى `HTML207`).
* **الحل**:
  1. نقل كود CSS الخاص بـ `.test-ad-wrapper`, `.test-ad-box`, `.test-ad-box:hover`, `.test-ad-label`, `.test-ad-dimensions` مرة واحدة داخل `<b:skin>`.
  2. حذف وسوم `<style> ... </style>` المتكررة بالكامل من كل ودجت إعلاني، مما يوفر كيلوبايتات من كود الصفحة ويحسن نقاء الـ HTML.

---

### 7. [بسيط] تقليل استخدام `!important` عبر رفع تخصيص الـ Selectors (Specificity)
* **المشكلة الحالية**: يوجد 208 استخداماً لـ `!important` (85 منها في `<b:template-skin>` للوحة التنسيق، و 123 في `<b:skin>` للموقع الحي).
* **الحل**:
  1. الإبقاء على ما يلزم في `<b:template-skin>` الخاص بـ `body#layout` لأن لوحة تحكم بلوجر تفرض أنماطاً ذات أولوية عليا.
  2. مراجعة وتقليل `!important` في كود CSS الرئيسي عبر رفع تخصيص الـ Selectors (مثلاً استخدام `html body .stream-card`, `body .sidebar .widget`, `.main-wrapper .main-content` بدلاً من استخدام `!important` العشوائي).
  3. فحص الموقع بصرياً والتأكد من عدم كسر أي تنسيق.

---

## Verification Plan

### Automated Tests
1. **XML Syntax & Well-Formedness**:
   `python -c "import xml.etree.ElementTree as ET; ET.parse('BlogTekMax Pro.xml'); print('VALID')"`
2. **In-Article Ads & Selectors Integrity Audit**:
   `python scratch/run_in_article_audit.py`
3. **Auditing the 7 Issues**:
   تشغيل سكربت الفحص المخصص `scratch/audit_7_issues.py` للتحقق من:
   * خلو السلايدر والتريندنج من `fetch`.
   * وجود وسوم `og:` و `twitter:`.
   * خلو الهيدر من `h1`.
   * ضبط رابط الخطوط على Cairo فقط في الرأس.
   * ضبط سمات `alt` على `data:post.title`.
   * خلو ودجات الإعلانات من كتل `<style>` المكررة.
   * انخفاض عدد `!important`.

### Manual & Visual Verification
* فحص المتصفح للصفحة الرئيسية وصفحة المقال عبر أداة المتصفح الفرعية للتأكد من ثبات التصميم المسطح وتناسق الألوان والخطوط 100%.
