## A little blog (小さなブログ)

#### What will be covered (この節で取り扱う内容)

 * Setting up the example blog application
 * Creating the models
 * Configuring your routes
 * RESTful controllers
 * Some of the view helpers in `merb_helpers`
 * Configuring and sending emails

 * サンプルとなるブログアプリケーションの設定
 * モデルの作成
 * ルーティングの設定構成
 * RESTful なコントローラ
 * `merb_helpers` で定義されているビューヘルパのいくつか
 * 電子メールの送信とそのための設定構成

In the examples, we'll be developing a small blogging application. It's a good
idea to grab the source code from [http://github.com/deimos1986/book_mdar/tree/master/code](http://github.com/deimos1986/book_mdar/tree/master/code), 
so you can follow along with the examples.

サンプルでは、小さいブログアプリケーションを作成します。
ソースコードは [http://github.com/deimos1986/book_mdar/tree/master/code](http://github.com/deimos1986/book_mdar/tree/master/code) から取ってこれますので、サンプルに沿って試すことができます。

First of all, let's define some of the functionality we would expect from any 
blogging application. 

まず最初に、ブログアプリケーションとして考えられる機能をいくつか定義しましょう。

* Publishing posts
* Leaving comments
* Sending email notifications
* Attaching images
* Authentication

* 投稿を公開する
* コメントを残す
* 電子メールで通知する
* 画像を添付する
* 認証する

We're going to call our app `golb`. Think of it as a backward blog. Feel free 
to change the name of your app, but if you do, remember to replace the word
`golb` with the name of your app.

このアプリケーションは、`golb` という名前にすることにします。
これは、`blog` の逆読みです。
アプリの名前は自由に変更して構いませんが、変更した場合は `golb` という単語が出てきたら変更後の名前に置き換えてください。

To make a new app we'll use the command

新しいアプリを作るには、次のコマンドを使います。

    merb-gen app golb

Set up the configuration files for your application, this lets Merb know what 
gems to load for plugins and generators.

自分のアプリケーションの設定構成ファイルを設定してください。
そうすることで、プラグインやジェネレータでどの gems を読み込むのか、Merb は知ることができるようになります。

`config/init.rb`

    use_orm :datamapper

    use_test :rspec

  	dependencies "dm-validations"


Now add a `config/database.yml` file with the following:

また `config/database.yml` ファイルを次のような内容で作成してください:

    ---
    # This is a sample database file for the DataMapper ORM
    development: &defaults
      # These are the settings for repository :default
      adapter:  mysql
      database: golb
      encoding: utf8
      username: root
      password: 
      host:     localhost

      # Add more repositories
      # repositories:
      #   repo1:
      #     adapter:  postgresql
      #     database: sample_development
      #     username: the_user
      #     password: secrets
      #     host:     localhost
      #   repo2:
      #     ...

    test:
      <<:       *defaults
      database: golb_test

      # repositories:
      #   repo1:
      #     database: sample_development

    production:
      <<:       *defaults
      database: golb_production

      # repositories:
      #   repo1:
      #     database: sample_development
  
	---
	    
	    
Note: DataMapper has a rake task to generate a default database.yml file:

注意: DataMapper には、デフォルトの database.yml ファイルを生成する rake タスクが用意されています。
    
    dm:db:database_yaml
    
You can also put a database URI in development.rb (or other environments) just as easily:

また development.rb (または他の環境設定ファイル) にデータベース URI を設定することも簡単です:

    Merb::BootLoader.after_app_loads do
      DataMapper.setup(:default, 'mysql://user:pass@localhost/database')
    end
      
Now we're ready to rock and roll ...

ここまできたら、ロックンロールする準備はできました...
