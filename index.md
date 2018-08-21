---
title: Flutter中文网
layout: page
homepage: true
comment: false
keywords: flutter
hide_title: true
---
<div class="homepage__illustration">
    <h1 class="homepage__illustration--text">
        <span> 最新版本：</span>
        &nbsp;
        <a href="https://medium.com/flutter-io/flutter-release-preview-1-943a9b6ee65a?linkId=53249457">Flutter Preview 1.0 </a>
    </h1>
    <img src="{{site.cdn}}/images/homepage/header-illustration.png"
         class="homepage__illustration--image"
         alt="Illustration with a mobile phone, a pencil, and an abstract drawing of widgets.">
</div>

<section class="homepage__key_points card">
    <h1 class="homepage__title">
        极速构建漂亮的原生应用
    </h1>

    <div class="homepage__tagline">
    Flutter是谷歌的移动UI框架，可以快速在iOS和Android上构建高质量的原生用户界面。
    Flutter可以与现有的代码一起工作。在全世界，Flutter正在被越来越多的开发者和组织使用，并且Flutter是完全免费、开源的。
    </div>

    <div class="homepage__button_row">
    <a href="/get-started/install/" class="get-started-button">快速开始</a>
    <a target="_blank" onclick="_track('juejin-homepage')" href="https://juejin.im/tag/Flutter?utm_source=flutterchina&utm_medium=word&utm_content=btn&utm_campaign=q3_website" style="width: 160px; border: #009bea 1px solid;background-color: white; color: #0074bb;text-transform:none; padding:15px" class="get-started-button">掘金Flutter社区</a>
    </div>

    <div class="key-points">

        <div class="homepage__key_point">
            <div class="homepage__key_point_title">快速开发</div>
            <p>
            毫秒级的热重载，修改后，您的应用界面会立即更新。使用丰富的、完全可定制的widget在几分钟内构建原生界面。
            </p>
        </div>

        <div class="homepage__key_point">
            <div class="homepage__key_point_title">富有表现力和灵活的UI</div>

            <p>
            快速发布聚焦于原生体验的功能。分层的架构允许您完全自定义，从而实现难以置信的快速渲染和富有表现力、灵活的设计。
            </p>
        </div>

        <div class="homepage__key_point">
            <div class="homepage__key_point_title">原生性能</div>
            <p>
            Flutter包含了许多核心的widget，如滚动、导航、图标和字体等，这些都可以在iOS和Android上达到原生应用一样的性能。
            </p>
        </div>

    </div>
</section>


<section class="homepage__hot_reload card">
    <h1>快速开发</h1>

    <p>
        Flutter的热重载可帮助您快速地进行测试、构建UI、添加功能并更快地修复错误。在iOS和Android模拟器或真机上可以在亚秒内重载，并且不会丢失状态。
    </p>

    <div class="hot-reload-gif-container">
        <img src="{{site.cdn}}/images/intellij/hot-reload.gif" class="hot-reload-gif" alt="Make a change in your code, and your app is changed instantly.">
    </div>
</section>

<section class="homepage__beautiful_uis card ">
    <h1>富有表现力，漂亮的用户界面</h1>

    <p>
    使用Flutter内置美丽的Material Design和Cupertino（iOS风格）widget、丰富的motion API、平滑而自然的滑动效果和平台感知，为您的用户带来全新体验。
    </p>

    <div class="screenshot-list">
        <img class="screenshot" src="{{site.cdn}}/images/homepage/screenshot-1.png" width="270" height="480" alt="Brand-first shopping design">
        <img class="screenshot" src="{{site.cdn}}/images/homepage/screenshot-2.png" width="270" height="480" alt="Fitness app design">
        <img class="screenshot" src="{{site.cdn}}/images/homepage/screenshot-3.png" width="270" height="480" alt="Contact app design">
        <img class="screenshot" src="{{site.cdn}}/images/homepage/ios-friendlychat.png" width="270" height="480" alt="iOS chat app design">
    </div>

    <p>
    浏览 <a href="/widgets/">widget 目录</a>.
    </p>
</section>

<section class="homepage__reactive_framework card">
    <h1>现代的，响应式框架</h1>

    <p>
    使用Flutter的现代、响应式框架，和一系列基础widget，轻松构建您的用户界面。使用功能强大且灵活的API（针对2D、动画、手势、效果等）解决艰难的UI挑战。
    </p>
{% prettify dart %}
class CounterState extends State<Counter> {
  int counter = 0;

  void increment() {
    // 告诉Flutter state已经改变, Flutter会调用build()，更新显示
    setState(() {
      counter++;
    });
  }

