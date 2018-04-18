# EOS.IO テクニカルホワイトペーパー

**2017年6月26日**

**要旨:** EOS.IOソフトウェアは、分散型アプリケーションの全方向へのスケーリングを可能にする、新しいブロックチェーン・アーキテクチャを導入します。 これは、アプリケーションをビルド可能なオペレーティグシステムに似た構成を作ることにより実現されます。 このソフトウェアは、アカウント、認証、データベース、非同期通信、数百にわたるCPUコアまたはクラスターを横断したアプリケーションのスケジューリングを提供します。 これにより、結果としてもたらされる技術は、一秒あたり数百万のトランザクションのスケールや、ユーザ手数料の撤廃、素早い分散型アプリケーションのデプロイを可能にするブロックチェーン・アーキテクチャです。

**注意事項: 当ホワイトペーパーにおける暗号トークンとは、EOS.IOソフトウェアを採用してローンチされたブロックチェーン上の暗号トークンであり、 EOS Token Distributionを通じてイーサリアムブロックチェーン上で配布されたERC20トークンではありません。**

Copyright © 2017 block.one

非営利、教育目的の利用においては、（商用、有料での利用を除いて）参照元と適切な著作権表示を記載した上で、当ホワイトペーパーの内容を許可なく複製、配布することを許可します。

**免責事項:** 当ホワイトペーパーは情報提供のみを目的としています。 block.one does not guarantee the accuracy of or the conclusions reached in this white paper, and this white paper is provided “as is”. block.one does not make and expressly disclaims all representations and warranties, express, implied, statutory or otherwise, whatsoever, including, but not limited to: (i) warranties of merchantability, fitness for a particular purpose, suitability, usage, title or noninfringement; (ii) that the contents of this white paper are free from error; and (iii) that such contents will not infringe third-party rights. block.one and its affiliates shall have no liability for damages of any kind arising out of the use, reference to, or reliance on this white paper or any of the content contained herein, even if advised of the possibility of such damages. In no event will block.one or its affiliates be liable to any person or entity for any damages, losses, liabilities, costs or expenses of any kind, whether direct or indirect, consequential, compensatory, incidental, actual, exemplary, punitive or special for the use of, reference to, or reliance on this white paper or any of the content contained herein, including, without limitation, any loss of business, revenues, profits, data, use, goodwill or other intangible losses.

