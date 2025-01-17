---
layout: post
title: Chromium OSのビルドの仕方
description: 通常よりもダウンロードが速くなるようにしました。
categories: google
highlight: false
disqus: true
author: okaits#7534
---

ChromiumOSのビルドの仕方です。<br>
通常よりもダウンロードが速くなってます。<br>

{:TOC}

<h1>作業フォルダを作る</h1>
作業フォルダを作ります。
<pre class="prettyprint"><code class="prettyprint lang-bash">cd $HOME
umask 022
mkdir -p google/chromiumos
</code></pre><br>
<h1>依存ソフトのインストール</h1>
ビルドに必要なソフトのインストールです。<br>
<pre class="prettyprint"><code class="prettyprint lang-bash">sudo apt-get install git-core gitk git-gui curl lvm2 thin-provisioning-tools python3-pkg-resources python3-virtualenv python3-oauth2client xz-utils python3.6</code></pre>
<h1>depot_tools導入</h1>
depot_toolsとは、ソースコードのダウンロードなどに使う「repo」コマンドなどをまとめたツールセットです。<br>
導入は簡単です。<br>
<pre class="prettyprint"><code class="prettyprint lang-bash">cd ${HOME}/google/
git clone https://chromium.googlesource.com/chromium/tools/depot_tools.git
echo 'export PATH="${HOME}/google/depot_tools:${PATH}"' >> ${HOME}/.bashrc
source ${HOME}/.bashrc
</code></pre><br>
<h1>ソースコードのダウンロード</h1>
ソースコードのダウンロードです。普通めっちゃ時間かるん<b>ですが</b>、なんと<code class="prettyprint">--depth=1</code>というオプションを<code class="prettyprint lang-bash">repo init</code>コマンドにつけ足すと、ものすごく速くなります。<br>
<pre class="prettyprint"><code class="prettyprint lang-bash">cd ${HOME}/google/chromiumos/
repo init -u https://chromium.googlesource.com/chromiumos/manifest.git --repo-url https://chromium.googlesource.com/external/repo.git --depth=1
repo sync -j4
</code></pre>
<p class="color: gray;">ちなみに、なぜ<code class="prettyprint">--depth=1</code>オプションをつけると速くなるというと、Gitには変更履歴を保存する機能があって、デフォルトではその変更履歴が一からダウンロードされます。Chrome OSのような変更履歴がものすごく多いレポジトリは、もちろんダウンロードに時間がかかります。そこで、<code class="prettyprint">--depth=1</code>オプションをつけると変更履歴が最後から1番目までしかダウンロードされません。なので、速くなるわけです。ちなみに、gitコマンドでも使えます。</p><br>
<p class="color: gray; height: 0.25cm;">すみません間違ってるかもしれません。</p><br>
<h1>Google APIの設定</h1>
Google APIを設定してChromium OSからログインできるようにします。<br>
まず、<a href="https://groups.google.com/a/chromium.org/forum/?fromgroups#!forum/chromium-dev">ここ</a>に参加します。<br>
参加したら、<a href="https://cloud.google.com/console">ここ</a>へ行って、左上の「My First Project」をクリックします。<br>
そしたら、右上の「新しいプロジェクト」をクリックして、適当に名前をつけて作成してください。<br>
それができたら、左上の「My First Project」をもう一度クリックして、自分でつけた名前が表示されているところの読み込みが終わるまで待ちます。<br>
読み込みが終わったら、そこをクリックしてください。<br>
そしたら、上の検索ボックスに、次の文字例をいれ、有効化してください。<br>
<ul>
<li>Cloud Search API<br></li>
<li>Geolocation API<br></li>
<li>Google Drive API<br></li>
<li>Safe Browsing API<br></li>
<li>Time Zone API<br></li>
<li>Admin SDK API<br></li>
<li>Geocoding API<br></li>
<li>Google Assistant API<br></li>
<li>Google Calendar API<br></li>
<li>Nearby Messages API<br></li>
</ul>
終わったら、左のメニューの「APIとサービス」、「認証情報」の順にクリックしてください。<br>
そして、「+ 認証情報を作成」、「OAuth クライアントID」、「同意画面の設定」の順にクリックし、<br>
「外部」を選択し、「作成」をクリックしてください。<br>
そして、アプリ名に適当な名前を入力し、ユーザーサポートメールとデベロッパーの連絡先情報に自分のメールアドレスを選択し、「保存して次へ」をクリックします。<br>
そして、「スコープを追加または削除」ボタンを押し、すべてにチェックをつけ、「更新」を押します。<br>
そして、「保存して次へ」を押します。<br>
そして、「+ ADD USERS」ボタンを押し、Chromium OSにログインするユーザーのメールアドレスを入力して、「追加」ボタンを押してください。<br>
そして、「保存して次へ」を押し、「ダッシュボードに戻る」ボタンを押してください。<br>
そしたらまた、「+ 認証情報を作成」、「OAuth クライアント ID」の順にクリックし、<br>
アプリケーションの種類に「デスクトップ アプリ」、名前に適当な名前をつけて作成してください。<br>
そしたら、クライアントIDとクライアントシークレットが出るので、どこかに保存してください。<br>
そしたらまた「+ 認証情報を作成」、「API キー」の順にクリックしてください。<br>
そしたら、APIキーが出てくるので、保存してください。<br>
そしたら、これを実行してください。<br>
<pre class="prettyprint"><code class="prettyprint lang-bash" contenteditable>api=[メモしておいたAPIキー]
clientId=メモしておいたクライアントID
clientSecret=メモしておいたクライアントシークレット
echo "'google_api_key': '"${api}"'," > ${HOME}/.googleapikeys
echo "'google_default_client_id: '"${clientId}"'," >> ${HOME}/.googleapikeys
echo "'google_default_client_secret': '"${clientSecret}"'," >> ${HOME}/.googleapikeys
</code></pre>
&uarr; 編集できるようになってます。コピーする前に最初のところのAPIキーとクライアントID,シークレットだけ書き換えたほうがいいと思います。<br>
<h1>ビルド</h1>
いよいよビルドです。<br>
<pre class="prettyprint"><code class="prettyprint">cros_sdk
export BOARD=amd64-generic
setup_board --board=${BOARD}
./set_shared_user_password.sh
./build_packages --board=${BOARD}
./build_image --board=${BOARD} --noenable_rootfs_verification test
</code></pre>
これで終わりです。