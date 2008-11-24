## RSpec

RSpec is a testing framework  which uses a Domain Specific Language or DSL to
provide more human readable test code.

RSpec �ϥƥ��ƥ��󥰥ե졼�����Ǥ��ꡢ�����ΰ����Ѹ��� (Domain Specific Language��DSL) ��Ȥ����Ȥǿʹ֤ˤȤä��ɤߤ䤹���ƥ��ȥ����ɤ��񤱤�褦�ˤ��Ƥ���Τ���ħ�Ǥ���


When using stubs with RSpec you can roughly categorise the methods you are 
going to use into two categories. On one side you have the `stub!` and 
`should_receive` methods which refine what methods you expect to be called 
with what parameters and potentially what they should return in the case of 
the test being run. On the other side you have assertions which test the output 
and value or variables. The `should` method is primarily used when asserting
specific results.

RSpec �ǥ����֤�Ȥ��Ȥ��ϡ��Ȥ��᥽�åɤ���ޤ��� 2 �Ĥ˥��ƥ���ʬ���Ǥ��ޤ���1 �Ĥ�� `stub!` �� `should receive` �᥽�åɤǤ��ꡢ�����ϼ¹Ԥ���ƥ��ȥ������ˤ����ơ��ɤΥ᥽�åɤ��ƤӽФ���뤳�Ȥ���Ԥ��Ƥ���Τ����ɤ�ʰ�����ȼ���Τ��������Ƥɤ���ͤ��֤��٤�����������ޤ�������: "refine" �� "define" �δְ㤤?�͡��⤦ 1 �Ĥϥ����������Ǥ��ꡢ���Ϥ��ѿ��ޤ����ͤ�ƥ��Ȥ��ޤ�������η�̤򥢥��������ǳΤ����Ȥ��ϡ���� `should` �᥽�åɤ��Ȥ��ޤ���


#### What to test?

(TODO) - how to write good test and what should just trust works