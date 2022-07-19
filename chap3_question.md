# 疑問(第3章)
　README.mdに追記したが、そういうものと思ってある程度進めること。  
しかし、疑問を持つことは大切なのでメモ程度に書き込んでいく。

## osbook_day03aの前
### レジスタの話。
　みかん本の前に少しだけOS自作入門を読んでいた。その内容に近い感じの内容。しかし、CPUを保護モードにするためレジスタの値を変えるというのはすべてのCPUアーキテクチャで共通なのだろうか？OS自作入門では確かx86の話が前提だと思うし、みかん本では節の初めのほうでx86-64の話だとしている。もし、今後x86-64しか触らないのであればこの話は忘れていいかもしれないが、そんなことはない。そういうことはおそらくみかん本の参考文献[3]が役に立つと思う。  
> - [Intel® 64 and IA-32 Architectures Software Developer Manuals](https://www.intel.com/content/www/us/en/developer/articles/technical/intel-sdm.html)  

しかし、試しに  ```Intel® 64 and IA-32 Architectures Software Developer’s Manual Combined Volumes: 1, 2A, 2B, 2C, 2D, 3A, 3B, 3C, 3D, and 4```  を開いたが、約5000ページある。なので今はやめておこう。  
もし、CPUのアーキテクチャを意識しなければならないときはこれを読む必要がある。  

## osbook_day03a
### Loader.inf
　これはedk2固有のファイルなので、OS作りの本質でない（ほかのツールなら不要であるため）。そのためこのファイルの変更はそういうものととらえる。  
　ただ、謎は```git diff osbook_day02b osbook_day03a```と実行し前回との差分をとったが、変化していないところまで表示されるのはなぜだろう？私のコマンドが間違っているのだろうか？ひとまず目視で差分を取れたのでそれを書き込んでいく。

### EFI boot stub?
### EFI_FILE_INFO型？
　次のページに詳細があった。

### "\\\kernel.elf"の文字数
　ヌル文字を含めると13文字に見える。初めの'\\'はエスケープシーケンスの意味だろうか？

### file_info_buffer[file_info_size]はなぜOK?
　私がC言語を学習したときは、配列の要素数は変数はダメだったはず…。気になって独習Cを覗きに行くと、p.122にC11の仕様でVLA(variable length array)が使えるらしい。なので今回だとVLAが有効なのでOKだということだと思う。
　ネットでも検索してみたら以下の記事がヒットした。  
> - [【C99】可変長配列を試してみる](https://tyfkda.github.io/blog/2019/12/31/variable-length-array.html)  

この記事によればC99でVLAが使えるとのことですが、C99とかC11の後の数字は多分西暦の下二桁だとおもう（調べたらその通りだった。[C99とC11](https://www.cloverfield.co.jp/2016/06/24/c99%E3%81%A8c11/))  
　C言語のバージョンによってある程度違うと思うのでバージョンも少し気にしておきたい。(現状コンパイラのバージョンについて言及されていないので出てきたらしっかり確認したい。)

### ```UINT8 file_info_buffer[file_info_size];```?
　ファイル情報を保存する配列はなぜUINT8の配列で保存するのだろうか？単純に、GetInfo()の仕様でUINT8であるだけであればいいが…。今はそういうものとする。

### EFI_PHYSICAL_ADDRESS ？
### typedef void EntryPointType(void); ?
　この一文でちょっと焦った。これまでも、   
```
    typedef int (*FunctionPointer)(int);
```
みたいな、関数ポインタ型の宣言を見てきたので、当然  
```
    typedef void EntryPointType(void);
```
こっちもあるよねというのは理解できるが想像していなかった。  

以下に参考を貼る。  
> - [C言語のtypedefについて具体例を用いて分かりやすく解説](https://daeudaeu.com/c-typedef/)  
> - [Typedefの考え方](https://qiita.com/aminevsky/items/82ecce1d6d8b42d65533)  

特に2つ目が参考になった。やっぱりだが自分と同じ疑問を持っている人がいて少しほっとした。typedefの書き方の基本から逸脱しているが間違いではない。  

とりあえず、認識としては、引数がvoid、返り値もvoidの関数の型を宣言するといった型宣言という認識でいいと思う。
そういう風に考えると関数ポインタの方も理解しやすい。  
それにtypedefは型名のあだ名を作るのが役割だし、その認識で間違いないと思う。

## osbook_day03b
### for文のi++と++i
　以下のURLにきれいにまとめてくれていた。  
> - [i++と++iの違い](https://qiita.com/suuungwoo/items/e054fdcb5a4805bb226b)  

for文においては違いはない。なので好みの問題。

この節は説明が少なく、そういうものと割り切るものが多かったので、写経がメインだった。  
一通り写経したので次に進む。

## osbook_day03c
　そういうものという感じのもの以外は疑問はない。   
　基本的にはブートローダで関数プロトタイプ宣言と、GOPデータを引数で渡す（GOPを事前に取得する必要はあるが）。  
　カーネル側の引数も変えて、渡された配列にデータを書き込むと、色を塗れるという感じだった。  
　講義動画ではABIについて言及されていた。その話の概要は難しくないが詳細はかなり知識がないと読み解けない内容だと思ったので今は放置する。