## Views

(TODO) - form helpers
(TODO) - mention you can use other template languages

### Partials

Use the partial method to render a partial from the current directory. If you pass a hash as the second argument the contents will be made available as local variables in the partial.

カレントディレクトリでの部分テンプレートを描写するには、partial メソッドを使います。
もし第 2 引数としてハッシュを渡すと、その値は部分テンプレート内ではローカル変数として使用可能です。

    partial :post, {:comments => @post.comments}

To display the latest posts on our blog's front page, we use the :with and :as arguments to render a collection.

自分のブログのフロントページで最新の posts を表示するために、:with と :as を使ってコレクションを描写します。

    partial :post, :with => @posts, :as => post