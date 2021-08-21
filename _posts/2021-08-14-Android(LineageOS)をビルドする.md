--
layout: post
title: Android 11 (LineageOS 18.1)をビルドする
description: x86もビルドできます。ちょっとダウンロードが速くなってます。
categories: google
highlight: false
disqus: true
author: okaits#7534
---
 <!-- EthereumAds -->
   <div id="EthereumAds-okaitslinblog"></div>
   <script src="https://ethereumads.com/adviewer.js">
   </script>
   <script>
       EthereumAds.initAdSlot({
           acceptedCurrencies: ["ALL"], // option ALL for all whitelisted tokens, ETH for Ethereum, DAI for DAI Stablecoin
           //validatorEndpoint:"", // optional custom validator
           mediaType: "image_320x50",
           fallback: "default", // default, none, custom url
           slot: "okaitslinblog",
           address: "0xd404f198c4f580727eb11cd69b581d5f10c7efd9",
           platform: "",
           affiliate: "",
           keywords:"", //comma separatedy
           adult: false,
           version: "1.00"
       });
       /*
        for responsive ads add and adjust this according to your needs:
        responsive: [
            { mediaType: "image_728x90", minWidth: 728 },
            { mediaType: "image_300x600" }
        ],
       */
   </script>
   <!-- /EthereumAds --> 
<h1>作業フォルダ作成</h1>
作業フォルダの作成です。<br>
<pre class="prettyprint"><code class="prettyprint lang-bash">
mkdir -p ${HOME}/google/android/lineageos/18.1/bin
cd ${HOME}/google/andorid/lineageos/18.1
echo 'export PATH="${PATH}:${HOME}/google/android/lineageos/18.1/bin"' >> ${HOME}/.bashrc
source ${HOME}/.bashrc
</code></pre><br>
<h1>依存ソフトのインストール</h1>
ビルドに必要なソフトのインストールです。<br>
<pre class="prettyprint"><code class="prettyprint lang-bash">
sudo apt install build-essential ccache libncurses5 libssl-dev m4 unzip zip
if ls ${HOME}/google/depot_tools >/dev/null 2>&1
  then
  true
  else
  git clone https://chromium.googlesource.com/chromium/tools/depot_tools.git
  echo 'export PATH="${HOME}/google/depot_tools:${PATH}"' >> ${HOME}/.bashrc
fi
</code></pre><br>
<h1>ソースコードのダウンロード</h1>
ソースコードのダウンロードです。<br>
ちなみに編集可能になってます。
<pre class="prettyprint"><code class="prettyprint lang-bash" contenteditable>
git config --global user.email [ビルドしてる人のメールアドレス]
git config --global user.name [ビルドしてる人の名前/ニックネーム]
repo init -u git://github.com/LineageOS/android.git -b lineage-18.1 --depth=1
repo sync -j4
</code></pre>
<h1>ビルド</h1>
いよいよビルドです。<br>
<pre class="prettyprint"><code class="prettyprint lang-bash">
source build/envsetup.sh
breakfast [デバイス名]
croot
brunch [デバイス名]
</code></pre>
これでできます。<br>