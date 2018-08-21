---
layout: page
title: "给新页面传值"
permalink: /cookbook/navigation/passing-data/
summary: Flutter教程-本文介绍了在Flutter中打开新的页面（路由）并传值
keywords: Flutter路由传值
---

通常，我们不仅要导航到新的页面 ，还要将一些数据传给页面。例如，我们想传一些关于我们点击的条目的信息。

请记住：页面只是Widgets&trade;。在这个例子中，我们将创建一个Todos列表。当点击一个todo时，我们将导航到一个显示关于待办事项信息的新页面 （Widget）。

## Directions

  1. 定义一个Todo类。
  2. 显示Todos列表。
  3. 创建一个显示待办事项详情的页面。
  4. 导航并将数据传递到详情页。

## 1. 定义一个 Todo 类

首先，我们需要一种简单的方法来表示Todos(待办事项)。在这个例子中，我们将创建一个包含两部分数据的类：标题和描述。

```dart
class Todo {
  final String title;
  final String description;

  Todo(this.title, this.description);
}
```

## 2. 创建一个Todos列表

其次，我们要显示一个Todos列表。在这个例子中，我们将生成20个待办事项并使用ListView显示它们。有关使用列表的更多信息，请参阅[基础 List](/cookbook/lists/basic-list/)。

### 生成Todos列表

```dart
final todos = new List<Todo>.generate(
  20,
  (i) => new Todo(
        'Todo $i',
        'A description of what needs to be done for Todo $i',
      ),
);
```

### 使用ListView显示Todos列表

```dart
new ListView.builder(
  itemCount: todos.length,
  itemBuilder: (context, index) {
    return new ListTile(
      title: new Text(todos[index].title),
    );
  },
);
```

到现在为止，我们生成了20个Todo并将它们显示在ListView中！

## 3. 创建一个显示待办事项(todo)详情的页面

现在，我们将创建我们的第二个页面 。页面的标题将包含待办事项的标题，页面正文将显示说明。

由于这是一个普通的`StatelessWidget`，我们只需要在创建页面时传递一个Todo！然后，我们将使用给定的Todo来构建新的页面。

```dart
class DetailScreen extends StatelessWidget {
  // Declare a field that holds the Todo
  final Todo todo;

  // In the constructor, require a Todo
  DetailScreen({Key key, @required this.todo}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    // Use the Todo to create our UI
    return new Scaffold(
      appBar: new AppBar(
        title: new Text("${todo.title}"),
      ),
      body: new Padding(
        padding: new EdgeInsets.all(16.0),
        child: new Text('${todo.description}'),
      ),
    );
  }
}
``` 

## 4. 导航并将数据传递到详情页

接下来，当用户点击我们列表中的待办事项时我们将导航到`DetailScreen`，并将Todo传递给`DetailScreen`。

为了实现这一点，我们将实现`ListTile`的[`onTap`](https://docs.flutter.io/flutter/material/ListTile/onTap.html)回调。
在我们的`onTap`回调中，我们将再次调用`Navigator.push`方法。

```dart
new ListView.builder(
  itemCount: todos.length,
  itemBuilder: (context, index) {
    return new ListTile(
      title: new Text(todos[index].title),
      // When a user taps on the ListTile, navigate to the DetailScreen.
      // Notice that we're not only creating a new DetailScreen, we're
      // also passing the current todo to it!
      onTap: () {
        Navigator.push(
          context,
          new MaterialPageRoute(
            builder: (context) => new DetailScreen(todo: todos[index]),
          ),
        );
      },
    );
  },
);
```      

## 完整的例子

```dart
import 'package:flutter/foundation.dart';
import 'package:flutter/material.dart';

class Todo {
  final String title;
  final String description;

  Todo(this.title, this.description);
}

void main() {
  runApp(new MaterialApp(
    title: 'Passing Data',
    home: new TodosScreen(
      todos: new List.generate(
        20,
        (i) => new Todo(
              'Todo $i',
              'A description of what needs to be done for Todo $i',
            ),
      ),
    ),
  ));
}

class TodosScreen extends StatelessWidget {
  final List<Todo> todos;

  TodosScreen({Key key, @required this.todos}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return new Scaffold(
      appBar: new AppBar(
        title: new Text('Todos'),
      ),
      body: new ListView.builder(
        itemCount: todos.length,
        itemBuilder: (context, index) {
          return new ListTile(
            title: new Text(todos[index].title),
            // When a user taps on the ListTile, navigate to the DetailScreen.
            // Notice that we're not only creating a new DetailScreen, we're
            // also passing the current todo through to it!
            onTap: () {
              Navigator.push(
                context,
                new MaterialPageRoute(
                  builder: (context) => new DetailScreen(todo: todos[index]),
                ),
              );
            },
          );
        },
      ),
    );
  }
}

class DetailScreen extends StatelessWidget {
  // Declare a field that holds the Todo
  final Todo todo;

  // In the constructor, require a Todo
  DetailScreen({Key key, @required this.todo}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    // Use the Todo to create our UI
    return new Scaffold(
      appBar: new AppBar(
        title: new Text("${todo.title}"),
      ),
      body: new Padding(
        padding: new EdgeInsets.all(16.0),
        child: new Text('${todo.description}'),
      ),
    );
  }
}
```