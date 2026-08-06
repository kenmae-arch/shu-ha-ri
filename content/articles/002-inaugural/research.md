# 創刊号テーマ選定のためのトレンド調査（英語圏テック・直近2〜6週間）

- 調査日: 2026-08-06（担当: リサーチ）
- 調査方法に関する重要な注記: 本調査環境ではWebFetch（ページ直接取得）が全ドメインでHTTP 403となり使用できなかったため、**すべてWebSearch（検索結果とその引用）に基づく**。公式ページのURLは検索結果で確認できたものを記載しているが、**ページ本文の直接確認はできていない**。確度欄は「複数の独立ソースで一致＝確認済み（検索ベース）」「1系統のソースのみ＝単一情報源」「確認できず＝未確認」とした。
- 数値（スター数・ベンチマーク等）は情報源により差があるものが多い。差がある場合はその旨を明記した。

---

## 調査事項1: GitHubで急上昇している注目リポジトリ（AI・開発ツール系）

### 1-1. usestrix/strix — 自律型AIペネトレーションテストエージェント

**わかったこと**
- OSSのAIペネトレーションテストツール。自律エージェントがアプリを実際に動かして脆弱性を探し、PoC（動く証拠）で検証し、修正パッチまで生成するとされる。Apache 2.0。
- スター数の伸び: 2026-07-03時点で32.8k → 7月中に39.4k（週+10,300超との記述）→ 43k超 → GitHub組織ページ表記46k、と情報源により幅があるが、「2026年で最も急成長したセキュリティ系リポジトリの一つ」という点は複数ソースで一致。正確な現在値は直接確認できなかった。

**出典（確認日: 2026-08-06）**
- https://github.com/usestrix/strix
- https://openllm.wavise.com/blog/strix-ai-penetration-testing
- https://www.coddykit.com/pages/blog-detail?id=512896&slug=strix-the-open-source-ai-penetration-testing-tool-that-finds-and-fixes-your-app-
- https://appsecsanta.com/strix

**確度**: 急成長の事実は確認済み（検索ベース・複数ソース一致）。スター数の正確な現在値は未確認（ソース間で32.8k〜46kの幅）。

### 1-2. xai-org/grok-build — xAIのコーディングエージェントCLI（2026-07-15にオープンソース化）

**わかったこと**
- xAIがターミナル型コーディングエージェント「Grok Build」のソースを2026-07-15にApache 2.0で公開。Rust製で、エージェントループ、ファイル編集・シェル実行ツール、TUI、skills/plugins/hooks/MCP/サブエージェントの拡張系まで含む。行数は「844,530行」とするソースと「100万行超」とするソースがあり不一致。
- スター数は公開直後（7月中旬）で約4.5kとの記述（単一情報源）。
- 重要な留意点: Apache 2.0だが**外部からのPull Requestは受け付けない**方針で、「コミュニティ主導のOSSではなくソース透明性の開示」と位置づける批評がある（2026-08-04付の批評記事）。
- 公開の経緯に大きな文脈がある（後述の調査事項3-2「Grok Build情報流出事件」参照。流出発覚の数時間〜数日後にオープンソース化された、と複数ソースが報道）。

**出典（確認日: 2026-08-06）**
- https://devops.com/xai-open-sources-grok-build-coding-agent-after-cloud-upload-exposes-ssh-keys-repos/
- https://www.marktechpost.com/2026/07/15/spacexai-open-sources-grok-build-the-rust-agent-harness-tui-and-tool-layer-behind-its-coding-cli/
- https://bex.co/blog/2026/08/04/xai-grok-build-apache-2-read-only-open-source-washing
- https://explainx.ai/blog/grok-build-open-source-spacexai-july-2026

**確度**: 公開日・ライセンス・外部PR不受理は確認済み（検索ベース・複数ソース一致）。スター数・正確な行数は単一情報源または不一致あり。

### 1-3. perplexityai/bumblebee — 読み取り専用サプライチェーンスキャナ

**わかったこと**
- Perplexityが社内ツールを2026-05-22にOSS化（※直近6週間より少し前の公開だが、7月のエージェント・セキュリティ文脈で言及が続いている）。Go製・Apache 2.0。
- 特徴: パッケージマネージャを一切実行しない「読み取り専用」設計。npm/pnpm/Yarn/Bun/PyPI/Go modules/RubyGems/Composerの8エコシステムに加え、**MCP設定ファイル**、VS Code/Cursor/Windsurf系拡張、ブラウザ拡張のメタデータを検査。「スキャン自体が感染経路になる」問題を構造的に回避する設計思想が特徴として繰り返し言及される。
- スター数: 公開1週間未満で1,450 → その後4.8k超（最終コミット2026-06-25時点との記述）。現在値は未確認。

