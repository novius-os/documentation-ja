アプリケーション
=================

アプリケーションは、Novius OS に機能を追加するもっとも簡単な方法です。いくつかのアプリケーションが利用できます。Blog, News stories, Slideshows そしてサンプルアプリケーションの `Monkeys <https://github.com/novius-os/noviusos_monkey>`_ 。

アプリケーション管理システムは、アプリケーションを本当に簡単に追加し、拡張することができるように実装されています。

* :ref:`applications_files-organisation`
* :ref:`applications_metadata-configuration-file`
* :ref:`applications_appdesk`

.. _applications_files-organisation:

Files organisation
------------------

ファイルは、FuelPHP のアプリケーションと同じ方法で纏められます。

.. image:: /technical/files_organisation.png

.. _applications_metadata-configuration-file:

Metadata configuration file
---------------------------

メタデータ設定ファイルは必須です。アプリケーションを使用するのに必要な情報をコアに通知します。Monkeys アプリケーションのメタデータファイルです。

.. code-block:: php

	<?php
	return array(
		'name'    => 'Monkey : Novius OS Application Bootstrap',
		'version' => '0.1',
		'icon16' => 'static/apps/noviusos_monkey/img/16/monkey.png',
		'provider' => array(
			'name' => 'Provider',
		),
		'namespace' => 'Nos\Monkey',
		'launchers' => array(
			'provider_launcher' => array(
				'name'    => 'Monkey',
				'url' => 'admin/noviusos_monkey/appdesk',
				'iconUrl' => 'static/apps/noviusos_monkey/img/32/monkey.png',
				'icon64'  => 'static/apps/noviusos_monkey/img/64/monkey.png',
			),
		),
		'enhancers' => array(
			'noviusos_monkey' => array(
				'title' => 'Monkey',
				'desc'  => '',
				//'enhancer' => 'noviusos_monkey/front',
				'urlEnhancer' => 'noviusos_monkey/front/main',
				'iconUrl' => 'static/apps/noviusos_monkey/img/16/monkey.png',
				'previewUrl' => 'admin/noviusos_monkey/application/preview',
				'dialog' => array(
					'contentUrl' => 'admin/noviusos_monkey/application/popup',
					'width' => 450,
					'height' => 200,
					'ajax' => true,
				),
			),
		),
	);

メタデータ設定ファイルは、key => value 配列を返します。

* ``name``: アプリケーション名
* ``version``
* ``icon16``: アプリケーションを示す 16x16 サイズのアイコン (タブで使用する)
* ``provider``: プロバイダ情報を示す key => value 配列
* ``namespace``: アプリケーションの全てのクラスの名前空間
* ``launchers``: ランチャーについての情報を示す key => value 配列 (デスクトップのアイコンはアプリケーションを新しいタブで開く)

  ``key`` はランチャーの識別子

  value は key => value 配列

    * ``name``
    * ``url``: アプリケーションを開くときに呼び出す url
    * ``iconUrl``: アプリケーションを開くタブで用いる 32x32 サイズのアイコン
    * ``icon64``: デスクトップの 64x64 サイズのアイコン

* ``enhancers``: [[Enhancers | (EN) Enhancers]] を参照
* ``data-catchers``: [[Sharing | (EN) Sharing]] を参照
* ``template``: 登録されたテンプレートを示す key => value 配列

	.. code-block:: php

		<?php
			'templates' => array(
				'top_menu' => array(
					'file' => 'noviusos_templates_basic::top_menu',
					'title' => 'Default template with a top menu',
					'cols' => 1,
					'rows' => 1,
					'layout' => array(
						'content' => '0,0,1,1',
					),
				),
				// ...
			),

各テンプレートは、いくつかのパーツに分けられます。全てを一つの場所で表示する標準テンプレートも可能ですが、例えば左右サイドバーを持つ複雑なテンプレートも可能です。Novius OS にこの情報を提供することで、コアシステムが複数の wysiwyg を管理できるようにしよう、という考えです。Wysiwygs はグリッドで表示されます。wysiwyg の位置と大きさを選択することができます。

	* ``file``: テンプレートの場所
	* ``title``: テンプレートのタイトル、ページを作成/編集し、そのページのテンプレートを選択するときに使用する。
	* ``cols``: グリッドのカラム数
	* ``rows``: グリッドの行数
	* ``layout``: グリッドの wysiwygs レイアウト。key => value 配列

		* key はテンプレート識別子
		* value は文字列、左からの位置、上からの位置、幅、高さをカンマで区切る。

.. _applications_appdesk:

App Desk
--------

App Desk は、アプリケーションデータを簡単にリストし、フィルタすることができます。:ref:`App Desk <en:appdesk_general-informations>` を参照
