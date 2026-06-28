# SLO / SLI / SLA — お絵かきYouTuberアカデミー HP

| 項目 | 値 |
|---|---|
| Spec Slug | `oekaki-youtuber-academy-hp` |
| 採用記法 | Google SRE SLO 入門に準拠 |
| 評価期間 | 月次（30 日ローリング） |
| 最終更新 | 2026-06-29 |

---

## 0. SLI / SLO / SLA の定義

| 用語 | 意味 | 本プロダクトでの位置づけ |
|---|---|---|
| **SLI** (Service Level Indicator) | 計測する具体的な指標 | Vercel Edge ステータスコード、Core Web Vitals 等 |
| **SLO** (Service Level Objective) | SLI に対して我々が掲げる目標値 | 後述 |
| **SLA** (Service Level Agreement) | 外部に保証する契約 | **本プロダクトは個人運営のため SLA は提示しない**（Vercel SLA に依拠） |

---

## 1. SLI 一覧

| ID | SLI | 計測方法 | 計測頻度 |
|---|---|---|---|
| SLI-1 | **可用性** (Availability): `/` への HTTP 200 比率 | Vercel ダッシュボード or 外形監視（UptimeRobot 等） | 1 分間隔 |
| SLI-2 | **応答時間** (Latency): TTFB (Time To First Byte) | Vercel Analytics / PageSpeed Insights | 任意（月次） |
| SLI-3 | **LCP** (Largest Contentful Paint) | PageSpeed Insights / Core Web Vitals | 月次 |
| SLI-4 | **CLS** (Cumulative Layout Shift) | PageSpeed Insights / Core Web Vitals | 月次 |
| SLI-5 | **コンテンツ正確性**: リリースチェックリストの合格率 | 手動 (push ごと) | push ごと |
| SLI-6 | **外部リンク健全性**: Voice カードの YouTube リンクの 200 応答比率 | 手動・四半期 | 四半期 |
| SLI-7 | **問い合わせ到達率**: mailto から実際にメールが届いた比率 | メール受信ログ | 月次 |

---

## 2. SLO（目標値）

| ID | SLO | 目標 | エラーバジェット (30 日) | 根拠 |
|---|---|---|---|---|
| SLO-1 | 可用性 ≥ 99.9% | 月間 43.2 分以下の停止 | 43.2 分 | Vercel SLA に準拠（Vercel 自体は 100% を目指すが現実値で 99.9%+） |
| SLO-2 | 国内 TTFB ≤ 500ms (p90) | 90% 以上が 500ms 以内 | 10% | Edge CDN 配信のため余裕あり |
| SLO-3 | LCP ≤ 2.5s (mobile, p75) | "Good" 評価 | 25% | Core Web Vitals "Good" 基準 |
| SLO-4 | CLS ≤ 0.1 (p75) | "Good" 評価 | 25% | 同上 |
| SLO-5 | リリースチェックリスト合格率 100% | push ごとに 100% | 0%（不合格があれば即 revert） | 静的サイトなので合格率 100% が現実的 |
| SLO-6 | YouTube リンク有効率 100% | リンク切れゼロ | 0%（発覚次第 24h 以内に差替/削除） | 信頼の柱 |
| SLO-7 | 問い合わせ到達 95% | 95% 以上は実際に届く | 5% | SPAM 誤分類等の現実値を考慮 |

---

## 3. エラーバジェットポリシー

| 状態 | アクション |
|---|---|
| エラーバジェット残 > 50% | 通常運用。新機能・装飾追加 OK |
| エラーバジェット残 25%〜50% | 慎重運用。新機能は最低限の検証付き |
| エラーバジェット残 < 25% | **凍結**。バグ修正と信頼性向上のみ。新規装飾・新規セクション禁止 |
| エラーバジェット使い切り | 全面凍結。原因分析と恒久対策に集中 |

---

## 4. 監視・アラート設計

### 4.1 最小構成（現状）

| アラート | しきい値 | 通知先 | 状態 |
|---|---|---|---|
| Vercel デプロイ失敗 | 失敗時 | Vercel メール | ✅ デフォルト有効 |
| Vercel 利用量超過警告 | Hobby 上限の 80% | Vercel メール | ✅ デフォルト有効 |

### 4.2 拡張構成（将来）

| アラート | しきい値 | 通知先 | 起動条件 |
|---|---|---|---|
| サイトダウン (`/` が非 200) | 3 回連続失敗 | メール / LINE | UptimeRobot 等を導入時 |
| 国内 TTFB が 1s 超え | p90 > 1s が 24h 継続 | メール | PageSpeed 自動測定導入時 |
| エラーバジェット残 < 25% | 月次 | プロダクトオーナー | 月次レビュー定例化時 |

---

## 5. 月次レビュー（運用ルーチン）

| 項目 | 確認方法 | 担当 |
|---|---|---|
| 可用性実績 | Vercel ダッシュボード | colabpaint |
| LCP / CLS | PageSpeed Insights を `/` で実行 | colabpaint |
| 問い合わせ件数 | メール受信ログ | colabpaint |
| YouTube リンク健全性 | 6 リンクをクリックして 200 確認 | 四半期 |
| エラーバジェット消費 | 直近 30 日のダウン時間 | colabpaint |
| 既知ギャップ進捗 | tasks.md の E5 セクション | colabpaint |

---

## 6. 災害シナリオの想定 SLI 影響

| シナリオ | 影響を受ける SLI | 推定停止時間 | 対応手段 |
|---|---|---|---|
| Vercel 全国障害 | SLI-1, 2 | 数十分〜数時間 | runbook §3 (GitHub Pages 緊急切替) |
| GitHub 障害 | デプロイ不能（SLI-1 には即時影響なし） | 数十分〜数時間 | 既存デプロイは継続。修正は障害復旧後 |
| Google Fonts 障害 | LCP/CLS 一時悪化 | 数分〜数時間 | システムフォントで描画継続（Graceful） |
| YouTube img CDN 障害 | Voice カードのサムネ非表示 | 数分〜数時間 | プレースホルダ画像で代替（将来 FUT） |

---

> SLO は本ドキュメント所有者が四半期ごとに再評価する。実測値と目標値の乖離が大きい場合、目標を現実に合わせて引き上げる／引き下げる。