**出典（確認日: 2026-08-06）**
- https://github.com/perplexityai/bumblebee
- https://www.perplexity.ai/hub/blog/perplexity-is-open-sourcing-bumblebee （公式ブログ。本文は直接確認できず）
- https://www.marktechpost.com/2026/05/23/perplexity-open-sources-bumblebee-a-read-only-supply-chain-scanner-for-developer-endpoints/
- https://byteiota.com/bumblebee-perplexity-supply-chain-scanner-developer/

**確度**: 公開日・設計思想・対応範囲は確認済み（検索ベース・複数ソース一致）。スター数現在値は未確認。

### 1-4. diegosouzapw/OmniRoute — 無料のマルチプロバイダAIゲートウェイ

**わかったこと**
- MITライセンスの無料AIゲートウェイ。OpenAI互換の単一エンドポイントで多数のプロバイダ・モデルにルーティング。Claude Code/Codex/Cursor/Cline/OpenCode等のクライアントから利用でき、クォータ検知の自動フォールバック、トークン圧縮（「RTK+Caveman圧縮で15〜95%削減」とリポジトリ説明に記載）、MCP/A2A対応を謳う。
- 数値はソース間の揺れが大きい: プロバイダ数「231+」「278+」「290+（うち90+無料）」、スター数「9.3k」→「20k超（1日で+1,343）」→「26,423（1日約+2,000ペース）」。急成長中で数値が短期間に変わっている可能性が高いが、現在値は未確認。
- リポジトリには公式の日本語READMEが存在する。

**出典（確認日: 2026-08-06）**
- https://github.com/diegosouzapw/OmniRoute
- https://nerdzap.com/news/omniroute-open-source-ai-gateway-free-tokens/
- https://www.coddykit.com/pages/blog-detail?id=512944&slug=omniroute-the-free-ai-gateway-that-routes-500-models-through-one-endpoint-20-000
- https://rohitraj.tech/en/notes/omniroute-ai-gateway-review-2026

**確度**: リポジトリの存在・急成長の傾向は確認済み（検索ベース・複数ソース一致）。スター数・プロバイダ数の正確な値は未確認（ソース間で大きな幅）。

### 1-5. 7月のGitHubトレンド全体の傾向

**わかったこと**
- 複数のトレンドまとめが共通して指摘するのは「研究論文発リポジトリからエージェント実装・インフラへ」のシフト。コーディングエージェント、ペンテストエージェント、MCPサーバー、AIゲートウェイ、エージェント向けセキュリティツールがトレンド上位を占める。

**出典（確認日: 2026-08-06）**
- https://www.analyticsvidhya.com/blog/2026/07/trending-ai-github-repositories/
- https://www.ai.joaoqueiros.com/blog/github-trending-weekly-daily-ai-builder-repositories-july-2026
- https://ossinsight.io/trending/ai

**確度**: 傾向としては確認済み（複数のまとめが一致）。ただしいずれも二次情報のまとめ記事。

---

## 調査事項2: 直近の主要なAI・開発ツールのリリース・発表

### 2-1. OpenAI GPT-5.6ファミリー（2026-07-09一般公開）

**わかったこと**
- 3バリアント構成: Luna（最速・最安）/ Terra（中間）/ Sol（フラッグシップ、Sol Ultra高負荷モードあり）。2026-06-26に限定プレビュー、規制当局（米商務省）の承認を経て07-09に一般公開、という経緯が報じられている。
- API価格（100万トークンあたり入力/出力）: Sol $5/$30、Terra $2.50/$15、Luna $1/$6。**2026-07-30にLunaを80%、Terraを20%値下げ**したとの記述あり（単一系統の情報源）。
- OpenAI公表ベンチマーク（二次ソース経由）: SWE-Bench Pro = Sol 64.6% / Terra 63.4% / Luna 62.7%。Terminal-Bench 2.1 = Sol 88.8% / Terra 87.4% / Luna 84.7%。
- 同日07-09に「ChatGPT Work」（チームのアプリ・ファイル・ワークフローから文脈を取得し、文書・スプレッドシート・プレゼン・レポートを自律作成）も発表。

