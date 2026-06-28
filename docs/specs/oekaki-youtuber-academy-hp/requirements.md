# 要件定義書 — お絵かきYouTuberアカデミー ブランドホームページ

| 項目 | 値 |
|---|---|
| プロジェクト名 | お絵かきYouTuberアカデミー HP |
| Spec Slug | `oekaki-youtuber-academy-hp` |
| バージョン | 1.0.0 |
| ステータス | **L3 (Implementation Ready)** — 既に本番デプロイ済み |
| 公開URL | https://oekaki-youtuber-academy.vercel.app/ |
| 主リポジトリ | https://github.com/colabpaint/oekaki-youtuber-academy |
| アーカイブリポジトリ | https://github.com/colabpaint/20260629oekaki-youtuber-academyHP |
| 作成日 | 2026-06-29 |
| 最終更新 | 2026-06-29 |
| オーナー | colabpaint（運営）／ ユウ（講師） |
| 採用記法 | EARS (Easy Approach to Requirements Syntax) |

---

## 1. ビジネス概要

### 1.1 プロダクトビジョン

本プロダクトは、ポスカアーティスト「ユウ」が運営する**お絵かきYouTuberアカデミー**（ポスカアート講座）の**ブランドホームページ（コーポレートサイトに相当）** である。広告のランディングページ（LP）ではなく、**「会社の顔」「名刺代わり」** として、検索流入・名前検索・広告流入後の信頼確認・パートナー紹介後の閲覧など、**多様な経路の訪問者に対して講師と講座の信頼性を提示する**ことを目的とする。

### 1.2 利害関係者（Stakeholders）

| 役割 | 氏名 / 識別子 | 関心事 |
|---|---|---|
| プロダクトオーナー | colabpaint | サイト全体の方針・公開範囲・予算 |
| 講師（コンテンツ主） | ユウ（POSCA ARTIST） | 自身のブランド・コピー・写真 |
| 訪問者A | 広告流入後の「不安確認」層 | 「ちゃんとした人か」「実在するか」 |
| 訪問者B | 名前検索・SNS経由 | 「公式情報があるか」 |
| 訪問者C | メディア・取材依頼者 | 「窓口・実績情報」 |
| 訪問者D | 既存受講生・LINE登録済 | 「最新情報・実績の確認」 |
| 運営（保守者） | colabpaint / Claude Code | コピー・画像差し替えの容易さ |

### 1.3 成功指標（North Star + 補助KPI）

| 区分 | 指標 | 目標値 | 計測方法 |
|---|---|---|---|
| **North Star** | サイト訪問者の信頼スコア（離脱率の低さで代理） | 直帰率 60% 以下 | Vercel Analytics（任意導入） |
| 補助1 | LINE登録CTAクリック率 | 訪問の 3% 以上 | リンククリック計測（任意） |
| 補助2 | 講師ストーリーセクションの平均閲覧時間 | 30秒以上 | Heatmap（任意） |
| 補助3 | お問い合わせメール到達率 | 月3件以上 | メール受信ログ |

> ※ 第一目的は「印象向上・信頼構築」であり、CV最大化ではない。KPIは補助的な健全性指標として位置づける。

### 1.4 非目的（Non-Goals）

| # | 非目的 | 理由 |
|---|---|---|
| NG-1 | 強い販売コピー（PASTOR完全版・追従CTA・締切煽り）の実装 | 広告LP（`fv045`系）の役割であり、本サイトは別役割 |
| NG-2 | ユーザー登録・会員機能・決済機能 | LINE/メール導線で十分。複雑化は信頼を損なう |
| NG-3 | 多言語対応 | 国内向けで日本語のみ。コンテンツ自体が「言葉を使わない」性質 |
| NG-4 | CMS（WordPress 等）導入 | 更新頻度低・運用者1人・静的サイトで十分 |
| NG-5 | 動画埋め込みの自動再生 | 帯域・印象悪化リスク。サムネ→YouTube遷移で代替 |

---

## 2. スコープ

### 2.1 In Scope（範囲内）

- ブランドHPトップページ1枚（`course.html`、ルーティングで `/` に表示）
- 全10セクション（後述）の構造・コピー・装飾・レスポンシブ対応
- 講師写真（背景透過版・通常版）
- YouTube銀の盾画像（実物の写真ベース）
- 受講生YouTube動画への外部リンク（6本）
- mailto: ベースのお問い合わせ導線
- LINE登録CTA（ボタンUIのみ。リンク先は後日差し替え）
- Vercel（無料Hobbyプラン）による静的ホスティング
- GitHub による自動デプロイ連携
- フッター法務リンク（運営者情報・プライバシーポリシー・特商法表記）の骨格（中身は後日）

