---
title: PHP的设计模式之行为模式（二）
urlname: php-behavioral-design-patterns-part2
date: 2018-04-05 14:52:10
category: 设计模式
tags: design-patterns
---

## Behavioral Patterns（行为模式）
#### Command（命令模式）
将一个请求封装为一个对象，从而使我们可用不同的请求对客户进行参数化；对请求排队或者记录请求日志，以及支持可撤销的操作。命令模式是一种对象行为型模式，其别名为动作(`Action`)模式或事务(`Transaction`)模式。

为什么需要命令模式

1. 使用命令模式，能够让请求发送者与请求接收者消除彼此之间的耦合，让对象之间的调用关系更加灵活。

2. 使用命令模式可以比较容易地设计一个命令队列和宏命令（组合命令），而且新的命令可以很容易地加入系统

有关命令模式的一个经典的例子，就是电视机与遥控器。有这样的对应关系：电视机是请求的接收者，遥控器是请求的发送者。遥控器上有一些按钮，不同的按钮对应电视机的不同操作。这些按钮就是对应的具体命令类。抽象命令角色由一个命令接口来扮演，有三个具体的命令类实现了抽象命令接口，这三个具体命令类分别代表三种操作：打开电视机、关闭电视机和切换频道。
<!-- more -->
```php
<?php
//抽象命令角色
abstract class Command{
  protected $receiver;
  function __construct(TV $receiver)
  {
      $this->receiver = $receiver;
  }
  abstract public function execute();
}
//具体命令角色 开机命令
class CommandOn extends Command
{
  public function execute()
  {
      $this->receiver->action();
  }
}
//具体命令角色 关机机命令
class CommandOff extends Command
{
  public function execute()
  {
      $this->receiver->action();
  }
}
//命令发送者   --遥控器
class Invoker
{
  protected $command;
  public function setCommand(Command $command)
  {
      $this->command = $command;
  }

  public function send()
  {
      $this->command->execute();
  }
}
//命令接收者 Receiver =》 TV
class TV
{
  public function action()
  {
      echo "接收到命令，执行成功".PHP_EOL;
  }
}

//实例化命令接收者 -------买一个电视机
$receiver = new TV();
//实例化命令发送者-------配一个遥控器
$invoker  = new Invoker();
//实例化具体命令角色 -------设置遥控器按键匹配电视机
$commandOn = new CommandOn($receiver);
$commandOff = new CommandOff($receiver);
//设置命令  ----------按下开机按钮
$invoker->setCommand($commandOn);
//发送命令
$invoker->send();
//设置命令  -----------按下关机按钮
$invoker->setCommand($commandOff);
//发送命令
$invoker->send();
```
在实际使用中，命令模式的 `receiver` 经常是一个抽象类，就是对于不同的命令，它都有对应的具体命令接收者。命令模式的本质是对命令进行封装，将发出命令的责任和执行命令的责任分割开。

#### Visitor（访问者模式）
表示一个作用于某对象结构中的各元素的操作。它使你可以在不改变各元素类的前提下定义作用于这些元素的新操作。
```php
<?php
//具体元素
class Superman{
    public $name;
    public function doSomething(){
        echo '我是超人，我会飞'.PHP_EOL;
    }
    public function accept(Visitor $visitor){
        $visitor->doSomething($this);
    }
}
//具体访问者
class Visitor{
    public function doSomething($object){
        echo '我可以返老还童到'.$object->age = 18;
    }
}
//实例化具体对象
$man = new Superman;
//使用自己的能力
$man->doSomething();
//通过添加访问者，把访问者能能力扩展成自己的
$man->accept(new Visitor);
```
