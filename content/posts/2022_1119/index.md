---
title: "リモートワークファシリテーションのまなび"
date: 2022-11-19T20:03:51+09:00
draft: false
---

関わっていたサービスが MBO を行い、半年ほど前からサービスラインの異なる新しいサービスに関わるようになりました。

そのチームでは当時開発チームの課題に関してメンバー全員で話す場がなく、「えいや！」と会議体を設計して、MTG のファシリテーションを行っています。

半年を経て、話される議題も「ベストプラクティスが整っているような問い」や「やってみることのリスクが少ないチャレンジ」から、複数の可能性や実行する際のメリデメをしっかりと把握しなければならない課題の比重が少しずつ増えてきています。

また、チームメンバーの数も増えてきて、結果としてチーム内の技術やサービスに関する理解の深度もバラバラになってきたため、ファシリテーションの難しさを感じることも多くなりました。

そんな折に fukabori.fm の[ファシリテーション会](https://fukabori.fm/episode/77)を聞き直し、またそこで紹介されていた[ファシリテーションに関するスライド](https://blog.copilot.jp/entry/remote-facilitation)を読んだところ学びや発見が多くあったので、ログがてら気になった点をまとめてみようと思います。

（文章内の引用は podcast の発言か、podcast 内で参考にあげらている COPILOT 社の「[リモートワークにおけるファシリテーションの方法論](https://docs.google.com/presentation/d/1dQgbxB6_0kosazzgfk0Gmoa8c7dInfOy_NvZejfpneo/edit#slide=id.g814b63fdb8_0_12)」から引用させて頂いています。）

## まなび

### 1. ファシリテーションとは、会議の進行役や運営役ではない。そのため関係するメンバー全員で行っていくべきものである

> より良いゴールを達成するために必要な一連の行為が適切に行われる状態をつくること
>
> -[本資料における「ファシリテーションの定義」](https://docs.google.com/presentation/d/1dQgbxB6_0kosazzgfk0Gmoa8c7dInfOy_NvZejfpneo/edit#slide=id.g852dd5b07b_2_564)-

- なぜ会議をするのか？
- そのためにファシリテーションがなぜ必要なのか？

上記の問いを改めて考えるとこの考えは当たり前かもしれないのですが、毎回進行役や事前のドキュメント準備などを進めているとどうしても「ファシリテーターと参加者」のように境界を作って考えがちになります。

役割の違いはあれど、そこに負荷・権限・責務の違いはなく、参加メンバー全員で協力して創造的な場を作るという捉え方はとても大切だと再認識しました。

### 2. 小分けできる MTG は小分けにする

> コミュニケーションがずれるリスクを軽減するために、MTG を小分けにしましょう。
>
> これまで週 1 回・2 時間の MTG を開催していた場合に、リモートワーク環境ではそれを 1 時間ずつ・週 2 回の MTG に小分けにすると、メンバー間での認識のズレは少なくなります
>
> -[コミュニケーションを小分けにする](https://docs.google.com/presentation/d/1dQgbxB6_0kosazzgfk0Gmoa8c7dInfOy_NvZejfpneo/edit#slide=id.g838bc711eb_0_164)-

リモートになってさらっと話す場の設定が難しくなり、MTG で詰めたいことを事前に洗い出して、一つの MTG で複数の論点に関して話すことが多くなったと感じています。

「あー今日も MTG が予定よりも長引いてしまった…」という状態が常態化していたのですが、長時間かけて複数の論点を話す場を作るのではなく、短時間で一つの論点を話す場にすることで、下記のメリットがあるのだなと改めて感じました。

#### ■ 議論したい論点を一つに絞ることで事前のゴール設計が容易になり、論点の散らばりを減らせる。

議論の前には前提やゴール設計が重要になりますが、論点が多くなれば多くなるほどここの設計や、参加者のインプットが複雑になります。

また論点が複数になることで、「あれ、今まで話してたのって xx だよね？いつの間にか〇〇の話になっていない？」と、MTG で話されるべき論点が散らばってしまって意見の収束も難しくなります。

上記の課題は、論点を絞ることで一定解決できるのだと考えています。

#### ■ 時間が生み出す環境の変化に対する不確実性を減らすことができる

週一の定例を設けて、PJ の進捗を話すような場合、一週間分の進捗や課題を 1 時間かけて確認する MTG になります。ここには多くの変化を踏まえた振り返りや、次の一週間全体という長い期間の意思決定を行う必要性があります。

しかし、それを火曜日と金曜日に 30 分ずつ確認する MTG にした場合、より少ない変化や、現在の状況に沿った意思決定ができ、またその意思決定の範囲も短くなることで不確実性をコントロールしやすくなります。

### 3. 事前に完了の定義を決めておく

> （事前の準備で必ず必要なのは）完了の定義ですね
>
> -podcast 内の発言より-

事前準備が大切ということは理解していましたが、MTG のアジェンダには下記 3 種類があり、それぞれのアジェンダがどの種類のものなのか、またその上で何が成果として出れば完了なのかを決めておくというのは真新しい考え方でした。

#### ■ アジェンダの種類

- 発散のアジェンダ
  - 参加者の意見を吸い上げるためのアジェンダ。
  - ブレストのようなもので、なにかの決定を伴わなくても考えを広げたり、深めたりすることが完了の定義になる。
- 収束のアジェンダ
  - 意思決定を行うアジェンダ。
  - 多くの意見を出すことではなく、しっかりネクストアクションにたどりつくことが完了の定義になる。
- 共有のアジェンダ
  - 認識合わせのアジェンダ
  - メンバーに共有したいことや認識をすりあわせたいことを話す。伝えることが完了の定義になる。

個人的にはどんなアジェンダに対してもネクストアクションに繋げようとファシリテーションすることが多かったのですが、ここの色の違いを認識して、その会議体では「ここまででも良い」という線を引くことが、「コストのかかりすぎる収束」という結果を防ぐために重要だなと感じました。

### 4. 合意は難しいという諦めのスタンスを持った上で意思決定を行う

「チームの課題を話そう」という場であると、どうしてもチームメンバー全員の合意がないと意見の収束が行いにくい気がしてしまいます。

しかし、複数人での完全な合意を得ることはコストが異常に高いですし、そもそもそれが必要なケースでない場合もあります。

そのため下記のような「決め方の決め方を決めておく」ことで意思決定をすすめることが大切だと感じました。

- その場でどの程度の合意が必要なのか？を決めておく
  - 多数決で良いとする
  - 反対がなければよいとする

また、反対意見は出しにくいという懸念に対しては、「一人一個違和感を書いてもらう」のような tips を用いて、「それぐらいの違和感ですんでるという安心感を共有する」ことで意思決定を進めてしまうこともコツの一つのようです。

### 5. 議事録の粒度を決める

基本的に議事録は自身でファシリテーションをしながらなるべく全部取るというスタイルでいつも行ってきました。（勿論協力してくれる人もいます。）

しかし、個人で議事録を取ると、個々人の認識の相違による誤ったログが生まれることや、複数タスクによる認知コストが大きくなり、ファシリテーションか議事録のどちらかの行為の質が下がることがあります。

それでも議事録を取るのは難しい行為なので、参加者それぞれにお願いするのは難しいと感じていました。

> 必要な程度の粒度でよいというラインを確認する
>
> - podcast 内の発言より-

前述の課題の解決策として、上記が話されていて、なるほど〜とおもいました。

「議事録を取る」という行為の難しさは、そもそも「なにを、どの程度書くのか」という「議事録の指針」がないため、書く人によって内容が変わり、結果としていつも同じ人が取る（方が安心するため書く人と書かない人に別れてしまう）という課題が生まれていました。

そのため、チームや会議体によってその指針を事前に決めてしまうこと。たとえば「質問とそれに対する答えのみを書く。発言者の名前や答えに至るまでの議論は省いても良い」など、具体的な指針を作ることで、誰もが書きやすい土壌を作るということですね。

---

以上です！

以前から、特定の podcast や一つのアウトプットをもとに学びを整理するブログを書いてみたいと思っていましたが、学びめちゃくちゃありました…！

ファシリテーションを制するものは、リモートワークを制する、ひいてはプロダクト開発を制するだと改めて思ったので、明日からのファシリテーションに活かしたいと思います。

---

## 参考リンク

[fukabori.fm 「77.リモートワークにおけるファシリテーション(前編)」](https://open.spotify.com/episode/7kxQEuKXVTlWDO2A76L4yT?si=G4YIPsYwS3aK7sF5ssSonA)

[リモートワークにおけるファシリテーションの方法論を公開します！](https://blog.copilot.jp/entry/remote-facilitation)
