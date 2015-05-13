#步骤 4 ：创建一个 PirateName 的类

在这一步中，你仅要更改 Dart 的代码，创建一个描述私有的名字的类，当创建这个类的一个实例的时候，随机从列表里选择一个名字和称谓，或者你可以给构造函数取一个名字和称谓。

##编辑 piratebadge.dart
在文件的顶部加入 import  
  
```
import 'dart:html';

import 'dart:math' show Random;
piratebadge.dart
```     
 
##关键信息
- 使用 `show` 关键字，你可以只导入你需要的类，方法，和属性。
- `Random ` 提供了一个随机数的发生器。  

---
在文件的底部加入一个类的声明 

```
...

class PirateName {
}
```  

- 声明这个类的名字  

---
创建一个类级别的 Random 对象

```
class PirateName {
  static final Random indexGen = new Random();
}
```

- `static` 定义类级别的字段，就是说随机数发生器可以被所有的类实例共享。
- Dart 编辑器强调静态的名字。
- 使用 `new` 调用一个构造函数。  

---  
在类中加入两个实例变量,一个定义 first name ，一个定义 appellation 。  

```
class PirateName {
  static final Random indexGen = new Random();
  String _firstName;
  String _appellation;
}
``` 
- 私有变量用`(_).`强调。Dart 没有 private 关键字。  
 
---
在类内创建两个静态的集合，提供 names 和 appellations 这两个集合供选择。

```
class PirateName {
  ...
  static final List names = [
    'Anne', 'Mary', 'Jack', 'Morgan', 'Roger',
    'Bill', 'Ragnar', 'Ed', 'John', 'Jane' ];
  static final List appellations = [
    'Jackal', 'King', 'Red', 'Stalwart', 'Axe',
    'Young', 'Brave', 'Eager', 'Wily', 'Zesty'];
}
```
- `final` 修饰的变量不能更改。
- 列表是 Dart 内置的，使用 List 来创建。
- List 类给列表提供 API。  

---
 给类提供一个构造函数。
 
 ```
 class PirateName {
  ...
  PirateName({String firstName, String appellation}) {
    if (firstName == null) {
      _firstName = names[indexGen.nextInt(names.length)];
    } else {
      _firstName = firstName;
    }
    if (appellation == null) {
      _appellation = appellations[indexGen.nextInt(appellations.length)];
    } else {
      _appellation = appellation;
    }
  }
}
```  
- 构造函数名和类名相同。
- 被包含在花括号 `({ })` 的参数是可选的被命名的参数。
- `nextInt()` 函数可以从随机数发生器里得到一个随机整数。
- 使用方括号 `([ ])` 为列表添加索引。
- 使用 `length` 属性返回列表中元素的个数。
- 代码使用随机数作为列表的索引。    

---
提供一个 `getter` 给私有字段。

```
class PirateName {
  ...
  String get pirateName =>
    _firstName.isEmpty ? '' : '$_firstName the $_appellation';
}
```  
- Getters 是一个提供访问对象的属性的特别方法。
- 三元运算符 `?:` 是 `if-then-else` 语句的简略写法。
- 字符串插入 `('$_firstName the $_appellation') ` 让我们很容易从其他对象构建字符串。
- 大箭头 `( => expr; )` 是 `{ return expr; }` 语法的一个简写。   
 
---
重写 `toString()` 方法。  

```
class PirateName {
  ...
  String toString() => pirateName;
}
```  
- 因为对象实现 `toString()` 函数没有给很多的信息，所以很多类重写了 `toString()`函数 。  
- 当你调用 `print(anObject)` 得到字符串，返回值事实是通过是 `anObject.toString()` 得到的。  
- 在的调试和输出的时候重写 `toString()` 特别有用。  
 
---
在修改 `setBadgeName()` 函数时要使用 PirateName 而不是 String 。    

```
void setBadgeName(PirateName newName) {
  querySelector('#badgeName').text = newName.pirateName;
}
```
- 代码调用 getter 得到一个字符串类型的 PirateName。  
  
---
更改 `updateBadge()`函数的代码，基于输入字段的值生成一个PirateName 对象。  

```
void updateBadge(Event e) {
  String inputName = (e.target as InputElement).value;
  
  setBadgeName(new PirateName(firstName: inputName));
  ...
}
```
- 调用构造函数给可选的命名参数赋值。  

---  
更改 ` generateBadge() ` 生成一个 PirateName 对象，而不是使用 `Anne Bonney`。

```
void generateBadge(Event e) {
  setBadgeName(new PirateName());
}

```  
- 在这种情况下，通过无参调用构造函数。

##运行应用  
使用 ` File > Save All` 保存。  

在 `piratebadge.html` 点击鼠标右键并选择 ` Run in Dartium`，运行应用。

把你的应用和下面的比较。

在输入框中输入。删除输入字段。点击按钮。  




  
