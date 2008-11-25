## The Framework (フレームワーク)

The directory structure of the project created should look like the following. 
We'll give brief overview of the framework here and go into further details of 
each component in subsequent chapters.

作成されたプロジェクトのディレクトリ構造は、次のようになっています。
ここではフレームワークの簡単な概要を説明し、次節以降で各コンポーネントの詳細を説明します。

	test
	  |--> app
	  |--> autotest
	  |--> config
	  |--> log
	  |--> public
	  `--> spec

The `app` folder contains your models, views, controllers and helpers. It also 
has Parts, they inherit from `AbstractController` and similar to the old Rails 
components, but are lightweight and are useful for sidebars, widgets etc. 

`app` フォルダの下に、models、views、controllers、helpers〔訳注: の各フォルダ〕があります。
また `app` フォルダは Parts も含んでいます。
これは `AbstractController` を継承しており、古い Rails コンポーネントに似ていますが、軽量で、サイドバーやウィジットなどに役立ちます。

`Mailers`, which also inherit from the `AbstractController` have their own 
folder where the controllers and views live. 

`Mailers` もまた `AbstractController` を継承しています。
`Mailers` は自身のフォルダを持っており、コントローラとビューをそこに置いています。

	app
	  |--> controllers
	  |--> models (generated with a model)
	  |--> helpers
	  |--> mailers (generated with a mailer)
	  |--> helpers
	  |--> parts (generated with a parts controller)
	  `--> views


The `config` folder has all the configuration files and environments. It's 
important to edit the `init.rb` and `database.yml` files in here before 
running Merb. 

`config` フォルダには、設定構成ファイルと環境のすべてが収められています。
このうち、`init.rb` と `database.yml` のファイルは Merb を起動する前に編集する必要があります。
<!-- Merb を起動する前に、ここにある `init.rb` と `database.yml` のファイルを編集することは重要です。 -->

The Merb router, which maps the incoming requests to the controllers is also 
here. The `rack.rb` file is the rack handler and you can pass options to 
`merb -a` to change rack adapter.

Merb のルータは、受信したリクエストをコントローラに対応づける役目を持っていますが、それもここにあります。
`rack.rb` ファイルは Rack ハンドラであり、`merb -a` にオプションを渡して Rack アダプタを変更することができます。

	config
	  `--> environments

RSpec specs can be found in the spec folder.

RSpec による仕様 (スペック) は、spec フォルダに置かれます。

	spec
	
In addition to these folders you can have a `gem` directory, which stores 
frozen gems (see Freezing Gems for more info), and a `lib` folder to store 
other ruby files.

これらのフォルダに加え、凍結した gems (詳細は『Gems を凍結する』を参照してください) を置くための `gem` ディレクトリや、他の Ruby ファイルを置くための `lib` フォルダを持つことができます。
