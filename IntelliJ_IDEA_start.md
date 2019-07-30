<!-- TITLE: IntelliJ IDEA が起動しなくて困ったら -->
<!-- TAGS: IntelliJ IntelliJIdea -->

ある日突然 IntelliJ IDEA が起動しなくなって困ることはありませんか？
私はありました。
うまく解決できるかもしれない方法を教えてもらったので、ここで共有します。

### 現象

- 操作: Dock にある IntelliJ IDEA のアイコンをクリックします。
  - これ→<img width="126" alt="スクリーンショット 2019-07-30 18.13.19.png" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/415135/b381c78f-b9e1-423f-57bb-8d497a7718ab.png">
- 期待: IntelliJ IDEAが起動。すごいプログラムをバリバリ書けるようになってほしいです。
- 実際: アイコンがピコピコ動くだけ。IntelliJ IDEA のプロセスすら立ち上がりません。

#### 環境

- MacOS Mojave 10.14.5
- IntelliJ IDEA ULTIMATE 2019.2

### 解決方法

以下のようにターミナルから直接 IntelliJ IDEA を起動すると、起動に失敗した理由を標準出力に表示してくれます。
標準エラー出力かも。

```bash
$ /Applications/IntelliJ IDEA.app/Contents/MacOS/idea
```

私の場合は以下のようなメッセージが出ました。

```
2019-07-30 16:40:50.078 idea[30387:314035] allVms required 1.8*,1.8+
2019-07-30 16:40:50.081 idea[30387:314042] Value of IDEA_VM_OPTIONS is (null)
2019-07-30 16:40:50.081 idea[30387:314042] Processing VMOptions file at /Users/ta.yamashita/Library/Preferences/IntelliJIdea2019.2/idea.vmoptions
2019-07-30 16:40:50.081 idea[30387:314042] Done
OpenJDK 64-Bit Server VM warning: Option UseConcMarkSweepGC was deprecated in version 9.0 and will likely be removed in a future release.
Error occurred during initialization of VM
Initial heap size set to a larger value than the maximum heap size
```

要するに、 `/Users/ta.yamashita/Library/Preferences/IntelliJIdea2019.2/idea.vmoptions` の読み込みがうまくできてなくて、その理由は `Initial heap size` が `maximum heap size` よりデカいせいらしいです。

当該のファイルには

```
-Xms4096m
-Xmx2048m
```

のような記述があったので、最大のヒープサイズを表す `-Xmx` の方を `4096m` に修正しました。
元々そうしていたつもりなんですけどね。

結果、アイコンをクリックして IntelliJ IDEA を起動することができるようになりました。
めでたし、めでたし。
