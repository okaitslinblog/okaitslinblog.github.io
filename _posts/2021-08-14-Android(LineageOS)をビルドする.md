---
layout: post
title: Android 11 (LineageOS 18.1)をビルドする
description: x86もビルドできます。ちょっとダウンロードが速くなってます。
categories: google
highlight: false
disqus: true
author: okaits#7534
---

Android 11をビルドする方法です。<br>

{:TOC}

<h1>作業フォルダ作成</h1>
作業フォルダの作成です。<br>
<pre class="prettyprint"><code class="prettyprint lang-bash">mkdir -p ${HOME}/google/android/lineageos/18.1/bin
cd ${HOME}/google/android/lineageos/18.1
echo 'export PATH="${PATH}:${HOME}/google/android/lineageos/18.1/bin"' >> ${HOME}/.bashrc
source ${HOME}/.bashrc
</code></pre><br>
<h1>依存ソフトのインストール</h1>
ビルドに必要なソフトのインストールです。<br>
<pre class="prettyprint"><code class="prettyprint lang-bash">sudo apt install build-essential ccache libncurses5 libssl-dev m4 unzip zip -y
if ls ${HOME}/google/depot_tools >/dev/null 2>&1
  then
  true
  else
  cd ${HOME}/google/
  git clone https://chromium.googlesource.com/chromium/tools/depot_tools.git
  echo 'export PATH="${HOME}/google/depot_tools:${PATH}"' >> ${HOME}/.bashrc
  source ${HOME}/.bashrc
  cd ${HOME}/google/android/lineageos/18.1
fi
</code></pre><br>
<h1>ソースコードのダウンロード</h1>
ソースコードのダウンロードです。<br>
ちなみに編集可能になってます。<br>
なお、<br>
<pre class="prettyprint"><code class="prettyprint">Testing colorized output (for 'repo diff', 'repo status'):
  black    red      green    yellow   blue     magenta   cyan     white
  bold     dim      ul       reverse
Enable color display in this user account (y/N)?
</code></pre><br>
と質問されたら、yと答えてください。<br>
また、名前とメールアドレスを聞かれますが、ビルドしている人の名前（ニックネームでも可）とメールアドレスを入力してください。<br>
<pre class="prettyprint"><code class="prettyprint lang-bash">
repo init -u git://github.com/LineageOS/android.git -b lineage-18.1 --depth=1 --config-name
repo sync -j4
</code></pre>
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
<pre class="prettyprint"><code class="prettyprint lang-bash contenteditable">source build/envsetup.sh
breakfast [デバイス名]
croot
brunch [デバイス名]
</code></pre>
これでできます。<br>