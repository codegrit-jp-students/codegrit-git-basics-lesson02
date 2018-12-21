# レッスン2. GitHub入門

## 目的

- GitHubとは何か理解する。
- GitHubアカウントを取得する。
- push, pullのような基本的なコマンドをマスターする。

## GitHubとは?

GitHubは、Gitリポジトリをクラウド上で管理するためのツールです。2018年にマイクロソフトの傘下となりました。オープンソースプロジェクトのほとんどがGitHubを利用しており、またFacebook、Googleなどの大企業はもちろんスタートアップも利用するデファクトスタンダード的なサービスです。

プロのプログラマには、GitとGitHubを利用したチーム開発の知識が求められます。また多くの企業では採用の際にGitHubアカウントの提示を求めGitHub上にあるプロジェクトやコードの確認を行います。GitHubに慣れ親しんでいると、今後のキャリアに有利です。

## なぜGitHubを使うのか

レッスン1では、Gitを使ってローカルリポジトリを作成しました。しかし、往々にしてパソコンを紛失する、盗難にあう、故障する、といったトラブルにあうことがあります。こうしたときに、クラウド上にリポジトリがあれば、いつでもプロジェクトのコードを復帰できます。

またチームでの開発においては、クラウド上でのファイル管理は欠かせません。GitHubにはフォークやPull RequestといったGitを拡張した機能が備わっており、GitHubを使うことがよりスムーズな開発体験に繋がります。

## GitHub以外のサービス

