## Stories

(TODO) - finish stories section

RSpec Stories are use to replace the specification phase in requirements 
gathering, in the form of scenarios. So we have both a spec and a integration 
tests.

RSpec �Υ��ȡ��꡼���׷�����ˤ�������ͺ���ե������򡢥��ʥꥪ�η����֤������ޤ������Τ��ᡢ���ͤ�����ƥ��Ȥ�ξ����������뤳�Ȥ��Ǥ��ޤ���


Add this line to your app's test environment:

��ʬ�Υ��ץ�� test �Ķ��˼��ιԤ��ɲä��Ƥ�������:

	dependency "merb_stories"
	
Now generate your story:

���˼�ʬ�Υ��ȡ��꡼���������ޤ�:

	merb-gen story mystory

Now run your story:

³���ƥ��ȡ��꡼��¹Ԥ��ޤ��礦:

	MERB_ENV=test rake story[mystory]
  
Yes, you must include the square brackets.

�ѥ��å���ɬ�פǤ��Τ���դ��Ƥ���������


Now fill out your story. There are some differences to Rails' versions. 
The best places to look for help are in the Merb code itself:

�����ǡ����ȡ��꡼�����Ƥ��ޤ��ޤ��礦��Rails �ξ��ȤϾ����㤦��������ޤ��������Ф󻲹ͤˤʤ�Τϡ�Merb �Υ����ɼ��ȤǤ�:

	spec/public/test/controller _matchers _spec.rb
	lib/merb-core/test/helpers
	lib/merb-core/test/matchers
	To start you off, here are the steps for a simple integration test:

	steps_for(:homepage) do
	  When("I visit the root") do
	    @mycontroller = get("/")
	  end
	  Then("I should see the home page") do
	    @mycontroller.should respond_successfully
	    @mycontroller.body.should contain("Hello") 
	  end    
	end