**出典（確認日: 2026-08-06）**
- https://openai.com/index/gpt-5-6/ （公式。本文は直接確認できず）
- https://en.wikipedia.org/wiki/GPT-5.6
- https://www.axios.com/2026/07/09/ai-openai-gpt-release
- https://www.cnbc.com/2026/07/08/openai-expanding-gpt-5point6-ai-model-release-ending-government-limits.html
- https://artificialanalysis.ai/articles/gpt-5-6-has-landed
- https://www.orcarouter.ai/blog/gpt-5-6-api-pricing （7/30値下げの記述）

**確度**: リリース日・バリアント構成は確認済み（検索ベース・複数ソース一致）。価格・ベンチマーク数値は二次ソース経由のため要再確認。7/30値下げは単一情報源。

### 2-2. Anthropic Claude Sonnet 5（2026-06-30）

**わかったこと**
- 「最もエージェント的なSonnet」と位置づけ。導入価格$2/$10（100万トークン、2026-08-31まで）、以後$3/$15。
- 二次ソース経由のベンチマーク: SWE-Bench Pro 63.2%（Sonnet 4.6: 58.1%、Opus 4.8: 69.2%）、OSWorld-Verified 81.2%、Terminal-Bench 2.1 80.4%、BrowseComp 84.7%。「Opus級の約63%性能をOpusの40%の価格で」という整理が複数ソースで共通。
- 関連: Claude Coworkが全有料プランでGA、モバイル/Web対応拡大。※Cowork自体の初出は2026年1月（日本語ソースによる）で、7月の動きは「拡大」。「7月7日ローンチ」と書く英語まとめもあり日付情報が錯綜している。

**出典（確認日: 2026-08-06）**
- https://www.anthropic.com/news/claude-sonnet-5 （公式。本文は直接確認できず）
- https://www.macrumors.com/2026/06/30/anthropic-claude-sonnet-5/
- https://www.datacamp.com/blog/claude-sonnet-5
- https://dev.to/ai_made_tools/claude-sonnet-5-complete-guide-to-benchmarks-pricing-and-features-2026-1n5b
- Cowork日本語情報: https://aismiley.co.jp/ai_news/claude-cowork-agent-os-vm/ ほか

**確度**: リリース日・価格は確認済み（検索ベース・複数ソース一致）。ベンチマークは二次ソース経由。Coworkの7月の変更点の正確な内容は未確認（ソース間で記述が錯綜）。

### 2-3. Moonshot AI「Kimi K3」（発表2026-07-16、オープンウェイト公開2026-07-26）

**わかったこと**
- 2.8兆パラメータのMoE（アクティブ104B）、100万トークンコンテキスト、ネイティブビジョン。「オープン3Tクラス」としては初と主張。
- Moonshot自己報告ベンチマークでは Claude Opus 4.8 max・GPT-5.5 high をおおむね上回り、最上位プロプライエタリ（GPT-5.6 Sol等）には及ばない、という位置づけ。
- Simon Willison、Nathan Lambert（Interconnects）ら英語圏の著名な書き手が個別に論評しており、コミュニティの注目度が高い。

**出典（確認日: 2026-08-06）**
- https://simonwillison.net/2026/Jul/16/kimi-k3/
- https://venturebeat.com/technology/chinas-moonshot-ai-releases-kimi-k3-the-largest-open-source-model-ever-rivaling-top-u-s-systems
- https://www.interconnects.ai/p/kimi-k3-the-open-weights-escalation

**確度**: 確認済み（検索ベース・複数ソース一致）。ベンチマークはベンダー自己報告。

### 2-4. DeepSeek V4-Flash-0731（2026-07-31正式公開）

**わかったこと**
- アーキテクチャ・サイズ不変（総284B/アクティブ13BのMoE、1Mコンテキスト）のまま、ポストトレーニングのみ刷新してエージェント性能を大幅向上。9つのエージェント系ベンチマーク全てで自社上位のV4-Pro-Previewを上回ったと主張。
- 数値例（ベンダー報告）: Terminal Bench 2.1 = 82.7（V4-Pro-Preview 72.1）、DeepSWE = 54.4（Flash Preview 7.3から大幅上昇）。API価格は入力$0.14/100万トークン。MITライセンス。
- 注意: 評価は「DeepSeek Harness minimal mode・max tier・top_p 0.95・temperature 1.0」での自己評価であり、エージェント系スコアはハーネス依存性が極めて高い、という但し書きが英語圏の報道でも付いている。

**出典（確認日: 2026-08-06）**
- https://deepseek.ai/blog/deepseek-v4-flash-ga-agent-benchmarks （公式ブログ。本文は直接確認できず）
- https://www.marktechpost.com/2026/07/31/deepseek-upgrades-deepseek-v4-flash-0731-with-major-agentic-and-coding-gains/
- https://www.techtimes.com/articles/322513/20260731/deepseek-retrained-v4-flash-beats-its-flagship-pro-nine-agent-benchmarks.htm

**確度**: リリース・仕様は確認済み（検索ベース・複数ソース一致）。ベンチマークはベンダー自己報告。

### 2-5. その他のリリース（裏取り薄め）

**わかったこと**
- 2026-07-17〜23の1週間に7つの注目モデルが集中リリースされたとするまとめあり: Kimi K3、Qwenの3連続リリース（72時間で3本）、GoogleのGemini 3.6 Flash / 3.5 Flash-Lite / 3.5 Flash Cyber、poolside Laguna S 2.1、Ant Ling-3.0-flash。Black Forest LabsのFLUX 3（動画・音声・ロボット行動予測を単一ウェイトで生成、動画は音声付き最大20秒）も同時期に発表とされる。
- xAI Grok 4.5（トークン効率を訴求）もこの数週間のリリースとして言及される。
- Googleの「CodeMender」プレビュー（コードベースの脆弱性をスキャンし修正を生成するエージェント、C/C++/Go/Java/Python/Ruby/Rust/TS対応）が7月のニュースとして言及される。

**出典（確認日: 2026-08-06）**
- https://www.digitalapplied.com/blog/seven-days-seven-releases-july-2026-model-wave
- https://blog.google/innovation-and-ai/technology/ai/google-ai-updates-july-2026/ （公式。403のため本文未確認）
- https://thehackernews.com/2026/07 （CodeMender言及）

**確度**: 単一情報源（Gemini 3.6 Flash群・Qwen・Laguna・Ling・FLUX 3・Grok 4.5・CodeMenderはいずれも1系統のソースのみで、公式発表の直接確認ができていない）。記事で使う場合は要再確認。

---

## 調査事項3: Hacker News・英語圏コミュニティで盛り上がっているテーマ

### 3-1. 「2x, not 10x: coding with LLMs in 2026」（HNフロントページ1位、2026-07-31）

**わかったこと**
- 2026-07-31にHNフロントページ1位になったエッセイ（obryant.dev）。HNアイテムID 49047839。主旨: 2026年にLLM導入が加速したのは「自動フィードバックループの中で回せるだけの信頼性に達したから」であり、モデル改良の追加的な生産性効果は逓減する。10倍化はモデル改良からは来ず、「今ある能力を前提に業界がリツーリングすること」から来る、という仮説。受け入れ基準が客観的に検証可能なタスクで約2倍、という控えめな見積もり。
- ポイント数・コメント数は直接確認できなかった（HN APIへのアクセス不可）。
- 周辺文脈: HNの論調が「熱狂」から「実務評価」へ移行しているという分析が複数ある。評価軸は価格・セッション制限・コンテキスト挙動・ハーネス設計・ワークフロー摩擦に移り、「AIスロップ（低価値な生成物の洪水）」への苛立ちと、「力の乗数としてのAI」との区別が明確化。エージェント生成PRの研究では「どの単一エージェントも全タスクカテゴリで優位ではなく、ツールの質はタスクの形状に依存する」との知見（元研究の特定は末尾「追加調査（レビュー対応）」の追-1参照）。

**出典（確認日: 2026-08-06）**
- https://obryant.dev/p/2x-not-10x/
- https://news.ycombinator.com/item?id=49047839
- https://blog.mean.ceo/hacker-news-trends-july-2026/
- https://www.developersdigest.tech/blog/what-hacker-news-gets-right-about-ai-coding-agents-2026

**確度**: エッセイの存在・HN1位（7/31時点）は確認済み（検索ベース・複数ソース一致）。ポイント数は未確認。周辺の論調分析は二次情報。

### 3-2. Grok Build情報流出事件（2026-07-12〜15）

