# MarkovAlgorithmOnTeX

`markovalgorithm.tex` を読み込むことでマルコフアルゴリズムを TeX で扱えるようにします。`\MarkovAlgorithmCode` で置換規則を定義し、`\MarkovAlgorithmInput` で入力した文字列に定義した置換規則を適用します。それぞれの構文は以下の通りです。
```tex
\MarkovAlgorithmCode[<label>]{<code>}
\MarkovAlgorithmInput[<label>]{<text>}
```
`<label>` は省略可能です。異なる置換規則に異なるラベルを付けておくことで、`\MarkovAlgorithmInput` 使用時にラベルに対応した置換規則を選択できます。

`<code>` の構文は以下の通りです。
```tex
<pattern>:<text>
```
または
```tex
<pattern>::<text>
```
前者は `<pattern>` を前から検索し、見つかったら対応する `<text>` に置換する規則です。後者はそれに加えて、置換直後にプロセスを終了する規則です。また、`<pattern>` と `<text>` の前後の空白文字は無視されます。

これらの規則は1行ごとに記述し、規則適用時は上の規則から順に走査・適用され、置換が起きるたびに走査が初めに戻されます。文字列 `:` または `::` を含まない行は無視されます。

以下は `o` の列の入力に対し、`o` の個数を出力する置換規則の定義例です。
```tex
\MarkovAlgorithmCode[mycount]{
0o:1
1o:2
2o:3
3o:4
4o:5
5o:6
6o:7
7o:8
8o:9
9o:o0
o:1
}
\MarkovAlgorithmInput[mycount]{oooooooooooo}% Output `12'
```
`\MarkovAlgorithmDebugModeOn` と `\MarkovAlgorithmDebugModeOff` は置換処理が終了しない場合への対策として、設定した数値を置換回数の上限として強制終了させるツールです。

たとえば
```tex
\MarkovAlgorithmDebugModeOn{4}
```
と記述してから規則を適用させると、4回の置換が行われた直後にプロセスが強制終了します。各置換での入力文字列の遷移が出力されます。
```tex
\MarkovAlgorithmDebugModeOff
```
と記述することでこのデバッグモードは終了します。
