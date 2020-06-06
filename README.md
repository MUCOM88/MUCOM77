# MUCOM77
 
MUCOM77 は PC88 シリーズ用音源ドライバである MUCOM88 を富士通 FM-7 シリーズ向けに移植したものです。<br>
OPN が搭載されている環境であれば FM 音源 3 音+PSG 音源 3 音の演奏が可能です。<br>
WHG 環境(OPN*2)であれば OPNA(FM6+PSG3) を使用した曲を無理やり演奏することもできます。<br> 
<br>
*MUCOM88* 公式サイトはこちら<br>
>https://www.ancient.co.jp/~mucom88<br>

移植には派生版である *mucom88em* を参考にさせていただきました。 <br>
>https://github.com/MUCOM88/mucom88<br>

# 使い方

PC88 版は MML の記述から演奏、音色の編集まで行える統合環境ですが、FM-7 版は演奏機能に特化した、音源ドライバのみの構成となっています。<br>
ドライバの実行やファイルのロードを簡易なものとするため、F-BASIC から扱えるようにしてあります。<br>
以下、エミュレータ環境での使用法について説明します。<br>
<br>
* 用意するもの<br>
  * F-BASIC フォーマット済みディスクイメージ<br>
ここでは MUCOM.D77 とします。<br>
  * d77fileutil (http://ysflight.in.coocan.jp/)<br>
D77 ファイルを読み書きするためのツールです。以下で公開されています。<br>
    >http://ysflight.in.coocan.jp/FM/realutil_j.html<br>
    
  * music<br>
本プロジェクトの bin フォルダの中に入っています。<br>
  * 演奏したい曲オブジェクトファイル<br>
mucom88win で作成された .mub ファイルにあたります。<br>
または、本家 PC88 版 MUCOM88 でコンパイル済みの曲データを抜き出しても OK です。<br>
<br>
  mub ファイルの場合は冒頭に 80byte のヘッダが付くのですがドライバ側で細工してそのまま使えるようにしています。<br>
ただし、ADPCM を使用した.mub ファイルはサイズが大きくなりがちなので、ADPCM データ部を削るなり工夫が必要です。<br>
<br>
  mucom88em でコンパイル済みの曲データから取り出す場合は alpha-dos の拡張メモリ bsave 機能を使い、ホストマシン上では先頭の bsave バイナリヘッダ 4byte を除去するようにしてください。<br>
<br>
  とりあえず、公式サンプルである sampl3.muc のオブジェクトファイル形式である sampl3.mub が手元にあることとします。
<br>
* ディスクイメージへの書き込み<br>
  ```
  d77fileutil MUCOM.D77 -savem music music 0x1000 0x1000
  d77fileutil MUCOM.D77 -savem sampl3.mub sampl3 0x2800 0x2800
  ```
  これでディスクイメージにドライバと曲データが書き込まれます。<br>
  エミュレータで files を取るか、以下でもチェックができます。<br>
  ```
  d77fileutil MUCOM.D77 -files
  ```
* ロードと演奏<br>
  <br>
  F-BASICを立ち上げ、MUCOM.D77 からロードする準備をします。<br>
    ```
    loadm"music"
    ```
    ドライバがロードできたら（ロードアドレス &H1000 を明示してもかまいません）<br>
    ```
    loadm"sampl3"
    ```
    曲データもロードします。同、ロードアドレス &H2800。<br>
    演奏開始の指示は EXEC で行います。<br>
    ```
    exec &H1000
    ```
    無事に演奏が開始されたでしょうか。<br>
    演奏を停止するには BREAK キーを押します。<br>
    停止後は BASIC に戻ってくるはずですが、現状では不具合があり戻ってこない場合もあります。<br>
停止後は先程と同じ要領で別の曲データをロードして演奏することもできます。<br>
# 本家との違い<br>
* CPU が違います。本家に無いバグを仕込んでしまっているかも。<br>
* 音源チップへの入力クロックの違いにより、音程・LFO・テンポなどが若干違います。<br>
    なるべく互換性を保つようにはしています。<br>
* OPNA の機能(HW-LFO やステレオ、リズム音源、ADPCM 音源)は使えません。<br>
* FM/PSG の音量バランスも若干違うと思います。<br>
  
# 謝辞<br>

オリジナル開発者ならびに派生版の開発者の方々、<br>
各種エミュレータや有用なツールの作者の方々に謝辞を捧げます。<br>