### 2.2 Out of Scope（範囲外）

- ブログ（更新運用が立ち上がるまで未実装）
- 問い合わせフォーム（mailtoで代替）
- 申込フォーム（LINEで代替）
- アクセス解析の本格運用（Vercel Analytics は任意導入）
- カスタムドメイン（必要になった時点で別ADRで判断）

### 2.3 将来スコープ（Backlog）

| ID | 項目 | 起動条件 |
|---|---|---|
| FUT-1 | 独自ドメイン取得（例: `oekaki-yu.com`） | プロダクト方針が固まったとき |
| FUT-2 | ブログ機能（記事5本以上の運用合意） | コンテンツ供給体制ができたとき |
| FUT-3 | プライバシーポリシー本文 | 法務確認できたとき |
| FUT-4 | 特定商取引法表記の本文 | 講座販売を正式に開始するとき |
| FUT-5 | アクセス解析の本格運用 | KPI追跡が必要になったとき |
| FUT-6 | OGP画像・構造化データ（JSON-LD）の追加 | SNSシェアが増えたとき |

---

## 3. ユーザーストーリー

### 3.1 主要ペルソナと心理状態

| ペルソナ | 行動文脈 | 期待 |
|---|---|---|
| **P1: 広告クリック直後の懐疑層** | 「絵だけで月70万円」の広告を見て、本サイトを別タブで確認 | 「ちゃんとした人か」「怪しくないか」 |
| **P2: 名前検索の確認層** | SNSや友人から名前を聞いて検索 | 「公式情報・実績の確認」 |
| **P3: メディア取材検討者** | TV・WebメディアのPDから連絡前の事前確認 | 「窓口・運営の実在確認」 |
| **P4: 既存LINE登録者** | 配信を見て、改めてサイトを訪問 | 「最新情報・実績の確認」 |

### 3.2 ユーザーストーリー一覧

#### US-001（P1: 懐疑層）
> **As a** 広告で刺激的訴求を見た直後の懐疑的訪問者として、  
> **I want** 講師の顔・実績・受講生の声を素早く確認したい、  
> **so that** 「ちゃんとした事業者かどうか」を3分以内に判断できる。

#### US-002（P2: 検索層）
> **As a** 名前検索で訪問した人として、  
> **I want** 講師の自己紹介と公式の連絡窓口を明確に見つけたい、  
> **so that** 「これが公式サイトだ」と確信できる。

#### US-003（P3: メディア）
> **As a** 取材検討者として、  
> **I want** メールでの問い合わせ窓口にすぐ到達したい、  
> **so that** 取材打診を 30 秒以内に開始できる。

#### US-004（P4: 既存LINE登録者）
> **As a** すでにLINE登録しているファンとして、  
> **I want** サイトを訪問しても重複してLINE登録を強制されない、  
> **so that** 押し付けがましさを感じずに最新情報を見られる。

#### US-005（運営）
> **As a** プロダクトオーナーとして、  
> **I want** 「ここの文字を変えて」「写真を差し替えて」を1メッセージで反映できる仕組みがほしい、  
> **so that** 専門知識なしに継続的な改善ができる。

---

## 4. 機能要件（FR: EARS構文）

> EARS Syntax:
> - **Ubiquitous**: `The <system> shall <response>`
> - **Event-driven**: `When <trigger>, the <system> shall <response>`
> - **State-driven**: `While <state>, the <system> shall <response>`
> - **Optional**: `Where <feature>, the <system> shall <response>`
> - **Unwanted**: `If <unwanted>, then the <system> shall <response>`

### 4.1 ページ構成・遷移

| FR | EARS文 | 検証方法 |
|---|---|---|
| **FR-001** | The system shall serve `course.html` as the document at path `/`. | `curl https://oekaki-youtuber-academy.vercel.app/` のHTMLが `course.html` の中身であること |
| **FR-002** | The system shall include all of the following 10 sections in the document, in order: Header, Hero, Philosophy, Voices, Achievement, FAQ, Profile, Curriculum, Contact, LINE CTA, Footer. | DOM の `section` 要素を順に走査し全11ブロックの存在確認（Header/Footer含む） |
| **FR-003** | When a visitor clicks an in-page navigation anchor (`#about`, `#voices`, `#profile`, `#contact`, `#line`), the system shall scroll to the corresponding section. | 手動操作・各アンカーの遷移確認 |
| **FR-004** | The system shall NOT auto-play any audio or video. | DOM 走査で `autoplay` 属性の不在を確認 |

