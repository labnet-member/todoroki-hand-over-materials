# 目次

- [1. 概要](#1-概要)
- [2. 開発環境の構築手順](#2-開発環境の構築手順)
  - [ローカルのマシンにインストール](#ローカルのマシンにインストール)
  - [リモートのマシンにインストール](#リモートのマシンにインストール)
- [3. ローカル環境での実行手順](#3-ローカル環境での実行手順)
  - [ブローカの実行](#ブローカの実行)
  - [クライアントの実行](#クライアントの実行)
- [4. クラウド環境での実行手順](#4-クラウド環境での実行手順)
  - [環境構築](#環境構築)
    - [ECS のクラスタ作成](#ecs-のクラスタ作成)
    - [ECS の実行に必要な Role の作成](#ecs-の実行に必要な-role-の作成)
    - [EC2 インスタンスの作成](#ec2-インスタンスの作成)
    - [タスク定義の作成](#タスク定義の作成)
  - [実験](#実験)
    - [Cloud Map の作成](#cloud-map-の作成)
    - [ECS サービスの起動](#ecs-サービスの起動)
    - [ECS タスクの起動](#ecs-タスクの起動)

# 1. 概要

このドキュメントは轟木が修士論文で実装したソフトウェアを動かすための手順書になる．修士論文では [MQTT Version5.0][MQTT V5] の共有サブスクリプション機能に動的負荷分散を実装しており，提案手法の実装には Go 言語で実装されたブローカである [Mochi MQTT Broker][Mochi] と，Java 言語で実装されたクライアントである [paho.mqtt.java][paho] を利用している．ソフトウェアの開発には JetBrains が開発している IDE を利用している．本資料ではソフトウェアを実行するための環境を構築し，検証を行えるまでの手順を説明する．

使用したツール一覧

- ライブラリ
  - Mochi-MQTT Server
    - v2.4.0
  - paho.mqtt.java
    - v1.2.5
- IDE
  - JetBrains Toolbox
  - IntelliJ IDEA
  - GoLand
  - Gateway

[MQTT V5]: https://docs.oasis-open.org/mqtt/mqtt/v5.0/mqtt-v5.0.html
[Mochi]: https://github.com/mochi-mqtt/server
[paho]: https://github.com/eclipse/paho.mqtt.java

# 2. 開発環境の構築手順

まず，JetBrains の製品を利用するためにアカウントの作成と学生向け無料ライセンスの申請を行う．[申請方法](https://blog.jetbrains.com/ja/2019/08/22/2105/)の内，「1.教育機関の公式メールアドレスで申し込む」か「3.学生証明書・教職員カードまたはその他の公式文書で申し込む（この場合は処理に数日かかることがあります）」で申請できるはず．申請が承認されると JetBrains の全ての製品を無料で利用できるようになる．

申請の承認が完了後，IDE 製品をインストールしていく．IDE のインストールには [JetBrains Toolbox App](https://www.jetbrains.com/toolbox-app/) (以下，Toolbox という)を利用する．これは JetBrains 製品を管理するためのツールであり，以下の画像のようにバージョン管理や新しい製品のインストールを容易に実現できる．インストール方法は HP から OS に合わせたインストーラーをダウンロードして実行する．

![JetBrains Toolbox App](images/section_2/Toolbox.png)

Toolbox のインストール完了後は IDE のインストールを行う．今回の実装で必要なのは以下の 2 つ．

- [Intellij IDEA](https://www.jetbrains.com/ja-jp/idea/)（Java 言語）
- [GoLand](https://www.jetbrains.com/ja-jp/go/)（Go 言語）

ここで，インストール先の選択肢が 2 つある．

- ローカルマシンにインストール
- リモートマシンにインストール

## ローカルのマシンにインストール

ローカルのマシンにインストールする場合，Toolbox から指定した IDE をインストールするだけである．インストールが完了後，Toolbox の IDE を選択すると起動する．

## リモートのマシンにインストール

リモートにインストールする場合，Toolbox から [Gateway](https://www.jetbrains.com/ja-jp/remote-development/gateway/) をインストールする．Gateway はリモートマシン上に IDE を起動し，Gateway 経由で IDE を操作することができる．Gateway を起動すると以下のような画面になり，<span style="color: red">赤枠の領域</span>がリモートの選択肢である．今回は SSH を利用する．そのため，リモートのサーバに SSH 接続するための設定を事前に行っておく必要がある．これ以降は SSH の設定が既に完了していることを前提にする．

まず，Gateway を起動した画面の「New Project」をクリックする（画像にはないが右上にある）．

![Gateway Top](images/section_2/Gateway_top.png)

以下の SSH 接続画面が表示されるため，「New Connection」，「ユーザー名」，「IP アドレス」，「秘密鍵のパス」を指定して次に．

![Gateway SSH](images/section_2/Gateway_SSH.png)

リモートで起動する IDE と起動時のディレクトリを指定して IDE を起動する．IDE は EAP（Early Access Program）以外を指定した方が良い．

![Gateway IDE](images/section_2/Gateway_IDE.png)

# 3. ローカル環境での実行手順

この節では 1 台のマシン上でブローカとクライアント実行する手順を示す．この手法は提案手法の改善や再検証などの場面に適している．以下では，ブローカとクライアント（パブリッシャ，サブスクライバ）の実行に必要なファイルや設定をそれぞれ説明する．

## ブローカの実行

## クライアントの実行

# 4. クラウド環境での実行手順

この節では提案手法を AWS ECS で検証した手順を示す．ただし，以下に示す手順は実験時（2024/2/29）で行った操作であり，このマニュアル作成時点（2024/3/16）でも正しく動作するかは保証できないため，コマンドが失敗する場合は随時インターネットでの検索で解決するようにお願いする．

## 環境構築

### ECS のクラスタ作成

### ECS の実行に必要な Role の作成

### EC2 インスタンスの作成

### タスク定義の作成

## 実験

### Cloud Map の作成

### ECS サービスの起動

### ECS タスクの起動
