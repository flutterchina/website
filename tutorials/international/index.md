---
layout: tutorial
title: "国际化Flutter App"
permalink: /tutorials/internationalization/
summary: 本文介绍了如何对Flutter应用进行国际化和多语言支持如果您的应用可能会给另一种语言的用户使用，那么您需要“国际化”它。这意味着您在编写应用程序时需要为应用程序支持的每种语言环境...
keywords: Flutter国际化, Flutter多语言支持
---

<div class="whats-the-point" markdown="1">

<b> <a id="whats-the-point" class="anchor" href="#whats-the-point" aria-hidden="true"><span class="octicon octicon-link"></span></a>你将会学到什么:</b>

* 如何跟踪设备的区域(locale)设置（用户的首选语言）
* 如何管理特定于区域环境(locale)的应用程序值.
* 如何使应用程序支持多区域环境

</div>

如果您的应用可能会给另一种语言的用户使用，那么您需要“国际化”它。这意味着您在编写应用程序时需要为应用程序支持的每种语言环境，
设置“本地化”的一些值，如文本和布局。Flutter提供一些widgets和类，以帮助实现国际化，而Flutter的库本身也是国际化的。

接下来的教程主要是根据Flutter MaterialApp类编写的，因为大多数应用程序都是这样编写的。根据较低级别的WidgetsApp类编写的应用程序也可以使用相同的类和逻辑进行国际化。

### 内容

