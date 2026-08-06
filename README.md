# 守破離AI（Shu-Ha-Ri）

海外テック・ディープリサーチメディアを運営する、AI社員つきのバーチャルカンパニーです。英語圏の一次情報（論文・GitHub・テック動向）を「守=正確に読む / 破=検証する / 離=日本の実務者に応用する」の3部構成で深掘りします。読者基盤の先に、ニッチ課題を解くマイクロSaaSを育てます。

このリポジトリそのものが会社です。事業計画も、就業規則も、AI社員も、プロダクト（記事・SNS投稿）も、すべてここにあります。

## 会社の構造

| 場所 | 中身 |
|---|---|
| `company/` | 会社の憲法。ミッション、事業計画、組織図、運営ルール |
| `CLAUDE.md` | 全AI社員共通の就業規則（Claude Codeが常に読むルール） |
| `.claude/agents/` | AI社員本体（Claude Codeのサブエージェント定義） |
| `.claude/skills/` | 業務フロー（`/write-article` などのスラッシュコマンド） |
| `docs/` | 文体ルール、業務手順書 |
| `templates/` | 企画書・投稿・意思決定記録のテンプレート |
| `content/` | プロダクト（記事とSNS投稿）。会社の生産物はここに貯まる |
| `business/` | バックログ、KPI、意思決定記録。会社の経営状態はここを見る |

## AI社員名簿

役職ではなく「業務単位」で採用しています（→ 理由は `company/org.md`）。

| 呼び方 | 担当業務 | 定義ファイル |
|---|---|---|
| 企画 | 記事テーマ・企画書の作成 | `.claude/agents/planner.md` |
| リサーチ | 調査とファクトチェック | `.claude/agents/researcher.md` |
| ライター | 記事の構成・初稿・改稿 | `.claude/agents/writer.md` |
| レビュー | 品質チェック（公開前の門番） | `.claude/agents/reviewer.md` |
| SNS | X向け投稿案の作成 | `.claude/agents/sns-marketer.md` |
| 秘書 | バックログ・KPI・週次整理 | `.claude/agents/secretary.md` |

## 社長（あなた）の仕事

この会社で人間がやることは3つだけです。

1. **承認する** — 企画の採用、記事の公開、SNS投稿の送信は必ず社長が決める（AIは公開ボタンを押せない）
2. **判断基準を言語化する** — 「なんか違う」と思ったら、なぜ違うかを `docs/style-guide.md` に書き足す。これが会社の資産になる
3. **週次レビューをまわす** — 週1回 `/weekly-review` を実行し、KPIとバックログを更新する

## 今日から動かす手順

```
# このリポジトリでClaude Codeを起動して:

/write-article        # 記事を1本、企画から公開準備まで作る
/weekly-review        # 週次のふりかえりと来週の計画
```

## 事業の現在地

- フェーズ: **1（週1本のディープリサーチ記事で読者をつくる）** — 詳細は `company/business-plan.md`
- 創刊号: `content/articles/002-inaugural/`（社長の承認待ち）
- 直近のやること: `business/backlog.md`
- 数字: `business/kpi.md`
- 主な意思決定: `business/decisions/`（0002 = リサーチメディアへのピボット）
