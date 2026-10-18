# مرجع الأكواد والأنماط التقنية — فيلم الحمراء

> **وظيفة هذه الوثيقة:** حفظ الأكواد والأمثلة التقنية التي وردت في الحوار المرجعي مع كلود، لاستخدامها كمرجع عند بناء المشروع.
> **تنبيه:** الأكواد أدناه أمثلة مرجعية تحتاج إلى اختبار وتعديل بعد فحص الأصول الفعلية، مدة التعليق، توفر الحزم، ودقة النص العربي. لا تعتبر هذه الوثيقة بديلاً عن وثيقة التوجيه الإنتاجية.

## قواعد استخدام المرجع

- خذ من هذه الوثيقة البنية والمنهج، لا التوقيتات أو أسماء الملفات بشكل أعمى.
- اربط التوقيت النهائي بملف التعليق الصوتي الفعلي.
- استبدل أي Plugin غير متاح ببديل مفتوح أو تنفيذ SVG/CSS/Canvas.
- ثبّت العشوائية في الجسيمات باستخدام Seed حتى يكون الرندر قابلاً للتكرار.
- اختبر تشكيل العربية داخل Chromium قبل الرندر الطويل.
- لا تستخدم أي مفتاح API أو قيمة سرية مكتوبة داخل الكود.


---

### مرجع كود — الأسطر الأصلية 150 وما بعدها

السياق الأصلي: ### 2. هيكل مجلدات العمل - Project Structure

```text
D:\Alhambra_Doc_4K\
│
├── 01_AEP\ Alhambra_Main_v01.aep
├── 02_Assets\
│   ├── 01_AI_Vector\ (Zellige_Patterns.ai, Lions.ai, Arches.ai)
│   ├── 02_PSD_Parallax\ (Alhambra_Palace_Layers.psd)
│   ├── 03_Textures\ (Red_Brick.jpg, Gold_Foil.jpg, Marble.jpg)
│   ├── 04_3D\ (Mocarnas.c4d)
│   └── 05_VO_Music\ (VO.wav, Music.wav, SFX\)
├── 03_PreRenders\
├── 04_Footage_Stock\ (Drone_Alhambra_4K.mp4)
└── 05_Export\ Alhambra_Final_4K.mp4
```


---

### مرجع كود — الأسطر الأصلية 199 وما بعدها

السياق الأصلي: ### الخطوة 1: إنشاء هيكل المجلدات — انسخ هذا الكود واحفظه باسم `create_project.bat` وشغله، رح ينشئ لك كل المجلدات تلقائياً في المسار `D:\Alhambra_Doc_4K`

```bat
@echo off
mkdir "D:\Alhambra_Doc_4K"
mkdir "D:\Alhambra_Doc_4K\01_AEP"
mkdir "D:\Alhambra_Doc_4K\02_Assets\01_AI_Vector"
mkdir "D:\Alhambra_Doc_4K\02_Assets\02_PSD_Parallax"
mkdir "D:\Alhambra_Doc_4K\02_Assets\03_Textures"
mkdir "D:\Alhambra_Doc_4K\02_Assets\04_3D"
mkdir "D:\Alhambra_Doc_4K\02_Assets\05_VO_Music\SFX"
mkdir "D:\Alhambra_Doc_4K\03_PreRenders"
mkdir "D:\Alhambra_Doc_4K\04_Footage_Stock"
mkdir "D:\Alhambra_Doc_4K\05_Export"
echo تم انشاء مشروع الحمراء بنجاح
pause
```


---

### مرجع كود — الأسطر الأصلية 221 وما بعدها

السياق الأصلي: ### الخطوة 2: بناء مشروع After Effects تلقائياً — **طريقة التشغيل:** افتح After Effects > File > Scripts > Run Script File > اختر الملف `Alhambra_Setup.jsx`

```javascript
// Alhambra_Setup.jsx - Cinematic 2.5D Project Builder
// Claude Opus 5.5 | For After Effects 2024+

var compW = 3840, compH = 2160, compDur = 240, compFPS = 25;
var mainComp = app.project.items.addComp("MAIN_4K_MASTER", compW, compH, 1, compDur, compFPS);
mainComp.bgColor = [0.039, 0.086, 0.157]; // #0A1628

// إنشاء الكاميرا السينمائية 35mm
var cam = mainComp.layers.addCamera("CINEMA CAM 35mm", [1920,1080]);
cam.property("Camera Options").property("Zoom").setValue(1200);
cam.property("Camera Options").property("Depth of Field").setValue(1);
cam.property("Camera Options").property("Focus Distance").setValue(2500);
cam.property("Camera Options").property("Aperture").setValue(180);
cam.property("Camera Options").property("Blur Level").setValue(9);

// إنشاء الإضاءة الثلاثية
var light1 = mainComp.layers.addLight("Key Light", [1920,1080]);
light1.property("Light Options").property("Intensity").setValue(140);
var light2 = mainComp.layers.addLight("Fill Light", [1920,1080]);
var nullCtrl = mainComp.layers.addNull(); nullCtrl.name = "MASTER_CONTROL";
nullCtrl.effect.addProperty("ADBE Color Control"); nullCtrl.effect.property(1).property("Color").setValue([0.77,0.66,0.50]); // Gold

// إنشاء 9 مشاهد Pre-Comps وربطها بالكومب الرئيسي
var scenes = [
    "01_INTRO_00-00-12", "02_ISOMETRIC_00-12", "03_MAP_1238_00-35",
    "04_BLUEPRINT_WATER_01-00", "05_RIHAN_HALL_01-30",
    "06_LIONS_HALL_02-00", "07_FALL_1492_02-40", "08_RESTORATION_03-10", "09_FINALE_03-30"
];
for(var i=0; i<scenes.length; i++){
    var sc = app.project.items.addComp(scenes[i], compW, compH, 1, compDur, compFPS);
    sc.bgColor = [0.545, 0.118, 0.118]; // #8B1E1E fallback
    mainComp.layers.add(sc);
}

// إضافة ماركرز متزامنة مع النص المنطوق
var markers = [
    {t:0, c:"هل يمكن لحجر أن يحفظ الشعر؟"},
    {t:12, c:"هنا فوق هضبة السبيكة - Isometric Reveal"},
    {t:35, c:"في عام 1238 - Map Animation"},
    {t:60, c:"اختار هضبة السبيكة - Blueprint Build"},
    {t:90, c:"بهو الريحان - Water Reflection"},
    {t:120, c:"بهو الأسود - 12 Lions + Muqarnas"},
    {t:160, c:"2 يناير 1492 - سقوط غرناطة"},
    {t:190, c:"ظن الجميع أن الحمراء ستموت - Restoration"},
    {t:210, c:"الخاتمة - ولا غالب إلا الله - Glow Out"}
];
for(var m=0; m<markers.length; m++){
    var mp = new MarkerValue(markers[m].c);
    mainComp.markerProperty.setValueAtTime(markers[m].t, mp);
}
alert("تم بناء مشروع الحمراء السينمائي 4K بنجاح! \n9 Scenes + Camera 3D + Markers جاهزة");
```


---

### مرجع كود — الأسطر الأصلية 331 وما بعدها

السياق الأصلي: ### 1. هيكل المشروع البرمجي

```bash
D:\Alhambra_CodeGen\
├── main.js                 # المحرك الرئيسي - يرندر كل المشاهد
├── config.js               # كل الألوان والخطوط والتوقيت
├── package.json
├── templates\
│   ├── base.html           # قالب HTML للرندر
│   └── scenes.json         # ستوري بورد كامل 9 مشاهد
├── assets\
│   ├── fonts\              # Reem Kufi, Amiri
│   ├── svg\                # Zellige.svg, Lions.svg
│   ├── textures\           # brick.jpg, gold.jpg
│   └── audio\              # VO.wav, music.wav
└── output\
    ├── frames\             # صور PNG لكل فريم
    └── Alhambra_Final_4K.mp4
```


---

### مرجع كود — الأسطر الأصلية 351 وما بعدها

السياق الأصلي: ### 2. الملف الأساسي `config.js` - الهوية البصرية بالأرقام

```javascript
// config.js
export const PALETTE = {
  nasridRed: "#8B1E1E",
  gold: "#C5A880",
  teal: "#0F4C5C",
  ivory: "#F2E8D5",
  midnight: "#0A1628"
};
export const FONTS = {
  title: "'Reem Kufi', sans-serif",
  quran: "'Amiri Quran', serif",
  body: "'IBM Plex Sans Arabic', sans-serif"
};
export const RENDER = {
  width: 3840,
  height: 2160,
  fps: 25,
  duration: 240 // 4 دقائق
};
// التزامن الدقيق للصوت - بالثواني
export const TIMELINE = [
  { id: "01_intro", start: 0, end: 12, vo: "هل يمكن لحجر أن يحفظ الشعر؟" },
  { id: "02_isometric", start: 12, end: 35, vo: "هنا فوق هضبة السبيكة" },
  { id: "03_map", start: 35, end: 60, vo: "في عام 1238" },
  { id: "04_blueprint", start: 60, end: 90, vo: "اختار هضبة السبيكة" },
  { id: "05_rihan", start: 90, end: 120, vo: "بهو الريحان" },
  { id: "06_lions", start: 120, end: 160, vo: "بهو الأسود - 12 أسداً" },
  { id: "07_fall", start: 160, end: 190, vo: "2 يناير 1492" },
  { id: "08_restore", start: 190, end: 210, vo: "لكن الحمراء رفضت أن تموت" },
  { id: "09_finale", start: 210, end: 240, vo: "ولا غالب إلا الله" },
];
```


---

### مرجع كود — الأسطر الأصلية 387 وما بعدها

السياق الأصلي: ### 3. محرك الرندر `main.js` - هذا اللي يسوي شغل الافتر ايفكت

```javascript
// main.js - Node.js + Puppeteer + FFmpeg
import puppeteer from 'puppeteer';
import { execSync } from 'child_process';
import { TIMELINE, RENDER } from './config.js';

const browser = await puppeteer.launch({ headless: true, args: ['--no-sandbox'] });
const page = await browser.newPage();
await page.setViewport({ width: 3840, height: 2160 });

// 1. رندر كل مشهد كـ PNG Sequence مع GSAP
for (const scene of TIMELINE) {
  await page.goto(`file://${process.cwd()}/templates/base.html?scene=${scene.id}`);
  await page.waitForTimeout(1500); // انتظار تحميل الخطوط والـ SVG
  
  // تسجيل 25 فريم لكل ثانية
  const frames = (scene.end - scene.start) * RENDER.fps;
  for (let f = 0; f < frames; f++) {
    await page.evaluate((frame) => window.seekTo(frame), f);
    await page.screenshot({ 
      path: `output/frames/${scene.id}_${String(f).padStart(4,'0')}.png`,
      omitBackground: false 
    });
  }
  console.log(`✓ تم رندر ${scene.id} - ${frames} فريم`);
}
await browser.close();

// 2. تجميع الفيديو بـ FFmpeg مع تأثيرات احترافية
execSync(`
ffmpeg -y -framerate 25 -i output/frames/%d.png 
-i assets/audio/VO.wav -i assets/audio/music.wav
-filter_complex "
[0:v] scale=3840:2160, 
      boxblur=luma_radius=2:luma_power=1:chroma_radius=0 [blur];
[blur] vignette=angle=PI/4:mode=forward:factor=0.25 [vignetted];
[vignetted] eq=contrast=1.05:saturation=1.1:brightness=0.02 [graded]
" 
-c:v libx264 -pix_fmt yuv420p -crf 16 -preset slow
-c:a aac -b:a 320k -shortest
output/Alhambra_Final_4K.mp4
`);
console.log("تم تصدير الفيديو النهائي 4K");
```


---

### مرجع كود — الأسطر الأصلية 436 وما بعدها

السياق الأصلي: ### 4. قالب `base.html` - هنا يصير الموشن الحقيقي

```html
<div id="scene-06" class="scene" style="background: linear-gradient(135deg, #0A1628, #8B1E1E)">
  <!-- طبقات الـ Parallax - كل طبقة تتحرك بسرعة مختلفة -->
  <div class="parallax-layer" data-speed="0.2" id="layer-sky"></div>
  <div class="parallax-layer" data-speed="0.5" id="layer-palace"></div>
  <div class="parallax-layer" data-speed="0.8" id="layer-lions">
    <svg id="lions-svg"><!-- 12 أسد مرسومة بـ SVG --></svg>
  </div>
  <!-- النص الذهبي مع تأثير Saber Glow -->
  <h1 class="gold-text" style="font-family:'Reem Kufi'; color:#C5A880; text-shadow: 0 0 20px #C5A880, 0 0 40px #C5A880;">
    ولا غالب إلا الله
  </h1>
  <!-- غبار ذهبي Particular -->
  <canvas id="particles"></canvas>
</div>

<script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.2/gsap.min.js"></script>
<script>
// تحريك احترافي مثل After Effects - S-Curve
window.seekTo = (frame) => {
  const progress = frame / (25*40); // 40 ثانية للمشهد
  
  // 1. Parallax: كل طبقة تتحرك بسرعة مختلفة لمحاكاة العمق 3D
  document.querySelectorAll('.parallax-layer').forEach(layer => {
    const speed = parseFloat(layer.dataset.speed);
    gsap.set(layer, { x: progress * 800 * speed, scale: 1 + progress*0.1*speed });
  });
  
  // 2. توهج النص - Saber Effect
  gsap.set('.gold-text', { opacity: progress, filter: `blur(${2-progress*2}px)` });
  
  // 3. دوران الأسود ببطء
  gsap.set('#lions-svg', { rotation: progress * 15 });
}
// جسيمات ذهبية - بديل Trapcode Particular
const canvas = document.getElementById('particles');
const ctx = canvas.getContext('2d');
// ... كود جسيمات 200 نقطة ذهبية تتحرك عشوائيا
</script>
```


---

### مرجع كود — الأسطر الأصلية 481 وما بعدها

السياق الأصلي: ### 5. أوامر FFmpeg النهائية للمسارات المتقدمة

```bash
# تأثير Chroma Key لإزالة الخلفية الخضراء للزخارف
ffmpeg -i scene.mov -vf "chromakey=0x00FF00:0.3:0.1" -c:v prores_ks output.mov

# تأثير Glow الذهبي للنقوش (بديل Saber)
ffmpeg -i input.mp4 -filter_complex "[0]split[a][b];[b]gblur=sigma=15,eq=brightness=0.5[aura];[a][aura]blend=screen:opacity=0.8" glow.mp4

