# EOS ۔ IO تکنیکی سفید کاغذ

**26 جون، 2017**

**خلاصہ <0\> EOS. IO سوفٹویر ایک نیا پیمانہ کاری بلاکچین کو متعارف کرتا ہے جو ڈیسنٹرالایزڈ اپلیکیشنز کو عمودی اور افقی سکیلنگ کے لیے معیاری بناتا ہے -. یہ اپلیکیشن جس پر ڈیولپ کر سکتے ھے ایک آپریٹنگ سسٹم کی طرح تعمیر کی تشکیل کی طرف سے حاصل ھوتا ھے -. یہ سافٹویئر اکاؤنٹ، توسیق کاری، ڈیٹابیس، ایسنچورنس، ابلاغ اور اپلیکیشنز کے سینکڑوں کروڑ CPU کلسٹرس فراہم کرتا ہے -. بنای گیی تکنیک ایک بلاکچین فن تعمیر ھے جو کروڑوں کا لین دین فی سیکنڈ کرتا ھے، صارف کا فیس ختم کرتا ہے اور ڈیسنٹرالایزڈ اپلیکیشنز کی فوری اور آسان ڈیپلایمنٹ کرتا ہے -.</p> 

**مہربانی کر کے یاد رہے:اس سفید کاغذ میں جو کرپٹوگرافک ٹوکن ھے وہ کرپٹوگرافک ٹوکنز دراصل EOS. IO سوفٹویر سے منسلک ہے -. ان کا تعلق ERC-20 ٹوکنز کے ساتھ نہیں ھے اتھیریم بلاکچین EOS ٹوکن تقسیم کے حوالے تقسیم ھو نے کی وجہ سے ھوتا ھے.**

کاپی رایٹ © 2017 پھلا بلاگ

اس سفید کاغذ پر بغیر کسی اجازت کے کوئی بھی اس کا استعمال، اس کی تقسیم غیر تجارتی کام یا تعلیماتی مقاصد کے استعمال میں لا سکتے ہیں بشرطیکہ اصل زرایع اور کابل اطلاق کاپی رائیٹ کا حوالہ دیا گیا ھو -.

<0\>DISCLAIMER EOS. IO تکنیکی سفیر کاغذ صرف معلوماتی مقاصد کے لیے ھے. پھلا بلاک کسی بھی درستگی کی ضمانت نہیں دیتا ھے اور یہ سفید کاغذ بالکل اپنی اصل شکل میں موجود رہتا ہے. پھلا بلاک تمام معروضات کو واضح طور پر داسکلایم اور وارنٹی، ایکسپریس، مضمر، قانونی یا بصورت دیگر جو کچھ بھی، بشمول تک محدود نہیں ہے -1))مفت پزیرائی کی وارنٹی، کسی اھم مقصد یا مناسبت کے بارے میں، استعمال کے لیے معضونیت کا عنوان یا نانفراگمنٹ -2))یہ سفید کاغذ غلطیوں سے بالکل مفت ھے؛ اور (٣)ایسے مشمولات دوسرے لوگوں کے حقوق کی خلاف ورزی نہیں کرتا ھے -. پھلا بلاک اور اس کے ساتھ منسلک کسی بھی چیز کی زمہ داری سے کسی بھی قسم کے استعمال کرنے کے لیے حوالے سے پیدا ہونے والے نقصانات کے لیے ھو گا یا یہ سفید کاغذ یا مواد کسی پر منحصر نہیں یھان تک کہ خطرات کے امناکات کا مشورہ دیا گیا ھو -. پہلے بلاک کی کسی بھی تقریب یا اس کے الحاق، یا فتگان براہ راست، یا بالواسطہ نتیجتاً، کمپنسیٹری، اتفاقی، اصل، مسالی، کسی خاص شخص کے لئے تعزیری، جواب دہ نہیں بناتا - اس سفید کاغذ میں کسی بھی کاروباری نقصان، آمدنی، منافع، ڑیٹا کے استعمال کی خیر گالی یا دوسرے طریقوں کی ضمانت دیتا ہے اور اس کے ساتھ منسلک چھوٹے چھوٹے نقصانات -.

- [Background](#background)
- [بلاکچین اپلیکیشنز کے لیے ضروریات-](#requirements-for-blockchain-applications) 
  - [یہ لاکھوں صارفین کی حمایت کرتا ہے-](#support-millions-of-users)
  - [مفت استعمال-](#free-usage)
  - [آسان اپگریڈ اور بگ کی وصولی-](#easy-upgrades-and-bug-recovery)
  - [قلیل چھپاو-](#low-latency)
  - [ترتیب وار کارکردگی-](#sequential-performance)
  - [متوازی کارکردگی-](#parallel-performance)
- [اتفاق رائے الگورزم (ڈی پی او)](#consensus-algorithm-dpos) 
  - [لین دین کی تصدیق](#transaction-confirmation)
  - [د لین دین کو سبوت کے طور پر داو پر رکھنا ( ٹاپوس)-](#transaction-as-proof-of-stake-tapos)
- [اکاؤنٹ](#accounts) 
  - [پیغامات اور اس کے رھبر-](#messages--handlers)
  - [کارکردگی پر مبنی اجازاتی انتظام](#role-based-permission-management) 
    - [نامی اجازت کی حدیں-](#named-permission-levels)
    - [نامی پیغامات نیٹ کار گروہ-](#named-message-handler-groups)
    - [نقشہ کاری کی اجازت-](#permission-mapping)
    - [جایزہ لینے کی اجازت-](#evaluating-permissions) 
      - [طے شدہ اجازاتی گروہ-](#default-permission-groups)
      - [متوازی جایزہ کی اجازت-](#parallel-evaluation-of-permissions)
  - [لازمی تاخیر کے ساتھ پیغامات](#messages-with-mandatory-delay)
  - [چوری شدہ چابیوں سے وصولی-](#recovery-from-stolen-keys)
- [اپلیکیشنز کے بیچ متوازی نفاذ-](#deterministic-parallel-execution-of-applications) 
  - [مواصلاتی چھپاو کو کم کرنا-](#minimizing-communication-latency)
  - [صرف ھینڈلرس کے انتخابات پڈھنا-](#read-only-message-handlers)
  - [ایک سے زائد اکاؤنٹ کے ساتھ اٹمی لین دین-](#atomic-transactions-with-multiple-accounts)
  - [بلاکچین کی صورتحال کی جزوی تشخیص-](#partial-evaluation-of-blockchain-state)
  - [بھترین کوشش اس کے لیے شرط ہے-کی](#subjective-best-effort-scheduling)
- [ٹوکن ماڈل اور وسائل کا استعمال](#token-model-and-resource-usage) 
  - [معروضی اور کتابی پیمائش-](#objective-and-subjective-measurements)
  - [صارف کا ادا کرنا-](#receiver-pays)
  - [صلاحیت کو استعمال کرنا-](#delegating-capacity)
  - [لین دین کو ٹوکن کی قیمت سے الگ کرنا-](#separating-transaction-costs-from-token-value)
  - [اصل صورت میں رکھنے کی لاگت-](#state-storage-costs)
  - [بلاک کے انعامات-](#block-rewards)
  - [لوگوں کے لیے اپلکیش کے فوائد-](#community-benefit-applications)
- [حکمرانی](#governance) 
  - [اکاؤنٹ منجمد کرنا](#freezing-accounts)
  - [اکاؤنٹ کوڈ بدلنا-](#changing-account-code)
  - [دستور](#constitution)
  - [آیین اور دستور کو بڈھاوا دینا-](#upgrading-the-protocol--constitution) 
    - [ھنگامی تبدیلیاں-](#emergency-changes)
- [مجازی اور نوشہ جات مشینیں](#scripts--virtual-machines) 
  - [شجرہ پیغامات کی وضاحت-](#schema-defined-messages)
  - [وضاحتی شجرہ ڈیٹابیس](#schema-defined-database)
  - [توسیق کاری کو اپلیکیشن سے الگ کرنا](#separating-authentication-from-application)
  - [فن تعمیر سے آزاد مجازی مشین](#virtual-machine-independent-architecture) 
    - [ویب اسمبلی (WASM)](#web-assembly-wasm)
    - [مجازی اتھیریم مشین EVM))](#ethereum-virtual-machine-evm)
- [بلاکچین کی اندرونی گفتگو](#inter-blockchain-communication) 
  - [لائیٹ کلائینٹ جواز کے لیے مارکلی کے سبوت LVC))](#merkle-proofs-for-light-client-validation-lcv)
  - [اندرونی چین اور ابلاغ کا چھپاو](#latency-of-interchain-communication)
  - [اختتامی سبوت](#proof-of-completeness)
- [مقصد](#conclusion)

# پس منظر

بلاکچین تکنیک bitcoin رقم کے ساتھ سال 2008 میں متعارف کی گئی، اور اس کے بعد تاجروں اور ڈیولپرس نے تکنیک کو عام کرنے میں اور تمام بلاکچین کو ایک ہی راستہ پر لانے کے لیے اپنا تعاون بخوبی دیا ہے.

چونکہ بلاکچین پلیٹ فارم کی ایک بڑی تعداد نے فنکشنل ڈیسنٹرالایزڈ اپلیکیشنز کے تعاون میں کافی جدوجہد کی، جبکہ مخصوص بلاکچین اپلیکیشنز جیسے کہ بٹشیر ڈیسنٹرالایزڈ ایکسچینج 2014 اور سٹیم سوشل میڈیا پلیٹ فارم 2016 بھت زیادہ استعمال ہونے والی بلاکچین بن گیی ھے جس کا استعمال روزانہ ھزاروں صارف کرتے ہیں. انھوں نے یہ ایک سیکنڈ میں ھزاروں لین دین کرنا جیسے کارکردگی میں اضافہ کرنے سے حاصل کیا، اخفاء کو ١. ٥ سیکنڈ سے کم کرنا، فیس ختم کرنا، اور سارفین کو ایسی خدمات دینا جو کہ موجودہ مرکزی خدمات فراہم کرتا ہے.

موجودہ بلاکچین پلیٹ فارم بھاری فیس کے دباؤ میں ھے جس وجہ سے صارفین اس کو بڈے پیمانے پر استعمال کرنے سے کتراتے ھے.

# بلاکچین اپلیکیشنز کے لیے ضروریات

بڈے پیمانے پر اس کا استعمال کرنے کے لئے بلاکچین کو ایک اچھے پلیٹ فارم کی ضرورت ہے جو کافی لچک دار ھو اس کے لیے مندرجہ ذیل عکوپمنٹس درکار ضروریات درکار ہے:

## لاکھوں صارفین کی حمایت حاصل کرنا

سوشل نیٹ ورکنگ ویب سائٹ جیسے ای بے، عبر، AirBnB, اور فیس بک کا کاروبار تباہ کرنے کے لیے، بلاکچین کے زیر اہتمام لاکھوں لوگ اس کا استعمال کرتے ہیں. کچھ معاملات میں اپلیکیشن تب تک کام نہیں کرتی جب تک صارفین کی ایک مخصوص تعداد نہ آجائے لہذا ایک ایسے پلیٹ فارم کو اہمیت دی جاتی ہے جو زیادہ سے زیادہ صارفین کا دباؤ سحن کر سکے.

## مفت استعمال

صارفین کو مفت خدمات فراہم کرنے کے لیے اپلیکیشن ڈیولپرس کو لچک کی ضرورت ہے؛ سارفین کو پلیٹ فارم کا استعمال کرنے اور اس سے فائدہ اٹھانے کے لیے فیس ادا نہیں کرنی ھو گی. ایسا بلاکچین پلیٹ فارم جو صارف کے لیے مفت دستیاب ھو سارفین اس کو بڈی تیزی سے اپنائے گے. کاروباری اور ڈیولپرس اس کے بعد موثر حکمت عملی بنا سکتے ہیں.

## آسان ترقی اور بگ کی وصولی

کاروبار پر مبنی تعمیراتی پلاکچین اپلیکیشنز کو بڈھاوا دینے کے لیے اپلیکیشنز میں لچک کی ضرورت ہے-.

تمام غیر معلوماتی سافٹویئر کا انحصار بگ پر ھے، ختاکہ سب سے زیادہ سختی کی باقاعدہ تصدیق سے ھے. بگ کو سنبھالنے کے لیے پلیٹ فارم کا مضبوط ھونا کافی ضروری ھے.

## قلیل چھپاو

صارف کو بھتر خدمات فراہم کرنے کے لیے چاہیے کہ جواب الجواب میں کچھ سیکنڈ سے زیادہ وقت نا لگے. لمبی تاخیر صارفین کو پریشانی میں ڈالتی ھے اور بلاکچین کو پھلے سے چلنے والی بلاکچین اپلیکیشنز کے مقابلے میں کم اھمیت حاصل ھوتی ھے.

## سلسلہ وار کارکردگی

کچھ اپلیکیشنز ایسی ھے جن کو ھم صرف جو ترتیب پر منحصر اقدامات کی وجہ سے متوازی الگورزم کے ساتھ لاگو نہیں کر سکتے ہیں. اپلیکیشنز جیسے کہ ایکسچینج بہت زیادہ ترتیبات کی حامل ہے کارکردگی کا مظاہرہ کرنے کے لیے. لھزا اس کے لیے تیز تربیاتی پلیٹ فارم کی ضرورت ہے.

## متوازی کارکردگی

بڈے پیمانے کی اپلیکیشنز کام تقسیم کرنے کے لیے متعدد CPU اور کمپیوٹرز کا استعمال کرتا ہے.

# اتفاق رائے الگورزم (ڈی پی اوس)

EOS. IO سوفٹویر صرف ڈیسنٹرالایزڈ الگورزم کا استعمال کرتا ھے جو کارکردگی پر کھرا اتر سکے بلاکچین اپلیکیشنز پر، DPOS))<0\>کو سبوت کے طور پر استعمال کیا جاتا ہے. اس الگورزم کے تحت، جو لوگ EOS. IO ٹوکن اپناے ایک بلاکچین رکھیں. سافٹویئر بلاک بنانے والے ایک مسلسل انتخابی عمل کے زریعے منتخب کر سکتے ہیں. اور کسی کو بھی اس کا حقدار بنا سکتے ہیں اور تمام پرڈوسدز کو ووٹس پر مناسب بلاکس پیدا کرنے کا موقع دیا جائے گا. زاتی بلاکچین کا انتظام اور ٹوکن کا استعمال کرنے کے لیے اور عملے کو ھٹانے کے لیے استعمال کر سکتے ہیں.</p> 

EOS. IO سوفٹویر ہر تین سیکنڈ کے بعد پیدا ہونے والے بلاکس کو بناتا ھے اور ایک پڑوسی کو کسی بھی بلاک پر پیدا کرنے کے لیے اجازت حاصل ہے. بلاک اگر مکررہ وقت پر پیدا نہ ہو تو اس کے لیے جگہ کو سکپ کیا جاتا ہے. جب ایک سے زائد بلاک سکپ کیے جاتے ہیں تب بلاکچین میں چھے سیکنڈ کا ایک وقفہ دیا جاتا ہے.

EOS. IO سافٹویئر کے استعمال سے اکیس چکروں میں بلاکس تیار ہوتے ہیں. اکیسویں چکر کے منفرد بلاک کے شروع میں پرڈوسدز کا انتخاب کیا جاتا ہے. ھر مرحلے کے بعد اوپری 20 منظور شدہ پرڈوسدز کا خود بہ خود انتخاب ھوتا ھے اور آخری پرڈوسدز متناسب کا ووٹ دوسرے پرڈوسدز، ان کی نسبت کا انتخاب کیا جاتا ہے. منتخب کیے گئے پرڈوسدز بلاک وقت سے ماخوذ کازبی تصادفی عمل کا استعمال کرتے ہوئے شفل کرتے ہیں. یہ ردوبدل اس مقصد کے لیے کی جاتی ہے تاکہ دوسرے پرڈوسدز کا توازن برقرار رکھا جا سکے-.

اگر ایک پرڈوسد ایک بلاک کو بنانا بھول جاتا ہے تو یہ بلاک چوبیس گھنٹوں کے اندر اس سے نکالا جاتا ہے تاکہ وہ بلاکچین بلاکس کی پیداوار شروع کرنے کے لیے اپنے ارادے ظاہر کرے وہ بڈے دھیان سے ہٹا دیے جاتے ہیں. یہ اس کی تصدیق کرتا ہے کہ نیٹورک آسانی سے چل جائے. ناقابل اعتبار بلاکس کی پالیسیوں کو کم کرنے سے-.

عام حالات میں ایک DPOS بلاکچین بلاکس کو پیدا کرنے کے بجائے مقابلہ بلاک پرڈوسدز کا تعاون کرتا ہے اور بلاکچین کسی فورکس کا ساتھ نہیں دیتا. اس عمل میں ایک فورک، بہ اتفاق رائے سے خود بخود سب سے طویل سلسلے کے لیے کھلے گی. یہ میٹرک اس وجہ سے کام کرتی ہے کیونکہ اس کی شرح جس پر بلاکس کے لیے ایک بلاکچین فورک شامل ہے وہی اجماع کی حصہ داری بلاک پرڈوسدز کے تناسب سے براہ راست جڈ جاتی ہے. دوسرے الفاض میں ہم یہ کہہ سکتے ہیں کہ، زیادہ فورک پرڈوسدز رکھنے والی بلاکچین کم تعداد والی سافٹویئر پرڈوسدز کے مقابلے میں زیادہ بڈھاوا حاصل کرے گی. علاوہ ازیں، کوئی بھی بلاک پرڈوسد دو فورکس پر ایک ساتھ بلاک نہیں بنا سکتا. اگر کسی بلاک پرڈوسد کو ایسا کرتے ہوئے پایا گیا تو ایسے بلاک پرڈوسد کو باہر نکال دیا جائے گا. غلط کام کرنے والوں کو خود بخود باہر کیا جا سکتا ہے اگر دوبار پروڈکشن کی.

## لین دین کی تصدیق

بنیادی DPOS بلاکچین بلاک بنانے میں 100فیصد شراکت داری رکھتا ہے. ایک ٹرانزکش کی تصدیق 99.9فیصد اوسط سے بڈا کاسٹ کا وقت 1.5سیکنڈ کے بعد سمجھا جا سکتا ہے-.

یہاں کچھ ایسے غیر معمولی واقعات موجود ہے جہاں ایک سافٹویئر بگ ،انٹرنیٹ بھیڈ، یا بغض پر مبنی بلاک پرڈوسد دو یا دو سے زائد فورکس پیدا کر دے گا. اس تصدیق کے لئیے کہ ٹرانزکش ناقابل تلافی ہے، 15 سے 21 بلاک پرڈوسدز کی طرف سے تصدیق کے لیے انتظار کرنا ایک گروہ انتخاب کر سکتا ہے. EOS.IO کنفگریشن پر مبنی بنیادی سافٹویئر، ان حالات میں عموماً 45 سیکنڈ کا وقت لیتا ہے. مگر پہلے سے طے شدہ نوڈز ایک بلاک کو 21 میں سے 15پرڈوسرز نا قابل طلافی کی طرف سے اس بات کی تصدیق پر غور کرے گیں اور ایک فورک جو اس طرح کے بلاک کی لمبائی سے قطع نظر سے خارج کر دے گا.

یہ ایک گروہ کے لیے ممکن ہے کہ وہ صارفین کو ایک عقلیتی فورک پر9 سیکنڈ کے اندر اندر فورق کا آغاز کریں. دو لگاتار مسڈ بلاکس کے بعد ایک گروہ پر ایک چھوٹا فورک ہے جس کے 95 فیصد امکان ہے. تین لگاتار مسڈ بلاکس کے بعد ایک عقلیتی فورک ہونے کی وجہ سے 99 فیصد یقینی ہے. ایک مظبوط پیشگوانہ ماڈل بنانے کے لیے مسڈ نوڑز کی جانکاری رکھنے کے بارے میں معلومات استعمال کرے گا، حالیہ شرکت کی شرح اور دوسرے خدشات مسالے تیزی سے آپریٹرز کی تنبیہ کسی غلطی پر پیدا کرنے کے لیے ممکن ہے.

ایسی تنبیہ کا پورا دارومدار اور جوابی عمل مکمل طور پر کاروباری لین دین کی نوعیت پر منحصر ہے مگر سب سے سادہ عمل15/ 21کنفرمیشن کا انتظار کرنا ہ. ے-.

## لین دین کو داو پر سبوت کے طور پر رکھنا TaPos))

EOS. IO سوفٹویر سے لین دین کی ایک حالیہ بلاک سر تحریر پیش کرنے کے لیے ہیش شامل کرنا ضروری ہے یہ ہیش دومقاصد کے لیے اپنی خدمات فراہم کرتا ہے:

1. لین دین کے محولہ کو روکنے کے لیے فورکس بلاک نہیں روکتا ہے؛ اور
2. اشارہ نیٹورک کاکسی مخصوص صارف اور اس داو پر ایک مخصوص فورک ہے.

وقت کے ساتھ ساتھ تمام صارفین براہ راست بلاکچین کی تصدیق کرتا ہے جو جعلی لین دین جایز طریقہ سے منتقل کرنے کے قابل نہیں ہوگا جیسا کہ جعلی چین فورج کو مشکل بناتا ہے.

# اکاؤنٹ

EOS.IO سافٹویئر تمام اکاونٹس کو ایک منفرد انسانی قابلِ مطالعہ 2سے32 الفاظ پر مشتمل حروف کا حوالہ دیا گیا ھو. نام اکاؤنٹ کی تخلیق کرنے والا منتخب کرتا ہے. اکاونٹس کی دیکھ بھال کے لیے تمام اکاونٹس میں اکاونٹ بناتے وقت ایک چھوٹی رقم رکھنا ضروری ہے. اکاونٹ کے نام ناموں کے درمیان مالک کا اکاونٹ @domainہےصارف صرف اسی domain پر اپنا اکاونٹ بنا سکتا ہے.

ایک ڑیسنٹرالایزڈ سیاق و سباق میں، اپلیکیشن ڈیولپر کو نیا اکاونٹ بنانے کے لیے ایک معمولی فیس ادا کرنی ہوگی. روایتی کاروباریوں نے پہلے ہی کافی پیسہ صارفین کے لیے اشتہارات کی شکل میں، مفت خدمات، وغیرہ. نیے بلاکچین اکاؤنٹ کو فنڈ کرنے کی قیمت مقابلہ میں غیر معمولی ہونی چاہیے. خوش قسمتی سے، ان صارفین کو اکاؤنٹ بنانے کی ضرورت نہیں جنھوں پہلے سے ہی دوسری اپلیکیشنز میں اپنا اکاونٹ بنایا ہو.

## پیغامات اور ان کی دیکھ بھال کرنے والے

ہر ایک اکاؤنٹ سے دوسرے اکاونٹ تک ساخت پیغامات پہنچانے جا سکتے ہیں اور موصول ہونے والے پیغامات سے نمٹنے کے لئے سکرپٹس کی وضاحت کی جاتی ہے. EOS.IO سافٹویئر ہر ایک اکاؤنٹ کو اس کا ایک مخصوص ڈیٹابیس دیتا ہے جن کو صرف پیغامات سنبھالنے والے استعمال کر سکتے ہیں. پیغامات سنبھالنے والے نوشوں کو بھی اکاونٹس میں منتقل کر دیا جا سکتا ہے. پیغامات کا مجموعہ اور خود کار پیغامات سنبھالنے والے EOS.IO زکی معاہدوں کی وضاحت کرتا ہے.

## کارکردگی پر مبنی اجازاتی انتظام

اجازاتی انتظام میں اس بات کی تصدیق ہوتی ہے کہ پیغام صحیح ہے یا غلط. اجازاتی انتظام کی سب سے سادہ شکل لین دین کی جانچ پڑتال دستخط دیکھنا، بشرطیکہ دستخط پہلے سے ہی پتہ ہو. عام طور پر اختیارات کسی فرد یا گروہ کو دیے جاتے ہیں اور یہ اکثر کمپارٹمنٹالایزڈ ہوتے ہیں. EOS.IO سافٹویئر ایک مدددل انتظامی اجازت نامہ فراہم کرتا ہے جو اکاؤنٹس کو اعلٰی سطح پر کنٹرول کرتا ہے کہ ان کا کب کہا کرنا ہے.

اجازات اور تصدیق کا انتظام کرنے کے لیے ضروری ہے کہ ان کو تجارتی اور منطق اپلیکیشنز سے علاحدہ رکھا جائے. عام طور پر اس سے آلات کو بڈھاوا دینے اور اجازت کا انتظام کرنے کے غرض سے اور کارکردگی کا مظاہرہ کرنے کے لیے اہم مواقع فراہم کرنے کے لیے تیار ہو.

ہر ایک اکاؤنٹ کو کنٹرول کرنے کے لیے نجی کلیدوں کی مجموعہ بندی اور مخصوص چابیاں ہوتی ہے. یہ اس بات کی عکاسی کرتا ہے کہ کس طرح اجازت حقیقت میں منظم کیے جاتے ہیں اور کس طرح موروثی اتھارٹی ساخت پیدا کرتا ہے، اور کثیرصارفین والے فنڈز پر پہلے سے زیادہ کنٹرول آسان بناتا ہے. متعدد صارفوں کا کنٹرول سیکورٹی کو اکیلے امداد کرتا ہے، اور جب صحیح استعمال ہوتا ہے تو چوری وغیرہ کے امکانات بہت حد تک نکال دیے جاتے ہیں.

EOS.IO سافٹویئر اکاونٹ کیز اور اکاونٹ سے جانے والی مخصوص پیغامات کی وضاحت کرنے کی اجازت دیتا ہے. مثال کے طور پر، یہ ممکن ہے کہ ایک صارف کے پاس سماجی میڈیا کے لیے ایک اور دوسرے ایکسچینج کے لیے الگ سے رسائی ممکن ہو. یہ بھی ممکن ہے کہ دوسرے صارفین کو اجازت دی جائے کہ وہ دوسرے صارفین کے اکاؤنٹس کو چلایے بغیر تفویض کرے یہ عمل ممکن ہو.

### نامی اجازت کی حدیں-

<img align="right" src="http://eos.io/wpimg/diagram3.png" width="228.395px" height="300px" />

EOS.IO سافٹویئر کے استعمال سے، اکاؤنٹ نامی اجازات کو بیان کرسکتے ہیں جو اوپری سطح کے اجازت سےنکالی گئی ہو. ہر نامی اجازتی حد ایک اتھارٹی کو بیان کرتی ہے؛ایک اتھارٹی متعدد دستخطوں کی جانچ کے لیے چابی کی طرح اور /یا دوسرے نامی اکاؤنٹس کی اجازاتی حدیں. For example, an account's "Friend" permission level can be set for the account to be controlled equally by any of the account's friends.

Another example is the Steem blockchain which has three hard-coded named permission levels: owner, active, and posting. The posting permission can only perform social actions such as voting and posting, while the active permission can do everything except change the owner. The owner permission is meant for cold storage and is able to do everything. The EOS.IO software generalizes this concept by allowing each account holder to define their own hierarchy as well as the grouping of actions.

### Named Message Handler Groups

The EOS.IO software allows each account to organize its own message handlers into named and nested groups. These named message handler groups can be referenced by other accounts when they configure their permission levels.

The highest level message handler group is the account name and the lowest level is the individual message type being received by the account. These groups can be referenced like so: **@accountname.groupa.subgroupb.MessageType**.

Under this model it is possible for an exchange contract to group order creation and canceling separately from deposit and withdraw. This grouping by the exchange contract is a convenience for users of the exchange.

### Permission Mapping

EOS.IO software allows each account to define a mapping between a Named Message Handler Group of any account and their own Named Permission Level. For example, an account holder could map the account holder's social media application to the account holder's "Friend" permission group. With this mapping, any friend could post as the account holder on the account holder's social media. Even though they would post as the account holder, they would still use their own keys to sign the message. This means it is always possible to identify which friends used the account and in what way.

### Evaluating Permissions

When delivering a message of type "**Action**", from **@alice** to **@bob** the EOS.IO software will first check to see if **@alice** has defined a permission mapping for **@bob.groupa.subgroup.Action**. If nothing is found then a mapping for **@bob.groupa.subgroup** then **@bob.groupa**, and lastly **@bob** will be checked. If no further match is found, then the assumed mapping will be to the named permission group **@alice.active**.

Once a mapping is identified then signing authority is validated using the threshold multi-signature process and the authority associated with the named permission. If that fails, then it traverses up to the parent permission and ultimately to the owner permission, **@alice.owner**.

<img align="center" src="http://eos.io/wpimg/diagram2grayscale2.jpg" width="845.85px" height="500px" />

#### Default Permission Groups

The EOS.IO technology also allows all accounts to have an "owner" group which can do everything, and an "active" group which can do everything except change the owner group. All other permission groups are derived from "active".

#### Parallel Evaluation of Permissions

The permission evaluation process is "read-only" and changes to permissions made by transactions do not take effect until the end of a block. This means that all keys and permission evaluation for all transactions can be executed in parallel. Furthermore, this means that a rapid validation of permission is possible without starting the costly application logic that would have to be rolled back. Lastly, it means that transaction permissions can be evaluated as pending transactions are received and do not need to be re-evaluated as they are applied.

All things considered, permission verification represents a significant percentage of the computation required to validate transactions. Making this a read-only and trivially parallelizable process enables a dramatic increase in performance.

When replaying the blockchain to regenerate the deterministic state from the log of messages there is no need to evaluate the permissions again. The fact that a transaction is included in a known good block is sufficient to skip this step. This dramatically reduces the computational load associated with replaying an ever growing blockchain.

## Messages with Mandatory Delay

Time is a critical component of security. In most cases, it is not possible to know if a private key has been stolen until it has been used. Time based security is even more critical when people have applications that require keys be kept on computers connected to the internet for daily use. The EOS.IO software enables application developers to indicate that certain messages must wait a minimum period of time after being included in a block before they can be applied. During this time they can be cancelled.

Users can then receive notice via email or text message when one of these messages is broadcast. If they did not authorize it, then they can use the account recovery process to recover their account and retract the message.

The required delay depends upon how sensitive an operation is. Paying for a coffee can have no delay and be irreversible in seconds, while buying a house may require a 72 hour clearing period. Transferring an entire account to new control may take up to 30 days. The exact delays chosen are up to application developers and users.

## Recovery from Stolen Keys

The EOS.IO software provides users a way to restore control of their account when their keys are stolen. An account owner can use any owner key that was active in the last 30 days along with approval from their designated account recovery partner to reset the owner key on their account. The account recovery partner cannot reset control of the account without the help of the owner.

There is nothing for the hacker to gain by attempting to go through the recovery process because they already "control" the account. Furthermore, if they did go through the process, the recovery partner would likely demand identification and multi-factor authentication (phone and email). This would likely compromise the hacker or gain the hacker nothing in the process.

This process is also very different from a simple multi-signature arrangement. With a multi-signature transaction, there is another company that is party to every transaction that is executed, but with the recovery process the agent is only a party to the recovery process and has no power over the day-to-day transactions. This dramatically reduces costs and legal liabilities for everyone involved.

# Deterministic Parallel Execution of Applications

Blockchain consensus depends upon deterministic (reproducible) behavior. This means all parallel execution must be free from the use of mutexes or other locking primitives. Without locks there must be some way to guarantee that all accounts can only read and write their own private database. It also means that each account processes messages sequentially and that parallelism will be at the account level.

In an EOS.IO software-based blockchain, it is the job of the block producer to organize message delivery into independent threads so that they can be evaluated in parallel. The state of each account depends only upon the messages delivered to it. The schedule is the output of a block producer and will be deterministically executed, but the process for generating the schedule need not be deterministic. This means that block producers can utilize parallel algorithms to schedule transactions.

Part of parallel execution means that when a script generates a new message it does not get delivered immediately, instead it is scheduled to be delivered in the next cycle. The reason it cannot be delivered immediately is because the receiver may be actively modifying its own state in another thread.

## Minimizing Communication Latency

Latency is the time it takes for one account to send a message to another account and then receive a response. The goal is to enable two accounts to exchange messages back and forth within a single block without having to wait 3 seconds between each message. To enable this, the EOS.IO software divides each block into cycles. Each cycle is divided into threads and each thread contains a list of transactions. Each transaction contains a set of messages to be delivered. This structure can be visualized as a tree where alternating layers are processed sequentially and in parallel.

        Block
    
          Cycles (sequential)
    
            Threads (parallel)
    
              Transactions (sequential)
    
                Messages (sequential)
    
                  Receiver and Notified Accounts (parallel)
    

Transactions generated in one cycle can be delivered in any subsequent cycle or block. Block producers will keep adding cycles to a block until the maximum wall clock time has passed or there are no new generated transactions to deliver.

It is possible to use static analysis of a block to verify that within a given cycle no two threads contain transactions that modify the same account. So long as that invariant is maintained a block can be processed by running all threads in parallel.

## Read-Only Message Handlers

Some accounts may be able to process a message on a pass/fail basis without modifying their internal state. If this is the case then these handlers can be executed in parallel so long as only read-only message handlers for a particular account are included in one or more threads within a particular cycle.

## Atomic Transactions with Multiple Accounts

Sometimes it is desirable to ensure that messages are delivered to and accepted by multiple accounts atomically. In this case both messages are placed in one transaction and both accounts will be assigned the same thread and the messages applied sequentially. This situation is not ideal for performance and when it comes to "billing" users for usage, they will get billed by the number of unique accounts referenced by a transaction.

For performance and cost reasons it is best to minimize atomic operations involving two or more heavily utilized accounts.

## Partial Evaluation of Blockchain State

Scaling blockchain technology necessitates that components are modular. Everyone should not have to run everything, especially if they only need to use a small subset of the applications.

An exchange application developer runs full nodes for the purpose of displaying the exchange state to its users. This exchange application has no need for the state associated with social media applications. EOS.IO software allows any full node to pick any subset of applications to run. Messages delivered to other applications are safely ignored because an application's state is derived entirely from the messages that are delivered to it.

This has some significant implications on communication with other accounts. Most significantly it cannot be assumed that the state of the other account is accessible on the same machine. It also means that while it is tempting to enable "locks" that allow one account to synchronously call another account, this design pattern breaks down if the other account is not resident in memory.

All state communication among accounts must be passed via messages included in the blockchain.

## Subjective Best Effort Scheduling

The EOS.IO software cannot obligate block producers to deliver any message to any other account. Each block producer makes their own subjective measurement of the computational complexity and time required to process a transaction. This applies whether a transaction is generated by a user or automatically by a script.

On a launched blockchain adopting the EOS.IO software, at a network level all transactions are billed a fixed computational bandwidth cost regardless of whether it took .01ms or a full 10 ms to execute it. However, each individual block producer using the software may calculate resource usage using their own algorithm and measurements. When a block producer concludes that a transaction or account has consumed a disproportionate amount of the computational capacity they simply reject the transaction when producing their own block; however, they will still process the transaction if other block producers consider it valid.

In general so long as even 1 block producer considers a transaction as valid and under the resource usage limits then all other block producers will also accept it, but it may take up to 1 minute for the transaction to find that producer.

In some cases a producer may create a block that includes transactions that are an order of magnitude outside of acceptable ranges. In this case the next block producer may opt to reject the block and the tie will be broken by the third producer. This is no different than what would happen if a large block caused network propagation delays. The community would notice a pattern of abuse and eventually remove votes from the rogue producer.

This subjective evaluation of computational cost frees the blockchain from having to precisely and deterministically measure how long something takes to run. With this design there is no need to precisely count instructions which dramatically increases opportunities for optimization without breaking consensus.

# Token Model and Resource Usage

**PLEASE NOTE: CRYPTOGRAPHIC TOKENS REFERRED TO IN THIS WHITE PAPER REFER TO CRYPTOGRAPHIC TOKENS ON A LAUNCHED BLOCKCHAIN THAT ADOPTS THE EOS.IO SOFTWARE. THEY DO NOT REFER TO THE ERC-20 COMPATIBLE TOKENS BEING DISTRIBUTED ON THE ETHEREUM BLOCKCHAIN IN CONNECTION WITH THE EOS TOKEN DISTRIBUTION.**

All blockchains are resource constrained and require a system to prevent abuse. With a blockchain that uses EOS.IO software, there are three broad classes of resources that are consumed by applications:

1. Bandwidth and Log Storage (Disk);
2. Computation and Computational Backlog (CPU); and
3. State Storage (RAM).

Bandwidth and computation have two components, instantaneous usage and long-term usage. A blockchain maintains a log of all messages and this log is ultimately stored and downloaded by all full nodes. With the log of messages it is possible to reconstruct the state of all applications.

The computational debt is calculations that must be performed to regenerate state from the message log. If the computational debt grows too large then it becomes necessary to take snapshots of the blockchain's state and discard the blockchain's history. If computational debt grows too quickly then it may take 6 months to replay 1 year worth of transactions. It is critical, therefore, that the computational debt be carefully managed.

Blockchain state storage is information that is accessible from application logic. It includes information such as order books and account balances. If the state is never read by the application then it should not be stored. For example, blog post content and comments are not read by application logic so they should not be stored in the blockchain's state. Meanwhile the existence of a post/comment, the number of votes, and other properties do get stored as part of the blockchain's state.

Block producers publish their available capacity for bandwidth, computation, and state. The EOS.IO software allows each account to consume a percentage of the available capacity proportional to the amount of tokens held in a 3-day staking contract. For example, if a blockchain based on the EOS.IO software is launched and if an account holds 1% of the total tokens distributable pursuant to that blockchain, then that account has the potential to utilize 1% of the state storage capacity.

Adopting the EOS.IO software on a launched blockchain means bandwidth and computational capacity are allocated on a fractional reserve basis because they are transient (unused capacity cannot be saved for future use). The algorithm used by EOS.IO software is similar to the algorithm used by Steem to rate-limit bandwidth usage.

## Objective and Subjective Measurements

As discussed earlier, instrumenting computational usage has a significant impact on performance and optimization; therefore, all resource usage constraints are ultimately subjective and enforcement is done by block producers according to their individual algorithms and estimates.

That said, there are certain things that are trivial to measure objectively. The number of messages delivered and the size of the data stored in the internal database are cheap to measure objectively. The EOS.IO software enables block producers to apply the same algorithm over these objective measures but may choose to apply stricter subjective algorithms over subjective measurements.

## Receiver Pays

Traditionally, it is the business that pays for office space, computational power, and other costs required to run the business. The customer buys specific products from the business and the revenue from those product sales is used to cover the business costs of operation. Similarly, no website obligates its visitors to make micropayments for visiting its website to cover hosting costs. Therefore, decentralized applications should not force its customers to pay the blockchain directly for the use of the blockchain.

A launched blockchain that uses the EOS.IO software does not require its users to pay the blockchain directly for its use and therefore does not constrain or prevent a business from determining its own monetization strategy for its products.

## Delegating Capacity

A holder of tokens on a blockchain launched adopting the EOS.IO software who may not have an immediate need to consume all or part of the available bandwidth, can give or rent such unconsumed bandwidth to others; the block producers running EOS.IO software on such blockchain will recognize this delegation of capacity and allocate bandwidth accordingly.

## Separating Transaction costs from Token Value

One of the major benefits of the EOS.IO software is that the amount of bandwidth available to an application is entirely independent of any token price. If an application owner holds a relevant number of tokens on a blockchain adopting EOS.IO software, then the application can run indefinitely within a fixed state and bandwidth usage. In such case, developers and users are unaffected from any price volatility in the token market and therefore not reliant on a price feed. In other words, a blockchain that adopts the EOS.IO software enables block producers to naturally increase bandwidth, computation, and storage available per token independent of the token's value.

A blockchain using EOS.IO software also awards block producers tokens every time they produce a block. The value of the tokens will impact the amount of bandwidth, storage, and computation a producer can afford to purchase; this model naturally leverages rising token values to increase network performance.

## State Storage Costs

While bandwidth and computation can be delegated, storage of application state will require an application developer to hold tokens until that state is deleted. If state is never deleted then the tokens are effectively removed from circulation.

Every user account requires a certain amount of storage; therefore, every account must maintain a minimum balance. As storage capacity of the network increases this minimum required balance will fall.

## Block Rewards

A blockchain that adopts the EOS.IO software will award new tokens to a block producer every time a block is produced. In these circumstances, the number of tokens created is determined by the median of the desired pay published by all block producers. The EOS.IO software may be configured to enforce a cap on producer awards such that the total annual increase in token supply does not exceed 5%.

## Community Benefit Applications

In addition to electing block producers, pursuant to a blockchain based on the EOS.IO software, users can elect 3 community benefit applications also known as smart contracts. These 3 applications will receive tokens of up to a configured percent of the token supply per annum minus the tokens that have been paid to block producers. These smart contracts will receive tokens proportional to the votes each application has received from token holders. The elected applications or smart contracts can be replaced by newly elected applications or smart contracts by token holders.

# Governance

Governance is the process by which people reach consensus on subjective matters that cannot be captured entirely by software algorithms. An EOS.IO software-based blockchain implements a governance process that efficiently directs the existing influence of block producers. Absent a defined governance process, prior blockchains relied ad hoc, informal, and often controversial governance processes that result in unpredictable outcomes.

A blockchain based on the EOS.IO software recognizes that power originates with the token holders who delegate that power to the block producers. The block producers are given limited and checked authority to freeze accounts, update defective applications, and propose hard forking changes to the underlying protocol.

Embedded into the EOS.IO software is the election of block producers. Before any change can be made to the blockchain these block producers must approve it. If the block producers refuse to make changes desired by the token holders then they can be voted out. If the block producers make changes without permission of the token holders then all other non-producing full-node validators (exchanges, etc) will reject the change.

## Freezing Accounts

Sometimes a smart contact behaves in an aberrant or unpredictable manner and fails to perform as intended; other times an application or account may discover an exploit that enables it to consume an unreasonable amount of resources. When such issues inevitably occur, the block producers have the power to rectify such situations.

The block producers on all blockchains have the power to select which transactions are included in blocks which gives them the ability to freeze accounts. A blockchain using EOS.IO software formalizes this authority by subjecting the process of freezing an account to a 17/21 vote of active producers. If the producers abuse the power they can be voted out and an account will be unfrozen.

## Changing Account Code

When all else fails and an "unstoppable application" acts in an unpredictable manner, a blockchain using EOS.IO software allows the block producers to replace the account's code without hard forking the entire blockchain. Similar to the process of freezing an account, this replacement of the code requires a 17/21 vote of elected block producers.

## Constitution

The EOS.IO software enables blockchains to establish a peer-to-peer terms of service agreement or a binding contract among those users who sign it, referred to as a "constitution". The content of this constitution defines obligations among the users which cannot be entirely enforced by code and facilitates dispute resolution by establishing jurisdiction and choice of law along with other mutually accepted rules. Every transaction broadcast on the network must incorporate the hash of the constitution as part of the signature and thereby explicitly binds the signer to the contract.

The constitution also defines the human-readable intent of the source code protocol. This intent is used to identify the difference between a bug and a feature when errors occur and guides the community on what fixes are proper or improper.

## Upgrading the Protocol & Constitution

The EOS.IO software defines a process by which the protocol as defined by the canonical source code and its constitution, can be updated using the following process:

1. Block producers propose a change to the constitution and obtains 17/21 approval.
2. Block producers maintain 17/21 approval for 30 consecutive days.
3. All users are required to sign transactions using the hash of the new constitution.
4. Block producers adopt changes to the source code to reflect the change in the constitution and propose it to the blockchain using the hash of a git commit.
5. Block producers maintain 17/21 approval for 30 consecutive days.
6. Changes to the code take effect 7 days later, giving all full nodes 1 week to upgrade after ratification of the source code.
7. All nodes that do not upgrade to the new code shut down automatically.

By default configuration of the EOS.IO software, the process of updating the blockchain to add new features takes 2 to 3 months, while updates to fix non-critical bugs that do not require changes to the constitution can take 1 to 2 months.

### Emergency Changes

The block producers may accelerate the process if a software change is required to fix a harmful bug or security exploit that is actively harming users. Generally speaking it could be against the constitution for accelerated updates to introduce new features or fix harmless bugs.

# Scripts & Virtual Machines

The EOS.IO software will be first and foremost a platform for coordinating the delivery of authenticated messages to accounts. The details of scripting language and virtual machine are implementation specific details that are mostly independent from the design of the EOS.IO technology. Any language or virtual machine that is deterministic and properly sandboxed with sufficient performance can be integrated with the EOS.IO software API.

## Schema Defined Messages

All messages sent between accounts are defined by a schema which is part of the blockchain consensus state. This schema allows seamless conversion between binary and JSON representation of the messages.

## Schema Defined Database

Database state is also defined using a similar schema. This ensures that all data stored by all applications is in a format that can be interpreted as human readable JSON but stored and manipulated with the efficiency of binary.

## Separating Authentication from Application

To maximize parallelization opportunities and minimize the computational debt associated with regenerating application state from the transaction log, EOS.IO software separates validation logic into three sections:

1. Validating that a message is internally consistent;
2. Validating that all preconditions are valid; and
3. Modifying the application state.

Validating the internal consistency of a message is read-only and requires no access to blockchain state. This means that it can be performed with maximum parallelism. Validating preconditions, such as required balance, is read-only and therefore can also benefit from parallelism. Only modification of application state requires write access and must be processed sequentially for each application.

Authentication is the read-only process of verifying that a message can be applied. Application is actually doing the work. In real time both calculations are required to be performed, however once a transaction is included in the blockchain it is no longer necessary to perform the authentication operations.

## Virtual Machine Independent Architecture

It is the intention of the EOS.IO software-based blockchain that multiple virtual machines can be supported and new virtual machines added over time as necessary. For this reason, this paper will not discuss the details of any particular language or virtual machine. That said, there are two virtual machines that are currently being evaluated for use with an EOS.IO software-based blockchain.

### Web Assembly (WASM)

Web Assembly is an emerging web standard for building high performance web applications. With a few small modifications Web Assembly can be made deterministic and sandboxed. The benefit of Web Assembly is the widespread support from industry and that it enables contracts to be developed in familiar languages such as C or C++.

Ethereum developers have already begun modifying Web Assembly to provide suitable sandboxing and determinism in with their [Ethereum flavored Web Assembly (WASM)](https://github.com/ewasm/design). This approach can be easily adapted and integrated with EOS.IO software.

### Ethereum Virtual Machine (EVM)

This virtual machine has been used for most existing smart contracts and could be adapted to work within an EOS.IO blockchain. It is conceivable that EVM contracts could be run within their own sandbox inside an EOS.IO software-based blockchain and that with some adaptation EVM contracts could communicate with other EOS.IO software blockchain applications.

# Inter Blockchain Communication

EOS.IO software is designed to facilitate inter-blockchain communication. This is achieved by making it easy to generate proof of message existence and proof of message sequence. These proofs combined with an application architecture designed around message passing enables the details of inter-blockchain communication and proof validation to be hidden from application developers.

<img align="right" src="http://eos.io/wpimg/Diagram1.jpg" width="362.84px" height="500px" />

## Merkle Proofs for Light Client Validation (LCV)

Integrating with other blockchains is much easier if clients do not need to process all transactions. After all, an exchange only cares about transfers in and out of the exchange and nothing more. It would also be ideal if the exchange chain could utilize lightweight merkle proofs of deposit rather than having to trust its own block producers entirely. At the very least a chain's block producers would like to maintain the smallest possible overhead when synchronizing with another blockchain.

The goal of LCV is to enable the generation of relatively light-weight proof of existence that can be validated by anyone tracking a relatively light-weight data set. In this case the objective is to prove that a particular transaction was included in a particular block and that the block is included in the verified history of a particular blockchain.

Bitcoin supports validation of transactions assuming all nodes have access to the full history of block headers which amounts to 4MB of block headers per year. At 10 transactions per second, a valid proof requires about 512 bytes. This works well for a blockchain with a 10 minute block interval, but is no longer "light" for blockchains with a 3 second block interval.

The EOS.IO software enables lightweight proofs for anyone who has any irreversible block header after the point in which the transaction was included. Using the hash-linked structure shown below it is possible to prove the existence of any transaction with a proof less than 1024 bytes in size. If it is assumed that validating nodes are keeping up with all block headers in the past day (2 MB of data), then proving these transactions will only require proofs 200 bytes long.

There is little incremental overhead associated with producing blocks with the proper hash-linking to enable these proofs which means there is no reason not to generate blocks this way.

When it comes time to validate proofs on other chains there are a wide variety of time/ space/ bandwidth optimizations that can be made. Tracking all block headers (420 MB/year) will keep proof sizes small. Tracking only recent headers can offer a trade off between minimal long-term storage and proof size. Alternatively, a blockchain can use a lazy evaluation approach where it remembers intermediate hashes of past proofs. New proofs only have to include links to the known sparse tree. The exact approach used will necessarily depend upon the percentage of foreign blocks that include transactions referenced by merkle proof.

After a certain density of interconnectedness it becomes more efficient to simply have one chain contain the entire block history of another chain and eliminate the need for proofs all together. For performance reasons, it is ideal to minimize the frequency of inter-chain proofs.

## Latency of Interchain Communication

When communicating with another outside blockchain, block producers must wait until there is 100% certainty that a transaction has been irreversibly confirmed by the other blockchain before accepting it as a valid input. Using an EOS.IO software-based blockchain and DPOS with 3 second blocks and 21 producers, this takes approximately 45 seconds. If a chain's block producers do not wait for irreversibility it would be like an exchange accepting a deposit that was later reversed and could impact the validity of the blockchain's consensus.

## Proof of Completeness

When using merkle proofs from outside blockchains, there is a significant difference between knowing that all transactions processed are valid and knowing that no transactions have been skipped or omitted. While it is impossible to prove that all of the most recent transactions are known, it is possible to prove that there have been no gaps in the transaction history. The EOS.IO software facilitates this by assigning a sequence number to every message delivered to every account. A user can use these sequence numbers to prove that all messages intended for a particular account have been processed and that they were processed in order.

# Conclusion

The EOS.IO software is designed from experience with proven concepts and best practices, and represents fundamental advancements in blockchain technology. The software is part of a holistic blueprint for a globally scalable blockchain society in which decentralised applications can be easily deployed and governed.