### 4.2 Hero セクション

| FR | EARS文 | 検証方法 |
|---|---|---|
| **FR-101** | The Hero section shall display the eyebrow text "POSCA ART SCHOOL", the main catchphrase "ポスカを使って／楽しく描くことで／生きていく。" (3 lines), and the lead "紙とポスカとスマホ一台で／言葉を使わず世界とつながる／新しい働き方をお伝えしています" (3 lines, no punctuation). | テキストノードの完全一致確認 |
| **FR-102** | The Hero section shall display the instructor's transparent-background portrait (`images/portrait-transparent.png`). | DOM の `img` src 確認 |
| **FR-103** | The Hero section shall display 4 yellow diagonal stripe decorations (`hero-stripe-left-1/2`, `hero-stripe-right-1/2`). | DOM 要素の存在確認 |
| **FR-104** | The Hero section shall provide two action buttons: "講座を詳しく見る" (links to `#about`) and "LINEで情報受け取る" (links to `#line`). | リンク href 確認 |

### 4.3 Voices（受講生実績）セクション

| FR | EARS文 | 検証方法 |
|---|---|---|
| **FR-201** | The Voices section shall display exactly 6 voice cards, each linking to the corresponding YouTube interview video. | DOM `.voice-card` 数=6, 各 href が `youtu.be/` で始まる |
| **FR-202** | Each voice card shall display: thumbnail (YouTube maxresdefault), result text with monthly income, student name + attribute, summary sentence, and a play mark. | カード内部要素確認 |
| **FR-203** | The Voices section shall include the disclaimer "※ 結果には個人差があります。" at the bottom. | テキスト存在確認 |
| **FR-204** | When a voice card is clicked, the system shall open the YouTube video in a new tab with `rel="noopener"`. | DOM `target="_blank"` および `rel="noopener"` の存在確認 |

**受講生6名の必須情報**（コピー差替時の検証用テーブル）:

| # | 動画ID | 名前 | 属性 | 期間 | 月収 |
|---|---|---|---|---|---|
| 1 | jKBxKTys4dk | 町田さん | 専業主婦 | 5ヶ月 | 43万円 |
| 2 | nMsvwMgBtXw | 今西さん | 専業主婦 | 6ヶ月 | 50万円 |
| 3 | d4eJ1EHQ9kQ | 黒田さん | 元・看護師 | (脱サラ) | 67万円 |
| 4 | Q9zXh0Ntlms | 松井さん | 元・派遣社員 | 半年 | 70万円 |
| 5 | dZIaKGP1JRQ | 牧野さん | 元・会社員 | (毎月安定) | 70万円 |
| 6 | -M61MlG1Q_Q | 徳長さん | 元・飲食店勤務 | (旅先) | 80万円 |

### 4.4 Achievement（YouTube実績）セクション

| FR | EARS文 | 検証方法 |
|---|---|---|
| **FR-301** | The Achievement section shall display the silver shield photograph (`images/silver-shield-real.jpg`). | DOM `img` src 確認 |
| **FR-302** | The Achievement section shall display 3 statistics: "10万人+" (subscribers), "1,000万回+" (cumulative views), "80億人" (global reach). | テキスト存在確認 |
| **FR-303** | The Achievement section shall describe the silver shield as YouTube's official award for reaching 100,000 subscribers. | テキスト存在確認 |

### 4.5 FAQ セクション

| FR | EARS文 | 検証方法 |
|---|---|---|
| **FR-401** | The FAQ section shall include at least the following 5 questions: (1) 絵が下手でも大丈夫か, (2) 顔出しは必要か, (3) 何語で発信するか, (4) 在庫・発送は必要か, (5) 子育て中でも可能か. | DOM `.faq-item` 数≧5, 各テキスト確認 |
| **FR-402** | Each FAQ item shall display "Q." and "A." marks. | DOM `.q-mark` / `.a-mark` 存在確認 |

### 4.6 Profile（講師プロフィール）セクション