# دمج 5 طبقات Parallax مع سرعات مختلفة
ffmpeg -i sky.png -i mountains.png -i palace.png -i lions.png -i text.png \
-filter_complex "[1][0]overlay=x='-200+t*5':y=0[tmp1];[2][tmp1]overlay=x='-400+t*10'[tmp2];[3][tmp2]overlay=x='-600+t*18'[tmp3];[4][tmp3]overlay" final.mp4
```


---

### مرجع كود — الأسطر الأصلية 495 وما بعدها

السياق الأصلي: # دمج 5 طبقات Parallax مع سرعات مختلفة — ### طريقة التشغيل عبر Claude Code

```bash
npm install puppeteer gsap
# ضع ملف التعليق الصوتي VO.wav في assets/audio/
node main.js
# النتيجة: output/Alhambra_Final_4K.mp4 جاهز 4 دقائق متزامن
```


---

### مرجع كود — الأسطر الأصلية 510 وما بعدها

السياق الأصلي: ### إعدادات ElevenLabs قبل التوليد

```text
Model: eleven_multilingual_v2
Voice: Adam - Deep Documentary  أو Bassam - Arabic Documentary
Stability: 50
Similarity Boost: 75
Style: 35
Speaker Boost: ON
Speed: 0.92
```


---

### مرجع كود — الأسطر الأصلية 525 وما بعدها

السياق الأصلي: ### السكريبت الكامل - انسخه كما هو — **المقطع 1 [00:00 - 00:12] - المقدمة - نبرة هادئة وعميقة**

```text
هل يمكن لحجرٍ أن يحفظ الشعر؟ 
وهل يمكن لجدرانٍ أن تروي تاريخاً عمره ثمانية قرون؟
```


---

### مرجع كود — الأسطر الأصلية 531 وما بعدها

السياق الأصلي: **المقطع 1 [00:00 - 00:12] - المقدمة - نبرة هادئة وعميقة** — **المقطع 2 [00:12 - 00:35] - كشف القصر**

```text
هنا... فوق هضبة السبيكة، حيث تعانق السماء مدينة غرناطة...
يقف قصرٌ لم يُبنَ ليكون مجرد قصر.

بل كُتب ليكون قصيدة.

إنه قصر الحمراء... آخر شاهدٍ حي على مجد الأندلس، وأجمل ما تركه المسلمون في أوروبا.
```


---

### مرجع كود — الأسطر الأصلية 541 وما بعدها

السياق الأصلي: **المقطع 3 [00:35 - 01:00] - الولادة**

```text
في عام ألفٍ ومئتين وثمانيةٍ وثلاثين للميلاد، والعالم الإسلامي في الأندلس ينهار...
مدنٌ تسقط واحدة تلو الأخرى بيد قشتالة.

ظهر رجلٌ واحد قرر أن يصنع معجزة.

إنه محمد بن يوسف بن نصر، الملقب بابن الأحمر.
```


---

### مرجع كود — الأسطر الأصلية 551 وما بعدها

السياق الأصلي: **المقطع 4 [01:00 - 01:30] - بناء الحلم**

```text
انسحب إلى غرناطة، وهناك على أنقاض قلعة مهجورة، وضع حجر الأساس لحلمٍ أخير.

اختار هضبة السبيكة الاستراتيجية، وجلب الماء من نهر دارو عبر قناة طولها ستة كيلومترات، فأحيا الأرض القاحلة.

وسماه الحمراء، للون طينه الأحمر الذي كان يجف تحت شمس الأندلس، وليكون لوناً لسلالته... بنو الأحمر.

لم يكن يبني حصناً فقط، كان يبني وطناً أخيراً للمسلمين في أوروبا.
```


---

### مرجع كود — الأسطر الأصلية 562 وما بعدها

السياق الأصلي: **المقطع 5 [01:30 - 02:00] - ذروة الجمال**

```text
وصلت الحمراء إلى ذروة سحرها في القرن الرابع عشر، في عهد يوسف الأول وابنه محمد الخامس.

هنا تحولت القلعة العسكرية إلى تحفة لا مثيل لها.

كل شيء هنا له معنى.

ادخل بهو الريحان، ستجد بركة مستطيلة تعكس السماء والقصر، ليشعر الملك أنه يملك السماء.
```


---

### مرجع كود — الأسطر الأصلية 573 وما بعدها

السياق الأصلي: **المقطع 6 [02:00 - 02:40] - بهو الأسود - أهم مقطع - نبرة مبهورة**

```text
ثم ادخل إلى قلب الحمراء النابض... بهو الأسود.

اثنا عشر أسداً من الرخام تحمل حوضاً، تحيط بها مئة وأربعة وعشرون عموداً من الرخام الأبيض.

على حافة الحوض، قصيدة للوزير ابن زمرك منقوشة، تقول إن الماء يتجمد جمالاً.

والسقف فوقك... قبة من المقرنصات، آلاف القطع المتدلية كأنها نجوم تجمدت في السماء.

وفي كل جدار، ستقرأ نفس الجملة تتكرر آلاف المرات: ولا غالب إلا الله.

ليست زخرفة، بل كانت عقيدة ودعاء جيش صغير يحاصره الأعداء من كل جانب.
```


---

### مرجع كود — الأسطر الأصلية 588 وما بعدها

السياق الأصلي: **المقطع 7 [02:40 - 03:10] - السقوط - نبرة حزينة وبطيئة**

```text
وفي الثاني من يناير، عام ألفٍ وأربع مئةٍ واثنين وتسعين، سلم آخر ملوك بني الأحمر، أبو عبد الله الصغير، مفاتيح الحمراء للملكين الكاثوليكيين.

وبكى وهو ينظر إليها للمرة الأخيرة، من تل يسمى اليوم... زفرة العربي الأخيرة.
```


---

### مرجع كود — الأسطر الأصلية 595 وما بعدها

السياق الأصلي: **المقطع 8 [03:10 - 03:30] - الإهمال والخلود**

```text
ظن الجميع أن الحمراء ستموت.

بنى كارلوس الخامس قصره داخلها، وحاول طمس هويتها، ثم أُهملت لقرون وتحولت لأكواخ للفقراء.

لكن الحمراء... رفضت أن تموت.

في القرن التاسع عشر، اكتشفها الرحالة والكتّاب الأوروبيون، فأسرت العالم من جديد.

واليوم، هي أهم معلم إسلامي في إسبانيا، وموقع تراث عالمي لليونسكو، يزورها أكثر من مليوني شخص كل عام.
```


---

### مرجع كود — الأسطر الأصلية 608 وما بعدها

السياق الأصلي: **المقطع 9 [03:30 - 04:00] - الخاتمة - نبرة ملحمية**

```text
قصر الحمراء ليس مجرد حجارة حمراء.

إنه رسالة صمود... تذكير بأن الحضارة لا تُقاس بطول بقائها، بل بعمق أثرها.

وقف ثمانية قرون، شاهداً على حب، وحرب، وشعر، وسقوط.

وما زال يهمس لكل زائر بآيته الخالدة...

ولا غالب إلا الله.
```


---

### مرجع كود — الأسطر الأصلية 624 وما بعدها

السياق الأصلي: ### للكود - ملف `scenes.json` للتزامن التلقائي — انسخ هذا مع ملفات الصوت عشان `main.js` يركبهم تلقائياً بـ FFmpeg:

```json
[
  { "file": "01_intro.wav", "start": 0 },
  { "file": "02_reveal.wav", "start": 12 },
  { "file": "03_birth.wav", "start": 35 },
  { "file": "04_build.wav", "start": 60 },
  { "file": "05_rihan.wav", "start": 90 },
  { "file": "06_lions.wav", "start": 120 },
  { "file": "07_fall.wav", "start": 160 },
  { "file": "08_restore.wav", "start": 190 },
  { "file": "09_finale.wav", "start": 210 }
]
```


---

### مرجع كود — الأسطر الأصلية 653 وما بعدها

السياق الأصلي: ### القاعدة الذهبية لـ ElevenLabs — **الإعدادات الصحيحة قبل اللصق:**

```text
Model: eleven_v3  // مهم جداً
Voice: Adam  أو  Hamid - Arabic deep
Stability: 45-55
Similarity: 75
Style Exaggeration: 35
Apply Text Normalization: OFF
```


---

### مرجع كود — الأسطر الأصلية 666 وما بعدها

السياق الأصلي: ### السكريبت الصحيح - انسخه كما هو في Projects

```text
[deep, calm, cinematic documentary voice, slow pace, reverberant]
هل يمكنُ لحجرٍ أن يحفظَ الشعر؟ <break time="0.9s" /> وهل يمكنُ لجدرانٍ أن تروي تاريخاً عمرهُ ثمانيةُ قرون؟ <break time="1.2s" />

[soft, mysterious whisper]
هنا... <break time="0.6s" /> فوقَ هضبةِ السبيكة، حيثُ تعانقُ السماءُ مدينةَ غرناطة، <break time="0.8s" /> يقفُ قصرٌ لم يُبنَ ليكونَ مجردَ قصر. <break time="1.0s" />

[calm, emotional]
بل كُتِبَ ليكونَ قصيدة. <break time="0.8s" />
إنهُ قصرُ الحمراء... <break time="0.7s" /> آخرُ شاهدٍ حيٍّ على مجدِ الأندلس، وأجملُ ما تركهُ المسلمونَ في أوروبا. <break time="1.5s" />

[serious, historical tone]
في عامِ ألفٍ ومئتينِ وثمانيةٍ وثلاثينَ للميلاد، <break time="0.5s" /> والعالمُ الإسلاميُّ في الأندلسِ ينهار، <break time="0.6s" /> مدنٌ تسقطُ واحدةً تلوَ الأخرى بيدِ قشتالة. <break time="0.9s" />

[dramatic pause, deep]
ظهرَ رجلٌ واحدٌ قررَ أن يصنعَ معجزة. <break time="1.0s" />
إنهُ محمدُ بنُ يوسفَ بنِ نصر، <break time="0.4s" /> الملقبُ بابنِ الأحمر. <break time="1.2s" />

[narrative, flowing]
انسحبَ إلى غرناطة، وهناكَ على أنقاضِ قلعةٍ مهجورة، وضعَ حجرَ الأساسِ لحلمٍ أخير. <break time="0.8s" />
اختارَ هضبةَ السبيكةِ الاستراتيجية، <break time="0.4s" /> وجلبَ الماءَ من نهرِ دارو عبرَ قناةٍ طولُها ستةُ كيلومترات، <break time="0.6s" /> فأحيا الأرضَ القاحلة. <break time="0.9s" />
وسماهُ الحمراء، للونِ طينهِ الأحمرِ الذي كانَ يجفُّ تحتَ شمسِ الأندلس، <break time="0.6s" /> وليكونَ لوناً لسلالتهِ... بنو الأحمر. <break time="1.2s" />

[proud, powerful]
لم يكن يبني حصناً فقط، <break time="0.5s" /> كانَ يبني وطناً أخيراً للمسلمينَ في أوروبا. <break time="1.5s" />

[awe, wonder, slow]
وصلتِ الحمراءُ إلى ذروةِ سحرِها في القرنِ الرابعَ عشر، <break time="0.5s" /> في عهدِ يوسفَ الأولِ وابنهِ محمدٍ الخامس. <break time="0.9s" />
هنا تحولتِ القلعةُ العسكريةُ إلى تحفةٍ لا مثيلَ لها. <break time="1.0s" />
كلُّ شيءٍ هنا لهُ معنى. <break time="0.8s" />

[calm, visual description]
ادخل بهوَ الريحان، ستجدُ بركةً مستطيلةً تعكسُ السماءَ والقصر، <break time="0.6s" /> ليشعرَ الملكُ أنهُ يملكُ السماء. <break time="1.2s" />

[amazed, majestic, slightly louder]
ثم ادخل إلى قلبِ الحمراءِ النابض... <break time="0.7s" /> بهوَ الأسود. <break time="0.8s" />
اثنا عشرَ أسداً من الرخامِ تحملُ حوضاً، <break time="0.5s" /> تحيطُ بها مئةٌ وأربعةٌ وعشرونَ عموداً من الرخامِ الأبيض. <break time="1.0s" />
على حافةِ الحوض، قصيدةٌ للوزيرِ ابنِ زمركَ منقوشة، <break time="0.5s" /> تقولُ إنَّ الماءَ يتجمدُ جمالاً. <break time="0.9s" />
والسقفُ فوقكَ... قبةٌ من المقرنصات، <break time="0.6s" /> آلافُ القطعِ المتدليةِ كأنها نجومٌ تجمدت في السماء. <break time="1.2s" />

[reverent, spiritual whisper]
وفي كلِّ جدار، ستقرأُ نفسَ الجملةِ تتكررُ آلافَ المرات: <break time="0.8s" /> [whispered, echo] ولا غالبَ إلا الله. <break time="1.0s" />
[calm, serious]
ليست زخرفة، بل كانت عقيدةً ودعاءَ جيشٍ صغيرٍ يحاصرهُ الأعداءُ من كلِّ جانب. <break time="1.5s" />

[sad, slow, heavy, low pitch]
وفي الثاني من يناير، عامَ ألفٍ وأربعِ مئةٍ واثنينِ وتسعين، <break time="0.7s" /> سلمَ آخرُ ملوكِ بني الأحمر، أبو عبدِ اللهِ الصغير، <break time="0.6s" /> مفاتيحَ الحمراءِ للملكينِ الكاثوليكيين. <break time="1.0s" />
وبكى وهوَ ينظرُ إليها للمرةِ الأخيرة، <break time="0.6s" /> من تلٍّ يسمى اليوم... <break time="0.8s" /> [sad whisper] زفرةُ العربيِّ الأخيرة. <break time="1.5s" />

[dark, cold tone]
ظنَّ الجميعُ أنَّ الحمراءَ ستموت. <break time="0.8s" />
بنى كارلوسُ الخامسُ قصرهُ داخلها، <break time="0.5s" /> وحاولَ طمسَ هويتها، <break time="0.6s" /> ثم أُهملت لقرونٍ وتحولت لأكواخٍ للفقراء. <break time="1.0s" />

[hopeful, rising tone]
لكنَّ الحمراء... <break time="0.7s" /> رفضت أن تموت. <break time="0.9s" />
في القرنِ التاسعَ عشر، اكتشفها الرحالةُ والكتّابُ الأوروبيون، <break time="0.6s" /> فأسرتِ العالمَ من جديد. <break time="0.9s" />
واليوم، هي أهمُّ معلمٍ إسلاميٍّ في إسبانيا، <break time="0.4s" /> وموقعُ تراثٍ عالميٍّ لليونسكو، <break time="0.5s" /> يزورُها أكثرُ من مليوني شخصٍ كلَّ عام. <break time="1.5s" />

