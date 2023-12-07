---
created: 2023-12-07 11:11
tags: []
type: 
aliases: []
---

# state
状態を定義する．
なぜimutable ?
	変更の検知を正確にするため
## 状態とは
uiを作るのに必要なデータ
静的ではないもの
### 例
todoアプリ
	toodのリスト
youtubeのホーム画面
	おすすめの動画のリスト
## stateの宣言方法
```dart
@freezed
class CounterState with _$CounterState {
  factory CounterState({
    required int count,
  }) = _CounterState;
}
```
上のを書いたら
`fvm flutter pub run build_runner build`
でコードの自動生成を行う．
-> copyWithなどの便利な関数が使えるようになる
# viewmodel
主な機能
- 実際にstateに値を入れて保持をする．
- 要求に応じてstateを書き換える．　（ロジック)
	ex. todoの追加命令が来たので，stateを置き換えてlistに追加する．
	ex. 検索して結果をstateに入れる．

## viewmodelの宣言方法
```dart
class CounterVM extends _$CounterVM {
  @override
  CounterState build() async{ //build時に返す型がstateになる！
    return CounterState(
	    count: 0, //ここには初期値が入る
    );
  }

  void count() {
	  state = CounterState(count: state.count++)
  }
}
```
viewModelがCounterStateを管理する．
## viewmodelの関数を実行するには
`ref.read(プロバイダー名.notifier).関数名()`

## (基本的にstateを書き換える関数はbuilder関数の中で使用してはいけない)
# view
## consumer
viewmodelのstateにアクセスするには，Consumerを用いる．
`state = ref.watch(CounterVMProvieder)`
stateが変わったら再buildされる． (builder関数が再度実行)
```
Consumer(
	builder(context, ref, child){
		
	}
)
```

# アプリを作ってみる
## ゴール１
入力フォームの値をボタンが押されたら表示する．
## ゴール２
bearPlaceAPIを使って特定の画像サイズのクマの写真を見ることができるアプリを作る
## AsyncValue
### 3つの状態
3つの状態にはそれぞれ型があるが，それらは全てAsyncValueの子クラスでありAsyncValue型の変数に直接代入することができる．
AsyncValue型の変数には以下の3つのどれかが入ってる
#### AsyncLoading
ローディング状態を表す
#### AsyncData
処理が終わってstateが収納されている状態
#### AsyncError
エラーの時

外部へアクセスする時や重い処理をするときには，非同期処理を用いなければいけない．
	なぜ? -> 処理をしている最中は他の処理が動かなくなってしまうから．

