# 設計書 (Design Document) — お絵かきYouTuberアカデミー HP

| 項目 | 値 |
|---|---|
| Spec Slug | `oekaki-youtuber-academy-hp` |
| 設計モデル | C4 Model (System Context / Container / Component / Code) |
| 関連要件 | `requirements.md` |
| 関連ADR | `adr/0001` 〜 `adr/0006` |
| 最終更新 | 2026-06-29 |

---

## 1. アーキテクチャ概観

### 1.1 アーキテクチャスタイル

**JAMstack（Static-First, Edge-Delivered）**

| 選択肢 | 採用？ | 理由 |
|---|---|---|
| 静的サイト（純HTML/CSS） | ✅ **採用** | 運用工数最小・ビルド不要・高速・低コスト |
| SSG（Next.js / Astro / Hugo） | ❌ | 更新頻度低・1ページ構成のため過剰 |
| WordPress（real045 と同じ） | ❌ | 保守・セキュリティ・コストの観点で不利 |
| SPA（React 等） | ❌ | SEO・初期表示・複雑性で不利 |

> ADR-0001 を参照。

### 1.2 設計原則

| 原則 | 説明 |
|---|---|
| **Single-Source Truth** | コピー・画像・装飾は `course.html` と `course.css` の2ファイルに集約。中間言語なし |
| **Declarative-Over-Imperative** | JavaScript は最小限（または 0）。挙動は HTML/CSS で完結 |
| **AI-Editable** | 自然言語指示 1 行で編集可能な構造を維持（クラス名は意味語、コメントで区画化） |
| **Progressive Enhancement** | JS / 高度な機能は飾りであり、HTML だけで機能完備 |
| **No-Build** | webpack/Vite 等のビルドステップを持たない。push 即デプロイ |

---

## 2. C4: System Context（システムコンテキスト）

```
                                    ┌──────────────────────┐
                                    │  Search / SNS / Ads  │
                                    │  (流入チャネル)        │
                                    └──────────┬───────────┘
                                               │  HTTPS
                                               ▼
   ┌──────────────┐                ┌──────────────────────────┐                ┌──────────────────────┐
   │   訪問者      │ ─── HTTPS ───▶ │  お絵かきYouTuberアカデミー  │ ── 外部 ─▶ │   YouTube              │
   │ (P1〜P4)     │                │  ブランドHP                 │   遷移     │  (受講生対談動画 x6)    │
   └──────────────┘                │  (Vercel Hobby Plan)      │            └──────────────────────┘
        │                          └──────────┬───────────────┘
        │                                     │
        │                                     ├─ mailto: ──▶ メールクライアント (運営アドレス)
        │                                     │
        │                                     └─ LINE link ──▶ LINE 公式アカウント (将来)
        │
        └──────────────────────────────────────┘
              個人情報の流れ: なし
              Cookie/Storage: なし (Vercel Analytics 未導入)
```

### 関係者・外部システム

| 要素 | 種別 | 役割 |
|---|---|---|
| 訪問者 (P1〜P4) | ヒト | 要件定義 §3 参照 |
| 検索エンジン・SNS・広告 | 流入チャネル | 訪問者をサイトに送る |
| ブランドHP本体 | システム | 本プロダクトのスコープ |
| YouTube | 外部システム | 受講生対談動画を保持 |
| メールクライアント | 外部システム | mailto: 経由で問い合わせ受信 |
| LINE Official Account | 外部システム | 将来配信先 |

---

## 3. C4: Container（コンテナ）

本プロダクトは「コンテナ」と呼べるレベルの構成要素を以下のように持つ。

```
┌─────────────────────────────────────────────────────────────────────────────┐
│  GitHub Repository (colabpaint/oekaki-youtuber-academy)                      │
│  ─ main branch                                                              │
│  ─ Source of Truth                                                          │
└──────────────┬──────────────────────────────────────────────────────────────┘
               │  git push (webhook)
               ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│  Vercel (Build + Edge CDN)                                                  │
│  ┌───────────────────────────────────────────────────────────────────────┐ │
│  │  Build Phase: 静的ファイルコピーのみ（No build step）                    │ │
│  │  Deploy Phase: Edge Network へ配信                                     │ │
│  │  Routing: vercel.json による rewrites（/ → /course.html）              │ │
│  └───────────────────────────────────────────────────────────────────────┘ │
│  ┌───────────────────────────────────────────────────────────────────────┐ │
│  │  Edge CDN (Vercel global edge)                                        │ │
│  │  ─ HTTPS/HTTP2 配信                                                    │ │
│  │  ─ HSTS / Cache-Control                                                │ │
│  └───────────────────────────────────────────────────────────────────────┘ │
└──────────────┬──────────────────────────────────────────────────────────────┘
               │  HTTPS GET /
               ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│  Browser (訪問者)                                                            │
│  ─ HTML parse → CSS apply → 画像読み込み → Google Fonts 読み込み              │
└─────────────────────────────────────────────────────────────────────────────┘
```

