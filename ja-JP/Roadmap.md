## EOS.IO ソフトウェアロードマップ

当ドキュメントは、大まかな開発計画をまとめたものであり、バージョン1.0.に向けた進捗があった際に更新されます。当ロードマップは、ブロックチェーンソフトウェアのみに適応されるものであり、その他のウォレットやブロック・エクスプローラ等のツールに適応されるものではありません。ウォレット等のその他のツールに関しては、Phase1完了時に個々に専用のチームとロードマップが構築されます。

***当ドキュメントに記載されている全ての内容は情報提供のみを目的としたもので、草稿段階であり、突然変更される可能性があります。 block.one は、当ロードマップに記載されている情報について正確性を保証しません。また、これらの情報はあくまで現段階におけるものであり、暗示、黙示に関わらずいかなる責任や保証も負いません。***

# Phase1 最低限の実行可能なテスト環境 - 2017年 夏 -

このフェーズの目的は、EOS.IO上で開発者がアプリケーションの構築・テストを開始するために必要なAPIを構築することです。 開発者がアプリケーションの開発を始めるためには、以下の機能が実装されている必要があります：

### スタンドアロン・ノード(Dan & Nathan)

スタンドアロン・ノードは、APIを公開している間、テスト・ブロックチェーンを運用し、ブロックを生成します。このノードは、P2Pネットワークコードを扱う必要はありません。

### ネイティブ・コントラクト(Nathan)

EOS.IOソフトウェアは、多くのネイティブ・コントラクトを持っています。これらのコントラクトは、WebAssemblyインターフェイス外にあり、ブロックチェーン運営の中核を管理します。ネイティブ・コントラクトには以下のものがあります：

1. @eos - EOSトークンの転送を管理
2. @stake - ロックされたEOSの管理、ブロック生成者の選挙、投票の管理
3. @system - 承認、メッセージ、コンタクトコード更新の管理

### バーチャル・マシンAPI(Dan)

コントラクトはWebAssembly(WASM)にコンパイルされます。WASMは規定のAPIを経由してブロックチェーンと連携していなければなりません。 このAPIは開発者がアプリをビルドする際に依存するもので、開発者がEOS上でのビルドを実際に開始できるようになる前に、比較的安定したものになります。

### RPCインターフェイス(Arhag, Nathan)

HTTPインターフェイス上で動作するシンプルなJSON-RPCが提供され、開発者はトランザクションを公開したり、アプリの状態を問い合わせたりすることが可能になります。 これはテストアプリを公開したり、テストアプリとやりとりする際に重要な意味を持ちます。

### コマンドラインツール(Arhag)

コマンドラインツールは、RPCインターフェイスと開発者のビルド環境の統合を容易にします。

### ベーシック・デベロッパー・ドキュメンテーション（Josh)

EOS.IO上でのブロックチェーン構築を開始する方法をまとめた開発者向けのドキュメントです。このドキュメントは、WASM API、RPC Interface、コマンドラインツールに関するドキュメントを含みます。

# Phase2 最低限の実行可能なテストネットワーク - 2017年 秋 -

Phase1における全てのものは、開発者自身のコードのみを実行する信頼された環境を前提としていました。テストネットワークがデプロイできるようになる前に、追加的ないくつかの機能が実装・テストされる必要があります。

### P2Pネットワークコード(Phil)

P2Pネットワークコードは、、二つのスタンドアロン・ノード間でのブロックチェーンの同期のためのプラグインです。

### WASMサニテーション & CPUサンドボクシング(Brian)

WASMコードは、浮動小数点演算や無限ループ等の非決定的な動作を点検するためにサニタイズする必要があります。

### リソース・利用状況のトラッキング & レート・リミッティング(Arhag)

濫用を防ぐため、リソースの監視と使用状況トラッキングのレートは、ステークされているEOSに応じてユーザーを制限します。

### ジェネシス・インポート・テスティング(DappHub)

EOSトークン配布状況のデータをエクスポートし、ジェネシス・コンフィギュレイション・ファイルを作るために、いくつかのツールが開発される必要があります。 これにより、誰でもトークン配布に参加し、最初のテストEOS（TEOS）を入手することができるようになります。

### インターブロクチェーン・コミュニケーション(Nathan)

この機能は、トランザクションのマークルハッシュが適切であることの検証を含みます。

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