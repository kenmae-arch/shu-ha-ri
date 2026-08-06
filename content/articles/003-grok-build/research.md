# 記事003「Grok Build事件の全タイムライン ——『ソース公開』はなぜ『オープンソース』ではないのか」調査

- 調査日: 2026-08-06（担当: リサーチ）
- 前提: 既存調査 `content/articles/002-inaugural/research.md` の調査事項1-2および3-2を出発点とし、その「未確認」部分を埋めることを目的とした。

## 調査環境に関する重要な注記（002からの変更点）

002の調査時点では「WebFetchが全ドメインで403」としていたが、**今回は一部ドメインでWebFetch（ページ本文の直接取得）が成功した**。具体的には:

- **成功**: `github.com`, `gist.github.com`, `raw.githubusercontent.com`, `code.claude.com`（Claude Code公式ドキュメント）
- **403で失敗**: `theregister.com`, `thehackernews.com`, `docs.github.com`, `docs.cursor.com`, `cursor.com`, `opensource.org`, `simonwillison.net`, `news.ycombinator.com`, `bex.co`, `copilot.github.trust.page`

本文書では、**本文を直接取得できたものを「一次確認済み」**、検索結果の引用のみのものを「検索ベース」と明示的に区別する。この区別が今回の調査の確度の中心である。

確度の表記:
- **一次確認済み**: 一次情報のページ本文をWebFetchで直接取得して確認した
- **確認済み（検索ベース）**: 複数の独立した情報源が一致するが、本文の直接確認はしていない
- **単一情報源**: 1系統の情報源のみ
- **未確認 / 確認できなかった**: 裏が取れなかった

---

# 調査事項1: 事件タイムラインの精緻化

## 1-A. 確定できた日付（複数ソース一致）

| 日付 | 出来事 | 確度 |
|---|---|---|
| 2026-07-12 | 研究者 cereblab がGitHub Gistで「wire-level analysis（回線レベル解析）」を公開。対象は Grok Build CLI **v0.2.93** | 確認済み（検索ベース・複数ソース一致） |
| 2026-07-13 | 同じ研究者が同じ0.2.93クライアントを再テスト。6回実行して `/v1/storage` へのアップロードがゼロになったことを確認。サーバーが `trace_upload_enabled: false` と新フラグ `disable_codebase_upload: true` を返すようになった。同日、Muskがアップロード済みデータの削除を表明 | 確認済み（検索ベース・複数ソース一致） |
| 2026-07-14 | 解析がHacker Newsのフロントページに到達。Gist本文に「Update（2026-07-14）」としてサーバー側停止と `/privacy` オプトアウト追加が追記される。The Register が報道 | 一次確認済み（Gist本文の更新注記）＋検索ベース（HNフロントページ入り） |
| 2026-07-15 | xAI（SpaceXAI）が Grok Build のハーネス全体を Apache 2.0 で `github.com/xai-org/grok-build` に公開。全ユーザーの利用上限をリセット | 確認済み（検索ベース・複数ソース一致）＋リポジトリの存在は一次確認済み |
| 2026-07-16 | The Register が「同じ週にオープンソース化」として続報 | 確認済み（検索ベース） |
| 2026-08-04 | bex.co が「Apache 2.0 read-only = open source washing」批評を公開 | 単一情報源（本文は403で未取得。タイトルとURLのみ確認） |

## 1-B. 相互に矛盾する報道（重要 —— 記事で使うなら明記が必要）

調査の結果、**発端の日付について3通りの記述が存在した**。

1. **7月12日説（最有力・多数派）**: TechTimes（7/14付記事）、The Next Web、Developers Digest、digitalapplied、Medium記事などが一致して「July 12に公開」とする。
2. **7月10日説**: qwe.edu.pl の記事が「published wire-level proof on July 10, 2026」と記述。他に同調するソースは見つからなかった。
3. **7月15日説（誤りの可能性が高い）**: ai-navigate-news.com が「On July 15, 2026, the developer community discovered...」と記述。これは公開日ではなくオープンソース化の日と混同している可能性がある。

**確度**: 7月12日説が最有力（複数の独立した媒体が一致）。ただし**Gist自体の公開日タイムスタンプを直接確認することはできなかった**（GistページのWebFetchは本文を取得できたが、公開日時のメタデータは取得内容に含まれていなかった）。記事では「7月12日（複数報道による。原Gistの公開日時は直接確認できず）」と書くのが安全。

もう一つの構造的な注意点として、**この事件には2つの別々の発火点がある**ことが分かった:

- Hacker Newsスレッド **id=48892468**「Grok CLI uploaded the whole home directory to GCS」= 一般ユーザーがホームディレクトリごとアップロードされたと報告したもの
- cereblab の体系的な回線レベル解析（Gist）

報道はこの2つをしばしば混ぜて書いている。「SSH鍵・パスワードマネージャDB・写真まで送られた」という記述は前者（個別ユーザーの報告）に由来し、「27,800倍」「5.1GB/73チャンク」は後者（cereblabの制御実験）に由来する。**記事で両者を同じ根拠のように書くと不正確になる。**

- 出典（確認日: 2026-08-06）
  - https://www.techtimes.com/articles/320420/20260714/grok-build-shipped-entire-codebases-xai-cloud-privacy-toggle-did-nothing.htm
  - https://thenextweb.com/news/grok-build-uploaded-entire-git-repositories-secrets
  - https://www.developersdigest.tech/blog/grok-cli-wire-level-analysis
  - https://www.digitalapplied.com/blog/grok-build-privacy-scandal-what-actually-fixed-it
  - https://news.ycombinator.com/item?id=48892468 （ホームディレクトリ報告のHNスレッド。本文は403で未確認）
  - https://news.ycombinator.com/item?id=48926590 （オープンソース化のHNスレッド。本文は403で未確認）
  - https://www.theregister.com/ai-and-ml/2026/07/16/spacex-open-sources-grok-build-after-data-retention-furore/5272333
  - https://www.qwe.edu.pl/tutorial/grok-cli-uploaded-home-directory-gcs/ （7月10日説）
  - https://ai-navigate-news.com/en/updates/2026-07-15/grok-build-cli-silently-uploaded-whole-repos-the-local-first-betrayal （7月15日説）

