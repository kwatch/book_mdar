## What's Merb, DataMapper & RSpec? (Merb と DataMapper と RSpec について)

> If you're not living on the edge, you're taking up too much room. - Alice Bartlett

> もし端っこに住んでいないなら、あなたはたくさんの部屋を専有しすぎている。〔訳注: ???〕- Alice Bartlett

Merb, DataMapper and RSpec are all open source projects that are great for 
building kick-ass web applications. They are all in active development and 
although it can be hard, we'll try our best to keep up-to-date.

Merb と DataMapper と RSpec はどれも、すごい Web アプリケーションを構築するのにとても役に立つオープンソースプロジェクトです。
3 つとも活発に開発中であり、そのため最新版についていくのは大変なのですが、最大限努力してみます。


### [Merb](http://merbivore.com/)

Merb is a relatively new web framework with an initial 0.0.1 release in October
2006.  [Ezra Zygmuntowicz](http://brainspl.at/) is Merb's creator, and 
continues to actively develop Merb along with a dedicated development team at 
[Engine Yard](http://www.engineyard.com) and many other community contributors.  

Merb は比較的新しい Web アプリケーションフレームワークであり、最初のバージョン 0.0.1 がリリースされたのは 2006 年 10 月です。[Ezra Zygmuntowicz](http://brainspl.at/) が Merb の作者であり、[Engine Yard](http://www.engineyard.com) の熱心な開発チームや他のコミュニティメンバとともに、活発な開発を継続しています。

Merb has obvious roots and inspiration in the 
[Ruby on Rails](http://www.rubyonrails.com) web framework.  If you know Ruby and
have used Rails you're likely to get the hang of Merb quite easily. 

Merb は明らかに [Ruby on Rails](http://www.rubyonrails.com) を源流とし、また影響を受けています。
もし Ruby を知っていて Rails を使ったことがあるなら、Merb のコツを得るのも簡単でしょう。

While there are similarities, Merb is not Ruby on Rails.  There are core 
differences in design and philosophy.  In many areas that Rails chooses to be 
opinionated, Merb is agnostic - with respect to the ORM, the JavaScript library 
and template language. The Merb philosophy also disbelieves in having a monolithic 
framework. Instead, it consists of a number of gems: `merb-core`, `merb-more` and 
`merb-plugins`. This means that it is possible to pick and choose the 
functionality you need, instead of cluttering up the framework with non-essential 
features. 

両者に似た点はあるものの、Merb は Ruby on Rails ではありません。
設計と哲学に、明確な違いがあります。
Rails には意固地な点が多々ありますが、Merb はそうではありません - たとえば ORM や、JavaScript ライブラリや、テンプレート言語についてがそうです。
Merb の哲学では、モノリシック (一枚岩) なフレームワークに対すして不信感を持っています。
かわりに、Merb は複数の gems から構成されています: `merb-core` と `merb-more` と `merb-plugins` です。
つまり、本質的でない機能でフレームワークをごちゃごちゃにするのではなく、自分が必要とする機能を取捨選択することができるというわけです。

The `merb` gem installs both `merb-core` and `merb-more`; all you need in order to 
get started straight away.  The benefit of this modularity is that the framework 
remains simple and focused with additional functionality provided by gems.

`merb` gem をインストールすると、`merb-core` と `merb-more` もインストールされます;
すぐに開始するのに必要なものはすべてインストールされます。
このモジュール性の高さによる利点は、付加的な機能は gems で提供することで、フレームワークをシンプルかつ〔訳注: 必要な機能だけに〕集中した状態に保たれることです。

Thanks to Merb's modularity, you are not locked into using any particular 
libraries. For example, Merb ships with plugins for several popular ORMs and 
provides support for both Test::Unit and RSpec.

Merb のモジュール性の高さのおかげで、どの特定のライブラリにも縛られることはありません。
たとえば、Merb は複数の人気のある ORM 用プラグインを同梱しており、また Test::Unit と RSpec の両方をサポートしています。

`merb-core` alone provides a lightweight framework 
(a la [camping](http://code.whytheluckystiff.net/camping/)) that can be used to 
create a simple web app such as an upload server or API provider where the 
functionality of an all-inclusive framework is not necessary.

`merb-core` はそれ自体で軽量なフレームワーク ([camping](http://code.whytheluckystiff.net/camping/) 流の) であり、たとえばアップロードサーバや API プロバイダのような、必ずしもすべての機能を備えたフレームワークを必要としないような、シンプルな Web アプリを作成するのに使うことができます。

### [DataMapper](http://datamapper.org/)

DataMapper is an Object-Relational Mapper (ORM) written in Ruby by Sam Smoot. 
We'll be using DataMapper with Merb. As previously mentioned, Merb does not require 
the use of DataMapper.  You can just as easily use the same ORM as Rails 
(ActiveRecord) if you prefer.

DataMapper は、Sma Smoot によって作られた、Ruby で実装されたオブジェクト-リレーショナルマッパー (ORM) です。
本稿では、DataMapper を Merb で使います。
前述したように、Merb は必ずしも DataMapper を必要としません。
Rails と同じ ORM (ActiveRecord) のほうが好きなら、そちらを使うのも簡単にできます。

We have chosen to use DataMapper because of it's feature set and performance. One
of the differences between it and ActiveRecord that I find useful is the way 
database attributes are handled. The schema, migrations and attributes are all 
defined in one place: your model. This means you no longer have to look around in 
your database or other files to see what is defined.  

本稿では、機能セットとパフォーマンスを考慮し、DataMapper を選択しました。
DataMapper と ActiveRecord の違いのうち私が有用だと思ったことのひとつは、データベースの属性の扱いについてです。
〔訳注: DataMapper では、〕スキーマとマイグレーションと属性は一カ所で管理されます: つまり、自分のモデルクラスです。
これにより、データベースや他のファイルを見て何が定義されているかを調べる必要がなくなります。

While DataMapper has similarities to ActiveRecord, we will be highlighting the 
differences as we go along.

DataMapper は ActiveRecord に似ていますが、本稿では両者の違いを強調するようにします。


### [RSpec](http://rspec.info/)

RSpec is a Behaviour Driven Development framework for Ruby. It consists of two
main pieces, a Story framework for integration tests and a Spec framework for
object tests. Both these components are implemented as Domain Specific
Languages which help to make the stories and specs created more readable.

RSpec は、Ruby 用の振る舞い駆動開発フレームワークです。
RSpec は主に、統合テストのためのストーリーフレームワークと、オブジェクトテスト〔訳注: 単体テストのことと思われる〕のための仕様フレームワークの、2 つの部分から構成されています。
これら両方のコンポーネントが特定ドメイン向け言語 (DSL) で実装されており、これによりストーリーと仕様がより読みやすくなります。

Merb currently supports the Test::Unit and RSpec testing frameworks. Both Merb
and Datamapper use the RSpec testing frameworks and so we will be covering some
aspects so that you may use it for your own applications.

Merb は現在、Test::Unit と RSpec の両方のテスティングフレームワークをサポートしています。
Merb と DataMapper は両方とも RSpec テスティングフレームワークを使っているので、自分自身のアプリケーションで RSpec を使うためのいくつかの側面について、本稿でも取り上げています。