[epic, cinematic, powerful, slow]
قصرُ الحمراءِ ليسَ مجردَ حجارةٍ حمراء. <break time="0.8s" />
إنهُ رسالةُ صمود... <break time="0.7s" /> تذكيرٌ بأنَّ الحضارةَ لا تُقاسُ بطولِ بقائها، <break time="0.6s" /> بل بعمقِ أثرها. <break time="1.0s" />
وقفَ ثمانيةَ قرون، شاهداً على حبٍّ، وحربٍ، وشعرٍ، وسقوط. <break time="1.0s" />
وما زالَ يهمسُ لكلِّ زائرٍ بآيتهِ الخالدة... <break time="1.2s" />

[deep echo, reverent whisper, long pause]
ولا غالبَ إلا الله.
```


---

### مرجع كود — الأسطر الأصلية 738 وما بعدها

السياق الأصلي: ### للكود البرمجي - كيف تولده تلقائياً عبر API

```javascript
// generate-vo.js
import fs from 'fs';

const res = await fetch(`https://api.elevenlabs.io/v1/text-to-speech/ADAM_VOICE_ID`, {
  method: 'POST',
  headers: { 'xi-api-key': 'YOUR_KEY', 'Content-Type': 'application/json' },
  body: JSON.stringify({
    model_id: "eleven_v3",
    text: fs.readFileSync('./vo_script.txt','utf-8'), // السكريبت أعلاه
    voice_settings: { stability: 0.5, similarity_boost: 0.75, style: 0.35, speed: 0.92 }
  })
});
fs.writeFileSync('./assets/audio/VO.wav', Buffer.from(await res.arrayBuffer()));
```


---

### مرجع كود — الأسطر الأصلية 850 وما بعدها

السياق الأصلي: ### 3. هيكل الكود الجديد - 36 كومب

```json
// templates/scenes.json - للكروم + FFmpeg
[
  {"id":"01_stone","start":0, "dur":5, "vo":"هل يمكن لحجر أن يحفظ الشعر؟"},
  {"id":"02_wall","start":5, "dur":7, "vo":"وهل يمكن لجدران أن تروي..."},
  {"id":"03_map_granada","start":12, "dur":8, "vo":"هنا فوق هضبة السبيكة"},
  {"id":"07_year1238","start":35, "dur":8, "vo":"في عام 1238"},
  {"id":"20_lions_12","start":138, "dur":7, "vo":"اثنا عشر أسداً"},
  {"id":"36_finale","start":215, "dur":25, "vo":"ولا غالب إلا الله"}
]
```


---

### مرجع كود — الأسطر الأصلية 863 وما بعدها

السياق الأصلي: **طريقة الرندر البرمجي:**

```bash
# كل مشهد HTML منفصل يرندر 25fps
node main.js --scene 20_lions_12 --fps 25
# FFmpeg يجمع 36 مشهد مع VO المتقطع تلقائياً
ffmpeg -f concat -i scenes_list.txt -i VO.wav -c:v libx264 -crf 16 -pix_fmt yuv420p output/Alhambra_Vox_4K.mp4
```


---

### مرجع كود — الأسطر الأصلية 979 وما بعدها

السياق الأصلي: **هيكل المجلدات للتنفيذ البرمجي:**

```text
D:\Alhambra_Vox\
├── config.js // الباليت والخطوط والتايملاين
├── main.js // Puppeteer يرندر 36 HTML الى PNG
├── templates/ 01_stone.html ... 36_finale.html
├── assets/svg/ // كل الزخارف والأسود والخرائط
└── output/frames/ + Alhambra_Final_4K.mp4
```


---

### مرجع كود — الأسطر الأصلية 1032 وما بعدها

السياق الأصلي: ### 4. بنك الصور الواقعية - 12 صورة أساسية نعيد استخدامها

```bash
# ضعه في D:\Alhambra_Vox\fetch_images.sh
mkdir -p assets/images_real

# صور حرة 100% من Wikimedia Commons + Unsplash - 4K
curl -L "https://upload.wikimedia.org/wikipedia/commons/c/c9/Alhambra_Generalife_Granada.jpg" -o assets/images_real/01_alhambra_aerial_4K.jpg
curl -L "https://upload.wikimedia.org/wikipedia/commons/8/8e/Patio_de_los_Leones.jpg" -o assets/images_real/02_lions_court_real.jpg
curl -L "https://upload.wikimedia.org/wikipedia/commons/d/d7/Patio_de_los_Arrayanes_Alhambra.jpg" -o assets/images_real/03_arrayanes_pool.jpg
curl -L "https://upload.wikimedia.org/wikipedia/commons/a/a5/Alhambra_muqarnas.jpg" -o assets/images_real/04_muqarnas_ceiling.jpg
curl -L "https://upload.wikimedia.org/wikipedia/commons/b/b1/Alhambra_wall_detail.jpg" -o assets/images_real/05_wall_calligraphy.jpg
curl -L "https://images.unsplash.com/photo-1533105079780-92b9be482077?q=80&w=3840" -o assets/images_real/06_granada_city.jpg
curl -L "https://images.unsplash.com/photo-1543785734-4b6e564642f8?q=80&w=3840" -o assets/images_real/07_old_map_texture.jpg
curl -L "https://upload.wikimedia.org/wikipedia/commons/9/9e/Alhambra_key.jpg" -o assets/images_real/08_golden_key.jpg

# معالجة كل الصور برمجيا للباليت الموحد
npx sharp -i "assets/images_real/*.jpg" --resize 3840x2160 --extract  --tint "#C5A880" --grain 8
```


---

### مرجع كود — الأسطر الأصلية 1110 وما بعدها

السياق الأصلي: ### 6. كود تحريك الصورة الواقعية - مثال مشهد 17 بهو الريحان

```html
<!-- templates/17_rihan.html -->
<div class="scene" style="background:#0A1628">
  <!-- الصورة الواقعية 4K مقسمة 3 طبقات بارالاكس -->
  <div class="parallax-wrap">
    <img class="layer" data-speed="0.2" src="../assets/images_real/03_arrayanes_pool.jpg" style="transform:scale(1.15); filter: saturate(1.1) contrast(1.05);">
    <img class="layer reflection" data-speed="0.5" src="../assets/images_real/03_arrayanes_pool.jpg" style="transform:scaleY(-1); opacity:0.4; filter: blur(2px);">
  </div>
  <!-- طبقة فيكتور فوق الصورة -->
  <svg class="overlay"><rect x="100" y="100" width="3640" height="1960" fill="none" stroke="#C5A880" stroke-width="6" /></svg>
  <h2 class="kinetic" style="font:180px 'Reem Kufi'; color:#C5A880; text-shadow:0 0 30px #C5A880">34m × 7m</h2>
</div>
<script src="gsap.min.js"></script>
<script>
window.seekTo = (frame) => {
  let p = frame / (25*10); // 10 ثواني
  // تحريك الصورة الحقيقية Ken Burns + Parallax
  document.querySelectorAll('.layer').forEach(l=>{
    let s = parseFloat(l.dataset.speed);
    gsap.set(l, { x: p*400*s, scale: 1.15 + p*0.1*s });
  });
  gsap.set('.kinetic', { opacity:p, y: (1-p)*50 });
}
</script>
```


---

### مرجع كود — الأسطر الأصلية 1137 وما بعدها

السياق الأصلي: **أمر FFmpeg للحركة السينمائية على الصورة:**

```bash
# Ken Burns على صورة واقعية بدون كروم
ffmpeg -loop 1 -i 03_arrayanes_pool.jpg -vf "zoompan=d=1:s=3840x2160:fps=25, vignette=angle=PI/4" -t 10 -c:v libx264 -crf 16 scene17.mp4
```


---

### مرجع كود — الأسطر الأصلية 1143 وما بعدها

السياق الأصلي: # Ken Burns على صورة واقعية بدون كروم — **هيكل المجلدات النهائي للكود:**

```text
D:\Alhambra_Vox\
├── fetch_images.sh      # يجلب 12 صورة 4K
├── config.js            # الباليت + التايملاين 36 مشهد
├── main.js              # Puppeteer يرندر 36 HTML + صور حقيقية
├── templates/ 01_stone.html ... 36_finale.html
├── assets/images_real/  # 12 صورة واقعية معالجة
└── output/Alhambra_Final_4K.mp4
```


---

### مرجع كود — الأسطر الأصلية 1195 وما بعدها

السياق الأصلي: ### 3. المعالجة الصوتية البرمجية - FFmpeg Ducking — **أولا: حضر ملفين للموشح:**

```bash
mkdir -p assets/audio
# 1. نسخة عود فقط بدون غناء - ابحث في archive.org عن Jadak Ghait oud instrumental - مجانية 100%
# 2. نسخة كورال - منفصلة
# اذا عندك نسخة واحدة فقط افصلها برمجيا:
pip install spleeter
spleeter separate -i jadak_original.mp3 -o assets/audio/ # يطلع لك vocals.wav و accompaniment.wav

# قصها 4 دقائق
ffmpeg -i assets/audio/accompaniment.wav -t 240 -af "aecho=0.8:0.88:60:0.4" assets/audio/jadak_oud_instrumental.wav
ffmpeg -i assets/audio/vocals.wav -ss 45 -t 30 assets/audio/jadak_vocal_choir.wav # خذ اجمل 30 ثانية فقط
```


---

### مرجع كود — الأسطر الأصلية 1210 وما بعدها

السياق الأصلي: # قصها 4 دقائق — **ثانيا: كود الدمج النهائي `main.js`:**

```javascript
// دمج VO + عود + كورال مع Ducking تلقائي
import { execSync } from 'child_process';

execSync(`
ffmpeg -y 
-i assets/audio/VO.wav 
-i assets/audio/jadak_oud_instrumental.wav 
-i assets/audio/jadak_vocal_choir.wav 
-filter_complex "
[1:a]volume=-18dB,apulsator=hz=0.1[oud];
[0:a]asplit[vo1][vo2];
[oud][vo1]sidechaincompress=threshold=0.03:ratio=8:attack=200:release=800:level_in=1[ducked_oud];
[2:a]volume=-3dB,adelay=160000|160000,apad[vocal_delayed];
[ducked_oud][vocal_delayed]amix=inputs=2:duration=longest:weights=1 0.8[music];
[music][vo2]amix=inputs=2:duration=longest:weights=0.7 1:normalize=0[final];
[final]loudnorm=I=-14:TP=-1.5:LRA=11
" 
-c:a aac -b:a 320k -shortest
output/Alhambra_Final_4K.mp4
`);
```


---

### مرجع كود — الأسطر الأصلية 1243 وما بعدها

السياق الأصلي: ### 4. التوظيف البصري - الكلمات تظهر مع الغناء — **مشهد 27 - السقوط 02:40:**

```html
<!-- فوق صورة المفتاح الذهبي الحقيقي 08_golden_key.jpg -->
<h2 class="lyric" style="font-family:'Amiri Quran'; color:#C5A880; font-size:72px; opacity:0">
  جادَكَ الغيثُ إذا الغيثُ هما
</h2>
<h3 style="font-family:'IBM Plex Sans Arabic'; color:#F2E8D5; font-size:32px">
  يا زمانَ الوصلِ بالأندلسِ
</h3>
<script>
// تظهر كلمة كلمة مع الغناء - متزامنة مع adelay
gsap.to('.lyric', { opacity:1, duration:2, stagger:0.4, ease:"power2.out" })
</script>
```


---

### مرجع كود — الأسطر الأصلية 1277 وما بعدها

السياق الأصلي: ### البرومبت الرئيسي - 4 دقائق كاملة - Instrumental — **استخدم هذا في Suno AI:**

```text
Settings: Instrumental = ON  |  Duration = 3:50  |  Model = v4.5
```


---

### مرجع كود — الأسطر الأصلية 1281 وما بعدها

السياق الأصلي: **استخدم هذا في Suno AI:**

```text
Melancholic Andalusian muwashah instrumental inspired by Jadaka Al-Ghayth, slow 70 BPM, D minor, cinematic documentary background music. Starts with solo expressive oud with spacious palace reverb and silence. Then soft qanun arpeggios and breathy nay flute enter subtly. Very soft riq frame drum, barely audible, warm low string pad in background. Nostalgic, intimate, ancient, Granada Alhambra atmosphere. No vocals, no choir, no singing, no heavy percussion, no drums, no electronic beats, no piano. Minimalist, loopable, low dynamic range, perfect for voice-over ducking, not overpowering, high quality 48kHz
```


---

### مرجع كود — الأسطر الأصلية 1286 وما بعدها

السياق الأصلي: **Style Tags انسخها معه:**

```text
andalusian, oud, qanun, nay, cinematic documentary, nostalgic, reverberant, 70bpm, D minor, acoustic, palace hall reverb
```


---

### مرجع كود — الأسطر الأصلية 1291 وما بعدها

السياق الأصلي: **Style Tags انسخها معه:** — **Negative Prompt لو الموقع يدعمها:**

```text
vocals, choir, singing, lyrics, heavy drums, trap, hip hop, electronic, piano, upbeat, happy
```


---

### مرجع كود — الأسطر الأصلية 1302 وما بعدها

السياق الأصلي: ### برومبت مقسم حسب الفصول - لو تبي تولد كل فصل لحاله وتركبهم بالكود — **1- للفصل 1 و 2 [00:00 - 01:30] الخطاف والولادة - عود منفرد فقط:**

```text
Solo oud instrumental, very sparse and intimate, single oud playing Jadaka Al-Ghayth motif slowly, 65 BPM, large empty hall reverb, silence between notes, melancholic Andalusian, no other instruments, no percussion, no vocals, documentary intro background
```


---

### مرجع كود — الأسطر الأصلية 1307 وما بعدها

السياق الأصلي: **1- للفصل 1 و 2 [00:00 - 01:30] الخطاف والولادة - عود منفرد فقط:** — **2- للفصل 3 [01:30 - 02:40] الذروة بهو الأسود - دخول القانون والناي:**

```text
Same Andalusian oud motif now with soft qanun tremolo and breathy nay flute joining, 70 BPM, warm string pad very low, subtle riq, richer but still calm and elegant, no vocals, no drums, cinematic
```


---

### مرجع كود — الأسطر الأصلية 1312 وما بعدها

السياق الأصلي: **2- للفصل 3 [01:30 - 02:40] الذروة بهو الأسود - دخول القانون والناي:** — **3- للفصل 4 [02:40 - 04:00] السقوط والخاتمة - ذروة عاطفية بدون غناء:**

```text
Emotional Andalusian cinematic swell, oud + qanun + nay + full warm strings, slow and epic, 70 BPM, D minor, building slightly but still soft, nostalgic and tearful, no vocals, no choir, no percussion climax, perfect for final scene fade out
```


---

### مرجع كود — الأسطر الأصلية 1327 وما بعدها

السياق الأصلي: ### بطاقة المشهد 01

```text
SHOT 01 / 36 | TC: 00:00 - 00:05 | DUR: 5 ثواني - 125 فريم 25fps
النص المنطوق: "هل يمكن لحجر أن يحفظ الشعر؟"
الموسيقى: Solo Oud 68 BPM D minor -18dB | SFX: Whoosh + Golden Dust
الانتقال للبعد: MorphSVG الحجر -> كلمة حجر
```


---

### مرجع كود — الأسطر الأصلية 1368 وما بعدها

السياق الأصلي: **منحنى الحركة:** كل حركة `GSAP power2.out` مدة 1.2 ثانية، لا يوجد حركة خطية ابدا. — ### 4. الكود البرمجي الجاهز `templates/01_stone.html`

```html
<div class="scene" id="scene-01" style="width:3840px;height:2160px;background:linear-gradient(135deg,#0A1628,#0F4C5C);overflow:hidden;position:relative">
  <!-- L1 الصورة الواقعية -->
  <img id="wall" src="../assets/images_real/05_wall_calligraphy.jpg" style="position:absolute;inset:0;width:100%;height:100%;object-fit:cover;transform:scale(1.15);filter:contrast(1.05) saturate(1.1)">
  <div style="position:absolute;inset:0;background:linear-gradient(135deg,#0A1628 0%, transparent 45%, rgba(197,168,128,0.15) 100%);mix-blend-mode:multiply"></div>
  <div style="position:absolute;inset:0;background:radial-gradient(ellipse at center, transparent 60%, #0A1628 100%);opacity:0.25"></div>
  <!-- L2 غبار -->
  <canvas id="dust" width="3840" height="2160" style="position:absolute;inset:0;opacity:0.6"></canvas>
  <!-- L3 نص ومكعبات -->
  <h1 id="word-hajar" style="position:absolute;left:50%;top:50%;transform:translate(-50%,-50%);font:700 140px 'Reem Kufi';color:#C5A880;text-shadow:0 0 30px #C5A880,0 0 60px #C5A880;letter-spacing:8px">حجر</h1>
  <svg id="cubes" style="position:absolute;inset:0;opacity:0"></svg>