**わかったこと**
- 2026-07-12、セキュリティ研究者（Cereblab名義）がネットワークレベルの解析を公開。Grok Build CLIが**リポジトリ全体（git履歴・削除済みシークレット含む）をGoogle Cloud Storageのバケット（grok-code-session-traces）へ送信**していたと報告。タスク遂行に必要な量の約27,800倍、5.1GBを73チャンクで送信したケースが示された。SSH鍵・認証情報・コミット履歴が対象に含まれ、ホームディレクトリで実行したユーザーはパスワードマネージャのDBや個人文書まで送信されたと報告。
- 「モデル改善に使わせない」プライバシートグルは学習利用の可否のみを制御し、**送信自体は止めていなかった**。
- xAIは07-13にサーバー側で送信を停止（セキュリティアドバイザリやバージョンノートなし、と批判あり）。Muskはアップロード済みデータの削除を約束。07-15にGrok Buildをオープンソース化（流出行為のコードは公開版に残っているとの指摘もある）。
- 英語圏では「7月13日以前にGrok Buildをシークレット含みリポジトリで実行した場合、その認証情報は送信済みとして扱え（ローテーション推奨）」という実務的な注意喚起が出ている。

**出典（確認日: 2026-08-06）**
- https://www.theregister.com/ai-and-ml/2026/07/14/musk-promises-purge-after-grok-build-caught-sending-entire-repos-to-the-cloud/5271123
- https://devops.com/xai-open-sources-grok-build-coding-agent-after-cloud-upload-exposes-ssh-keys-repos/
- https://www.techtimes.com/articles/320420/20260714/grok-build-shipped-entire-codebases-xai-cloud-privacy-toggle-did-nothing.htm
- https://cryptobriefing.com/xai-grok-build-cli-private-code-leak/

**確度**: 事件の骨子（無断アップロード・トグル無効・7/13停止・7/15公開）は確認済み（検索ベース・複数の独立報道が一致）。「27,800倍」「5.1GB/73チャンク」等の細部数値はCereblabの元解析に依拠しており、元レポートへの直接アクセスは未確認。

### 3-3. HalluSquatting攻撃（AIコーディングエージェントを狙う新型サプライチェーン攻撃）

**わかったこと**
- ハルシネーション（AIが実在しないリポジトリ/パッケージ/スキル名をもっともらしく生成する）とプロンプトインジェクションを連鎖させる攻撃。攻撃者はモデルが生成しがちな偽名を事前調査し、GitHubやプラグインストアに先回り登録して悪性の指示を仕込む。
- 研究報告では「話題の新しいリポジトリをクローンして」と指示した場合、リポジトリ名のハルシネーション発生率85%、スキルでは100%。Cursor、Cursor CLI、Gemini CLI、Windsurf、GitHub Copilot、Cline等が影響対象として報告され、ボットネット構築へのスケールも実証されたとされる。
- 2025年に報告された「Slopsquatting」（トレンドマイクロが日本語解説済み）の発展形という位置づけ。

**出典（確認日: 2026-08-06）**
- https://thehackernews.com/2026/07/new-hallusquatting-attack-could-trick.html
- https://www.securityweek.com/hallusquatting-turns-ai-hallucinations-into-botnet-delivery-mechanism/
- https://devops.com/hallusquatting-compromises-ai-coding-agents-to-install-malware-create-botnets/

**確度**: 確認済み（検索ベース・複数の独立したセキュリティ媒体が一致）。85%/100%の数値は元研究の自己報告。

### 3-4. その他の話題

**わかったこと**
- MicrosoftのClaude Code / GitHub Copilot CLIの2026年初頭の全社ロールアウトを対象にした研究が話題（元論文の特定は末尾「追加調査（レビュー対応）」の追-2参照）。
- AIエージェントのセキュリティインシデントを時系列でまとめたリポジトリ（webpro255/awesome-ai-agent-attacks、2024〜2026年の事件を出典付きで収集）が存在。
- 7月のHNの話題クラスタは「AI生成ディスインフォメーション」「サイバーセキュリティとOSSの疲弊」「堅実なソフトウェア選択への回帰」の3つ、という分析（単一情報源）。

**出典（確認日: 2026-08-06）**
- https://github.com/BlackJack-Cao/agents-radar/issues/46
- https://github.com/webpro255/awesome-ai-agent-attacks
- https://blog.mean.ceo/hacker-news-trends-july-2026/

**確度**: 単一情報源（いずれも1系統のみ。使用時は要再確認）。

