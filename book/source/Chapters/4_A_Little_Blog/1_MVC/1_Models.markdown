## Models (モデル)

(TODO) - rewrite for DM 0.9 almost done, just need to finish it!

### Getting started (始めてみる)

Building a model with Merb and DataMapper requires generating a model,
specifying attributes (properties), and running a migration to create the
database table and all the properties. Generating a model is similar to Rails,
as is running a migration. But unlike ActiveRecord, DataMapper does not use
migration files to define the model.

Merb と DataMapper を使ってモデルを構築するには、モデルを生成し、属性 (プロパティ) を指定し、マイグレーションを実行してデータベーステーブルとすべてのプロパティを生成する必要があります。
モデルを生成するのとマイグレーションを実行するのは Rails に似ていますが、ActiveRecord と違い、DataMapper はモデルを定義するためのマイグレーションファイルは使いません。

Instead, properties are defined in the model itself. This allows you to easily
see how your models map to the database and removes the headache of trying to
use separate migration files (when you have conflicting or irreversible migrations).

そうではなくて、プロパティはモデルそのものに定義されます。
これにより、自分のモデルがどうデータベースに対応付けされるかが簡単にわかるので、マイグレーション用の別ファイルを使うことによる頭痛のタネ (マイグレーションが衝突したり取り消せなかったり) をなくすことができます。

#### The Model Generator (モデルジェネレータ)

DataMapper has a model generator just as Rails does:

DataMapper は、Rails と同じようなモデルのジェネレータを備えています:

    merb-gen model post

This will make a post model for you, provided that you have defined an ORM
and the database golb, in the previous steps.

これを実行すると、前の手順で定義した ORM とデータベース golb を使って、post モデルが生成されます。

Note: Sometimes you might prefer to directly create a _resource_ (Model, Controller, View) instead of calling the generator tree times:

注意: _リソース_ (モデル、コントローラ、ビュー) を生成する場合は、ジェネレータを 3 回呼び出すよりも、直接リソースを生成したほうがいいでしょう:

    merb-gen resource post


#### Properties (プロパティ)

So DataMapper models differ a bit from ActiveRecord models as previously
stated. Defining the database columns is achieved with the `property` method.

前述したように、DataMapper のモデルは ActiveRecord のそれとは少し違います。
データベースカラムの定義は `property` メソッドを実行することで行います。

`app/models/post.rb`

    property :title,  String, :lazy => false

This is the `title` property of the post model. As we can see, the parameters
are the name of the table column followed by the type and finally the options.

これは post モデルにおける `title` プロパティです。
見ればわかるように、パラメータはテーブルカラムの名前であり、そのあとに型が続き、最後にオプションが指定されます。

Note: We could have also directly set the properties when we called the generator:

注意: ジェネレータを呼び出すときに、プロパティを直接指定することもできます:

    merb-gen model post title:string
    
By default, the lazy attribute is set to _false_ for everything except text fields.

デフォルトでは、テキストフィールド以外を除き、遅延 (lazy) 属性は _false_ に設定されます。

Some of the available options are:
(TODO) - cover more properties

利用可能なオプションの一部:
(TODO) - より多くのプロパティをカバーすること

    :public, :protected, :private, :accessor, :reader, :writer,
    :lazy, :default, :nullable, :key, :serial, :field, :size, :length,
    :format, :index, :check, :ordinal, :auto_validation, :validates, :unique,
    :lock, :track, :scale, :precision

    :key          - Set as primary key
    :serial       - auto-incrementing key
    :lazy         - Lazy load the specified property (:lazy => true).
    :default      - Specifies the default value
    :field        - Specifies the table column
    :nullable     - Can the value be null?
    :index        - Creates a database index for the column
    :accessor     - Set method visibility for the property accessors. Affects both
                    reader and writer. Allowable values are :public, :protected, :private.
    :reader       - Like the accessor option but affects only the property reader.
    :writer       - Like the accessor option but affects only the property writer.
    :protected    - Alias for :reader => :public, :writer => :protected
    :private      - Alias for :reader => :public, :writer => :private

    :public, :protected, :private, :accessor, :reader, :writer,
    :lazy, :default, :nullable, :key, :serial, :field, :size, :length,
    :format, :index, :check, :ordinal, :auto_validation, :validates, :unique,
    :lock, :track, :scale, :precision

    :key          - 主キーとして設定する
    :serial       - 自動採番キー
    :lazy         - 指定したプロパティを遅延 (lazy) 読み込みにする (:lazy => true).
    :default      - デフォルト値を指定
    :field        - テーブルカラムを指定
    :nullable     - 値が NULL を取ることができるか?
    :index        - そのカラム用にデータベースインデックスを作成する
    :accessor     - プロパティアクセッサ用のメソッドの可視性。読み書き両方に作用。利用可能な値は :public、:protected、:private。
    :reader       - accessor オプションと同じだがプロパティの読み出しのみに作用。
    :writer       - accessor オプションと同じだがプロパティの書き込みのみに作用。
    :protected    - :reader => :public, :writer => :protected と同じ
    :private      - :reader => :public, :writer => :private と同じ