</div>
<script src="gsap.min.js"></script>
<script src="MorphSVGPlugin.min.js"></script>
<script>
window.seekTo = (frame) => {
  let p = frame / 125;
  // L1 Ken Burns
  gsap.set("#wall", { scale: 1.15 + p*0.10, x: p*80 });
  // L3 Morph في الفريم 55
  if(frame > 55) gsap.set("#word-hajar", { opacity: 1 - (p-0.44)*2 });
  if(frame === 55) gsap.to("#word-hajar", { duration:1.2, morphSVG:"#cubes", ease:"power2.out", stagger:0.04 });
}
// غبار ذهبي 120 نقطة
const c=document.getElementById('dust'),x=c.getContext('2d');let pts=[...Array(120)].map(_=>({x:Math.random()*3840,y:Math.random()*2160,r:Math.random()*2+1,s:Math.random()*0.6+0.2}));
(function loop(){x.clearRect(0,0,3840,2160);pts.forEach(p=>{p.y-=p.s;x.fillStyle="#C5A880";x.globalAlpha=0.5;x.beginPath();x.arc(p.x,p.y,p.r,0,Math.PI*2);x.fill();if(p.y<0)p.y=2160});requestAnimationFrame(loop)})();
</script>
```


---

### مرجع كود — الأسطر الأصلية 1398 وما بعدها

السياق الأصلي: **أمر FFmpeg للمعالجة:**

```bash
ffmpeg -loop 1 -i 05_wall_calligraphy.jpg -vf "scale=3840:2160,zoompan=d=1:s=3840x2160:fps=25,eq=contrast=1.05:saturation=1.1,vignette=angle=PI/4" -t 5 -c:v libx264 -crf 16 scene01.mp4
```


---

### مرجع كود — الأسطر الأصلية 1406 وما بعدها

السياق الأصلي: ### 5. برومبت توليد صورة الستوري بورد الموشن للمشهد 01 — انسخ هذا البرومبت في `Ideogram 2.0` ليطلع لك لوحة تشرح الطبقات والتحريك للعميل:

```text
Professional motion graphics storyboard breakdown sheet for single shot, vertical layout 9:16, white studio template with thin grid lines

Top header: SHOT 01 /36 | TC 00:00-00:05 | DUR 5s | ECU | ALHAMBRA

Main center: Large 16:9 cinematic frame showing photorealistic extreme close-up of Alhambra red clay wall with golden Arabic calligraphy "ولا غالب إلا الله" engraved, shallow depth of field, cinematic volumetric light, duotone grade #0A1628 shadows and #C5A880 gold highlights 35%, grain 8%, vignette, rounded corners 24px

Below main frame, 4 horizontal strips showing MOTION BREAKDOWN:
Strip 1 - LAYERS STACK: Diagram showing 5 layers stacked vertically with labels L0 Gradient #0A1628, L1 Real Photo 05_wall_calligraphy.jpg, L2 Golden Dust Particles 120pts, L3 Kinetic Text حجر Reem Kufi 140px, L4 Glow Saber, with arrows indicating parallax speeds 0.2/0.5/0.8
Strip 2 - TIMELINE 0-5s: Horizontal timeline bar with 5 keyframes at 00:00 00:01.5 00:02.2 00:03.0 00:05.0 showing morph from word to isometric stone cubes, Ken Burns push-in 1.15->1.25
Strip 3 - CAMERA & EASING: Camera icon with Slow Push-In arrow, text power2.out 1.2s, Stagger 0.04s, MorphSVG
Strip 4 - AUDIO: VO text "هل يمكن لحجر أن يحفظ الشعر؟" in IBM Plex Sans Arabic, Music Solo Oud -18dB, SFX Whoosh + Dust

Right sidebar metadata: VO, AUDIO, SFX, TRANSITION Whip Pan icons
Footer: Palette swatches #8B1E1E #C5A880 #0F4C5C #F2E8D5 #0A1628, Fonts legend

Style: Ultra clean minimalist Vox x Kurzgesagt documentary style, 8K, print-ready, correct Arabic calligraphy --ar 9:16
```


---

### مرجع كود — الأسطر الأصلية 1426 وما بعدها

السياق الأصلي: **Negative Prompt:**

```text
cartoon, anime, blurry, distorted Arabic, low quality, 3d plastic, watermark, hand sketch
```


---

### مرجع كود — الأسطر الأصلية 1442 وما بعدها

السياق الأصلي: ### بطاقة المشهد 02

```text
SHOT 02 / 36 | TC: 00:05 - 00:12 | DUR: 7 ثواني - 175 فريم 25fps
النص المنطوق: "وهل يمكن لجدرانٍ أن تروي تاريخاً عمره ثمانية قرون؟"
الموسيقى: Solo Oud مستمر -20dB DUCKED | SFX: Tick Clock + Counter Roll
الانتقال للبعد: Whip Pan + Counter Morph -> خريطة غرناطة
```


---

### مرجع كود — الأسطر الأصلية 1484 وما بعدها

السياق الأصلي: **منحنى الحركة:** كل شيء `GSAP power3.out` للعداد و `power2.out` للخط. العداد يستخدم `ease: none` مع `snap` كل 10 أرقام لإحساس Vox. — ### 4. الكود البرمجي `templates/02_timeline.html`

```html
<div class="scene" id="scene-02" style="width:3840px;height:2160px;background:linear-gradient(135deg,#0A1628,#0F4C5C);overflow:hidden;position:relative">
  <!-- L1 الصورة الضبابية -->
  <img id="wall-blur" src="../assets/images_real/05_wall_calligraphy.jpg" style="position:absolute;inset:0;width:100%;height:100%;object-fit:cover;filter:blur(20px) brightness(0.45) saturate(0.8);transform:scale(1.1)">
  <div style="position:absolute;inset:0;background:radial-gradient(ellipse at center, transparent 30%, #0A1628 100%);opacity:0.6"></div>
  <!-- L2 خط الزمن -->
  <svg id="timeline" width="3840" height="2160" style="position:absolute;inset:0">
    <line id="line" x1="520" y1="1080" x2="3320" y2="1080" stroke="#C5A880" stroke-width="6" stroke-linecap="round" opacity="0.9"/>
    <circle cx="520" cy="1080" r="18" fill="#C5A880" id="dot-start"/><circle cx="3320" cy="1080" r="18" fill="#C5A880" id="dot-end"/>
    <text x="520" y="1150" fill="#C5A880" font-family="IBM Plex Sans Arabic" font-size="42" text-anchor="middle">1238</text>
    <text x="3320" y="1150" fill="#C5A880" font-family="IBM Plex Sans Arabic" font-size="42" text-anchor="middle">2026</text>
  </svg>
  <!-- L3 العداد -->
  <div id="counter-wrap" style="position:absolute;left:50%;top:42%;transform:translate(-50%,-50%);text-align:center">
    <div id="counter" style="font:700 220px 'Anton';color:#F2E8D5;letter-spacing:8px;line-height:1">0</div>
    <div id="label" style="font:300 56px 'Reem Kufi';color:#C5A880;letter-spacing:12px;opacity:0">عام</div>
    <div id="sub" style="font:300 32px 'IBM Plex Sans Arabic';color:#F2E8D5;opacity:0.7;margin-top:20px">ثمانية قرون من الحكاية</div>
  </div>
</div>
<script src="gsap.min.js"></script>
<script>
let counterEl = document.getElementById('counter');
window.seekTo = (frame) => {
  let p = frame / 175;
  // L1 Parallax بطيء
  gsap.set("#wall-blur", { scale: 1.10 + p*0.05, x: p*40 });
  // L2 Trim Path من الفريم 25 الى 100
  if(frame >= 25) {
    let tp = Math.min(1, (frame-25)/75);
    gsap.set("#line", { drawSVG: `0% ${tp*100}%` }); // يحتاج DrawSVGPlugin
    gsap.set("#line", { attr: { x2: 520 + tp*2800 } });
  }
  // L3 Counter من الفريم 50 الى 112
  if(frame >= 50 && frame <= 112) {
    let cp = (frame-50)/62;
    let val = Math.floor(cp*800);
    counterEl.textContent = val;
    gsap.set("#counter", { scale: 1 + Math.sin(cp*20)*0.02 });
  }
  if(frame > 112) { counterEl.textContent = "800"; gsap.set("#label",{opacity:1, y:0}); gsap.set("#sub",{opacity:0.7}); }
}
// Fallback بدون DrawSVGPlugin
gsap.set("#line",{strokeDasharray:2800, strokeDashoffset:2800});
</script>
```


---

### مرجع كود — الأسطر الأصلية 1533 وما بعدها

السياق الأصلي: **أمر FFmpeg للمعالجة:**

```bash
ffmpeg -loop 1 -i 05_wall_calligraphy.jpg -vf "gblur=sigma=20,eq=brightness=0.45:saturation=0.8,scale=3840:2160,vignette=angle=PI/4" -t 7 -c:v libx264 -crf 16 scene02_bg.mp4
```


---

### مرجع كود — الأسطر الأصلية 1541 وما بعدها

السياق الأصلي: ### 5. برومبت توليد لوحة الستوري بورد للمشهد 02 — انسخه في `Ideogram 2.0` بنفس الـ Seed 42:

```text
Professional motion graphics storyboard breakdown sheet for single shot, vertical layout 9:16, white studio template with thin grid lines

Top header: SHOT 02 /36 | TC 00:05-00:12 | DUR 7s | SCALE: WS Wide Shot | ALHAMBRA

Main center: Large 16:9 cinematic frame showing photorealistic blurred Alhambra red wall background brightness 0.45 blur 20px, sharp foreground Vox vector timeline: horizontal thin gold line #C5A880 from 1238 to 2026 with glowing dots at both ends, large Anton font number "800" centered above line in ivory #F2E8D5 with gold label "عام" below, subtle grain 8% vignette 25%, rounded corners 24px

Below main frame, 4 horizontal strips showing MOTION BREAKDOWN:
Strip 1 - LAYERS STACK: Diagram 6 layers L0 Gradient #0A1628, L1 Real Photo Blur 20px speed 0.2, L2 Timeline Vector Trim Path, L3 Counter Anton 220px, L4 Labels, L5 Vignette Grain
Strip 2 - TIMELINE 0-7s: Horizontal bar with 5 keyframes at 00:05 Blur 00:06 Trim Path 00:07 Counter 0->800 00:08.5 Settle 800 عام 00:12 Whip Pan
Strip 3 - CAMERA & EASING: Text "Zoom Out 1.25->1.10 + Parallax 0.2 | power3.out | Counter snap 10 | Trim Path 3s"
Strip 4 - AUDIO: VO text "وهل يمكن لجدران أن تروي تاريخاً عمره ثمانية قرون؟" IBM Plex Sans Arabic, Music Solo Oud -20dB ducked, SFX Tick + Counter Roll + Whoosh

Right sidebar metadata icons: VO, AUDIO, SFX, TRANSITION Whip Pan
Footer: Palette swatches #8B1E1E #C5A880 #0F4C5C #F2E8D5 #0A1628, Fonts Anton/Reem Kufi/IBM Plex

