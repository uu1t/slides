<!-- $theme: gaia -->

# 新しいタブで学習する
@uu1t
フリーなITもくもく会＠ギーク
2019/05/03 (金)

---

# 自己紹介
- 谷川 祐一　@uu1t
- フロントエンドエンジニア
- Vue.js, Rails を書いています
    - 業務 Rails 歴半年

---

# Rails がわからない 🤔 
- メソッドが多い（自分調べ）
    - v5.2： 3,675
    - v5.0： 3,483
- 単語帳みたいに普段から学習すればよいのでは？

---

# 作ったもの
- [Random Rails API tab](https://github.com/uu1t/random-rails-api-tab)
- 新しいタブで Rails のドキュメントをランダムに開くブラウザ拡張
    - ドキュメント： https://api.rubyonrails.org/
- [Chrome](https://chrome.google.com/webstore/detail/random-rails-api-tab/ngbeahjnndjoedgapccnoennbmalppbk) と [Firefox](https://addons.mozilla.org/en-US/firefox/addon/random-rails-api-tab/) 用を公開中

---

# 実装 (1)
- クローラ
    - https://api.rubyonrails.org/ をクローリングして、メソッドとその URL のデータを作成
	- Scrapy を使用
    - JavaScript で読み込めるように JSON として出力

---

# 実装 (2)
- ブラウザ拡張
	- 新しいタブを上書き
    - メソッド一覧の JSON を読み込む
    - ランダムに 1 つ選んで `iframe` で表示

---

# 複数バージョン対応
- 複数のバージョンのドキュメントがある
    - https://api.rubyonrails.org/v5.2/
    - https://api.rubyonrails.org/v5.1/
- どれを表示するか設定で選べるように
- すべての JSON を読み込んでいたら重い
    - v5.2, v5.1, v5.0, v4.2 それぞれ 300KB

---

# 複数バージョン対応 続き
- webpack dynamic imports
    - 1 つのバージョンの JSON だけを動的に読み込む
```js
const importMethods = version => {
  switch (version) {
    case 'v5.2':
      return import(
        /* webpackChunkName: "v5.2" */ 'v5.2.json'
       )
    case 'v5.1':
      return import(
        /* webpackChunkName: "v5.1" */ 'v5.1.json'
      )
  }
}
```

---

# 課題
- タブを開いてからコンテンツが表示されるまで 100~200 ミリ秒かかる
    - ネットワークリクエストが原因
    - `iframe` で表示する以外の実装が必要そう

---

# まとめ
- Rails のメソッド多すぎ問題
- 新しいタブでランダムにドキュメントを開いて学習するという解決策？
- コメントがないメソッドもあったりして、学習効果の程は微妙
    - 作る過程のほうが学習があったかも
    - 単純接触効果に期待（Rails をより好きになる？）