**HNのポイント数・コメント数は確認できなかった**（news.ycombinator.com が403）。

---

# 調査事項2: Cereblabの元解析レポート —— 特定できた

002で「未確認（報道経由）」としていた元レポートを**特定し、本文をWebFetchで直接取得できた**。これが今回の調査の最大の成果。

## 2-A. 一次情報の所在

1. **解析レポート本体（GitHub Gist）**
   https://gist.github.com/cereblab/dc9a40bc26120f4540e4e09b75ffb547
   タイトル: "What xAI Grok Build CLI actually sends to xAI - a wire-level analysis (grok 0.2.93)"

2. **再現用リポジトリ**
   https://github.com/cereblab/grok-build-exfil-repro
   「Reproduce it yourself」を掲げ、mitmproxyハーネス＋カナリアリポジトリ＋証拠ファイルを同梱

3. （参考・未検証）第三者による監視ハーネス: https://github.com/weklund/grok-network-monitor
   検索結果で存在を確認したのみで、内容・信頼性は**確認できなかった**。

## 2-B. 「27,800倍」「5.1GB/73チャンク」の出所 —— 特定できた

Gist本文（一次確認済み）に以下の記述がある:

- テスト対象は**12GBのテストリポジトリ**（内容は「一度も読ませていないランダムファイル」）
- 2つのチャネルが同時に動く:
  - モデルターン用チャネル `POST /v1/responses` → 約 **192KB**（エージェントが実際に読んだ分）
  - ストレージ用チャネル `POST /v1/storage` → **5.1GB を 73チャンク（1チャンク約75MB）**
- この差が **約27,800倍**。つまり「27,800倍」は総送信量の比ではなく、**同一セッション内の2チャネル間の転送量比**である。
- 送信先は Google Cloud Storage バケット `grok-code-session-traces`。パス形式は `gs://grok-code-session-traces/repo_changes_dedup/v2/`。バケット名はバイナリ内の文字列、staging用 `metadata.json`、`storage.googleapis.com` への実際のmultipart PUTの3つから同定したとされる。
- 解析対象バイナリのSHA-256: `2a97ba675bd992aa9b981e2e83776460d94f469b510c0b8efe28b50d236d767c`（grok 0.2.93）
- 手法: macOS上で **mitmproxy** によりHTTPSを傍受。テストファイルにカナリア文字列を埋め込み。証拠として wire log、復号済みリクエストボディ、アップロードから復元したgit bundleを保存。

**カナリア実験（再現リポジトリのREADMEで一次確認済み）**:
`CANARY-XR47P2-NEVERREAD-UNIQUE` というマーカー付きファイルを置き、エージェントには「ファイルを開くな」と指示した上で実行。後からアップロードされたgit bundleを回収したところ、そのファイルがコミット履歴ごと逐語的に復元できた。

再現リポジトリのREADMEにある最も重要な一文（一次確認済み）:

> "A permission deny stops the agent READING a file into the chat; it does NOT stop the whole-repo git-bundle upload."
> （パーミッションの拒否はエージェントがファイルをチャットに読み込むことを止めるが、リポジトリ全体のgit bundleアップロードは止めない）

そしてバンドルからファイルを除外できる唯一の手段は `.gitignore` である、とされている。

## 2-C. 「プライバシートグルが効かなかった」の中身（一次確認済み）

Gistは**送信（transmission）と学習利用（training usage）を明確に区別**している。

- 「モデル改善に使わせない」トグルが制御していたのは `/v1/responses` チャネル（と保持ポリシー）であり、`/v1/storage` チャネルは**クラウド発行のサーバーサイドフラグにのみ反応**していた。
- 実験: 「Improve the model」をオフにして再実行しても、設定エンドポイントは `trace_upload_enabled: true` を返し、リポジトリ全体のアップロードは継続した。
- Gistの整理: 送信されていること＝**実証済み**。学習に使われたか＝**ポリシーの問題であり実証されていない**。

**この区別は記事の中核になる。cereblab本人が「学習に使われた」とは主張していない点は正確に扱うこと。**

## 2-D. Gistに記載された緩和策（一次確認済み）

- `~/.grok/config.toml` に `[harness] disable_codebase_upload = true`
- 環境変数 `GROK_TELEMETRY_TRACE_UPLOAD=false`
- サーバー側フラグは現在 `in_remote_trace_upload_enabled: false` を返す

**確度**: 2-A〜2-Dはすべて**一次確認済み**（Gist本文および再現リポジトリREADMEを直接取得）。ただしcereblabの実験そのものを当方で再現検証はしていない。また、cereblabという研究者の身元・所属は**確認できなかった**（ハンドル名のみ）。

---

# 調査事項3: xAI / Musk側の公式声明

## 3-A. セキュリティアドバイザリは出ていない（複数ソース＋一次確認）

- 複数の報道が一致して「**正式なセキュリティアドバイザリは発行されていない**」「バージョンノート・チェンジログでの告知もない」と記述している。
- xAIは**X（旧Twitter）上で対応を説明**し、アドバイザリやチェンジログという形は取らなかった、とされる。
- **一次確認**: 公開されたリポジトリの `SECURITY.md` をWebFetchで直接取得したところ、記載は「脆弱性はHackerOneプログラム（https://hackerone.com/x ）経由で報告せよ」「セキュリティ報告のために公開GitHub issueを立てるな」のみで、**2026年7月のアップロード問題に関する記述は一切ない**。開示の期限や約束に関する記載もない。

