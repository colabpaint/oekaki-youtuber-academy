# ADR-0002: `/` を `course.html` にルーティングする（rewrites）

| 項目 | 値 |
|---|---|
| 状態 | Accepted |
| 日付 | 2026-06-28 |
| 関連要件 | FR-001 |

---

## 1. 文脈・問題

`course.html` をブランドホームページの中核として磨いてきたが、Vercel のデフォルト挙動では `/` リクエストには `index.html` を返す。以前のリポジトリには古い `index.html` も残っており、これがそのまま `/` に表示されていた。

要件: 公開 URL `/` を開いたとき、磨き上げた `course.html` の内容を表示したい。同時に「直接 `course.html` を編集する」という指示・運用は維持したい（CLAUDE.md の指示を含め、ドキュメント全体が `course.html` を主とみなしている）。

## 2. 検討した選択肢

| 案 | 概要 | 利点 | 欠点 |
|---|---|---|---|
| A. `index.html` の中身を `course.html` でコピー保持 | 静的コピー | シンプル | 二重管理。同期忘れ事故が起きる |
| B. `course.html` を `index.html` にリネーム | ファイル名統一 | 標準的 | 既存ドキュメント・指示の全てを更新が必要、混乱を招く |
| C. **`vercel.json` の `rewrites` で `/` → `/course.html`** | URL を変えず内部書き換え | DRY、ファイル名そのまま、URL も `/` のまま | Vercel 固有設定（他に移植時は要対応） |
| D. `cleanUrls` 有効化 | `.html` を自動除去 | URL が綺麗 | 内部リンク（`#about` 等）と衝突したり、`course.html` が `/course` にリダイレクトされる副作用 |

## 3. 決定

**案 C: `vercel.json` の `rewrites` を使用** する。

```json
{
  "rewrites": [
    { "source": "/", "destination": "/course.html" }
  ]
}
```

### 理由

1. ファイル名 `course.html` を維持できる（既存ドキュメント・指示・運用知識が無変更）
2. 二重管理を回避できる（A 案の欠点を回避）
3. URL は `/` のまま、訪問者からは違和感なし
4. `cleanUrls` を有効化しない（D 案の副作用を回避）

## 4. 結果

- 古い `index.html` をリポジトリから削除（`git rm index.html`）
- `vercel.json` に上記設定を追加
- 公開 URL `/` で `course.html` の中身が表示される
- 直接 `/course.html` を開いても同じ内容（リダイレクトせず両方とも有効）

## 5. トレードオフ

- 他のホスティングへ移行時は同等の rewrite 設定が必要（GitHub Pages では使えない → runbook §3 で別手順を案内）
- `cleanUrls` を使えないため、サブページを追加する際に `.html` 拡張子が URL に残る（許容範囲）
