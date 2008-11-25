## Gotchas

### Merb
<table>
    <th>
        The Rails way
    </th>
    <th>
        The Merb way
    </th>
    <tr>
        <td>
            script/server
        </td>
        <td>
            merb
        </td>
    </tr>
    <tr>
        <td>
            script/console
        </td>
        <td>
            merb -i
        </td>
    </tr>
    <tr>
        <td>
            script/generate
        </td>
        <td>
            merb-gen
        </td>
    </tr>
    <tr>
        <td>
           redirect_to blog_path(@blog)
        </td>
        <td>
           redirect url(:blog, @blog)
        </td>
    </tr>
    <tr>
        <td>
           respond_to
        </td>
        <td>
           provides :xml, :js, :yaml
        </td>
    </tr>
    <tr>
        <td>
           format
        </td>
        <td>
           content_type
        </td>
    </tr>
    <tr>
        <td>
          format.html
        </td>
        <td>
           only_provides :html
        </td>
    </tr>
    <tr>
        <td>
           render :xml => @post
        </td>
        <td>
           render @post
        </td>
    </tr>
    <tr>
        <td>
          render :file => 'public/404.html', :status => 404
        </td>
        <td>
           raise NotFound
        </td>
    </tr>
    <tr>
        <td>
          logger
        </td>
        <td>
           Merb.logger
        </td>
    </tr>
    <tr>
        <td>
          before_filter
        </td>
        <td>
           before
        </td>
    </tr>
    <tr>
        <td>
          render :partial
        </td>
        <td>
           partial
        </td>
    </tr>
    <tr>
        <td>
          f.text_field :name
        </td>
        <td>
          text_control :first_name
        </td>
    </tr>
    <tr>
        <td>
          RAILS_ENV
        </td>
        <td>
          Merb.environment
        </td>
    </tr>    
</table>

#### Freezing Gems
As Merb is spilt up into various gems, and it's hard to keep update with each one it's a good idea to freeze them into your application, so an update to one gem doesn't break your app.

Merb ��ʣ���� gems ��ʬ����Ƥ��ꡢ���줾��򹹿���³����ΤϺ���Ǥ���
���Τ��ᡢ������ʬ�Υ��ץꥱ����������� (freeze) ����ΤϤ����ͤ��Ǥ���

The easiest way to freeze a gem is to add -i gems as a command line option to specify the location for the installed gem. And then add the gem as a dependency in your `init.rb`.

Gem ����뤹��Τ˺Ǥ��ñ����ˡ�ϡ�-i gems �򥳥ޥ�ɥ饤�󥪥ץ����Ȥ����դ��ơ����󥹥ȡ���Ѥߤ� gem �ξ�����ꤹ�뤳�ȤǤ������줫�� `init.rb` ����� gem ���¸ʪ�Ȥ����ɲä��ޤ���

    gem install aquarium -i gems

When running this command from the root of your merb application, it will install the gem inside the gem directory

Merb ���ץꥱ�������Υ롼�ȥǥ��쥯�ȥ�Ǥ��Υ��ޥ�ɤ�¹Ԥ���ȡ�gem �ǥ��쥯�ȥ����� gem �����󥹥ȡ��뤵��ޤ���������: "gems" �ǥ��쥯�ȥ�δְ㤤?��

If you want to freeze the version of the gem that you have installed which is from trunk, you'll need to find where your gems are located and pass that parameter to the gem install command.

������: Git ��ݥ��ȥ�Ρ�trunk ���饤�󥹥ȡ��뤷�� gem �ΥС���������뤷������硢��ʬ�� gems ���ɤ��ˤ���Τ���Ĵ�١������ gem install ���ޥ�ɤΥѥ�᡼�����Ϥ��Ƥ��ޤ���

    gem environment gemdir
    
As I have installed Ruby via port my gem folder is located at `/opt/local/lib/ruby/gems/1.8`.
To freeze the `aquarium` gem I have from trunk I would need to run:

ɮ�Ԥξ�硢port������: MacPorts �Τ��ȡͤ� Ruby �򥤥󥹥ȡ��뤷���Τǡ�gem �ե������ `/opt/local/lib/ruby/gems/1.8` �ˤʤäƤ��ޤ���
���Ȥ��� trunk ������äƤ��� `aquarium` gem ����뤷�����Ȥ��ϡ�����¹Ԥ���ɬ�פ�����Ǥ��礦:

    gem install /opt/local/lib/ruby/gems/1.8/cache/aquarium-0.4.1/ -i gems

If you want to freeze merb itself you need to add this to your `init.rb`, then run the following:

Merb ���켫�Τ���뤷�������ϡ���������: "require 'merb-freezer'" �Τ��ȡͤ� `init.rb` ���ɲä���ɬ�פ�����ޤ���
�ޤ����Τ��ȡ���������: �� rake ���ޥ�ɡͤ�¹Ԥ��Ƥ���������

	require 'merb-freezer'
	
	rake freeze:core
	rake freeze:more
	rake freeze:plugins
	 
Once the `merb` gem is frozen, you can run merb with `frozen-merb`. If you want to update your frozen gem version, pass the update parameter to the rake task:

���ä��� `merb` gem ����뤵�줿���Ȥϡ�`frozen-merb` ���ޥ�ɤ� merb ��¹Ԥ��ޤ���
��뤷�� gem �ΥС������򹹿��������Ȥ��ϡ������Τ���Υѥ�᡼���� rake ���������Ϥ��Ƥ��ޤ�:

	rake freeze:core UPDATE=true

### DataMapper
(TODO) - DM / AR diffs

### RSpec