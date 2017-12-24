## IO.EOS سوفٹؤر میپ

یہ دستاویز ترقیاتی منصوبہ بندی سے ایک اونچی سطحی چیزوں اور ورژن 1.0 کی جانب گامزن ھے اور تازہ کی جایے گی. یہ دھیان رکھیی کہ یی میپ صرف بلاکچین سوفٹوئد کے بادے میی ھے نھ کے باقی اوزار اور افادیت جیسے کہ بٹوے اور بلاک ایکسپلورر جنکے پاس اپنی ٹیمیے اور وقفشدہ میپ ھی ایکب بار فیز 1 مکمل ھوجایے.

***اس دستاویزمین موجودتمامعلومات ڈرافٹ یعنی مسودے صورت میں ھے کونسی بھی وقت تبدیل کی جاسکتی ھے اور محض معلوماتی مقاصد کے لیے فراہم کی جاتی ھے".". دستاویز میں موجود اول نقطہ معلومات کی درستگی بےجوفراھم کی جاتی ہے " جیسا کہ" کویی نمایندگی یاکسی چیز کے صحیح یا کھرے ھونے کی ضمانت، ظاہر کرنا یا مضمر کے ساتھ اس بات کی کویی ضمانت نھی.***

# پھلا مرحلہ-موسم گرما 2017-کم سے کم قابلِ عمل؛ماحولیاتی جانچ

اس مرحلے کا مقصد APIs کی تشکیل دینا ہے جو تعمیدےکرنے والوں کو EOS. IO کی بناوٹ اور اس کی جانچ میں درکار ہو گی. درخواستوں کی جانچ شروع کر کے لیے ڈیولپرز کومندر جہ زیل معاہدے پر عمل کرنا ہوگا:

### کسی سسٹم سے الگ آزادانہ طور پر کام کرنا ہوگا (Dan & Nathan)

ایک یکتا گرہ ایک جانچ کرنے والی بلاکچین چلاتا بے اور APIکو دکھاتے ھوے بلاکچین کی تشکیل دیتا ہے اس گروہ کو بزات خود کسی P2Pنیٹورکنگ کوڈ کے ساتھ تعلق رکھنے کی کوئی ضرورت نھین.

### بنیادی معاھدے(Nathan)

SOS. 10 سافٹویئر کے کیی بنیادی معاہدے بے یھی وہ معاہدے بے جو بلاکچین اوربنیادی عمل کا انتظام کرتے بے جوویب اسمبلی انٹرفیس کے باھر موجود رھتے ھے ان معاہدوں میں موجود ھیں:

1. @EOS_eos ٹوکن کو منتقل کرنے کا انتظام کر تا بے
2. @stake _. مقفل ھوے EOS،ووٹنگ اور پرڈوسر انتخابات کا انتظام کرتا ہے
3. system@-رابطہ کوڈ کی تازہ ترین معلومات پیغامات کا انتظام کرتا ھے

### مجازی آلہAPI (دان)

معاہدے ویب اسمبلی WASIM)) سے منسلک ھے اور ایک وضاحتی API کے زریعے بلاکچین کے ساتھ وعسم مواجہ چاہیے. یہ وہAPIھے جس پر ڈیولپرس منحصر ہے اپلیکیشنز بنانے کے لیے اورڈیولپرز واقعی EOS پر استوار کرنے کا آغاز کر سکتے ہیں جو کہ انحصار ھے.

### RPC interface (ارھاگ اور ناتھن)

ڈیولپرس کے لین دین کو نشر اور اپلیکیشن کا صورتحال طلب کرنے کے لیے ایک سادہ JASON HTTP RPC انٹرفیس اس کو قابل عمل بنانے کے لیے ضروری ہے. اپلیکیشنز کو شایع کرنے اور ان کا ٹیسٹ کرنے کے لیے یہ بھت ھی اھم ھے.

### کمانڈ لائن آلہ (ارھاگ)

کمانڈ لائن آلہ RPC انٹرفیس کے ساتھ ڑیولپرز تعمیر ماحول میں شمولیت کی سھولیت فراہم ھے.

### بنیادی تعمیراتی دستاویزات ( جوش)

Documents that teach developers how to get started with building on EOS.IO blockchains. This includes documentations of the WASM API, RPC Interface, and Command Line Tools.

# دوسرا مرحلہ کم از کم قابل قبول نیٹورک - fall 2017

پھلا مرحلہ میں سب کچھ صرف ڈیولپرس کے اپنے ضابطہ سے چلتا ہے ایک قابل قدر ماحول مان لیا گیا ہے اس سے پہلے کہ ایک ٹیسٹ کے نیٹ ورک کو تاینات کیا جا سکتا کھی اضافی خصوصیات نافذ اور ٹیسٹ کرنے کی ضرورت ہے.

### P2P Network Code (Phil)

This is a plugin that is responsible for synchronizing the blockchain state between two standalone nodes.

### WASM Sanitation & CPU Sandboxing (Brian)

The WASM code needs to be sanitized to check for non-deterministic behavior such as floating point operations and infinite loops.

### Resource Usage Tracking & Rate Limiting (Arhag)

To prevent abuse the resource monitoring and usage tracking rate limits users according to staked EOS.

### Genesis Import Testing (DappHub)

Tools need to be developed to export data from the EOS Token Distribution state and create a genesis configuration file. This will enable anyone participating in the Token Distribution to acquire some initial test EOS (TEOS).

### Interblockchain Communication (Nathan)

This feature involves verifying the Merkle hashing of transactions is proper.

# Phase 3 - Testing & Security Audits - Winter 2017, Spring 2018

During this phase the platform will undergo heavy testing with a focus on finding security issues and bug. At the end of Phase 3 version 1.0 will be tagged.

### Develop Example Applications

Example applications are critical to proving the platform provides the features required by real developers.

### Bounties for Successfully Attacking Network

Attacking the network with spam, virtual machine exploits, and bug crashes, and non-deterministic behavior will be a heavily involved process but necessary to ensure that version 1.0 is stable.

### Language Support

Adding support for additional languages to be compiled to WASM: C++, Rust, etc.

### Documentation & Tutorials

# Phase 4 - Parallel Optimization Summer / Fall 2018

After getting a stable 1.0 product released, we will move toward optimizing the code for parallel execution.

# Phase 5 - Cluster Implementation The Future