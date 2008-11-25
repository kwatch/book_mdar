## The Framework (�ե졼����)

The directory structure of the project created should look like the following. 
We'll give brief overview of the framework here and go into further details of 
each component in subsequent chapters.

�������줿�ץ������ȤΥǥ��쥯�ȥ깽¤�ϡ����Τ褦�ˤʤäƤ��ޤ���
�����Ǥϥե졼�����δ�ñ�ʳ��פ�������������ʹߤǳƥ���ݡ��ͥ�Ȥξܺ٤��������ޤ���

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

`app` �ե�����β��ˡ�models��views��controllers��helpers������: �γƥե�����ͤ�����ޤ���
�ޤ� `app` �ե������ Parts ��ޤ�Ǥ��ޤ���
����� `AbstractController` ��Ѿ����Ƥ��ꡢ�Ť� Rails ����ݡ��ͥ�Ȥ˻��Ƥ��ޤ��������̤ǡ������ɥС��䥦�����åȤʤɤ���Ω���ޤ���

`Mailers`, which also inherit from the `AbstractController` have their own 
folder where the controllers and views live. 

`Mailers` ��ޤ� `AbstractController` ��Ѿ����Ƥ��ޤ���
`Mailers` �ϼ��ȤΥե��������äƤ��ꡢ����ȥ���ȥӥ塼�򤽤����֤��Ƥ��ޤ���

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

`config` �ե�����ˤϡ����깽���ե�����ȴĶ��Τ��٤Ƥ�������Ƥ��ޤ���
���Τ�����`init.rb` �� `database.yml` �Υե������ Merb ��ư���������Խ�����ɬ�פ�����ޤ���
<!-- Merb ��ư�������ˡ������ˤ��� `init.rb` �� `database.yml` �Υե�������Խ����뤳�ȤϽ��פǤ��� -->

The Merb router, which maps the incoming requests to the controllers is also 
here. The `rack.rb` file is the rack handler and you can pass options to 
`merb -a` to change rack adapter.

Merb �Υ롼���ϡ����������ꥯ�����Ȥ򥳥�ȥ�����б��Ť������ܤ���äƤ��ޤ���������⤳���ˤ���ޤ���
`rack.rb` �ե������ Rack �ϥ�ɥ�Ǥ��ꡢ`merb -a` �˥��ץ������Ϥ��� Rack �����ץ����ѹ����뤳�Ȥ��Ǥ��ޤ���

	config
	  `--> environments

RSpec specs can be found in the spec folder.

RSpec �ˤ����� (���ڥå�) �ϡ�spec �ե�������֤���ޤ���

	spec
	
In addition to these folders you can have a `gem` directory, which stores 
frozen gems (see Freezing Gems for more info), and a `lib` folder to store 
other ruby files.

�����Υե�����˲ä�����뤷�� gems (�ܺ٤ϡ�Gems ����뤹��٤򻲾Ȥ��Ƥ�������) ���֤������ `gem` �ǥ��쥯�ȥ�䡢¾�� Ruby �ե�������֤������ `lib` �ե��������Ĥ��Ȥ��Ǥ��ޤ���
