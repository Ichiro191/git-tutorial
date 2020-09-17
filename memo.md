# GitHub実践入門

## 第4章 Gitを操作しながら学ぶ

### 4.1 基本的な操作

- `git init`  
    p.40 リポジトリの初期化．.gitディレクトリが生成される．

- `git status`  
    p.41 リポジトリの状態を表示．

- `git add file_name`  
    p.42 ステージ領域（またはインデックス）と呼ばれるコミットをする前の1次領域にファイルを登録．

- `git commit -m "foo bar"`  
    p.43 ステージ領域に登録されている時点のファイル群をリポジトリの歴史として実際に記録する．復元などが可能になる．  
    `-m`オプション付きで要約したコミットメッセージを記述．  
    `-m`オプション無しで詳細なコミットメッセージを記述するためのエディタが起動．この場合は1行目は要約，2行目は空行，3行目に詳細を記述．中止なら保存せずにエディタを閉じる．

- `git log`  
    p.44 リポジトリにコミットされたログを確認．  
    `--pretty=short`オプション付きでコミットメッセージの要約のみを表示．  
    ファイル名を指定すると，そのファイルのログのみ表示．  
    `-p`オプション付きでそのコミットで行われた差分も表示される．

- `git diff`  
    p.46 ワークツリー（またはワーキングツリー，ファイルの編集を行っている場所）・ステージ領域・最新コミット間の差分を確認．

    - オプションなしの `git diff`  
        ワークツリーとステージ領域間の差分を確認．

    - `git diff HEAD`  
        ワークツリーと最新コミット（HEAD）間の差分を確認．


### 4.2 ブランチの操作

- `git branch`  
    p.49 ブランチ名の一覧の表示と，現在のブランチの確認  
    ブランチの名前を追加することで，新たなブランチの作成も可能．

- `git checkout -b branch_name`  
    p.50 branch_nameという名前のブランチを作成して，移動する．  
    `git branch branch_name && git checkout branch_name`と同じ働きをする．

- `git checkout -`  
    p.51 一つ前のブランチに切り替える．

- `git merge --no-ff branch_name`  
    p.53 現在のブランチに，指定したブランチをマージする．  
    `--no-ff`をつけるとエディタが開いてマージコミットを作成できる．

- `git log --graph`  
    p.54 ブランチの分岐と統合を視覚的に確認できる．


### 4.3 コミットを変更する操作

- `git reset --hard hash_value`  
    p.56 リポジトリのHEAD，ステージ，現在のワーキングツリーを指定した以前の状態（コミット）に戻す．  
    ハッシュ値はgit logとかで調べる．なお，ハッシュ値は4文字以上入力すれば動作する．

- `git reflog`  
    p.58 現在のリポジトリで行われた作業のログを確認できる．  
    歴史を戻す前のハッシュ値を見つけて，`git reset --hard`で指定することで，歴史を戻す前の状態を復元できる．  
    GitのGCが行われない限り．

- `git commit --amend`  
    p.61 直前のコミットメッセージを修正する．エディタが起動する．

- `git commit -am "foo bar"`  
    p.64 git add と git commit -m を同時に行えるコマンド．

- `git rebase -i HEAD~2`  
    p.65 HEAD（最新コミット）を含めた指定数字回までのコミットを統合する．  
    起動するエディタで，統合してなくしたいコミットの行頭のpickをfixupに書き換える．


### 4.4 リモートリポジトリへの送信

- `git remote add origin git@github.com:User/repository_name.git`  
    p.68 GitHubでリポジトリを作った後，ローカルリポジトリのリモートリポジトリとして登録する．  
    originは名前（識別子）であり，以後リモートレポジトリを指すようになる．

- masterブランチにいる状態で，`git push -u origin master`  
    p.68 originという名前のリモートリポジトリのmasterブランチに，現在のブランチ（ここではローカルリポジトリにあるmasterブランチ）の内容を送信する．  
    `-u`オプションは，ローカルリポジトリの現在のブランチのupstreamが，originリポジトリのmasterブランチであることを同時に設定する．このオプションにより，`git pull`コマンドを実行する時に引数を与えずとも，このローカルリポジトリのブランチはoriginのmasterブランチから取得されるようになる．  
    p.69 他のブランチから`git push -u origin other_branch`すれば，同じく送信できる．upstreamがリモート（origin）のother_branchになる点も同様．

- `git push`  
    一度`-u`オプション付きで実行すればupstreamが設定され追跡されているため，その後は引数なしでもOK．


### 4.5 リモートリポジトリから取得

- 別のディレクトリで，`git clone git@github.com:User/repository_name.git`  
    p.70 リモートリポジトリを持ってくる．  
    直後にいるブランチはmasterリポジトリになっている．  
    clone元のリモートリポジトリはoriginという名前で参照できるように自動で設定されている．

