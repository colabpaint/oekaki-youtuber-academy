# Runbook（運用手順書） — お絵かきYouTuberアカデミー HP

| 項目 | 値 |
|---|---|
| Spec Slug | `oekaki-youtuber-academy-hp` |
| 対象運用者 | colabpaint（非エンジニア・AI 補助前提） |
| 最終更新 | 2026-06-29 |

> 本書は **AI 補助（Claude Code）と一緒に運用する前提** で書かれている。各手順には「AI に何と頼めばよいか」のコピペ用テンプレを併記する。

---

## 0. 平常運用フロー（一般的な編集）

### 0.1 「ここの文字を変えて」「写真を差し替えて」

#### あなたの作業

1. Cursor / Claude Code を開く
2. 以下のテンプレで AI に頼む:

```
お絵かきYouTuberアカデミーの公開HPで、
[どのセクションの] [どの部分を]
[こう変えて] ください。
変更したら git push して公開版に反映してください。
```

例:
```
ヒーローのキャッチコピー「ポスカを使って 楽しく描くことで 生きていく。」を
「ポスカで生きる、楽しさを伝える」に変えてください。push までお願いします。
```

#### AI の作業（参考）

1. 該当ファイルを Read
2. Edit で修正
3. `git add` → `git commit` → `git push`
4. Vercel が 30 秒〜2 分で反映

#### 反映確認

- URL を開いてハードリロード (`Cmd+Shift+R`)

---

## 1. インシデント対応

### 1.1 サイトが表示されない（500 / 全停止）

#### 第一手（5 分以内）

```
[AI 向けテンプレ]
公開サイト oekaki-youtuber-academy.vercel.app が表示できません。
状況を診断して、可能なら原因と対処を教えてください。
```

#### AI 推奨手順

1. `curl -I https://oekaki-youtuber-academy.vercel.app/` でステータス確認
2. Vercel ダッシュボードで最新デプロイのステータス確認
3. 直前の commit を確認（`git log -5`）
4. 直前の commit でデプロイが失敗していれば revert

#### 緊急回避（Vercel 障害時）

`./scripts/emergency-github-pages.sh`（**未作成**・将来 FUT-7 として整備）

応急処置として GitHub Pages を有効化する手順:

```
1. GitHub リポジトリ → Settings → Pages
2. Source: Deploy from a branch → main / root を選択
3. Save → 数分で https://colabpaint.github.io/oekaki-youtuber-academy/course.html で配信開始
4. SNS や顧客対応で代替 URL を案内
5. Vercel 復旧後に GitHub Pages を Disable
```

---

### 1.2 Voice カードのサムネが映らない

#### 原因可能性

1. YouTube 動画が削除された
2. YouTube img CDN の障害
3. 動画 ID の typo

#### AI 向けテンプレ

```
Voiceセクションで [名前]さんのサムネイル画像が表示されません。
動画IDが [XXX] です。リンクが生きているか確認して、
死んでたら一旦そのカードを非表示にしてください。
```

#### AI 推奨手順

1. `curl -I "https://img.youtube.com/vi/{ID}/maxresdefault.jpg"` で 200 確認
2. `https://youtu.be/{ID}` を開いて動画が生きているか確認
3. 死んでいたら、該当 `.voice-card` を一時的に非表示（`hidden` 属性 or CSS `display:none`）
4. 講師に連絡して新しい受講生動画があれば差し替え

---

### 1.3 デプロイが失敗する

#### AI 向けテンプレ

```
直前の push でVercelデプロイが失敗しました。
ログを見て原因を教えてください。可能なら自動修復してください。
```

#### AI 推奨手順

1. Vercel CLI または gh CLI でビルドログ取得
2. 静的サイトなので主な失敗原因は (a) `vercel.json` の文法エラー、(b) 巨大ファイル誤コミット
3. 直前 commit を revert → push で復旧
4. 原因を特定して再 commit

---

### 1.4 不適切なコンテンツの報告を受けた

#### 第一手（1 時間以内）

1. 報告内容を確認
2. 該当部分を即座に非公開化（コミットで削除 → push）
3. 報告者に対応完了の連絡

#### AI 向けテンプレ

```
[受講生 / セクション] [○○] の掲載に関して、本人から削除依頼が来ました。
該当部分を直ちに非表示にしてください。
削除前の状態は git 履歴に残るので、後で復旧可能です。
```

---

## 2. 定期メンテナンス

### 2.1 月次タスク（月初に実行）