---

## 調査事項4: 日本語カバレッジの状況（ギャップ分析）

日本語検索で確認した結果（確認日: 2026-08-06）。「カバー済み」でも多くは速報・使い方ガイドであり、深掘り解説の有無は別問題である点に注意。

| トピック | 日本語カバー状況 | 根拠 |
|---|---|---|
| Grok Build（使い方・料金） | **十分カバー済み**（WEEL、issoh、ai-revolution等の解説記事多数） | https://weel.co.jp/media/tech/grok-build/ ほか |
| Grok Build流出事件 | **速報レベルでカバー**（ライブドアニュース 7/13、個人系メディア）。ただし全タイムライン整理＋「ソース公開≠OSS」というガバナンス論点（bex.co 8/4）を扱う日本語記事は**見つからず** | https://news.livedoor.com/topics/detail/31812294/ / https://aifriends.jp/grok-build-repo-upload-credential-leak/ |
| Strix | **実践記事あり**（note・企業テックブログの「動かしてみた」系が複数） | https://note.com/hokosaki_inc/n/n6605beb260e9 ほか |
| Bumblebee | **カバー済み**（ZDNET Japan、Qiita、innovatopia） | https://qiita.com/kahibella/items/2921426db9ffd24f418f ほか |
| HalluSquatting | **カバー済み**（CyberCrew、Codebook、個人ブログ。前身のSlopsquattingはトレンドマイクロが解説） | https://codebook.machinarecord.com/threatreport/silobreaker-cyber-alert/46541 ほか |
| DeepSeek V4-Flash | **十分カバー済み**（WEEL、AI総合研究所、note等多数） | https://weel.co.jp/media/tech/deepseek-v4-flash ほか |
| Claude Cowork | **十分カバー済み**（解説記事多数） | https://cloudpack.jp/column/generative-ai/claude-cowork-guide.html ほか |
| 「2x, not 10x」と生産性議論・HNの評価軸の変化 | **見つからず**。日本語検索では該当エッセイへの言及・翻訳・解説を確認できなかった（「10倍」を謳う日本語記事は多数あるが、逆方向の実証的議論は見当たらない） | 検索クエリ「LLMコーディング 生産性 10倍 2倍 obryant」「"2x, not 10x" はてなブックマーク」で言及ゼロ |
| OmniRoute | **見つからず**（Qiita/Zenn記事を確認できず。リポジトリ内の公式日本語READMEのみ存在） | 検索クエリ「OmniRoute AIゲートウェイ Qiita Zenn」 |
| エージェントPR研究「単一エージェントは全タスクで優位でない」 | 未調査（時間の制約により日本語検索を実施していない） | — |

**所感:** モデルリリース系（GPT-5.6、Sonnet 5、DeepSeek等）は日本語圏の追随が速く、創刊号の差別化にはならない。ギャップが明確なのは (1)「2x, not 10x」に代表される英語圏の生産性議論の転換、(2) Grok Build事件の構造的な深掘り（事件・OSS化・ガバナンスの一連）、(3) OmniRoute等のコスト最適化インフラ、の3領域。

---

## 未確認事項・制約のまとめ

- WebFetch不可のため、公式発表ページ（openai.com、anthropic.com、blog.google、deepseek.ai、perplexity.ai）の本文を直接確認できていない。執筆時には必ず原文を再確認すること。
- 全リポジトリのスター数現在値は未確認（各ソースの時点値のみ）。執筆時にGitHubで直接確認すること。
- HNのポイント数・コメント数は未確認。
- 「Cereblab」による解析レポートの原文は未確認（報道経由の引用のみ）。
- Gemini 3.6 Flash群、Qwen 7月リリース、FLUX 3、Grok 4.5、CodeMenderプレビューは単一情報源のままで裏取り不十分。

---

## 創刊号アングル3案（詳細は最終報告と同一）

1. **「10倍ではなく2倍」— 英語圏のAIコーディング生産性議論は静かに転換した**（推し度: 高）
2. **Grok Build事件の全タイムライン — 「ソース公開」はなぜ「オープンソース」ではないのか**（推し度: 高）
3. **AIエージェント時代のサプライチェーン攻防 — HalluSquattingとBumblebee/Strixを一本の線でつなぐ**（推し度: 中）

各案の「なぜ今か」「一次情報の量」は最終報告参照。

---

## 追加調査（レビュー対応）