- `git branch -a`  
    p.71 ローカルリポジトリだけでなくリモートリポジトリを含んだブランチ情報を表示する．  
    ちなみに`-r`オプションはリモートリポジトリのブランチ情報のみ表示する．

- `git checkout -b new_local_repository_name origin/remote_repository_name`  
    p.71 リモートリポジトリのブランチを元に，ローカルリポジトリにブランチを作成する．  
    `-b`オプションの後ろが新たにローカルリポジトリに作成するブランチの名前．  
    その後ろに与えているのが，チェックアウト元となるリモートリポジトリのブランチ．  
    ローカルのブランチとリモートのブランチは同じ名前にすると分かりやすい．

- リモートリポジトリの変更を反映したいブランチで`git pull origin remote_branch_name`  
    ローカルリポジトリのブランチをリモートリポジトリの状態に更新する．  
    更新したいブランチにいる状態で，upstreamがリモートのリポジトリに設定されていれば，`git pull`だけでOK．


## Other Git Commands

- ファイルのリネーム  
    参考: [【 git mv 】コマンド――ファイルを移動する／リネームする](https://www.atmarkit.co.jp/ait/articles/2006/12/news017.html)  
    `git mv old_file new_file`．ステージ領域にファイル名の変更として登録される．  
    「git mv」ではなく「mv」コマンドでファイル名を変更・移動した場合は「ワークツリーでファイルを削除し、新しいファイルを追加した」という扱いになる．

- git add で複数ファイルを同時に行うコマンド  
    参考: [git add -u と git add -A と git add . の違い](https://note.nkmk.me/git-add-u-a-period/)

- remoteに追加されたブランチを見れるようにする  
    参考: [gitでリモートに追加されたブランチが表示されないときは](https://www.atotok.co.jp/labo/4040620404d4987071e9dfe58eee87a0)  
    `git ls-remote (指定する場合はリモートリポジトリ名，originとか）`で新しくリモートに追加されたブランチが確認できたら，  
    `git fetch`（リモートブランチの最新の情報を取得するコマンド）を実行する．

- ローカルのブランチが追跡しているリモートブランチを確認する方法  
    参考: [gitでローカルのブランチが追跡しているリモートブランチを確認する方法](https://qiita.com/kz_morita/items/c624f8cf27ec82de0baa)  
    `git branch -vv` 結果として以下のような情報が得られる。  
    ```
    ブランチ名 コミット番号 [追跡ブランチ名] コミットメッセージ
    ```
    リモートのブランチを追跡していないブランチに対して，追跡するように設定する方法もある．

- Git で不要になったローカルブランチ・リモートブランチを削除する方法  
    参考（2013年なので，少し古い）: [Git で不要になったローカルブランチ・リモートブランチを削除する方法](https://qiita.com/iorionda/items/c7e0aca399371068a9b8)

- git fetch、merge、pullの違い  
    参考: [【初心者向け】git fetch、git merge、git pullの違いについて - Qiita](https://qiita.com/wann/items/688bc17460a457104d7d)  
    `git fetch`…リモートの全ブランチ → ローカルの全「origin/xxx」ブランチ．  
    `git merge xxx`…ローカルの「origin/xxx」ブランチ → ローカルの「xxx」ブランチ  
    `git pull origin xxx`  = fetch + merge． つまり，リモートの「xxx」ブランチ → ローカルの「origin/xxx」ブランチ → ローカルの「xxx」ブランチ


## Other Git tips

- [GitのHEADとは何者なのか - Qiita](https://qiita.com/ymzk-jp/items/00ff664da60c37458aaa)  


## Others

- ssh のパスフレーズを変更  
    参考: [sshの秘密鍵パスフレーズの確認と変更](https://qiita.com/shukan0728/items/e654953c89aab6f847a4)  
    `ssh-keygen -f 秘密鍵ファイル名 -p`

- git bashの青色が読みづらい問題  
    解決策 参考: [Windows環境で快適にBash(Git Bash)とターミナル(mintty)を使うために色とフォントを変える](https://qiita.com/Cesaroshun/items/6f7aa548ca65b137ca15)  
    dracula が一番読みやすく感じる．

- GitHubからリポジトリを削除  
    参考: [GitHub でリポジトリを作成・削除する方法 - Qiita](https://qiita.com/TK-C/items/c72edc46e74ccbb7ba78)  
    削除したいリポジトリのsettingsの一番下から．

- powershellのプロンプトを編集  
    [posh-gitのインストール＆プロンプトカスタマイズ - Qiita](https://qiita.com/wenbose/items/3ebc2e8acd4513d71ac3)
    [PowerShellでGitを使いやすくする - shuhelohelo’s blog](https://shuhelohelo.hatenablog.com/entry/2019/10/22/025603)
    改行文字 `n