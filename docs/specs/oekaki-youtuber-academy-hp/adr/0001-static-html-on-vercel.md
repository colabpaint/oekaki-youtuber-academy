# ADR-0001: 静的 HTML + Vercel Hobby プランで構築する

| 項目 | 値 |
|---|---|
| 状態 | Accepted |
| 日付 | 2026-06-28 |
| 決定者 | colabpaint（オーナー）／ Claude Code（補助） |
| 関連要件 | C-1, C-2, NFR-M1, NFR-M2 |

---

## 1. 文脈・問題

お絵かきYouTuberアカデミーのブランドホームページを公開するにあたり、ホスティング・構成技術を決める必要があった。前提条件:

- 運用者は非エンジニア 1 名（colabpaint）
- 編集は AI 補助前提
- 予算ゼロ希望
- 更新頻度は低〜中（コピー修正・写真差し替えが中心）
- ページ数は当面 1 ページ
- 高速・安定・SEO そこそこ

## 2. 検討した選択肢

| 案 | 概要 | 利点 | 欠点 |
|---|---|---|---|
| **A. 静的 HTML + Vercel Hobby** | 純粋な HTML/CSS を Vercel で配信 | ゼロ円・No-Build・1〜2 分で反映・Edge 配信 | カスタムドメインに無料の制約あり（ドメインは自前購入） |
| B. WordPress（real045 と同じ） | サーバー・テーマ・プラグインで構成 | エディタが GUI で直感的 | 月額サーバ代・脆弱性対応・バックアップ運用 |
| C. Next.js (SSG) on Vercel | フレームワーク使用 | 拡張性高、TypeScript、コンポーネント設計 | 過剰スペック、ビルド時間、AI 編集の障壁 |
| D. GitHub Pages | GitHub で完結 | ゼロ円 | デプロイ・ルーティング機能が貧弱、Edge も国内最適化されていない |
| E. Cloudflare Pages | CF で配信 | 無料・高速 | 国内アクセスでは Vercel と僅差、UI が技術者向け |

## 3. 決定

**案 A: 静的 HTML + Vercel Hobby プラン** を採用する。

### 理由

1. **運用工数最小**: ビルド不要。push 即デプロイ。
2. **AI 編集が容易**: 1 ファイル `course.html` と `course.css` で完結し、自然言語指示でも迷いにくい。
3. **ゼロコスト**: Vercel Hobby は個人サイト規模で無料。
4. **十分な性能**: Edge CDN で国内 < 50ms 配信。
5. **将来の拡張性**: 必要になれば Next.js 等に移行可能（HTML 資産は流用可能）。

## 4. 結果

- 公開 URL: `https://oekaki-youtuber-academy.vercel.app/`
- リポジトリ: `colabpaint/oekaki-youtuber-academy` (Public)
- `vercel.json` で `/` → `/course.html` を rewrite（→ ADR-0002）
- HSTS / Brotli / HTTP/2 が自動で有効
- 自動デプロイ: GitHub `main` への push → 1〜2 分で反映

## 5. トレードオフ

- カスタムドメインを使う場合、別途ドメイン取得（年 1,000〜数千円）が必要
- Hobby プランは商用利用に制約あり（個人サイトと判定されるラインに注意）
- 月間 100 GB 帯域・1000 GB-Hour 等の Soft Cap あり（個人サイトでは通常超えない）
- Hobby は自動課金されない設計（超過時は配信停止 → 問題化前に把握できる）