### コンテナ一覧

| Container | 技術 | 役割 |
|---|---|---|
| GitHub Repository | Git | バージョン管理・PR・履歴 |
| Vercel Build | Node.js (本件は実質ファイルコピー) | デプロイパイプライン |
| Vercel Edge CDN | Edge Network | エンドユーザー配信 |
| Browser | あらゆるモダンブラウザ | レンダリング・操作 |
| Google Fonts | フォント配信 CDN | カスタムフォント配信 |
| YouTube img.youtube.com | サムネ画像 CDN | Voice カードのサムネ |

---

## 4. C4: Component（コンポーネント）

ここでは「HTML ドキュメント内のセクション構成」を Component として扱う。

```
course.html
├── <head>
│   ├── meta (charset, viewport, theme-color, description)
│   ├── <title>
│   ├── preconnect: fonts.googleapis.com, fonts.gstatic.com
│   ├── Google Fonts: Inter, Yusei Magic, Klee One, Noto Sans JP
│   └── <link rel="stylesheet" href="course.css">
└── <body>
    ├── Header           (.site-header)
    │   ├── Logo         (.site-logo: 2-line)
    │   ├── Nav          (.site-nav)
    │   └── CTA          (.header-cta → #line)
    ├── Hero             (.hero)
    │   ├── Stripes x4   (.hero-stripe-*)
    │   ├── Hero Text    (.hero-text)
    │   │   ├── Eyebrow  (.hero-eyebrow)
    │   │   ├── Catch    (.hero-catch)
    │   │   ├── Lead     (.hero-lead)
    │   │   └── Actions  (.hero-actions: 2 buttons)
    │   └── Hero Visual  (.hero-visual / portrait-transparent.png)
    ├── Philosophy       (.philosophy)
    │   ├── Quote        (.philosophy-quote)
    │   └── Body x2      (.philosophy-body)
    ├── Voices           (.voices #voices)
    │   ├── Section Head (.section-head)
    │   └── Grid         (.voices-grid x6 .voice-card → YouTube)
    ├── Achievement      (.achievement)
    │   ├── Section Head
    │   ├── Shield Row   (photo + text)
    │   └── Stats Grid   (3 stats)
    ├── FAQ              (.faq #about)
    │   ├── Section Head
    │   └── FAQ List     (.faq-item x5+)
    ├── Profile          (.profile #profile)
    │   ├── Section Head
    │   ├── Profile Image (.profile-image)
    │   └── Profile Text
    │       ├── Name + Role
    │       ├── Facts List (6 items)
    │       └── Story (5 paragraphs)
    ├── Curriculum       (.curriculum)
    │   ├── Section Head
    │   └── Curriculum List (6 numbered items)
    ├── Contact          (.contact #contact)
    │   ├── Section Head
    │   ├── Contact Text
    │   ├── mailto: button
    │   └── Email plaintext (copy fallback)
    ├── LINE CTA         (.line-cta #line)
    │   ├── Title
    │   ├── Body
    │   ├── LINE Button (Big)
    │   └── Opt-out Note
    └── Footer           (.site-footer)
        ├── Brand
        ├── Nav
        └── Copyright
```

---

## 5. C4: Code（実装の重要パターン）

### 5.1 CSS 構成

`course.css` は以下の方針で構造化されている:

```
:root { /* デザイントークン: 色・余白・タイポ */ }

/* リセット & ベース */
* { ... }
body { ... }

/* 共通: .container, .section-head, .marker, .btn-outline, .btn-line */

/* セクション順: header, hero, philosophy, voices, achievement, faq, profile, curriculum, contact, line-cta, footer */
.site-header { ... }
.hero { ... }
  .hero-stripe-* { /* 黄色斜めストライプ */ }
  .hero-img-cutout { /* 背景透過写真 */ }
...

/* AB 装飾パターン: 奇数セクション=斜めストライプ / 偶数セクション=コーナーブロック */
.philosophy::before, .achievement::before, .profile::before { /* A: stripe */ }
.voices::before, .faq::before, .curriculum::before { /* B: corner block */ }

/* レスポンシブ */
@media (max-width: 900px) { /* tablet / mobile */ }
@media (max-width: 480px) { /* small mobile (iPhone SE) */ }
```