出典:
- https://github.com/xai-org/grok-build/blob/main/SECURITY.md （一次確認済み、確認日 2026-08-06）
- https://www.techtimes.com/articles/320420/20260714/grok-build-shipped-entire-codebases-xai-cloud-privacy-toggle-did-nothing.htm
- https://chatforest.com/builders-log/xai-grok-build-cli-repository-upload-google-cloud-no-statement-builder-alert/

**確度**: 「リポジトリのSECURITY.mdに事件への言及がない」は一次確認済み。「一切のアドバイザリが存在しない」ことの証明は**性質上できない**（不存在の証明）。記事では「確認できた範囲でアドバイザリは見つからず、リポジトリのSECURITY.mdにも言及がない」という書き方に留めるべき。

## 3-B. 報じられているxAI公式アカウントの説明

複数の報道が引用する内容（**原文のポストは確認できなかった**）:

- 公式アカウント（SpaceXAI）は「**zero data retention（ZDR）のエンタープライズ顧客のコードやトレースデータは保存されたことがない**」と説明したとされる。
- 一般ユーザーには**CLI内で `/privacy` コマンドを実行**してデータ保持を無効化し、同期済みデータを削除するよう案内したとされる。
- ただし cereblab は「`/privacy` は回線テストの結果、**データ保持（retention）の設定であって、何が送信されるかをブロックするものではなかった**」と記述している（Gistで一次確認済み）。

**この食い違い（xAI側は「/privacyで対処できる」、研究者側は「/privacyは送信を止めない」）が事件の核心の一つ。**

## 3-C. Muskの発言

- 報道による引用: アップロード済みの全ユーザーデータは "**completely and utterly deleted**"（完全かつ徹底的に削除される）、何も残さない、と述べたとされる。
- **原文のポストは確認できなかった**（X上の投稿を直接取得できていない）。引用はThe Register、TechTimes等の報道経由。
- 複数の報道が共通して指摘する未解決点: **影響を受けたユーザー数、削除のタイムライン、開発者が自分のデータ削除を検証する手段のいずれも公表されていない**。削除証明書・監査ログ・第三者による証明（attestation）も公開されていない。

出典（確認日: 2026-08-06、いずれも本文は403のため検索結果の引用）:
- https://www.theregister.com/ai-and-ml/2026/07/14/musk-promises-purge-after-grok-build-caught-sending-entire-repos-to-the-cloud/5271123
- https://the-decoder.com/xai-open-sources-grok-build-on-github-after-massive-data-breach/
- https://www.techtimes.com/articles/320727/20260716/x-vows-full-codebase-open-source-after-grok-build-exfiltrated-dev-repos.htm

**確度**: 発言の存在は確認済み（検索ベース・複数ソース一致）。**原文（Xのポスト）は確認できなかった**ため、記事では「〜と報じられている」と書くこと。

## 3-D. アップロードコードは公開版に残っている

- Simon Willison（英語圏の著名な技術ライター）が公開当日に指摘したとされる内容: 「Google Cloudへ全部アップロードしていたコードの名残がまだあるが、現在は無効化されているようだ」。具体的に `xai-grok-shell/src/upload/gcs.rs` にGCSバケットへのアップロードコードがあり、`upload/trace.rs` の `upload_session_state()` は `session_state_upload_unavailable` というハードコードされたエラーを返す、と記述されている。
- **一次確認**: `https://github.com/xai-org/grok-build/blob/main/crates/codegen/xai-grok-shell/src/upload/gcs.rs` をWebFetchで直接取得し、**このファイルが実在することを確認した**。内容は、`AuthManager` と連携するGCSアップロードのアダプタで、
  - バケットは環境変数 `GROK_SESSION_TRACES_BUCKET_DEFAULT` で設定、`GROK_TELEMETRY_GCS_BUCKET` で上書き可能
  - コード中に「None disables trace uploads until a bucket is configured（バケットが設定されるまでトレースアップロードは無効）」というコメントがある
  - 関数として `upload_bytes()`, `upload_file()`, `upload_stream()`, `upload_to_auth_diagnostics()`（`auth-diagnostics/{version}/{user_id}/{ts}.jsonl` へ診断ログを送る）が存在
  - 明示的に無効化・スタブ化されている箇所は、取得できた範囲では見当たらなかった
- **`upload/trace.rs` の直接確認はできなかった**（推測したパスが404。正確なパスを特定できなかった）。したがって `upload_session_state()` がハードコードエラーを返すという点は**Willisonの指摘の検索結果引用のみに依拠しており、当方では未確認**。
- 別の指摘（単一情報源・byteiota）: 「サーバーサイドフラグで止めただけで新しいバイナリはリリースされておらず、アップロードコードは**バージョン0.2.99にもコンパイルされたまま**残っている。xAIはソフトウェア更新なしに、任意のユーザー・任意のセッションでアップロードを再度有効化する技術的能力を保持している」。この0.2.99という具体的バージョンについては**裏取りができなかった（単一情報源）**。

出典:
- https://github.com/xai-org/grok-build/blob/main/crates/codegen/xai-grok-shell/src/upload/gcs.rs （一次確認済み）
- https://simonwillison.net/2026/Jul/15/grok-build/ （本文403。検索結果の引用のみ）
- https://byteiota.com/grok-build-is-open-source-but-the-upload-code-remains/ （単一情報源）
- https://www.techtimes.com/articles/320671/20260716/grok-build-open-sourced-after-covert-upload-code-exfiltrate-repos-stays.htm

---

# 調査事項4: 「オープンソースウォッシング」批判の中身

## 4-A. 一次確認できた事実（これが最も強い材料）

