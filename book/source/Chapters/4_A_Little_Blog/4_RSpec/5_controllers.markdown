### Spec'ing Controllers

#### Getting started

Testing controllers typically involves stubbing out some methods, making a fake
request and then ensuring the right variables are assigned, exceptions are
raised and views rendered.

����ȥ���Υƥ��Ȥϡ�ŵ��Ū�ˤϤ����Ĥ��Υ᥽�åɤΥ����֤�ޤߡ����ߡ��Υꥯ�����Ȥ��������ޤ��������ơ��ѿ����������������줿���Ȥ䡢ɬ�פǤ�����㳰��ȯ���������Ȥ䡢�ӥ塼�����̤��줿���Ȥ�Τ���ޤ���

A good start is testing the show action in our Posts controller.

�����ۤɤ� Posts ����ȥ���ˤ����� show ���������Υƥ��Ȥ򤷤Ƥߤޤ��礦��

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

�ǽ�Υƥ��ȤǤϡ�/posts/1 �˥������������ä��Ȥ��� Post.get!(1) ���ƤӽФ���뤳�Ȥ�Τ��ᡢ�ޤ� Post ��¸�ߤ��Ƥ���ʤ�쥹�ݥ󥹥����ɤ� 200 OK �Ǥ��뤳�Ȥ�Τ���ޤ���

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

�ǽ�� should_receive �Ǥϡ�Post.get!(1) ���ƤӽФ���뤳�Ȥ��ǧ���Ƥ��ޤ���Post ���󥹥��󥹤��ä��֤����Ȥ�Ǥ��ޤ���������ξ��Ϥ��Υ᥽�åɤ��ƤФ���㳰��ȯ�������ʤ����Ȥ��狼��н�ʬ�Ǥ���������: "we could mock out a Post instance to return here" ���褯�狼����

Next we use the get method to make a request to the controller.  The get method
yields the controller, allowing us to stub out the render method, as we're not
interested in how that behaves.  Anything inside the get method's block will be
executed before the request is dispatched.

���ˡ�get �᥽�åɤ�Ȥäƥ���ȥ���˥ꥯ�����Ȥ��������ޤ���get �᥽�åɤ�Ȥäƥ���ȥ����ƤӽФ���render �᥽�åɤ��ƤӽФ�����褦�����ꤷ�ޤ���������餬�ɤ������񤦤����Τ�ɬ�פϤ���ޤ���get �᥽�åɤΥ֥�å�����ǹԤʤ��Ƥ��뤳�Ȥϡ��ꥯ�����Ȥ��ǥ����ѥå���������˼¹Ԥ���ޤ���

After the request has been dispatched, it returns the controller.  Several
methods are available to examine the results from the request: body, status,
params, cookies, headers, session, response and route.

�ꥯ�����Ȥ��ǥ����ѥå����줿���ȡ�get �᥽�åɤϥ���ȥ��饪�֥������Ȥ��֤��ޤ����ꥯ�����Ȥ��������ͤ�Ĵ�٤뤿��Υ᥽�åɤ������Ĥ����Ѳ�ǽ�Ǥ�: body, status, params, cookies, headers, session, response, route ������ޤ���

This test was fairly simple, and it's likely you won't need to such tests if
your controllers are as simple as ours. But once you have more than a few lines
in your controller, simple response status checks can be useful for ensuring the
overall integrity of your app.

���Υƥ��Ȥ϶ˤ�ƥ���ץ�Ǥ��ꡢ�ߤʤ���Υ���ȥ��餬����ۤɥ���ץ�Ǥʤ��¤�ϡ����֤�ߤʤ���Ϥ��Τ褦�ʥƥ��Ȥ�ɬ�פȤ��ʤ��Ǥ��礦�����������٥���ȥ��餬 2��3 �����٤ˤϼ��ޤ�ʤ��褦�ˤʤ�С��쥹�ݥ󥹥��ơ�������ñ��ʥ����å��Ǥ⡢��ʬ�Υ��ץ����Τδ�������Τ����Ȥ��ˡ����Ω�ĤǤ��礦��

A more important test would be ensuring that a 404 is returned when the post
cannot be found in the database. When Datamapper cannot find a record it raises
Datamapper::ObjectNotFoundError. Merb has several useful exception classes which
will set the correct status and then call the relevant action in your Exceptions
controller. Raising NotFound will set the status to 404 and then call the
not_found action, which can return a much nicer.

