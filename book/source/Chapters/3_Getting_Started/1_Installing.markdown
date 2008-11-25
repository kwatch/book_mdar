## Getting Started (始めてみる)

〔訳注: ここで説明されている方法はお勧めしません。Ruby 1.8.X と MySQL と RugyGems 1.3.1 をインストールして、which mysql_config でパスが通っていることを確認したあと、gem install merb --development を実行してください。なお Ruby 1.9 や古い RubyGems ではうまく動きません。〕

<a href="http://xkcd.com/303/" target="_blank"> <img src="http://imgs.xkcd.com/comics/compiling.png" alt="XKCD - Compiling"> </a>

Before we get started I'm going to assume you have the following installed:

始めるまえに、みなさんは以下のものをインストールしていることを確認してください:

* [Ruby](http://www.ruby-lang.org/) 
* [RubyGems >= 1.1.0](http://www.rubygems.org/)
* A DBMS (we'll use [MySQL](http://mysql.org/))
* [SVN](http://subversion.tigris.org/) and [git](http://git.or.cz/)

* [Ruby](http://www.ruby-lang.org/) 〔訳注: 現在のMerb は 1.9 では動きません。1.8.X を使ってください〕
* [RubyGems >= 1.1.0](http://www.rubygems.org/) 〔訳注: 1.3.1 以上をインストールしてください〕
* DBMS (本稿では [MySQL](http://mysql.org/) を使います)
* [SVN](http://subversion.tigris.org/) と [git](http://git.or.cz/) 〔訳注: なくても構いません〕

#### What will be covered (取り上げる内容)

* Installing Merb, DataMapper and RSpec
* Creating a temporary test app
* The basic directory structure for the framework

* Merb と DataMapper と RSpec のインストール
* 一時的なテストアプリの作成
* フレームワークの基本的なディレクトリ構造

### The Easy Way (簡単な方法)

If you're on a *nix operating system then keeping up to date with all the edge 
versions of these gems can be made really easy by using the Sake tasks.

もし *nix 系のオペレーティングシステムを使っているなら、Sake タスクを使うことで、これらの gems のバージョンを最新のものに保つのが実に簡単にできます。

Merb sake tasks can be found in merb-more repository under tools directory.
Sake tasks for DataMapper are in dm-dev repository at
http://github.com/dkubb/dm-dev/.

Merb の sake タスクは、merb-more リポジトリの tools ディレクトリ下にあります。
DataMapper 用の Sake タスクは、http://github.com/dkubb/dm-dev/ にある dm-dev リポジトリの中にあります。

To install Sake tasks run sake -i PATH where PATH is path to Sake tasks file
on your local machine. For example,

Sake タスクをインストールするには、自分のローカルマシンで sake -i PATH を実行します (ここで PATH は Sake タスクファイルへのパスです)。

    sake -i ~/dev/opensource/merb/merb-more/tools/merb-dev.rake

To do a fresh clone of all repositories use sake dm:clone and	merb:clone,
respectively. And then to keep up to date you just need to execute:

すべてのリポジトリについてフレッシュクローンを行うなら、それぞれ sake dm:clone と merb:clone を実行してください。
そのあと、Merb と DataMapper の gems を最新版に更新するには、

    sake dm:update

and

と

    sake merb:update

to update Merb and DataMapper gems.

を実行してください。

But what you really want is probably to wipe out Merb and DM gems before update,
do the update and install new updated gems. Use sake merb:gems:refresh and dm:gems:refresh to do so.

しかし、本当に必要としているのはおそらく、Merb と DM の gems を更新するより前にきれいに消し去り、それから最新版の gems をインストールすることだと思います。
そのためには、sake merb:gems:refresh と dm:gems:refresh を実行してください。

### If You're Hardcore (もしツワモノであれば)

#### Installing Merb (Merb のインストール)
***
If you have an older version of Merb (<0.9.2) you should remove all merb and 
datamapper related gems before continuing. Use `gem list` to see your installed
gems. The following command will uninstall the gem you specify:

もし古いバージョン (<0.9.2) の Merb をインストールしているなら、関連するすべての merb と datamapper の gems を事前に削除しておく必要があります。
`gem list` を実行して、自分がイントールした gems を確認してください。
次のコマンドを使うと、指定した gem をアンインストールすることができます。

    sudo gem uninstall the_gem_name
***
Installing the `merb` gems should be as simple as:

`merb` gems をインストールするのは非常に簡単です:
    
    sudo gem install merb --source http://merbivore.org
    
*or for JRuby:*

*または、JRuby を使っているなら:*
    
    jruby -S gem install merb mongrel 
    
__Unfortunately__ we are living right on the edge of development so we'll need 
to get down and dirty with building our own gems from source. Luckily this is 
much easier than it sounds... 

__残念ですが__、本稿では最新の開発版を使っているため、ソースから自分の gems を構築するという汚れ仕事をしなければなりません。
幸運にも、これは思った以上にずいぶんと簡単です...

Start by installing the `gem` dependancies:

まずは依存するものを `gem` でインストールします:

    sudo gem install rack mongrel json erubis mime-types rspec hpricot \
        mocha rubigen haml markaby mailfactory ruby2ruby

*or for JRuby:*
*または JRuby を使っているなら:*

    jruby -S gem install rack mongrel json_pure erubis mime-types rspec hpricot \
        mocha rubigen haml markaby mailfactory ruby2ruby

Then download the `merb` source:
そして `merb` のソースをダウンロードします:

    git clone git://github.com/sam/extlib.git
    git clone git://github.com/wycats/merb-core.git
    git clone git://github.com/wycats/merb-plugins.git
    git clone git://github.com/wycats/merb-more.git

Then install the gems via rake:
それから rake を使ってこれらの gems をインストールします:

    cd extlib ; rake install ; cd ..
    cd merb-core ; rake install ; cd ..    
    cd merb-more ; rake install ; cd ..
    cd merb-plugins; rake install ; cd ..

Note that Merb and DataMappers share Extlib library since after 0.9.3 release of DM.
The `json_pure` gem is needed for merb to install on [JRuby](http://jruby.codehaus.org/) (Java implementation of a Ruby Interpreter), otherwise use the `json` gem as it's faster.

注意事項ですが、Merb と DataMapper は、DM 0.9.3 リリース以降は Extlib ライブラリを共有します。
Merb を [JRuby](http://jruby.codehaus.org/) (Ruby インタプリタの Java 実装) にインストールするときは、`json_pure` gem が必要になります。
それ以外では、より高速な `json` gem を使いましょう。

Merb is ORM agnostic, but as the title of this book suggests we'll be using 
DataMapper. Should you want to stick with ActiveRecord or play with Sequel, 
check the [Merb documentation](http://merb.rubyforge.org/files/README.html) for install instructions.

Merb では ORM を自由に選べますが、本稿のタイトルが示すように、ここでは DataMapper を使います。
もし ActiveRecord に固執したり、Sequel で遊んでみたいという場合は、[Merb documentation](http://merb.rubyforge.org/files/README.html) を読んでインストール手順を確認してください。

#### Installing DataMapper (DataMapper のインストール)

***
DataMapper has spit into the gems `dm-core` and `dm-more`, the old `datamapper` 
gem is now outdated.

DataMapper は、`dm-core` と `dm-more` という、2 つの gems に分かれています。
`datamapper` という gem は古いので、使わないでください。

If you have an older version of `datamapper`, `data_objects`, or `do_mysql`, 
`merb_datamapper` (< 0.9) you should remove them first.

もし `datamapper` や `data_objects` や `do_mysql` や `merb_datamapper` (< 0.9) といった古いバージョンをインストールしているなら、まず最初にこれらを削除してください。
***

We will use MySQL in the following example, but you can use either sqlite3 or 
PostgreSQL, just install the appropriate gem. You will also need to ensure that 
MySQL is on your system path for the gem to install correctly.

本稿では以降のサンプルに MySQL を使用しますが、適切な gem をインストールすれば、sqlite3 や PostgreSQL を使うことも可能です。
また gem が正しくインストールされるために、MySQL が自分のシステムパスに存在することを確認する必要があります。

To get the gems from source:

ソースから gems を構築するには:

    git clone git://github.com/sam/extlib.git  
    git clone git://github.com/sam/do.git
    
    cd extlib
    rake install ; cd ..
    cd do
    cd data_objects
    rake install ; cd ..
    cd do_mysql  # || do_postgres || do_sqlite3
    rake install

    git clone git://github.com/sam/dm-core.git
    git clone git://github.com/sam/dm-more.git

    cd dm-core ; rake install ; cd ..
    cd dm-more
    rake install
    
To update a gem from source, run `git pull` and `rake install` again.

ソースから gem を更新するには、`git pull` と `rake install` を再度実行してください。

#### Install RSpec (RSpec のインストール)

The `rspec` gem was installed in the Merb section above. However, if you want 
to grab the source, run one of the following commands:

`rspec` の gem は、先の Merb セクションでインストールされています。
しかし、もしソースからインストールしたいなら、次のコマンドのうちどちらかを実行してください:

    gem install -r rspec
    
    # or
    
    git clone git://github.com/dchelimsky/rspec.git
    cd rspec
    rake gem
    sudo gem install pkg/rspec-*.gem