`https://github.com/xai-org/grok-build/blob/main/CONTRIBUTING.md` をWebFetchで直接取得。以下は**原文の引用**:

> "This repository does not accept external pull requests or unsolicited patches."
> （このリポジトリは外部からのプルリクエストや依頼していないパッチを受け付けません）

> "No contributor license agreement is offered because external contributions are not accepted."
> （外部からの貢献を受け付けないため、コントリビュータライセンス契約は提供されません）

さらにCONTRIBUTING.mdは、xAIが本ソフトウェアを**社内で開発しており、公開リポジトリは Apache License 2.0 の下での「ソースの透明性（source transparency）」とローカルビルドのためだけに提供されている**と明記している。

**この「source transparency」という自己申告の語は、記事タイトルの『ソース公開』を裏づける一次的な根拠として使える。**

## 4-B. リポジトリの現況（一次確認済み、確認日 2026-08-06）

`https://github.com/xai-org/grok-build` をWebFetchで取得:

- 説明: "Terminal-based AI coding agent"、フルスクリーンTUI、コードベース理解、ファイル編集、シェル実行、Web検索
- ライセンス: Apache License 2.0（サードパーティコードは各元ライセンス）
- 言語: Rust
- **スター数: 24.3k**（002調査時点の「公開直後で約4.5k」から大幅増）
- **main ブランチのコミット数: 21件のみ**
- 「SpaceXAIのモノレポから定期的に同期される」旨の記載。バージョン追跡は `SOURCE_REV` ファイル
- READMEには**テレメトリ/プライバシーに関する明示的な記述は見当たらなかった**。`disable_codebase_upload` についてもREADMEには言及なし
- トップレベル構成: `.cargo`, `bin`, `crates`, `prod/mc/cli-chat-proxy-types`, `third_party`, `CONTRIBUTING.md`, `LICENSE`, `README.md`, `SECURITY.md`, `SOURCE_REV`, `THIRD-PARTY-NOTICES` ほか

**「21コミット」と「モノレポから定期同期」は、開発の実態が外部から見えない（＝コミット単位の履歴が公開されていない）ことを示す具体的な証拠として使える。これは一次確認済み。**

## 4-C. 報じられている批判の論点

bex.co（2026-08-04、本文は403で未取得。以下は検索結果の引用）を中心に、以下の論点が挙げられている:

1. **ライセンスと開発プロセスのギャップ**: Apache 2.0は使用・改変・再配布を許諾するが、CONTRIBUTING.mdは外部PRを拒否する。結果として「readable and forkable, but unshapeable（読めるしフォークできるが、形を変えることはできない）」。
2. 「地球上の誰もが読み、監査し、フォークし、セルフホストできる。しかしxAIの外部の誰も1行たりともマージできない」という対比。
3. **open-washing（オープンウォッシング）という一般的な枠組み**: 「openness（開放性）の信用を借りながら、それを正当化している部分は手放さない」パターン。批判者は「open」を複数の軸に分解して個別に検査せよと主張する。参照枠組みとして **TechPolicy.Press の4基準** と **OSI の Open Source AI Definition をめぐる議論** が挙げられている。

出典:
- https://bex.co/blog/2026/08/04/xai-grok-build-apache-2-read-only-open-source-washing （本文未確認）
- https://www.opensourceforu.com/2026/07/xai-open-sources-grok-build-after-repository-upload-controversy/

**確度**: 単一情報源（bex.co）。ただし批判の前提となる事実（Apache 2.0＋外部PR不受理）は4-Aで一次確認済み。

## 4-D. OSIのオープンソース定義との関係 —— 重要な但し書き

**調査の結果、この論点で最も注意すべき点が判明した。**

- 検索で確認した限り、**OSI（Open Source Initiative）の Open Source Definition（OSD）は「ライセンスの条項」についての定義であり、プロジェクトが外部からの貢献を受け入れることを要求していない**。OSIのFAQには、パッチ受け入れ時のコントリビュータ契約などは「プロジェクトレベルの慣行であってOSIの要件ではない」旨の記述がある。また「オープンソースライセンスでソフトウェアを公開することは、OSIに連絡したり何らかのプロセスに登録したりすることを含まない。OSI承認ライセンスを付けて公開するだけでよい」とも述べられている。
- したがって、**「外部PRを受け付けないからOSDに違反する／オープンソースではない」という主張は、OSDの文言上は成立しない**。Apache 2.0はOSI承認ライセンスであり、Grok Buildの公開はOSDの文字面には反していない。
- 批判の本質は「OSDへの違反」ではなく「**OSDが定義しているのはライセンスだけであり、一般の開発者が『オープンソース』という語に期待するもの（コミュニティによる共同開発、パッチの受け入れ、開発履歴の可視性）はOSDの外側にある**」という点にある。これは定義違反の話ではなく、**語の期待値と定義のズレ**の話である。

- **OSI自身がGrok Buildについて公式に見解を表明したという事実は、検索した範囲では確認できなかった。** OSIの名前は批評記事の中で「枠組みの参照先」として言及されているだけで、OSIによる直接のコメントは見つからなかった。
- 同様に、**この件について発言した個人の識者としてはSimon Willisonの技術的な観察が確認できたのみ**で、法務・ライセンス分野の専門家による評価は確認できなかった。

- 参考: 別件だが、xAIの「Grok 2.5」公開（Grok 2 Community License Agreement による）については「撤回可能」「用途限定」「"Powered by xAI" 表示義務」「競合モデルの学習への出力利用禁止」を理由に「いかなるオープンソース定義にも適合しない」「open source ではなく open weights」という評が出ている。**Grok Build（Apache 2.0）とGrok 2.5（独自ライセンス）は法的性質がまったく異なるので、記事で混同しないこと。**

