# Sutra_copying-Mikanos
## 本リポジトリの目的  
　Mikanosを写経して学習する。あくまで学習が目的です。もしソースコードが必要な方はMikanosの作者のGitHubリポジトリにアクセスしてください。URLは以下の通りです。  
> - [https://github.com/uchan-nos/mikanos.git](https://github.com/uchan-nos/mikanos.git)  

## 写経について
　ゼロからのOS自作入門でOSの作り方について学習する。写経の方法や写経用のGitHubのpublicリポジトリの作成については以下の方のブログとリポジトリを参考にさせていただいた。  
> - [自作OS初心者によるmikanos写経日記](https://zenn.dev/three/articles/2e736b8230e58b)  
> - [three-0-3/my-mikanos](https://github.com/three-0-3/my-mikanos)  

　写経の仕方は上記ブログと同様に、写経ではあるが自分で考えたコードであるように取り組む。また、進む速度は遅々としたものになるが楽しく脱線して本の内容以外にもいろいろ取り込んでいけたらと思う。  
　脱線するとはいえ、なるべきOS作りの本質的なところで脱線したい。わからないところが多いができるだけ本質的かどうか見極めて行きたい。

## Gitの操作失敗(未来の自分用)
　pushする前にマイクロコミットがたまっていたので```git rebase -i ~```でコミットをまとめようとした。その結果、途中のマイクロコミットを消して最新のコミットだけ残そうとした結果、dropの指定をして実行してしまった。その結果、見事ファイルが消えた。ファイルを救済しようと```git reflog + git reset --hard HEAD@{n}```を実行して巻き戻そうとしたが、どうやらgit reset はあくまでHEADを追いかけるらしく、途中のコミットが消えて最新のコミットが残った場合ファイルが復元できないらしい（もっと詳しく調べたらなんとかなるかもしれないが現状不明）。  
　とりあえず、マイクロコミットをなくしたい場合は```git rebase```でdropするのではなく、squashでマージしないといけないということを学んだ。幸い一番時間がかかっていたファイルは事前に救出していたので回復はそんなに時間がかからないが、危険な操作を体感できたことは良かったのでこれから注意したいと思う。