# ATEEZ 應援練習網站：給 AI 的共用規則

這個專案由 GPT／Codex 和 Claude 輪流維護，兩邊都要遵守這份規則。用中文回覆。

## 工作流程
1. 開始前先提醒使用者在 GitHub Desktop 按「Fetch origin / Pull」，拿到最新版本，不要用舊的 HTML 覆蓋。
2. 只改使用者要求的部分，保留既有功能（包含別人加的）。
3. 改完用中文簡短說明改了哪些地方，再提醒使用者到 GitHub Desktop Commit 和 Push。
4. 分工（2026-10-28 起）：Claude 只在本機資料夾 `~/Documents/Claude ai/ATEEZ 應援` 改檔案，不 Commit、不 Push；GPT／Codex 在同一個資料夾檢查 `git diff` 後負責 Commit 和 Push。上傳時只上傳現有的修改，不要重寫或覆蓋檔案。

## 檔案
- `index.html`：介面、功能、預設歌單（DEFAULT_SONGS）。
- `lyrics.js`：完整歌詞與應援標記（FULL_LYRICS），由 index.html 載入。
- 兩個檔都在 main 分支根目錄，GitHub Pages 會自動發布。
- 公開網站：https://j0151222.github.io/ateez-fanchant/

## 省 token：只讀需要的檔案和段落
- 改介面／功能：只碰 `index.html`（約 60KB），**不要讀 `lyrics.js`**。先用 `grep -n` 找到段落再讀那幾行。
- 改歌詞：只碰 `lyrics.js`，用 `grep -n '^  "歌名"'` 找到那首歌，只讀那一段。
- 要確認歌詞資料時，用程式解析後只印出歌名和行數，不要印出全文。

## 改 lyrics.js 的規則
- 格式：一首歌一段，`"歌名": { "page": 頁數 或 "source": "來源", "lines": [ 每句一行 ] }`。
- 新增歌詞時，歌名要和 DEFAULT_SONGS 的 title 一致（比對時忽略大小寫、空白和符號）。
- **每次改 lyrics.js，都要把 index.html 裡 `<script src="lyrics.js?v=1">` 的數字加 1**，不然訪客的瀏覽器可能還在用舊歌詞。

## 程式結構（用 grep 找這些標記）
- `<style>`：CSS。完整歌詞的樣式在 `.lyrics-list`。
- `const DEFAULT_SONGS`：預設歌單，目前 18 首。
- `<script src="lyrics.js?v=…">`：載入歌詞檔；`lyricKey()`、`lyricsFor()` 依歌名找歌詞。目前 14 首有歌詞。
- `/* ---------- 儲存 ---------- */`：`persist()`、`load()`，localStorage key 是 `ateez-room-v1`。
- `/* ---------- 歌單 ---------- */`、`/* ---------- 應援稿面板 ---------- */`、`/* ---------- 播放器 ---------- */`
- `/* ---------- 只顯示應援字、每句倒數、時間微調 ---------- */`：`countdown()`、`shiftCue()`、`setCueNow()`；整首偏移存在歌曲物件的 `offset`（秒，正數＝延後），使用者微調後的時間存在歌曲的 `tl`。
- `/* ---------- 編輯 ---------- */`、`/* 邊看邊打 */`、`/* 打點模式 */`、`/* ---------- 啟動 ---------- */`

## 一定要保留
- 現有版型，以及播放、A–B 循環、收藏、歌單編輯、打點、備份上傳／下載功能。
- 「完整歌詞／同步應援稿」切換。
- 已加入的歌詞、彩色應援標記（`<span class="lyric-...">`）、括號發音與動作提示。
- 使用者瀏覽器裡的自訂資料：不要改 localStorage key，也不要清除 state。

## 歌詞資料
- 歌詞是完整原文，不是翻譯。只使用使用者提供的資料，不要自行補寫缺少的歌詞。
- 不要修改 DEFAULT_SONGS 和 FULL_LYRICS 的內容，除非使用者明確要求調整格式。
- 不要編造時間碼。同步時間只用使用者提供的，或使用者在網站上用「微調時間」自己調的。
- 還沒有歌詞的歌：Adrenaline、Lemon Drop、Deja Vu、Answer。
