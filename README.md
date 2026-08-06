# 守破離AI（Shu-Ha-Ri）

AI活用を「守破離」で身につけるための情報発信・コンテンツ事業を運営する、AI社員つきのバーチャルカンパニーです。

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

最初の一歩はすでに踏み出してあります。第1号記事が `content/articles/001-shu-ha-ri-roadmap/` に、初週のSNS投稿案が `content/sns/` にあります。**社長の最初の仕事は、この2つをレビューして公開を承認（または差し戻し）することです。**

## 事業の現在地

- フェーズ: **1（無料発信で読者をつくる）** — 詳細は `company/business-plan.md`
- 直近のやること: `business/backlog.md`
- 数字: `business/kpi.md`
