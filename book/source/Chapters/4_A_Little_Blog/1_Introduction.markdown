## A little blog (�����ʥ֥�)

#### What will be covered (������Ǽ�갷������)

 * Setting up the example blog application
 * Creating the models
 * Configuring your routes
 * RESTful controllers
 * Some of the view helpers in `merb_helpers`
 * Configuring and sending emails

 * ����ץ�Ȥʤ�֥����ץꥱ������������
 * ��ǥ�κ���
 * �롼�ƥ��󥰤����깽��
 * RESTful �ʥ���ȥ���
 * `merb_helpers` ���������Ƥ���ӥ塼�إ�ѤΤ����Ĥ�
 * �Żҥ᡼��������Ȥ��Τ�������깽��

In the examples, we'll be developing a small blogging application. It's a good
idea to grab the source code from [http://github.com/deimos1986/book_mdar/tree/master/code](http://github.com/deimos1986/book_mdar/tree/master/code), 
so you can follow along with the examples.

����ץ�Ǥϡ��������֥����ץꥱ��������������ޤ���
�����������ɤ� [http://github.com/deimos1986/book_mdar/tree/master/code](http://github.com/deimos1986/book_mdar/tree/master/code) �����äƤ���ޤ��Τǡ�����ץ�˱�äƻ���Ȥ��Ǥ��ޤ���

First of all, let's define some of the functionality we would expect from any 
blogging application. 

�ޤ��ǽ�ˡ��֥����ץꥱ�������Ȥ��ƹͤ����뵡ǽ�򤤤��Ĥ�������ޤ��礦��

* Publishing posts
* Leaving comments
* Sending email notifications
* Attaching images
* Authentication

* ��Ƥ��������
* �����Ȥ�Ĥ�
* �Żҥ᡼������Τ���
* ������ź�դ���
* ǧ�ڤ���

We're going to call our app `golb`. Think of it as a backward blog. Feel free 
to change the name of your app, but if you do, remember to replace the word
`golb` with the name of your app.

���Υ��ץꥱ�������ϡ�`golb` �Ȥ���̾���ˤ��뤳�Ȥˤ��ޤ���
����ϡ�`blog` �ε��ɤߤǤ���
���ץ��̾���ϼ�ͳ���ѹ����ƹ����ޤ��󤬡��ѹ��������� `golb` �Ȥ���ñ�줬�ФƤ������ѹ����̾�����֤������Ƥ���������

To make a new app we'll use the command

���������ץ����ˤϡ����Υ��ޥ�ɤ�Ȥ��ޤ���

    merb-gen app golb

Set up the configuration files for your application, this lets Merb know what 
gems to load for plugins and generators.

��ʬ�Υ��ץꥱ�����������깽���ե���������ꤷ�Ƥ���������
�������뤳�Ȥǡ��ץ饰����䥸���ͥ졼���Ǥɤ� gems ���ɤ߹���Τ���Merb ���Τ뤳�Ȥ��Ǥ���褦�ˤʤ�ޤ���

`config/init.rb`

    use_orm :datamapper

    use_test :rspec

  	dependencies "dm-validations"


Now add a `config/database.yml` file with the following:

�ޤ� `config/database.yml` �ե�����򼡤Τ褦�����ƤǺ������Ƥ�������:

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

���: DataMapper �ˤϡ��ǥե���Ȥ� database.yml �ե�������������� rake ���������Ѱդ���Ƥ��ޤ���
    
    dm:db:database_yaml
    
You can also put a database URI in development.rb (or other environments) just as easily:

�ޤ� development.rb (�ޤ���¾�δĶ�����ե�����) �˥ǡ����١��� URI �����ꤹ�뤳�Ȥ��ñ�Ǥ�:

    Merb::BootLoader.after_app_loads do
      DataMapper.setup(:default, 'mysql://user:pass@localhost/database')
    end
      
Now we're ready to rock and roll ...

�����ޤǤ����顢��å�����뤹������ϤǤ��ޤ���...