(TODO) - talk about accessors and overriding them

(TODO) - アクセッサとそれらの上書きについて追記する

DataMapper supports the following properties in the core:

DataMapper は以下のプロパティをコア機能でサポートします:

* TrueClass, Boolean
* String
* Text (limit of 65k characters by default)
* Float
* Fixnum, Integer
* BigDecimal
* DateTime
* Date
* Object (marshalled out during serialization)
* Class (datastore primitive is the same as String. Used for Inheritance)

* TrueClass, Boolean
* String
* Text (デフォルトでは 64 * 1024 文字に制限)
* Float
* Fixnum, Integer
* BigDecimal
* DateTime
* Date
* Object (シリアライゼーションするときにマーシャルされる)
* Class (データベースでの型は String と同じ、継承で利用される)

(TODO) - creating your own custom properties

(TODO) - 独自のカスタムプロパティを作成する

### CRUD (作成、読み出し、更新、削除)

#### Creating (作成)
Before a new record is created, be sure you have syncronized your model with the database.
In order to do this, load the merb console with:

新しいレコードを作成するまえに、自分のモデルとデータベースとを同期させましょう。
そのためには、merb コンソールをロードします:
    
     merb -i
     
Then migrate your Post model with:

そして、次のようにして Post モデルをマイグレートします:

    Post.auto_migrate!

To create a new record, just call the method create on a model and pass it your
attributes.

新しいレコードを作成するには、モデルに対して属性データつきで create メソッドを呼び出すだけです。

    @post = Post.create(:title => 'My first post')

Or you can instantiate an object with #new and save it to the repository later:
または、#new を使ってオブジェクトのインスタンスを生成したあと、リポジトリ層でセーブする方法でもいいです:

    @post = Post.new
    @post.title = 'My first post'
    @post.save

There is also an AR like method to `find\_or\_create` which attempts to find an
object with the attributes provided, and creates the object if it cannot find it:

AR ライクな `find\_or\_create` メソッドも用意されています。
このメソッドは、与えられた属性を使ってオブジェクトを検索し、見つからなければオブジェクトを作成します:

    @post = Post.first_or_create(:title => 'My first post')

There are a couple of different ways to set attributes on a model:

モデルの属性を設定するには、いくつか異なる方法があります。

    @post.title = 'My first post'
    @post.attributes = {:title => 'My first post'}
    @post.attribute_set(:title, 'My first post')

Find out if an attribute has been changed (aka is dirty):

