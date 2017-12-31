# EOS. IOایک تکنیکی سفیر کاغذ ھے -

**26 جون 2017**

**خلاصہ <0\> EOS. IO سوفٹویر ایک نیا پیمانہ کاری بلاکچین کو متعارف کرتا ہے جو ڈیسنٹرالایزڈ اپلیکیشنز کو عمودی اور افقی سکیلنگ کے لیے معیاری بناتا ہے -. یہ اپلیکیشن جس پر ڈیولپ کر سکتے ھے ایک آپریٹنگ سسٹم کی طرح تعمیر کی تشکیل کی طرف سے خاصل ھوتا ھے -. یہ سافٹویئر فراہم کرتا ھے اکاؤنٹ، توسیق کاری، ڈیٹابیس، ایسنچورنس، ابلاغ اور اپلیکیشنز کے سینکڑوں کروڑ CPU کلسٹرس فراہم کرتا ہے -. بنای گیی تکنیک ایک بلاکچین فن تعمیر ھے جو کروڑوں کا لین دین فی سیکنڈ کرتا ھے، صارف کا فیس ختم کرتا ہے اور ڈیسنٹرالایزڈ اپلیکیشنز کی فوری اور آسان ڈیپلایمنٹ کرتا ہے -.</p> 

**مہربانی کر کے یاد رہے:اس سفید کاغذ میں جو کرپٹوگرافک ٹوکن ھے وہ کرپٹوگرافک ٹوکنز دراصل EOS. IO سوفٹویر سے منسلک ہے -. ان کا تعلق ERC-20 ٹوکنز کے ساتھ نہیں ھے اتھیریم بلاکچین EOS ٹوکن تقسیم کے حوالے تقسیم ھو نے کی وجہ سے ھوتا ھے.**

کاپیرایٹ © 2017 پھلا بلاگ

اس سفید کاغذ پر بغیر کسی اجازت کے کوئی بھی اس کا استعمال، اس کی تقسیم غیر تجارتی کام یا تعلیماتی مقاصد کے استعمال میں لا سکتے ہیں بشرطیکہ اصل زرایع اور کابل اطلاق کاپی رائیٹ کا حوالہ دیا گیا ھو -.

<0\>DISCLAIMER EOS. IO تکنیکی سفیر کاغذ صرف معلوماتی مقاصد کے لیے ھے. پھلا بلاک کسی بھی درستگی کی ضمانت نہیں دیتا ھے اور یہ سفید کاغذ بالکل اپنی اصل شکل میں موجود رہتا ہے. پھلا بلاک تمام معروضات کو واضح طور پر داسکلایم اور وارنٹی، ایکسپریس، مضمر، قانونی یا بصورت دیگر جو کچھ بھی، بشمول تک محدود نہیں ہے -1))مفت پزیرائی کی وارنٹی، کسی اھم مقصد یا مناسبت کے بارے میں، استعمال کے لیے معضونیت کا عنوان یا نانفراگمنٹ -2))یہ سفید کاغذ غلطیوں سے بالکل مفت ھے؛ اور (٣)ایسے مشمولات دوسرے لوگوں کے حقوق کی خلاف ورزی نہیں کرتا ھے -. پھلا بلاک اور اس کے ساتھ منسلک کسی بھی چیز کی زمہ داری سے کسی بھی قسم کے استعمال کرنے کے لیے حوالے سے پیدا ہونے والے نقصانات کے لیے ھو گا یا یہ سفید کاغذ یا مواد کسی پر منحصر نہیں یھان تک کہ خطرات کے امناکات کا مشورہ دیا گیا ھو -. پہلے بلاک کی کسی بھی تقریب یا اس کے الحاق، یا فتگان براہ راست، یا بالواسطہ نتیجتاً، کمپنسیٹری، اتفاقی، اصل، مسالی، کسی خاص شخص کے لئے تعزیری، جواب دہ نہیں بناتا - اس سفید کاغذ میں کسی بھی کاروباری نقصان، آمدنی، منافع، ڑیٹا کے استعمال کی خیر گالی یا دوسرے طریقوں کی ضمانت دیتا ہے اور اس کے ساتھ منسلک چھوٹے چھوٹے نقصانات -.

