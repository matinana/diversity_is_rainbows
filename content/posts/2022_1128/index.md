---
title: "良いPRに含まれているもの"
date: 2022-11-28T00:38:08+09:00
draft: false
---

良い PR と、良くない PR が世の中にはあると思いますが、それを切り分けるものは「コード自体の品質」や「どういうスコープで切り分けているか」など、複数の要素が合わさって判断されていると考えます。

今日は「良い PR」の構成要素の一つ、コードやスコープではなく、「PR 自身の中身」がどうあると良い PR になるのだろうということを考えてみます。

# 良い PR とは？

> コードは理解しやすくなければいけない。
>
> - リーダブルコード -

名著、リーダブルコードで書かれているように、良いコードとは理解しやすいコードです。

それはコードというものは自身が書く時間よりも、他者に読まれる時間の方が長いから必然ではあります。

PR も同様だと考えています。他者にとって理解しやすい内容が網羅されている PR こそ良い PR だと考えています。

これは PR がコードと同じく下記の特性を持っているからです。

### 長期に渡って読まれる

なにか既存の実装を変更しようと思った際に、「ここの背景ってどうなっているんだろう？」と当時のチケットや slack を探しても、見つけるのが難しい場合があります。

ですが、PR であればコードから blame で一瞬で拾えます。

自分も今でも数年前の PR を読むことはザラにありますが、PR にコード以外の情報がないとコードをしっかり読む必要があり、色んなコストが発生してしまいます。

### 多くの人に読まれる

PR を読む対象は目の前のレビュアーだけではありません。

レビューする人が決まっていて、「この人なら詳しいから省略しても良い」は多くのケースで当てはまりません。

レビュー時点ではレビュアーがドメインの知識を持っているため詳細が省略可能であっても、未来の時点で他の人が参照する場合はそうだとは限らないからです。

---

上記のような特性がある以上、コードに気を使うエンジニアがコードを取りまとめる PR に気を使わないのは本末転倒です。

では、どんな PR だと理解されやすい PR になるでしょうか？

次項で自分が「あると良いなぁ」と思っている構成要素を上げてみようと思います！

# 具体的な PR の構成要素

## （その前に）対象の PR の性質

PR の情報をどこまで詳細に書くのかは、下記のような情報によって異なります。

- 機能のサービスレベル
- PR で扱うスコープの大きさ
- レビューからマージまでの優先度・緊急度
- レビュー相手の PR で取り扱う実装や仕様への理解度

ここでは、障害対応などでなく、ある程度スケジュールに余裕があり、粒度も極端に小さくなく、レビュアーもランダムな方にレビューする場合の PR の場合とします。

## 概要

### 1. 対応内容

まずは最初に何をしている PR なのかを簡潔に書きます。

背景や詳細な仕様ではなく「何をしているか」から書くことで、その後の情報に見るための指針ができます。

長めの文章になりがちな背景や外部リンクなどになりがちな詳細な仕様ページへの遷移があるとしても、何をしているかがわかれば、それらを読み解きやすくなると考えています。

### 2. 対応の背景

なぜ上記の対応が必要なのかの背景を書きます。

背景がないと、対応方法に違う解が生まれる可能性があります。

たとえば、「ハンバーグを作りました」という対応内容があるときに、背景が

- 高級レストランのコースの一品としてハンバーグを作りました。
- 自然災害の帰宅難民でまるまる一日ご飯を食べていなかった人に避難所でハンバーグを作りました。

上記では、ハンバーグの作成意図が大きく変わります。

極端な例でしたが、コードも背景によって対応内容が変わるので、対応の背景はしっかりあると良いと思います。

### 3. 仕様のドキュメント

仕様のドキュメントがあればそちらを記載します。

背景だけではわからない考慮すべき点が仕様にはまとまっていることがあるので、レビュー時にその考慮すべき点や、該当 PR 後の実装内容などに影響がないか等を把握する助けにもなります。

たとえば先の程のハンバーグの例ですが、ハンバーグのあとに鉄板焼とフォアグラと伊勢海老が出てくるとしたら、ハンバーグは小さめのほうがよいかもしれませんよね？笑

## 変更内容

### 4_A. 具体的に変更している点

こちらは実際に細かい変更内容なります。

対応内容が「高級料理店のコースのハンバーグを作る」だとしたら、その過程である下記などが変更内容にあたるでしょうか？（ハンバーグでの説明がわかりにくくなってきましたｗ）

- 具材の準備
- ハンバーグの調理
- ソースの作成
- 盛り付け

具体的な変更点を列挙する理由は、前情報としてテキストでスコープを切り分けてあげることでコードリーディングがしやすくなるからです。

### 4_B. 変更点の画像

ビューに変更がある場合は、変更している箇所の画像があるとイメージが湧きやすくなります。

4_A の内容に沿って話すと、

- ハンバーグで使う具材の写真
  - これがあるとどんな調理工程が必要になるのか、レシピ（コード）を読む前にイメージが湧きますよね
- ハンバーグの調理
  - 実際にできたハンバーグがあれば、大きさや形がわかります

ソースや盛り付けも同じです。

最初に話したように、「良い PR ＝理解しやすい PR」です。

百聞は一見にしかず。文字よりも画像で伝えられるものは画像で伝えるべきだと思っています。

（画像があることで PR の対応内容が何倍もわかりやすくなるので、私は PR のスコープが大きい場合は、1 の対応内容のところに主な変更点を書くこともあります。）

## 5. 動作確認

何より大切な項目の一つだと考えています。

動作確認はテスト項目と同じようなものだと個人的には思っているので、ハンバーグの作成であれば下記のような動作確認を自分なら書きます。

- 具材の準備が出来ていること
  - 合いびき肉があること
    - 250g~300g であること
  - たまねぎがあること
    - 200~250g のものが一つあること
  - パン粉があること
    - 大さじ 4 杯
  - …
- ハンバーグの調理が出来ていること
  - 玉ねぎをみじん切りにしていること
  - フライパンでたまねぎを炒めていること
    - サラダ油をおおさじ１入れていること
    - 中火であること
    - 8~9 分炒めて甘みが出ていること
  - ハンバーグの成形が出来ていること
    - たまねぎと合挽肉が混ざっていること
    - …

上記のようにテストを書くような形でなるべく細かく記載しています。

この意図としては下記になります。

- 細かく書くことで適正な動作確認ができて障害やデグレを防げる（動作確認として）
- 動作確認を読むと仕様がわかる(レビュアーが理解しやすいように)
- 仕様がわかるように書くと漏れに気づく（実装時＆セルフレビュー時のチェックとして）

動作確認欄を適当に書く人も結構たくさんいる気がしますが（笑）個人的には上記複数の目線で有益な情報になると考えているのでとても大切にしています。

---

以上、ハンバーグを例に色々と書いてみました…笑

PR の観点では具体的な本文の内容だけでなく、下記の情報もとても大切だと思っています！

- セルフレビュー
- 適切な粒度のコミット
- PR に閉じ込める粒度

上の話はそれぞれで一記事かけちゃうと思うので、また別の機会に記事にしてみますね！

PR の情報は結構人によってマチマチだと思いますが、コードは同じでも、レビュアーの負担を大きく減らせる PR は絶対にあると思っているので、ぜひみなさんが思うよい PR に関して、教えてくださいね！

それではみなさん、Happy Hacking!
