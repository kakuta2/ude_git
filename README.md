# ude_git

## ファイルの削除
```
git rm ファイル名
git rm -r ディレクトリ名
```

## gitからは削除するがファイルの実態は残す
→リポジトリのファイルのみ削除される
```
git rm --cached ファイル名
```

## ステージに上がっているファイルのdiff
```
git diff --staged
```

## コマンドにエイリアスをつける
```
git config --global alias.ci commit
git config --global alias.st status
git config --global alias.br branch
git config --global alias.co checkout
```
※globalをつけることによってPC全体の設定になる
~/.git/config

globalをつけない場合はそのプロジェクトのみ
<project>/.git/config

## バージョン管理しないファイル
- パスワード
- キャッシュ

.gitignoreを編集
```
#指定したファイルを除外
index.html

#ルートディレクトリを指定
/root.html

#ディレクトリ以下を除外
dir/

#/以外の文字列にマッチ「*」
#→１つディレクトリを潜った次の階層のcssファイルを除外
/*/*.css
```


■■変更を取り消す
→ステージの内容をワークツリーに上書きされる
```
git checkout -- <ファイル名>
git checkout -- <ディレクトリ名>

git checkout -- .
```

■■ステージに追加した変更を取り消す
→リポジトリの内容をステージに上書きされる
※ワークツリーが変更前に戻るわけではないので注意
　戻したい場合はresetした後にcheckoutする
```
git reset HEAD ファイル名
git reset HEAD ディレクトリ名

git reset HEAD .
```

■■直前のcommitをやり直す
※リモートリポジトリにpuchしたコミットはやり直しはNG
```
git commit --amend
```

①ファイルをコミット
間違いに気づく
②ワークツリーのファイルを修正してgit add でステージに追加
③git commit --amend
→さっきのコミットが上書きされる


■■リモートリポジトリの情報を確認する
```
git remote

#対応するURLを表示
git remote -v
```

>origin  git@github.com:kakuta2/sb_batch_sample.git (fetch)
>origin  git@github.com:kakuta2/sb_batch_sample.git (push)
→fetchの場合とpushの場合で分けることができる(この場合は同じ)

■■リモートリポジトリを追加する
```
git remote add <リモート名> <リモートURL>
```
```
# ex)
originというショートカット名で追加
git remote add origin http://github.com/user/repo.git
tutorialというショートカット名で追加
git remote add tutorial http://github.com/user/repo.git
```

■■リモートから取得
■フェッチ編
```
git fetch <リモート名>
git fetch origin
```
→リモートリポジトリからローカルリポジトリ（※）に情報を取得
　ワークツリーに取得するわけではないので留意
※remotes/リモート/ブランチ
```
git branch -a
  remotes/origin/HEAD -> origin/main
  remotes/origin/ST001
  remotes/origin/main
```

ワークツリーの内容をremotes/origin/mainの内容に切り替える
```
git checkout remotes/origin/main
```

ローカルリポジトリからワークツリーに情報を持ってくる場合
```
git merge origin/main
```
→orignの内容をmainにマージ

■プル編※フェッチのほうが安全
リモートから情報を取得してマージまで一度にやる
```
git pull <リモート名> <ブランチ名>
git pull origin main

#上記は省略可能
git pull

#下記と同じ
git fetch origin main
git merge origin/main
```

## リモートの詳細情報を表示
```
git remote show <リモート名>
git remote show origin
```


## リモート名の変更
git remote rename <旧リモート名> <新リモート名>
git remote rename tuto tuto_new 

## リモート名の変更
git remote rm <リモート名>
git remote rm tuto_new 


■■■ブランチ■■■
■■ブランチとは
コミットを指したポインタ
→.git/refs/
■■HEAD
今自分が作業しているブランチを指示したポインタ
→.git/HEAD

■ブランチの作成
```
git branch <ブランチ名>
git branch feature
→ブランチが作成されるが、ブランチの切替はされないので留意
```

■ブランチの一覧
```
git branch 

#すべて(リモートも含めた)のブランチの表示
git branch -a
```

■ログ
```
git log --oneline --decorate
git log --oneline --graph --decorate
```
→decorateでどのブランチがどのコミットを指しているか

■ブランチの切替え
```
git checkout <既存のブランチ名>
git checkout feature
#ブランチを新規作成して切替
git checkout -b <新ブランチ名>
→-bはブランチの意味
```

■変更をマージする
```
git merge <ブランチ名>
git merge <リモート名/ブランチ名>
git merge origin/master 
→作業中のブランチにマージする
```

■ブランチの削除
```
git branch -d <ブランチ名>
→マスタにマージしていない変更が残っている場合は削除しない
```
■■マージの種類
fast foward

■ブランチ間の差分
```
git diff <ブランチ名A> <ブランチ名B>
```

■リモートから特定のブランチを指定してcloneする方法
```
git clone -b ブランチ名 https://リポジトリのアドレス
git clone -b kadai01 git@github.com:kakuta2/ude_git.git
```

■リポジトリに「CR LF」の改行コードがあるかを調べる
```
git grep --cached -I $'\r'
```
■[Git] 作業用に切ったブランチに元ブランチの最新内容を取り込む
https://pasomaki.com/git-branch-flow/
