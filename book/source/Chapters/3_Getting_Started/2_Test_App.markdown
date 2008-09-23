## Creating an App (���ץ���äƤߤ�)

One of the best ways to become familiar with a framework is to jump in and get 
your hands dirty.  So now that we've got everything installed, it's time to roll 
up your sleeves and create a test Merb application. 

�ե졼�����˴����Ǥ�褤��ˡ�ΤҤȤĤϡ���ʬ�μ�Ǽºݤ˻�Ƥߤ뤳�ȤǤ���
ɬ�פʤ�ΤϤ��٤ƥ��󥹥ȡ��뤷���Ϥ��Ǥ��Τǡ�µ��ޤ��ä� Merb ���ץꥱ��������ҤȤĺ�äƤߤޤ��礦��

Merb-more comes with a `gem` called `merb-gen`, this gives you a command line 
tool by the same name which is used for all of your generator needs. You can 
think of it as `script/generate`. Running `merb-gen` from the command line with 
no arguments will show you all of the generators that are available.

Merb-more �򥤥󥹥ȡ��뤹��ȡ�`merb-gen` �Ȥ��� gem �⥤�󥹥ȡ��뤵��ޤ���
���� gem �ϡ������ͥ졼����ɬ�פʾ��̤ǻȤ���Ʊ̾�Υ��ޥ�ɥ饤��ġ�����󶡤��ޤ�������: ���ޤ������ʤ��͡�
���礦�ɡ�����: Ruby on Rails �ˤ������ `script/generate` �Τ褦�ʤ�Τ��ȻפäƤ���������
���ޥ�ɥ饤�󤫤� `merb-gen` ������ʤ��Ǽ¹Ԥ���ȡ����Ѳ�ǽ�ʤ��٤ƤΥ����ͥ졼����ɽ������ޤ���

Merb follows the same naming convention for projects as rails, so 
'my\_test\_app' and 'Test2' are valid names but 'T 3' is not (they need to be 
valid SQL table names).

�ץ������ȤǤϡ�Merb �� Rails ��Ʊ��̿̾�����§�äƤ��ޤ���
���Ȥ��� 'my\_test\_app' �� 'Test2' ��ͭ����̾���Ǥ�����'T 3' �Ϥ����ǤϤ���ޤ��� (������ SQL �ˤ�����ͭ���ʥơ��֥�̾�Ǥ���ɬ�פ�����ޤ�)��

    merb-gen app test
    
This will generate an empty Merb app, so lets go in and take a look. You'll 
notice that the directory structure is similar to Rails, with a few differences.

���Υ��ޥ�ɤ�¹Ԥ���ȡ����� Merb ���ץ꤬��������ޤ���
���ͳ�˸��ƤߤƤ���������
2��3 �ΰ㤤�Ϥ����ΤΡ��ǥ��쥯�ȥ깽¤�� Rails �ˤ褯���Ƥ��뤳�Ȥ��狼��Ǥ��礦��

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


### Configuring Merb (Merb �����깽��)

Before we get the server running, you'll need to edit the `init.rb` file and 
un-comment the following line (this is only necessary if you need to connect 
to a database, which we do in our case):

�����Ф�ư�������ˡ�`init.rb` �ե�������Խ����Ƽ��ιԤΥ����Ȥ򳰤�ɬ�פ�����ޤ�
(����ϥǡ����١�������³������Τ�ɬ�פǤ����ܹƤǤ�ǡ����١�������³����Τ�ɬ�פȤʤ�ޤ�)��

`config/init.rb`

    use_orm :datamapper
    
Typing `merb` now in your command line will start the server.

�����ޤǤ����顢���ޥ�ɥ饤��� `merb` �ȥ����פ���Х����Ф���ư����ޤ���

    Loaded DEVELOPMENT Environment...
    No database.yml file found in /Users/work/merb/example_one/config, assuming database connection(s) established in the environment file in /Users/work/merb/example_one/config/environments
    loading gem 'merb_datamapper' ...
    Compiling routes...
    Using 'share-nothing' cookie sessions (4kb limit per client)
    Using Mongrel adapter

As you can see, however, we did not yet configure the database. Let's create the
database.yml file that merb is looking for:

���������Ƥ��̤ꡢ�ǡ����١��������깽����ޤ����Ƥ��ޤ���Ǥ�����
Merb ��õ���Ƥ��� database.yml �Ȥ����ե������������ޤ��礦:

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

�����åȤ���ꤹ��Τ�˺��ʤ��褦�ˤ��Ƥ���������
�⤷�����åȤ��ɤ��ˤ���Τ��狼��ʤ����ϡ�������ˡ��õ�����Ȥ��Ǥ��ޤ�:

    mysql_config --socket

Starting Merb again shows that everything is running okay.

���� Merb ��ư����ȡ����٤Ƥ���Ĵ��ư���ޤ���

The following command will give you access to the Merb interactive console:

���Υ��ޥ�ɤ�Ȥ��ȡ�Merb �Υ��󥿥饯�ƥ��֥��󥽡���˥��������Ǥ��ޤ�:

    merb -i

You'll notice Merb runs on port 4000, but this can be changed with flag 
`-p [port number]`. More options can be found by typing:

Merb �ϥݡ��� 4000 �֤Ǽ¹Ԥ���ޤ�����`-p [port number]` �ե饰��Ȥ����ѹ��Ǥ��ޤ���
���ץ����ξܺ٤ˤĤ��Ƥϰʲ���¹Ԥ��Ƥ�������:

    merb --help
    
You can even run Merb with any application server that supports rack 
(thin, evented_mongrel, fcgi, mongrel, and webrick):

Merb �ϡ�Rack �����ݡ��Ȥ���Ƥ���ФɤΥ��ץꥱ������󥵡��о�Ǥ�ư��ޤ�
(thin��evented_mongrel��fcgi��mongrel��webrick �ʤ�)��

    merb -a thin

If you see a 500 error with the following error message when trying to navigate
to localhost:4000 in your browser:

�֥饦���� localhost:400 �˥������������Ȥ��ˡ��⤷ 500 ���顼��ȯ�����ưʲ��Τ褦�ʥ��顼��å�������ɽ�����줿��:
    
    undefined method `match' for Merb::Router:Class - (NoMethodError)

This means Merb has been started outside of your applications root directory.

����� Merb ����ʬ�Υ��ץꥱ�������Υ롼�ȥǥ��쥯�ȥ�ʳ��ξ�꤫�鵯ư���줿���Ȥ��̣���ޤ���
