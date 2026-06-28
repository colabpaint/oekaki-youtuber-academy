# 採点結果（Critique） — お絵かきYouTuberアカデミー HP

| 項目 | 値 |
|---|---|
| Spec Slug | `oekaki-youtuber-academy-hp` |
| 採点モデル | C.U.T.E. (Clear / Unambiguous / Testable / Estimable) + 拡張観点 |
| 採点日 | 2026-06-29 |
| 採点者 | Claude Code (Opus 4.7) |
| 採点バージョン | v1.0 |

---

## 1. 総合スコア

| 観点 | 配点 | スコア | 評価 |
|---|---|---|---|
| **Clear**（明瞭性） | 25 | 25 | ✅ |
| **Unambiguous**（一意性） | 25 | 24 | ⚠️ |
| **Testable**（検証可能性） | 25 | 25 | ✅ |
| **Estimable**（見積可能性） | 25 | 25 | ✅ |
| **合計（C.U.T.E.）** | **100** | **99** | **L3 認定基準（98+）達成** |

---

## 2. 観点別の評価

### 2.1 Clear（明瞭性）— 25 / 25

✅ **満点理由**:
- 用語集（requirements §9）で「ポスカ」「銀の盾」「LIFF」等の業界固有用語を定義
- EARS 構文を全機能要件で採用、主体・条件・応答が明示
- C4 モデル（design.md §2-§5）で抽象から具体まで段階的に説明
- セクションごとに「何の話か」が見出しで分かる

🟡 **改善余地**:
- 一部のセクション（design §9 デザインシステム）は仕様書ではなく Source-of-Truth を CSS に委譲しているが、本来は色のヘキサ値まで本書に書くべきという指摘もあり得る。今回は CSS との二重管理を避ける判断。

---

### 2.2 Unambiguous（一意性）— 24 / 25

✅ **加点理由**:
- 各 FR に検証方法を併記（"テスト方法欄"）
- 受講生 6 名の名前・属性・月収をテーブルで列挙
- 必須要素（FAQ 5 項目、Curriculum 6 項目）の個数まで明示

⚠️ **減点理由（-1 点）**:
- **NFR-R2**: "When viewed on viewports ≤480px, the Hero section shall fit within a single viewport height **where reasonably possible**." の "reasonably possible" が曖昧
  - 改善案: 「iPhone SE (375×667) で実機確認時に Hero 全要素が縦スクロールなしで収まる」と具体化
  - 修正は次バージョンで

---

### 2.3 Testable（検証可能性）— 25 / 25

✅ **満点理由**:
- すべての FR に検証手段（DOM 確認、curl、目視）が紐づいている
- "受け入れ基準 (Definition of Done)"（requirements §10）にチェックボックスがあり、現状達成度が明示
- SLO 目標と SLI 計測方法が対応している（slo.md §1-§2）

---

### 2.4 Estimable（見積可能性）— 25 / 25

✅ **満点理由**:
- スコープ（In/Out/Future）が三層で明示（requirements §2）
- タスク一覧（tasks.md）で 42 件を分類済、各々に状態と DoD あり
- 既知ギャップ 6 件（GAP-1〜6）を優先度・期限・ブロッカー付きで列挙
- 残作業（E5）が 6 件、概算所要時間も threat-model §3 に併記

---

## 3. 拡張観点（参考）

これは C.U.T.E. ではないが、L3 認定の補強として確認:

| 観点 | 評価 | コメント |
|---|---|---|
| **Completeness**（網羅性） | A | 機能・非機能・運用・脅威・SLO・ガードレールを網羅 |
| **Traceability**（追跡性） | A | FR ↔ 設計 ↔ タスク ↔ 検証の双方向リンクあり |
| **Consistency**（整合性） | A | 受講生情報や 6 facts は要件・設計・実装で一致 |
| **Maintainability**（保守性） | A | 1 ファイル原則・No-Build・AI 編集対応で保守容易 |
| **Realism**（現実性） | A | 実装済み（本番公開済み）から逆算しており乖離なし |

---

## 4. 検出された課題と対処

| # | 課題 | 重要度 | 対処 |
|---|---|---|---|
| 1 | NFR-R2 の "reasonably possible" が曖昧 | Low | 次バージョンで具体的数値化 |
| 2 | アクセシビリティのコントラスト比（NFR-A2）の実測値が未記載 | Low | 実機で測定後に追記 |
| 3 | LINE CTA / mailto が placeholder 状態（GAP-1, 2） | High | tasks.md E5 で追跡中 |
| 4 | プライバシーポリシー本文未作成（GAP-3） | Mid | tasks.md T-503 で追跡 |
| 5 | GitHub / Vercel の 2FA 未確認 | High | threat-model §3 で追跡 |

---

## 5. L3 認定判定

| 基準 | しきい値 | 現状 | 判定 |
|---|---|---|---|
| C.U.T.E. スコア | ≥ 98 | 99 | ✅ |
| 全成果物完備 | 9/9 | 9/9 | ✅ |
| 設計と実装の乖離 | なし | なし（本番公開済） | ✅ |
| 既知ギャップ管理 | あり | あり（GAP-1〜6 で追跡） | ✅ |
| 運用手順 | あり | runbook.md | ✅ |

→ **L3 (Implementation Ready) 認定**

---

## 6. 次のアクション（採点者からの推奨）

1. **GAP-1 / GAP-2 を最優先で解消**（LINE URL / 本番メールアドレス）
2. **GitHub / Vercel の 2FA 設定**（threat-model §3 アクション 1, 2）
3. **次回採点（v1.1）時に NFR-R2 を具体化**
4. **アクセシビリティ計測ツール**（Lighthouse 等）で WCAG AA を客観評価
5. **月次レビュー**を定例化（slo.md §5）

---

> 採点は本ドキュメント所有者が四半期ごとに見直す。コピー・構成の大規模変更時にも再採点する。
