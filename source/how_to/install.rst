Novius OS をインストールする方法
=============================

Translated by `Fumito Mizuno <http://github.com/ounziw>`_.

このガイドは Ubuntu 上で実行する方法です。コマンド類はお使いの OS に合わせてください。

Step 1: サンプルウェブサイトレポジトリのダウンロード
----------------------------------------------

Step 1-1: 事前準備、GIT をインストールする
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

::

	sudo apt-get install git

Step 1-2: ダウンロード
^^^^^^^^^^^^^^^^^^^^

私たちはサブモジュールを使用しているので、それらを適切に取得してください。``--recursive`` オプションを付けると、必要な作業をすべて行ってくれます。

::

    cd ~
    git clone --recursive git://github.com/novius-os/novius-os.git
    sudo mv novius-os /var/www/

このコマンドは、サンプルレポジトリと、2 つのサブモジュールをダウンロードします。

* novius-os : Novius OS のコア、fuel-core や fuel-orm のようなサブモジュールを含む
* local/applications フォルダ無いのいくつかのモジュール: blog, news および comments

Step 2(任意): 使用するバージョンの変更、意欲的な方向け
------------------------------------------------

私たちは、レポジトリのクローンを最新のリリース (執筆時点では rel/0.1) になるように設定しています。

私たちがバージョンをデプロイするとき、新しいブランチを作成します。
現時点では、私たちは、すべての依存するレポジトリを保持しています。したがって、私たちの Github で提供するアプリケーションは、コアと同じバージョン番号になります。 (まだリリースされていませんが) novius-os/core version 0.3 を使用する場合、novius-os/app も同じ version 0.3 を使用する必要があります。

クローンした後に、バージョンを変更するには、 *git サブモジュールを更新するのを忘れないでください。*
'dev' ブランチから最新のナイトリービルドを使用する例です。

::

    cd /var/www/novius-os/
    git checkout dev
    git submodule update --recursive

Step 3: ウェブサイトにアクセスする Apache 設定
------------------------------------------

Step 3-1: Apache の mod_rewrite が有効かどうかの確認
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

::

    sudo a2enmod rewrite

Step 3-2: VirtualHost の設定
^^^^^^^^^^^^^^^^^^^^^^^^^^^

Novius OS 用に新しい VirtualHost を作成します。(nano はお好みのエディタに置き換えてください)

::

    sudo nano /etc/apache2/sites-available/novius-os

以下の設定をファイルにコピーし、保存してください。

::

    <VirtualHost *:80>
        DocumentRoot /var/www/novius-os/public
        ServerName   novius-os
        <Directory /var/www/novius-os/public>
            AllowOverride All
            Options FollowSymLinks
        </Directory>
    </VirtualHost>

デフォルトの設定は、*public* ディレクトリを含んでいます。webroot がこのディレクトリを指すようにしてください。

VirtualHost を有効にする

::

    sudo a2ensite novius-os

Apache を再起動して、新しい設定を有効にします。

::

    sudo service apache2 reload

Step 3-3: hosts ファイルの設定
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

``ServerName`` が ``localhost`` と異なる場合 (上の例では ``novius-os``)、*hosts* ファイルにサーバ名を追加する必要があります。

::

    sudo nano /etc/hosts

以下の行を追加してください。

::

    127.0.0.1   novius-os

Step 4: インストールの完了
-----------------------

大変な部分は完了しました。ここからは簡単です。 :-)

(ブラウザで) http://novius-os/install.php に行き、ウィザードに従ってください。

Step 4-1: 要件の確認
^^^^^^^^^^^^^^^^^^^

赤い表示がたくさん表示されても、心配することはありません。いくつかのディレクトリに書き込み権限が必要なだけです。各ディレクトリの説明が表示されます。

.. image:: /how_to/step-1a.png
	:alt: Step 1a


面倒なら、ページ最下部のコマンドサマリをコピーペーストして (コマンドラインで実行して) ください。

.. image:: /how_to/step-1b.png
	:alt: Step 1b


Step 4-2: MySQL データベースのセットアップ
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

このステップでは、データベースが存在し、関連付けられたユーザーが書き込み権限を持っている必要があります。ホストが ``localhost`` の場合、以下で設定できます。

.. code-block:: sql

    CREATE DATABASE `database_name` DEFAULT CHARACTER SET utf8 COLLATE utf8_general_ci;
    GRANT ALL PRIVILEGES ON `database_name`.* TO 'username'@localhost IDENTIFIED BY 'password';
    FLUSH PRIVILEGES;

設定に合致するように、4 つのフィールドを入力してください。データベースが存在している必要があるので、インストール前にデータベースを作成する必要があります。

.. image:: /how_to/step-2.png
	:alt: Step 2

2 つのファイル *local/config/db.php* と *local/config/crypt.php* を作成します。

Step 4-3: 管理者アカウントの作成
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. image:: /how_to/step-3.png
	:alt: Step 3

Step 4-4: インストールの完了
^^^^^^^^^^^^^^^^^^^^^^^^^

.. image:: /how_to/step-4.png
	:alt: Step 4


Novius OS の開始
---------------

.. image:: /how_to/step-login.png
	:alt: Login Screen

最初にログインすると、アプリケーションマネージャが表示されます (なぜならあなたが管理者だからです)。

これで OS を満喫できます。お楽しみください。