出典（確認日: 2026-08-06。opensource.org は403のため本文未確認）:
- https://opensource.org/faq
- https://opensource.org/osd
- https://drewdevault.com/blog/Open-source-is-defined-by-the-OSD/
- https://technewsday.com/xai-issues-open-source-grok-2-5-but-how-open-is-it-really/ （Grok 2.5の別件）

**確度**: 「OSDはライセンスの定義であり貢献受け入れを要求しない」は確認済み（検索ベース・複数ソースの記述が一致し、OSDの一般的理解とも整合）。ただし**OSD本文（10項目）を直接取得できていない**ため、記事でOSDの条項を引用する場合は必ず原文を再確認すること。「OSIがGrok Buildに言及した」は**確認できなかった**。

---

# 調査事項5: 他社コーディングエージェントのテレメトリ方針の比較

**この節は特に慎重に扱う。公式ドキュメント本文を直接取得できたのは Claude Code のみである。他は検索ベースにとどまる。**

## 5-A. Claude Code（Anthropic）— 一次確認済み

`https://code.claude.com/docs/en/data-usage` の本文をWebFetchで直接取得（確認日: 2026-08-06）。

**送信されるもの:**
- モデルとのやり取り: 「すべてのユーザープロンプトとモデル出力」がTLS 1.2+で暗号化されて送信される（これは機能上必須）
- **メトリクス**: レイテンシ・信頼性・利用パターン。Anthropicおよびサードパーティのログ基盤へTLSで送信。原文引用: 「**Metrics never include your code, prompts, or file paths.**（メトリクスにコード、プロンプト、ファイルパスが含まれることは決してない）」。`DISABLE_TELEMETRY=1` でオプトアウト
- **エラーレポート**: Claude Code自身のエラーメッセージとスタックトレース。サードパーティのエラートラッキングサービスへ送信。既知のシークレット・ファイルパス・メールアドレス等のパターンは**送信前にローカルで秘匿化**される。`DISABLE_ERROR_REPORTING=1` でオプトアウト。有効になるのはPro/Max sign-in かつ v2.1.198以降 かつ Claude APIへ直接接続 かつ ZDR/HIPAA契約がない場合に限られる
- **`/feedback`（`/bug`, `/share` も同経路）**: 「コードを含む会話履歴のコピー」がAnthropicへ送られる。送信前に含める範囲（現在のセッションのみ＝デフォルト／同プロジェクトの過去24時間・7日間）を選択する。保存先はGoogle Cloud Storage。**保持期間は5年**。オプションで公開リポジトリにGitHub issueが作られる。`DISABLE_FEEDBACK_COMMAND=1` でオプトアウト
- **セッション品質アンケート**: 評価そのものは会話内容を一切収集しない。その後の追加質問で「Yes」を選んだ場合のみ、会話トランスクリプト・サブエージェントのトランスクリプト・生のセッションログがアップロードされる。既知のAPIキー/トークンパターンは秘匿化されるが、**ソースコードやファイル内容はそのままアップロードされる**。共有トランスクリプトの保持は最大6か月。「Yesを選ばない限り何もアップロードされない」と明記
- **WebFetchドメイン安全性チェック**: URL取得前にホスト名のみを `api.anthropic.com` に送信（フルURL・パス・ページ内容は送らない）。これは `CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC` の影響を受けず、`skipWebFetchPreflight: true` で個別に無効化する

**保持期間:**
- 個人（Free/Pro/Max）: モデル改善に同意 → 5年 / 不同意 → 30日
- 商用（Team/Enterprise/API）: 標準30日。Zero Data Retention は Claude for Enterprise の適格アカウントに対して個別に有効化（標準のEnterpriseプランには含まれない）
- **ローカルキャッシュ**: `~/.claude/projects/` に**平文で**セッショントランスクリプトを既定30日保存。`cleanupPeriodDays` で調整

**学習利用:**
- 個人（Free/Pro/Max）: 設定がオンのとき学習に使われる（Claude Codeの利用分を含む）
- 商用（Team/Enterprise/API/サードパーティプラットフォーム/Claude Gov）: 顧客が明示的に提供を選択（例: Development Partner Program）しない限り、Claude Codeに送られたコードやプロンプトを生成モデルの学習に使わない

**一括オプトアウト:** `CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC`（ただしWebFetchチェックと公式マーケットプレイス自動インストールは対象外。後者は `CLAUDE_CODE_DISABLE_OFFICIAL_MARKETPLACE_AUTOINSTALL`）

**プロバイダ別の既定値:** Amazon Bedrock / Google Cloud's Agent Platform / Microsoft Foundry / Claude Platform on AWS 経由では、エラーレポート・テレメトリ・バグレポートは**既定でオフ**（セッション品質アンケートとWebFetchチェックは例外）

**確度**: **一次確認済み**（公式ドキュメント本文を直接取得）。

## 5-B. Gemini CLI（Google）— 部分的に一次確認、ただし食い違いあり

**(1) OpenTelemetry テレメトリ（一次確認済み）**
`https://github.com/google-gemini/gemini-cli/blob/main/docs/cli/telemetry.md` をWebFetchで直接取得。

- これは**ユーザーが自分のコレクタに送るための仕組み**であり、Googleへの送信とは別物。既定は `enabled: false`（**既定で無効**）
- 収集対象: ログ（セッション設定、プロンプト送信、ツール実行、ファイル操作、APIリクエスト/レスポンス、エラー、モデルルーティング判断、チャット圧縮、ストリーミング、エージェントのライフサイクル、IDE接続）、メトリクス（ツール呼び出し数・レイテンシ、APIリクエスト数・レイテンシ、トークン使用量、ファイル操作数、エージェント実行時間、起動性能、メモリ使用量）、トレース
- **重要**: 「Include prompts in telemetry logs」= `logPrompts` の**既定値は `true`**。つまりテレメトリを有効化した場合、**プロンプトは既定でログに含まれる**。除外するには `.gemini/settings.json` で `logPrompts: false`
- 無効化: 既定のまま、または `enabled: false` / 環境変数 `GEMINI_TELEMETRY_ENABLED`