- [背景](#background)
- [ブロックチェーンアプリケーション要件](#requirements-for-blockchain-applications) 
  - [数百万のユーザーをサポートできること](#support-millions-of-users)
  - [無料で使用可能であること](#free-usage)
  - [簡単なアップグレードとバグの修復](#easy-upgrades-and-bug-recovery)
  - [短い待ち時間](#low-latency)
  - [順次処理性能](#sequential-performance)
  - [並列処理性能](#parallel-performance)
- [コンセンサス・アルゴリズム(DPOS)](#consensus-algorithm-dpos) 
  - [トランザクションの承認](#transaction-confirmation)
  - [Transaction as Proof of Stake (TaPoS)](#transaction-as-proof-of-stake-tapos)
- [アカウント](#accounts) 
  - [メッセージ & ハンドラー](#messages--handlers)
  - [役割ベースの許可管理](#role-based-permission-management) 
    - [名前付きの許可レベル](#named-permission-levels)
    - [名前付きのメッセージ・ハンドラー・グループ](#named-message-handler-groups)
    - [許可のマッピング](#permission-mapping)
    - [許可の評価](#evaluating-permissions) 
      - [デフォルトの許可グループ](#default-permission-groups)
      - [許可の並列評価](#parallel-evaluation-of-permissions)
  - [強制的な遅延付きのメッセージ](#messages-with-mandatory-delay)
  - [鍵の盗難からの復旧](#recovery-from-stolen-keys)
- [アプリケーションの決定的な並列実行](#deterministic-parallel-execution-of-applications) 
  - [コミュニケーション待ち時間の最小化](#minimizing-communication-latency)
  - [読み取り専用のメッセージ・ハンドラー](#read-only-message-handlers)
  - [複数アカウントでのアトミック・トランザクション](#atomic-transactions-with-multiple-accounts)
  - [ブロックチェーン状態の部分評価](#partial-evaluation-of-blockchain-state)
  - [主観的な最善努力型のスケジューリング](#subjective-best-effort-scheduling)
- [トークンモデルとリソースの使用状況](#token-model-and-resource-usage) 
  - [客観的測定と主観的測定](#objective-and-subjective-measurements)
  - [受領者の支払い](#receiver-pays)
  - [容量の委譲](#delegating-capacity)
  - [トークンの価値から独立したトランザクション費用](#separating-transaction-costs-from-token-value)
  - [ステート・ストレージ・コスト](#state-storage-costs)
  - [ブロック報酬](#block-rewards)
  - [コミュニティ・ベネフィット・アプリケーション](#community-benefit-applications)
- [Governance](#governance) 
  - [Freezing Accounts](#freezing-accounts)
  - [Changing Account Code](#changing-account-code)
  - [Constitution](#constitution)
  - [Upgrading the Protocol & Constitution](#upgrading-the-protocol--constitution) 
    - [Emergency Changes](#emergency-changes)
- [Scripts & Virtual Machines](#scripts--virtual-machines) 
  - [Schema Defined Messages](#schema-defined-messages)
  - [Schema Defined Database](#schema-defined-database)
  - [Separating Authentication from Application](#separating-authentication-from-application)
  - [Virtual Machine Independent Architecture](#virtual-machine-independent-architecture) 
    - [Web Assembly (WASM)](#web-assembly-wasm)
    - [Ethereum Virtual Machine (EVM)](#ethereum-virtual-machine-evm)
- [Inter Blockchain Communication](#inter-blockchain-communication) 
  - [Merkle Proofs for Light Client Validation (LCV)](#merkle-proofs-for-light-client-validation-lcv)
  - [Latency of Interchain Communication](#latency-of-interchain-communication)
  - [Proof of Completeness](#proof-of-completeness)
- [Conclusion](#conclusion)

# 背景

ブロックチェーン技術は2008年のビットコインの誕生と共に社会に導入されました。それ以来、起業家や開発者は、一つのブロックチェーンプラットフォーム上で、幅広いアプリケーションをサポートするため、ブロックチェーン技術の一般化を試みてきました。

多くのブロックチェーンプラットフォームが、実用的な分散型アプリケーションのサポートに四苦八苦する一方で、分散型取引所のBisShares(2014)や、ソーシャルメディアプラットフォームのSteem(2016)といったアプリケーションに特化型のブロックチェーンは、毎日何万ものアクティブユーザーに利用されるブロックチェーンになっています。 それらのサービスは、一秒間に数千のトランザクションを処理できるようにし、待ち時間を1.5秒に抑え、手数料を撤廃し、既存の中央集権サービスで提供されているレベルのユーザエクスペリエンスを提供することでこのような結果を実現しています。

既存のブロックチェーンプラットフォームは、高額な手数料や限られた計算能力といった障壁によって広範なブロックチェーンの採用を阻まれています。

# ブロックチェーンアプリケーション要件

幅広く利用してもらうために、ブロックチェーン上のアプリケーションは以下の要件を満たすレベルで柔軟である必要があります:

## 数百万のユーザーをサポートできること

EbayやUber、AirBnB、Facebookといった破壊的なビジネスは、毎日数千万のアクティブユーザーを扱えるブロックチェーン技術を必要とします。 特定のケースにおいて、最低限必要なユーザーがリーチしなければアプリケーションは機能しない可能性があります。したがって、プラットフォームにおいては多数のユーザーを扱えることが最重要です。

## 無料で使用可能であること

アプリケーション開発者はユーザーに無料でサービスを提供するために柔軟性を必要としています。ユーザーはプラットフォームを利用するためにお金を払うべきではありません。 ユーザーが無料で使えるブロックチェーンプラットフォームはより広範に利用される可能性が高いと考えられます。 それにより、開発者や企業は効率的なマネタイゼーション戦略を立てることができます。

## 簡単なアップグレードとバグの修復

企業が構築するブロックチェーンベースのアプリケーションは、新機能の追加によってアプリケーションを強化するために、柔軟性を必要とします。

もっとも厳格な形式的検証を用いても、全ての重要なソフトウェアはバグに直面します。プラットフォームは不可避のバグを修復するために十分頑丈でなければなりません。

## 短い待ち時間

優れたユーザーエクスペリエンスは、数秒以内に確実なフィードバックが帰ってくることを必要とします。 長い遅延はユーザーを苛立たせ、既存のブロックチェーンを使用していないアプリケーションに対するブロックチェーン上のアプリケーションの競争力を低下させます。

## 順次処理性能

いくつかのアプリケーションは、順次的な依存した手順のために並列アルゴリズムを実装することができません。 取引所のようなアプリケーションは大きなボリュームを取り扱うために十分な順次処理性能が必要です。そのため、素早い順次処理性能を備えたプラッフォームが必要とされます。

## 並列処理性能

大規模なアプリケーションは複数のCPUとコンピュータ間で作業負荷を分担する必要があります。

# コンセンサス・アルゴリズム(DPOS)

EOS.IOはブロックチェーン上のアプリケーションのパフォーマンス要件を満たす唯一の分散型コンセンサスアルゴリズムである[Delegated Proof of Stake(DPOS)](https://steemit.com/dpos/@dantheman/dpos-consensus-algorithm-this-missing-white-paper)を採用しています。 このアルゴリズムにおいて、EOS.IOソフトウェアを採用しているブロックチェーン上のトークン保有者は、連続した承認投票システムによってブロック生成者を選任することが可能です。また、誰でもブロック生成に参加することができ、他のブロック生成者と比較した得票数に応じてブロックを生成する権利が与えられます。 プライベートブロックチェーンにおいては、管理者はITスタッフを追加または解任するためにトークンを使用できます。

EOS.IOソフトウェアは、一人のブロック生成者に任意の時点でブロックを生成する権利を与え、3秒ごとに1つのブロックが正確に生成されることを可能にします。 予定時刻にブロックが生成されなかった場合、その時間枠におけるブロックはスキップされます。 もし一つ以上もブロックがスキップされた場合、6秒以上のギャップがブロックチェーンにあることになります。

EOS.IOソフトウェアを使用した場合、ブロックは21のラウンドで生成されます。 それぞれのラウンドの初めに、21人のユニークなブロック生成者が選ばれます。 もっとも承認数の多かった上位20人のブロック生成者は自動的に全てのラウンドで選任され、残る1人のブロック生成者は、他のブロック生成者と比較した得票数に比例して選任されます。 選任されたブロック生成者はブロックタイムから算出された擬似乱数を用いてシャッフルされます。 このシャッフルは、全てのブロック生成者が他の全ての生成者に対して偏りのない接続を維持するために行われます。

ブロック生成者が24時間以内にブロックを生成していなかった場合、そのブロック生成者は、ブロックの生成を再び開始する意志をブロックチェーンに通知するまで、考慮から外されます。 これにより、信頼できないブロック生成者によるブロック生成の失敗を最小限に抑えることができ、スムーズなネットワークの動作を保証できます。

通常の条件下では、ブロック生成者は競争より協力を選ぶため、DPOSブロックチェーンはフォークしません。 フォークが生じた場合には、コンセンサスは自動的に最も長いチェーンに切り替わります。 このメトリックが働く理由は、どちらのフォークにブロックが追加されたかの割合と、同様のコンセンサスを共有するブロック生成者の割合は直接相関するためです。 言い換えると、より多くのブロック生成者を持つフォークは、より少ないブロック生成者を持つフォークより早く長くなります。 さらに、ブロック生成者は2つのフォークで同時にブロックを生成するべきではありません。 もしブロック生成者がそのようなことをやっていることが発見された場合、そのブロック生成者は投票によってブロック生成者から外される可能性が高いです。 このような二重生成に関する暗号証拠は、自動的に濫用者を除外するためにも使用することができます。

## トランザクションの承認

典型的なDPOSブロックチェーンにおいては、100%のブロック生成者が参加します。トランザクションは、送信から平均1.5秒後に99.9%の角度で承認されたと考えられます。

ソフトウェアのバグや、インターネットの混雑、あるいは悪意のあるブロック生成者が2つ以上のフォークを作るなどの、特異なケースが生じる場合があります。 トランザクションは確実に不可逆であるため、ノードは21のブロック生成者のうち15の承認が得られるまで待つことを選択します。 EOS.IOの標準構成に基づくと、このプロセスは通常の状況下において平均で45秒かかります。 デフォルトで全てのノードは、21のブロック生成者のうち15の承認を得たブロックは不可逆であると考え、長さに関わらずそのようなブロックを排除するフォークに切り替えることはしないでしょう。

ノードはユーザーに、少数派のフォークにいる可能性が高いということを、そのフォークのスタートから9秒以内に警告することができます。 2回連続でブロック生成が失敗している場合、95%の可能性でノードは少数派のフォークにいます。 3回連続でブロック生成が失敗している場合、99%の可能性で少数派のフォークにいます。 直近の参加率や、どのノードがブロック生成に失敗しているか、またその他の要因を利用して、オペレータに何かがおかしいことを早急に伝えるための堅牢な予測モデルを構築することが可能です。

こうした警告への応答はそのトランザクションの性質に完全に依存しますが、最も単純な応答は、警告が止むまで21分の15の承認を待つことです。

## Transaction as Proof of Stake (TaPoS)

EOS.IOソフトウェアは全てのトランザクションについて、直近のブロックヘッダーのハッシュを含むことを要求します。このハッシュには2つの目的があります:

1. 参照先のブロックを含まないフォーク上のトランザクションの再生を防ぎ
2. 特定のユーザーとそのステークが特定のフォーク上にあることをネットワークに通知する

時間が経つにつれて全てのユーザーは直接ブロックチェーンを承認するようになるため、偽造されたチェーンは正規のチェーンからトランザクションを移行できなくなり、チェーンを偽装することは難しくなります。

# アカウント

EOS.IOソフトウェアは、2文字から32文字の読み取り可能なユニークな人名に関連づけられているアカウントを許可します。 アカウントの作成者によってその名前は決められます。 データの保存費用を賄うため、全てのアカウントは作成時に最低限の残高が要求されます。 アカウント名はネームスペースもサポートしています。アカウントの@domainの保有者だけがアカウントの@user.domainを作成することができます。

分散型のコンテキストにおいて、アプリ開発者は、新しいユーザーを作成するためのアカウント作成にわずかな費用だけを支払います。 従来のビジネスは、無料のサービス提供や広告を通じて獲得した顧客に対して、莫大な資金を費やしています。 新しいブロックチェーンアカウントを作るための投資は、それと比較してわずかな額に抑えられるべきです。 幸運にも、既に他のアプリケーションに登録しているユーザーについては、新しいアカウントを作る必要はありません。

## メッセージ & ハンドラー

それぞれのアカウントは、他のアカウントに構造化されたメッセージを送ることができ、また、それらのメッセージが受け取られた際にそれを処理するスクリプトを定義することができます。 EOS.IOソフトウェアは、それぞれのメッセージのハンドラーのみがアクセスできるプライベートなデータベースをアカウントごとに付与します。 メッセージを処理するスクリプトは、メッセージを他のアカウントに送ることもできます。 このメッセージと自動化されたメッセージのハンドラーの組み合わせこそが、EOS.IOにおけるスマートコントラクトの定義です。

## 役割ベースの許可管理

許可管理は、メッセージが適切に承認されるかどうかを決定します。 許可管理の最もシンプルな形は、トランザクションが必要な署名を持っているか確認することです。しかし、これは必要な署名が既に知られているということを意味します。 一般的に権限は個人またはグループに紐づいていて、しばしば区分が区切られています。 EOS.IOソフトウェアは宣言型の許可管理システムを提供します。これは、誰が何をいつできるかに関して、きめ細かくハイレベルな制御をアカウントに付与します。

認証と許可管理は標準化されて、アプリケーションのビジネスロジックからは分離されていることが必須です。 これにより、汎用的なやり方に則した許可管理ツールの開発が可能になり、パフォーマンス最適化の機会が大幅に向上します。

全てのアカウントは、重み付けされたその他のアカウントと秘密鍵の組み合わせによって制御できます。 これは、現実における許可の構成方法を反映した階層的権限構造を生み出し、複数ユーザーの資金管理を容易にします。 複数ユーザー管理は、唯一にして最大のセキュリティであり、適切に使用することでハッキングによる盗難リスクを大幅に取り除くことができます。

EOS.IOソフトウェアのアカウントは、特定のメッセージを他アカウントに送ることが出来るキーとアカウントの組み合わせを定義することができます。 例えば、ソーシャルメディアアカウント用のキーと、取引所用の別のキーを設定することが可能です。 また、ユーザーの代わりに行動する権限を、キーの割り当てなしに、他ユーザーに与えることも可能です。

### 名前付きの許可レベル

<img align="right" src="http://eos.io/wpimg/diagram3.png" width="228.395px" height="300px" />

EOS.IOソフトウェアのアカウントは、名前付きの許可レベルを作成することが可能です。それぞれの許可レベルは、より高レベルな名前付き許可から派生させることができます。 それぞれの名前付きの許可レベルは、権限を定義することができます。権限は、他アカウントの名前付きの許可レベルとキーから成るマルチシグネチャーの閾値チェックです。 例えば、あるアカウントの"Friend"という許可レベルは、そのアカウントの友人のいずれかによって平等に制御されるアカウントに設定することができます。

他の例では、例えばSteemのブロックチェーンは"owner", "active", "posting"という3つのハードコーディングされた名前付き許可レベルを持っています。 "posting"は投票や投稿といったソーシャルアクションにのみ扱うことができ、一方で、"active"は"owner"の変更以外の全てを行うことができます。 "owner"はコールドストレージのためにあるもので、全てを行うことができます。 EOS.IOソフトウェアは、それぞれのアカウントに、独自のアクションのグループ化と階層構造の定義を許可することでこのコンセプトを一般化しています。

### 名前付きのメッセージ・ハンドラー・グループ

EOS.IOソフトウェアのアカウントは、それぞれのメッセージ・ハンドラーを名前付きの入れ子になったグループにまとめることができます。 他アカウントは、それぞれの許可レベルを設定する際に、これらの名前付きのメッセージ・ハンドラー・グループを参照できます。

最も高いレベルのメッセージ・ハンドラー・グループはアカウント名で、最も低レベルのものは、そのアカウントが受信した個々のメッセージタイプのものです。 これらのグループは以下のように参照できます: **@accountname.groupa.subgroupb.MessageType**.

このモデルでは、取引所コントラクトが注文の作成とキャンセルを入金と出金から切り離してまとめることが可能です。 取引所コントラクトによるこのようなグルーピングは、取引所のユーザーにとって便利です。

### 許可のマッピング

EOS.IOソフトウェアのそれぞれのアカウントは、あらゆるアカウントの名前付きメッセージ・ハンドラー・グループとそれら自身の名前付き許可レベルのマッピングを定義することができます。 例えば、あるアカウントの保有者は、そのアカウント保有者のソーシャルメディアアプリを、そのアカウントの"Friend"許可グループにマッピングすることができます。 これによって、あらゆる友達がそのアカウント保有者のソーシャルメディアに、そのアカウントの保有者として投稿することができます。 彼らがそのアカウントの保有者として投稿したとしても、彼らはメッセージの署名に彼ら自身のキーを使用します。 これにより、どの友達がどのようにそのアカウントを使用したのか常に識別することが可能です。

### 許可を評価する

"**Action**" タイプのメッセージを **@alice** から **@bob** に送る際、EOS.IOソフトウェアはまず **@alice** が **@bob.groupa.subgroup.Action** に関して許可のマッピングを定義しているか確認します。 もし何も見つからなかった場合、**@bob.groupa.subgroup** のマッピングを確認し、次に **@bob.groupa** 、そして最後に **@bob** 確認します。 もし一致するものが見つからなかった場合、仮のマッピングが **@alice.active** の名前付き許可グループになります。

マッピングが特定されると、署名権限はマルチシグネイチャーの閾値プロセスを利用して承認され、その権限は名前付き許可と紐づけられます。 もし失敗した場合、親の許可まで横断し、最終的にはオーナー許可 **@alice.owner** まで横断します。

<img align="center" src="http://eos.io/wpimg/diagram2grayscale2.jpg" width="845.85px" height="500px" />

#### デフォルトの許可グループ

EOS.IOの技術により、全てのアカウントは全ての権限を持った"owner"グループと、"owner"グループの変更以外の全ての権限を持った"active"グループを持つことができます。 その他全ての許可グループは"avtive"から派生します。

#### 許可の並列評価

許可の評価プロセスは読み取り専用で、トランザクションによって生じた許可の変更はブロックの終わりまで効力を持ちません。 これは全てのキーと全てのトランザクションの許可評価は並列で実行できるということを意味します。 加えて、ロールバックを必要とするコストのかかるアプリケーションのロジックを開始することなく、迅速な許可の承認が可能であることを意味します。 さらには、トランザクションの許可は、保留中のトランザクションを受け取った際に評価することができ、それらのトランザクションが適用された際に再評価する必要はありません。

全てを考慮に入れると、トランザクションを検証するために必要な計算能力のうちかなりの割合を許可の検証が占めます。 これを読み取り専用にすることと、自明的に並列化可能なプロセスによって劇的にパフォーマンスが向上します。

メッセージのログから決定的な状況を再構築するためにブロックチェーンを再生する際に、許可を改めて評価する必要はありません。 良いブロックとして認知されているブロックにトランザクションが格納されているという事実によって、このステップはスキップすることができます。 これにより、成長を続けるブロックチェーンの再生に関わる計算量を劇的に削減することができます。

## 強制的な遅延付きのメッセージ

時間はセキュリティにおける重要な要素です。 ほとんどの場合において、秘密鍵が盗まれたかどうかはその鍵が使用されるまで知ることができません。 インターネットに接続された普段付いのコンピュータに保存されているキーを必要とするアプリケーションを人々が持っている場合、時間ベースのセキュリティはより重要です。 EOS.IOソフトウェアを使うことで、アプリケーションの開発者は、特定のメッセージが適応可能になる前に、ブロックに格納されてから最低限の時間待つ必要があるということを示すことができます。 この待ち時間の間にメッセージはキャンセルすることができます。

いずれかのメッセージがブロードキャストされた際に、ユーザーはメールまたはテキストメッセージで通知を受けることができます。 もしユーザーがそのメッセージを承認していない場合、アカウント復旧プロセスを使うことで、アカウントを復旧し、メッセージを撤回することができます。

必要な遅延時間はオペレーションの取り扱いの難しさによって変わります。 コーヒーの支払いに遅延時間は必要なく、数秒以内に取り消し不可能になっても問題ありませんが、家の購入は72時間の決済期間が必要です。 アカウント全体を新しいコントロールに転送するには30日間かかることがあります。 遅延時間の選択はアプリケーションの開発者とユーザー次第です。

## 鍵の盗難からの復旧

EOS.IOソフトウェアでは、ユーザーが鍵を盗まれた際にアカウントの制御を復旧する方法があります。 アカウントの所有者は、指定されたアカウント復旧パートナーによる承認を受けることで、過去30日以内にアクティブだったあらゆる所有者キー使い、所有者キーのリセットをすることができます。 アカウント復旧パートナーは、アカウント所有者の協力なくアカウントの制御をリセットすることはできません。

このように既にアカウントは制御されているため、ハッカーが復旧プロセスを突破しようと試みることによって得られるものは何もありません。 加えて、もしハッカーがこのプロセスを突破したとしても、復旧パートナーが身分証明と（電話かメールでの）マルチファクター承認を求めるでしょう。 これにより、ハッカーはおそらく何も得ることができないでしょう。

このプロセスはシンプルなマルチ・シグネチャーとは大きく異なります。 マルチ・シグネチャーのトランザクションでは、実行される全てのトランザクションは別の会社が関与しますが、復旧プロセスでは代理人のみが関与し、代理人は日常の取引については全く力を持っていません。 これにより、全ての関係者の法的責任とコストは劇的に減少します。

# アプリケーションの決定的な並列実行

ブロックチェーンのコンセンサスは決定的（再現可能）な動作に依存しています。 これは全ての並列実行はミューテックまたは他のロックプリミティブから自由でなかればならないということを意味します。 ロックせずに全てのアカウントがそれぞれのプライベートなデータベースのみに読み書きできることを保証する方法がなければいけません。 これは、それぞれのアカウントはメッセージを連続的に処理し、アカウントレベルでは並列処理がされることを意味します。

EOS.IOソフトウェアベースのブロックチェーンでは、メッセージを並列して評価できるようにするため、メッセージを独立したスレッドに配信することがブロック生成者の仕事です。 それぞれのアカウントの状態は、そのアカウントに配信されたメッセージにのみ依存します。 スケジュールはブロック生成者のアウトプットで、これは決定的に実行されます。しかし、スケジュールを生成するプロセスは決定的である必要はありません。 これは、ブロック生成者がトランザクションをスケジュールに入れるために並列アルゴリズムを利用できることを意味します。

並列実行の一環は、スクリプトが新しいメッセージを生成した際に、直ちに配信されるのではなく、次のサイクルで配信されるようスケジュールされるということを意味します。 メッセージを直ちに配信することができない理由は、受信者が他スレッドの自身の状態を修正している可能性があるためです。

## コミュニケーション待ち時間の最小化

待ち時間は、あるアカウントが別のアカウントにメッセージを送信し、応答を受信するまでにかかる時間です。 ゴールは2つのアカウントがそれぞれのメッセージにつき3秒以内に1つのブロック内でメッセージのやりとりを行えるようにすることです。 これを可能にするために、EOS.IOソフトウェアはそれぞれのブロックをサイクルに分割します。 それぞれのサイクルはスレッドに分割され、それぞれのスレッドはトランザクションのリストを含みます。 それぞれのトランザクションは配信される一連のメッセージを含んでいます。 この構造はツリー構造で表すことができ、そこでは相互に重なった層が連続的に、また並列して処理されます。

        ブロック
    
          サイクル (連続)
    
            スレッド(並列)
    
              トランザクション (連続)
    
                メッセージ (連続)
    
                  レシーバーとノーティファイド・アカウント (並列)
    
    
    

あるサイクルで生成されたトランザクションは、後に続くあらゆるサイクルまたはブロックに配信することができます。 ブロック生成者は、wall clock timeの限界がすぎるか、または配信するトランザクションがなくなるまでサイクルをブロックに追加し続けます。

与えられたサイクルの中で、同じアカウントを修正したトランザクションを含むスレッドが2つないことを検証するために、ブロックの静的解析を利用することができます。 不変性が維持されている限り、全てのスレッドを並列して実行することによってブロックを処理することができます。

## 読み取り専用のメッセージ・ハンドラー

いくつかのアカウントは、そのアカウントの内部状態を変更することなく pass/fail 方式でメッセージを処理することができるかもしれません。 もしそうであれば、特定のアカウントの読み取り専用のメッセージ・ハンドラーが特定のサイクル内で一つ以上のスレッドに含まれている限り、これらのハンドラーは並列で実行することができます。

## 複数アカウントでのアトミック・トランザクション

時に、メッセージが自動で複数のアカウントに配信され承認されることを保証することが望ましい場合があります。 この場合、両方のメッセージは1つのトランザクションに配置され、両方のアカウントは同じスレッドと連続的に適用されるメッセージに割り当てられます。 この状況はパフォーマンス最適ではなく、ユーザーに使用料を請求する段階になると、ユーザーはトランザクションから参照したユニークなアカウントの数に基づいて請求を受けることになります。

パフォーマンスとコストの理由から、頻繁に利用されている2つ以上のアカウントを含むアトミック・オペレーションは最小限に抑えられるのがよいでしょう。

## ブロックチェーン状態の部分評価

ブロックチェーン技術のスケーリングのためにはコンポーネントがモジュール式であることが必要です。特にアプリケーションのわずかな一部のみを利用する必要がある場合において、アプリケーションの全てを実行する必要はありません。

取引所のアプリケーション開発者は、取引所の状態をユーザーに表示するためにフルノードを実行します。 この取引所アプリはソーシャルメディアアプリに関連する状態を必要としません。 EOS.IOソフトウェアのあらゆるフルノードは、実行するアプリケーションの任意のサブセットを選択できます。 あるアプリケーションの状態はそのアプリケーションに配信されたメッセージから完全に派生するものであるため、他のアプリケーションに配信されるメッセージは安全に無視されます。

これは他のアカウントとのコミュニケーションにおいて重要な意味を持ちます。 最も重要なことは、同じマシン上で他のアカウントの状態にアクセス可能だと仮定することはできないということです。 これは、あるアカウントが他のアカウントを同期的に呼び出せるようにする"lock"を有効にしたくなった時に、もし他のアカウントがメモリに常駐していなかった場合、この設計パターンは正常に動かなくなります。

アカウント間の全てのステート・コミュニケーションは、ブロックチェーンに含まれるメッセージを通して渡される必要があります。

## 主観的な最善努力型のスケジューリング

EOS.IOソフトウェアは、ブロックプロ生成者に任意のメッセージを任意のアカウントに送るよう義務付けることはできません。 それぞれのブロック生成者は、トランザクションを処理するために必要な計算量と時間について独自の個人的な測定を行います。 これは、トランザクションがユーザーによって作られたか、スクリプトによって自動的に作られたかを適用します。

EOS.IOを採用しているローンチされたブロックチェーン上では、ネットワークレベルで全てのトランザクションは、実行に0.1msかかるか上限の10msかかるかに関わらず固定の計算帯域幅コストを請求されます。 しかしながら、ソフトウェアを使用している個々のブロック生成者は独自のアルゴリズムと測定によってリソース使用状況を計算するでしょう。 ブロック生成者がトランザクションまたはアカウントが不釣り合いな計算能力を消費しているとみなした場合、彼らは彼ら自身のブロックを生成する際に、そのトランザクションを拒否します。しかしながら。他のブロック生成者がそのトランザクションを有効だとみなした場合、ブロック生成者はそのトランザクションを処理します。

一般的に、一人でもブロック生成者があるトランザクションをリソース使用状況の制限において有効だとみなす限り、他の全てのブロック生成者もそのトランザクションを受け入れます。しかし、そのトランザクションが生成者を見つけ出すためには最大で1分間かかる可能性があります。

いくつかのケースにおいて、ブロック生成者は許容可能範囲を超えた桁のトランザクションを含むブロックを生成する可能性があります。 この場合、次のブロック生成者はこのブロックを拒否することを選択するでしょう。そして、このつながりは第三のブロック生成者によって破壊されます。 これは、大きなブロックがネットワーク伝播遅延を起こした場合に生じるものとなんら変わりはありません。 コミュニティは悪用のパターンに気づき、最終的に不正なブロック生成者から表を取り除くでしょう。

この主観的な計算コストの評価により、ブロックチェーンは正確かつ決定的に何かを実行するためにどの程度の時間がかかるかを測定する必要がなくなります。 この設計により、正確に命令をカウントする必要がなくなり、コンセンサスを破ることなく最適化の機会を劇的に向上させることができます。

# トークンモデルとリソースの使用状況

**注意事項: 当ホワイトペーパーにおける暗号トークンとは、EOS.IOソフトウェアを採用してローンチされたブロックチェーン上の暗号トークンであり、 EOS Token Distributionを通じてイーサリアムブロックチェーン上で配布されたERC20トークンではありません。**

全てのブロックチェーンはリソースに制約があり、濫用を防ぐシステムが必要です。 EOS.IOを使用したブロックチェーンでは、アプリケーションによって消費されるリソースは3つに大別されます。

1. 帯域幅とログストレージ(ディスク)
2. 計算と計算バックログ(CPU)
3. ステート・ストレージ(RAM)

帯域幅と計算は、瞬間的使用と短期的使用の2つのコンポーネントがあります。 ブロックチェーンは全てのメッセージのログを保持し、このログは最終的に全てのフルノードによって格納され、ダウンロードされます。 メッセージのログにより、全てのアプリケーションの状態を再構築することができます。

計算債務とは、メッセージログから状態を再生成するために実行されなければいけない計算です。 計算債務が大きくなりすぎた場合、ブロックチェーンの状態のスナップショットを取得し、ブロックチェーンの履歴を破棄することが必要になります。 計算債務があまりに早く増大した場合、1年分のトランザクションを再生するために6ヶ月の時間を要する可能性があります。 したがって、計算債務は慎重に管理されることが重要です。

ブロックチェーン・ステート・ストレージはアプリケーションのロジックからアクセス可能な情報です。 これにはオーダーブックやアカウントの残高などの情報が含まれています。 もし状態がアプリケーションによって読み込まれなかった場合、その状態は保存されるべきではありません。 例えば、ブログの投稿内容やコメントはアプリケーションのロジックによって読み込まれないため、ブロックチェーンの状態に保存されるべきではありません。 一方で、投稿やコメントの存在や、投票数、その他のプロパティはブロックチェーンの状態の一部として保存されます。

ブロックの生成者は、利用可能な彼らの帯域幅と計算力、状態の容量を公開します。 EOS.IOのそれぞれのアカウントは、3日間のステーキング・コントラクトで保持されているトークンの量に比例した割合の利用可能容量を消費することができます。 例えば、EOS.IOソフトウェアベースのブロックチェーンがローンチされ、もしあるアカウントがそのブロックチェーンに準じて配布可能な全トークンのうち1％を保持していた場合、そのアカウントはステート・ストレージ容量のうち1%を利用できる見込みがあります。

EOS.IOソフトウェアをローンチされたブロックチェーンに適用することは、帯域幅と計算能力を部分準備基準で割り当てることを意味します。なぜなら、（未使用の容量は将来の使用のために残しておくことはできず）帯域幅と計算能力は一時的なものだからです。 EOS.IOソフトウェアで使用されているアルゴリズムは、Steemで利用されているアルゴリズムと帯域幅利用レート制限の点で似ています。

## 客観的測定と主観的測定

前述したように、計算使用料をインストルメント化することはパフォーマンスと最適化に大きな影響を持ちます。そのため、全てのリソース使用制限は最終的には主観的であり、ブロック生成者が個々のアルゴリズムと見積もりに基づいて実行されます。

つまり、客観的に測定するには些細なものもあります。 配信されたメッセージの数と、内部データベースに格納されているデータのサイズは客観的に測定するにはチープです。 EOS.IOソフトウェアのブロック生成者は、こうした客観的測定に同様のアルゴリズムを適応できますが、主観的測定により厳格な主観的アルゴリズムを適用するでしょう。

## 受領者の支払い

これまでは、オフィスの賃貸や、計算能力、その他のビジネスを運営するために必要なコストを支払うことがビジネスでした。 顧客はそのビジネスから特定の商品を購入し、その収入がビジネスの運営費をまかなっていました。 同様に、ウェブサイトの運営費をまかなうために、サイトの訪問者に対して少額の支払いを義務付けているウェブサイトはありません。 したがって、分散型アプリケーションは、利用者にブロックチェーンの利用料の直接的な支払いを強制するべきではありません。

EOS.IOソフトウェアを使用してローンチされたブロックチェーンは、ユーザーにブロックチェーンの使用料を直接支払わせることはなく、また、それによよって事業が商品の独自の収益戦略を決定することを阻害したり、制約を課したりすることはありません。

## 容量の委譲

EOS.IOを採用してローンチしたブロックチェーン上の、利用可能な帯域幅の一部または全てを今すぐに利用する必要のないトークン保有者は、未使用の帯域幅を他者に与える、または貸し出すことができます。そのブロックチェーン上でEOS.IOソフトウェアを実行しているブロック生成者は、その容量の委譲を確認し、ただちに帯域幅の割り当てを行います。

## トークンの価値から独立したトランザクション費用

EOS.IOソフトウェアの主な利点の一つとして、アプリケーションに利用可能な帯域幅の送料があらゆるトークンの価値から完全に独立していることが挙げられます。 もしアプリケーションの保有者がEOS.IOソフトウェアを採用したブロックチェーン上の適切な寮のトークンを所有していた場合、そのアプリケーションは固定された状態と帯域幅利用の中で無期限に実行されることができます。 そのような場合、開発者とユーザーはトークンマーケットでのあらゆる価格変動の影響を受けることはなく、それゆえプライス・フィードに依存しません。 言い換えると、EOS.IOソフトウェアのブロック生成者は、トークンの価値に依存せず、トークンあたりの利用可能な帯域幅や、計算、ストレージを自然に増加させることができるということです。

EOS.IOソフトウェアを使用したブロックチェーンは、ブロック生成者がブロックを生成するたびに褒賞を与えます。 トークンの価値は、ブロック生成者が購入可能な帯域幅や、計算、ストレージの量に影響を与えます。このモデルは、ネットワークのパフォーマンスを向上させるために、トークンの価値を上昇させることを自然に活用しています。

## ステート・ストレージ・コスト

帯域幅と計算が委譲できる一方で、アプリケーション状態のストレージは、アプリケーションの開発者に、その状態が削除されるまでトークンを保有しておくことを要求します。 もし状態が削除されない場合、トークンは効率的にサーキュレーションから取り除かれます。

全てのユーザーは一定のストレージ量を必要とします。それゆえ、全てのアカウントは最低限の残高を保持しなければいけません。ネットワークのストレージ容量が増加するにつれて、この最低限の残高は減少します。

## ブロック報酬

EOS.IOソフトウェアを採用したブロックチェーンでは、ブロック生成者はブロックを生成するたびに、新たなトークンを受け取ります。 このような状況で、作成されるトークンの数は、全てのブロック生成者によって公開された希望報酬額の中央値によって決定されます。 EOS.IOソフトウェアは、年次のトークン供給量の増加が5%を超えないようにするといったように、ブロック生成者の報酬の上限を制限するよう設定することもできます。

## コミュニティ・ベネフィット・アプリケーション

ブロック生成者を選任することに加えて、EOS.IOソフトウェアベースのブロックチェーンに従って、ユーザーはスマートコントラクトとしても知られる3つのコミュニティ・ベネフィット・アプリケーションを選ぶことができます。 これらの3つのアプリケーションは、ブロック生成者に支払われたトークンを除いた年間供給量のうち定められた割合までのトークンを受け取ることができます。 これらのスマートコントラクトは、トークン保有者から受け取った票数に比例してトークンを受け取ります。 選任されたアプリケーションまたはスマートコントラクト、トークン保有者によって新たに選任されたアプリケーションまたはスマートコントラクト によって置き換えられる可能性があります。

# ガバナンス

ガバナンスとは、ソフトウェアのアルゴリズムによって完全に補足することができない主観的な事項に基づいて人々が合意に至るプロセスです。 EOS.IOソフトウェアベースのブロックチェーンは、ブロック生成者の既存の影響を効率的に管理するガバナンス・プロセスを実装しています。 定義されたガバナンス・プロセスが存在しない場合、以前のブロックチェーンは、アドホックで非公式で、しばしば論議を呼んでいるガバナンス・プロセスに依存し、結果的に予期しない結果をもたらします。

EOS.IOソフトウェアベースのブロクチェーンは、パワーをブロック生成者に委任しているトークン保有者から生じるパワーを認識します。 ブロック生成者には、アカウンの凍結と、欠陥のあるアプリケーションのアップデート、および内部プロトコルへのハードフォークの変更を提案するための制限付きの権限が与えられます。

EOS.IOソフトウェアに組み込まれるということは、ブロック生成者の選任です。 ブロックチェーンのあらゆる変更が行われる前に、これらのブロック生成者はそれを承認する必要があります。 もしブロック生成者が、トークン保有者によって要求された変更を拒否した場合、トークン保有者は投票により退任させることができます。 もしブロック生成者がトークン保有者の許可なく変更を加えた場合、全ての非生産フルノード承認者（取引所など）はその変更を拒否することができます。

## アカウントの凍結

スマートコントラクトは時に予期せぬ挙動により意図していた実行に失敗することがあります。またある時には、アプリケーションまたはアカウントは、異常な寮のリソースを消費する脆弱性を発見することがあります。 そのような問題が起きることは不可避であるため、ブロック生成者はそのような状況を強制するためのパワーを備えておく必要があります。

全てのブロックチェーン上のブロック生成者は、どのトランザクションがブロックに含まれるか選択する力があり、これにより、アカウントを凍結することができます。 EOS.IOソフトウェアを使用しているブロックチェーンは、この権限を形式化していて、アクティブなブロック生成者の17/21の票によってアカウント凍結のプロセスをかけることができます。 もしブロック生成者がこの力を濫用した場合には、ユーザーはそのブロック生成者を投票により退任させてアカウントの凍結を解除することができます。

## アカウントコードの変更

予期せぬ形で止めることのできないアプリケーション（"unstoppable application")が始動し、他に対処する手がなくなってしまった場合、EOS.IOソフトウェアを使用しているブロックチェーンのブロック生成者は、ブロックチェーン全体をハードフォークすることなくそのアカウントコードを取り替えることができます。 アカウントの凍結プロセスと同様に、このコードの取り換えは選任されたブロック生成者の17/21の票が必要です。

## 規約

EOS.IOソフトウェアをを使用したブロックチェーンは、規約（"constitution")と称されるP2Pの利用規約合意または拘束力のある契約を、それらに署名したユーザーとの間で結ぶことができます。 この規約の内容は、コードによって完全に強制されることのできないユーザー間の義務を定義し、相互に受け入れられたルールに基づいた法の選択と権限の確立による議論の解決を促進します。 ネットワークに公開される全てのトランザクションは、署名の一環として規約のハッシュを内包する必要があり、それによって署名者をそのコントラクトに明示的に紐付けます。

規約は人間が読める形式のソースコードプロトコルの趣旨を定義します。 この趣旨は、エラーが生じた際のバグと機能の判別に使われ、コミュニティにどの部分を修正するのが適切または不適切かを伝えます。

## プロトコルと規約のアップグレード

EOS.IOソフトウェアは、正当なソースコードと規約によって定義されたプロトコルが以下のような手順でアップデートできるようになるプロセスを定義します:

1. ブロック生成者が規約の変更を提案し、17/21の賛成を得ます
2. ブロック生成者が30日間連続で17/21の承認を維持します
3. 全てのユーザーが新しい規約のハッシュによる署名を要求されます
4. ブロック生成者が、規約の変更を反映するために、ソースコードへの変更を適用し、その変更をgitコミットのハッシュを用いてブロックチェーンに提案します。
5. ブロック生成者が30日間連続で17/21の承認を維持します
6. コードの変更は7日後に発効されます。全てのフルノードにはソースコードの批准後に1週間のアップグレード期間が与えられます。
7. 新たなコードにアップグレードしなかった全てのノードは自動的にシャットダウンされます。

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