レビュー指摘「出典のない研究引用」への対応として、2件の研究の元ソース特定を実施（調査日: 2026-08-06。WebFetch不可のため検索ベース）。

### 追-1. 「エージェント生成PRの分析で、どの単一エージェントも全タスクカテゴリで優位ではなかった」の元研究

**わかったこと**
- 該当するとみられる元研究を特定した: arXiv論文 **「How Do AI Coding Agents Contribute to Software Development? An Empirical Study of Agentic Pull Requests」（arXiv:2607.21832）**。
- 検索結果の要旨によれば、AIDevデータセットの**7,156件のPull Request**を分析し、5つのコーディングエージェント（OpenAI Codex、GitHub Copilot、Devin、Cursor、Claude Code）を比較。主な知見:
  - PRのタスク種別が受理率の支配的要因（ドキュメント系82.1% vs 新機能66.1%で16ポイント差。この差は多くのタスクでエージェント間の差より大きい）
  - **「No single agent performs best across all task types」**（Claude Codeはドキュメント92.3%と機能72.6%で首位、Cursorは修正タスク80.4%で首位）
  - OpenAI Codexは全9タスクカテゴリで一貫して高受理率（59.6%〜88.6%）だが全カテゴリ首位ではない
  - Devinのみ受理率が一貫して上昇傾向（32週で週+0.77%）
- 関連する近縁研究（同じAIDevデータセット系）も存在するため、引用時は取り違えに注意: 「On the Use of Agentic Coding: An Empirical Study of Pull Requests on GitHub」（arXiv:2509.14745、2025年9月）、「Understanding the Rejection of Fixes Generated by Agentic Pull Requests」（arXiv:2606.13468）、「Test Coverage Analysis of Agentic Pull Requests」（arXiv:2607.18057）。

**出典（確認日: 2026-08-06）**
- https://arxiv.org/html/2607.21832v1 （本命の元研究とみられる）
- https://arxiv.org/abs/2509.14745 （近縁の先行研究）

**確度**: 単一情報源。論文の存在とタイトル・数値は検索結果の要旨で確認したが、**論文本文を直接開いて「no single agent」の記述と数値の対応を確認できていない**（WebFetch不可のため）。また、research.md本文の当該記述の出所（developersdigest.techの記事）がこの論文を指しているかの直接の紐付けも未確認。執筆時はarXiv本文を必ず直接確認すること。

### 追-2. 「MicrosoftのClaude Code / GitHub Copilot CLI全社ロールアウト研究」の元論文

**わかったこと**
- 元論文を特定した: **「Adoption and Impact of Command-Line AI Coding Agents: A Study of Microsoft's Early 2026 Rollout of Claude Code and GitHub Copilot CLI」（arXiv:2607.01418）**。筆頭著者はEmerson Murphy-Hillほか2名。
- 検索結果の要旨によれば: Microsoftの2026年1月〜3月の段階的ロールアウトを対象に、数万人規模のエンジニアの直接テレメトリ（アンケートではない）で導入と成果を測定した初のフィールドスタディ。主な知見:
  - 初回利用は主にソーシャルネットワーク（同僚経由）で伝播
  - 継続利用は属性よりコーディング活動量と相関
  - 導入者はマージPRが**約24%増**（4か月の観測期間で持続。ただし著者ら自身が「マージPR＝価値そのものではない」と留保）
- 注意: 当初の調査メモでは「査読付き（peer-reviewed）」と記載したが、これは二次ソース（agents-radarのHNダイジェスト）の表現。**arXiv掲載はプレプリントであり、査読の有無は確認できなかった**。記事で使う場合は「査読付き」とは書かず「arXiv論文」「フィールドスタディ」等と表現すること。

**出典（確認日: 2026-08-06）**
- https://arxiv.org/abs/2607.01418 （元論文）
- https://arxiv.org/html/2607.01418v1
- https://www.researchgate.net/publication/408403915_Adoption_and_Impact_of_Command-Line_AI_Coding_Agents_A_Study_of_Microsoft's_Early_2026_Rollout_of_Claude_Code_and_GitHub_Copilot_CLI （同論文のResearchGate掲載）

**確度**: 論文の存在・タイトル・著者は確認済み（検索ベース・arXivとResearchGateの複数掲載が一致）。「24%増」等の数値は検索結果の要旨経由であり、本文の直接確認は未実施。「査読付き」は未確認（プレプリントの可能性が高い）。