�����פʥƥ��Ȥϡ��ǡ����١����� Post �����Ĥ���ʤ��ä����� 404 ���֤���뤳�Ȥ��ǧ���뤳�ȤǤ��礦��DataMapper �ϥ쥳���ɤ򸫤Ĥ����ʤ��ä���硢DataMapper::ObjectNotFoundError ���֤��ޤ���Merb �����Ω���㳰���饹�򤤤��Ĥ����äƤ��ꡢ���������������ơ����������ꤷ���㳰�ϥ�ɥ��Ŭ�ڤʥ���������ƤӽФ��Ƥ���ޤ���NotFound �㳰��ȯ��������硢���ơ������ˤ� 404 �����ꤵ�졢not_found ��������󤬸ƤӽФ���ޤ���not_found ���������ϼ¤ˤ��Ф餷���쥹�ݥ󥹤��֤��ޤ���

    it "should return 404 if post doesn't exist" do
      Post.should_receive(:get!).with("1").and_raise(DataMapper::ObjectNotFoundError)
      controller = get('/posts/1')
      controller.status.should == 404
    end

Unlike the last test there was no need for us to stub the render method because
DataMapper::ObjectNotFoundError is raised before it is reached.

�Ǹ�Υƥ��ȤȰ㤤��render �᥽�åɤΥ����֤���ɬ�פϤ���ޤ��󡣤ʤ��ʤ顢render �᥽�åɤ�ƤӽФ��ޤ��� DataMapper::ObjectNotFoundError ��ȯ�����뤿��Ǥ���


#### Testing multipart forms

(TODO: Make and example of uploading assets in the simple blog)

The multipart_post method allows you to include files in a fake request. There
must however be an actual file to be opened and submitted. If you put the file
in the same directory as your spec, use File.dirname(__FILE__) to ensure the
full path is used.

multipart_post �᥽�åɤ�Ȥ��ȡ����ߡ��Υꥯ�����Ȥ˥ե������ޤ�뤳�Ȥ��Ǥ��ޤ����ե�����ϼºݤ˳������Ȥ��Ǥ��������Ǥ���ɬ�פ�����ޤ����ե������ spec �ե������Ʊ���ǥ��쥯�ȥ���֤������ϡ�File.dirname(__FILE__) ��Ȥäƥե�ѥ����Ȥ��Ƥ��뤳�Ȥ�Τ���ޤ���

If you are going to open the tempfile which is uploaded, remember to stub out
File.open. Watch out though, if you use simply open instead of File.open it
won't be the File.open you stubbed out. The other issue here is within the spec
we have no way of knowing what the filename of the tempfile is, so we have to
assume it's correct and use an_instance_of(String) so any filename is accepted.

�⤷���åץ��ɤ��줿 tempfile �򳫤����ϡ�File.open �Υ����֤��������褦�ˤ��Ƥ������������ȤǸ���褦�ˡ��⤷ File.open �ǤϤʤ�ñ�� open ��Ȥ����ϡ�����ϥ����֤�������� File.open �Ȥϰ㤦�Τ���դ��Ƥ���������¾�ˡ�spec ����Ǥ� tempfile �Υե�����̾���Τ���ˡ���ʤ��Ȥ������꤬����ޤ������Τ��ᡢ���åץ��ɤ��줿���Ƥ��������Ȳ��ꤷ���ɤΥե�����̾�Ǥ�����դ���褦�� an_instance_of(String) ��Ȥ�ɬ�פ�����ޤ���������: �狼��͡���

(TODO: test code)

    describe Posts, "create action" do 
      it "should receive file" do
        File.should_receive(:open).with(an_instance_of(String))
        multipart_post("/posts", {:image => File.open(File.join( File.dirname(__FILE__), "picture.jpg"))})
        controller.assigns(:filename).should == "picture.jpg"
      end
    end

Your controller would look something like this.

����ȥ��鼫�ȤϤ���ʴ����ˤʤ�Ǥ��礦��

    class Posts < Application
      def create
        fp = File.open(params[:image][:tempfile].path)
        @filename = params[:image][:filename]
      end
    end

#### More ways to dispatch a request

There are several other ways to dispatch a request in your test. 
Look at [Merb's Wiki](http://wiki.merbivore.com/pages/controller-specs) for more information

�ƥ��Ȥˤ����ƥꥯ�����Ȥ�ǥ����ѥå�������ˡ�ϡ��ۤ��ˤ⤢��ޤ���[Merb's Wiki](http://wiki.merbivore.com/pages/controller-specs) �򻲾Ȥ��ƤߤƤ���������
