#步骤 6：从 JSON 编码的文件里读取名字

在这一步中，你可以更改 PirateName 类以便从 JSON 文件中获取名字和称谓的列表。这将给你机会给程序添加的更多名字和称谓。 

##创建 piratenames.json  
 
使用`File > New File… ` 创建一个名为 `piratenames.json` 含有以下内容的 JSON 编码文件。
把文件放在 `1-blankbadge` 你以前写过的 Dart 文件和 HTML 文件的旁边。

```
{ "names": [ "Anne", "Bette", "Cate", "Dawn",
        "Elise", "Faye", "Ginger", "Harriot",
        "Izzy", "Jane", "Kaye", "Liz",
        "Maria", "Nell", "Olive", "Pat",
        "Queenie", "Rae", "Sal", "Tam",
        "Uma", "Violet", "Wilma", "Xana",
        "Yvonne", "Zelda",
        "Abe", "Billy", "Caleb", "Davie",
        "Eb", "Frank", "Gabe", "House",
        "Icarus", "Jack", "Kurt", "Larry",
        "Mike", "Nolan", "Oliver", "Pat",
        "Quib", "Roy", "Sal", "Tom",
        "Ube", "Val", "Walt", "Xavier",
        "Yvan", "Zeb"],
  "appellations": [ "Awesome", "Captain",
        "Even", "Fighter", "Great", "Hearty",
        "Jackal", "King", "Lord",
        "Mighty", "Noble", "Old", "Powerful",
        "Quick", "Red", "Stalwart", "Tank",
        "Ultimate", "Vicious", "Wily", "aXe", "Young",
        "Brave", "Eager",
        "Kind", "Sandy",
        "Xeric", "Yellow", "Zesty"]}
        
```

###关键信息

- 文件包含 JSON 编码的 map ，它包含两个字符串列表。

##编辑 piratebadge.html
使输入框和按钮不可用

```
...
  <div>
    <input type="text" id="inputName" maxlength="15" disabled>
  </div>
  <div>
    <button id="generateButton" disabled>Aye! Gimme a name!</button>
  </div>
...

```
- 在私有名字成功从 JSON 文件中读出来之后，Dart 代码启用输入框和按钮。

##编辑 piratebadge.dart
在文件顶部添加一个 `import`  

```
import 'dart:html';
import 'dart:math' show Random;
import 'dart:convert' show JSON;

import 'dart:async' show Future;

```  

- `dart:async` 库提供异步编程的方案。
-  `Future` 给你提供一种方式，让你可以获得将来的一个值。（对于 JavaScript 开发者：Futures 和 Promises 一样。）

---
把 `names` 和 `appellations` 替换成下面这些静态的，空的列表。

```
class PirateName {
  ...
  static List<String> names = [];
  static List<String> appellations = [];
  ...
}
```
- 确认从这些声明中移除了 `final` 。 

- `[ ]` 相当于 `new List()` 。  

- List 是一种通用的类型- List 可以包含任何类型的对象。如果你打算要一个只包含字符串的列表，你可以声明成 `List<String>` 这样子。
___
添加两个静态的方法到 PirateName 类：

```
class PirateName {
  ...

  static Future readyThePirates() async {
    String path = 'piratenames.json';
    String jsonString = await HttpRequest.getString(path);
    _parsePirateNamesFromJSON(jsonString);
  }
  
  static _parsePirateNamesFromJSON(String jsonString) {
    Map pirateNames = JSON.decode(jsonString);
    names = pirateNames['names'];
    appellations = pirateNames['appellations'];
  }
}
```  
- `readyThePirates` 被 `async` 关键字标记。一个异步的函数会立即返回一个 `Future`对象，所以调用者在等待函数完成的时候可以做其他事情。

- `HttpRequest` 用来检索来自 URL 的数据。

-  `getString()` 方法做一个简单的 GET 请求并返回字符串的操作是很方便的。

-  `getString()` 是异步的，它建立了一个 GET 请求，当这个GET 请完成 求的时候返回一个 `Future` 对象。

-  `await` 表达式，你只可以在异步的方法里使用。在 GET 请求完成之前处于暂停状态。（当 `getString()` 函数完成的时候返回`Future`）
-  在 GET 请求返回了 JSON 字符串之后，代码从字符串中提取私有名字和称谓。

---
添加一个顶级变量

```
SpanElement badgeNameElement;

void main() {
  ...
}
```
- 用存储  span 元素重复使用的方式代替从 DOM 里查询。
___ 

对 `main()` 函数做这些更改

```
void main() {
  InputElement inputField = querySelector('#inputName');
  inputField.onInput.listen(updateBadge);
  genButton = querySelector('#generateButton');
  genButton.onClick.listen(generateBadge);
  
  badgeNameElement = querySelector('#badgeName');
  ...
}
```
- 在全局变量存储这些 span 元素，同时在局部变量里存储 input 元素。

___
然后添加用来得到 JSON 文件里面的名字的代码，同时处理成功和失败的情况。

```
main() async {
  ...
  
  try {
    await PirateName.readyThePirates();
    //on success
    inputField.disabled = false; //enable
    genButton.disabled = false; //enable
    setBadgeName(getBadgeNameFromStorage());
  } catch (arrr) {
    print('Error initializing pirate names: $arrr');
    badgeNameElement.text = 'Arrr! No names.';
  }
}
```

- 用`async` 修饰函数体，这样函数就可以 `await` 关键字了。让 `main` 函数去除 `void` 作为返回值。异步方法必须返回一个 `Future`对象，所以你可以指定 `Future` 作为返回值或者让它为空。
- 调用 ` readyThePirates()` 方法，将会立即返回一个 Future 对象。
- 使用 `await ` 关键字在 `Future` 返回之前执行暂停操作。
- 当 `Future`通过 `readyThePirates()` 函数成功完成返回的时候，设置界面。
- 使用 `try` 和 `catch` 关键字来检测和处理错误。
___

##运行应用
通过 `File > Save All` 保存文件。

通过正确点击 `piratebadge.html`，选择 `Run in Dartium`，运行应用。

如果你想看到应用找不到 `.json` 文件情况下会发生什么事情，那么请在代码里改变文件的名字并重新运行这个程序。

