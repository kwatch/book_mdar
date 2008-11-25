### Spec'ing Controllers

#### Getting started

Testing controllers typically involves stubbing out some methods, making a fake
request and then ensuring the right variables are assigned, exceptions are
raised and views rendered.

コントローラのテストは、典型的にはいくつかのメソッドのスタブを含み、ダミーのリクエストを生成します。
そして、変数が正しく代入されたことや、必要であれば例外が発生したことや、ビューが描写されたことを確かめます。

A good start is testing the show action in our Posts controller.

さきほどの Posts コントローラにおける show アクションのテストをしてみましょう。

    class Posts < Application
      provides :html
  
      def show
        @post = Post.get!(params[:id])
        render @post
  
        rescue DataMapper::ObjectNotFoundError
        raise NotFound
      end
    end

Our first test will ensure that Post.get!(1) is called when /posts/1 is visited,
and when the post exists the response code is 200 OK.

最初のテストでは、/posts/1 にアクセスがあったときに Post.get!(1) が呼び出されることを確かめ、また Post が存在しているならレスポンスコードが 200 OK であることを確かめます。

    describe Posts, "show action" do
      it "should find post and render show view" do
        Post.should_receive(:get!).with("1")
        controller = get('/posts/1') do |controller|
          controller.stub!(:render)
        end
        controller.should be_successful
      end
    end

The first should_receive ensures that Post.get!(1) is called, we could mock out
a Post instance to return here, but in this case we're only interested in it
being called and not raising an exception.

最初の should_receive では、Post.get!(1) が呼び出されることを確認しています。
Post インスタンスを作って返すこともできますが、今回の場合はそのメソッドが呼ばれて例外を発生させないことがわかれば十分です。〔訳注: "we could mock out a Post instance to return here" がよくわからん〕

Next we use the get method to make a request to the controller.  The get method
yields the controller, allowing us to stub out the render method, as we're not
interested in how that behaves.  Anything inside the get method's block will be
executed before the request is dispatched.

次に、get メソッドを使ってコントローラにリクエストを送信します。
get メソッドを使ってコントローラを呼び出し、render メソッドが呼び出させるように設定しますが、それらがどう振る舞うかは知る必要はありません。
get メソッドのブロックの中で行なわれていることは、リクエストがディスパッチされる前に実行されます。

After the request has been dispatched, it returns the controller.  Several
methods are available to examine the results from the request: body, status,
params, cookies, headers, session, response and route.

リクエストがディスパッチされたあと、get メソッドはコントローラオブジェクトを返します。
リクエストからの戻り値を調べるためのメソッドがいくつか利用可能です: body, status, params, cookies, headers, session, response, route があります。

This test was fairly simple, and it's likely you won't need to such tests if
your controllers are as simple as ours. But once you have more than a few lines
in your controller, simple response status checks can be useful for ensuring the
overall integrity of your app.

このテストは極めてシンプルであり、みなさんのコントローラがこれほどシンプルでない限りは、たぶんみなさんはこのようなテストを必要としないでしょう。
しかし一度コントローラが 2、3 行程度には収まらないようになれば、レスポンスステータスの単純なチェックでも、自分のアプリ全体の完全性を確かめるときに、役に立つでしょう。

A more important test would be ensuring that a 404 is returned when the post
cannot be found in the database. When Datamapper cannot find a record it raises
Datamapper::ObjectNotFoundError. Merb has several useful exception classes which
will set the correct status and then call the relevant action in your Exceptions
controller. Raising NotFound will set the status to 404 and then call the
not_found action, which can return a much nicer.

より重要なテストは、データベースに Post が見つからなかった場合に 404 が返されることを確認することでしょう。
DataMapper はレコードを見つけられなかった場合、DataMapper::ObjectNotFoundError を返します。
Merb は役に立つ例外クラスをいくつか持っており、それらは正しいステータスを設定して例外ハンドラの適切なアクションを呼び出してくれます。
NotFound 例外が発生した場合、ステータスには 404 が設定され、not_found アクションが呼び出されます。
not_found アクションは実にすばらしいレスポンスを返します。

    it "should return 404 if post doesn't exist" do
      Post.should_receive(:get!).with("1").and_raise(DataMapper::ObjectNotFoundError)
      controller = get('/posts/1')
      controller.status.should == 404
    end

Unlike the last test there was no need for us to stub the render method because
DataMapper::ObjectNotFoundError is raised before it is reached.

最後のテストと違い、render メソッドのスタブを作る必要はありません。
なぜなら、render メソッドを呼び出すまえに DataMapper::ObjectNotFoundError が発生するためです。


#### Testing multipart forms

(TODO: Make and example of uploading assets in the simple blog)

The multipart_post method allows you to include files in a fake request. There
must however be an actual file to be opened and submitted. If you put the file
in the same directory as your spec, use File.dirname(__FILE__) to ensure the
full path is used.

multipart_post メソッドを使うと、ダミーのリクエストにファイルを含めることができます。
ファイルは実際に開くことができて送信できる必要があります。
ファイルを spec ファイルと同じディレクトリに置いた場合は、File.dirname(__FILE__) を使ってフルパスが使われていることを確かめます。

If you are going to open the tempfile which is uploaded, remember to stub out
File.open. Watch out though, if you use simply open instead of File.open it
won't be the File.open you stubbed out. The other issue here is within the spec
we have no way of knowing what the filename of the tempfile is, so we have to
assume it's correct and use an_instance_of(String) so any filename is accepted.

もしアップロードされた tempfile を開く場合は、File.open のスタブを作成するようにしてください。
あとで見るように、もし File.open ではなく単に open を使う場合は、それはスタブを作成した File.open とは違うので注意してください。
他に、spec の中では tempfile のファイル名を知る方法がないという問題があります。
そのため、アップロードされた内容が正しいと仮定し、どのファイル名でも受け付けるように an_instance_of(String) を使う必要があります。〔訳注: わかんねー〕

(TODO: test code)

    describe Posts, "create action" do 
      it "should receive file" do
        File.should_receive(:open).with(an_instance_of(String))
        multipart_post("/posts", {:image => File.open(File.join( File.dirname(__FILE__), "picture.jpg"))})
        controller.assigns(:filename).should == "picture.jpg"
      end
    end

Your controller would look something like this.

コントローラ自身はこんな感じになるでしょう。

    class Posts < Application
      def create
        fp = File.open(params[:image][:tempfile].path)
        @filename = params[:image][:filename]
      end
    end

#### More ways to dispatch a request

There are several other ways to dispatch a request in your test. 
Look at [Merb's Wiki](http://wiki.merbivore.com/pages/controller-specs) for more information

テストにおいてリクエストをディスパッチする方法は、ほかにもあります。[Merb's Wiki](http://wiki.merbivore.com/pages/controller-specs) を参照してみてください。