属性が変更された (つまりダーティである) ことを見てみましょう 

    @post = Post.first
    @post.dirty?
    => false
    @post.attribute_dirty?(:title)
    => false
    @post.title = 'Changing the title'
    @post.dirty?
    => true
    @post.attribute_dirty?(:title)
    => true
    @post.dirty_attributes
    => Set: {#<Property:Post:title>}


#### Reading (aka finding) (読み出し (つまり検索))

The syntax for retrieving data from the database is clean an simple. As you
can see with the following examples.

データベースからデータを抽出するための文法は、次の例で見ればわかりますが、きれいでかつシンプルです。

Finding a post with one as its primary key is done with the following:

主キーを使って post を検索するのは、次のようにします:

    # will raise a DataMapper::ObjectNotFoundError if not found
    # use #get to just return nil if not record is found
    Post.get!(1)

    # 見つからない場合は例外 DataMapper::ObjectNotFoundError が発生します。
    # 見つからない場合は単に nil を返してほしいなら、#get を使ってください。
    Post.get!(1)

To get an array of all the records for the post model:

post モデルの全レコードを配列として取り出すには:

    Post.all

To get the first post, with the condition author = 'Matt':

author = 'Matt' という条件で検索し、最初の post だけを取り出すには:

    Post.first(:author => 'Matt')

When retrieving data the following parameters can be used:

データを取り出すときには、次のパラメータが利用できます:

    #   Posts.all :order => 'created_at desc'              # => ORDER BY created_at desc
    #   Posts.all :limit => 10                             # => LIMIT 10
    #   Posts.all :offset => 100                           # => OFFSET 100
    #   Posts.all :includes => [:comments]

If the parameters are not found in these conditions it is assumed to be an
attribute of the object.

もしパラメータがこれらの条件になかった場合は、オブジェクトの属性であると仮定されます〔訳注: どういうこと???〕。

You can also use symbol operators with the find to further specify a condition,
for example:

また検索するときに、Symbol の演算子を使ってより詳しい条件を指定することもできます。

    Posts.all :title.like => '%welcome%', :created_at.lt => Time.now

This would return all the posts, where the tile was like 'welcome' and was
created in the past.

この例では、すべての post のうち、タイトルに 'welcome' を含み、かつ過去に作成されたものをすべて返します。

Here is a list of the valid operators:

こちらが利用可能な演算子の一覧です:

* gt    - greater than
* lt    - less than
* gte   - greater than or equal
* lte   - less than or equal
* not   - not equal
* like  - like
* in    - will be used automatically when an array is passed in as an argument

* gt    - より大きい
* lt    - より小さい
* gte   - 以上
* lte   - 以下
* not   - 等しくない
* like  - マッチする
* in    - 引数として配列が渡された場合に自動的に使われる

TODO: execute sql via the adaptor.

TODO: アダプタ経由で SQL を実行する

#### Updating (更新)

Updating attributes has a similar syntax to ARs `update_attributes`:

属性を更新するのは、AR の `update_attributes` と似たような文法でできます:

    @post.update_attributes(:title => 'Opps the title has changed!')

You can also just set attributes and then save:

単に属性を設定してから保存してもいいです:

    @post = Post.first
    @post.title = 'New Title!'
    @post.save


#### Destroying (削除)

You can destroy database records with the method `destroy`, this work much like AR.

データベースレコードを削除するには `destory` メソッドを使います。
これも AR と同じです。

    bad_comment = Comment.first
    bad_comment.destroy

### Associations (関連づけ)

Like ActiveRecord, DataMapper has associations which define relationships
between models. There is a difference in syntax but the underlying idea is the
same. Continuing with the `Post` model we can see a few of the associations
defined:

ActiveRecord と同じように、DataMapper はモデル間の関係を定義する関連づけを行うことができます。
文法こそ違うものの、下敷きとなる考え方は同じです。
引き続き `Post` モデルを使って、関連づけの定義を見てみましょう:

    has n, :comments
    belongs_to :author, :class => 'User', :child_key => [:author_id]

The `has n` syntax is a very flexible way to define associations and the
standard way in DataMapper > 0.9. It can be used to model all of ActiveRecord
associations plus more.  The types of associations currently in DataMapper are:

`has n` という文法は、関連づけを定義するうえでたいへん柔軟な方法であり、DataMapper 0.9 以降では標準の方法です。
これを使うと、ActiveRecord で定義できる関連付けのすべてが定義でき、またそれ以上のことも可能です。
DataMapper での関連の種類は、現在のところ以下のようになっています:

     # DataMapper 0.9  | ActiveRecord
     has n, :things     # has_many :things
     has 1, :thing     # has_one :thing
     belongs_to :item  # belongs_to :item
     many_to_one :item # belongs_to :item
     has n, :items, :through => Resource # has_and_belongs_to_many :items
     has n, :gizmos, :through => :things  # has_many :gizmos, :through => :things

The `has n` syntax is more powerful than above, since n is the cardinality of
the association, it can be an arbitrary range.  Some examples:

`has n` という文法はこれより強力です。
なぜなら、n が関連づけにおける基数 (cardinality) を表し、任意の範囲を設定できるからです。
例をお見せします:

    has 0..n #=> will have a MIN of 0 records and a MAX of n
    has 1..n #=> will have a MIN of 1 record and a MAX of n
    has 1..3 #=> will have a MIN of 1 record and a MAX of 3

Pretty straight forward. A few things you should note however, you do not need
to specify the foreign key as a property if it's defined in the association.

実にわかりやすいですね。
注意しなければいけないこととしては、もし外部キーが関連づけの中で定義されてあれば、プロパティとして外部キーを指定する必要はありません。

You also don't have to specify a relationship at all if you don't want to, as
models can have one way relationships.

また望まないのであれば、関連づけを指定する必要は一切ありません。
その場合、〔訳注: 相手の〕モデルは一方向の関係を持つことになります。
<-- またモデルが一方向の関係を持つことができるように、望まないのであれば関連づけを指定する必要は一切ありません。 -->


#### Polymorphic associations (多相関連づけ)

(TODO) -polly assoc

(TODO) 多相関連づけ

#### Where is my `has\_many :through`?! (私のかわいい `has\_many :through` はどこ?!)
DataMapper > 0.9 now supports has_many :through.  For example, if you have a
Post model that has many Categories through the Categorization model you
would define these associations:

DataMapper 0.9 以降では、has_many :through をサポートするようになりました。
たとえば、Categorization モデル経由で Post モデルが複数の Category を持っているような場合、次のようにしてこれらの関連づけを定義できます:

     class Post
       include DataMapper::Resource

       has n, :categorizations
       has n, :categories, :through => :categorizations

     end

     post = Post.first
     post.categorizations #=> []
     post.categories #=> []
     # to attach a category to a post:
     post.categorizations << Categorization.new(:category => Category.first)
     # or you could just create a Categorization object passing in both category and post:
     Categorization.create(:post => post, :category => Category.first)


#### Has And Belongs To Many (HABTM)

A `has n :through` relationship is useful, especially when the join model itself
has a lot of information on it.  Perhaps a subscription which contains the join
date and the users rating for the feed it tracks.  Sometimes, however, the join
model is very simple, just a table with two id columns.

`has n :through` 関係は役に立ちます。
ジョインモデル自体が多くの情報を持っているような場合は特にそうです。
もしかしたら購読 (subscription) は、ジョインした日付と、購読対象のフィードに対してつけられた、ユーザによる採点 (rating) を含んでいるかもしれません。
しかしジョインモデルは、テーブルの id カラムを 2 つ持つだけのような、とてもシンプルな場合もあります。
〔訳注: Subscription モデルは、Feed モデルと User モデルを N : M で関連づけるためのジョインモデルであり、feed id と user id を含んでいる。それ以外に、subscription モデルデータが作成された日付 (= ジョインした日付) と、ユーザによる rating もデータとして持つことが考えられる。それらが上記の『ジョインモデルが (持つ) 多くの情報』にあたる。〕

For this, DataMapper offers an alternative to `:through => :models`, which is
`:through => Resource`.  The use of Resource tells DataMapper to automatically
create a join table.  So to revisit the previous example:

このために DataMapper は、`:through => :models` の代替となる `:through => Resource` を提供しています。
リソースの使用を DataMapper に教えることで、ジョインテーブルを自動生成します。
これを使うと、前の例は次のようになります:

     class Post
       include DataMapper::Resource

       has n, :categories, :through => Resource

     end

     post = Post.first
     post.categories #=> []
     post.categories << Category.first
     post.save
     post.categories #=> [Category.first]

The join table this would create be called `posts_categorizations` which would
contain the two keys of each post-categorization pair.

これによって生成されるジョインテーブルは `posts_categorizations` という名前になり、Post と Categorization の組における 2 つのキーを含みます。

### Validation (バリデーション)

(TODO) - still needs 0.9 love - mostly done now, I think

(TODO) - 0.9 への愛がまだ必要 - ほとんどできたとは思ってる

It's a known fact that users will enter invalid, blank or malicious data into
your web app.

よく知られた事実ですが、ユーザは正しくないデータや、空欄や、悪意のあるデータをアプリに入力します。

We need to guard against user error by validating anything that we need to
save out to our persistence layers. Sometimes that means guarding against
hack attempts, but most of the time it means guarding against invalid data
and accidents.

永続化層 (persistence layer) 〔訳注: データベースのこと〕にセーブする必要のあるデータはすべて、バリデーションをすることで、ユーザエラーを防がなくてはなりません。
バリデーションはハッキング防止の意味を持つこともありますが、たいていは妥当でないデータや事故を防止することを意味します。

Both ActiveRecord and DataMapper have a concept called Validations, which is
ultimately a set of callbacks which fire right before an object gets saved out
to our persistence layer and interrupt things when it detects something awry.
To use them in DataMapper, all we have to do is require the gem dm-validations.

ActiveRecord と DataMapper の両方とも、バリデーションの概念を持っています。
それは突き詰めると、コールバックの集合といえます。
このコールバックは、オブジェクトが永続化層に保存される直前に呼び出され、エラーがあれば処理を中断させます。

    require 'dm-validations'

    class Post
      include DataMapper::Resource

      property :id, Integer, :serial => true
      property :title, String, :length => 0..255
      property :body, Text
      property :original_uri, String, :length => 0..255
      property :created_at, DateTime
      property :can_be_displayed, Boolean, :default => false

      validates_present :body

    end

How many validations do we have on the content of the post class? To someone
familiar with ActiveRecord, the answer is obviously one.  We have a validation
that the body must contain something - that it is present.  In fact DataMapper,
through dm-validations, has set up _four_ validations for us.  When we declare
properties like `:length => 0..255` as well as declaring the maximum length for
the field, it also adds a validation to check that the supplied values will fit
within that field.  So when we validate our model DataMapper will check we ...

どれだけたくさんのバリデーションが Post クラスにあるかわかりますか?
ActiveRecord に慣れている人なら、答えは明らかに「1」でしょう。
body が何らかのデータを含んでいなければならない - つまり body が存在してないといけない、というバリデーションだけがあるからです。
しかし DataMapper の場合、dm-validations によって、_4 つ_のバリデーションが設定されます。
たとえばプロパティで `:length => 0..255` と宣言した場合、これはフィールドの最大長を宣言しただけでなく、入力された値がこのフィールド内に収まるかどうかをチェックするようなバリデーションも追加されるのです。
そのため、モデルをバリデーションすると、DataMapper は以下のことをチェックします...

* have a `body`, which contains ... something, at least

* `body` があるかどうか、また少なくとも何かデータが入力されているかどうか

And also, without us having to type anything, that we ...

また、特に何も追加することなく、以下のこともチェックしてくれます...

* have a `title`, with a length somewhere between 0 and 255 characters
* have an `original_url`, with a length somewhere between 0 and 255 characters.
* have a value for `can_be_displayed` which is `true` or `false` (but not `nil`)

* `title` があるかどうか、また長さが 0 から 255 文字以内であるか
* `original_url` があるかどうか、また長さが 0 から 255 文字以内であるか
* `can_be_displayed` に値があってそれが `true` または `false` (ただし `nil` ではない) であるか 

We can test this by calling `valid?` on one of our posts:

Post モデルに `valid?` メソッドを呼び出すことで、これらを確認することができます:

    @post = Post.new
    @post.valid?
    => false
    @post.title = "A cool story!"
    @post.body = "It was a dark and stormy ..."
    @post.valid?
    => true

If an object isn't valid, you can access its the errors by calling its `errors` method.

もしオブジェクトのデータが正しくない場合、`errors` メソッドを呼び出すことでエラーにアクセスすることができます。

    @post.errors
    => #<DataMapper::Validate::ValidationErrors:0x2537e40 @errors={:body=>["Body must not be blank"]}>


#### Contextual Validation (文脈別バリデーション)

A problem arises when your website has users creating content and content being
created automatically from scrapers or some sort of automated background process
(be it from RSS feeds, an FTP server or a web service). No idiots are involved
in the creation of content when it's imported into the system and you likely
really want that content to appear in your system. This is where context specific
validations come into play.

自分の Web サイトに、コンテンツを作成するユーザと、スクレイパ (scraper) あるいはある種の自動化されたバックグラウンドプロセスによって (たとえば RSS フィードや FTP サーバや Web サービスから) 自動的に作成されたコンテンツがある場合、問題が発生します。
コンテンツ作成において、それがシステムに取り込まれるときにミスをするような人間はおらず、またそのコンテンツがシステム内に現れるのをおそらく本当に望んでいます〔訳注: ???〕。
これは、文脈によって異なるバリデーションが作用している場面です。

Contexts let you control which validations run when you perform a particular
operation. You might want to make sure that a user enters the title for a blog
post in your system, but you don't really want such a check for when that blog post
comes in off of your RSS scraping system. Maybe you'd send those imported blog
posts into a holding pen somewhere so that they can be rescued later, rather than
preventing their save and never importing them in at all.

文脈によって、ある操作を実行するときにどのバリデーションを実行するかを制御することができます。
システムにおいて、ユーザがブログへ投稿するときにはタイトルが入力されているかを確かめたいという場合もありますし、RSS スクレイピングシステムからのブログの投稿ならそういったチェックはしたくないという場合だってあります。
<!-- システムにおいて、ブログへの投稿のタイトルをユーザが入力していることを確かめたいという場合もありますし、RSS スクレイピングシステムからのブログの投稿ならそういったチェックはしたくないという場合だってあります。 -->
これらの読み込んだブログの投稿を、保存しないようにして一度に読み込むことがないようにするよりも、あとから救出可能なように留置所かどこかに送信したいと思うかもしれません〔訳注: ???〕。

With ActiveRecord, if you declare a `validates\_presence\_of` on `:title`,
that's it - game over. The only way to bypass that validation is to
`save\_without\_validations` and that skips all of your validations, rather
than just this one.

ActiveRecord では、`validates\_presence\_of` を `:title` に宣言してしまうと、そこでおしまいです〔訳注: それ以上のことはできない〕。
唯一の方法は、`save\_without\_validations` を使ってバリデーションを回避することですが、それだと今度はそのバリデーションだけをスキップするのではなく、すべてのバリデーションをスキップしてしまいます。

But with DataMapper and dm-validations , you can check for the validity of an
object depending on the circumstance you're in. Here's what that blog post model
would look like if we wanted to validate blog posts by idiots, but not from our
not-so-idiotic scrapper:

しかし DataMapper と dm-validations を使えば、状況によってオブジェクトのバリデーションを使い分けることができます。
<!-- しかし DataMapper と dm-validations を使えば、状況に従ったオブジェクトのバリデーションを行うことができます。 -->
以下に示すブログの Post モデルでは、ユーザがブログの投稿をしたときは間違った入力をする可能性があるのでバリデーションを行い、スクレイパからの投稿であればエラーの可能性がないのでバリデーションをしないようにしています。

    require 'dm-validations'

    class Post
      include DataMapper::Resource

      property :id, Integer, :serial => true
      property :title, String, :length => 0..255, :auto_validation => false
      property :body, Text
      property :original_uri, String, :length => 0..255, :auto_validation => false
      property :created_at, DateTime
      property :can_be_displayed, Boolean, :default => false

      # user creation
      validates_present :title, :when => [:default, :display]
      validates_present :body, :when => [:default, :display, :import]

      # automated import
      validates_length :original_uri, :in => 0..255, :when => :import

      # a callback to set can_be_displayed appropriately (more on these later)
      before :save do
        self.can_be_displayed = true if self.valid? :display
      end
    end

Running quickly through my sample here, you'll spot a few things.  The first
is the `:auto_validation => false` on the `title` and the `original_uri`.
Because we want to define custom contexts for when we need these properties to
be checked, we have to override the ones dm-validations adds by default.  The
second are the `:when => [...]` following some of our validations.  These define
in what situation (or _context_) these validations will be applied.

このサンプルを実行してみたとき、2、3 のことに気づくでしょう。
1 つめは、`title` と `original_uri` につけられた `:auto_validation => false` です。
これらのプロパティがいつチェックされる必要があるかを表すための文脈を自分で定義したいので、dm-validations がデフォルトで付加するオプションを上書きする必要があります。
2 つめは、いくつかのバリデーションに `:when => [...]` が追加されていることです。
これらは、どのような状況で (つまりどのような_文脈_で) これらのバリデーションが適用されるかを定義します。

To check if a post is valid in a particular context, we pass the context as an
argument to `valid?`.  For example `@post.valid? :display` tells us if the post
is valid for displaying. These contexts are also honoured by the `save` method,
allowing us to call `@post.save :import` after our RSS scrapper has parsed the
RSS feed and assigned our variables.

ある文脈で、投稿にエラーがないかどうかをチェックするには、`valid?` メソッドの引数として文脈を渡します。
たとえば、`@post.valid? :display` は表示する場合においてその投稿にエラーがないかどうかを表します。
このような文脈は `save` メソッドでも効果を持ち、たとえば RSS スクレイパが RSS フィードを構文解析して必要なデータを取り出したあとで、`@post.save :import` を呼び出すといったことができます。

You'll notice that I gave `:body` a `validates\_present` for all my contexts.
This means that, no matter what, that validation callback will kick in.  At
present there doesn't appear to be a meta "all" context, which will fire under
any circumstances.

また、`:body` に対してはすべての文脈において `validates\_present` が有効になっていることに気づいていることと思います。
これはつまり、どんな場合でも、そのバリデーションのコールバックが呼び出されることを意味します。
現在では、どんな状況でも呼び出されるような、「すべて」を表すようなメタな文脈を指定することはできません。

Also of note is the `can\_be\_displayed` boolean and the `before :save` manual
callback I defined. Here, I'm helping myself out later on so that it's easy to
pull out valid blog posts that can be displayed without worrying about nil field
values and such:

また、`can\_be\_displayed` が真偽値を返すことと、`before :save` という手動でのコールバックを定義していることにも注目しましょう。
ここでは、エラーのないブログ投稿データを引き出すのを簡単にすることで、あとで私自身楽をしようと思っています。
そうしておけば、フィールド値が nil であるかどうかを気にすることなくブログ投稿データを表示することができます。
たとえばこれが:

    @posts = Post.all(
      :title.not => nil,
      :slug.not => nil,
      :order => :created_at.desc,
      :limit => 10
    )

Becomes…

このようになります…

    @posts = Post.all(
      :can_be_displayed => true,
      :order => :created_at.desc,
      :limit => 10
    )

Pretty sexy, no? I can't off-hand think of a way to get this functionality from
ActiveRecord objects without a lot of fuss and bother - perhaps using
single-table inheritance and with the validations on the subclasses?

これってすごくないですか? これと同じ機能を ActiveRecord オブジェクトで実現しようと思ったら、私は多くの不平やイライラなしではいられないでしょう - 単一テーブル継承と、サブクラスでのバリデーションを使えば、もしかしたらできるかも?

With the proper use of validation contexts, you end up saving yourself a lot of
headache and work later on down the line, as well as supporting different
scenarios where a post might be valid or might not -- all without having to
hack-around. How enterprise-y!

文脈ごとのバリデーションを適切に使えば、最後には頭痛と労働から自分自身を解放することができます〔訳注: "later on down the line" って何???〕。
また投稿がエラーを含むべきでなかったり、または含んでいてもいいというような、異なるシナリオに対応しなければならなくても -- ハックする必要がまったくありません。
なんというエンタープライジー (enterprise-y) !

#### validates\_with\_method

Another very powerful feature in dm-validations is `validates\_with\_method`.
Think of it as like overloading `valid?` only with the full power of real
validations still there too.

dm-validations が持っているもうひとつの強力な機能は、`validates\_with\_method` です。
これはちょうど、バリデーションの強力さはそのままに、`valid?` をオーバーロードしたようなものです。

Say, for example, you've got an Event model that needs to make sure the
`end\_date` for the event is greater than the start_date. Wouldn't want to
break the laws of physics, so we'd do something like:

たとえば Event というモデルがあったとし、これの `end\_date` が start_date より大きいことを確認しておきたいとします。
物理法則を無視することはしたくないので、たとえば次のようにします:

    class Event < ActiveRecord::Base
      def valid?
        start_time < end_time
      end
    end

Yup, it's pretty simple with ActiveRecord. Just toss in our own valid? method and
we're done. With DataMapper, things are a touch more complicated, but not
difficult, and buy you the full power of dm-validations:

自分自身で valid? メソッドを定義するだけであり〔訳注: わからん???〕、ActiveRecord を使って実にシンプルにできました。
DataMapper の場合は、より複雑になりますが、難しくはなく、また dm-validations の力をすべて享受できます。

    class Event
      include DataMapper::Resource

      # properties here

      validates_with_method :check_times

      def check_times(context = :default)
        if start_time < end_time
          return true
        else
          return [false, 'End time must be after start time']
        end
      end
    end


So, a couple of things are going on here.  First, we declare that we're going to
use our custom validation method `check_times` for the model.  Then comes the
method itself.  It's a pretty simple method.  If our `start\_time` is before
our `end\_time`, return `true` as we're valid.  Otherwise, it return an array. The
first entry in the array is `false`, which lets DataMapper know the validation
has failed.  The second entry is a string, which is added to `@event.errors` so
the user has some idea what has gone wrong.

ここでは、2 つのことが行われています。
最初に、モデルに対して `check_times` というバリデーション専用のメソッドを使うことを宣言します。
続いて、メソッド自身が定義されています。
これは実にシンプルなメソッドです。
もし `start\_time` が `end\_time` より前であれば、エラーがないことを表す `true` を返します。
そうでなければ、配列を返します。
配列の最初の要素は `false` であり、これによって DataMapper はバリデーションが失敗したことを知ります。
2 番目の要素は文字列であり、ユーザに何が間違っていたかを知らせるために、この文字列が `@event.errors` に追加されます。

Of course, this custom validator can also be applied only in certain contexts,
just by adding a `:when => [...]` on the `validates_with_method` line.  This
brings us a lot of flexibility, and as we're validating with a ruby method, we
can get as complex as we need to specify our behaviour.  Much nicer than just
overriding valid.  It's this functionality which requires the context to be
passed in (Although your method can feel free to ignore it).

もちろん、この特注のバリデータは、ある特定の文脈でのみ適用させるようにすることができます。
そのためには `validates_with_method` の行に `:when => [...]` を追加します。
これにより、多大な柔軟性を得ることができます。
Ruby のメソッドを使ってバリデートしているので、自分の好きなだけ振る舞いを指定できます。
単に valid メソッドをオーバーライドするよりもずっといいです。
この機能こそ、文脈を指定することが必要とされるものです (もちろん文脈を無視してもいっこうに構いません)。

### Migrations (マイグレーション)

There is a rake task to migrate your models, but be warned these are currently
destructive!

自分のモデルをマイグレートするための rake タスクが用意されています。
しかし、現時点ではこれらは破壊的であると警告されます!

    rake dm:db:automigrate    # Automigrates all models
    rake dm:db:autoupgrade     # Perform non destructive automigration

    rake dm:db:automigrate    # 全モデルについて自動マイグレーションを実行
    rake dm:db:autoupgrade    # 非破壊的な自動マイグレーションを実行

You can also create databases from the Merb console (`merb -i`)

データベースは Merb コンソール (`merb -i`) で作成することもできます。


    Post.auto_migrate!

or

または

    Post.auto_upgrade!

This does the same job as the rake task migrating all your models.

これは、自分のすべてのモデルについてマイグレーションを行う rake タスクと同じことを行います。

    DataMapper.auto_migrate!

Why the two commands? They both do slightly different things.

なぜ 2 つのコマンドがあるのでしょうか? 両者は少しだけ違うことを行います。

The first, `auto_migrate!`, works by dropping the table (if it exists) and all
of its data then working out which columns need to exist from the model
definition, before finally rebuilding the table in the database.  This includes
any constraints imposed. For example, `:nullable => false` will add a `NOT NULL`
to the column definition.

最初の `auto_migrate!` のほうは、テーブルとデータを削除し (もしあれば)、それからモデル定義からどのカラムが必要かを判断し、最後にテーブルをデータベースに作成します。
また、制約もすべて考慮されます。
たとえば、`:nullable => false` が指定されていればカラム定義に `NOT NULL` が付加されます。

`auto_upgrade!` on the other hand, creates the table from nothing only if the
table isn't there already.  If it _is_ there, then it compares the current table
to the model.  If there are properties in the model not defined as columns in
the table, it will add them to the table.  It does have some limitations though.
It doesn't delete columns, and it can't detect renaming them.

一方、`auto_upgrade!` のほうは、テーブルがまだ存在していない場合にのみテーブルを作成します。
もしすでに_存在している_場合は、現在のテーブルとモデルとをを比較します。
テーブルのカラムにないプロパティがモデル中にあれば、それをテーブルに追加します。
ただし、いくつか制限があります。
カラムは削除しませんし、カラム名の変更も検出できません。

#### Migration Files

Whilst the preceding commands and tasks can keep the database schema in
perfect sync with the models, they can also wipe out any data you might have in
the database, or fail to remove columns which are no longer needed.  To avoid
this, AR style migrations are also supported.  These are stored in `schema/migrations`
and are ruby files.

    migration(1, :add_homepage_to_comments ) do
      up do
        modify_table :comments do
          add_column :homepage, String, :length => 100, :nullable => true
        end
      end

      down do
        modify_table :comments do
          drop_column :homepage
        end
      end
    end

The first line of the file is what identifies the migration, and there are two
components to it.  The more important one is the name, `:add_homepage_to_comments`,
which must be unique across all the migrations applied to the database.  The
other parameter, `1` in this case, is the level or order and migrations are
applied.  This number doesn't have to be unique, although a migration mustn't
have a higher number than a migration it depends on.  You shouldn't define
modifications to a table to happen before that table is made, for example.

The `up` and `down` blocks describe the actual behaviour of the migration. `up`
is what happens when the migration is applied, and `down` happens when the
migration is 'undone'.  This might not mean undone in the literal sense - if you
migrate to remove a column, and add it back in the `down` migration, while the
column will be there, all the data will be lost.

In this case, as the name suggested, we add a homepage column to comments.
It's specified much like a property (and should match up with the relevant
property in the model.rb file).  It also takes many of the same options -
essentially all those which are just database features: `:length`, `:nullable`
are valid, but `:private`, which is a pure ruby option is not allowed.  The
`down` migration is the opposite - it removes the column.

