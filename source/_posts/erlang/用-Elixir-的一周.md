title: 用 Elixir 的一周
date: 2015-12-27 16:38:08
tags:
  - Elixir
  - erlang
categories:
  - Elixir
---


大约一周前我开始学习Elixir. 关于这个我也只是有些模糊的印象但还没有仔细去看。

但在Dave Thomas 出版了 [Programming Elixir](https://pragprog.com/book/elixir/programming-elixir)之后一切都发生了改变. 
 Dave Thomas 帮我修订过Erlang这本书并且是Ruby的倡导者, 只要Dave对什么产生了兴趣那绝不是空穴来风.
 
 Dave 对Elixir很感兴趣, 在他的书里这样写道:
 
 ```
 I came across Ruby in 1998 because I was an avid 
reader of comp.lang.misc (ask your parents). I 
downloaded it, compiled it, and fell in love. 
As with any time you fall in love,
it’s difficult to explain why. 
It just worked the way I work, 
and it had enough depth to keep me interested.

Fast forward 15 years. All that time I’d been 
looking for something new that gave me the same feeling.

I came across Elixir a while back, but for some 
reason never got stuck in. But a few months ago I 
was chatting with Corey Haines. I was
bemoaning the fact that I wanted to find a 
way to show people functional programming concepts 
without the kind of academic trappings those books 
seem to attract. He told me to look again at Elixir. I
did, and I felt the same way I felt when I first saw Ruby.
 ```
 
 
 我能体会.纯粹的感官体验. 就像我知道一件事是对的但还不知道原因，
 几周甚至几年后这个问题总能回答出来. Malcolm Gladwell在Blink: 
 The Power of Thinking Without Thinking一书中曾探讨过这个问题. 
 某些领域专家们总能凭直觉判断事情的正确与否，但却给不出具体原因.

但我发现Dave 的描述后，我很想知道为什么他会这样.

无独有偶, Simon St. Laurent也出了本Elixir的书. 
Simon的 Introducing Erlang… 一书表现不俗，我和他还通过邮件沟通过几次，还有有些熟悉的. 
从Pragmatic Press 和O'Reilly 出版社都在争着出版Elixir可见一斑。关于Erlang VM, 我确实一窍不通。

我给Dave和Simon 发了封邮件，之后他们借给我了样书，现在可以开始阅读了 …  谢了 …

### 上周我下载了elixir 开始学习…

没多久我觉得就上手了. 确实好东西. 有趣的是Erlang和Elixir 实际上是同源的.
事实上也确实这样，他们都会被EVM (Erlang Virtual Machine)编译 - 
大家管这个EVM 叫 “Beam” VM 但为了和JVM区别就管他叫EVM 吧.

Erlang和Elixir为啥有相同的“语意”? 这得从底层谈起. 
垃圾回收, 独立并发机制, 错误处理和代码装载机制都是一样的.
 还有: 他们都运行在相同的VM里. 这也是Scala 和 Akka区别于 Erlang的原因. 
Scala和Akka是运行在JVM 上的, 垃圾回收和代码装载机制从本质上就不同.


Elixir最明显的特点就是语法的表示，和Ruby很像.  简单易懂，还有很多外部资源。

Erlang’s 的语法源自 Prolog 还受到smalltalk, CSP 和功能性编程语言的影响.
 Elixir 受到Erlang 和Ruby的影响. 类型匹配, 功能优先级错误处理机制都是从Erlang来的.sigils, 
 语法简写从Ruby 来。 当然也有自己的创新,  |> 管道操作, Prologs DCGs 和Haskell monads 
 (简化了一些类似于Unix的管道操作) 宏引用,是从lisp quasiquote得来，还有逗号操作符.

Elixir更新了AST机制 , 和Erlang AST独有展示的模式不同,
 Elixir AST 统一了模式这使得meta-programming 更简单.
 
 
 实现起来也中规中矩，只是有几处需要注意. 字符串插入 (主意不错) ：
 
```
 IO.puts "...#{x}..."
```

获取x并按格式打印出来. 但只对简单模式的x起作用.

 这可以通过从Elixir调用Erlang的方法来弥补
 
 IO.puts “…#{pp(x)}…” 就可以. 只是需要把 pp(x) 改成
 
 ```
 def pp(x) do 
    :io_lib.format("~p", [x])
    |> :lists.flatten
    |> :erlang.list_to_binary
end
 ```
 
 Erlang 描述如下:
 
 ```Erlang
 pp(X) ->
  list_to_binary(lists_flatten(li_lib:format("~p),[X])))
 ```
 
 这和Elixir的描述一样. Elixir的写法也更容易阅读. |> 操作符用来把io_lib:format 的结果输入到 
 lists:flatten 然后再到list_to_binary. 跟unix的管道符|一样
 
 
 Elixir区别与Erlang在 - 变量可复用. 
  结果集始终可以表示为static-single-assignment (SSA) . 
  但在循环结构里千万别这么做.好在 Elixir只用了递归而没有循环结构. 
  如果循环结构里引用了可变的参数，那远端EVM就没法编译了. 
  当在SSA顺序结构中使用变量时, EVM 就知道如何处理.循环结构Elixir就没办法了. 
  其根源可追溯到LLVM汇编- 那就是另一个问题了.


## 编程语言设计的三定律

- 你做对的，无人为你提。

- 你做错的，有人跟你急。

- 难点必须不断重复解释。


有些语言在一些方面做得很好，它们正确，优雅，易于理解，但没人不辞麻烦地提及这些。

错误的地方非常糟糕。你成了笨蛋，如果好处比重大于坏处，你可能被原谅。那些你想在以后消除的坏处，
却因为向后兼容性或者是有些傻子（或曰你的狂粉）已经用所有那些坏处写了无数行代码等原因，而不能动。


难以理解的内容是真正倒霉的事情。你必须一遍又一遍地解释，直到你吐血，可还是有些人永远不懂，
你必须写上百邮件和数千文字来一遍又一遍地解释这些内容是什么意思以及它们为什么会如此。
对于一个语言的设计者或作者来说，这是一个痛苦的深渊。

我将要提到的几件事就是我认为落入这三类情况中的。

在我开始前，我要说 Elixir 做对了好多好多的事情，好处远远大于坏处。

关于 Elixir 的好处是，及时改正它的坏处还不算晚。
这只能在无数代码行被写下和众多程序员开始使用它之前才能做到——所以只有少数时间来解决这些问题了。