### 5.2 命名規則（クラス名）

| パターン | 例 | 意図 |
|---|---|---|
| `.section-name` | `.hero`, `.voices` | セクションのルート |
| `.section-name__part` (使わず) | — | BEM は採用せず、代わりに `-` 連結 |
| `.section-name-part` | `.hero-text`, `.voice-card` | セクション内部の要素 |
| `.utility-class` | `.container`, `.marker`, `.btn-outline` | 横断的に再利用される共通スタイル |

### 5.3 装飾の AB 交互パターン

| セクション位置 | 装飾 | クラス |
|---|---|---|
| 1 (Hero) | 斜めストライプ x4 | `.hero-stripe-*` |
| 2 (Philosophy) | A: 黄色斜めストライプ（左） | `::before` |
| 3 (Voices) | B: コーナーブロック（左上） | `::before` |
| 4 (Achievement) | A: 斜めストライプ | `::before` |
| 5 (FAQ) | B: コーナーブロック | `::before` |
| 6 (Profile) | A: 斜めストライプ | `::before` |
| 7 (Curriculum) | B: コーナーブロック | `::before` |
| 8 (Contact) | なし | — |
| 9 (LINE CTA) | なし（強いカラーブロック） | — |

---

## 6. データフロー

### 6.1 表示時（GET /）

```
1. ブラウザ → DNS → vercel.app → Vercel Edge
2. Vercel: vercel.json の rewrites を評価 → "/" を "/course.html" に内部書き換え
3. Vercel Edge: course.html をキャッシュから返す（HSTS / cache-control 付与）
4. ブラウザ: HTML パース
5. ブラウザ: course.css 取得 → 適用
6. ブラウザ: Google Fonts 取得（preconnect で先行接続）
7. ブラウザ: 画像（portrait-transparent.png, silver-shield-real.jpg, voice サムネ x6）取得
   - Voice サムネは img.youtube.com から取得（loading="lazy"）
8. ブラウザ: 描画完了
```

### 6.2 編集 → 公開のフロー

```
1. ユーザー: "ここの文字を X に変えて" と指示
2. Claude Code: course.html / course.css を Edit
3. Claude Code: git add → commit → push
4. Vercel: GitHub webhook を受信 → 新しいデプロイ開始
5. Vercel: 30秒〜2分で完了
6. Edge CDN: 新コンテンツに切り替わる
7. ブラウザ: ハードリロードで反映確認
```

---

## 7. インフラ構成

### 7.1 Vercel 設定（`vercel.json`）

```json
{
  "rewrites": [
    { "source": "/", "destination": "/course.html" }
  ]
}
```

| 設定 | 意図 |
|---|---|
| `rewrites` | `/` リクエストを `course.html` の中身で返す（URL は `/` のまま） |
| `cleanUrls` を**外した**理由 | `course.html` を `/course` にリダイレクトしてしまい、内部リンクと衝突するため |
| プロジェクト設定 | Framework Preset: Other (静的) / Build Command: なし / Output Directory: ./ |

### 7.2 GitHub

| 項目 | 値 |
|---|---|
| リポジトリ | `colabpaint/oekaki-youtuber-academy` (Public) |
| デフォルトブランチ | `main` |
| ブランチ保護 | なし（オーナー1人運用） |
| GitHub Actions | なし（Vercel が直接 webhook） |
| `.gitignore` | macOS metadata (`.DS_Store`, `._*`), editor files, `*.bak` |

### 7.3 アーカイブリポジトリ（新規）

| 項目 | 値 |
|---|---|
| リポジトリ | `colabpaint/20260629oekaki-youtuber-academyHP` |
| 用途 | 2026-06-29 時点のスナップショット（HP + 設計書 + リサーチ資料を含む） |
| Vercel 連携 | **なし**（公開は主リポジトリで継続） |

---

## 8. 依存ライブラリ・外部リソース

| 依存先 | 用途 | リスク | 緩和策 |
|---|---|---|---|
| Google Fonts | Inter / Yusei Magic / Klee One / Noto Sans JP | サブセット未指定で帯域増 | preconnect 済、必要なら `&text=` 指定検討 |
| `img.youtube.com` | Voice サムネ画像 (`maxresdefault.jpg`) | YouTube 動画削除でサムネ消失 | 自前画像にミラーする運用を将来検討（FUT） |
| Vercel | ホスティング・CDN・HTTPS | 障害時にサイト全停止 | 月間 99.9% 想定。重大障害時は GitHub Pages へ手動切替手順を runbook に記載 |
| GitHub | リポジトリ・トリガー | サービス障害でデプロイ不能 | 上記同様、別 git ホストへ移行手順を runbook に |

