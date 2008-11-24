## RSpec

RSpec is a testing framework  which uses a Domain Specific Language or DSL to
provide more human readable test code.

RSpec はテスティングフレームワークであり、問題領域専用言語 (Domain Specific Language、DSL) を使うことで人間にとって読みやすいテストコードが書けるようにしているのが特徴です。


When using stubs with RSpec you can roughly categorise the methods you are 
going to use into two categories. On one side you have the `stub!` and 
`should_receive` methods which refine what methods you expect to be called 
with what parameters and potentially what they should return in the case of 
the test being run. On the other side you have assertions which test the output 
and value or variables. The `should` method is primarily used when asserting
specific results.

RSpec でスタブを使うときは、使うメソッドを大まかに 2 つにカテゴリ分けできます。1 つめは `stub!` と `should receive` メソッドであり、これらは実行するテストケースにおいて、どのメソッドが呼び出されることを期待しているのか、どんな引数を伴うのか、そしてどんな値を返すべきかを定義します〔訳注: "refine" は "define" の間違い?〕。もう 1 つはアサーションであり、出力と変数または値をテストします。特定の結果をアサーションで確かめるときは、主に `should` メソッドが使われます。


#### What to test?

(TODO) - how to write good test and what should just trust works