| FR | EARS文 | 検証方法 |
|---|---|---|
| **FR-501** | The Profile section shall display the instructor name "ユウ", role "POSCA ARTIST / SCHOOL FOUNDER", a portrait image, a facts list, and a multi-paragraph story. | DOM 各要素確認 |
| **FR-502** | The facts list shall include exactly these items: (a) 元・東京の美容師, (b) 3児の父, (c) 都会から地方への移住を実現, (d) YouTube銀の盾を保有（複数チャンネル）, (e) 累計再生回数1,000万回以上, (f) 国内で唯一、ポスカアートを教える講師. | 各 `li` テキスト確認 |

### 4.7 Curriculum セクション

| FR | EARS文 | 検証方法 |
|---|---|---|
| **FR-601** | The Curriculum section shall display exactly 6 items, numbered 01〜06, each with a title and description. | DOM `.curriculum-item` 数=6 |
| **FR-602** | The 6 items shall cover: (01) 基本技術, (02) SNS発信, (03) 広告収入, (04) 商品販売, (05) ライフスタイル設計, (06) 継続して成果を出す考え方. | テキスト確認 |

### 4.8 Contact セクション

| FR | EARS文 | 検証方法 |
|---|---|---|
| **FR-701** | The Contact section shall provide a `mailto:` link with a URL-encoded subject containing "【お問い合わせ】お絵かきYouTuberアカデミー". | DOM `a[href^="mailto:"]` の subject クエリ確認 |
| **FR-702** | The Contact section shall display the email address as plain text (for copy-paste fallback). | DOM `.contact-email-addr` テキスト確認 |
| **FR-703** | The Contact section shall NOT include any HTML form element (`<form>`). | DOM `form` 不存在確認 |

### 4.9 LINE CTA セクション

| FR | EARS文 | 検証方法 |
|---|---|---|
| **FR-801** | The LINE CTA section shall display a primary CTA button labeled "LINEで友だち追加". | テキスト確認 |
| **FR-802** | The LINE CTA section shall include the de-pressure note "不要になったら、いつでも解除できます。". | テキスト確認 |
| **FR-803** | Where the LINE official URL is not yet configured, the system shall use `href="#"` as a placeholder and document this gap. | 実装の placeholder + 本ドキュメントに既知ギャップとして記載 |

### 4.10 Footer セクション

| FR | EARS文 | 検証方法 |
|---|---|---|
| **FR-901** | The Footer shall display the brand name, tagline "描くことで、生きていく。", and copyright "© Oekaki YouTuber Academy 2026". | テキスト確認 |
| **FR-902** | The Footer shall include navigation links for 講座について, 講師について, LINE登録, お問い合わせ, 運営者情報, プライバシーポリシー, 特定商取引法に基づく表記. | リンク存在確認（中身は placeholder 可） |

---

## 5. 非機能要件（NFR）

### 5.1 パフォーマンス

| NFR | EARS文 | 目標値 |
|---|---|---|
| **NFR-P1** | When a visitor opens the root URL from a standard 4G mobile connection, the system shall render the LCP (Largest Contentful Paint) within 2.5 seconds. | Core Web Vitals "Good" |
| **NFR-P2** | The system shall maintain a CLS (Cumulative Layout Shift) below 0.1. | Core Web Vitals "Good" |
| **NFR-P3** | The total page weight (initial transfer) shall not exceed 2 MB. | リソース合計確認 |

### 5.2 アクセシビリティ

| NFR | EARS文 | 目標値 |
|---|---|---|
| **NFR-A1** | The system shall provide alternative text (`alt` attribute) for every meaningful image. | DOM `img[alt]` 完全カバー |
| **NFR-A2** | Body text shall maintain a contrast ratio of at least 4.5:1 against its background (WCAG AA). | デザイントークン側で担保 |
| **NFR-A3** | All interactive elements (links, buttons) shall be reachable and operable via keyboard. | Tab キー検証 |

### 5.3 SEO・メタデータ

| NFR | EARS文 | 目標値 |
|---|---|---|
| **NFR-S1** | The system shall provide a `<title>` containing the brand name. | DOM `head > title` 確認 |
| **NFR-S2** | The system shall provide a `<meta name="description">` summarizing the brand in under 160 characters. | 文字数確認 |
| **NFR-S3** | Where SNS sharing is anticipated, OGP/Twitter Card metadata shall be included. | **将来要件（FUT-6）** |