**(2) Usage Statistics（Googleへの送信）— 未確認の部分あり**

検索結果は、公式ドキュメントページ（`google-gemini.github.io/gemini-cli/docs/get-started/configuration.html` および `.../tos-privacy.html`）に以下の記述があるとする:

- 認証方式によって扱いが異なる。「Google アカウント＋個人向け Gemini Code Assist」では、この設定が有効なとき Google は匿名テレメトリ（実行したコマンド、性能メトリクス等）**および プロンプトと回答（コードを含む）**をモデル改善のために収集できる
- 一方で「プロンプトやレスポンスの内容はログしない」「CLIが読み書きしたファイルの内容はログしない」という記述も引用されている
- 有料 Gemini Code Assist アカウントおよび Vertex AI GenAI API のAPIキー利用では、入力は機密として扱われ学習に使われない
- オプトアウト: `settings.json` に `{ "privacy": { "usageStatisticsEnabled": false } }`。「Usage Statistics 設定はGemini CLIにおける任意のデータ収集すべての単一のコントロールである」とされる

**ただし重大な注意**: `raw.githubusercontent.com` から `docs/resources/tos-privacy.md` を直接取得したところ、そこには「Gemini CLI の Usage Statistics は Google のプライバシーポリシーに従って扱われる」「指示に従ってオプトアウトできる」という記述と、認証方式ごとの参照先プライバシーポリシーへのリンクがあるだけで、**上記の「we do not log」形式の具体的な記述や認証方式ごとの詳細表は含まれていなかった**。

つまり、検索結果が引用している「プロンプト内容はログしない」等の文言は、**現行のリポジトリ内ドキュメントでは当方が確認できなかった**。ドキュメントが改訂された、あるいは別ページにある可能性がある。

**確度**:
- OpenTelemetry の仕様（既定オフ、`logPrompts` 既定true）は**一次確認済み**
- Googleへの Usage Statistics の内容（認証方式ごとの学習利用の有無、「ログしない」の文言）は**未確認**。検索結果の引用のみで、当方が取得できたドキュメント本文とは一致しなかった。**記事に書く場合は必ず原文を再確認すること。断定して書かないこと。**

出典（確認日: 2026-08-06）:
- https://github.com/google-gemini/gemini-cli/blob/main/docs/cli/telemetry.md （一次確認済み）
- https://raw.githubusercontent.com/google-gemini/gemini-cli/main/docs/resources/tos-privacy.md （一次確認済み。ただし詳細記述なし）
- https://google-gemini.github.io/gemini-cli/docs/tos-privacy.html （403で未確認）
- https://google-gemini.github.io/gemini-cli/docs/get-started/configuration.html （未取得）

## 5-C. GitHub Copilot CLI — 検索ベースのみ（本文未確認）

`docs.github.com` と `copilot.github.trust.page` はいずれも403で**本文を直接確認できなかった**。以下は検索結果の引用。

- GitHub Copilot Trust Center のFAQに、アクセス方法ごとの保持ポリシーがあるとされる:
  - **コードエディタ（IDE）内**: プロンプトとサジェスチョンは**保持しない**
  - **github.com上のチャット、モバイル、CLI**: プロンプトとサジェスチョンを**28日間保持**する。理由は「これらの機能はスレッド履歴を使うことで有効性が成り立つため」
  - ユーザーエンゲージメントデータ: 2年間
- Business/Enterprise では、プロンプト・セッションログ・出力を公開AIモデルの学習には使わない、とされる
- 個人向けティアでは既定でプロンプトとテレメトリがモデル改善のために保持されることがあり、アカウント設定で手動オプトアウトできる、とされる
- 旧 `gh-copilot` 拡張のテレメトリは**オプトイン**で、同意の質問文は「Allow GitHub to collect optional usage data to help us improve? **This data does not include your queries**」（クエリは含まない）。送信ペイロードには `device_id`, `invocation_id`, アーキテクチャ、OS、コマンド情報等のディメンションが含まれるとされる。ログモードで「送信されるはずのJSONペイロードを実際には送らずにstderrに出力して確認できる」機能があるとされる
- 2026-07-08のGitHub Changelogで「Enterprise-managed OpenTelemetry export for VS Code and CLI」が発表され、組織がOTelの送信先コレクタを指定でき、**プロンプト・レスポンス・ツール内容をキャプチャするか、開発者がそれを変更できるかを組織が制御できる**とされる

**確度**: **すべて検索ベース・単一〜少数系統。公式ドキュメント本文は確認できなかった。** 特に「28日保持」「個人ティアの既定オプトイン」は二次ソース（ブログ・フォーラム）由来のものが混じっており、**記事で数値を書く場合は必ず原文を確認すること**。また `gh-copilot` 拡張と現行の `copilot` CLI は別物であり、混同のおそれがある。

出典（確認日: 2026-08-06、いずれも本文未確認）:
- https://copilot.github.trust.page/faq
- https://docs.github.com/en/github-cli/github-cli/github-cli-telemetry
- https://github.blog/changelog/2026-07-08-enterprise-managed-opentelemetry-export-for-vs-code-and-cli/
- https://docs.github.com/en/copilot/how-tos/copilot-sdk/observability/opentelemetry

## 5-D. Cursor — 検索ベースのみ（本文未確認）

`cursor.com/data-use` と `docs.cursor.com` はいずれも403で**本文を直接確認できなかった**。以下は検索結果の引用。

