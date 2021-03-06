# 体調確認メール回答自動化ツール
## ツールについて
体調確認メールへの回答を自動化するツールである。業務時間外に暇つぶしで作ったものです。
- 環境
    - Gmailにメールが届く
    - Winodws10
- 回答の手順は以下のとおりである。
    1. IMAPを使って、Gmailに届いた体調確認メールに書かれたURLを取得する。
    1. SeleniumでGoogleChromeを自動操作する。  
    1. 回答結果のスクリーンショットを`png`ディレクトリに保存する。  
        `logger.log`を`log`ディレクトリに出力する。

- 回答は朝6:30に自動で行われる。
- 回答内容は以下のようになっているので、それ以外の場合(体調不良、年次休暇、出社など)は手動で入力する必要がある。
    ```
    1. いいえ
    2. いいえ
    3. 営業日：テレワーク
       休日：休み
    4. オフィス在籍
    5. 自宅(テレワーク)
    ``` 

- 祝日判定は[jpholiday](https://pypi.org/project/jpholiday/)を使っているので必ずしも会社の休日と一致するとは限らない。

## Gmail API
~~[GmailAPI公式ドキュメント](https://developers.google.com/gmail/api/guides?hl=ja)を参照~~  #申請方法が分からず途中で断念,IMAPを使う方法に変更

## IMAP
[IMAP公式ドキュメントを参照](https://docs.python.org/ja/3/library/imaplib.html)  
- GmailでIMAP機能を有効にする必要がある([Gmailヘルプ](https://support.google.com/mail/answer/7126229?hl=ja))  
- [Googleログインページ](https://myaccount.google.com/u/1/?tab=kk)から
2段階認証プロセスを有効にして、アプリパスワードを発行する。
    - 上記の方法を取らなくてもいけるが、その場合[Googleログインページ](https://myaccount.google.com/u/1/?tab=kk)から"セキュリティー→安全性の低いアプリのアクセスをオンにする"とする必要があり、セキュリティー的にあまり良くなさそう。

## ChromeDriver
http://chromedriver.chromium.org/downloads
から、GoogleChromeのバージョンに合った
chromedriver.exeをダウンロードして、`driver`ディレクトリに格納する。

## Selenium
- [Selenium公式ドキュメント](https://kurozumi.github.io/selenium-python/locating-elements.html)を参照  
- ログが出力される場合[この方法](https://stackoverflow.com/questions/47755571/how-i-can-hide-chromedriver-log-on-console-through-selenium-p)で非表示にすることが出来る

## 実行方法
Windowsのタスクスケジューラーを使って毎日6:30に`main.py`を実行する。([タスクスケジューラにpythonを登録する方法](https://qiita.com/__init__/items/95ac1d45858b8b24a59c)、[スリープ時の実行方法](https://faq.nec-lavie.jp/qasearch/1007/app/servlet/relatedqa?QID=020406)を参照)  
尚実行前に、`sample.envvar.py`で環境変数を指定して、`envvar.py`に名前変更しておく必要がある。
