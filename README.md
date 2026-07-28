# 積層 (Sekiso)

やったこと・できたことを加点だけで積み上げる、個人用の記録アプリ。

- 目標値・達成率・連続日数・未達表示なし
- 累計ポイントは減らない
- 記録した日だけが「地層」になり、記録しなかった日は表示されない

## 構成

外部依存ゼロの静的サイト。ビルド不要。

```
index.html               アプリ本体（HTML / CSS / JS すべて内包）
manifest.webmanifest     PWA マニフェスト
sw.js                    Service Worker（オフライン動作）
icon-192.png             アイコン
icon-512.png             アイコン
icon-maskable-512.png    マスカブルアイコン
```

## 公開手順（GitHub Pages）

1. リポジトリ直下に上記6ファイルを置く
2. Settings → Pages → Source: `Deploy from a branch` / Branch: `main` / `/(root)`
3. `https://<user>.github.io/<repo>/` で公開される
4. Android Chrome で開き、メニューから「アプリをインストール」

すべてのパスが相対指定なので、リポジトリ名に依存せず動作する。

## データの扱い

記録は端末内の IndexedDB / localStorage にのみ保存される。サーバーへの送信は一切ない。
そのため端末間の同期はなく、ブラウザの閲覧データ削除で失われる。
設定タブの「ファイルに書き出す」で JSON の控えを取れる。

`file://` と `https://` は別オリジンのため、ローカル版のデータは自動では引き継がれない。
書き出し → 「控えを読み込む」→「今のものと統合する」で移行する。

## 更新するとき

`index.html` を変更したら、**必ず `sw.js` の `VERSION` を上げる**。

```js
const VERSION = "sekiso-v1";   // → "sekiso-v2"
```

上げないと端末に古いキャッシュが残り、更新が反映されない。
