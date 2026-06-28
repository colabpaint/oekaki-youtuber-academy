# タスク分解 (Tasks) — お絵かきYouTuberアカデミー HP

| 項目 | 値 |
|---|---|
| Spec Slug | `oekaki-youtuber-academy-hp` |
| 記法 | Kiro 形式（タスク = エピック・ストーリー・サブタスク）｜ Done / Doing / Todo を明示 |
| 関連要件 | `requirements.md` |
| 関連設計 | `design.md` |
| 最終更新 | 2026-06-29 |

---

## 0. ステータスサマリ

| エピック | 件数 | Done | Doing | Todo |
|---|---|---|---|---|
| E1: 要件・素材整備 | 6 | 6 | 0 | 0 |
| E2: HP実装 | 12 | 12 | 0 | 0 |
| E3: デプロイ・公開 | 5 | 5 | 0 | 0 |
| E4: SDD ドキュメント | 7 | 7 | 0 | 0 |
| E5: 既知ギャップ解消 | 6 | 0 | 0 | 6 |
| E6: 将来拡張 | 6 | 0 | 0 | 6 |
| **合計** | **42** | **30** | **0** | **12** |

---

## E1. 要件・素材整備 ✅ (6/6 done)

| ID | タスク | 状態 | 関連 | 完了日 |
|---|---|---|---|---|
| T-101 | 参考 LP 4 URL（real045 / fv045 x3）の構造分析 | Done | `research/lp-analysis.md` | 2026-06-09 |
| T-102 | 講師・受講生コンテンツ（11文書）精読・要約 | Done | `research/poskart-source-summary.md` | 2026-06-09 |
| T-103 | LP 設計骨子（11セクション案）作成 | Done | `research/poskart-lp-blueprint.md` | 2026-06-10 |
| T-104 | 設計ブリーフ作成（目的・トーン・ターゲット） | Done | `research/poskart-lp-brief.md` | 2026-06-10 |
| T-105 | プロジェクト永続情報シート整備 | Done | `PROJECT-RESOURCES.md` | 2026-06-10 |
| T-106 | スタイル／フォントカタログ作成 | Done | `lp-mockup/font-catalog.html` 他 | 2026-06-11 |

---

## E2. HP 実装 ✅ (12/12 done)

| ID | タスク | 状態 | DoD（受け入れ条件） | 関連要件 |
|---|---|---|---|---|
| T-201 | HTML 骨格（10 セクション）作成 | Done | DOM に全セクション存在 | FR-002 |
| T-202 | Hero セクション実装（コピー・写真・斜めストライプ） | Done | FR-101〜104 を満たす | FR-101〜104 |
| T-203 | Philosophy セクション実装 | Done | 句読点なしコピー反映 | FR-002 |
| T-204 | Voices セクション実装（6 カード） | Done | YouTube リンク・サムネ・名前・属性が完全 | FR-201〜204 |
| T-205 | Achievement セクション実装（銀の盾） | Done | 写真・3 統計の表示 | FR-301〜303 |
| T-206 | FAQ セクション実装（5 項目以上） | Done | Q/A マーク・5 項目 | FR-401, 402 |
| T-207 | Profile セクション実装（軌跡） | Done | 6 facts + 5 段落ストーリー | FR-501, 502 |
| T-208 | Curriculum 6 項目実装 | Done | 番号付き・6 項目 | FR-601, 602 |
| T-209 | Contact mailto 実装 | Done | URL-encoded subject 含む | FR-701〜703 |
| T-210 | LINE CTA 実装（opt-out 注記つき） | Done | プレースホルダ運用＋注記表示 | FR-801〜803, GAP-1 |
| T-211 | Footer 実装（法務リンク骨格） | Done | コピーライト・ナビ・法務リンク（placeholder） | FR-901, 902 |
| T-212 | レスポンシブ（900px / 480px） | Done | 各幅で破綻なし | NFR-R1, R2 |

---

## E3. デプロイ・公開 ✅ (5/5 done)

| ID | タスク | 状態 | DoD | 関連 |
|---|---|---|---|---|
| T-301 | GitHub リポジトリ作成・初回 push | Done | `colabpaint/oekaki-youtuber-academy` が存在 | C-3 |
| T-302 | Vercel プロジェクト作成（Hobby） | Done | Vercel に project 存在 | C-2 |
| T-303 | GitHub 連携・自動デプロイ確認 | Done | push → 1〜2 分で edge 反映 | NFR-Av1 |
| T-304 | `vercel.json` で `/` → `course.html` ルーティング | Done | curl `/` が course.html を返す | FR-001 |
| T-305 | アーカイブリポジトリへ全資料 push | Done | `colabpaint/20260629oekaki-youtuber-academyHP` に push 済 | — |

---

## E4. SDD ドキュメント ✅ (7/7 done)

| ID | タスク | 状態 | 成果物 |
|---|---|---|---|
| T-401 | requirements.md 作成（EARS + C.U.T.E. 採点） | Done | `docs/specs/oekaki-youtuber-academy-hp/requirements.md` |
| T-402 | design.md 作成（C4 モデル） | Done | `docs/specs/oekaki-youtuber-academy-hp/design.md` |
| T-403 | tasks.md 作成（本ファイル） | Done | このファイル |
| T-404 | threat-model.md 作成（STRIDE） | Done | `docs/specs/oekaki-youtuber-academy-hp/threat-model.md` |
| T-405 | slo.md 作成 | Done | `docs/specs/oekaki-youtuber-academy-hp/slo.md` |
| T-406 | runbook.md 作成 | Done | `docs/specs/oekaki-youtuber-academy-hp/runbook.md` |
| T-407 | guardrails.md / critique.md / score.json / ADR x6 作成 | Done | `docs/specs/oekaki-youtuber-academy-hp/{guardrails,critique}.md`, `score.json`, `adr/` |

---

## E5. 既知ギャップ解消 ⚠️ (0/6 done — Todo)

| ID | タスク | 優先度 | 関連 GAP | DoD | ブロッカー |
|---|---|---|---|---|---|
| T-501 | LINE 公式アカウント開設・href 差し替え | **High** | GAP-1 | `.line-cta` の `href` が実 URL | LINE 公式開設準備 |
| T-502 | 本番メールアドレス取得・`info@example.com` 置換 | **High** | GAP-2 | mailto: が実アドレス、平文も同期 | ドメイン or Gmail 決定 |
| T-503 | プライバシーポリシー本文作成 | Mid | GAP-3 | `/privacy.html` 作成・footer リンク差し替え | 文面ドラフト承認 |
| T-504 | 特定商取引法に基づく表記の本文作成 | Mid | GAP-3 | `/tokushoho.html` 作成・footer リンク差し替え | 直販開始の有無確認 |
| T-505 | 運営者情報ページ作成 | Mid | GAP-3 | `/about-operator.html` 作成・footer リンク差し替え | 法人/個人事業の確認 |
| T-506 | OGP / Twitter Card メタデータ追加 | Low | GAP-4 | `<head>` に og:* / twitter:* メタ追加・専用画像配置 | OGP 画像作成 |

### T-501〜506 の実行順推奨

1. T-501 → 2. T-502 → 3. T-505 → 4. T-503 → 5. T-504 → 6. T-506

> 理由: T-501/502 は機能停止系・即影響大。T-503〜505 は法務的な静的ページで内容承認待ち。T-506 は見栄え系。

---

## E6. 将来拡張 (0/6 done — Backlog)

| ID | タスク | 優先度 | 関連 | DoD | 起動条件 |
|---|---|---|---|---|---|
| T-601 | カスタムドメイン取得・Vercel 設定 | Low | FUT-1 | `oekaki-yu.com` 等で配信 | ブランド名確定 |
| T-602 | ブログ機能追加（Astro 等） | Low | FUT-2 | `/blog/` が稼働 | 記事 5 本以上の供給合意 |
| T-603 | アクセス解析導入（Vercel Analytics or Plausible） | Mid | FUT-5 | ダッシュボードで指標確認可 | KPI 追跡開始 |
| T-604 | 講師写真の本物差し替え（更新時） | Low | A-1 | 新写真で表示 | 新写真撮影 |
| T-605 | 受講生対談の動画追加（7 人目以降） | Low | — | Voice カード追加 | 講師同意・新動画公開 |
| T-606 | パフォーマンス最適化（WebP 化 等） | Low | NFR-P1 | LCP < 2.0s | 通信ボトルネック顕在化時 |

---

## 1. 受け入れテスト計画（軽量）

> 本プロダクトは静的サイトで自動テストインフラを持たない。代わりに**チェックリスト形式の手動テスト**を採用する。

### 1.1 リリース時チェックリスト（毎 push 後 30 秒で実行）

- [ ] `https://oekaki-youtuber-academy.vercel.app/` が 200 OK
- [ ] Hero キャッチが「ポスカを使って 楽しく描くことで 生きていく。」（句読点なし）
- [ ] Voice カードが 6 枚表示
- [ ] 銀の盾画像が表示（404 でない）
- [ ] mailto: ボタンが反応する（クリックでメールクライアント起動）
- [ ] レスポンシブが iPhone SE 想定 (375px) で破綻なし

### 1.2 月次レビュー（運用開始後）

- [ ] Vercel ダッシュボードでエラー率確認
- [ ] お問い合わせメール到達確認
- [ ] LINE 登録数の変化（GAP-1 解消後）
- [ ] 既知ギャップの進捗確認

---

## 2. リスクと対処

| リスク | 影響 | 対処 | 担当 |
|---|---|---|---|
| LINE CTA が `#` のまま放置 | 期待した導線が機能しない | T-501 を最優先で実行 | colabpaint |
| メールアドレスが仮のまま放置 | 取材依頼が届かない | T-502 を最優先で実行 | colabpaint |
| YouTube 動画削除でサムネ消失 | Voice カードの画像が読めない | サムネのミラー保存を T-605 に併記 | 運営 |
| Vercel 障害 | サイト全停止 | runbook.md の手順で GitHub Pages に切替 | colabpaint |

---

> **次の成果物**: `threat-model.md`（STRIDE）、`slo.md`、`runbook.md`、`guardrails.md`、`critique.md`、`score.json`、`adr/`
