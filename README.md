# Django Introductory and Basic
### Django
- project (startproject) と app (startapp)
  - 統括 (project) とその下にある アプリ:機能 (startapp)
- urls.py と views.py
  - リクエストを受け取る (urls.py) / レスポンスを送る (views.py)
- function based view と class based view
  - django で view を記載する場合は大きく2つの方法がある
- **リクエストとレスポンス　=　django の全ての base となる概念**
### Hello world を表示させるアプリ
- django を使用して Hello world アプリを作成する
- 仮想環境を使用(venv : ベンブ)
- 今回は、自分のPCからだけ立ち上げる事ができる web server を作成
  - djago を使用する事によって簡単に web server を立ち上げる事ができる

## 仮想環境構築
#### terminal に下記のコマンド実行
    python3 -m venv venv
- 何かしらの directory 作成後に、directory を editor で開き、作成した directoryの中に venv の仮想環境を構築する　
#### 仮想環境の入る
    source venv/bin/activate
- 仮想環境に入ると (venv : ベンブ) の表示がされる
  - 基本的に作業は仮想環境内で行う
  - 仮想環境内に入っていないと、python, django が実行されないので（使用できない）ので注意する
#### 仮想環境から抜ける
    　deactivate
## プロジェクト作成
    pip install django
#### 1. django upgrade する
    pip install --upgrade pip
#### 2. プロジェクト名
    django-admin startproject < directory名 > .
- directory 名はその時によって変更
#### 3. プロジェクト directory に降りて下記のコマンド実行
    python manage.py runserver
- Starting development server at http://127.0.0.1:8000/
- server が立ち上がっている事を確認
### runserver (127.0.0.1.:8000)
- localhost = 127.0.0.1.:8000 = 自分の PC brawser
- 8000 = ポート
## urls.py と views.py の役割
- server から http リクエスト
　↓
- urls.py　->　リクエスト object を受け取る
　↓
- views.py　->　 urls.py が受け取ったリクエスト object を呼び出していく
　↓
-  views.py が http レスポンス object を server に返していく
### veiw file 作成のルール
function Based View も class Based viwe も出来るもの（完成された物）に大きな違いはない
#### **function Based View**
 - 昔ながらの台所（キッチン）= 手間がかかる
 - 自由度が高い。技量によってなんでも作れる。簡単に変更・修正・工夫が出来る
- 設計をして一から code を書いていくイメージ
  - 手間はかかるが、中で何がどう動いているか詳細がわかる
#### **class Based View**
- 今風のシステムキッチン = 自動で機械がしてくれるので簡単に出来る
- 自動調理中なので、あまり中身に手を加える事ができない。臨機応変に対応きない
- django が準備をしてくれて簡単い実装できるイメージ
  - 簡易的で手間が掛からない一方…ブラックボックス化しやすい
## Django 内アプリ作成
- EC site でれば
  - project (urls.py, settings.py )
    - pay : 支払い：(urls.py, views.py, models.py, pay.html )
    - item : 商品一覧: (urls.py, views.py, models.py, item.html )
    - contact : お問い合わせ : (urls.py, views.py, models.py, contact.html )
- 上記のように各処理系統によって、まとまっている
  - 例）
  - 1. project urls.py で http リクエストを受け取る
  - 2. urls.py から item views.py に指示を送る
  - 3. item の中で models.py と html など、必要な処理、モノを取得
  - 4. item views.py が http レスポンスをする
### App 作成コマンド
    python manage.py startapp < directory名 >
- manage.py と同じ階層に作成
- app を作成したら django にこんなアプリケーション作成しましたよと指示を出す
  - project の settings.py  **INSTALLED_APPS** に作成したアプリを記述
  - 例）
  - **sampleapp.apps.SampleappConfig**
  - apps.頭文字は大文字appConfig ※ コンフィグの頭文字も大文字
### App 内 file について
- __ init __.py　->　python で package を作成時に自動で作成される
  - import, export 時に必須なので package には必ず付ける。自動で作成される
- admin.py　->　管理画面
- apps　->　アプリの名前、シグナル
- models　->　DB（データベース）data の整理、作成
  - ここでは、data の設計図を記述していく
- tests.py　->　django のテスト code を記述していく
  - 実際の deploy では、project の内容にそってテスト code を記述する事
- views.py　->　DB に対する指示書
#### ※ urls.py は最初に入ってないが、App ないに新規で作成する
- ある url を受け取ったらこのアプリに繋ぎ込みをしていく役割が必要なので
- 一般的な開発でもそうしている
#### ※ project 作成時になぜ views.py が作成されなかったのか？
- django では基本的に project は統括をしている所だから
  - project 内の urls.py で http リクエストを受け取り、App の views.py にリクエスト情報を送る統括的な役割
  - 細かい指示は App の views.py file で処理を行うという形になっている
### App を繋ぐ
- **project で受けた url はそのまま App に引き継がられない**
- project で受け取った url は省略（消されてしまう）
  - App urls.py で重複しないように注意する