| 項目 | 所要時間 | 確認方法 |
|---|---|---|
| Vercel ダッシュボードでエラー率確認 | 5 分 | vercel.com/colabpaint-studio |
| PageSpeed Insights で LCP/CLS 計測 | 10 分 | pagespeed.web.dev で URL 入力 |
| 問い合わせメール受信数の確認 | 5 分 | Gmail / 利用メーラー |
| 既知ギャップ（GAP-1〜6）の進捗確認 | 10 分 | tasks.md の E5 |

### 2.2 四半期タスク

| 項目 | 所要時間 | 確認方法 |
|---|---|---|
| Voice カード 6 件の YouTube リンク健全性 | 10 分 | 6 リンクをクリック |
| 既知ギャップの再優先度づけ | 30 分 | チーム合議 |
| 講師写真の更新検討 | 30 分 | 講師と相談 |

### 2.3 年次タスク

| 項目 | 所要時間 | 確認方法 |
|---|---|---|
| フッターの著作権年号更新 | 5 分 | `© Oekaki YouTuber Academy 2026` を翌年へ |
| 全体のコピー見直し（古びていないか） | 1 時間 | 通読 |
| 既知ギャップ・将来要件のリスタート | 1 時間 | requirements.md §8, tasks.md E6 |

---

## 3. 緊急切替手順（Vercel 障害時の GitHub Pages フォールバック）

> 完全な手順書。Vercel 全停止のような重大障害時にのみ実行。

### 前提条件

- GitHub リポジトリ `colabpaint/oekaki-youtuber-academy` への push 権限あり
- リポジトリは Public

### 手順

```
1. GitHub にログイン
2. リポジトリ Settings → Pages
3. Build and deployment:
   - Source: "Deploy from a branch"
   - Branch: main / root (/)
4. Save
5. 3〜5 分で https://colabpaint.github.io/oekaki-youtuber-academy/course.html が稼働
6. 公式 SNS / メール署名で代替 URL を周知
7. Vercel 復旧確認後、GitHub Pages を Disable
```

### 補足

- GitHub Pages は HTTPS 対応
- `vercel.json` の rewrites は GitHub Pages では効かないため、`/` ではなく `/course.html` で配信される
- これが嫌なら、緊急時のみ `index.html` を新規作成して `course.html` に redirect する HTML を入れる:

```html
<!doctype html><meta http-equiv="refresh" content="0; url=/oekaki-youtuber-academy/course.html">
```

---

## 4. AI と運用するうえでの注意

### 4.1 AI に頼むときの良い指示

✅ **OK**:
- 「ヒーローの文字を 48px から 60px に変えて」
- 「黒田さんの月収を 67 万円から 68 万円に変えて、push までして」

❌ **避けたい**:
- 「ちょっといい感じに直して」（指示が曖昧）
- 「全部リファクタして」（破壊的変更を招く）

### 4.2 AI が間違えたとき

- Cursor のチャットで「直前の変更を取り消して」と言えば、`git revert` で巻き戻せる
- 一番安全な巻き戻し:

```
[AI 向けテンプレ]
直前の commit を revert してください。
revert commit を push して、公開版を一つ前の状態に戻してください。
```

---

## 5. 連絡先・参照先

| 用途 | URL / 連絡先 |
|---|---|
| 公開サイト | https://oekaki-youtuber-academy.vercel.app/ |
| 主リポジトリ | https://github.com/colabpaint/oekaki-youtuber-academy |
| アーカイブリポジトリ | https://github.com/colabpaint/20260629oekaki-youtuber-academyHP |
| Vercel ダッシュボード | https://vercel.com/colabpaint-studio (Hobby team) |
| 受講生対談動画 (YouTube) | requirements §4.3 のテーブル参照 |
| 設計資料 | `docs/specs/oekaki-youtuber-academy-hp/` |

---

## 6. よくあるトラブル即解決集

| 症状 | 即解決アクション |
|---|---|
| 反映されない | ハードリロード (`Cmd+Shift+R`) |
| まだ反映されない | Vercel ダッシュボードでデプロイ完了確認、未完了なら待つ |
| 画像が表示されない | パス確認、ファイル名のスペル確認、`git status` で commit 済か確認 |
| LINE ボタンが効かない | href が `#` のまま（GAP-1）。LINE 公式 URL に差し替え |
| メールが届かない | mailto アドレスが仮（GAP-2）。実アドレスに差し替え |
| フッターの法務リンクが空 | placeholder（GAP-3）。本文ページ作成して差し替え |