Style: Ultra clean minimalist Vox x Kurzgesagt documentary, 8K, print-ready, correct Arabic --ar 9:16
```


---

### مرجع كود — الأسطر الأصلية 1561 وما بعدها

السياق الأصلي: **Negative Prompt:**

```text
cartoon, anime, blurry text, wrong Arabic, low quality, 3d plastic, watermark, hand sketch
```


---

### مرجع كود — الأسطر الأصلية 1574 وما بعدها

السياق الأصلي: ### بطاقة المشهد 03

```text
SHOT 03 / 36 | TC: 00:12 - 00:20 | DUR: 8 ثواني - 200 فريم 25fps
النص المنطوق: "هنا... فوق هضبة السبيكة"
الموسيقى: Solo Oud مستمر -20dB DUCKED | SFX: Satellite Zoom + Pulse
الانتقال للبعد: Map Zoom 800% -> المشهد 04 القصر Isometric
```


---

### مرجع كود — الأسطر الأصلية 1613 وما بعدها

السياق الأصلي: **منحنى الحركة:** الزوم `GSAP power2.inOut` مدة 3.5 ثانية، النبض `sine.inOut` كل 1.2 ثانية. — ### 4. الكود البرمجي `templates/03_map.html`

```html
<div class="scene" id="scene-03" style="width:3840px;height:2160px;background:#0A1628;overflow:hidden;position:relative">
  <!-- L1 صور حقيقية -->
  <img id="sat" src="../assets/images_real/satellite_spain.jpg" style="position:absolute;inset:0;width:100%;height:100%;object-fit:cover;transform:scale(0.15);filter:contrast(1.1) saturate(0.9)">
  <img id="aerial" src="../assets/images_real/01_alhambra_aerial_4K.jpg" style="position:absolute;inset:0;width:100%;height:100%;object-fit:cover;opacity:0;transform:scale(1.4)">
  <div style="position:absolute;inset:0;background:radial-gradient(ellipse at center, transparent 50%, #0A1628 100%);opacity:0.5"></div>
  <!-- L2 خريطة فيكتور -->
  <svg width="3840" height="2160" style="position:absolute;inset:0" viewBox="0 0 3840 2160">
    <path id="spain-path" d="M 1200 800 L 2800 750 L 3000 1400 L 1400 1500 Z" fill="rgba(197,168,128,0.08)" stroke="#C5A880" stroke-width="6" stroke-linejoin="round" opacity="0.9"/>
  </svg>
  <!-- L3 نقطة نابضة -->
  <div id="pulse-wrap" style="position:absolute;left:58%;top:62%;transform:translate(-50%,-50%)">
    <div id="pulse-ring" style="position:absolute;left:50%;top:50%;width:80px;height:80px;border:3px solid #C5A880;border-radius:50%;transform:translate(-50%,-50%);opacity:0.6"></div>
    <div id="dot" style="width:26px;height:26px;background:#8B1E1E;border:4px solid #C5A880;border-radius:50%;box-shadow:0 0 20px #C5A880"></div>
    <div id="label" style="position:absolute;left:50%;top:50px;transform:translateX(-50%);background:rgba(10,22,40,0.85);border:1px solid #C5A880;border-radius:12px;padding:10px 22px;white-space:nowrap;opacity:0">
      <span style="font:700 36px 'Reem Kufi';color:#C5A880">هضبة السبيكة</span>
      <span style="font:300 28px 'IBM Plex Sans Arabic';color:#F2E8D5;margin-right:12px">740m</span>
    </div>
  </div>
</div>
<script src="gsap.min.js"></script>
<script>
window.seekTo = (frame) => {
  let p = frame / 200;
  // L1 Zoom من الفضاء
  if(frame >= 37) {
    let zp = Math.min(1, (frame-37)/88);
    let eased = gsap.parseEase("power2.inOut")(zp);
    gsap.set("#sat", { scale: 0.15 + eased*1.25, x: eased*180, y: eased*90 });
    gsap.set("#aerial", { opacity: zp > 0.6 ? (zp-0.6)*2.5 : 0, scale: 1.4 - zp*0.15 });
    gsap.set("#spain-path", { opacity: 1 - zp*1.4 });
  }
  // L3 Pulse
  if(frame > 125) {
    let pulse = (Math.sin(frame*0.18)+1)/2;
    gsap.set("#pulse-ring", { scale: 1 + pulse*0.8, opacity: 0.6 - pulse*0.4 });
    gsap.set("#label", { opacity: 1, y: 0 });
  }
}
</script>
```


---

### مرجع كود — الأسطر الأصلية 1656 وما بعدها

السياق الأصلي: **أمر FFmpeg للخلفية:**

```bash
ffmpeg -loop 1 -i satellite_spain.jpg -vf "scale=3840:2160,zoompan=d=1:s=3840x2160:fps=25:z='if(lte(p,75),0.15+0.012*p,1.4)'" -t 8 -c:v libx264 -crf 16 scene03_bg.mp4
```


---

### مرجع كود — الأسطر الأصلية 1664 وما بعدها

السياق الأصلي: ### 5. برومبت توليد لوحة الستوري بورد للمشهد 03 — انسخه في `Ideogram 2.0` بنفس الـ Seed 42:

```text
Professional motion graphics storyboard breakdown sheet for single shot, vertical layout 9:16, white studio template with thin grid lines

Top header: SHOT 03 /36 | TC 00:12-00:20 | DUR 8s | SCALE: ELS Extreme Long Shot Satellite | ALHAMBRA

Main center: Large 16:9 cinematic frame showing satellite photorealistic view of southern Spain, Vox vector gold outline of Spain #C5A880 over satellite image, glowing pulsing red dot #8B1E1E with expanding gold ring over Granada, label "هضبة السبيكة 740m" in Reem Kufi gold on dark badge, photorealistic aerial inset of Alhambra palace on hill blending in, cinematic duotone, grain 8%, vignette, rounded corners 24px

Below main frame, 4 horizontal strips showing MOTION BREAKDOWN:
Strip 1 - LAYERS STACK: Diagram 6 layers L0 #0A1628, L1 Satellite Photo + Aerial 01_alhambra_aerial_4K.jpg scale 0.15->1.4 speed 0.8, L2 Spain Vector Trim Path, L3 Pulsing Dot #8B1E1E + Ring #C5A880 1.2s loop, L4 Labels Granada, L5 Vignette Grain
Strip 2 - TIMELINE 0-8s: Horizontal bar with keyframes at 00:12 800->Dot 00:13 Map Fade In 00:13.5 Zoom 800% start 00:15.5 Dissolve to Aerial 00:18.5 Label Pop 740m
Strip 3 - CAMERA & EASING: Text "Space to Place Zoom 800% | power2.inOut 3.5s | Pulse sine.inOut 1.2s infinite | Parallax 0.8"
Strip 4 - AUDIO: VO text "هنا... فوق هضبة السبيكة" IBM Plex Sans Arabic, Music Oud -20dB, SFX Satellite Zoom + Pulse Tick + Whoosh

Right sidebar metadata icons: VO, AUDIO, SFX, TRANSITION Dolly Forward
Footer: Palette swatches #8B1E1E #C5A880 #0F4C5C #F2E8D5 #0A1628, Fonts legend, Seed 42

Style: Ultra clean minimalist Vox x Kurzgesagt documentary, 8K, print-ready, correct Arabic calligraphy --ar 9:16
```


---

### مرجع كود — الأسطر الأصلية 1684 وما بعدها

السياق الأصلي: **Negative Prompt:**

```text
cartoon, anime, blurry, wrong Arabic, distorted map, low quality, 3d plastic, watermark, hand sketch
```


---

### مرجع كود — الأسطر الأصلية 1697 وما بعدها

السياق الأصلي: ### بطاقة المشهد 04

```text
SHOT 04 / 36 | TC: 00:20 - 00:27 | DUR: 7 ثواني - 175 فريم 25fps
النص المنطوق: "حيث تعانق السماء مدينة غرناطة"
الموسيقى: Solo Oud مستمر -20dB DUCKED | SFX: Wind Granada + Soft Drone
الانتقال للبعد: Dolly Orbit 15deg -> المشهد 05 Morph قصر -> قلم
```


---

### مرجع كود — الأسطر الأصلية 1738 وما بعدها

السياق الأصلي: **منحنى الحركة:** كل الطبقات `GSAP power2.out` مدة 5 ثواني، الـ Orbit يستخدم `power1.inOut`. — ### 4. الكود البرمجي `templates/04_palace_isometric.html`

```html
<div class="scene" id="scene-04" style="width:3840px;height:2160px;background:linear-gradient(180deg,#0A1628 0%, #0F4C5C 100%);overflow:hidden;position:relative;perspective:1200px">
  <!-- L0 Sky -->
  <img id="l0-sky" src="../assets/images_real/06_granada_sky.png" style="position:absolute;inset:0;width:110%;height:110%;object-fit:cover;left:-5%;top:-5%">
  <!-- L1 Mountains -->
  <img id="l1-mount" src="../assets/images_real/06_granada_mountains.png" style="position:absolute;bottom:0;width:110%;height:65%;object-fit:cover;left:-5%">
  <!-- L2 City -->
  <img id="l2-city" src="../assets/images_real/06_granada_city_cut.png" style="position:absolute;bottom:0;width:110%;height:40%;object-fit:cover;left:-5%">
  <!-- L3 Palace Real - المقصوص -->
  <img id="l3-palace" src="../assets/images_real/01_alhambra_cutout.png" style="position:absolute;left:38%;top:32%;width:42%;filter:drop-shadow(0 25px 50px rgba(0,0,0,0.6)) contrast(1.05) saturate(1.1)">
  <!-- L4 Isometric Outline -->
  <svg id="l4-outline" width="3840" height="2160" style="position:absolute;inset:0">
    <path d="M 1450 850 L 2100 700 L 2650 950 L 2000 1150 Z" fill="none" stroke="#C5A880" stroke-width="5" stroke-linejoin="round" opacity="0"/>
  </svg>
  <!-- L5 Label -->
  <div id="label-04" style="position:absolute;left:62%;top:28%;background:rgba(10,22,40,0.85);border:1px solid #C5A880;border-radius:14px;padding:14px 24px;opacity:0;transform:translateY(20px)">
    <div style="font:700 40px 'Reem Kufi';color:#C5A880">قصر الحمراء</div>
    <div style="font:300 28px 'IBM Plex Sans Arabic';color:#F2E8D5">هضبة السبيكة 740m</div>
  </div>
  <div style="position:absolute;inset:0;background:radial-gradient(ellipse at center, transparent 55%, #0A1628 100%);opacity:0.35;pointer-events:none"></div>
</div>
<script src="gsap.min.js"></script>
<script>
window.seekTo = (frame) => {
  let p = frame / 175;
  // Parallax كل طبقة بسرعة مختلفة
  gsap.set("#l0-sky", { x: p*40*0.15, scale: 1 + p*0.02 });
  gsap.set("#l1-mount", { x: p*80*0.3, scale: 1 + p*0.03 });
  gsap.set("#l2-city", { x: p*120*0.5, y: p*15 });
  gsap.set("#l3-palace", { x: p*180*0.8, y: -p*25, scale: 1 + p*0.08, rotationY: p*15 });
  // L4 Trim Path
  if(frame > 37) {
    let tp = Math.min(1, (frame-37)/45);
    gsap.set("#l4-outline path", { opacity: tp, strokeDasharray: 2400, strokeDashoffset: 2400 - tp*2400 });
  }
  if(frame > 75) gsap.set("#label-04", { opacity:1, y:0, duration:0.6, ease:"power2.out" });
}
</script>
```


---

### مرجع كود — الأسطر الأصلية 1779 وما بعدها

السياق الأصلي: **أوامر قص الصور البرمجي قبل الرندر:**

```bash
# قص الصورة الجوية 5 طبقات تلقائيا
npx sharp -i 01_alhambra_aerial_4K.jpg --extract sky --output 06_granada_sky.png
npx sharp -i 01_alhambra_aerial_4K.jpg --extract palace --matte --feather 2 --output 01_alhambra_cutout.png
```


---

### مرجع كود — الأسطر الأصلية 1789 وما بعدها

السياق الأصلي: ### 5. برومبت توليد لوحة الستوري بورد للمشهد 04 — انسخه في `Ideogram 2.0` بنفس الـ Seed 42:

```text
Professional motion graphics storyboard breakdown sheet for single shot, vertical layout 9:16, white studio template with thin grid lines

Top header: SHOT 04 /36 | TC 00:20-00:27 | DUR 7s | SCALE: ELS Isometric 45deg | ALHAMBRA

Main center: Large 16:9 cinematic frame showing photorealistic Alhambra palace on hill Isometric 45 degree view, 5 distinct parallax layers visible: sky with clouds, distant mountains, Granada city below, palace cutout sharp in center with drop shadow, thin gold isometric vector outline #C5A880 around palace walls, label badge "قصر الحمراء 740m" in Reem Kufi gold, cinematic duotone shadows #0A1628 highlights #C5A880 35%, grain 8%, vignette, rounded corners 24px

Below main frame, 4 horizontal strips showing MOTION BREAKDOWN:
Strip 1 - LAYERS STACK: Diagram 7 layers L0 Sky speed 0.15, L1 Mountains 0.3, L2 City 0.5, L3 Palace Real Cutout 0.8 scale 1->1.08, L4 Isometric Outline Trim Path gold 4px, L5 Label Pop, L6 Vignette Grain 8%
Strip 2 - TIMELINE 0-7s: Horizontal bar with keyframes at 00:20 Dolly Orbit 15deg start 00:21.5 Outline Trim 00:23 Label Pop 740m 00:26 Settle
Strip 3 - CAMERA & EASING: Text "Dolly Forward + Orbit 15deg + 5-Layer Parallax 0.15/0.3/0.5/0.8 | power2.out 5s | Glow Pulse"
Strip 4 - AUDIO: VO text "حيث تعانق السماء مدينة غرناطة" IBM Plex Sans Arabic, Music Oud -20dB, SFX Wind Granada + Drone + Whoosh

Right sidebar metadata icons: VO, AUDIO, SFX, TRANSITION Morph to Pen Icon
Footer: Palette swatches #8B1E1E #C5A880 #0F4C5C #F2E8D5 #0A1628, Fonts Reem Kufi/IBMPlex, Seed 42