Gitのリモートリポジトリとして利用できるサービスはGitHubの他にもいくつか有名なものがあります。一つはアトラシアン社の運営している[Bitbuket](https://bitbucket.org/product)です。また、レッスン5で少しふれますが[GitLab](https://about.gitlab.com/)というサービスはオープンソースとして公開されていることや、無料で非公開のリポジトリを作成出来ることもあり人気が高まっています。基本的にはGitHubと機能面は似ているので、まずはGitHubに慣れることがオススメです。

## GitHubへのユーザー登録

![GitHubトップ](../images/github1.png)

まずはGitHubトップページより、ユーザー登録を行いましょう。

## SSHキーペアを作成する

さてGitHubにユーザー登録を行い、これからGitHubにリモートリポジトリを作成しローカルリポジトリのコードをアップロードしていきます。

しかし、その前にしておくべきことがあります。それがSSH認証の設定です。SSHの詳細な仕組みはここでは触れませんが、クラウド上のサーバーと安全にやり取りするための仕組みだと考えてください。

初心者の方にとってちょっとハードルが高いのですが、ローカル上でSSHキーペアを発行し、GitHubに公開鍵を登録して、GitHubとのやり取りの準備を整えます。

人によっては既にSSHキーペアがある場合があります。試しにコマンドラインに以下のコマンドを入力してみてください。

```bash
$ cat ~/.ssh/id_rsa.pub
```

そこで文字の羅列が出てきたらそれがSSHキーペアです。文字の羅列をコピーして次の項目に進んでください。

もし何も出てこなかった場合は以下の手順でSSHキーペアを作成します。

```bash
$ ssh-keygen -t rsa -C "あなたのメールアドレス" -b 4096
Generating public/private rsa key pair.
Enter file in which to save the key (/Users/user_name/.ssh/id_rsa): 
```
ここは現在SSHキーペアがない状態なので、そのまま`Enter`キーを押せば大丈夫です。次にパスフレーズを聞かれます。

```bash
Enter passphrase (empty for no passphrase): 
```

ここには任意のパスワードを入れましょう。確認用にもう一度同じパスワードを求められるので再度入力します。完了すると、以下のようにメッセージが表示されるはずです。

```bash
The key's randomart image is:
+---[RSA 4096]----+
|                 |
|     色々な記号    |
|                 |
+----[SHA256]-----+
```

無事保存が終わったら、作成されたキーペアの公開鍵を確認してコピーします。

```bash
$ cat ~/.ssh/id_rsa.pub
```

## GitHubに公開鍵を登録する

先ほどの項目でコピーした公開鍵をGitHub上で登録します。

![GitHub SSH設定1](../images/github-ssh-1.png)
まずは、GitHubの画面右上のメニューから"Settings"に進み、その後左のメニューから"SSH and GPG keys"を選びます。


![GitHub SSH設定2](../images/github-ssh-2.png)
次の画面では、右上に"New SSH key"と書かれたボタンがあるのでこちらをクリックしてください。あるいは直接以下のURLから登録ページに移動できます。

[GitHub SSH設定ページ](https://github.com/settings/ssh/new)

![GitHub SSH設定3](../images/github-ssh-3.png)
移動したら、分かりやすい名前をつけて、コピーした公開鍵を貼り付けて保存します。

これで準備完了です。

## GitHubにリモートリポジトリを作成する。

次にGitHubにリポジトリを作成しましょう。Lesson1で学んだ通り、このリポジトリのことをパソコン上にあるローカルリポジトリと対比させてリモートリポジトリと呼びます。

作成するには、まずトップページの左上のRepositoriesというメニューから、"New"を選ぶ、あるいは右上の"+"ボタンのメニューから"New Repository"を選びます。

![リポジトリ作成1](../images/github-create-repo-1.png)

すると、以下のようなページに移動します。

![リポジトリ作成2](../images/github-create-repo-2.png)

こちらで、適当な名前をつけて、下にある"Create repository"というボタンをクリックすれば完了です。

## GitHub上のリポジトリのURLをローカル上に保存する

リポジトリを作成すると、次のようなページに移動します。

![リポジトリ作成3](../images/github-create-repo-3.png)

まずは、ローカル上に自分のつけたリポジトリの名前でフォルダを作成して、リモートリポジトリを作成していきます。

1. リポジトリの作成

```bash
$ mkdir test-project
$ cd test-project
$ touch README.md
```

2. README.mdの編集後

```bash
$ git init
$ git add .
$ git commit -m "initial commit"
```

3. リモートリポジトリ(GitHub上に作成したリポジトリの追加)

次に、GitHub上に作成したリポジトリのURLを`origin`という名前で保存します。この作業には`git remote add`というコマンドを利用します。具体的には以下のようにします。こちらのコマンドはGitHubのページ上でも同じものがあるのでそれをコピーしましょう。

```bash
$ git remote add origin https://github.com/ユーザー名/test-project.git
```

4. `git push`コマンドでローカルリポジトリをGitHubにアップロードする

最後に`git push`というコマンドを使用して、ローカルリポジトリをGitHubに作成したリポジトリにアップロードします。すると次のようにアップロードが行われるはずです。

```bash
$ git push origin master
Counting objects: 3, done.
Writing objects: 100% (3/3), 230 bytes | 230.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0)
To https://github.com/ユーザー名/test-project.git
 * [new branch]      master -> master
```

ここまで出来たら、GitHubのリポジトリのページをリロードしましょう。ローカルリポジトリの内容がアップロードされたことが確認出来るはずです。

![リポジトリ作成4](../images/github-create-repo-4.png)

## `git pull`でリモート上の変更をローカルに反映する。

さて、次にGitHub上でREADME.mdを編集してみましょう。

![READMEの編集](../images/github-pull-1.png)

変更したらページ下部の"commit change"ボタンをクリックして変更を保存します。

この段階で、GitHub上のリポジトリはローカルのリポジトリに加えて1回分の変更が行われた状態となっています。

この変更をローカルリポジトリに反映しましょう。そのためには`git pull`というコマンドを利用します。

```bash
$ git pull
remote: Counting objects: 3, done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), done.
From https://github.com/ユーザー名/test-project
   3a6e5bd..df18a30  master     -> origin/master
Updating 3a6e5bd..df18a30
Fast-forward
 README.md | 2 ++
 1 file changed, 2 insertions(+)
```

すると上記のように、情報が表示されてGitHub上のリポジトリの変更がローカルリポジトリに反映されました。

今回は、GitHub上で直接編集しましたが、例えばチーム開発ならチームメンバーが加えた変更を`git pull`でローカルリポジトリに加えるというような使い方をするのが一般的です。

## 更に学ぼう

### 本で学ぶ

- [Pro Git](https://git-scm.com/book/ja/v2)

第6章の部分にGitHubのことが書かれています。