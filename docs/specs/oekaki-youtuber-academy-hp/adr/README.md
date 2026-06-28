# Architecture Decision Records (ADR)

| ID | タイトル | 状態 | 日付 |
|---|---|---|---|
| [ADR-0001](./0001-static-html-on-vercel.md) | 静的 HTML + Vercel Hobby プランで構築する | Accepted | 2026-06-28 |
| [ADR-0002](./0002-route-root-to-course-html.md) | `/` を `course.html` にルーティングする（rewrites） | Accepted | 2026-06-28 |
| [ADR-0003](./0003-mailto-instead-of-form.md) | 問い合わせは mailto: で代替し、HTML フォームを置かない | Accepted | 2026-06-28 |
| [ADR-0004](./0004-no-blog-section.md) | ブログセクションを当面実装しない | Accepted | 2026-06-28 |
| [ADR-0005](./0005-link-instead-of-embed-youtube.md) | YouTube 動画は埋め込まずリンクのみとする | Accepted | 2026-06-28 |
| [ADR-0006](./0006-public-repo-for-vercel-hobby.md) | GitHub リポジトリを Public とする | Accepted | 2026-06-28 |

---

## ADR の運用方針

- 重要な技術判断・スコープ判断は ADR として記録する
- 一度 Accept した ADR は変更せず、上書きする場合は新規 ADR を切って "Supersedes ADR-XXXX" を明記する
- ステータス: `Proposed` → `Accepted` → (必要時) `Deprecated` / `Superseded`
- 各 ADR は **問題 / 検討案 / 決定 / 結果** の 4 セクションを必ず持つ
