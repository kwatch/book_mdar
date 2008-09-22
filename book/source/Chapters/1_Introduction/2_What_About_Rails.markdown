## What About Ruby On Rails? (Ruby on Rails について)

> [Merb is] Harder, Better, Faster, Stronger, to quote Daft Punk - Max Williams

> Daft Punk を引用するには、[Merb は] より堅くて、より良くて、よりくて、より堅牢である〔訳注: ???〕 - Max Williams

So what's the big deal? We have Ruby on Rails and that's enough, isn't it? 
There is little doubt that Ruby on Rails has rocked the web application 
development world. You have to give credit where credit's due, and Ruby on Rails 
is definitely a great web framework.  However, there is no such thing as a 
one-size fits all solution.  Ruby on Rails is opinionated software which 
provides many benefits such as Convention over Configuration.  On the other 
hand, this also means that Ruby on Rails can be unforgiving if you don't want 
to do things 'the Rails way'.

大きな違いは何だろうか? すでに Ruby on Rails があるし、それで十分じゃね?
Ruby on Rails が Web アプリケーション開発の世界を揺るがしていることはほぼ間違いありません。
人は、賞賛すべきものはきちんと賞賛すべきであり、そして Ruby on Rails は間違いなくすばらしい Web フレームワークです。
しかし、それ 1 つですべての問題を解決してしまうような万能なものはありません。
Ruby on Rails は、たとえば設定より規約 (Convention over Configuration, CoC) のように、たくさんの利点を提供するけど頑固なソフトウェアです。
一方で、'Rails 流' 以外の方法でなにかしたいとき、Ruby on Rails はそれを許してはくれません。
 
Where Rails is opinionated, Merb is agnostic. For example, you can easily use 
your favourite ORM (ActiveRecord, DataMapper, Sequel) or none at all.  
Similarly, you can choose the Javascript library and template language that you 
are most comfortable with, or that best meets the requirements of your specific 
project.

Rails は頑固であり、Merb はそうではありません。
たとえば、もし自分の好きな ORM (ActiveRecord, DataMapper, Sequel) を使いたい、あるいは何も使いたくないと思ったら、簡単にできます。
同様に、JavaScript ライブラリやテンプレート言語でも、自分が使いたいものを、あるいはプロジェクトの要件に最もよく合うものを選ぶことができます。

If performant were a word, Merb would be it.  One of Merb's design mantras is 
"No code is faster than no code".  Merb has super-fast routing and is 
thread-safe. The core functionality is kept separate from the other plugins 
and it uses less Ruby 'magic', making it easier to understand and hack.

もしパフォーマンスが大事なら〔訳注: ???〕、Merb を選ぶといいでしょう。
Merb の設計におけるスローガンのひとつに、『何も書かない (no code) よりも速いコードはない (No code)』というものがあります。
Merb は非常に高速なルーティングを持ち、かつ Merb はスレッドセーフです。
コア機能は他のプラグインから分離されており、Ruby の「魔法」はほぼ使ってないため、理解してハックするのは簡単でしょう。

Rails (and consequently Ruby) has received a lot of criticism for not being 
suitable for large scale web applications, which isn't necessarily true. Merb 
has been built from the outset to prove that Ruby is a viable language for 
building fast and scalable web applications.

Rails (だけでなく Ruby も) は、大規模な Web アプリケーションには向かないという批判を受けてきました。
が、これらは必ずしも正しくはありません。
Merb は、Ruby が高速でスケーラブルな Web アプリケーションを構築できるだけの能力を持った言語であることを証明するための発端となるよう、構築されています。

At the end of the day it's about choice. There are many new Ruby frameworks 
springing up, undoubtedly encouraged by the success of Rails.  In our opinion,
Merb shows the most promise of these.

最終的には、こういったことは選択です〔訳注: ???〕。
明らかに Rails の成功に刺激され、Ruby 用の新しいフレームワークがたくさん現れてきています。
私たちの意見としては、Merb はこれらの中では最も有望です。

If you'd like to take a look at some other frameworks these links should get you 
started:

もし他のフレームワークを見てみたいなら、下のリンクが参考になるでしょう:

- [http://camping.rubyforge.org/files/README.html](http://camping.rubyforge.org/files/README.html)
- [http://www.nitroproject.org/](http://www.nitroproject.org/)
- [http://ramaze.rubyforge.org/](http://ramaze.rubyforge.org/)
- [http://sinatra.rubyforge.org/](http://sinatra.rubyforge.org/)
- [http://halcyon.rubyforge.org/](http://halcyon.rubyforge.org/)
- [http://wisteria.swiftcore.org/](http://wisteria.swiftcore.org/)
- [http://api.mackframework.com/](http://api.mackframework.com/)
