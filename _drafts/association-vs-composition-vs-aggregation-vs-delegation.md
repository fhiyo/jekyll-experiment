---
layout: post
date: 2017-07-19 01:30
title: "委譲とその周りの用語について"
categories: [プログラミング, UML]
tags: [c++]
comments: true
published: true
---

委譲 (delegation) ってどんなのだっけ？とよく忘れてしまうのでその辺りに関連した用語を少しまとめてみる．

##### association
関連．クラス間に参照や実体を保持する関係があり，個々のクラスのlifecycleに制約は無く，また所有関係が無くても構わない．

例: 「車」と「走行中の道路」の関係．お互いの関係は

##### aggregation
集約．associationの特別なもので，個々のクラスのlifecycleの制約はないが，所有関係がある (親子関係がある)．
子のオブジェクトは同時に複数の親を持つことはできない．"has-a"関係として関係を捉えることができる．

例: 「車」と「運転手」の関係．運転手は車を運転するときに車に所有されているとみなすことができ，運転手は一つの車を運転しているときは他の車を運転できない (他の車インスタンスを親に持つことができない) ．


##### composition
コンポジション． aggregationの特別なもので，子は親のlifecycleを共有する．つまり，親が死ぬと子も同時に死ぬ．

例: 「車」と「部品」 (タイヤなど) の関係．車が壊れてスクラップされたら部品も同時に無くなる．ただし，車に部品を追加することは可能 (ドライブレコーダーを後から取り付ける，など) ．


##### delegation
委譲．他にも意味として，代表団，使節団というものがある．あるメソッドの実装は他のインスタンスに任せたよ，という状態．


association, aggregation, compositionの関係を図で表すと以下の通りになる．associationが一番広い意味で使われており，aggregationがその中の一部，compositionがaggregationの一部である．

![association-vs-composition-vs-aggregation-vs-delegation](/assets/img/association-vs-composition-vs-aggregation-vs-delegation/association_aggregation_composition.png)

#### delegationの例 (c++で書いた)

```cpp:DelegationSample.cpp
#include <iostream>

class Foo
{
  public:
    Foo();
    virtual void func();
};
Foo::Foo() {}
void Foo::func() {
  std::cout << "Foo func()" << std::endl;
}

class ConcreateFoo : public Foo
{
  public:
    ConcreateFoo();
    virtual void func();
};
ConcreateFoo::ConcreateFoo() {}
void ConcreateFoo::func() {
  std::cout << "ConcreateFoo func()" << std::endl;
}

class DelegateFoo : public Foo
{
  private:
    Foo* foo;
  public:
    DelegateFoo();
    void setFoo(Foo& foo);
    virtual void func();
};
DelegateFoo::DelegateFoo() {
  this->foo = new Foo;
}
void DelegateFoo::setFoo(Foo& foo) {
  this->foo = &foo;
}
void DelegateFoo::func() {
  this->foo->func();
}

int main(int argc, char** argv)
{
  ConcreateFoo c_f = ConcreateFoo();
  std::cout << "Call ConcreateFoo::func()" << std::endl;
  c_f.func();  // Output: ConcreateFoo func()

  DelegateFoo d_f = DelegateFoo();
  std::cout << "Call DelegateFoo::func()" << std::endl;
  d_f.func();  // Output: Foo func()

  d_f.setFoo(c_f);
  std::cout << "Call DelegateFoo::func()" << std::endl;
  d_f.func();  // Output: ConcreateFoo func()

  return 0;
}
```

DelegateFooはfunc()を自分が所有するfooポインタに委譲する形で実装をしている．そのためfooポインタが指すインスタンスによってfunc()の振る舞いが変わる様子が確認できる．


## 参考
[Association, Aggregation and Composition Relationships with Examples](https://www.go4expert.com/articles/association-aggregation-composition-t17264/)
[uml超入門\_第2章](http://objectclub.jp/technicaldoc/uml/umlintro2)