### 5.4 レスポンシブ

| NFR | EARS文 | 目標値 |
|---|---|---|
| **NFR-R1** | The system shall provide layouts optimized for viewports of 1440px (PC), 768px–900px (tablet), 480px (mobile), and 375px (small mobile / iPhone SE). | CSS `@media (max-width: 900px)`, `@media (max-width: 480px)` 確認 |
| **NFR-R2** | When viewed on viewports ≤480px, the Hero section shall fit within a single viewport height where reasonably possible. | 手動確認 |

### 5.5 セキュリティ

| NFR | EARS文 | 対策 |
|---|---|---|
| **NFR-Sec1** | The system shall serve all content over HTTPS. | Vercel デフォルト |
| **NFR-Sec2** | The system shall send `Strict-Transport-Security` (HSTS) headers. | Vercel デフォルト |
| **NFR-Sec3** | All external links (target="_blank") shall include `rel="noopener"` to prevent reverse tabnabbing. | DOM 一括検証 |
| **NFR-Sec4** | The system shall NOT embed any third-party JavaScript that is not strictly required. | 現状で外部JSは 0 件であることを維持 |

### 5.6 可用性

| NFR | EARS文 | 目標 |
|---|---|---|
| **NFR-Av1** | The system shall maintain >= 99.9% monthly availability (≤ 43 min/month downtime). | Vercel SLA に依存 |

### 5.7 保守性

| NFR | EARS文 | 目標 |
|---|---|---|
| **NFR-M1** | The system shall be modifiable by a non-developer with AI assistance (Claude Code) through single natural-language instructions. | "ここの文字をXに変えて" で完結する設計 |
| **NFR-M2** | Source files shall be limited to: `course.html`, `course.css`, `images/*`, `vercel.json`. No build step. | リポジトリ構成確認 |

### 5.8 法務・プライバシー

| NFR | EARS文 | 状態 |
|---|---|---|
| **NFR-L1** | If user personal data is collected (form submissions, cookies for analytics), the system shall provide a privacy policy. | **現状: PII 収集なし。フォーム未実装。Analytics 未導入。要件発火せず** |
| **NFR-L2** | If the site sells a paid course directly, the system shall provide a Specified Commercial Transaction Act notice. | **現状: 直接販売なし。要件発火せず。LINE経由で別フローに送る** |
| **NFR-L3** | Where the LINE link is enabled and configured, a clear opt-out statement shall be visible adjacent to the CTA. | 実装済み（FR-802） |

### 5.9 コンテンツ表現（広告関連）

| NFR | EARS文 | 根拠 |
|---|---|---|
| **NFR-C1** | When displaying student income testimonials, the system shall include a "結果には個人差があります" disclaimer. | 景品表示法・特商法上のリスク軽減 |
| **NFR-C2** | The system shall NOT make absolute guarantees of income (e.g., "誰でも必ず月収XX万円"). | 同上 |
| **NFR-C3** | The system shall keep all displayed monthly income claims consistent with the source YouTube interview content. | 4.3 のテーブルが正典 |

---

## 6. 制約条件（Constraints）

| C | 制約 | 由来 |
|---|---|---|
| C-1 | 静的サイト（バックエンドなし） | 運用工数最小化 |
| C-2 | ホスティング: Vercel Hobby プラン（無料） | 予算ゼロ |
| C-3 | リポジトリ: GitHub Public | Vercel Hobby は Private 不可（実質） |
| C-4 | 講師の本名・顔写真は公開可 | 講師合意済み |
| C-5 | 受講生の YouTube 動画は埋め込まずリンクのみ | 帯域・許諾範囲・印象を考慮 |
| C-6 | 言語は日本語のみ | 国内ターゲット |
| C-7 | カスタムフォントは Google Fonts のみ | ライセンス・配信安定性 |

---

## 7. 前提条件（Assumptions）

| A | 前提 | 検証手段 |
|---|---|---|
| A-1 | 講師ユウ本人が顔写真・本名公開に同意している | 講師との直接合意（要記録） |
| A-2 | 受講生6名の YouTube 動画は本人が出演し、リンク掲載に許諾済 | 講師経由で確認済の前提 |
| A-3 | LINE 公式アカウントは将来的に開設される | LINE Official Account 準備中 |
| A-4 | 「info@example.com」は仮アドレスであり、本番運用前に実アドレスに置換される | プロジェクト管理上の追跡項目 |
| A-5 | YouTube サムネ画像は `img.youtube.com` から取得可能 | Google 公開規約に基づく利用 |