* [设置一个国际化的应用程序: flutter_localizations package](#setting-up)
* [跟踪区域设置: Locale类和Localizations widget](#tracking-locale)
* [加载和获取本地化的值](#loading-and-retrieving)
* [使用打包好的LocalizationsDelegates](#using-bundles)
* [为应用的本地化资源定义一个类](#defining-class)
* [指定应用的supportedLocales参数](#specifying-supportedlocales)
* [指定本地化资源的另一个类](#alternative-class)
* [附录: 使用 Dart intl 工具](#dart-tools)

<aside class="alert alert-info" markdown="1">
**国际化APP的例子**<br>

如果您想通过阅读国际化Flutter应用程序的代码开始，下面是两个小例子。第一个旨在尽可能简单，第二个使用[intl](https://pub.dartlang.org/packages/intl)包提供的API和工具。
如果您没用过Dart的intl包，请参阅[使用Dart intl 工具](#dart-tools)。

* [简单的国际化应用](https://github.com/flutter/website/tree/master/_includes/code/internationalization/minimal/)
* [基于 `intl` 包的国际化应用](https://github.com/flutter/website/tree/master/_includes/code/internationalization/intl/)
</aside>

<a name="setting-up"></a>
## 设置一个国际化的应用程序: the flutter_localizations package

默认情况下，Flutter仅提供美国英语本地化。要添加对其他语言的支持，应用程序必须指定其他MaterialApp属性，并包含一个名为的单独包-"flutter_localizations"。
截至2017年10月，该软件包支持15种语言。

要使用flutter_localizations，请将该包作为依赖项添加到您的`pubspec.yaml`文件中：

{% prettify yaml %}
dependencies:
  flutter:
    sdk: flutter
  flutter_localizations:
    sdk: flutter
{% endprettify %}

接下来，导入flutter_localizations库，并指定MaterialApp的`localizationsDelegates`和`supportedLocales`：

{% prettify dart %}
import 'package:flutter_localizations/flutter_localizations.dart';

new MaterialApp(
 localizationsDelegates: [
   // ... app-specific localization delegate[s] here
   GlobalMaterialLocalizations.delegate,
   GlobalWidgetsLocalizations.delegate,
 ],
 supportedLocales: [
    const Locale('en', 'US'), // English
    const Locale('he', 'IL'), // Hebrew
    // ... other locales the app supports
  ],
  // ...
)
{% endprettify %}

基于WidgetsApp的应用程序类似，只是不需要`GlobalMaterialLocalizations.delegate`。

`localizationsDelegates`列表中的元素是生成本地化值集合的工厂。`GlobalMaterialLocalizations.delegate` 为Material Components库提供了本地化的字符串和其他值。
`GlobalWidgetsLocalizations.delegate`定义widget默认的文本方向，从左到右或从右到左。

有关这些应用程序属性的更多信息，它们所依赖的类型以及如何国际化Flutter应用程序，这些都可以在下面找到。

<a name="tracking-locale"></a>
## 跟踪区域设置: Locale类和Localizations widget

该[`Locale`](https://docs.flutter.io/flutter/dart-ui/Locale-class.html)类是用来识别用户的语言环境。
移动设备支持通过系统设置菜单为所有应用程序设置区域。国际化应用程序通过显示区域设置特定的值进行响应。
例如，如果用户将设备的语言环境从英语切换到法语，则显示“Hello World”的文本widget将用“Bonjour le monde”重建。

[`Localizations`](https://docs.flutter.io/flutter/widgets/Localizations-class.html)小部件定义其子项的区域设置以及子项依赖的本地化资源。
如果系统的语言环境发生变化，[WidgetsApp](https://docs.flutter.io/flutter/widgets/WidgetsApp-class.html)将创建一个Localizations widget并重建它。

您始终可以通过以下方式查找应用的当前区域设置：

{% prettify dart %}
Locale myLocale = Localizations.localeOf(context);
{% endprettify %}

<a name="loading-and-retrieving"></a>
## 加载和获取本地化的值

Localizations widget用于加载和查找包含本地化值的集合的对象。应用程序通过[`Localizations.of(context,type)`](https://docs.flutter.io/flutter/widgets/Localizations/of.html)来引用这些对象。
如果设备的区域设置发生更改，则Localizations widget会自动加载新区域设置的值，然后重新构建使用它们的widget。
发生这种情况是因为Localizations像[InheritedWidget](https://docs.flutter.io/flutter/widgets/InheritedWidget-class.html)一样工作 。
当build函数引用了继承的widget时，会创建对继承的widget的隐式依赖关系。当继承的widget发生更改（Localizations widget的区域设置发生更改时），将重建其依赖的上下文。

本地化值由Localizations widget的 [LocalizationsDelegate](https://docs.flutter.io/flutter/widgets/LocalizationsDelegate-class.html)s 列表加载 。
每个委托必须定义一个异步load() 方法，以生成封装了一系列本地化值的对象。通常这些对象为每个本地化值定义一个方法。

在大型应用程序中，不同的模块或软件包可能会与自己的本地化捆绑在一起。
这就是Localizations widget管理对象表的原因，每个LocalizationsDelegate都有一个(对象表)。
要检索由LocalizationsDelegate`load`方法之一产生的对象，可以指定一个BuildContext和对象的类型。

例如，Material Component widgets的本地化字符串由[MaterialLocalizations](https://docs.flutter.io/flutter/material/MaterialLocalizations-class.html)类定义。
此类的实例由[MaterialApp](https://docs.flutter.io/flutter/material/MaterialApp-class.html)类提供的LocalizationDelegate创建。
它们可以通过`Localizations.of`被获取到：

{% prettify dart %}
Localizations.of<MaterialLocalizations>(context, MaterialLocalizations);
{% endprettify %}

这个特殊的`Localizations.of()`表达式经常使用，所以MaterialLocalizations类提供了一个方便的简写：

{% prettify dart %}
static MaterialLocalizations of(BuildContext context) {
  return Localizations.of<MaterialLocalizations>(context, MaterialLocalizations);
}

/// References to the localized values defined by MaterialLocalizations
/// are typically written like this:

tooltip: MaterialLocalizations.of(context).backButtonTooltip,
{% endprettify %}

<a name="using-bundles">
## 使用打包好的LocalizationsDelegates

为了尽可能小而且简单，flutter软件包中仅提供美国英语值的MaterialLocalizations和WidgetsLocalizations接口的实现。
这些实现类分别称为DefaultMaterialLocalizations和DefaultWidgetsLocalizations。
除非与应用程序的localizationsDelegates参数指定了相同基本类型的不同delegate，否则它们会自动包含 。

flutter_localizations软件包包含称为GlobalMaterialLocalizations和GlobalWidgetsLocalizations的本地化接口的多语言实现。
国际化的应用程序必须按照[设置国际化应用程序](#setting-up)中的说明为这些类指定本地化代理 。

{% prettify dart %}
import 'package:flutter_localizations/flutter_localizations.dart';

new MaterialApp(
 localizationsDelegates: [
   // ... app-specific localization delegate[s] here
   GlobalMaterialLocalizations.delegate,
   GlobalWidgetsLocalizations.delegate,
 ],
 supportedLocales: [
    const Locale('en', 'US'), // English
    const Locale('fr', 'CA'), // canadian French
    // ... other locales the app supports
  ],
  // ...
)
{% endprettify %}

全局本地化delegates构造相应类的特定于语言环境的实例。例如，`GlobalMaterialLocalizations.delegate`是一个产生GlobalMaterialLocalizations实例的LocalizationsDelegate。

截至2017年10月，国际化代理的类支持约[15种语言](https://github.com/flutter/flutter/tree/master/packages/flutter_localizations/lib/src/l10n)

<a name="defining-class"></a>
## 为应用的本地化资源定义一个类

将所有这些放在一起用于国际化应用程序通常从封装应用程序本地化值的类开始。下面的例子是这些类的典型例子。

这个示例的[完整源代码](https://github.com/flutter/website/tree/master/_includes/code/internationalization/intl/)

本示例基于[intl](https://pub.dartlang.org/packages/intl)包提供的API和工具 。[指定本地化资源的另一个类](#alternative-class)描述了一个不依赖于intl包的示例。

DemoLocalizations类包含应用程序的字符串（仅用于示例），该字符串被翻译为应用程序支持的语言环境。
使用Dart的[intl 包](https://pub.dartlang.org/packages/intl)生成的函数`initializeMessages()`来加载翻译的字符串，并使用[`Intl.message()`](https://www.dartdocs.org/documentation/intl/0.15.1/intl/Intl/message.html)查找它们。

{% prettify dart %}
class DemoLocalizations {
  static Future<DemoLocalizations> load(Locale locale) {
    final String name = locale.countryCode.isEmpty ? locale.languageCode : locale.toString();
    final String localeName = Intl.canonicalizedLocale(name);
    return initializeMessages(localeName).then((Null _) {
      Intl.defaultLocale = localeName;
      return new DemoLocalizations();
    });
  }

  static DemoLocalizations of(BuildContext context) {
    return Localizations.of<DemoLocalizations>(context, DemoLocalizations);
  }

  String get title {
    return Intl.message(
      'Hello World',
      name: 'title',
      desc: 'Title for the Demo application',
    );
  }
}
{% endprettify %}

基于intl包的类导入生成的message目录，该目录提供`initializeMessages()`函数和每个语言环境的后备存储`Intl.message()`。
message目录由一个[intl工具](#dart-tools)生成，该工具分析包含Intl.message()调用的类的源代码。在这个示例中，这是DemoLocalizations类。

<a name="specifying-supportedlocales"></a>
## 指定应用程序的supportedLocales参数

虽然Flutter的Material Components library包含对大约16种语言的支持，但默认情况下仅提供英文。
开发人员需要决定支持哪种语言，因为工具库支持与应用程序不同的一组语言环境是没有意义的。

MaterialApp[`supportedLocales`](https://docs.flutter.io/flutter/material/MaterialApp/supportedLocales.html)参数限制语言环境更改。
当用户更改其设备上的区域设置时，新区域如果是列表的成员，则应用程序的`Localizations` widget 就适用于该区域。如
果找不到设备区域设置的精确匹配项（译者语：指语言和地区同时匹配，如中文，中国），则使用第一个匹配区域设置[`languageCode`](https://docs.flutter.io/flutter/dart-ui/Locale/languageCode.html)（译者语：只设置语言，而不指定地区）。
如果失败，则`supportedLocales`使用列表的第一个元素 。

就之前的DemoApp示例而言，该应用只接受美国英语或加拿大法语语言环境，并将美国英语（列表中的第一个语言环境）替换为其他任何内容。

可以提供一个[`localeResolutionCallback`](https://docs.flutter.io/flutter/widgets/LocaleResolutionCallback.html)，在应用获取用户设置的语言区域时会被回调。
例如，要让您的应用程序无条件接受用户选择的任何区域设置：

{% prettify dart %}
class DemoApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return new MaterialApp(
       localeResolutionCallback(Locale locale, Iterable<Locale> supportedLocales) {
         return locale;
       }
       // ...
    );
  }
}
{% endprettify %}

<a name="alternative-class"></a>
## 指定本地化资源的另一个类


之前的DemoApp示例是根据Dart`intl`包定义的。为了简单起见，开发人员可以选择自己的方法来管理本地化值，也可以与不同的i18n框架进行集成。
这个示例的[完整源代码](https://github.com/flutter/website/tree/master/_includes/code/internationalization/minimal/)


在此版本的DemoApp中，包含应用程序本地化的类DemoLocalizations将直接在每种语言Map中包含其所有的翻译：


{% prettify dart %}
class DemoLocalizations {
  DemoLocalizations(this.locale);

  final Locale locale;

  static DemoLocalizations of(BuildContext context) {
    return Localizations.of<DemoLocalizations>(context, DemoLocalizations);
  }

  static Map<String, Map<String, String>> _localizedValues = {
    'en': {
      'title': 'Hello World',
    },
    'es': {
      'title': 'Hola Mundo',
    },
  };

  String get title {
    return _localizedValues[locale.languageCode]['title'];
  }
}
{% endprettify %}

在[简单的国际化应用](https://github.com/flutter/website/tree/master/_includes/code/internationalization/minimal/)中，
DemoLocalizationsDelegate略有不同。它的load方法返回一个[SynchronousFuture](https://docs.flutter.io/flutter/foundation/SynchronousFuture-class.html)， 因为不需要进行异步加载。

{% prettify dart %}
class DemoLocalizationsDelegate extends LocalizationsDelegate<DemoLocalizations> {
  const DemoLocalizationsDelegate();

  @override
  bool isSupported(Locale locale) => ['en', 'es'].contains(locale.languageCode);

  @override
  Future<DemoLocalizations> load(Locale locale) {
    return new SynchronousFuture<DemoLocalizations>(new DemoLocalizations(locale));
  }

  @override
  bool shouldReload(DemoLocalizationsDelegate old) => false;
}
{% endprettify %}

<a name="dart-tools"></a>
## 附录: 使用Dart intl工具

在使用Dart [`intl`](https://pub.dartlang.org/packages/intl)包构建API之前， 您需要查看`intl`包的文档。以下是根据intl软件包本地化应用程序的过程摘要。

示例程序依赖于一个生成的源文件`l10n/messages_all.dart` ，它定义了应用程序使用的所有本地化字符串。

重新构建 `l10n/messages_all.dart` 需要两个步骤.

<ol markdown="1">
<li markdown="1">将应用程序的根目录作为当前目录，从`lib/main.dart`生成`l10n/intl_messages.arb`

{% prettify sh %}
$ flutter pub pub run intl_translation:extract_to_arb --output-dir=lib/i10n lib/main.dart
{% endprettify %}

该`intl_messages.arb`文件是一个JSON格式的map，拥有一个在`main.dart`中定义的`Intl.message()`函数入口。
此文件作为英语和西班牙语翻译的一个模板，`intl_en.arb`和`intl_es.arb`。这些翻译是由您，开发人员创建的。
</li>

<li markdown="1">使用应用程序的根目录作为当前目录，为每个`intl_<locale>.arb`文件生成`intl_messages_<locale>.dart`，并在`intl_messages_all.dart`中导入所有message文件：

{% prettify sh %}
$ flutter pub pub run intl_translation:generate_from_arb --output-dir=lib/l10n \
   --no-use-deferred-loading lib/main.dart lib/l10n/intl_*.arb
{% endprettify %}

DemoLocalizations类使用生成的`initializeMessages()` 函数（定义在`intl_messages_all.dart`）来加载本地化的message并使用`Intl.message()`来查找它们。
</li>

</ol>