  Widget build(BuildContext context) {
    // 当 setState 被调用时，这个方法都会重新执行.
    // Flutter 对此方法做了优化，使重新执行变的很快
    // 所以你可以重新构建任何需要更新的东西，而无需分别去修改各个widget
    return new Row(
      children: <Widget>[
        new RaisedButton(
          onPressed: increment,
          child: new Text('Increment'),
        ),
        new Text('Count: $counter'),
      ],
    );
  }
}
{% endprettify %}

    <p>
    浏览 <a href="/widgets/">widget 目录</a>
    ，了解更多关于
    <a href="/widgets-intro/">响应式框架</a>.
    </p>

</section>

<section class="homepage__interop card">
    <h1>访问本地功能和SDK</h1>
    <p>
    通过平台相关的API、第三方SDK和原生代码让您的应用变得强大易用。
    Flutter允许您复用现有的Java、Swift或ObjC代码，访问iOS和Android上的原生系统功能和系统SDK。
    </p>
    <div>
    访问平台功能非常简单。以下是<a href="https://github.com/flutter/flutter/tree/master/examples/platform_channel">interop example（互操作示例）</a>中的一个片段：
     </div>
{% prettify dart %}
Future<Null> getBatteryLevel() async {
  var batteryLevel = 'unknown';
  try {
    int result = await methodChannel.invokeMethod('getBatteryLevel');
    batteryLevel = 'Battery level: $result%';
  } on PlatformException {
    batteryLevel = 'Failed to get battery level.';
  }
  setState(() {
    _batteryLevel = batteryLevel;
  });
}
{% endprettify %}

    <p>
    了解如何使用 <a href="/using-packages/">packages</a>, 或编写
    <a href="/platform-channels/">平台通道</a>,
    来访问原生代码、API、SDK。
    </p>

</section>

<section class="homepage__features card">
    <h1>统一的应用开发体验</h1>

    <p>
    Flutter拥有丰富的工具和库，可以帮助您轻松地同时在iOS和Android系统中实现您的想法和创意。
    如果您没有任何移动端开发体验，Flutter是一种轻松快捷的方式来构建漂亮的移动应用程序。
    如果您是一位经验丰富的iOS或Android开发人员，则可以使用Flutter作为视图(View)层，
    并可以使用已经用Java / ObjC / Swift完成的部分（Flutter支持混合开发）。
    </p>

    <div class="feature-lists">

        <div class="feature-list-group">
            <h3>构建</h3>

            <h4>漂亮的用户界面</h4>

                <ul>
                    <li>丰富的2D GPU加速API</li>
                    <li>响应式框架</li>
                    <li>动画/运动API</li>
                    <li>Material组件和Cupertino widgets</li>
                </ul>

            <h4>流畅的编码体验</h4>

            <ul>
                <li>亚秒级，有状态的热重载</li>
                <li>IntelliJ: 代码重构、补全等</li>
                <li>Dart语言和核心库</li>
                <li>包管理器</li>
            </ul>

            <h4>全功能的应用程序</h4>

            <ul>
                <li>与移动操作系统API和SDK交互</li>
                <li>Maven/Java</li>
                <li>Cocoapods/ObjC/Swift</li>
            </ul>
        </div>

        <div class="feature-list-group">
            <h3>优化</h3>

            <h4>测试</h4>

            <ul>
                <li>单元测试</li>
                <li>集成测试</li>
                <li>设备上测试</li>
            </ul>

            <h4>调试</h4>

            <ul>
                <li>IDE 调试器</li>
                <li>基于Web的调试器</li>
                <li>async/await aware</li>
                <li>Expression evaluator</li>
            </ul>

            <h4>性能分析</h4>

            <ul>
                <li>时间线（Timeline）</li>
                <li>CPU和内存</li>
                <li>应用内性能图标</li>
            </ul>
        </div>

        <div class="feature-list-group">
            <h3>部署</h3>

            <h4>编译</h4>

            <ul>
                <li>Native ARM 代码</li>
                <li>Dead code elimination</li>
            </ul>

            <h4>发布</h4>

            <ul>
                <li>App Store</li>
                <li>Play Store</li>
            </ul>
        </div>

    </div>

    <p>
     了解详细的<a href="/technical-overview/">Flutter技术总览</a>
    </p>
</section>

<section class="homepage__try_flutter card">

    <div class="homepage__try_today">现在就试试Flutter，入门很简单！</div>

    <div class="homepage__button_row">
    <a href="/get-started/install/" class="get-started-button">开始使用</a>
    </div>

</section>