---

## 8. 既知のギャップ・リスク

| ID | 内容 | 影響 | 対処期限 |
|---|---|---|---|
| **GAP-1** | LINE CTA の href が `#`（プレースホルダ） | LINE 登録が機能しない | LINE 公式開設次第（高優先度） |
| **GAP-2** | お問い合わせメールアドレスが `info@example.com`（仮） | 問い合わせが届かない | 公開ドメイン決定次第（高優先度） |
| **GAP-3** | フッターの法務リンクが空（プライバシーポリシー等） | 取材依頼・受講検討で減点 | 文面確定次第（中優先度） |
| **GAP-4** | OGP / Twitter Card 未設定 | SNS シェア時の見栄え悪化 | SNS 露出が増えたとき |
| **GAP-5** | アクセス解析未導入 | 改善判断の根拠データ欠如 | KPI 追跡が必要になったとき |
| **GAP-6** | カスタムドメイン未取得 | URL に `vercel.app` が含まれる | ブランド戦略確定後 |

---

## 9. 用語集（Glossary）

| 用語 | 定義 |
|---|---|
| ポスカ | 三菱鉛筆株式会社の水性顔料マーカーペン「POSCA」。本講座の主要画材 |
| ポスカアート | ポスカマーカーで描かれるアート作品。本講座が体系化した発信スタイル |
| 銀の盾 | YouTube クリエイターアワードの一段階。チャンネル登録者 10 万人達成の証として YouTube から授与される盾 |
| LIFF | LINE Front-end Framework。LINE 公式アカウント内で動作する Web アプリ基盤 |
| EARS | Easy Approach to Requirements Syntax。要件記述の構造化記法 |
| LCP / CLS | Largest Contentful Paint / Cumulative Layout Shift。Web Core Vitals 指標 |
| WCAG | Web Content Accessibility Guidelines |
| ADR | Architecture Decision Record。アーキテクチャ判断記録 |

---

## 10. 受け入れ基準（Definition of Done）

本要件定義書を満たすには、以下すべてを確認すること:

- [x] 公開URL `https://oekaki-youtuber-academy.vercel.app/` が 200 OK で `course.html` を返す（FR-001）
- [x] 10セクションすべてが描画される（FR-002）
- [x] 6 件の Voice カードがすべて表示され、各 YouTube リンクが存在する（FR-201）
- [x] 銀の盾画像と 3 統計値が表示される（FR-301, FR-302）
- [x] 講師プロフィールの 6 項目が正確に表示される（FR-502）
- [x] Curriculum 6 項目が表示される（FR-601）
- [x] mailto リンクが正しい subject を含む（FR-701）
- [x] LINE CTA に opt-out 注記が併記されている（FR-802）
- [x] HTTPS 配信・HSTS 有効（NFR-Sec1, NFR-Sec2）
- [x] レスポンシブが 1440 / 900 / 480 / 375px の各幅で破綻しない（NFR-R1）
- [x] 受講生証言に個人差注記がある（NFR-C1）
- [ ] 既知ギャップ GAP-1, GAP-2 の解消（**未完了**）
- [ ] 既知ギャップ GAP-3〜GAP-6 の解消（**未完了・優先度に応じ順次**）

---

## 11. C.U.T.E. セルフスコア

| 観点 | 配点 | 自己評価 | 根拠 |
|---|---|---|---|
| **Clear**（明瞭性） | 25 | 25 | 全 FR が EARS 構文、用語集を完備 |
| **Unambiguous**（一意性） | 25 | 24 | 各 FR に検証方法を併記。NFR-R2 のみ "可能な限り" を含む（-1） |
| **Testable**（検証可能性） | 25 | 25 | 全 FR が手動 or 自動で検証可能、Definition of Done あり |
| **Estimable**（見積可能性） | 25 | 25 | スコープ・非スコープ・ギャップ・将来要件を明示 |
| **合計** | **100** | **99** | 目標 ≧98 を達成 |

---

> **次の成果物**: `design.md`（C4 モデル設計書）、`tasks.md`（タスク分解）、`threat-model.md`、`slo.md`、`runbook.md`、`guardrails.md`、`adr/`。本要件定義書を変更する場合は、関連成果物すべての整合性を再確認のこと。
