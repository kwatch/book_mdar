## Views

(TODO) - form helpers
(TODO) - mention you can use other template languages

### Partials

Use the partial method to render a partial from the current directory. If you pass a hash as the second argument the contents will be made available as local variables in the partial.

�����ȥǥ��쥯�ȥ�Ǥ���ʬ�ƥ�ץ졼�Ȥ����̤���ˤϡ�partial �᥽�åɤ�Ȥ��ޤ���
�⤷�� 2 �����Ȥ��ƥϥå�����Ϥ��ȡ������ͤ���ʬ�ƥ�ץ졼����Ǥϥ������ѿ��Ȥ��ƻ��Ѳ�ǽ�Ǥ���

    partial :post, {:comments => @post.comments}

To display the latest posts on our blog's front page, we use the :with and :as arguments to render a collection.

��ʬ�Υ֥��Υե��ȥڡ����Ǻǿ��� posts ��ɽ�����뤿��ˡ�:with �� :as ��Ȥäƥ��쥯���������̤��ޤ���

    partial :post, :with => @posts, :as => post