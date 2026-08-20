# VegeKet（ベジケット）

Django で構築した EC サイトです。Udemy 講座を教材に、実務で使われる Web アプリケーション開発の基礎（モデル設計、認証、決済連携など）を学ぶ目的で制作しました。

## 📖 このプロジェクトについて

**教材**：Udemy「【中級者向け・Django4対応】Python/DjangoによるECサイト開発講座（Django3.2系にも対応）」

製造業で10年間、生産管理・品質管理に携わる中で、Excel VBA によるデータ集計自動化や、pandas を使った歩留まり・不良率の可視化に取り組んできました。この経験を活かし、データ分析・Web開発の領域でのキャリアチェンジを目指して、Django を使った実践的な EC サイト構築に取り組みました。

## 🛠 使用技術

- **バックエンド**：Python 3.12 / Django 4.0
- **決済連携**：Stripe API
- **フロントエンド**：HTML / Bootstrap 4.5
- **データベース**：SQLite（開発環境）
- **開発環境**：WSL2 (Ubuntu) / VS Code

## ✨ 実装した機能

- ユーザー認証（サインアップ・ログイン・ログアウト）
- 商品一覧・詳細表示
- カート機能
- Stripe を使ったオンライン決済（テスト環境）
- 注文履歴・プロフィール管理
- Django admin を使った商品・カテゴリ・タグの管理

## 💡 開発でつまずいたポイントと解決アプローチ

講座教材は Django 4.0 系を前提に作られていましたが、実際に自分の環境（Python 3.12 / 最新の Django）で動かすと、バージョン差分による多数のエラーに直面しました。ここでは、その中でも特に学びが大きかったものを紹介します。

### 1. 「動画通りに書いたのに `runserver` が通らない」問題

環境構築時、動画の手順通りに進めているはずなのに `runserver` が何度も失敗しました。代表的なものだけでも以下のような連鎖でした。

- `ModuleNotFoundError: No module named 'stripe.api_resources'`（Stripe SDK のバージョン差による内部モジュール構成の変更）
- Pillow のビルドエラー（`zlib` → `gcc` → `Python.h` と、不足しているビルド依存関係を1つずつ特定して解消）
- `django.core.exceptions.ImproperlyConfigured: Set the SECRET_KEY environment variable`（環境変数ファイルの配置場所の誤認）

**学んだこと**：エラーメッセージの一番下（直接の原因）から順に読み解き、「今何が足りないのか」を1つずつ切り分けて対処する姿勢を身につけました。教材通りに動かない状況は実務でも頻繁に起こるため、この地道な原因特定のプロセスそのものが良い訓練になったと感じています。

### 2. Django フレームワーク特有の書き方への理解不足

`{% block %}` や `{% include %}` などのテンプレート継承の仕組み、Django ORM の `ForeignKey` / `ManyToManyField`、Class-Based View の継承構造など、Django 特有の書き方の意味を理解しないまま写経していた部分がありました。特に、Django 4系以降 `LogoutView` が POST リクエストのみ受け付けるようになった仕様変更に気づかず、ログアウトボタンが 405 エラーを返す原因になったことは印象的な経験でした（`<a>` タグでのリンクから `<form method="post">` への書き換えで解決）。

**学んだこと**：「動くコードを写す」だけでなく、「なぜこの書き方が必要なのか」を1つずつ調べながら進めることの重要性を実感しました。

### 3. HTML の文法を体系的に学ぶ前の写経の難しさ

HTML/CSS を体系的に学習する前にこの講座に着手したため、テンプレートファイルの構造（`<head>` と `<body>` の役割、Bootstrap のクラス設計など）を理解しないままコードを書き写す場面が多くありました。

**学んだこと**：Django のテンプレートは HTML の知識が前提になっている部分が大きいと痛感し、並行して HTML/CSS の基礎学習にも取り組むきっかけになりました。

## 🚀 セットアップ方法

```bash
git clone https://github.com/tnwym2857/vegeket-ec-site.git
cd vegeket-ec-site

python3 -m venv venv
source venv/bin/activate

pip install -r requirements.txt

# secrets/.env.dev を作成し、以下を設定
# SECRET_KEY=（ランダムな文字列）
# DEBUG=True
# ALLOWED_HOSTS=127.0.0.1,localhost
# STRIPE_API_SECRET_KEY=（Stripeのテスト用シークレットキー）
# MY_URL=http://127.0.0.1:8000

python manage.py migrate
python manage.py createsuperuser
python manage.py runserver
```

## 🔭 今後の展望

- 本番環境へのデプロイ（Render を予定）
- pandas を使った EC サイトの売上・在庫データの可視化機能の追加
- テストコードの整備

## 📝 補足

`secrets/.env.dev` には機密情報（SECRET_KEY、Stripe APIキーなど）が含まれるため `.gitignore` で除外しています。動作確認には各自でファイルを作成してください。
