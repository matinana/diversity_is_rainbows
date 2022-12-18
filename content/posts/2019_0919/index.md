---
title: "React Native(Expo)で画像をFirebase StorageとFirestoreで管理する"
date: 2019-09-19T22:43:58+09:00
draft: false
---

# 本記事について(ゴール）

こんにちは matinana です。技術記事人生初執筆です。宜しくお願いします。

本記事では、下記 GIF のように React Native(Expo)を用いてフォトライブラリの画像を Firebase Storage に保存し、ストレージのダウンロード URL と画像のキャッチコピーを Firestore で管理するまでの一連の実装をしてみます。

![](https://paper-attachments.dropbox.com/s_8AA2D39648B42347FD356E63E7DE527736C77DFC141C11EEDA3D647BDB881D28_1568201104187_topdemo2.gif)

---

# はじめに

## 主なフレームワーク等のバージョンについて

2019 年 9 月 11 日現在最新（SDK34)の Expo を使っています。
"expo": "^34.0.1",
"react": "16.8.3",
"firebase": "^6.4.0",

## ソースコードの記述について

この記事では要所のみを説明しています。そのため、記事内のソースコードは全文ではありません。実際に手を動かしてみる場合は、こちらのソースコードをコピーして使うというよりも、<a href="https://github.com/matinana/expoFirebase" target="_blank" rel="noopener noreferrer">Github</a>の方を参照お願いします。

---

# 実装の流れ

1. コンポーネントを作る
2. imagePicker を使ってフォトライブラリの画像を取得する
3. Firebase をプロジェクトに導入する
4. フォトライブラリから取得した画像を Firebese Storage に保存し、保存した画像のダウンロード URL を Firestore と紐付ける

---

# 1. コンポーネントを作る

まずは外観を作るところから始めましょう。
と、言っても複雑な見た目は作っておらず、要素は３つのみです。

・PostScreen.js: モーダルを表示するボタンと追加された投稿を一覧で表示するスクリーン
・AddPostModa.js: 写真を追加するための登録フォーム（モーダル）
・Post.js: それぞれの投稿のリスト

```javascript:screens/PostScreen.js
import React from 'react';
import {
  StyleSheet,
  View,
  Dimensions,
  KeyboardAvoidingView,
} from 'react-native';
import { Button } from 'react-native-elements';
import Modal from 'react-native-modal';
import Post from '../components/Post';
import AddPostModal from '../components/AddPostModal';

//　略

render() {
    return (
      <View style={styles.container}>
        {/* モーダルを表示するボタン */}
        <Button
          containerStyle={styles.addPostBtnContainer}
          buttonStyle={styles.addPostBtn}
          title="写真を追加"
          onPress={this.togglePostModal}
        />
        {/* モーダル */}
        <Modal
          isVisible={this.state.isAddPostModalVisible}
          deviceWidth={deviceWidth}
          deviceHeight={deviceHeight}
          animationIn="fadeIn"
          animationInTiming={300}
          style={styles.modal}
        >
          <Button
            buttonStyle={styles.cancelBtn}
            titleStyle={styles.cancelBtnText}
            title="キャンセル"
            onPress={this.togglePostModal}
          />
          <KeyboardAvoidingView behavior="padding" keyboardVerticalOffset={120}>
            <AddPostModal
              updateAddedPostState={this.updateAddedPostState}
              togglePostModal={this.togglePostModal}
            />
          </KeyboardAvoidingView>
        </Modal>
        {/* 投稿写真を表示するリスト */}
        <Post allPosts={this.state.allPosts} />
      </View>
    );
  }
}
```

### <a href="https://react-native-training.github.io/react-native-elements" target="_blank" rel="noopener noreferrer">react-native-elements</a>

モーダルを表示するボタンに関しては、<a href="https://react-native-training.github.io/react-native-elements" target="_blank" rel="noopener noreferrer">react-native-elements</a>を使っています。
ボタン、アイコン、リストなど使いやすいエレメントが用意されています。

![](https://paper-attachments.dropbox.com/s_8AA2D39648B42347FD356E63E7DE527736C77DFC141C11EEDA3D647BDB881D28_1568180768271_+2019-09-11+14.45.46.png)

### <a href="https://github.com/react-native-community/react-native-modal" target="_blank" rel="noopener noreferrer">react-native-modal</a>

モーダルには<a href="https://github.com/react-native-community/react-native-modal" target="_blank" rel="noopener noreferrer">react-native-modal</a>を使っています。
react native 標準のモーダルよりもカスタマイズがしやすく使いやすいです。

![](https://paper-attachments.dropbox.com/s_8AA2D39648B42347FD356E63E7DE527736C77DFC141C11EEDA3D647BDB881D28_1567402741357_modal.gif)

```javascript:components/AddPostModal.js
import React from 'react';
import {
  StyleSheet,
  View,
  Alert,
  TouchableOpacity,
  Image,
  Dimensions,
} from 'react-native';
import { Button, Icon } from 'react-native-elements';
import { Fumi } from 'react-native-textinput-effects';
import { FontAwesome } from '@expo/vector-icons';

// 略

render() {
    return (
      <View style={styles.container}>
        <TouchableOpacity
          style={styles.imageContainer}
          onPress={() => this.onAddImagePressed()}
        >
          {this.state.imgUrl ? (
            <Image style={styles.image} source={{ uri: this.state.imgUrl }} />
          ) : (
            <Icon
              name="camera-retro"
              type="font-awesome"
              size={50}
              containerStyle={styles.cameraIcon}
              color="gray"
            />
          )}
        </TouchableOpacity>
        {/* react-native-textinput-effectsでテキストインプットにアニメーションを実装 */}
        <Fumi
          label={'キャッチコピーをつけてね'}
          iconClass={FontAwesome}
          iconName={'hashtag'}
          iconColor={'#f4d29a'}
          inputPadding={16}
          inputStyle={{ color: '#444' }}
          labelStyle={{ color: '#ddd' }}
          style={styles.textContainer}
          onChangeText={phrase => this.setState({ phrase })}
          value={this.state.phrase}
        />
        <Button
          buttonStyle={[
            styles.addPostBtn,
            { display: this.state.imgUrl ? 'flex' : 'none' },
          ]}
          title="追加"
          onPress={() => {
            if (this.state.phrase.length >= 1) {
              this.onPressAdd();
            } else {
              Alert.alert('キャッチコピーを入力してね', '');
            }
          }}
        />
      </View>
    );
  }
}
```

### <a href="https://github.com/halilb/react-native-textinput-effects" target="_blank" rel="noopener noreferrer">react-native-textinput-effects</a>

登録フォームのテキストインプットでは<a href="https://github.com/halilb/react-native-textinput-effects" target="_blank" rel="noopener noreferrer">react-native-textinput-effects</a>というテキストインプットにおしゃれなアニメーションをつけられるライブラリを使っています。
数種類のアニメーションがあるのでぜひライブラリのページを見てください。

![](https://paper-attachments.dropbox.com/s_8AA2D39648B42347FD356E63E7DE527736C77DFC141C11EEDA3D647BDB881D28_1567402556186_textinput.gif)

##

```javascript:components/Post.js
import React from 'react';
import { Text, Image, StyleSheet, Dimensions, FlatList } from 'react-native';
class Post extends React.Component {
  renderPost({ item }) {
    return (
      <React.Fragment>
        <Image style={styles.image} source={{ uri: item.imgUrl }} />
        <Text style={styles.phrase}>#{item.phrase}</Text>
      </React.Fragment>
    );
  }
  render() {
    return (
      <FlatList
        contentContainerStyle={{ alignItems: 'center' }}
        // 上から投稿順に表示
        data={[...this.props.allPosts].sort(
          (a, b) => b.postIndex - a.postIndex,
        )}
        keyExtractor={item => item.postIndex}
        renderItem={this.renderPost}
      />
    );
  }
}
```

Post.js はシンプルに Flatlist で投稿を表示しているだけです。
React.Fragment に関してご存知ない方は<a href="https://qiita.com/kaba/items/b681ffe3412a9af32f92" target="_blank" rel="noopener noreferrer">こちらの記事</a>を参照してください。Qiita で ReactNative・Expo 関連の記事を多く執筆してくださっている<a href="https://qiita.com/kaba" target="_blank" rel="noopener noreferrer">@kaba</a>さんの記事です。

---

# 2. imagePicker を使ってフォトライブラリの画像を取得する

Expo の imagePicker を使ってイメージライブラリから画像を引っ張ってきます。
その後、ImageManipulator を使って画像をリサイズしています。

```javascript:components/addPostModal.js
// Permissionsなどのimportの書き方がExpoの最新バージョンでは以前と異なっているので要確認
// SDK32までは下記のようにExpoから直接読み込めた
// import { Permissions, ImagePicker, ImageManipulator } from 'expo'　

// SDK33からはそれぞれ個別に読みこむようになっている
import * as Permissions from 'expo-permissions';
import * as ImagePicker from 'expo-image-picker';
import * as ImageManipulator from 'expo-image-manipulator';

class AddPostModal extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      imgUrl: '',
      phrase: '',
      addedPost: [],
    };
  }

onAddImagePressed = async () => {
    const { status } = await Permissions.askAsync(Permissions.CAMERA_ROLL);
    if (status === 'granted') {
      const result = await ImagePicker.launchImageLibraryAsync({
        allowsEditing: true,
        mediaTypes: ImagePicker.MediaTypeOptions.Images,
      });
      if (!result.cancelled) {
        // ImageManipulatorでリサイズ処理
        const actions = [];
        actions.push({ resize: { width: 350 } });
        const manipulatorResult = await ImageManipulator.manipulateAsync(
          result.uri,
          actions,
          {
            compress: 0.4,
          },
        );
        this.setState({
          imgUrl: manipulatorResult.uri,
        });
      }
    }
  };
```

---

# 3. Firebase をプロジェクトに導入する

<a href="https://console.firebase.google.com/" target="_blank" rel="noopener noreferrer">Firebase</a>でグーグルログインした後に、「プロジェクトを追加」から新規プロジェクトを立ち上げます。プロジェクトの名前と Google アナリティクスの設定（今回は無効に変更）の２つだけの設定でプロジェクトが作成できます。

今回はデータベースとして Firestore を利用し、画像は Storage に保存するため、それらの設定も含め以下の４つの設定を Firebase で行います。
3-1. Firebase をアプリに追加する
3-2. 匿名認証の設定をする
3-3. Firestore の設定をする
3-4. Storage の設定をする

## 3-1. Firebase をアプリに追加する

![](https://paper-attachments.dropbox.com/s_8AA2D39648B42347FD356E63E7DE527736C77DFC141C11EEDA3D647BDB881D28_1568167659794_+2019-09-11+10.36.04.png)

まずは上記の Project Overview からオレンジの丸で囲った「ウェブアプリへの追加」を選択し、アプリのニックネームを入力しアプリを登録します。
この際「このアプリを Firebase Hosting も設定します」という項目もありますが、今回はチェックを入れず無視して大丈夫です。

![](https://paper-attachments.dropbox.com/s_8AA2D39648B42347FD356E63E7DE527736C77DFC141C11EEDA3D647BDB881D28_1567405425798_+2019-09-02+15.jpg)

アプリを登録すると上記のような firebaseConfig の情報が表示されます。
こちらは後ほど使うのでどこかにコピペしておいてください。
現在はまだ storageBucket の部分が空欄だと思いますが、後ほど Storage の設定を行った際に追記します。

## 3-2 匿名認証の設定をする

左のタブから Authentication を選択し「ログイン方法を設定」を選びます。
ログインプロバイダの「匿名」を選択し「有効にする」にチェックを入れて保存します。

![](https://paper-attachments.dropbox.com/s_8AA2D39648B42347FD356E63E7DE527736C77DFC141C11EEDA3D647BDB881D28_1568168691985_+2019-09-11+10.39.14.png)

## 3-3. Firestore の設定をする

左のタブから Database を選択。

![](https://paper-attachments.dropbox.com/s_8AA2D39648B42347FD356E63E7DE527736C77DFC141C11EEDA3D647BDB881D28_1568168817118_+2019-09-11+10.39.55.png)

上記のデータベースの作成を選択。
セキュリティに関する選択肢が出てくるので、今回は「テストモードで開始」を選択。
Cloud Firestore のロケーションを選択する画面に切り替わるので、ロケーションを「asia-northeast1」に選択して完了。 \*ロケーションに関して詳しく知りたい方は<a href="https://cloud.google.com/firestore/docs/locations?hl=ja" target="_blank" rel="noopener noreferrer">公式サイトのドキュメント</a>をお読みください。

## 3-4. Storage の設定をする

左のタブから Storage を選択。
「スタートガイド」を選択するとセキュリティの画面が表示されるのでそのまま「次へ」→「完了」。

![](https://paper-attachments.dropbox.com/s_8AA2D39648B42347FD356E63E7DE527736C77DFC141C11EEDA3D647BDB881D28_1568169554167_+2019-09-11+11.jpg)

上の画面に遷移すると思いますので、下記オレンジの丸で囲った部分の情報を先程メモした firebaseConfig の storageBucket へ記述してください。

これで Firebase 側での一連の設定は終わりになります。

---

# 4. フォトライブラリから取得した画像を Firebese Storage に保存し、保存した画像のダウンロード URL を Firestore と紐付ける

まずはプロジェクトに Firebase をインストールしましょう。

    $ npm install firebase

Firebase 関連のコードは Fire.js にまとめて記述する形式にします。
uploadPost 以外の実装に関しては<a href="https://tech.maricuru.com/entry/2018/08/03/180543" target="_blank" rel="noopener noreferrer">こちらのサイト</a>を参考にしているので、こちらを読んでいただいた方がよいかと思います。（<a href="https://tech.maricuru.com/" target="_blank" rel="noopener noreferrer">maricuru</a>さん大変参考になりました。ありがとうございます。）

```javascript:utils/Fire.js
import firebase from 'firebase';
import 'firebase/firestore';
class Fire {
  constructor() {
    // Githubのコードではconfig.jsとして別ファイルから読み込んでいます
    firebase.initializeApp({
      // ここに先程メモしたFirebaseConfigの内容を記述
      apiKey: '***',
      authDomain: '***',
      databaseURL: '***',
      projectId: '***',
      storageBucket: '***',
      messagingSenderId: '***',
      appId: '***',
  });
    // 匿名認証
    firebase.auth().onAuthStateChanged(user => {
      if (!user) {
        firebase.auth().signInAnonymously();
      }
    });
  }

  // 投稿時の処理
  uploadPost = async ({ url, phrase, postIndex }) => {
    const uploadRef = await this.postCollection.doc(postIndex);
    uploadRef
      .set({
        imgUrl: url,
        phrase,
        postIndex,
      })
      .then(() => {
        console.log('書き込みができました');
      });
  };

  // Firestoreに保存した情報をPostScreenで取得し、setStateする際の処理
  getPosts = async () => {
    const querySnapshot = await this.postCollection.get();
    const res = [];
    querySnapshot.forEach(doc => {
      res.push(doc.data());
    });
    return res;
  };

  get userCollection() {
    return firebase.firestore().collection('users');
  }
  get postCollection() {
    return this.userCollection.doc(this.uid).collection('posts');
  }
  get uid() {
    return (firebase.auth().currentUser || {}).uid;
  }
}
Fire.shared = new Fire();
export default Fire;
```

各コンポーネントのファイルでも Firebase 関連の記述をします。
ストレージへの画像の保存は公式ドキュメントの<a href="https://firebase.google.com/docs/storage/web/upload-files?hl=ja" target="_blank" rel="noopener noreferrer">こちら</a>を、Firestore へのデータの追加は<a href="https://firebase.google.com/docs/firestore/manage-data/add-data?hl=ja" target="_blank" rel="noopener noreferrer">こちら</a>を参照しながら実装しています。

```javascript:components/addPostModal.js
import firebase from 'firebase';
import Fire from '../utils/Fire';
class AddPostModal extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      imgUrl: '',
      phrase: '',
      addedPost: [],
    };
  }
  // 投稿時の処理
  async onPressAdd() {
    await this.uploadPostImg();
    const { imgUrl, phrase, postIndex } = await this.state;
    this.uploadPost(imgUrl, phrase, postIndex);
    this.setState(
      {
        addedPost: [
          {
            imgUrl,
            phrase,
            postIndex,
          },
        ],
      },
      () => this.updateAddedPostState(),
      this.props.togglePostModal(),
    );
  }

  onAddImagePressed = async () => {
    // 略
  };

  uploadPostImg = async () => {
    const metadata = {
      contentType: 'image/jpeg',
    };
    const postIndex = Date.now().toString();
    const storage = firebase.storage();
    const imgURI = this.state.imgUrl;
    const response = await fetch(imgURI);
    const blob = await response.blob();
    const uploadRef = storage.ref('images').child(`${postIndex}`);

    // storageに画像を保存
    await uploadRef.put(blob, metadata).catch(() => {
      alert('画像の保存に失敗しました');
    });

    // storageのダウンロードURLをsetStateする
    await uploadRef
      .getDownloadURL()
      .then(url => {
        this.setState({
          imgUrl: url,
          postIndex,
        });
      })
      .catch(() => {
        alert('失敗しました');
      });
  };

  // stateに入っているダウンロードURLなどをFirestoreに記述する
  uploadPost(url, phrase, postIndex) {
    Fire.shared.uploadPost({
      url,
      phrase,
      postIndex,
    });
  }

  // PostScreen.jsで投稿データのstateを管理する
  updateAddedPostState() {
    this.props.updateAddedPostState(this.state.addedPost);
  }

  render() {
    // 略
  }
}
```

```javascript:screens/PostScreen.js
import firebase from 'firebase';
import Fire from '../utils/Fire';
import Post from '../components/Post';
import AddPostModal from '../components/AddPostModal';

class PostsScreen extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      allPosts: [],
      isAddPostModalVisible: false,
    };
    //  Firestoreのデータを読みこむ
    this.downloadAllPosts();
  }

  // addPostModal.jsで追加した投稿をchildStateとして受け取り、PostScreenで全投稿を管理する
  updateAddedPostState = childState => {
    this.setState({
      allPosts: [...this.state.allPosts, ...childState],
    });
  };

  async downloadAllPosts() {
    firebase.auth().onAuthStateChanged(async user => {
      if (user) {
        const posts = await Fire.shared.getPosts();
        this.setState({
          allPosts: posts,
        });
      }
    });
  }

  render() {
  // 略
  }
}
```

## 一連の流れの確認

1. モーダルで画像とキャッチコピーを入力し追加ボタンを押すと onPressAdd()が始まる
2. onPressAdd()で以下の処理が走る
   - uploadPostImg()の処理で Firebase Storage への画像のアップロードが完了し、その後画像のダウンロード URL が addPostModal の state に書き込まれる
   - uploadPost()の処理で state の情報が Firestore に書き込まれる
   - updateAddedPostState()で addPostModal で投稿した情報を PostScreen に集約し、Post に流す
3. 投稿が Post.js の FlatList で描画される

以後 Firestore に投稿内容がある場合は、downloadAllPosts()で以前 Firestore に追加した情報が PostScreen に setState されることで、アプリを再起動した時などでも追加したデータが画面に描写されることになっています。

---

# 最後に

今回は削除機能などを実装しなかったため、以上で実装は終わりになります。
上記の実装はとりあえず動く形での実装になっており、改善点等もあるかと思います。
Expo、React Native に関する情報はまだまだネット上で多くはないため、動く実装の一つとして記事投稿いたしました。もし改善案などありましたらご提案頂ければ幸いです。

Happy Hacking! ＼(^o^)／
