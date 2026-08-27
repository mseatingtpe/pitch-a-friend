# Pitch a Friend｜素材收集問答網頁

在 Pitch a Friend 派對上台幫朋友做簡報前，先讓本人自己填的素材表。
24 題分 5 段，手機優先，填完一鍵複製 / 下載 / mailto 傳回來。

**網址**：https://mseatingtpe.github.io/pitch-a-friend/

## 設計重點

- **純前端，沒有後端**。答案只存在填答者自己的瀏覽器 `localStorage`，
  不送任何 request、沒有 analytics、沒有外部 CDN。
  只有按下「複製 / 下載 / 寄出」，資料才會離開她的手機。
- **repo 是 public，但答案不會進 repo**——這裡只有題目。
- 全部塞在 `index.html` 一個檔：HTML + inline CSS + inline JS，無 build、無相依套件。

## 怎麼改題目

改 `index.html` 裡的 `SECTIONS` 陣列就好，**不用碰任何邏輯**，畫面會照著重新生成。

```js
{ id: 'story', type: 'textarea', required: false,
  label: '題目', hint: '補充說明（可省略）' }
```

- `type`：`'text'`（單行）或 `'textarea'`（多行）
- `id`：拿來當 localStorage 的 key，**改了會讓填答者的舊草稿對不上**，上線後盡量別動
- `required: true` 才會擋關，目前只有第一題是必填

改收件人：同檔案上方的 `RECIPIENT_EMAIL` / `RECIPIENT_NAME` 兩個常數。

## 部署

推上 `main` 就會自動更新（GitHub Pages，source = `main` / root）。

```bash
git add -A && git commit -m "feat: ..." && git push
```

## 測試

`/tmp/paf-test/test.mjs` 有一份 jsdom 煙霧測試（36 項：渲染、必填擋關、草稿續填、
輸出格式、三個出口、清除）。它是臨時的，不在這個 repo 裡——要重跑就重寫一份。

**最容易壞的一環是 iOS Safari 的「一鍵複製」**，改完一定要用實機走一次，
不能只在桌機瀏覽器上測。