To apply the migrations, there are a couple of rake tasks available through
merb_datamapper

    rake dm:db:migrate:up                   # migrates the database up
    rake dm:db:migrate:down                 # migrates the database down

Which apply or remove all the migrations in turn.  Sometimes, you don't want to
go all the way up (or down) and so you can also specify a level to migrate to,
via `VERSION=2` or invoking a task like `rake dm:db:migrate:up[2]`.  For both up
and down migrations, the version determines the highest order that will be
reflected in the table, either by applying `up` migrations until the level is
complete or applying all the `down` migrations greater than the given level.

There are a couple of generators to make migrations

    merb-gen migration name_of_migration    # an empty migration
    merb-gen resource_migration Post        # a migration for the post class

The first creates an empty migration stub with the name defined and an `up` and
`down` block.  The second loads up the class in question from app/models and
does it's best to construct the appropriate migration from the properties of the
model.  It currently doesn't generate anything to do with relationships, however.

### Other Misc Things

#### Callbacks

Callbacks in DataMapper > 0.9 are very powerful.  In any DataMapper::Resource
you can set before and after callbacks on any instance/class method.  There are
a couple of different ways to define callbacks:

    class Post
      include DataMapper::Resource

      property :id, Integer, :serial => true
      property :title, String, :length => 200

      # before save call the instance method make_permalink
      before :save, :make_permalink

      def make_permalink
        self.title = PermalinkFu.permalink(self.title)
      end

      #callbacks can be defined for any method
      after :publish, :send_message

      def publish
        # do some publishing here
      end

      def send_message
        # email someone here
      end

      # defining a callback on a class method, passing in a block to run before its created.
      before_class_method :create do
        # do something before a record is created
      end

    end