Style: Ultra clean minimalist Vox x Kurzgesagt documentary, 8K, print-ready, correct Arabic calligraphy --ar 9:16
```


---

### مرجع كود — الأسطر الأصلية 1809 وما بعدها

السياق الأصلي: **Negative Prompt:**

```text
cartoon, anime, blurry, wrong Arabic, distorted palace, low quality, 3d plastic, watermark, hand sketch, extra buildings
```


---

### مرجع كود — الأسطر الأصلية 1823 وما بعدها

السياق الأصلي: ### بطاقة المشهد 05

```text
SHOT 05 / 36 | TC: 00:27 - 00:32 | DUR: 5 ثواني - 125 فريم 25fps
النص المنطوق: "قصر لم يُبنَ ليكون مجرد قصر... بل كُتب ليكون قصيدة"
الموسيقى: Solo Oud مستمر -20dB DUCKED مع نغمة واحدة طويلة | SFX: Soft Morph Whoosh + Ink Stroke
الانتقال للبعد: MorphSVG قصر -> قلم ريشة -> المشهد 06 Title Card
```


---

### مرجع كود — الأسطر الأصلية 1863 وما بعدها

السياق الأصلي: **منحنى الحركة:** المورف `power2.inOut 1.4s`، النص `power3.out` مع `stagger 0.05s`، لا يوجد حركة خطية. — ### 4. الكود البرمجي `templates/05_morph.html`

```html
<div class="scene" id="scene-05" style="width:3840px;height:2160px;background:linear-gradient(135deg,#0A1628,#0F4C5C);overflow:hidden;position:relative">
  <!-- L1 صورة ضبابية -->
  <img id="bg-blur" src="../assets/images_real/01_alhambra_aerial_4K.jpg" style="position:absolute;inset:0;width:100%;height:100%;object-fit:cover;filter:blur(25px) brightness(0.35) saturate(0.7);transform:scale(1.08)">
  <div style="position:absolute;inset:0;background:radial-gradient(ellipse at center, transparent 35%, #0A1628 85%);opacity:0.7"></div>
  <!-- L2 أيقونة المورف -->
  <svg width="3840" height="2160" style="position:absolute;inset:0" viewBox="0 0 3840 2160">
    <g id="icon-wrap" transform="translate(1920, 900)">
      <path id="palace-icon" d="M -180 80 L -180 -60 L -100 -120 L 0 -160 L 100 -120 L 180 -60 L 180 80 Z M -120 80 L -120 0 L -60 0 L -60 80 M 60 80 L 60 0 L 120 0 L 120 80 M -40 80 L -40 -40 L 40 -40 L 40 80" fill="none" stroke="#C5A880" stroke-width="10" stroke-linejoin="round" stroke-linecap="round"/>
      <path id="pen-icon" d="M -140 100 C -80 40 20 -80 140 -140 L 160 -120 L 40 80 C 0 120 -40 140 -140 100 Z M 100 -100 L 130 -70 L 40 20 L 10 -10 Z" fill="none" stroke="#C5A880" stroke-width="10" stroke-linejoin="round" stroke-linecap="round" opacity="0"/>
    </g>
  </svg>
  <!-- L3 نص كينيتك -->
  <div id="text-wrap" style="position:absolute;left:50%;top:62%;transform:translateX(-50%);text-align:center">
    <div id="word-qasida" style="font:700 160px 'Reem Kufi';color:#C5A880;text-shadow:0 0 30px #C5A880, 0 0 60px rgba(197,168,128,0.5);opacity:0;letter-spacing:12px">قصيدة</div>
    <div id="sub" style="font:300 42px 'IBM Plex Sans Arabic';color:#F2E8D5;opacity:0;margin-top:18px;letter-spacing:2px">لم يُبنَ... بل كُتب</div>
  </div>
  <canvas id="ink" width="3840" height="2160" style="position:absolute;inset:0;pointer-events:none"></canvas>
</div>
<script src="gsap.min.js"></script>
<script src="MorphSVGPlugin.min.js"></script>
<script>
window.seekTo = (frame) => {
  let p = frame / 125;
  gsap.set("#bg-blur", { scale: 1.08 + p*0.04 });
  // ظهور الأيقونة
  if(frame >= 20) gsap.set("#icon-wrap", { opacity:1, scale: 0.8 + Math.min(1,(frame-20)/15)*0.2 });
  // المورف في الفريم 37
  if(frame === 37) {
    gsap.to("#palace-icon", { duration:1.4, morphSVG:"#pen-icon", ease:"power2.inOut" });
    gsap.to("#pen-icon", { duration:0.2, opacity:1 });
  }
  // كتابة قصيدة في الفريم 62
  if(frame >= 62) {
    let tp = Math.min(1, (frame-62)/30);
    gsap.set("#word-qasida", { opacity: tp, scale: 0.9 + tp*0.18, y: (1-tp)*30 });
  }
  if(frame >= 100) gsap.set("#sub", { opacity: 0.9, y: 0 });
}
// حبر متطاير بعد المورف
const c=document.getElementById('ink'),x=c.getContext('2d');let pts=[];
function emit(){for(let i=0;i<40;i++)pts.push({x:1920,y:900,vx:(Math.random()-0.5)*12,vy:(Math.random()-0.5)*12-2,r:Math.random()*3+1,a:1})}
let emitted=false;
(function loop(){x.clearRect(0,0,3840,2160);pts.forEach(p=>{p.x+=p.vx;p.y+=p.vy;p.vy+=0.3;p.a-=0.015;x.globalAlpha=Math.max(0,p.a);x.fillStyle="#C5A880";x.beginPath();x.arc(p.x,p.y,p.r,0,Math.PI*2);x.fill()});pts=pts.filter(p=>p.a>0);requestAnimationFrame(loop)})();
// استدعاء emit عند الفريم 80
setTimeout(()=>{if(!emitted){emit();emitted=true}}, 80/25*1000);
</script>
```


---

### مرجع كود — الأسطر الأصلية 1915 وما بعدها

السياق الأصلي: **أمر FFmpeg للخلفية الضبابية:**

```bash
ffmpeg -loop 1 -i 01_alhambra_aerial_4K.jpg -vf "gblur=sigma=25,eq=brightness=0.35:saturation=0.7,scale=3840:2160,vignette=angle=PI/4" -t 5 -c:v libx264 -crf 16 scene05_bg.mp4
```


---

### مرجع كود — الأسطر الأصلية 1923 وما بعدها

السياق الأصلي: ### 5. برومبت توليد لوحة الستوري بورد للمشهد 05 — انسخه في `Ideogram 2.0` بنفس الـ Seed 42:

```text
Professional motion graphics storyboard breakdown sheet for single shot, vertical layout 9:16, white studio template with thin grid lines

Top header: SHOT 05 /36 | TC 00:27-00:32 | DUR 5s | SCALE: CU Close Up Icon Morph | ALHAMBRA

Main center: Large 16:9 cinematic frame showing photorealistic blurred Alhambra palace background blur 25px brightness 0.35, sharp center Vox vector icon morph sequence: left side palace outline stroke 4px gold #C5A880 morphing into feather pen icon on right, thin gold motion trail between them, below large Arabic word "قصيدة" in Reem Kufi Bold 160px gold #C5A880 with glow, subtitle "لم يُبنَ... بل كُتب" in small ivory, golden ink particles floating, grain 8% vignette 25% rounded corners 24px

Below main frame, 4 horizontal strips showing MOTION BREAKDOWN:
Strip 1 - LAYERS STACK: Diagram 6 layers L0 Gradient #0A1628, L1 Real Photo Blur 25px speed 0.2, L2 Icon Morph Palace->Pen MorphSVG 1.4s, L3 Kinetic Text قصيدة Reem Kufi 160px Stagger 0.05s, L4 Ink Particles 40pts, L5 Vignette Grain
Strip 2 - TIMELINE 0-5s: Horizontal bar with keyframes at 00:27 Blur 00:27.8 Icon Pop 00:28.5 Morph 00:29.5 Write-On قصيدة 00:31 Sub Fade
Strip 3 - CAMERA & EASING: Text "Static Camera + Icon Morph | MorphSVG power2.inOut 1.4s | Text power3.out Stagger 0.05s | Glow Pulse"
Strip 4 - AUDIO: VO text "قصر لم يُبن ليكون مجرد قصر... بل كُتب ليكون قصيدة" IBM Plex Sans Arabic, Music Oud -20dB, SFX Morph Whoosh + Ink Stroke

Right sidebar metadata icons: VO, AUDIO, SFX, TRANSITION Zoom In to Title Card
Footer: Palette swatches #8B1E1E #C5A880 #0F4C5C #F2E8D5 #0A1628, Fonts Reem Kufi/IBM Plex, Seed 42

Style: Ultra clean minimalist Vox x Kurzgesagt documentary, 8K, print-ready, correct Arabic calligraphy --ar 9:16
```


---

### مرجع كود — الأسطر الأصلية 1943 وما بعدها

السياق الأصلي: **Negative Prompt:**

```text
cartoon, anime, blurry text, wrong Arabic, distorted icon, low quality, 3d plastic, watermark, hand sketch, extra text
```


---

### مرجع كود — الأسطر الأصلية 1956 وما بعدها

السياق الأصلي: ### بطاقة المشهد 06

```text
SHOT 06 / 36 | TC: 00:32 - 00:35 | DUR: 3 ثواني - 75 فريم 25fps
النص المنطوق: "إنه قصر الحمراء"
الموسيقى: Solo Oud ذروة خفيفة -16dB + وتر طويل | SFX: Epic Hit + Gold Shimmer
الانتقال للبعد: Title Hold + Camera Push -> المشهد 07 رقم 1238
```


---

### مرجع كود — الأسطر الأصلية 1997 وما بعدها

السياق الأصلي: **منحنى الحركة:** كل الحروف `GSAP back.out(1.4)` لإحساس Vox القوي، الشمر `power2.inOut`. — ### 4. الكود البرمجي `templates/06_title.html`

```html
<div class="scene" id="scene-06" style="width:3840px;height:2160px;background:#0A1628;overflow:hidden;position:relative;display:flex;align-items:center;justify-content:center;flex-direction:column">
  <!-- L1 صورة ليلية -->
  <img id="night" src="../assets/images_real/01_alhambra_night_4K.jpg" style="position:absolute;inset:0;width:100%;height:100%;object-fit:cover;filter:brightness(0.42) contrast(1.1) saturate(1.05);transform:scale(1.10)">
  <div style="position:absolute;inset:0;background:radial-gradient(ellipse at center, transparent 30%, #0A1628 85%);opacity:0.75"></div>
  <div style="position:absolute;inset:0;background:linear-gradient(180deg, rgba(10,22,40,0.6) 0%, transparent 50%, rgba(10,22,40,0.85) 100%)"></div>
  <!-- L2 العنوان -->
  <h1 id="title" style="position:relative;font:700 220px 'Reem Kufi';color:#C5A880;letter-spacing:8px;line-height:1;text-shadow:0 0 40px rgba(197,168,128,0.6), 0 20px 60px rgba(0,0,0,0.8);display:flex;gap:6px">
    <span>ا</span><span>ل</span><span>ح</span><span>م</span><span>ر</span><span>ا</span><span>ء</span>
  </h1>
  <!-- L4 خط ذهبي -->
  <div id="gold-line" style="width:800px;height:4px;background:#C5A880;margin-top:24px;transform:scaleX(0);box-shadow:0 0 20px #C5A880"></div>
  <!-- L3 فرعي -->
  <div id="sub-06" style="margin-top:28px;text-align:center;opacity:0;transform:translateY(30px)">
    <div style="font:400 42px 'Amiri Quran';color:#C5A880;letter-spacing:4px">ولا غالب إلا الله</div>
    <div style="font:300 28px 'IBM Plex Sans Arabic';color:#F2E8D5;opacity:0.8;margin-top:8px;letter-spacing:6px">قصر الحمراء — غرناطة</div>
  </div>
  <!-- L5 Shimmer -->
  <div id="shimmer" style="position:absolute;left:30%;top:46%;width:40%;height:220px;background:linear-gradient(90deg, transparent, rgba(255,255,255,0.35), transparent);transform:translateX(-140%);pointer-events:none;mix-blend-mode:overlay;filter:blur(8px)"></div>
</div>
<script src="gsap.min.js"></script>
<script>
window.seekTo = (frame) => {
  let p = frame / 75;
  gsap.set("#night", { scale: 1.10 + p*0.05 });
  // L2 Type Reveal حرف حرف من الفريم 12
  if(frame >= 12) {
    document.querySelectorAll("#title span").forEach((s,i)=>{
      let appearAt = 12 + i*2; // Stagger 0.08s = 2 فريم
      let tp = Math.min(1, Math.max(0, (frame - appearAt)/8));
      gsap.set(s, { opacity: tp, scale: 0.8 + tp*0.2, y: (1-tp)*40 });
    });
  }
  // L5 Shimmer في الفريم 40
  if(frame >= 40) {
    let sp = Math.min(1, (frame-40)/30);
    gsap.set("#shimmer", { x: -140 + sp*280 + "%" });
    gsap.set("#title", { textShadow: `0 0 ${40+sp*20}px rgba(197,168,128,0.6)` });
  }
  // L4 خط ذهبي في الفريم 50
  if(frame >= 50) {
    let lp = Math.min(1, (frame-50)/20);
    gsap.set("#gold-line", { scaleX: lp });
  }
  if(frame >= 60) gsap.set("#sub-06", { opacity:1, y:0, duration:0.6 });
}
</script>
```


---

### مرجع كود — الأسطر الأصلية 2047 وما بعدها

السياق الأصلي: **أمر FFmpeg للخلفية:**

```bash
ffmpeg -loop 1 -i 01_alhambra_night_4K.jpg -vf "scale=3840:2160,eq=brightness=0.42:contrast=1.1,zoompan=d=1:s=3840x2160:fps=25,z='1.10+0.0006*on',vignette=angle=PI/4" -t 3 -c:v libx264 -crf 16 scene06_bg.mp4
```


---

### مرجع كود — الأسطر الأصلية 2055 وما بعدها

السياق الأصلي: ### 5. برومبت توليد لوحة الستوري بورد للمشهد 06 — انسخه في `Ideogram 2.0` بنفس الـ Seed 42:

```text
Professional motion graphics storyboard breakdown sheet for single shot, vertical layout 9:16, white studio template with thin grid lines

Top header: SHOT 06 /36 | TC 00:32-00:35 | DUR 3s | SCALE: Title Card WS | ALHAMBRA

Main center: Large 16:9 cinematic frame showing photorealistic Alhambra palace at night illuminated golden, background brightness 0.42 blur 2px vignette 30%, sharp center huge Arabic typography "الحمراء" in Reem Kufi Bold 220px gold #C5A880 with long shadow and gold shimmer highlight, thin gold line #C5A880 below title, small subtitle "ولا غالب إلا الله" in Amiri Quran gold and "قصر الحمراء - غرناطة" in small ivory, grain 8% rounded corners 24px, epic cinematic