- 公式ページ `cursor.com/data-use` の記述として引用されている文言: 「Privacy Mode をオフにした場合、当社はコードベースデータ、プロンプト、エディタ操作、コードスニペット、その他のコードデータおよび操作を、AI機能の改善とモデルの学習のために使用・保存することがある」
- **コードベースのインデックス**: インデックスを選択すると、コードベースは小さなチャンクに分割されてCursorのサーバーにアップロードされ、埋め込み（embeddings）が計算される。**平文のコードはリクエストのライフサイクルを超えて存在しない**とされる。埋め込みとメタデータ（ハッシュ、ファイル名）はデータベースに保存されることがある。ファイル名とパスは難読化・暗号化され、長期間非アクティブなインデックスは削除される
- **Privacy Mode有効時**: コードデータがCursorのサーバーやサブプロセッサに平文で保存されることはないとされる。埋め込みとメタデータは機能実現のために保存されるが、コード内容そのものはリクエスト中のメモリ上でのみ可視で、その後は保持されない
- **通常の推論**: カーソル周辺のスニペットやチャットに貼り付けた内容は、Cursorのサーバーおよび背後のAIプロバイダに送信される。ファイル種別のメタデータとセッション識別子も伴う

**確度**: **すべて検索ベース。公式ページ本文は確認できなかった。** 特に「埋め込みは保存されるがコードは保存されない」という主張は、Cursor自身の主張であり第三者検証ではない点に注意。

出典（確認日: 2026-08-06、いずれも本文未確認）:
- https://cursor.com/data-use
- https://cursor.com/help/security-and-privacy/privacy

## 5-E. 比較表（確度の注記つき）

| | Claude Code | Gemini CLI | GitHub Copilot CLI | Cursor | Grok Build（7/13以前） |
|---|---|---|---|---|---|
| 本調査での確認レベル | **一次確認済み** | 部分的に一次確認 | 検索ベースのみ | 検索ベースのみ | 一次確認済み（第三者解析） |
| 「読んだ以外のコード」の自動送信 | 記載なし。メトリクスは「コード・プロンプト・ファイルパスを含まない」と明記 | 未確認 | 未確認 | インデックス機能を**有効にした場合**にチャンク送信（ユーザーの選択） | **あり（リポジトリ全体をgit bundleで送信）** |
| テレメトリの既定 | メトリクスは既定オン（Claude API直結時）。`DISABLE_TELEMETRY=1` | OTelは**既定オフ**。有効化時 `logPrompts` は既定true | 未確認（旧拡張はオプトイン） | 未確認 | サーバー側フラグで制御。ユーザーからは不可視だった |
| プロンプト/コードの保持 | 商用30日 / 個人は同意により5年 or 30日 | 未確認 | CLIは28日とされる（未確認） | Privacy Mode時は平文保持なしとされる（未確認） | 削除を約束したが**検証手段の提供は確認できず** |
| 一括オプトアウト | `CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC` | `usageStatisticsEnabled: false`（未確認） | 未確認 | Privacy Mode | `disable_codebase_upload`（事件後に追加） |

**この表は「A社は安全でB社は危険」を示すものではない。確認できた粒度が各社で大きく違うこと自体が事実である。** 特に Claude Code だけが詳細に確認できたのは、当該ドキュメントがWebFetchで取得できたという調査環境上の理由であり、他社のドキュメントが不十分だという証拠ではない。

---

# 調査事項6: 実務的な対処法

## 6-A. Grok Build を7月13日以前に使った場合（英語圏で推奨されている手順）

複数の情報源が共通して挙げる内容:

1. **対象範囲の判定**: 2026-07-13より前に Grok Build CLI を、**トラックされたファイルまたはgit履歴のいずれかに**シークレットを含むリポジトリで実行したなら、その認証情報は「送信済み」として扱う。対象例: APIキー、DBパスワード、クラウドトークン、SSH鍵、Webhookシークレット
2. **git履歴が対象であることの理解**: 「ローカルのファイルを消した」「作業ツリーから削除した」では不十分。**アップロードされたのはgit bundle（履歴込み）であり、過去にコミットして後から削除したシークレットも含まれる**。これは cereblab の再現リポジトリで一次確認済みの挙動と整合する
3. **ローテーション**: 本番コードで実行した場合、`.env` の値、APIキー、DB認証情報、該当リポジトリのSSH鍵をローテーションする
4. **履歴の棚卸し**: git履歴に埋もれたシークレットを洗い出す（gitleaks等のシークレットスキャナの利用が一般的だが、**特定のツール名を推奨する記述は今回の調査では確認できなかった**）
5. **組織としての対応**: DPA（データ処理契約）、SOC2の表明、PIIポリシーの再評価
6. **恒久対策**: 「シークレットをgit履歴に一切入れない」衛生管理。ベンダー側のスイッチが壊れても機能する唯一の防御である、という整理

出典（確認日: 2026-08-06、いずれも本文は未確認・検索結果の引用）:
- https://devops.com/xai-open-sources-grok-build-coding-agent-after-cloud-upload-exposes-ssh-keys-repos/
- https://reptile.haus/journal/grok-build-xai-privacy-data-exfiltration-ai-coding-agents-2026/
- https://www.digitalapplied.com/blog/grok-build-privacy-scandal-what-actually-fixed-it

**確度**: 「7/13以前の実行分は送信済みとして扱う」「git履歴も対象」は確認済み（検索ベース・複数ソース一致、かつ2-Bの一次確認と整合）。個別の手順は各媒体の推奨であり、公式な指示ではない。

## 6-B. Grok Build 側の設定（Gistで一次確認済み）

- `~/.grok/config.toml` に `[harness] disable_codebase_upload = true`
- 環境変数 `GROK_TELEMETRY_TRACE_UPLOAD=false`
- CLI内の `/privacy` コマンド（**ただしこれは保持設定であって送信を止めるものではない、と研究者は記述している**）