---

## 9. デザインシステム（要約）

### 9.1 カラートークン

| 役割 | 値（CSS 変数） |
|---|---|
| 背景 | `#fdfaf3`（クリーム） |
| テキスト基色 | `#222` |
| アクセント（オレンジ系） | `#f47a3a` 系統 |
| 黄色装飾 | `#ffd633` 系統（マーカー・ストライプ） |
| 罫線 | `#eee` |

> 厳密な変数定義は `course.css` の `:root` を Source-of-Truth とする。本ドキュメントは概念表記のみ。

### 9.2 タイポグラフィ

| 用途 | フォント | サイズの目安 |
|---|---|---|
| 大見出し（ヒーローキャッチ） | Yusei Magic / Noto Sans JP Black | 48px (PC) / 28px (mobile) |
| セクション見出し | Noto Sans JP 700/900 | 32px (PC) / 24px (mobile) |
| Eyebrow | Inter 900 | 24-26px |
| 本文 | Noto Sans JP 400 | 16-18px |
| 装飾的（ロゴ等） | Klee One / Yusei Magic | 36px (logo-main) |

### 9.3 スペーシング

`container` を 1080px 〜 1200px の幅で運用。セクション間の縦余白は `padding: 80-120px 0`（モバイルでは縮小）。

---

## 10. 互換性

| ターゲット | 動作確認 |
|---|---|
| Chrome / Edge | 最新版 ✅ |
| Safari (macOS / iOS) | 最新版 ✅ |
| Firefox | 最新版（軽微なフォントレンダ差は許容） |
| IE 11 | **非対応** |
| ビューポート 1440 / 1024 / 900 / 768 / 480 / 375 px | 確認済み |

---

## 11. オブザーバビリティ（最小構成）

| レイヤ | 現状 | 将来検討 |
|---|---|---|
| アクセスログ | Vercel の標準ログ（ダッシュボードから閲覧可） | Vercel Analytics の有償化、Plausible 等への置き換え |
| エラー監視 | なし（静的サイトのため） | JS が増えたら Sentry など |
| 稼働監視 | なし | UptimeRobot 等の無料外形監視 |

---

## 12. パフォーマンス設計

| 施策 | 状態 | 効果 |
|---|---|---|
| HTTP/2 配信 | ✅（Vercel デフォルト） | 並列リクエスト最適化 |
| Brotli 圧縮 | ✅（Vercel デフォルト） | 転送量削減 |
| Google Fonts preconnect | ✅ | 接続時間短縮 |
| 画像 lazy loading | ✅（Voice サムネ） | 初期 LCP 改善 |
| 画像最適化 | 一部（透過 PNG は大きめ） | 将来: WebP 化 |
| CSS / JS minify | ❌（人間が読める方を優先） | 将来: 必要なら Vercel の自動最適化 |

---

## 13. 設計判断と却下案

| トピック | 採用案 | 却下案 | 理由 |
|---|---|---|---|
| ホスティング | Vercel | Netlify / GitHub Pages / Cloudflare Pages | UX・無料枠・速度・国内安定性のバランス |
| `/` のルーティング | rewrite (vercel.json) | index.html を作って中身を course.html と同期 | DRY と保守性（同期忘れ防止） |
| ファイル名 | `course.html` のまま | `index.html` にリネーム | 既存指示・ドキュメント参照との一貫性 |
| ブログ機能 | 実装しない | Astro / Hugo で実装 | 運用負荷とのトレードオフで現時点は不要 |

詳細は `adr/` を参照。

---

## 14. 将来の拡張ポイント

| 拡張 | 設計上の準備 |
|---|---|
| カスタムドメイン | Vercel ダッシュボードから 5 分で追加可能。設計変更不要 |
| OGP / 構造化データ | `<head>` への `<meta>` / `<script type="application/ld+json">` 追加で完結 |
| ブログ | `/blog/` サブディレクトリに Astro 等を追加・別ビルド構成 |
| 多言語化 | `/en/course.html` 等のサブパスで複製。i18n フレームワーク導入は要件発火時に再検討 |
| アナリティクス | Vercel Analytics ON、または `<script>` 1 行追加（Plausible 推奨） |

---

> **次の成果物**: `tasks.md` — 本設計を実現するための作業（既に完了済み + ギャップ対応）を Kiro 形式で列挙する。
