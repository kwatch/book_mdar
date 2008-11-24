## Stories

(TODO) - finish stories section

RSpec Stories are use to replace the specification phase in requirements 
gathering, in the form of scenarios. So we have both a spec and a integration 
tests.

RSpec のストーリーは要件収集における仕様策定フェーズを、シナリオの形で置き換えます。そのため、仕様と統合テストの両方を手に入れることができます。


Add this line to your app's test environment:

自分のアプリの test 環境に次の行を追加してください:

	dependency "merb_stories"
	
Now generate your story:

次に自分のストーリーを生成します:

	merb-gen story mystory

Now run your story:

続いてストーリーを実行しましょう:

	MERB_ENV=test rake story[mystory]
  
Yes, you must include the square brackets.

角カッコは必要ですので注意してください。


Now fill out your story. There are some differences to Rails' versions. 
The best places to look for help are in the Merb code itself:

ここで、ストーリーを埋めてしまいましょう。Rails の場合とは少し違う点があります。いちばん参考になるのは、Merb のコード自身です:

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


