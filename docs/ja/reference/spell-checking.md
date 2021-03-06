# 辞書とスペルチェック

他のモダンなライティングアプリケーションと同様にZettlrは、強力なスペルチェックエンジンを内蔵しています。実際に書いていくプロセスとスペルチェックとを分けておきたいと考える人も多いため、この機能はデフォルトで無効化されています。しかし、有効化するのは簡単です。さらに、Zettlrでは複数の言語を同時にチェックすることも可能です。これはバイリンガルな文章を書いている場合に便利です。(例えば、英語のジャーナル論文にドイツ語の引用を含んでいる場合など。)

## スペルチェックを有効化する

スペルチェックを有効化するには、メニュー項目かツールバーのボタンを押す、もしくは`Cmd+Ctrl+,`のショートカットで設定ダイアログを開きます。そして、エディタタブに移動し、有効にしたい辞書をすべて選択します。左側は選択可能な辞書の一覧です。クリックしたものは右側に移動し、有効化された辞書は緑色の帯で示されます。緑色の帯をクリックすると辞書を無効化することができます。

上部のテキスト欄に検索語句を入力することで、選択可能な辞書の一覧を絞り込むこともできます。入力した検索語句をその名前に含まない辞書は、自動的に非表示になります。検索欄の文字をすべて削除すると、再びすべての辞書が表示されます。

スペルチェックの設定を保存すると、すべての辞書の読み込みが自動的に開始されます。辞書の準備が完了するまでに少し時間が掛かる可能性があります。特に、イタリア語やドイツ語などのような巨大な辞書の場合は少し時間が掛かります。アプリケーションを終了して次回以降のZettlrの起動には、辞書の読み込みのために少し時間が掛かります。

## スペルチェックを無効化する

スペルチェックを再び無効化するには、設定ダイアログのエディタタブの右側にある緑の帯をクリックし、すべての辞書を無効化します。一つも有効化されていなければ、文書に対するスペルチェックは行われなくなります。

## カスタム辞書

バージョン`1.3.0`以降では、ユーザ定義辞書に単語を追加する機能がサポートされました。特に人名などについて、それらを正しいものとしてマークし、名前の下に表示される赤い下線を取り除くことができるので便利です。単語をユーザ辞書に追加するには、マークされている単語を右クリックし「辞書に追加」を選択します。それ以降は、その単語がスペルミスとマークされることはなくなります。

## 新しい辞書を追加する

Zettlrにはいくつかの辞書が内蔵されていますが、自分が使用する言語の辞書を追加したい場合があるかもしれません。辞書を追加するには、hunspell形式の辞書を用意する必要があります。一般的には、フォルダ内に2つのファイル(`.dic`形式と`.aff`形式)が含まれた形をしています。`.dic`ファイルには、すべての単語と接辞(例えば"end"という語幹に対して"ends"や"ending"などのように、異なる活用語尾を取り得る単語が存在することを、アルゴリズムに伝えるためのフラグ)を含みます。接辞フラグは「この単語は活用語尾が`-ing`になることがある」ということを表すものです。そして、`.aff`ファイルには、接辞フラグの定義が書かれています。

これらの辞書を追加するには、オンラインで検索を行ってください。まずは、多くの言語を揃えている[GitHubユーザーwooormのリポジトリ](https://github.com/wooorm/dictionaries)を見てみると良いでしょう。いずれかのフォルダーをダウンロードします。それから、Zettlrで ファイル -> 「辞書をインポート...」の順にクリックすると、Zettlrの`dict`フォルダーが、コンピュータのファイルブラウザで表示されます。ダウンロードした辞書フォルダを丸ごと`dict`フォルダ内にコピーしてください。これで、設定画面で辞書が選択できるようになります。

Zettlrは辞書が有効なものであるかどうかを確認する簡単なテストを行います。辞書が設定画面に表示されて選択可能になるためには以下のルールに従っている必要があります。

1. `.dic`ファイルと`.aff`ファイルを格納したフォルダの名前は、その辞書に含まれる言語に対応する[BCP-47タグ](https://tools.ietf.org/html/bcp47)になっている必要があります。「BCP-47」というのは聞き覚えがないかもしれませんが、単なる一般的な言語タグのことです。例えば、ドイツ語の辞書なら`de-DE`(ドイツのドイツ語)や`de-CH`(スイスのドイツ語)、または単にイタリア語の辞書として`it`などです。[利用可能なすべての言語の一覧はこちらです](https://www.iana.org/assignments/language-subtag-registry/language-subtag-registry)。
2. フォルダ内には少なくとも`.dic`ファイルと`.aff`ファイルの2つが必要です。ファイル名はフォルダと同じBCP-47タグ名を付けるか、もしくは`index.dic`/`index.aff`とします。
3. 辞書フォルダには、他にも作成者リストやライセンスなどのファイルを含むことができますが、これらは無視されます。

まとめると、Zettlrでは辞書が有効かどうかを調べるために、次のようなパスの存在をチェックします。

- `bcp-47/bcp-47.dic` と `bcp-47/bcp-47.aff` _もしくは_
- `bcp-47/index.dic` と `bcp-47/index.aff`

> 注意: JavaScriptで実装されたhunspellのアルゴリズムは、現時点ではLibreOfficeで使われているメインのアルゴリズムほど強力ではないので、大きすぎる辞書は読み込めない可能性があります。辞書の読み込み後、Zettlrが固まってしまった場合は、アプリケーションを強制終了してから辞書の選択を解除してください。