Below main frame, 4 horizontal strips showing MOTION BREAKDOWN:
Strip 1 - LAYERS STACK: Diagram 7 layers L0 Gradient #0A1628, L1 Night Photo 01_alhambra_night_4K.jpg brightness 0.42 scale 1.10->1.15 speed 0.2, L2 Title Reem Kufi 220px #C5A880 Type Reveal, L3 Subtitle Amiri Quran, L4 Gold Line Trim 800px, L5 Shimmer Overlay, L6 Vignette Grain 8%
Strip 2 - TIMELINE 0-3s: Horizontal bar with keyframes at 00:32 Fade In 00:32.5 Letter ا Pop 00:32.8 Stagger 0.08s 00:33.6 Shimmer 00:34 Gold Line 00:34.4 Sub Fade
Strip 3 - CAMERA & EASING: Text "Static Camera + Type Reveal | back.out(1.4) Stagger 0.08s | Shimmer power2.inOut 1.2s | Long Shadow 45deg"
Strip 4 - AUDIO: VO text "إنه قصر الحمراء" IBM Plex Sans Arabic, Music Oud swell -16dB, SFX Epic Hit + Gold Shimmer + Whoosh

Right sidebar metadata icons: VO, AUDIO, SFX, TRANSITION Zoom Out to Number 1238
Footer: Palette swatches #8B1E1E #C5A880 #0F4C5C #F2E8D5 #0A1628, Fonts Reem Kufi/Anton/IBM Plex/Amiri, Seed 42

Style: Ultra clean minimalist Vox x Kurzgesagt documentary, 8K, print-ready, correct Arabic calligraphy --ar 9:16
```


---

### مرجع كود — الأسطر الأصلية 2075 وما بعدها

السياق الأصلي: **Negative Prompt:**

```text
cartoon, anime, blurry text, wrong Arabic, distorted palace, low quality, 3d plastic, watermark, hand sketch, extra text, oversaturated
```


---

### مرجع كود — الأسطر الأصلية 2088 وما بعدها

السياق الأصلي: ### بطاقة المشهد 07

```text
SHOT 07 / 36 | TC: 00:35 - 00:43 | DUR: 8 ثواني - 200 فريم 25fps
النص المنطوق: "في عام ألفٍ ومئتين وثمانيةٍ وثلاثين للميلاد"
الموسيقى: Solo Oud ينخفض -22dB DUCKED + نغمة توتر | SFX: Clock Tick + Number Crack
الانتقال للبعد: Number Shatter -> المشهد 08 خريطة الأندلس 6 مربعات
```


---

### مرجع كود — الأسطر الأصلية 2127 وما بعدها

السياق الأصلي: **منحنى الحركة:** العداد `power2.out`، التكسير `back.in(1.2)` لكل قطعة، الشقوق `power2.in`. — ### 4. الكود البرمجي `templates/07_year1238.html`

```html
<div class="scene" id="scene-07" style="width:3840px;height:2160px;background:linear-gradient(135deg,#0A1628,#1a0f0f);overflow:hidden;position:relative;display:flex;align-items:center;justify-content:center">
  <!-- L1 خريطة قديمة -->
  <img id="old-map" src="../assets/images_real/07_old_map_texture.jpg" style="position:absolute;inset:0;width:100%;height:100%;object-fit:cover;filter:brightness(0.45) sepia(0.6) contrast(1.1);transform:scale(1.1);opacity:0.7">
  <div style="position:absolute;inset:0;background:radial-gradient(ellipse at center, transparent 40%, #0A1628 85%);opacity:0.8"></div>
  <!-- L2 الرقم -->
  <div id="num-wrap" style="position:relative;text-align:center">
    <div id="number" style="font:700 520px 'Anton';color:#F2E8D5;letter-spacing:12px;line-height:1;text-shadow:0 20px 60px rgba(0,0,0,0.8), 0 0 40px rgba(197,168,128,0.3);-webkit-text-stroke:4px #C5A880;filter:drop-shadow(0 25px 50px rgba(0,0,0,0.6))">0</div>
    <div id="label-year" style="font:300 48px 'Reem Kufi';color:#C5A880;letter-spacing:12px;opacity:0;margin-top:-30px">للميلاد</div>
    <div id="sub-year" style="font:300 32px 'IBM Plex Sans Arabic';color:#F2E8D5;opacity:0;margin-top:12px">القرن الثالث عشر</div>
  </div>
  <!-- L3 شقوق -->
  <svg id="cracks" width="3840" height="2160" style="position:absolute;inset:0;pointer-events:none">
    <path d="M 1920 600 L 1850 900 L 2000 1200 L 1920 1600" stroke="#C5A880" stroke-width="5" fill="none" opacity="0"/>
    <path d="M 1400 800 L 1800 1080 L 2200 950" stroke="#C5A880" stroke-width="4" fill="none" opacity="0"/>
    <path d="M 2400 700 L 2100 1100 L 2500 1500" stroke="#C5A880" stroke-width="4" fill="none" opacity="0"/>
  </svg>
</div>
<script src="gsap.min.js"></script>
<script>
let numEl = document.getElementById('number');
window.seekTo = (frame) => {
  let p = frame / 200;
  gsap.set("#old-map", { scale: 1.10 + p*0.04, x: p*30 });
  // L2 Counter من الفريم 20 الى 62
  if(frame >= 20 && frame <= 62) {
    let cp = (frame-20)/42;
    let eased = gsap.parseEase("power2.out")(cp);
    let val = Math.floor(eased*1238);
    numEl.textContent = val;
    gsap.set("#number", { scale: 1.3 - eased*0.3 });
  }
  if(frame > 62) numEl.textContent = "1238";
  if(frame >= 87) gsap.set("#label-year",{opacity:1, y:0});
  if(frame >= 100) gsap.set("#sub-year",{opacity:0.7, y:0});
  // L3 شقوق في الفريم 112
  if(frame >= 112) {
    let tp = Math.min(1, (frame-112)/30);
    document.querySelectorAll("#cracks path").forEach((path,i)=>{
      gsap.set(path,{opacity:tp, strokeDasharray:800, strokeDashoffset:800 - tp*800});
    });
  }
  // L2 Shatter في الفريم 150
  if(frame >= 150) {
    let sp = Math.min(1, (frame-150)/30);
    gsap.set("#number",{ scale: 1 + sp*0.4, opacity: 1 - sp, rotation: sp*8, filter:`blur(${sp*12}px)` });
    gsap.set("#label-year",{ opacity: 1 - sp*1.5, y: -sp*100 });
  }
}
</script>
```


---

### مرجع كود — الأسطر الأصلية 2180 وما بعدها

السياق الأصلي: **أمر FFmpeg للخلفية:**

```bash
ffmpeg -loop 1 -i 07_old_map_texture.jpg -vf "scale=3840:2160,eq=brightness=0.45:saturation=0.6,sepia=0.6,zoompan=d=1:s=3840x2160:fps=25,z='1.10+0.0004*on',vignette=angle=PI/4" -t 8 -c:v libx264 -crf 16 scene07_bg.mp4
```


---

### مرجع كود — الأسطر الأصلية 2188 وما بعدها

السياق الأصلي: ### 5. برومبت توليد لوحة الستوري بورد للمشهد 07 — انسخه في `Ideogram 2.0` بنفس الـ Seed 42:

```text
Professional motion graphics storyboard breakdown sheet for single shot, vertical layout 9:16, white studio template with thin grid lines

Top header: SHOT 07 /36 | TC 00:35-00:43 | DUR 8s | SCALE: FS Full Screen Number | ALHAMBRA

Main center: Large 16:9 cinematic frame showing photorealistic vintage parchment map texture background brightness 0.45 sepia 0.6, huge Anton font number "1238" centered 520px ivory #F2E8D5 with gold stroke 4px #C5A880 and long shadow, golden crack lines #C5A880 branching on number, subtitle "للميلاد" in Reem Kufi gold below, grain 8% vignette 30% rounded corners 24px, cinematic Vox data viz style

Below main frame, 4 horizontal strips showing MOTION BREAKDOWN:
Strip 1 - LAYERS STACK: Diagram 6 layers L0 Gradient #0A1628, L1 Old Map 07_old_map_texture.jpg brightness 0.45 speed 0.15, L2 Number Anton 520px #F2E8D5 Stroke 4px Counter 0->1238, L3 Cracks Trim Path gold 4px, L4 Label للميلاد, L5 Vignette Grain 8%
Strip 2 - TIMELINE 0-8s: Horizontal bar with keyframes at 00:35 Zoom Out 00:35.8 Counter 0->1238 00:38.5 Label Pop 00:41 Shatter 8 pieces Stagger 0.04s 00:43 Grid Wipe
Strip 3 - CAMERA & EASING: Text "Static Camera + Number Scale 1.3->1.0 | Counter power2.out | Shatter back.in(1.2) | Crack Trim 0.8s"
Strip 4 - AUDIO: VO text "في عام ألف ومئتين وثمانية وثلاثين للميلاد" IBM Plex Sans Arabic, Music Oud -22dB ducked, SFX Clock Tick + Counter Roll + Crack Shatter

Right sidebar metadata icons: VO, AUDIO, SFX, TRANSITION Grid Wipe to 6 Squares
Footer: Palette swatches #8B1E1E #C5A880 #0F4C5C #F2E8D5 #0A1628, Fonts Anton/Reem Kufi/IBM Plex, Seed 42

Style: Ultra clean minimalist Vox x Kurzgesagt documentary, 8K, print-ready, correct Arabic calligraphy --ar 9:16
```


---

### مرجع كود — الأسطر الأصلية 2208 وما بعدها

السياق الأصلي: **Negative Prompt:**

```text
cartoon, anime, blurry text, wrong Arabic, distorted numbers, low quality, 3d plastic, watermark, hand sketch, extra text
```


---

### مرجع كود — الأسطر الأصلية 2220 وما بعدها

السياق الأصلي: ### بطاقة المشهد 08

```text
SHOT 08 / 36 | TC: 00:43 - 00:52 | DUR: 9 ثواني - 225 فريم 25fps
النص المنطوق: "والعالم الإسلامي في الأندلس ينهار... مدن تسقط واحدة تلو الأخرى"
الموسيقى: Solo Oud مستمر -22dB DUCKED + نبضة توتر كل 2 ثانية | SFX: Tick + Grey Thud + Gold Pulse
الانتقال للبعد: Grid Fade -> المشهد 09 أسهم قشتالة
```


---

### مرجع كود — الأسطر الأصلية 2265 وما بعدها

السياق الأصلي: **منحنى الحركة:** الـ Pop `back.out(1.6)`، التحول الرمادي `power2.out 0.6s`، النبض الذهبي `sine.inOut 1.4s infinite`. — ### 4. الكود البرمجي `templates/08_grid.html`

```html
<div class="scene" id="scene-08" style="width:3840px;height:2160px;background:linear-gradient(135deg,#0A1628,#1a0f0f);overflow:hidden;position:relative;display:flex;align-items:center;justify-content:center">
  <!-- L1 خريطة ضبابية -->
  <img src="../assets/images_real/07_old_map_texture.jpg" style="position:absolute;inset:0;width:100%;height:100%;object-fit:cover;filter:brightness(0.35) blur(8px) sepia(0.5);transform:scale(1.1);opacity:0.6">
  <div style="position:absolute;inset:0;background:radial-gradient(ellipse at center, transparent 30%, #0A1628 85%);opacity:0.8"></div>
  <!-- L2 Grid 3x2 -->
  <div id="grid" style="display:grid;grid-template-columns:520px 520px 520px;grid-template-rows:380px 380px;gap:28px;position:relative;z-index:2">
    <!-- 5 مربعات ساقطة -->
    <div class="cell fall" data-city="قرطبة"><span>قرطبة</span><div class="x">✕</div></div>
    <div class="cell fall" data-city="إشبيلية"><span>إشبيلية</span><div class="x">✕</div></div>
    <div class="cell fall" data-city="بلنسية"><span>بلنسية</span><div class="x">✕</div></div>
    <div class="cell fall" data-city="مرسية"><span>مرسية</span><div class="x">✕</div></div>
    <div class="cell fall" data-city="بطليوس"><span>بطليوس</span><div class="x">✕</div></div>
    <!-- 1 مربع ذهبي -->
    <div class="cell gold" data-city="غرناطة"><span>غرناطة</span><div class="pulse-ring"></div><div class="label-gold">تبقى</div></div>
  </div>
