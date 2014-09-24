---
layout: post
title:  "Promise и Deferred. Часть 3. EventMachine."
date:   2013-11-24 12:00
languages: Ruby
categories:
---

В продолжении темы промисов хочется показать, какие возможности в этом отношении есть у других языков, способных работать в асинхнонной парадигме. Одним из таких языков является Ruby в сочетании с фреймворком EventMachine.

В данной статье я не буду долго рассказывать про EventMachine. Просто скажу, что EventMachine - это фреймворк на Ruby, реализующий паттерн Reactor и предоставляющий возможности аналогичные [JBoss Netty](netty), [Apache MINA](mina), [Twisted](twisted), [Node.js](node), libevent and libev.

[netty]: http://www.jboss.org/netty
[mina]: http://mina.apache.org/ 
[twisted]: http://twistedmatrix.com
[node]: http://nodejs.org

<!--more-->

Данный фреймворк предоставляет свою реализацию Deferred, о которой я хотел бы рассказать. Основаная функциональность заключена в модуле EventMachine::Deferrable, который можено подмешать к любому классу и получить в свое распоряжение следующие возможности:

~~~ruby
class TestClass
  include EventMachine::Deferrable
end

test = TestClass.new
test.callback do
	print 'Операция завершилась успешно'
end
test.errback do
	print 'Операция провалилась'
end
~~~

Методы `callback` и `errback` являются аналогами первого и второго аргументами в методе `then` библиотеки Q. Зарезолвить или зареждектить промис в EM можно при помощи методов `succeed` и `fail`.

Также, если вам просто нужно создать Deferred-объект и не хочется ради этого создавать лишний класс — можно воспользоваться готовым классом-пустышкой EventMachine::DefaultDeferrable. В исходном коде он определен таким образом:

~~~ruby
class DefaultDeferrable
  include Deferrable
end
~~~

[Реализация][deferrable] Deferred в EM довольно проста и рекомендуется к ознакомлению всем, кто хочет лучше изнать, что такое Deferred.

[deferrable]: https://github.com/eventmachine/eventmachine/blob/master/lib/em/deferrable.rb