#### Bulk Operations

Sometimes, you have to operate on a large number of records at once, to do
exactly the same thing to each of them.  The example earlier for deleting old
posts via each.  It involved several `SELECT`s and then lots of `DELETE`s, potentially
hundreds, depending on the size of the database.  Wouldn't it be nice if you
could just go `Comments.all(:date.lt => Date.today - 20).destroy!` and it would
produce an appropriate query to do it in one operation and without loading all
those posts which are about to be deleted?

Well, that's what happens.  `Collection`s are 'lazily evaluated', which is to
say, they don't do anything until they've been 'kicked'.  `.each`, mentioned
earlier, is a kicker method. It issues a `SELECT` appropriate to the conditions.
`.destroy!` is another one, except it issues a `DELETE`.  The other bulk method is
`update!`, which looks like (example taken from the DataMapper source)

    Person.all(:age.gte => 21).update!(:allow_beer => true)

This command would update the `allow_beer` attribute of all people aged 21 or
older in the database, all in one `UPDATE` statement.

Note: ActiveRecord has a well known `Model.delete_all` class method to erase all table entries. In DataMapper to delete all instances of an Object in the database, you would do `Model.all.destroy!`

#### Aggregates

DataMapper by default does not provide aggregator methods, but dm-aggregates
in dm-more does. After adding `dependency "dm-aggregates"` to your merb `init.rb`
file, your resource model will have aggregator methods including `count`, `min`,
`max`, `avg`, and `sum`.  You can pass conditions to any of these aggregator
methods the same as Resource.first or Resource.all

    Post.count :title.like => "%hello world%"

    # you can also do a count on an association:
    @post.comments.count

    Post.avg(:reads_count)

    Post.sum(:comments_count)


#### Each

Each works like like expected iterating over a number of rows and you can pass
a block to it. The difference between `Comments.all.each` and `Comments.each`
is that instead of retrieving all the rows at once, each works in batches
instantiating a few objects at a time and executing the block on them (so is less
resource intensive). Each is similar to a finder as it can also take options:

    Comments.all.each(:date.lt => Date.today - 20).each do |c|
      c.destroy
    end

NB: This isn't currently working in DataMapper.  It instead fetches all the
records.  However, it will be reimplemented soon.

#### Changing the Table Name

You can set the name of the database table in your model if it is called
something different by overriding a method in the class:

    def default_storage_name
      'list_of_posts'
    end

This is only necessary if you are using an already existing database.  If you
have a lot of tables to rename, consider instead a `NamingConvention`, detailed
later.

TODO: Write NamingConventions section.