</div>
<style>
  .cell{position:relative;background:rgba(197,168,128,0.08);border:4px solid #C5A880;border-radius:22px;display:flex;align-items:center;justify-content:center;flex-direction:column;opacity:0;transform:scale(0.7)}
  .cell span{font:700 42px 'Reem Kufi';color:#F2E8D5}
  .cell .x{position:absolute;inset:0;display:flex;align-items:center;justify-content:center;font-size:140px;color:#8B1E1E;opacity:0;transform:scale(1.4);text-shadow:0 0 30px rgba(139,30,30,0.6)}
  .cell.gold{background:#C5A880;border:6px solid #F2E8D5;box-shadow:0 0 50px rgba(197,168,128,0.7)}
  .cell.gold span{color:#0A1628}
  .pulse-ring{position:absolute;inset:-12px;border:3px solid #C5A880;border-radius:26px;opacity:0}
  .label-gold{position:absolute;bottom:-42px;background:rgba(10,22,40,0.9);border:1px solid #C5A880;border-radius:12px;padding:6px 18px;font:700 26px 'IBM Plex Sans Arabic';color:#C5A880;opacity:0}
</style>
<script src="gsap.min.js"></script>
<script>
window.seekTo = (frame) => {
  // L2 Grid Pop Stagger من الفريم 20
  if(frame >= 20) {
    document.querySelectorAll('.cell').forEach((cell,i)=>{
      let appearAt = 20 + i*3; // 0.12s
      let p = Math.min(1, Math.max(0, (frame-appearAt)/12));
      let eased = gsap.parseEase("back.out(1.6)")(p);
      gsap.set(cell,{opacity:p, scale:0.7 + eased*0.3});
    });
  }
  // L3 + L4 سقوط 5 مربعات Stagger من الفريم 50
  document.querySelectorAll('.cell.fall').forEach((cell,i)=>{
    let fallAt = 50 + i*20;
    if(frame >= fallAt){
      let p = Math.min(1,(frame-fallAt)/15);
      gsap.set(cell,{backgroundColor:`rgba(90,90,90,${p*0.9})`, borderColor: p>0.5 ? "#5a5a5a" : "#C5A880"});
      gsap.set(cell.querySelector('.x'),{opacity:p, scale:1.4 - p*0.4});
      gsap.set(cell.querySelector('span'),{opacity:1-p*0.4, textDecoration: p>0.8 ? "line-through" : "none"});
    }
  });
  // L5 نبض غرناطة من الفريم 150
  if(frame >= 150){
    let pulse = (Math.sin(frame*0.18)+1)/2;
    gsap.set(".cell.gold",{scale:1 + pulse*0.04, boxShadow:`0 0 ${40+pulse*20}px rgba(197,168,128,0.7)`});
    gsap.set(".pulse-ring",{opacity:0.6 - pulse*0.3, scale:1+pulse*0.12});
    gsap.set(".label-gold",{opacity:1, y:0});
  }
}
</script>
```


---

### مرجع كود — الأسطر الأصلية 2325 وما بعدها

السياق الأصلي: **أمر FFmpeg للخلفية:**

```bash
ffmpeg -loop 1 -i 07_old_map_texture.jpg -vf "gblur=sigma=8,eq=brightness=0.35:saturation=0.5,sepia=0.5,scale=3840:2160,vignette=angle=PI/4" -t 9 -c:v libx264 -crf 16 scene08_bg.mp4
```


---

### مرجع كود — الأسطر الأصلية 2333 وما بعدها

السياق الأصلي: ### 5. برومبت توليد لوحة الستوري بورد للمشهد 08 — انسخه في `Ideogram 2.0` بنفس الـ Seed 42:

```text
Professional motion graphics storyboard breakdown sheet for single shot, vertical layout 9:16, white studio template with thin grid lines

Top header: SHOT 08 /36 | TC 00:43-00:52 | DUR 9s | SCALE: WS Data Viz Grid 3x2 | ALHAMBRA

Main center: Large 16:9 cinematic frame showing photorealistic vintage Al-Andalus map blurred background brightness 0.35, sharp foreground Vox data viz grid 3 columns x 2 rows, 6 squares with rounded corners 22px gold border 4px #C5A880, 5 squares grey #5a5a5a with large red X #8B1E1E and strikethrough city names, 1 square bottom-right golden #C5A880 glowing with pulse ring and label "غرناطة - تبقى", city names in Reem Kufi: قرطبة إشبيلية بلنسية مرسية بطليوس غرناطة, grain 8% vignette 30% Vox style

Below main frame, 4 horizontal strips showing MOTION BREAKDOWN:
Strip 1 - LAYERS STACK: Diagram 7 layers L0 Gradient #0A1628, L1 Old Map Blur 8px, L2 Grid 520x380 6 cells Stagger 0.12s, L3 Grey Transition 0.6s, L4 Red X Scale 1.4->1, L5 Gold Pulse 1.4s infinite, L6 City Labels Strikethrough, L7 Vignette Grain
Strip 2 - TIMELINE 0-9s: Horizontal bar with keyframes at 00:43 Grid Pop 00:45 قرطبة Fall 00:45.8 إشبيلية 00:46.6 بلنسية 00:47.4 مرسية 00:48.2 بطليوس 00:49 Granada Pulse Gold
Strip 3 - CAMERA & EASING: Text "Static Camera + Grid Stagger | back.out(1.6) | power2.out 0.6s | Pulse sine.inOut 1.4s infinite"
Strip 4 - AUDIO: VO text "والعالم الإسلامي في الأندلس ينهار... مدن تسقط" IBM Plex Sans Arabic, Music Oud -22dB ducked, SFX Tick x6 + Grey Thud x5 + Gold Shimmer

Right sidebar metadata icons: VO, AUDIO, SFX, TRANSITION Arrow Wipe to Next
Footer: Palette swatches #8B1E1E #C5A880 #0F4C5C #F2E8D5 #0A1628, Fonts Reem Kufi/IBM Plex, Seed 42

Style: Ultra clean minimalist Vox x Kurzgesagt documentary data viz, 8K, print-ready, correct Arabic calligraphy --ar 9:16
```


---

### مرجع كود — الأسطر الأصلية 2353 وما بعدها

السياق الأصلي: **Negative Prompt:**

```text
cartoon, anime, blurry text, wrong Arabic, distorted grid, low quality, 3d plastic, watermark, hand sketch, extra squares
```


---

### مرجع كود — الأسطر الأصلية 2366 وما بعدها

السياق الأصلي: ### انسخ هذا القالب كامل وضعه كـ SYSTEM PROMPT للوكيل

```markdown
# وكيل موشن الحمراء - Vox Hybrid Documentary Agent

## هويتك
انت خبير موشن جرافيك وثائقي بأسلوب Vox + Kurzgesagt + Johnny Harris
مهمتك تكمل ستوري بورد مشروع "الحمراء - قصيدة من حجر" بنفس المنهجية بدون اي انحراف

## القواعد الذهبية - ممنوع كسرها
1. لا يوجد فريم ثابت اكثر من 2 ثانية - كل ثانية فيها حركة
2. كل مشهد = صورة واقعية 4K حقيقية + فيكتور ذهبي فوقها (هجين)
3. كل صورة واقعية معالجة Duotone: ظلال #0A1628 + اضاءة #C5A880 بنسبة 35% + Grain 8% + Vignette 25%
4. لا تستخدم انتقال Dissolve ابدا - فقط Morph / Trim Path / Iris Wipe / Whip Pan / Grid Wipe
5. كل حركة GSAP power2.out او power3.out او back.out(1.4) - ممنوع الحركة الخطية
6. Seed ثابت لكل الصور = 42 للحفاظ على وحدة الالوان
7. ترد دائما بنفس الهيكل الخماسي لكل مشهد بدون اختصار

## الهوية البصرية الثابتة
PALETTE: #8B1E1E احمر ناصري 60% | #C5A880 ذهب 20% | #0F4C5C ازرق زليجي 10% | #F2E8D5 عاج 5% | #0A1628 كحلي 5%
تدرج: linear-gradient 135deg #0A1628 -> #8B1E1E
خطوط: Reem Kufi Bold 140-220px عناوين | Anton 200-520px ارقام | IBM Plex Sans Arabic Light 28-48px شرح | Amiri Quran ايات
ستروك: 4px #C5A880 | زوايا: 24px | ظل: 30% | Grain: 8% | Vignette: 25%
ابعاد: 3840x2160 25fps

## هيكل المجلدات البرمجي
D:\Alhambra_Vox\
├── config.js
├── main.js // Puppeteer + Chrome Headless يرندر HTML 25fps
├── templates/ 01_stone.html ... 36_finale.html
├── assets/images_real/ // 12 صورة 4K حقيقية
└── output/

## بنك الصور الواقعية الـ 12
01_alhambra_aerial_4K.jpg | 02_lions_court_real.jpg | 03_arrayanes_pool.jpg | 04_muqarnas_ceiling.jpg | 05_wall_calligraphy.jpg | 06_granada_city.jpg | 07_old_map_texture.jpg | 08_golden_key.jpg (+ 4 مقصوصة PNG شفافة)

## قائمة المشاهد الـ 36 كاملة - التزم بها
01 | 00:00-00:05 | 5s  | هل يمكن لحجر أن يحفظ الشعر؟ | ECU جدار نقش
02 | 00:05-00:12 | 7s  | وجدران تروي تاريخ 8 قرون؟ | WS Timeline 800
03 | 00:12-00:20 | 8s  | هنا فوق هضبة السبيكة | ELS خريطة فضاء زوم 800%
04 | 00:20-00:27 | 7s  | حيث تعانق السماء غرناطة | ELS Isometric 5 طبقات Parallax
05 | 00:27-00:32 | 5s  | قصر لم يُبن ليكون قصر.. بل قصيدة | CU Morph قصر->قلم
06 | 00:32-00:35 | 3s  | إنه قصر الحمراء | Title Card 220px
07 | 00:35-00:43 | 8s  | في عام 1238 للميلاد | FS رقم 1238 يتكسر
08 | 00:43-00:52 | 9s  | والعالم ينهار مدن تسقط | WS Grid 6 مربعات 5 رمادي + 1 ذهبي
09 | 00:52-01:00 | 8s  | مدن تسقط بيد قشتالة | WS اسهم حمراء + اسماء تنشطب
10 | 01:00-01:08 | 8s  | ظهر رجل واحد.. معجزة ابن الاحمر | CU سيلويت Particles Assemble
11 | 01:08-01:16 | 8s  | على انقاض قلعة مهجورة | WS Blueprint يرسم
12 | 01:16-01:24 | 8s  | جلب الماء 6km نهر دارو | WS خط ازرق + عداد 0->6km
13 | 01:24-01:30 | 6s  | فأحيا الارض وسماه الحمراء | WS Wipe صحراء->خضراء + طوب Isometric
14 | 01:30-01:38 | 8s  | ذروة القرن 14 يوسف ومحمد الخامس | WS Timeline Jump 1333
15 | 01:38-01:45 | 7s  | تحولت القلعة لتحفة | CU Morph سيف->زخرفة 8 اضلاع
16 | 01:45-01:53 | 8s  | كل شيء له معنى | CU Pattern شبكة هندسية
17 | 01:53-02:03 | 10s | بهو الريحان بركة تعكس السماء | WS Split Reflection 34x7m
18 | 02:03-02:10 | 7s  | ليشعر الملك انه يملك السماء | WS Mirror غيوم
19 | 02:10-02:18 | 8s  | قلب الحمراء بهو الاسود | WS Iris Wipe قوس
20 | 02:18-02:25 | 7s  | 12 اسدا تحمل حوضا | TOP-DOWN 12 اسد + عداد 1/12
21 | 02:25-02:33 | 8s  | 124 عمود رخام | WS Stagger اعمدة 0.02s
22 | 02:33-02:42 | 9s  | قصيدة ابن زمرك | CU نص دائري Text On Path
23 | 02:42-02:52 | 10s | قبة المقرنصات 5000 قطعة | CU سقف Isometric Build
24 | 02:52-03:00 | 8s  | نجوم تجمدت | CU مقرنصات->نجوم Particle Stars
25 | 03:00-03:10 | 10s | ولا غالب الا الله + هنا ولد جادك الغيث | WS Pattern + Lyric جادك الغيث
26 | 03:10-03:18 | 8s  | 2 يناير 1492 | CU تقويم يقلب ختم احمر
27 | 03:18-03:25 | 7s  | سلم المفاتيح | CU مفتاح ذهبي يسقط Slow Motion
28 | 03:25-03:33 | 8s  | زفرة العربي الاخيرة | WS خريطة نقطة تبتعد + دموع
29 | 03:33-03:40 | 7s  | ظنوا انها ستموت | WS فلتر رمادي + تشققات
30 | 03:40-03:48 | 8s  | قصر كارلوس داخلها | WS Overlay دائري رمادي فوق الاحمر
31 | 03:48-03:55 | 7s  | اهملت لاكواخ | WS Timelapse غبار
32 | 03:55-04:05 | 10s | لكنها رفضت ان تموت | WS Brush Wipe ترميم
33 | 04:05-04:15 | 10s | اكتشفها الرحالة القرن 19 | WS كتب تفتح Sketch
34 | 04:15-04:25 | 10s | اليوم يونسكو 2.7 مليون | WS عداد + شعار UNESCO
35 | 04:25-04:35 | 10s | رسالة صمود | CU حجارة->حروف Morph
36 | 04:35-04:00 | 25s | ولا غالب الا الله + لم يكن وصلك الا حلما كورال | WS ليلية مضاءة + Glow Dissolve + Vocal

## الهيكل الخماسي الاجباري لكل رد - لا تختصره
عندما يطلب منك "اكمل" او "المشهد التالي" يجب ان ترد بهذا الترتيب بالضبط:

### 1. بطاقة المشهد
SHOT XX /36 | TC: XX-XX | DUR: Xs - X فريم 25fps
النص المنطوق: "..."
الموسيقى: ... | SFX: ... | الانتقال للبعد: ...

### 2. الفكرة الحركية
فقرة واحدة تشرح الانتقال من المشهد السابق بدون قطع + الفكرة Vox

### 3. الطبقات - جدول 6-7 طبقات
| طبقة | المحتوى | المصدر | الحركة والسرعة |
L0 Background ... الخ

### 4. التسلسل الزمني بالفريم - جدول دقيق
| الوقت | الفريم | ماذا يحدث بصريا | الصوت |

### 5. الكود البرمجي templates/XX_name.html
كود HTML + CSS + GSAP كامل يرندر بالكروم 25fps مع window.seekTo + Canvas Particles + امر FFmpeg للخلفية

### 6. برومبت توليد لوحة الستوري بورد
برومبت Ideogram 2.0 9:16 Seed 42 + Negative Prompt + الاعدادات

## مثال مرجعي - هذا هو مستواك المطلوب (مشهد 08)
[انسخ هنا مشهد 08 كامل كمرجع للجودة والتفصيل]

## كيف تتصرف
- اذا قال "اكمل" تكمل المشهد التالي مباشرة بالترتيب
- اذا قال "المشهد X" تذهب له مباشرة
- لا تسأل اسئلة - نفذ مباشرة
- حافظ على Seed 42 ونفس الباليت والخطوط في كل المشاهد
- كل كود HTML يجب ان يكون جاهز للرندر ب Puppeteer بدون اخطاء
- كل برومبت صور يجب ان يكون بالانجليزي ومفصل ويذكر correct Arabic calligraphy

## ملاحظة الموسيقى - جادك الغيث
اللحن Instrumental 68 BPM D minor يشتغل طول الفيديو -18 الى -22dB Ducked تحت الـ VO
الغناء Vocal يدخل فقط مرتين: مشهد 25 (جادك الغيث اذا الغيث هما) ومشهد 36 (لم يكن وصلك الا حلما) مع adelay 165000 في FFmpeg sidechaincompress
```


---

## ملاحظات دمج مهمة للوكيل

1. أنشئ أولاً نسخة Preview بمشهد أو ثلاثة، ثم اربط بقية المشاهد.
2. افصل طبقات: `config`, `timeline`, `scene renderer`, `audio mix`, `FFmpeg assembly`, و`QA`.
3. لا تستخدم `setTimeout` أو `requestAnimationFrame` كمصدر وحيد للحالة في رندر فريم-بفريم؛ اجعل الحالة مشتقة من `frame`.
4. أي كود يستخدم `Math.random()` يجب تحويله إلى مولد عشوائي ثابت.
5. أي استخدام لـ `DrawSVGPlugin` أو `MorphSVGPlugin` يجب أن يملك fallback.
6. راجع مدد المشاهد؛ بعض الأكواد في الحوار كانت أمثلة تعليمية وليست جدولاً زمنياً نهائياً.
7. قبل التصدير، نفّذ فحصاً للصوت، النص العربي، الحواف، الوميض، وتزامن الانتقالات.
