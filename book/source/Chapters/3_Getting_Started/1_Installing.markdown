## Getting Started (�Ϥ�Ƥߤ�)

<a href="http://xkcd.com/303/" target="_blank"> <img src="http://imgs.xkcd.com/comics/compiling.png" alt="XKCD - Compiling"> </a>

Before we get started I'm going to assume you have the following installed:

�Ϥ��ޤ��ˡ��ߤʤ���ϰʲ��Τ�Τ򥤥󥹥ȡ��뤷�Ƥ��뤳�Ȥ��ǧ���Ƥ�������:

* [Ruby](http://www.ruby-lang.org/) 
* [RubyGems >= 1.1.0](http://www.rubygems.org/)
* A DBMS (we'll use [MySQL](http://mysql.org/))
* [SVN](http://subversion.tigris.org/) and [git](http://git.or.cz/)

* [Ruby](http://www.ruby-lang.org/) 
* [RubyGems >= 1.1.0](http://www.rubygems.org/)
* DBMS (�ܹƤǤ� [MySQL](http://mysql.org/) ��Ȥ��ޤ�)
* [SVN](http://subversion.tigris.org/) �� [git](http://git.or.cz/)

#### What will be covered (���夲������)

* Installing Merb, DataMapper and RSpec
* Creating a temporary test app
* The basic directory structure for the framework

* Merb �� DataMapper �� RSpec �Υ��󥹥ȡ���
* ���Ū�ʥƥ��ȥ��ץ�κ���
* �ե졼�����δ���Ū�ʥǥ��쥯�ȥ깽¤

### The Easy Way (��ñ����ˡ)

If you're on a *nix operating system then keeping up to date with all the edge 
versions of these gems can be made really easy by using the Sake tasks.

�⤷ *nix �ϤΥ��ڥ졼�ƥ��󥰥����ƥ��ȤäƤ���ʤ顢Sake ��������Ȥ����Ȥǡ������� gems �ΥС�������ǿ��Τ�Τ��ݤĤΤ��¤˴�ñ�ˤǤ��ޤ���

Merb sake tasks can be found in merb-more repository under tools directory.
Sake tasks for DataMapper are in dm-dev repository at
http://github.com/dkubb/dm-dev/.

Merb �� sake �������ϡ�merb-more ��ݥ��ȥ�� tools �ǥ��쥯�ȥ겼�ˤ���ޤ���
DataMapper �Ѥ� Sake �������ϡ�http://github.com/dkubb/dm-dev/ �ˤ��� dm-dev ��ݥ��ȥ����ˤ���ޤ���

To install Sake tasks run sake -i PATH where PATH is path to Sake tasks file
on your local machine. For example,

Sake �������򥤥󥹥ȡ��뤹��ˤϡ���ʬ�Υ�����ޥ���� sake -i PATH ��¹Ԥ��ޤ� (������ PATH �� Sake �������ե�����ؤΥѥ��Ǥ�)��

    sake -i ~/dev/opensource/merb/merb-more/tools/merb-dev.rake

To do a fresh clone of all repositories use sake dm:clone and	merb:clone,
respectively. And then to keep up to date you just need to execute:

���٤ƤΥ�ݥ��ȥ�ˤĤ��ƥե�å��奯�����Ԥ��ʤ顢���줾�� sake dm:clone �� merb:clone ��¹Ԥ��Ƥ���������
���Τ��ȡ�Merb �� DataMapper �� gems ��ǿ��Ǥ˹�������ˤϡ�

    sake dm:update

and

��

    sake merb:update

to update Merb and DataMapper gems.

��¹Ԥ��Ƥ���������

But what you really want is probably to wipe out Merb and DM gems before update,
do the update and install new updated gems. Use sake merb:gems:refresh and dm:gems:refresh to do so.

��������������ɬ�פȤ��Ƥ���ΤϤ����餯��Merb �� DM �� gems �򹹿����������ˤ��줤�˾ä���ꡢ���줫��ǿ��Ǥ� gems �򥤥󥹥ȡ��뤹�뤳�Ȥ��Ȼפ��ޤ���
���Τ���ˤϡ�sake merb:gems:refresh �� dm:gems:refresh ��¹Ԥ��Ƥ���������

### If You're Hardcore (�⤷�ĥ��ΤǤ����)

#### Installing Merb (Merb �Υ��󥹥ȡ���)
***
If you have an older version of Merb (<0.9.2) you should remove all merb and 
datamapper related gems before continuing. Use `gem list` to see your installed
gems. The following command will uninstall the gem you specify:

�⤷�Ť��С������ (<0.9.2) �� Merb �򥤥󥹥ȡ��뤷�Ƥ���ʤ顢��Ϣ���뤹�٤Ƥ� merb �� datamapper �� gems ������˺�����Ƥ���ɬ�פ�����ޤ���
`gem list` ��¹Ԥ��ơ���ʬ������ȡ��뤷�� gems ���ǧ���Ƥ���������
���Υ��ޥ�ɤ�Ȥ��ȡ����ꤷ�� gem �򥢥󥤥󥹥ȡ��뤹�뤳�Ȥ��Ǥ��ޤ���

    sudo gem uninstall the_gem_name
***
Installing the `merb` gems should be as simple as:

`merb` gems �򥤥󥹥ȡ��뤹��Τ����˴�ñ�Ǥ�:
    
    sudo gem install merb --source http://merbivore.org
    
*or for JRuby:*

*�ޤ��ϡ�JRuby ��ȤäƤ���ʤ�:*
    
    jruby -S gem install merb mongrel 
    
__Unfortunately__ we are living right on the edge of development so we'll need 
to get down and dirty with building our own gems from source. Luckily this is 
much easier than it sounds... 

__��ǰ�Ǥ���__���ܹƤǤϺǿ��γ�ȯ�Ǥ�ȤäƤ��뤿�ᡢ���������鼫ʬ�� gems ���ۤ���Ȥ�������Ż��򤷤ʤ���Фʤ�ޤ���
�����ˤ⡢����ϻפä��ʾ�ˤ����֤�ȴ�ñ�Ǥ�...

Start by installing the `gem` dependancies:

�ޤ��ϰ�¸�����Τ� `gem` �ǥ��󥹥ȡ��뤷�ޤ�:

    sudo gem install rack mongrel json erubis mime-types rspec hpricot \
        mocha rubigen haml markaby mailfactory ruby2ruby

*or for JRuby:*
*�ޤ��� JRuby ��ȤäƤ���ʤ�:*

    jruby -S gem install rack mongrel json_pure erubis mime-types rspec hpricot \
        mocha rubigen haml markaby mailfactory ruby2ruby

Then download the `merb` source:
������ `merb` �Υ��������������ɤ��ޤ�:

    git clone git://github.com/sam/extlib.git
    git clone git://github.com/wycats/merb-core.git
    git clone git://github.com/wycats/merb-plugins.git
    git clone git://github.com/wycats/merb-more.git

Then install the gems via rake:
���줫�� rake ��ȤäƤ����� gems �򥤥󥹥ȡ��뤷�ޤ�:

    cd extlib ; rake install ; cd ..
    cd merb-core ; rake install ; cd ..    
    cd merb-more ; rake install ; cd ..
    cd merb-plugins; rake install ; cd ..

Note that Merb and DataMappers share Extlib library since after 0.9.3 release of DM.
The `json_pure` gem is needed for merb to install on [JRuby](http://jruby.codehaus.org/) (Java implementation of a Ruby Interpreter), otherwise use the `json` gem as it's faster.

��ջ���Ǥ�����Merb �� DataMapper �ϡ�DM 0.9.3 ��꡼���ʹߤ� Extlib �饤�֥���ͭ���ޤ���
Merb �� [JRuby](http://jruby.codehaus.org/) (Ruby ���󥿥ץ꥿�� Java ����) �˥��󥹥ȡ��뤹��Ȥ��ϡ�`json_pure` gem ��ɬ�פˤʤ�ޤ���
����ʳ��Ǥϡ�����®�� `json` gem ��Ȥ��ޤ��礦��

Merb is ORM agnostic, but as the title of this book suggests we'll be using 
DataMapper. Should you want to stick with ActiveRecord or play with Sequel, 
check the [Merb documentation](http://merb.rubyforge.org/files/README.html) for install instructions.

Merb �Ǥ� ORM ��ͳ�����٤ޤ������ܹƤΥ����ȥ뤬�����褦�ˡ������Ǥ� DataMapper ��Ȥ��ޤ���
�⤷ ActiveRecord �˸Ǽ������ꡢSequel ��ͷ��Ǥߤ����Ȥ������ϡ�[Merb documentation](http://merb.rubyforge.org/files/README.html) ���ɤ�ǥ��󥹥ȡ�������ǧ���Ƥ���������

#### Installing DataMapper (DataMapper �Υ��󥹥ȡ���)

***
DataMapper has spit into the gems `dm-core` and `dm-more`, the old `datamapper` 
gem is now outdated.

DataMapper �ϡ�`dm-core` �� `dm-more` �Ȥ�����2 �Ĥ� gems ��ʬ����Ƥ��ޤ���
`datamapper` �Ȥ��� gem �ϸŤ��Τǡ��Ȥ�ʤ��Ǥ���������

If you have an older version of `datamapper`, `data_objects`, or `do_mysql`, 
`merb_datamapper` (< 0.9) you should remove them first.

�⤷ `datamapper` �� `data_objects` �� `do_mysql` �� `merb_datamapper` (< 0.9) �Ȥ��ä��Ť��С������򥤥󥹥ȡ��뤷�Ƥ���ʤ顢�ޤ��ǽ�ˤ����������Ƥ���������
***

We will use MySQL in the following example, but you can use either sqlite3 or 
PostgreSQL, just install the appropriate gem. You will also need to ensure that 
MySQL is on your system path for the gem to install correctly.

�ܹƤǤϰʹߤΥ���ץ�� MySQL ����Ѥ��ޤ�����Ŭ�ڤ� gem �򥤥󥹥ȡ��뤹��С�sqlite3 �� PostgreSQL ��Ȥ����Ȥ��ǽ�Ǥ���
�ޤ� gem �����������󥹥ȡ��뤵��뤿��ˡ�MySQL ����ʬ�Υ����ƥ�ѥ���¸�ߤ��뤳�Ȥ��ǧ����ɬ�פ�����ޤ���

To get the gems from source:

���������� gems ���ۤ���ˤ�:

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

���������� gem �򹹿�����ˤϡ�`git pull` �� `rake install` ����ټ¹Ԥ��Ƥ���������

#### Install RSpec (RSpec �Υ��󥹥ȡ���)

The `rspec` gem was installed in the Merb section above. However, if you want 
to grab the source, run one of the following commands:

`rspec` �� gem �ϡ���� Merb ���������ǥ��󥹥ȡ��뤵��Ƥ��ޤ���
���������⤷���������饤�󥹥ȡ��뤷�����ʤ顢���Υ��ޥ�ɤΤ����ɤ��餫��¹Ԥ��Ƥ�������:

    gem install -r rspec
    
    # or
    
    git clone git://github.com/dchelimsky/rspec.git
    cd rspec
    rake gem
    sudo gem install pkg/rspec-*.gem