- [پس منظر-](#background)
- [بلاکچین اپلیکیشنز کے لیے ضروریات-](#requirements-for-blockchain-applications) 
  - [یہ لاکھوں صارفین کی حمایت کرتا ہے-](#support-millions-of-users)
  - [مفت استعمال-](#free-usage)
  - [آسان اپگریڈ اور بگ کی وصولی-](#easy-upgrades-and-bug-recovery)
  - [قلیل چھپاو-](#low-latency)
  - [سلسلہ وار کارکردگی-](#sequential-performance)
  - [متوازی کارکردگی-](#parallel-performance)
- [اتفاق رائے الگورزم (ڈی پی او)](#consensus-algorithm-dpos) 
  - [تصدیق لین دین-](#transaction-confirmation)
  - [داو پر لین دین کو سبوت کے طور پر رکھنا ( ٹاپوس)-](#transaction-as-proof-of-stake-tapos)
- [اکاسنٹس-](#accounts) 
  - [پیغامات اور اس کے رھبر-](#messages--handlers)
  - [انتظامی اجازت پر مبنی-](#role-based-permission-management) 
    - [نامی اجازت کی حدیں-](#named-permission-levels)
    - [نامی پیغامات نیٹ کار گروہ-](#named-message-handler-groups)
    - [نقشہ کاری کی اجازت-](#permission-mapping)
    - [جایزہ لینے کی اجازت-](#evaluating-permissions) 
      - [طے شدہ اجازاتی گروہ-](#default-permission-groups)
      - [متوازی جایزہ کی اجازت-](#parallel-evaluation-of-permissions)
  - [پیغامات لازمی تاخیر کے ساتھ-](#messages-with-mandatory-delay)
  - [چوری شدہ کیز سے وصولی-](#recovery-from-stolen-keys)
- [اپلیکیشنز کے بیچ متوازی نفاذ-](#deterministic-parallel-execution-of-applications) 
  - [مواصلاتی چھپاو کو کم کرنا-](#minimizing-communication-latency)
  - [صرف ھینڈلرس کے انتخابات پڈھنا-](#read-only-message-handlers)
  - [ایک سے زائد اکاؤنٹ سے اٹمی لین دین-](#atomic-transactions-with-multiple-accounts)
  - [بلاکچین کی صورتحال کی جزوی تشخیص-](#partial-evaluation-of-blockchain-state)
  - [بھترین کوشش اس کے لیے شرط ہے-](#subjective-best-effort-scheduling)
- [وسائل کا استعمال اور ٹوکن کا ماڈل-](#token-model-and-resource-usage) 
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
    - [ویب اسمبلی WASIM))](#web-assembly-wasm)
    - [مجازی اتھیریم مشین EVM))](#ethereum-virtual-machine-evm)
- [بلاکچین کی اندرونی گفتگو](#inter-blockchain-communication) 
  - [لائیٹ کلائینٹ جواز کے لیے مارکلی کے سبوت LVC))](#merkle-proofs-for-light-client-validation-lcv)
  - [اندرونی چین اور ابلاغ کا چھپاو](#latency-of-interchain-communication)
  - [اختتامی سبوت](#proof-of-completeness)
- [مقصد](#conclusion)

# پس منظر

بلاکچین تکنیک bitcoin رقم کے ساتھ سال 2008 میں متعارف کی گئی، اور اس کے بعد تاجروں اور ڈیولپرس نے تکنیک کو عام کرنے میں اور تمام بلاکچین کو ایک ہی راستہ لانے کے لیے اپنا تعاون بخوبی دیا ہے.

وھی بلاکچین پلیٹ فارم کی ایک بڑی تعداد نے فنکشنل ڈیسنٹرالایزڈ اپلیکیشنز کے تعاون میں کافی جدوجہد کی، جبکہ مخصوص بلاکچین اپلیکیشنز جیسے کہ بٹشیر ڈیسنٹرالایزڈ ایکسچینج 2014 اور سٹیم سوشل میڈیا پلیٹفارم 2016 بھت زیادہ استعمال ہونے والی بلاکچین بن گیی ھے جس کا استعمال روزانہ ھزاروں صارف کرتے ہیں. انھوں نے یہ ایک سیکنڈ میں ھزاروں لین دین کرنا جیسے کارکردگی میں اضافہ کرنے سے حاصل کیا، اخفاء کو ١. ٥ سیکنڈ سے کم کرنا، فیس ختم کرنا، اور سارفین کو ایسی خدمات دینا جو کہ موجودہ مرکزی خدمات فراہم کرتا ہے.

موجودہ بلاکچین پلیٹ فارم بھاری فیس کے دباؤ میں ھے جس وجہ سے صارفین اس کو بڈے پیمانے پر استعمال کرنے سے کتراتے ھے.

# بلاکچین اپلیکیشنز کے لیے ضروریات

بڈے پیمانے پر اس کا استعمال کرنے کے لئے بلاکچین کو ایک اچھے پلیٹ فارم کی ضرورت ہے جو کافی لچک دار ھو اس کے لیے مندرجہ ذیل عکوپمنٹس درکار ہے:

## لاکھوں صارفین کی حمایت حاصل کرنا

سوشل نیٹ ورکنگ ویب سائٹ جیسے ای بے، عبر، AirBnB, اور فیس بک کا کاروبار تباہ کرنے کے لیے، بلاکچین کے زیر اہتمام لاکھوں لوگ اس کا استعمال کرتے ہیں. کچھ معاملات میں اپلیکیشن تب تک کام نہیں کرتا جب تک صارفین کی ایک مخصوص تعداد نا آجائے بھت؛زیادہ اھمیت کا حامل ہے.

## مفت استعمال

اپلیکیشن ڈیولپرس کو لوگوں کو مفت خدمات فراہم کرنے کے لیے لچک کی ضرورت ہے؛ سارفین کو پلیٹ فارم کا استعمال کرنے اور اس سے فائدہ اٹھانے کے لیے فیس ادا نہیں کرنی ھو گی. ایسا بلاکچین پلیٹ فارم جو صارف کے لیے مفت دستیاب ھو سارفین اس کا استعمال بڈی؛تیزی سے کریں گے. کاروباری اور ڈیولپرس اس کے بعد موثر حکمت عملی بنا سکتے ہیں.

## آسان ترقی اور بگ کی وصولی

کاروبار پر مبنی تعمیراتی پلاکچین اپلیکیشنز کو بڈھاوا دینے کے لیے اپلیکیشنز میں لچک کی ضرورت ہے-.

تمام غیر معلوماتی سافٹویئر کا انحصار بگ پر ھے، ختاکہ سب سے زیادہ سختی کی باقاعدہ تصدیق سے ھے بگ کو سنبھالنے لیے پلیٹفارم کا مضبوط ھونا کافی ضروری ھے.

## قلیل چھپاو

صارف کو بھتر خدمات فراہم کرنے کے لیے چاہیے کہ جلد سے جلد جانکاری حاصل ھو. لمبی تاخیر صارفین کو پریشانی میں ڈالتی ھے اور بلاکچین کو پھلے سے چلنے والی بلاکچین اپلیکیشنز کے مقابلے میں کم اھمیت حاصل ھوتی ھے.

## سلسلہ وار کارکردگی

کچھ اپلیکیشنز ایسی ھے جن کو ھم صرف جو ترتیب پر منحصر اقدامات کی وجہ سے متوازی الگورزم کے ساتھ لاگو نہیں کیا جاسکتا ہے. اپلیکیشنز جیسے کہ ایکسچینج بہت زیادہ ترتیبات کا حامل ہے کارکردگی کا مظاہرہ کرنے کے لیے لھزا اس کے لیے تیز تربیاتی پلیٹ فارم کی ضرورت ہے.

## متوازی کارکردگی

بڈے پیمانے کی اپلیکیشنز کام تقسیم کرنے کے لیے متعدد CPU اور کمپیوٹرز کا استعمال کرتا ہے.

# بہ اتفاق رائے الگورزم DPOS))

EOS. IO سوفٹویر صرف ڈیسنٹرالایزڈ الگورزم کا استعمال کرتا ھے جو کارکردگی پر کھرا اتر سکے بلاکچین اپلیکیشنز پر، DPOS))<0\>کو سبوت کے طور پر استعمال کیا جاتا ہے. اس الگورزم کے تحت، جو لوگ EOS. IO ٹوکن اپناے ایک بلاکچین رکھیں سافٹویئر بلاک بنانے والے ایک مسلسل انتخابی عمل کے زریعے منتخب کر سکتے ہیں اور کسی کو بھی اس کا حقدار بنا سکتے ہیں اور تمام پرڈوسدز کو ووٹس پر مناسب بلاکس پیدا کرنے کا موقع دیا جائے گا. زاتی بلاکچین کا انتظام اور ٹوکن کا استعمال کرنے کے لیے اور عملے کو ھٹانے کے لیے استعمال کر سکتے ہیں.</p> 

EOS. IO سوفٹویر ھر تین سیکنڈ کے بعد پیدا ہونے والے بلاکس کو بناتا ھے اور ایک پڑوسی کو کسی بھی بلاک پر پیدا کرنے کے لیے اجازت حاصل ہے. بلاک اگر مکررہ وقت پر پیدا نہ ہو تو اس کے لیے سلاٹ سکپ کیا جاتا ہے. جب ایک سے زائد بلاک سکپ کیے جاتے ہیں تب بلاکچین میں چھے سیکنڈ کا ایک وقفہ دیا جاتا ہے.

EOS. IO سافٹویئر کے استعمال سے اکیس مرحلوں میں بلاکس تیار ہوتے ہیں. اکیسویں منفرد بلاک کے شروع میں پرڈوسدز کا انتخاب کیا جاتا ہے. ھر مرحلے کے بعد اوپری 20 منظور شدہ پرڈوسدز خود بہ خود انتخاب ھوتا ھے اور آخری پرڈوسدز متناسب کا ووٹ دوسرے پرڈوسدز، ان کی نسبت کا انتخاب کیا جاتا ہے. منتخب کیے گئے پرڈوسدز بلاک وقت سے ماخوذ کازبی تصادفی عمل کا استعمال کرتے ہوئے شفل کرتے ہیں. یہ ردوبدل اس مقصد کے لیے کی جاتی ہے تاکہ دوسرے پرڈوسدز کا توازن برقرار رکھا جا سکے-.

اگر ایک پرڈوسد ایک بلاک کو بنانا بھول جاتا ہے تو یہ بلاک چوبیس گھنٹوں کے اندر اس سے نکالا جاتا ہے تاکہ وہ بلاکچین بلاکس کی پیداوار شروع کرنے کے لیے اپنے ارادے ظاہر کرے وہ بڈے دھیان سے ہٹا دیے جاتے ہیں. یہ اس کی تصدیق کرتا ہے کہ نیٹورک آسانی سے چل جائے ناقابل اعتبار بلاکس کی پالیسیوں کو کم کرنے سے-.

عام حالات میں ایک DPOS بلاکچین بلاکس کو پیدا کرنے کے بجائے مقابلہ بلاک پرڈوسد تعاون کرتا ہے اور بلاکچین کسی فورکس کا ساتھ نہیں دیتا. اس عمل میں ایک فورک، بہ اتفاق رائے سے خود بخود سب سے طویل سلسلے کے لیے کھلے گی. یہ میٹرک اس وجہ سے کام کرتی ہے کیونکہ اس کی شرح جس پر بلاکس کے لیے ایک بلاکچین فورک شامل ہے وہی اجماع کی حصہ داری بلاک پرڈوسدز کے تناسب سے براہ راست جڈ جاتی ہے. دوسرے الفاض میں ہم نے کھ سکتے ہیں کہ، زیادہ فورک پرڈوسد رکھنے والی بلاکچین کم تعداد والی سافٹویئر پرڈوسدز کے مقابلے میں زیادہ بڈھاوا حاصل کرے گی. علاوہ ازیں، کوئی بھی بلاک پرڈوسد دو فورکس پر ایک ساتھ بلاک نہیں بنا سکتا. اگر کسی پرڈوسد کو ایسا کرتے ہوئے پایا گیا تو وہ اس سے باہر نکال دیا جائے گا. غلط کام کرنے والوں کو خود بخود باہر کیا جا سکتا ہے اگر دوبار پروڈکشن کی.

## تصدیق لین دین

بنیادی DPOS بلاکچین بلاک بنانے میں 100فیصد شراکت داری رکھتا ہے ایک ٹرانزکش کی تصدیق 99.9فیصد اوسط سے بڈا کاسٹ کا وقت 1.5سیکنڈ کے بعد سمجھا جا سکتا ہے-.

یہاں کچھ ایسے غیر معمولی واقعات موجود ہے جہاں ایک سافٹویئر بگ انٹرنیٹ بھیڈ، یا بغض پر مبنی بلاک پرڈوسد دو یا دو سے زائد فورکس پیدا کر دے گا. اس تصدیق کے لئیے کہ ٹرانزکش ناقابل تلافی ہے، 15 سے 21 بلاک پرڈوسدز کی طرف سے تصدیق کے لیے انتظار کرنا ایک گروہ انتخاب کر سکتا ہے. EOS.IO کنفگریشن پر مبنی بنیادی سافٹویئر، ان حالات میں 45 سیکنڈ کا وقت لیتا ہے. مگر پہلے سے طے شدہ نوڈز ایک بلاک کو 21 میں سے 15پرڈوسرز نا قابل طلافی کی طرف سے اس بات کی تصدیق پر غور کرے گا اور ایک فورک جو اس طرح کے بلاک کی لمبائی سے قطع نظر سے خارج کر دے گا.

یہ ایک گروہ کے لیے ممکن ہے کہ وہ صارفین کو ایک عقلیتی فورک پر9 سیکنڈ کے اندر اندر فورق کا آغاز کریں. After 2 consecutive missed blocks there is a 95% probability a node is on a minority fork. تین لگاتار مسڈ بلاکس کے بعد ایک عقلیتی فورک ہونے کی وجہ سے 99 فیصد یقینی ہے. ایک مظبوط پیشگوانہ ماڈل بنانے کے لیے مسڈ نوڑز کی جانکاری رکھنے کے بارے میں معلومات استعمال کرے گا، حالیہ شرکت کی شرح اور دوسرے خدشات مسالے تیزی سے آپریٹرز کی تنبیہ کسی غلطی پر پیدا کرنے کے لیے ممکن ہے.

ایسی تنبیہ کا پورا دارومدار اور جوابی عمل مکمل طور پر کاروباری لین دین کی نوعیت پر منحصر ہے مگر سب سے سادہ عمل15/ 21کنفرمیشن کا انتظار کرنا ہ. ے-.

## لین دین کو داو پر سبوت کے طور پر رکھنا TaPos))

The EOS.IO software requires every transaction to include the hash of a recent block header. This hash serves two purposes:

1. لین دین کے محولہ کو روکنے کے لیے فورکس بلاک نہیں روکتا ہے؛ اور
2. signals the network that a particular user and their stake are on a specific fork.

وقت کے ساتھ ساتھ تمام صارفین براہ راست بلاکچین کی تصدیق کرتا ہے جو جعلی لین دین جایزہ طریقہ سے منتقل کرنے کے قابل نہیں ہوگا جیسا کہ جعلی چین فورج کو مشکل بناتا ہے.

# اکاؤنٹ

EOS.IO سافٹویئر تمام اکاونٹس کو ایک منفرد انسانی قابلِ مطالعہ 2سے32 الفاظ پر مشتمل حروف کا حوالہ دیا گیا ھو. نام اکاؤنٹ کا بنانے والا منتخب کرتا ہے. اکاونٹس کی دیکھ بھال کے لیے تمام اکاونٹس میں اکاونٹ بناتے وقت ایک چھوٹی رقم رکھنا ضروری ہے. اکاونٹ کے نام ناموں کے درمیان مالک کا اکاونٹ @domainہےصارف صرف اسی domain پر اپنا اکاونٹ بنا سکتا ہے.

ایک ڑیسنٹرالایزڈ سیاق و سباق میں، اپلیکیشن ڈیولپر کو نیا اکاونٹ بنانے کے لیے ایک معمولی فیس ادا کرنی ہوگی. روایتی کاروباریوں نے پہلے ہی کافی پیسہ صارفین کے لیے اشتہارات کی شکل میں، مفت خدمات، وغیرہ. نیے بلاکچین اکاؤنٹ کو فنڈ کرنے کی قیمت مقابلہ میں غیر معمولی ہونی چاہیے. خوش قسمتی سے، ان صارفین کو اکاؤنٹ بنانے کی ضرورت نہیں جنھوں پہلے سے ہی دوسری اپلیکیشنز میں اپنا اکاونٹ بنایا ہو.

## پیغامات اور ان کی دیکھ بھال کرنے والے

ہر ایک اکاؤنٹ سے دوسرے اکاونٹ تک ساخت پیغامات پہنچانے جا سکتے ہیں اور موصول ہونے والے پیغامات سے نمٹنے کے لئے سکرپٹس کی وضاحت کی جاتی ہے. EOS.IO سافٹویئر ہر ایک اکاؤنٹ کو اس کا ایک مخصوص ڈیٹابیس دیتا ہے جن کو صرف پیغامات سنبھالنے والے استعمال کر سکتے ہیں. پیغامات سنبھالنے والے نوشوں کو بھی اکاونٹس میں منتقل کر دیا جا سکتا ہے. پیغامات کا مجموعہ اور خود کار پیغامات سنبھالنے والے EOS.IO زکی معاہدے کی وضاحت کرتا ہے.

## انتظامی اجازت پر مبنی

اجازاتی انتظام میں اس بات کی تصدیق ہوتی ہے کہ پیغام صحیح ہے یا غلط. اجازاتی انتظام کی سب سے سادہ شکل لین دین کی جانچ پڑتال دستخط دیکھنا، بشرطیکہ دستخط پہلے سے ہی پتہ ہو. عام طور پر اختیارات کسی فرد یا گروہ کو دیے جاتے ہیں اور یہ اکثر کمپارٹمنٹالایزڈ ہوتے ہیں. EOS.IO سافٹویئر ایک مدددل انتظامی اجازت نامہ فراہم کرتا ہے جو اکاؤنٹس کو اعلی سطح پر کنٹرول کرتا ہے کہ ان کا کب کہا کرنا ہے.

اجازات اور تصدیق کا انتظام کرنے کے لیے ضروری ہے کہ ان کو تجارتی اور منطق اپلیکیشنز سے علیحدہ رکھا جائے. اس سے آلات کو بڈھاوا دینے اور اجازت کا انتظام کرنے کے غرض سے اور کارکردگی کا مظاہرہ کرنے کے لیے اہم مواقع فراہم کرنے کے لیے تیار ہو.

ہر ایک اکاؤنٹ کو کنٹرول کرنے کے لیے نجی کلیدوں کی مجموعہ بندی اور مخصوص چابیاں ہوتی ہے. یہ اس بات کی عکاسی کرتا ہے کہ کس طرح اجازت حقیقت میں منظم کیے جاتے ہیں اور کس طرح موروثی اتھارٹی ساخت پیدا کرتا ہے، اور کسیر صارفین والے فنڈز پر پہلے سے زیادہ کنٹرول آسان بناتا ہے. کیی صارفوں کا کنٹرول سیکورٹی کو اکیلے امداد کرتا ہے، اور جب صبح استعمال ہوتا ہے تو چوری وغیرہ کے امکانات بہت حد تک نکال دیے جاتے ہیں.

EOS.IO سافٹویئر اکاونٹ کیز اور اکاونٹ سے جانے والی مخصوص پیغامات کی وضاحت کرنے کی اجازت دیتا ہے. مثال کے طور پر، یہ ممکن ہے کہ ایک صارف کے پاس سماجی میڈیا کے لیے ایک اور دوسرے ایکسچینج کے لیے رسائی ممکن ہو. یہ بھی ممکن ہے کہ دوسرے صارفین کو اجازت دی جائے کہ وہ دوسرے صارفین کے اکاؤنٹس کو چلایے بغیر تفویض کیے یہ عمل ممکن ہو.

### نامی اجازت کی حدیں-

<img align="right" src="http://eos.io/wpimg/diagram3.png" width="228.395px" height="300px" />

EOS.IO سافٹویئر کے استعمال سے، اکاؤنٹ نامی اجازات کو بیان کرسکتے ہیں جو اوپری سطح کے اجازت سے. نکالی گئی ہو. ہر نامی اجازتی حد ایک اتھارٹی کو بیان کرتی ہے؛ایک اتھارٹی متعدد دستخطوں کی جانچ کے لیے چابی کی طرح اور /دوسرے نامی اکاؤنٹس کی اجازاتی حدیں. مثال کے طور پر، ایک دوست کی اجازت پر اکاونٹ میں موجود تمام دوست اکاؤنٹ کو برابر کنٹرول کر سکتے ہیں.

اور ایک مثال سٹیم بلاکچین ہے جس کے تین مظبوط کوڈڈ نامی اجازتی حدیں، مالک، فعال، اور پوسٹنگ. پوسٹنگ اجازت صرف سماجی کام جیسے ووٹنگ اور پوسٹنگ کر سکتا ہے، جبکہ فعال اجازت مالک کی تبدیلی کو چھوڑ کر سب کچھ کر سکتے ہیں. مالک کی اجازت کو سرد سٹوریج کے لیے بنایا جاتا ہے اور یہ کچھ بھی کر سکتا ہے. EOS.IO سافٹویئر اس تصور کو عام کرنے کے لیے ہر ایک اکاؤنٹ ھولڈر کی درجہ بندی اور درجہ بندی کے کام کی وضاحت کرتا ہے.

### نامی پیغامات نیٹ کار گروہ-

EOS.IO سافٹویئر ہر ایک اکاؤنٹ کو یہ اجازت دیتا ہے کہ وہ اندرونی گروپس کو منظم کرنے کے لیے اجازت دیتا ہے. یہ نامی پیغامات دوسرے اکاونٹس میں ایک لاہ عمل کے طور پر اکاونٹس کی تشکیل اور اجازت کا کا حوالہ دے سکتے ہیں.

سب سے اعلٰی درجے کا پیغام اکاؤنٹ کا نام اور سب سے کم درجے کا پیغام انفرادی قصم کا پیغام جو کسی اکاؤنٹ سے حاصل ہوا ہو. ان گروہوں کا حوالہ اس طرح سے دیا جا سکتا ہے: **ٹ@ اکاونٹ کا نام. groupa. subgroup. 0\>MessageType>.</p> 

اس ماڈل کے تحت ایک ایکسچینج معاہدے کے لیے ممکن ہے کہ وہ آزادانہ طور پر گروہ آڈر کر سکتا ہے اور جمع اور نکال بھی سکتا ہے. یہ گروپ بندی ایکسچینج کی طرف سے صارفین کے لیے ایک سہولت ہے.

### نقشہ کاری کی اجازت-

EOS.IO سافٹویئر ہر ایک اکاؤنٹ کو اجازت دیتا ہے کہ وہ گروہ نامی انتخابات کے درمیان ایک نقشہ واضح کرنے کے لیے اجازاتی حد. مثال کے طور پر، ایک اکاؤنٹ ہولڈر سوشل میڈیا اکاونٹ کی نقشہ کاری دوسرے دوست کے اجازاتی گروپ کے ساتھ ضم کرسکتا ہے. اس نقشہ کاری سے کوئی بھی دوست ایک اکاؤنٹ ھولڈر کا اکاونٹ سوشل میڈیا پر شائع کر سکتا ہے. اگرچہ وہ اکاونٹ ہولڈر کے طور پر پوسٹ کو شایع کریں گے، مسیح کو ڈالنے کے لئے انہیں اپنی چابیاں استعمال میں لانی ہوگی. اس سے یہ جاننا ہر وقت ممکن ہے کہ اکاونٹ کو کس دوست نے کس طرح استعمال کیا ہے.

### جایزہ لینے کی اجازت-

اس قسم کا پیغام بھیجتے وقت " **Action**", from **@alice** to **@bob** the EOS.IO software will first check to see if **@alice** has defined a permission mapping for **@bob.groupa.subgroup.Action**. اگر کچھ بھی نہیں جاتا ہے تو **@bob.groupa.subgroup** then **@bob.groupa**, and lastly **@bob** کے لیے نقشہ کاری کی جانچ پڑتال ہوگی. اگر مزید کوئی مشابہ نا ملے تو، نقشہ کاری کو نامی اجازتی گروپ **@alice.active**.تصور کیا جایے گا.

نقشہ کاری کی نشاندہی کے بعد ایک بار پھر اتھارٹی پر دستخط کرنے کے چوکھٹ کثیر دستخط عمل اور اجازت نامے کے ساتھ وابستہ کا اختیار استعمال کرتے ہوئے درست قرار دیا ہے. اس کے ناکام ہونے کی صورت میں یہ پہلے بنیادی اجازت اور پھر **@alice.owner**. مالک کی اجازت.

<img align="center" src="http://eos.io/wpimg/diagram2grayscale2.jpg" width="845.85px" height="500px" />

#### طے شدہ اجازاتی گروہ-

EOS.IO سافٹویئر تکنیک سب اکاونٹس کو یہ اجازت دیتا ہے کہ وہ اپنا ایک "مالک" گروپ رکھ سکتے ہے اور ایک فعال گروپ جو مالک کے گروپ کی تبدیلی کے سوا کچھ بھی کر سکتا ہے. دیگر تمام اجازاتی گروپ فعال سے نکلے ہوئے ہیں.

#### متوازی جایزہ کی اجازت-

اجازات کے جایزہ کا عمل فقط پڈھنا ہے اور لین دین کی طرف سے بغیر اجازت کی تبدیلیاںایک بلاک کے آخر تک اثر انداز نہیں ہوتی. اس کا یہ مطلب ہے کہ تمام کیز اور اجازاتی تشخیص لین دین کے متوازی چلتا ہے. علاوہ ازیں، ایک تیز رفتار کا جواب دہی عمل قیمتی اپلیکیشنز کے منطق شروع کی جا سکتی ہے. آخر میں، اس کا مطلب یہ ہے کہ لین دین کی اجازات کی تشخیص کے بعد دوبارہ تشخیص کی ضرورت نہیں.

تمام چیزوں کو مدنظر رکھتے ہوئے، اجازاتی تصدیق کے نمایندو کی ایک بڑی فیصد لین دین کی تصدیق کرتی ہے. اسکو فقط ایک مطعالاتی اور متوازی عمل بنانے سے اس کی کارکردگی میں ڈرامائی اضافہ ہونے کے قابل بناتا ہے.

بلاکچین کو جواب دیتے وقت کہ اس کی بنیادی شکل کو پیغامات کے لاگ سے تشخیص کرنے کے لیے دوبارہ اجازت لینے کی ضرورت نہیں. حقیقت یہ ہے کہ لین دین ایک پہچان والے بلاک سے ہوتا ہے جو اس قدم کو سکپ کرنے کے لیے کافی ہے. بلاکچین پر پڈھنے والے شمارندگ لوڈ کو ڈرامائی طور پر کم کرتا ہے.

## پیغامات لازمی تاخیر کے ساتھ-

وقت سلامتی کا ایک اہم جز ہے. اکثر ضرورتوں میں، یہ جاننا ممکن نہیں ہے کہ پوشیدہ چابی کو کو چوری کے بعد استعمال کیا گیا ہے یا نہیں. وقت پر مبنی سلامتی کی اہمیت اس وقت بڈھ جاتی ہے جب لوگوں کے پاس وہ اپلیکیشنز ہو جن کے پاس روزانہ استعمال کے لیے کمپیوٹر اور انٹرنیٹ سے منسلک رہنے کے لئے چابیوں کی ضرورت پڈتی ہیں. EOS.IO سافٹویئر اپلیکیشن ڈیولپرس کو اس قابل بناتا ہے کہ کچھ پیغامات کو بلاک پر ڈالنے سے پہلے لازماً ایک چھوٹے سے وقفے کے لیے رکنا پڈتا ہے. اس دوران انہیں منسوخ کیا جا سکتا ہے.

ان پیغامات کے نشر ہونے کے بعد صارفین کو ایمیل یا ٹیکسٹ کی شکل میں ایک نوٹس ملتا ہے. ان کی طرف سے اجازت نہ ملنے پر، وہ اکاونٹ کو ریکور کرنے کے لیے اور پیغامات کو نکالنے کے لیے ایک عمل کرتے ہیں.

ضروری تاخیر آپریشن کے احساس پر منحصر ہے. ایک قافی کپ کی ادائیگی کرنے میں دھیری نہیں لگتی اس کے لیے بس چند سیکنڈ کا وقت درکار رہتا ہے وہی ایک مکان خریدنے کی ادائیگی میں 72 گھنٹے تک کا وقت لگ سکتا ہے. پودا اکاونٹ ٹرانسفر کرنے کے لیے 30 دن لگ سکتے ہیں. برابر دھیری اپلیکیشن بنانے والے طے کرتے ہیں.

## چوری شدہ کیز سے وصولی-

EOS.IO سافٹویئر صارفین کو چوری ہوے اکاونٹس کی چابیاں ریسٹور کرنے کا اختیار دیتا ہے. ایک اکاؤنٹ مالک کسی بھی مالک کی چابیاں استعمال میں لا سکتا ہے جو پچھلے 30 دنوں سے فعل رہا ہو دوسرے اکاونٹ شراکت دار سے اکاؤنٹ کی چابیاں ریکور کرنے کے لیے. اکاونٹ ریکوری شراکت دار مالک کی مدد کے بغیر اکاونٹ کو کنٹرول نہیں کر سکتا ہے.

ایک ھیکر اگر ریکوری عمل کی کوشش کریں تو اس سے اسکو کوئی فائدہ نہ ہوگا کیونکہ اس پر پہلے سے ہی ان کا کنٹرول رہتا ہے. علاوہ ازیں، اگر وہ ایک اچھا لاہ عمل اپناے گیں، ریکوری شراکت دار کثیر مرحلہ اجازت اور پہچان کی مانگ کرے گا (ایمیل اور فون). اس سے شاید ایک ھیکر کو اس عمل کے دوران شاید نہ ہو گا.

یہ عمل متعدد دستخطی عمل سے کافی مختلف ہے. متعدد دستخطی لین دین کے عمل میں اس کے علاوہ اور ایک کمپنی شراکت رکھتی ہے مگر روزانہ وصولی کے عمل میں دوسرے شراکت داروں کا کوئی عمل دخل نہیں رہتا. یہ جڈے ہوئے تمام افراد کے حقیقی بقایاجات میں ڈرامائی کمی لاتا ہے.

# اپلیکیشنز کے درمیان متوازی نفاذ

کنسینس بلاکچین کا انحصار ڈٹرمنسٹک جنم کے روپ پر رہتا ہے. اس کا مطلب تمام متوازی عمل موٹیکسز اور دوسرے لانگ پرمیٹوز سے مفت ہونے چاہئیں. تالو کے بغیر یہاں لازماً ایک ایسا راستہ ہونا چاہیے جو اس بات کی ضمانت دیں کہ اکاونٹس صرف ان کے پوشیدہ ڈیٹابیس سے پڈھ اور لکھ سکے. اس کا مطلب یہ بھی ہے کہ اکاونٹ کے ہر ایک عمل میں ایک مسلسل اور متوازی عمل ہر ایک اکاؤنٹ حد پر رہے.

EOS.IO سافٹویئر پر مبنی ایک بلاکچین میں، انتخابات کو آزادانہ طور پر ایک جگہ سے دوسری جگہ منتقل کرنے کا کام اور ان کے متوازی جانچ کا کام بلاک پرڈوسد کا ہے. ہر ایک اکاؤنٹ کی صورت اس پر بیجھے ہوے انتخابات پر منحصر ہے. ترتیب بلاک پرڈوسد کا مطبوعہ ہے اور اس کو عمل میں لانے کو آمادہ کرتا ہے. مگر اس عمل کو بڈھاوا دینے کے لیے عمل کا یقینی ہونا ضروری ہے. اس کا مطلب یہ ہے کہ بلاک پرڈوسد شڈول کے مطابق لین دین کو متوازی الگورزم کا استعمال کرسکتا ہے.

متوازی عمل کا ایک حصہ کا مطلب ہے کہ یہ انتخابات کو فوری طور پر منتقل نہیں کرتا، اسکے بدلے میں یہ دوسری دفعہ میں منتقل کردے گا. اس کا جلدی سے منتقل نا ہونے کی وجہ یہ ہوسکتی ہے کہ ریسیور حالیہ وقت میں اپنی صورت کو ترمیم کرنے میں لگا ہو.

## مواصلاتی چھپاو کو کم کرنا-

مواصلاتی وقت وہ وقت ہے جو ایک اکاؤنٹ کو دوسرے اکاونٹ تک پیغام پہنچانے میں اور اس کا جواب ملنے میں درکار رہتا ہے. اس کا اصل مقصد ایک اکاؤنٹ سے دوسرے اکاونٹ تک پیغامات پہنچانا اور تین سیکنڈ سے کم وقت میں پیغام کا جواب طلب کرنا. اس مقصد کے لیے EOS.IO سافٹویئر ہر ایک بلاک کو مختلف مرحلوں میں تقسیم کرتا ہے. ہر ایک سائیکل کو دھاگوں میں تقسیم کیا جاتا ہے اور ہر ایک دھاگے کے ساتھ لین دین کی ایک لسٹ ہوتی ہے. ہر ایک ٹرانزکش کے ساتھ پیغامات بجھنے کا ایک لسٹ ساتھ میں رہتا ہے. یہ ڈھانچہ ایک درخت کی صورت میں دیکھا جا سکتا ہے جہاں متبادل تہیں ایک مخصوص متوازی عمل میں.

        بلاک
    سائیکل (تسلسلی)
    
    دھاگے (متوازی)
    
    لین دین (تسلسلی)
    
    پیغامات (تسلسلی)
    
    وصول کنندہ اور اطلاعی اکاونٹس (متوازی)
    

ایک چکر کے دوران تیار ہوچکے لین دین کو اس کے اگلے چکر یا بلاک میں منتقل کیا جا سکتا ہے. ایک بلاک پرڈوسد تب تک بلاک میں سائیکل جھوڈتا رہے گا جب تک نہ بلاک کی آخری حد پہنچ جائیں یا وہاں منتقلی کے لئے کوئی بھی لین دین نہ ہو.

ایک بلاک کی جامد جانچ کے لیے یہ ممکن ہے کہ کوئی بھی دو دھاگے ایک ہی اکاونٹ کے لین دین کو تبدیل کر سکے. جب تک کہ وہ ناقابل تغیر قائم کیا جائے ایک بلاک کے تمام دھاگوں کو متوازی عمل کے زریعے چلایا جا سکتا ہے.

## صرف ھینڈلرس کے انتخابات پڈھنا-

کچھ اکاونٹس پیغاماتی عمل کو پاس/فیل کی بنا پر بغیر اندرونی ردوبدل کے کر سکتے ہیں. اگر یہ معاملہ ہو تو صرف فقط مطالعہ پیغام سہولت کار ایک مخصوص اکاؤنٹ کے لیے ایک یا زائد تریڈس میں ایک مخصوص سائیکل کے اندر شامل ہیں جب تک کہ تو ان کے رہبروں کے متوازی میں سرانجام دیا جائے کر سکتے ہیں ۔.

## ایک سے زائد اکاؤنٹ سے اٹمی لین دین-

کبھی کبھار اس خواہش کو یقینی بنایا ضروری ہے کہ پیغامات دوسرے اکاونٹس سے خود بخود موصول ہو. اس صورت میں دونوں پیغامات ایک ہی ٹرانزکشن میں رکھے جاتے ہیں اور دونوں اکاونٹس ایک ہی دھاگے کے ساتھ ترتیب سے تفویض ہوگے. یہ صورت عملی طور پر موزوں نہیں اور جب صارفین کو سہولیات کے استعمال کی ادائیگی کرنی ہو تو وہ بہت ساری تکنیکوں سے لین دین کی ادائیگی کر سکتے ہیں.

لاگت اور کارکردگی کی وجوہات کے لیے یہ بہتر ہے کہ دو کافی استعمال ہونے والے اکاونٹس کے درمیان ہونے والے اٹمی آپریشنز کو کم کیا جائے.

## بلاکچین کی صورتحال کی جزوی تشخیص-

پیمانہ کاری بلاکچین تکنیک کے لیے اجزاء کا معیاری ہونا لازمی ہے. ہر ایک کو سب کچھ چلانے کی ضرورت نہیں، خصوصاً اگر انہیں استعمال کے لیے اپلیکیشنز کا ایک چھوٹا طاقم درکار ہو.

ایک exchange ایپلیکیشن ڈویلپر اپنے صارفین کو ایکسچینج کی صورت دکھانے کے لئے اس کو پورا گروہ چلاتا ہے. اس ایکسچینج اپلیکیشن کو اس کی کوئی ضرورت نہیں ہے کہ سوشل میڈیا اپلیکیشنز کس صورت میں ہے. EOS.IO سافٹویئر کسی بھی مکمل گرہ کو طاقم اپلیکیشنز چلانے کی اجازت دیتا ہے. دوسری اپلیکیشنز پر منتقل ہونے والے پیغامات محفوظ طریقے سے نظر انداز کیے جاتے ہیں کیونکہ ایک اپلیکیشن کی اصل صورت پوری طرح سے اس پر منتقل ہونے والے پیغامات پر منحصر ہے.

اسے دوسرے اکاونٹس کے ساتھ ہونے والے مواصلات پر کچھ اہم مضمرات ہے. سب سے زیادہ نمایاں طور پر یہ نہیں کہا جا سکتا ہے کہ دوسرے اکاونٹ کی صورت کو ایک ہی مشین میں قابل رسائی بنایا جا سکے. اس کا مطلب یہ بھی ہے کہ ایک لاک کو اس قابل بنانے کی کوشش میں کہ وہ ایک اکاؤنٹ سے دوسرے اکاونٹ کو چلانے کی اجازت دیں، یہ ڈیزائن پیٹرن ٹوٹ جاتا ہے اگر دوسرا اکاونٹ اس کی یادداشت میں نہ ہو تو.

بلاکچین میں موجود تمام تر مواصلات کو اکاؤنٹس میں انتخابات کے زریعے پاس کیا جانا چاہیے.

## بھترین کوشش اس کے لیے شرط ہے-

EOS.IO سافٹویئر بلاک پرڈوسدز کو قانوناً اس بات کی اجازت نہیں دیتا کہ وہ دوسرے اکاونٹ تک کوئی پیغام پہنچایے. ہر بلاک پروڈیوسراپنا ایک کتابی پیمانہ بناتا ہے جس سے وہ پیچیدہ شمارندگ اور لین دین کے عمل کے لیے درکار وقت کو ماپ سکے. اس سے یہ پتہ چلتا ہے کہ آیاں لین دین صارف کی طرف سے ہوا ہے یا خود بخود سکرپٹ سے ہوا ہے.

کسی چلنے والی بلاکچین پر جو EOS.IO سافٹویئر سے جڈ جات ہے، تمام لین دین کو ایک نیٹورک کی سطح پر ایک مقررہ شمارندگ بینڈوڈتھ کی ضرورت بنا پر طے کیا جاتا ہے اس سے قطع نظر کہ آیا اس کو عملانے میں 01 یا پورے 10ms لگے. تاہم، اس سافٹویئر کو استعمال کرنے والا ہر ایک فرد اس میں استعمال ہونے والے وسائل کی پیمائش اپنے الگورزم اور پیمائش سے شمار کر سکتا ہے. جب ایک بلاک پرڈوسد کو یہ لگتا ہے کہ لین دین کے دوران اکاونٹ سے متناسب رقم کی شمارندگ ہوئی ہے تو اس وقت وہ اپنا بلاک بنانے کے دوران اس ٹرانزکشن کو رد کریں گے، تاہم، وہ اس لین دین کے عمل کو پھر بھی چلا سکتے ہیں اگر دوسرے بلاکچین پدروسرز اس کو جایز ٹھہرایے.

عام طور پر وسائل کی دستیابی کے وقت اگر کوئی ایک بلاک پرڈوسد لین دین کے عمل کو جایز ٹھہرایے تو اس حالت میں باقی تمام بلاک پرڈوسدز یہ تسلیم کریں گے، لیکن. پرڈوسد کو ڈھونڈنے میں لین دین کو ایک منٹ کا وقت لگ سکتا ہے.

بعض صورتوں میں ایک پرڈوسد ایک بلاک بنا سکتا ہے جس میں ایسا لین دین موجود ہو جو قبول حد اطلاق سے باہر ہو. اس صورت میں دوسرا بلاک پرڈوسد بلاک کو مسترد کرنا چاہے گا اور اس صورت کو توڈنے میں تیسرے بلاک پرڈوسد کا عمل رہے گا. یہ اس سے مختلف نہیں کہ اس وقت کیا ہوگا جب ایک بڈے بلاک نیٹورک کی جالیات میں تاخیر ہوگی. کمیونیٹی زیادتیوں کا جائزہ لے گی اور بالآخر بدمعاش پرڈوسدز کے ووٹوں کو نکال دے گے.

یہ کتابی شمارندگ جانچ پڑتال بلاکچین کو اس لاگت سے مفت کرتی ہے کہ کون سی چیز کو چلانے اور اس کی پیمائش کرنے میں کتنا وقت لگتا ہے. اس ڈیزائن سے ان ہدایات ت کو جمع کرنے کی کوئی ضرورت نہیں ہے جو ڈرامائی طور پر بہ اتفاق رائے ریاضیات کو بڈھاوا دینے کے مواقع فراہم کرتا ہے.

# وسائل کا استعمال اور ٹوکن کا ماڈل-

**مہربانی کر کے یاد رہے:اس سفید کاغذ میں جو کرپٹوگرافک ٹوکن ھے وہ کرپٹوگرافک ٹوکنز دراصل EOS. IO سوفٹویر سے منسلک ہے -. ان کا تعلق ERC-20 ٹوکنز کے ساتھ نہیں ھے اتھیریم بلاکچین EOS ٹوکن تقسیم کے حوالے تقسیم ھو نے کی وجہ سے ھوتا ھے.**

تمام بلاکچین وسائل کے ساتھ محدود ہے اور زیادتی سے بچنے کے لیے ایک نظام درکار ہے. اس بلاکچین کے ساتھ جو EOS.IO سافٹویئر استعمال کرتا ہے، یہاں وسائل کے تین بڑے اقسام ہیں جو اپلیکیشنز کی طرف سے بہسم ہوتے ہیں:

1. بینڈوڈتھ اور لاگ اسٹوریج (ڈسک) ۔;
2. کمپیوٹیش اور کمپیوٹیشنل بیک لاگ CPU))
3. اصل صورت میں رکھنے کی لاگت-.

بینڈوڈتھ اور محاسبہ کے تین جز ہے، فی الفور استعمال اور طویل المیعاد استعمال ہے. ایک بلاکچین انتخابات کے تمام لاگ کو منظم رکھتا ہے اور بالآخر تمام گروہ سے ڈاؤن لوڈ کر کے زخیرہ کیا جاتا ہے. اپلیکیشن کے لاگ کے ساتھ یہ اپلیکیشن کی تعمیر نو ممکن ہے.

شمارندگ قرض کا حساب انتخابات کے لاگ کو باز تخلیق صورت کو عملانے کے لیے ضروری ہیں. اگر شمارندگ قرض میں کافی بڈھاوا ہو تو اس صورت میں یہ ضروری بن جاتا ہے کہ بلاکچین کے ماضی کو مسترد کرنے کے لیے بلاکچین کے سنیپ شاٹ لیے جائیں. اگر شمارندگ قرض میں کافی تیز بڈھاوا ہو اس صورت میں ایک سال پرانی ٹرانزکشن کا جواب طلب کرنے میں چھے ماہ کا وقت لگ سکتا ہے. شمارندگ قرض کا انتظام کرنے کے لیے لھزا یہ کافی نازک ہے.

بلاکچاان میں ذخیرہ کی گیی معلومات درخواست کی منطق سے قابل رسائی ہے. اس میں کتابی ترتیب اور اکاونٹ بیلنس کی معلومات موجود ہے. اگر صورتحال اپلیکیشن سے نا پڈھی جائے تو ایسی معلومات کو زخیرہ نہیں کرنا چاہیے. For example, blog post content and comments are not read by application logic so they should not be stored in the blockchain's state. Meanwhile the existence of a post/comment, the number of votes, and other properties do get stored as part of the blockchain's state.

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