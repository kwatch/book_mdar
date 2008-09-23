## Creating an App (アプリを作ってみる)

One of the best ways to become familiar with a framework is to jump in and get 
your hands dirty.  So now that we've got everything installed, it's time to roll 
up your sleeves and create a test Merb application. 

フレームワークに慣れる最もよい方法のひとつは、自分の手で実際に試してみることです。
必要なものはすべてインストールしたはずですので、袖をまくって Merb アプリケーションをひとつ作ってみましょう。

Merb-more comes with a `gem` called `merb-gen`, this gives you a command line 
tool by the same name which is used for all of your generator needs. You can 
think of it as `script/generate`. Running `merb-gen` from the command line with 
no arguments will show you all of the generators that are available.

Merb-more をインストールすると、`merb-gen` という gem もインストールされます。
この gem は、ジェネレータが必要な場面で使われる同名のコマンドラインツールを提供します〔訳注: うまく訳せない〕。
ちょうど〔訳注: Ruby on Rails における〕 `script/generate` のようなものだと思ってください。
コマンドラインから `merb-gen` を引数なしで実行すると、利用可能なすべてのジェネレータが表示されます。

Merb follows the same naming convention for projects as rails, so 
'my\_test\_app' and 'Test2' are valid names but 'T 3' is not (they need to be 
valid SQL table names).

プロジェクトでは、Merb は Rails と同じ命名規約に則っています。
たとえば 'my\_test\_app' や 'Test2' は有効な名前ですが、'T 3' はそうではありません (これらは SQL において有効なテーブル名である必要があります)。

    merb-gen app test
    
This will generate an empty Merb app, so lets go in and take a look. You'll 
notice that the directory structure is similar to Rails, with a few differences.

このコマンドを実行すると、空の Merb アプリが作成されます。
中を自由に見てみてください。
2、3 の違いはあるものの、ディレクトリ構造は Rails によく似ていることがわかるでしょう。

    # expected output
    RubiGen::Scripts::Generate
      create  log
      create  gems
      create  app
      create  app/controllers
      create  app/helpers
      create  app/views
      create  app/views/exceptions
      create  app/views/layout
      create  autotest
      create  config
      create  config/environments
      create  public
      create  public/images
      create  public/stylesheets
      create  spec
      create  app/controllers/application.rb
      create  app/controllers/exceptions.rb
      create  app/helpers/global_helpers.rb
      create  app/views/exceptions/internal_server_error.html.erb
      create  app/views/exceptions/not_acceptable.html.erb
      create  app/views/exceptions/not_found.html.erb
      create  app/views/layout/application.html.erb
      create  autotest/discover.rb
      create  autotest/merb.rb
      create  autotest/merb_rspec.rb
      create  config/rack.rb
      create  config/router.rb
      create  config/init.rb
      create  config/environments/development.rb
      create  config/environments/production.rb
      create  config/environments/rake.rb
      create  config/environments/test.rb
      create  public/merb.fcgi
      create  public/images/merb.jpg
      create  public/stylesheets/master.css
      create  spec/spec.opts
      create  spec/spec_helper.rb
      create  /Rakefile


### Configuring Merb (Merb の設定構成)

Before we get the server running, you'll need to edit the `init.rb` file and 
un-comment the following line (this is only necessary if you need to connect 
to a database, which we do in our case):

サーバを起動する前に、`init.rb` ファイルを編集して次の行のコメントを外す必要があります
(これはデータベースに接続する場合のみ必要です。本稿でもデータベースに接続するので必要となります)。

`config/init.rb`

    use_orm :datamapper
    
Typing `merb` now in your command line will start the server.

ここまできたら、コマンドラインで `merb` とタイプすればサーバが起動されます。

    Loaded DEVELOPMENT Environment...
    No database.yml file found in /Users/work/merb/example_one/config, assuming database connection(s) established in the environment file in /Users/work/merb/example_one/config/environments
    loading gem 'merb_datamapper' ...
    Compiling routes...
    Using 'share-nothing' cookie sessions (4kb limit per client)
    Using Mongrel adapter

As you can see, however, we did not yet configure the database. Let's create the
database.yml file that merb is looking for:

しかし見ての通り、データベースの設定構成をまだしていませんでした。
Merb が探している database.yml というファイルを作成しましょう:

`config/database.yml`

    # This is a sample database file for the DataMapper ORM
    development:
       adapter: mysql
       database: test
       username: root
       password: 
       host: localhost
	   socket: /tmp/mysql.sock

Don't forget to specify your socket, if you do not know it's location, you 
can find it by typing:

ソケットを指定するのを忘れないようにしてください。
もしソケットがどこにあるのかわからない場合は、次の方法で探すことができます:

    mysql_config --socket

Starting Merb again shows that everything is running okay.

再度 Merb を起動すると、すべてが順調に動きます。

The following command will give you access to the Merb interactive console:

次のコマンドを使うと、Merb のインタラクティブコンソールにアクセスできます:

    merb -i

You'll notice Merb runs on port 4000, but this can be changed with flag 
`-p [port number]`. More options can be found by typing:

Merb はポート 4000 番で実行されますが、`-p [port number]` フラグを使えば変更できます。
オプションの詳細については以下を実行してください:

    merb --help
    
You can even run Merb with any application server that supports rack 
(thin, evented_mongrel, fcgi, mongrel, and webrick):

Merb は、Rack がサポートされていればどのアプリケーションサーバ上でも動作します
(thin、evented_mongrel、fcgi、mongrel、webrick など)。

    merb -a thin

If you see a 500 error with the following error message when trying to navigate
to localhost:4000 in your browser:

ブラウザで localhost:400 にアクセスしたときに、もし 500 エラーが発生して以下のようなエラーメッセージが表示されたら:
    
    undefined method `match' for Merb::Router:Class - (NoMethodError)

This means Merb has been started outside of your applications root directory.

これは Merb が自分のアプリケーションのルートディレクトリ以外の場所から起動されたことを意味します。
