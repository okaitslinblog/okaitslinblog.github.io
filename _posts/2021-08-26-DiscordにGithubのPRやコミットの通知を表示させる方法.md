---
layout: post
title: DiscordにGithubのPRやコミットの通知を表示させる方法
description: DiPRやコミット以外にも、Starやフォーク、ActionsやPagesなどについても通知してくれます。
categories: github
highlight: true
disqus: true
author: okaits#7534
---
今回は、DiscordにGithubのPRやコミットの通知を表示させる方法です。<br>
例えば、Discordで繋がっている友達や開発仲間などと一緒にGithubを用いて開発をしているときにコミットの情報が出てくるといろいろと便利です。<br>
今回はそれをやってみましょう。<br>

{:TOC}

<br>
<h1>Discord側の設定</h1>
Discord側の設定です。<br>
まず、右上のサーバー名が書いてある所をクリックし、サーバー設定をクリックします。<br>
そしたら、右側の連携サービスをクリックして、ウェブフックをクリックします。<br>
クリックしたら、新しいウェブフックボタンを押します。<br>
すると、「Captain Hook」が出てくるので、お名前に「github」、チャンネルに通知させるチャンネルを指定して保存してください。<br>
保存したら、ウェブフックURLをコピーボタンを押してください。<br>
<h1>Github側の設定</h1>
Github側の設定です。<br>
まず、レポジトリのSettingsを開いて、Webhookをクリックして、Add webhookボタンを押してください。<br>
そしたら、Payload URLにコピーしたURLを貼り付け、<b>最後に「/github」を付け足し、</b>Content typeはapplication/jsonにし、Send me everything.を選択し、Add webhookボタンを押してください。<br>
これだけでできます。<br>