## 6-C. コーディングエージェント全般の「何が出ていくか」点検方法

事件から一般化できる、**手法として確認できた**もの:

1. **回線レベルの傍受（最も確実）**: mitmproxy等のHTTPS傍受プロキシを立て、エージェントを通して実際の送信内容を見る。cereblab の再現リポジトリはこの手順を4つのスクリプトに分解して公開しており（プロキシ設定 → カナリアリポジトリ作成 → 「ファイルを開くな」プロンプトでエージェント実行 → キャプチャの検証）、**任意のエージェントに応用できるテンプレートになっている**（一次確認済み）
2. **カナリアファイル法**: 「エージェントが絶対に読まないはずのファイル」に一意なマーカー文字列を置き、送信内容にそれが現れるかを見る。**「エージェントが読んだもの」と「クライアントが送ったもの」を切り分けられる**のがこの手法の要点（一次確認済み）
3. **パーミッション拒否は送信の否認ではない**という前提: cereblab の一文「A permission deny stops the agent READING a file into the chat; it does NOT stop the whole-repo git-bundle upload.」（一次確認済み）
4. **ドキュメント上の確認**: 各ツールの公式データ利用ドキュメントで「テレメトリにコード/プロンプト/ファイルパスが含まれるか」を明示的に探す。Claude Code は「Metrics never include your code, prompts, or file paths」と明記している（一次確認済み）
5. **ローカル残留物の確認**: Claude Code は `~/.claude/projects/` に**平文で**セッショントランスクリプトを既定30日保存する（一次確認済み）。送信だけでなくローカルの平文残留も点検対象になる
6. **`.gitignore` の位置づけ**: Grok Build のケースでは、バンドルからファイルを除外できる唯一の仕組みが `.gitignore` だった（一次確認済み）。ただしこれは Grok Build 固有の挙動であり、他ツールに一般化できるとは**確認できていない**

**未確認**: 送信内容を点検するための標準化されたチェックリストや業界ガイドラインが存在するかは、今回の調査では**確認できなかった**。

---

# まとめ: 記事執筆時に必ず守るべき注意点

1. **日付**: 発端は7月12日が最有力だが、7月10日説・7月15日説の報道も存在する。Gistの公開日時タイムスタンプは直接確認できていない。
2. **2つの発火点を混同しない**: 「SSH鍵・パスワードDB・写真」は個別ユーザーのHN報告由来、「27,800倍・5.1GB/73チャンク」はcereblabの制御実験由来。
3. **「27,800倍」の意味**: 総量の比ではなく、同一セッション内の `/v1/responses`（192KB）と `/v1/storage`（5.1GB）の比。12GBのテストリポジトリでの数値。
4. **送信と学習利用を分ける**: 研究者自身が「送信は実証済み、学習利用は未実証」と明記している。「学習に使われた」と書いてはいけない。
5. **Muskの発言は原文未確認**: 「〜と報じられている」と書く。
6. **OSDとの関係**: 「外部PRを受け付けないからオープンソースではない」はOSDの文言上は成立しない。批判の本質は「定義（ライセンス）と期待値（共同開発）のズレ」である。OSIがこの件で公式見解を出したことは確認できなかった。
7. **他社比較**: Claude Code だけが一次確認済みで、他社は検索ベース。**確認粒度の非対称性を記事に明示しないと不公平な比較になる。**
8. **Grok Build（Apache 2.0）と Grok 2.5（独自ライセンス）を混同しない。**

## 確認できなかったこと（記事に書かない、または「確認できなかった」と明記する）

- cereblab の身元・所属（ハンドル名のみ）
- Gist の正確な公開日時タイムスタンプ
- Musk および xAI 公式アカウントの投稿原文
- xAI が何らかの形でセキュリティアドバイザリを出していないことの決定的証明（不存在の証明は不可能。SECURITY.mdに記載がないことのみ確認済み）
- `upload/trace.rs` の `upload_session_state()` がハードコードエラーを返すこと（正確なファイルパスを特定できず。Willisonの指摘の引用のみ）
- アップロードコードがバージョン 0.2.99 に残っていること（単一情報源）
- HN両スレッドのポイント数・コメント数
- OSD本文（10項目）の原文
- OSI によるこの件への公式コメント
- GitHub Copilot CLI および Cursor の公式ドキュメント本文
- Gemini CLI の Usage Statistics の認証方式別の詳細（取得できたリポジトリ内ドキュメントには記載がなかった）
- 影響を受けたユーザー数、削除の完了状況
- bex.co 記事の本文（タイトルと検索結果の引用のみ）
- Simon Willison の記事本文（403）

---

## 所感（事実ではなく、担当者の見立て）

**所感1:** この事件の最も再利用可能な資産は、事件そのものより **cereblab の再現ハーネス** だと考える。「カナリアファイル＋mitmproxy＋『開くな』プロンプト」という3点セットは、どのコーディングエージェントにも適用できる汎用的な検証手順になっており、読者が自分の環境で実行できる。事件のまとめより、この手順の解説のほうが日本語圏での希少価値が高い。

**所感2:** 「外部PRを受け付けないApache 2.0」への批判は、事実関係としては CONTRIBUTING.md の原文（一次確認済み）で完全に裏が取れる一方、「だからオープンソースではない」という主張はOSDの定義上は成立しない。この**ねじれ**こそが記事の面白いところであり、どちらか一方の側に立って断定すると記事が弱くなると考える。

**所感3:** 他社比較の節は、書き方を間違えると「Claude Codeは安全」という記事に読めてしまうリスクがある。実際には調査環境の都合でClaude Codeのドキュメントだけが取得できただけであり、その非対称性を先に開示してから比較表を出す構成が安全だと考える。
