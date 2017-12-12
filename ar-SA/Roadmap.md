## خارطة الطريق لبرنامج EOS.IO

توضح هذه الوثيقة خطة التنمية من مستوى عال وسيتم تحديثها مع التقدم المحرز نحو الإصدار 1.0. وتجدر الإشارة إلى أن خارطة الطريق هذه تنطبق فقط على برامج بلوكشين وليس على الأدوات والمرافق الأخرى مثل المحفظة ومستكشفي البلوك الذي سيكون لهم فرق خاصة وخرائط الطريق مخصصة عند اكتمال المرحلة 1.

***كل ما ورد في هذه الوثيقة هو في شكل مسودة وقابل للتغيير في أي وقت، ويعرض لأغراض المعلومات فقط. block.one لا يضمن دقة المعلومات الواردة في خارطة الطريق هذه، وتوفر المعلومات "كما هي" دون أي تعهدات أو ضمانات، صريحة أو ضمنية.***

# المرحلة 1 - الحد الأدنى لبيئة إختبار قابلية الحياة

والهدف من هذه المرحلة هو برمجة واجهات التطبيقات التي سيحتاجها المطورون للبدء في بناء واختبار التطبيقات على EOS.IO. ولكي يبدأ المطورون في اختبار تطبيقاتهم، سيحتاجون إلى تنفيذ ما يلي:

### العقدة المستقلة (Dan & Nathan)

عقدة مستقلة تختبر بلوكشين وتنتج بلوكات في حين استخدام API. هذه العقدة لا تحتاج لتكون لها صلة مع أي رمز من رموز شبكات P2P.

### العقود الأصلية (Nathan)

يحتوي البرنامج EOS.IO على عدد من العقود المحلية. هذه هي العقود هي التي تدير العمليات الأساسية للبلوكشين وتوجد خارج واجهة الويب. وتشمل هذه العقود ما يلي:

1. @eos إدارة نقل رمزية EOS
2. @stake - تدير EOS المقفلة، والتصويت، واختيار المنتج
3. @system - يدير التصريحات والرسائل، وتحديثات رمز الاتصال

### الجهاز الظاهري أبي (Dan)

يتم تجميع العقود إلى مركب الويب (WASM) و WASM يجب أن يتفاعل مع بلوكشين عبر API معرفة. هذا أبي هو ما يعتمد عليه المطورين لبناء التطبيقات وتكون مستقرة نسبيا قبل أن يتمكن المطورين حقا من الإنشاء على EOS.

### RPC Interface (Arhag, Nathan)

A simple JSON RPC over HTTP interface will be provided that enables developers to broadcast transactions and query application state. This is critical for both publishing and interacting with test applications.

### Command line Tools (Arhag)

Command line tools facilitate integrating the RPC interface with developer build environments.

### Basic Developer Documentation (Josh)

Documents that teach developers how to get started with building on EOS.IO blockchains. This includes documentations of the WASM API, RPC Interface, and Command Line Tools.

# Phase 2 - Minimal Viable Test Network - Fall 2017

Everything in Phase 1 assumes a trusted environment that only runs the developer's own code. Before a test network can be deployed several additional features need to be implemented and tested.

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