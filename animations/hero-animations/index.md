---
layout: page
title: "Flutter Hero动画"
permalink: /animations/hero-animations/
summary: Hero指的是可以在路由(页面)之间“飞行”的widget，可以跨页面进行对象共享，这是一个非常棒的特性...
keywords: Flutter英雄动画, hero动画
---

<div class="whats-the-point" markdown="1">

<p> <a id="whats-the-point" class="anchor" href="#whats-the-point" aria-hidden="true"><span class="octicon octicon-link"></span></a>你会学到什么：</p>
- Hero指的是可以在路由(页面)之间“飞行”的widget。
- 使用Flutter的Hero widget创建hero动画。
- 将 hero从一个路由飞到另一个路由。
- 将 hero 的形状从圆形转换为矩形，同时将其从一个路由飞到另一个路由的过程中进行动画处理。
- Flutter中的Hero widget实现了通常称为 *共享元素转换* 或 *共享元素动画*的动画风格。

</div>

你可能多次看过 hero 动画。例如，路由显示代表待售物品的缩略图列表。选择一个条目会将其跳转到一个新路由，新页面中包含更多详细信息和“购买”按钮。 在Flutter中将图片从一个路由飞到另一个路由称为**hero动画**，尽管相同的动作有时也称为 *共享元素转换*。

本指南演示如何构建标准 hero 动画，以及在“飞行”过程中将图片从圆形转换为方形的 hero 动画。

<aside class="alert alert-info" markdown="1">
**例子**<br>

本指南的这些链接中提供了各种风格的 hero 动画的示例：

* [标准的 hero 动画](#standard-hero-animation-code)
* [Radial hero 动画 ](#radial-hero-animation-code)

</aside>

* TOC Placeholder
{:toc}


<aside class="alert alert-info" markdown="1">
**刚接触Flutter?**<br>本页假定您知道如何使用Flutter的 widget创建布局。有关更多信息，请参阅 [在Flutter中构建布局](/tutorials/layout/)。
</aside>

<aside class="alert alert-info" markdown="1">
**术语:**
一个 [_Route_](/routing-and-navigation/) 在Flutter中代表一个页面，以后再Flutter中提到页面时指的就是路由。
</aside>

你可以在 Flutter中用 Hero widget中创建这个动画。当 hero 通过动画从源页面飞到目标页面时，目标页面（不包括 hero ）逐渐淡入视野。通常， hero 是用户界面的一小部分，如图片，它通常在两个页面都有。从用户的角度来看， hero 在页面之间“飞翔”。本指南介绍如何创建以下 hero 动画：

**标准 hero 动画**<br>

一个*标准的 hero 动画*将一个 hero 从一个页面飞到一个新的页面，通常会以不同尺寸降落在不同位置。我们看一个视频（youtube上，需要翻墙）：

该视频以慢速录制，显示了一个典型示例。在页面中心轻击脚蹼，将它们飞到新的蓝色页面的左上角，尺寸更小。在蓝色页面中轻击脚蹼（或使用设备返回键）将脚蹼飞回原来页面。

<!--
  Use this instead of the default YouTube embed code so that the embed
  is reposnisve.
-->
<div class="embed-container"><iframe src="https://www.youtube.com/embed/CEcFnqRDfgw?rel=0" frameborder="0" allowfullscreen></iframe></div>

如果视频看不到没关系，我们为您录制了Flutter Gallery示例中的一个Gif:

<div style="text-align: center; padding: 30px"><img src="/images/hero.gif" style="border: #eee 1px solid"/></div>

**Radial hero 动画**<br>

在Radial(径向) hero 动画中，在 hero 在页面之间“飞行”的同时，其形状从圆形变为矩形。我们看一个视频（youtube上的，需要翻墙）：

该视频以慢速录制，显示了径向 hero 动画的示例。在开始时，一行三个圆形图片出现在页面底部。点击任何一个圆形图片都会将图片飞到一个以正方形显示它的新页面。点击正方形图片将 hero 飞回原页面，并以圆形显示。

<div class="embed-container"><iframe src="https://www.youtube.com/embed/LWKENpwDKiM?rel=0" frameborder="0" allowfullscreen></iframe></div>

在详细来了解[标准](/animations/hero-animations/#standard-hero-animations) 或[radial](/animations/hero-animations/#radial-hero-animations) hero 动画之前 ，请阅读 hero 动画的[基本结构](/animations/hero-animations/#basic-structure) 以了解如何构建 hero 动画代码，并了解了解Flutter如何[在幕后](/animations/hero-animations/#behind-the-scenes)执行 hero 动画。

<a name="basic-structure"></a>
## Hero动画的基本结构

<div class="whats-the-point" markdown="1">

<b> <a id="whats-the-point" class="anchor" href="#whats-the-point" aria-hidden="true"><span class="octicon octicon-link"></span></a> **重点是什么?**</b>

* 在不同路由中使用两个 hero  widget，但使用匹配的标签来实现动画。
* 导航器管理包含应用程序路由的栈。
* 从导航器栈中推入或弹出路由会触发动画。
* Flutter框架会计算一个[补间矩形](https://docs.flutter.io/flutter/animation/RectTween-class.html) ，用于定义在从源路由“飞行”到目标路由时 hero 的边界。在“飞行”过程中， hero 会移动到应用程序上的一个叠加层，以便它出现在两个页面之上。

</div>

<aside class="alert alert-info" markdown="1">
**术语:**如果您对补间(tweens)的概念还不了解，请参阅 [Flutter 动画教程](/tutorials/animation/)。
</aside>

 hero 动画是使用两个 [Hero](https://docs.flutter.io/flutter/widgets/Hero-class.html)  widget实现的：一个描述源路由中的 widget，另一个描述目标路由中的 widget。从用户的角度来看， hero 似乎是共享的，只有开发者需要了解这个实现细节。

<aside class="alert alert-info" markdown="1">
**关于对话框的注意事项:**

 hero 从一个PageRoute飞到另一个。对话框（例如，通过`showDialog()`显示的）使用的事PopupRoutes，它们不是PageRoutes。至少现在，你不能将 hero 飞到一个对话框。有关将来的开发（以及可能的解决方法），请关注此 [issue](https://github.com/flutter/flutter/issues/10667) 。

</aside>

 hero 动画代码具有以下结构:

1. 定义一个起始 hero  widget，称为*源 hero *。 hero 指定其图形表示（通常是图片）和识别标记，并且位于源路由定义的当前显示的 widget树中。
2. 定义一个结束的 hero  widget，称为*目标 hero *。这位 hero 也指定了它的图形表示，以及与源 hero 相同的标记。重要的是**两个 hero  widget都使用相同的标签创建**，通常是代表底层数据的对象。为了获得最佳效果， hero 应该有几乎相同的 widget树。
3. 创建一个包含目标 hero 的路由。目标路由定义了动画结束时的 widget树。
4. 通过导航器将目标路由入栈来触发动画。Navigator推送和弹出操作会为每对 hero 配对，并在源路由和目标路由中使用匹配的标签触发 hero 动画。

Flutter计算从起点到终点对 hero 界限进行动画处理的补间（生成每一帧大小和位置），并在叠加层中执行动画。

下一节将详细的介绍整个过程。

## 在幕后

以下描述了Flutter如何执行从一个路由飞到另一个路由的转换。

<img src="images/hero-transition-0.png" alt="Before the transition the source hero appears in the source route">

在转换之前，源 hero 会在源路由的 widget树中等待。目标路由尚不存在，叠加层为空。

---

<img src="images/hero-transition-1.png" alt="The transition begins">

将路由push到导航器(即跳转到新页面)时会触发动画。在t = 0.0时，Flutter执行以下操作：

* 使用Material motion规范中所述的曲线运动计算目标 hero 的路径。现在Flutter知道 hero 在哪里结束。
* 将目标 hero 放置在叠加层中，与源 hero 的位置和大小相同。将 hero 添加到叠加层会更改其Z序，以使其出现在所有路由的顶部
* 将源 hero 移出路由。

---

<img src="images/hero-transition-2.png" alt="The hero flies in the overlay to its final position and size">

当 hero “飞行”时，其矩形边界使用 hero 的 [`createRectTween`](https://docs.flutter.io/flutter/widgets/CreateRectTween.html)属性中指定的 [Tween ](https://docs.flutter.io/flutter/animation/Tween-class.html)进行动画。默认情况下，Flutter使用[MaterialRectArcTween](https://docs.flutter.io/flutter/material/MaterialRectArcTween-class.html)的一个实例，该实例沿曲线路径对矩形的对角进行动画处理。（有关使用不同的补间动画的示例，请参阅 [径向 hero ](/animations/hero-animations/#radial-hero-animations)动画。）

---

<img src="images/hero-transition-3.png" alt="When the transition is complete, the hero is moved from the overlay to the destination route">

“飞行”完成时:

* Flutter将 hero  widget从叠加层移动到目标路由。叠加层现在是空的。
* 目标 hero 出现在目标路由的最终位置。
* 源 hero 恢复到其路由。

---

Pop路由(返回时)执行相同的过程，将 hero 的动画设置恢复到其在源路由中的大小和位置。

### 基本类

本指南中的示例使用以下类来实现 hero 动画：

[Hero](https://docs.flutter.io/flutter/widgets/Hero-class.html)
: 从源路由飞到目标路由的 widget。为源路由定义一个 hero ，为目标路由定义另一个 hero ，并为每个标签分配相同的标签。Flutter为具有匹配标签的 hero 配对。

[Inkwell](https://docs.flutter.io/flutter/material/InkWell-class.html)
: 指定点击 hero 时发生的情况。InkWell的`onTap()`方法构建新路由并将其push到导航器的栈。

[Navigator](https://docs.flutter.io/flutter/widgets/Navigator-class.html)
: 导航器管理一个路由栈。从导航器栈中push或pop路由会触发动画(打开新页或返回上一页)。

[Route](https://docs.flutter.io/flutter/widgets/Route-class.html)
: 指定一个路由或页面。除了最基本的应用之外，大多数应用都有多条路由。

## 标准 hero 动画

<div class="whats-the-point" markdown="1">

<b> <a id="whats-the-point" class="anchor" href="#whats-the-point" aria-hidden="true"><span class="octicon octicon-link"></span>**重点是什么？**</a></b>

* 使用MaterialPageRoute、CupertinoPageRoute指定路由，或使用PageRouteBuilder构建自定义路由。本节中的示例使用MaterialPageRoute。
* 通过将目标图片包装到SizedBox中来过渡结束时的图片大小。
* 将目标图片放入布局 widget中，更改图片的位置。这个示例使用Container。

</div>

<a name="standard-hero-animation-code"></a>
<aside class="alert alert-info" markdown="1">
**标准 hreo动画的代码**<br>

以下每个示例都演示了将图片从一个路由飞到另一个路由。本指南将详细介绍第一个示例。<br><br>

[hero_animation](https://github.com/flutter/website/tree/master/_includes/code/animation/hero_animation/)
: 将 hero 代码封装在自定义的PhotoHero widget中。如 Material motion规范中所述，沿着曲线路径来给 hero 执行动画。

[basic_hero_animation](https://github.com/flutter/website/tree/master/_includes/code/animation/basic_hero_animation/)
: 直接使用 hero  widget。本指南中未介绍这个示例，仅供您参考。

</aside>

### 发生了什么?

使用Flutter的 hero  widget可以轻松地将图片从一个路由飞到另一个路由。使用MaterialPageRoute指定新路由时，图片将沿着弯曲的路径“飞行”，如[Material Design运动规范所述](https://material.io/guidelines/motion/movement.html)。

[创建一个新的Flutter示例](/get-started/test-drive/)并使用[GitHub目录中](https://github.com/flutter/website/tree/master/_includes/code/animation/hero_animation/)的文件进行更新。

运行示例:

* 点击主页路由的图片，将其放到一个新的路由上，在不同的位置和比例显示相同的照片。
* 通过点击图片或使用设备的返回键返回到前一路由。
* 您可以使用该`timeDilation` 属性进一步减缓过渡。

### PhotoHero 类

自定义的PhotoHero类在点击时维护 hero 及其大小、图片和行为。PhotoHero构建了以下 widget树：

<img src="images/photohero-class.png" alt="widget tree for the PhotoHero class">

代码如下：

<!-- skip -->
{% prettify dart %}
class PhotoHero extends StatelessWidget {
  const PhotoHero({ Key key, this.photo, this.onTap, this.width }) : super(key: key);

  final String photo;
  final VoidCallback onTap;
  final double width;

  Widget build(BuildContext context) {
    return new SizedBox(
      width: width,
      child: new Hero(
        tag: photo,
        child: new Material(
          color: Colors.transparent,
          child: new InkWell(
            onTap: onTap,
            child: new Image.asset(
              photo,
              fit: BoxFit.contain,
            ),
          ),
        ),
      ),
    );
  }
}
{% endprettify %}

重要信息:

* InkWell包裹图片，来轻松给源和目标 hero 添加点击事件。
* 使用透明色定义“Material” widget可使图片在飞向目标时看到背景。
* SizedBox在动画的开始和结束处指定 hero 的大小。
* 将图片的`fit`属性设置为`BoxFit.contain`，可以确保图片在转换过程中尽可能大而不改变其长宽比。

### HeroAnimation 类

HeroAnimation类创建源和目标PhotoHeroes，并设置过渡。然后将它作为应用程序MaterialApp的home属性，启动程序时则首先会显示它。

代码如下：

<!-- skip -->
{% prettify dart %}
class HeroAnimation extends StatelessWidget {
  Widget build(BuildContext context) {
    [[highlight]]timeDilation = 5.0; // 1.0 means normal animation speed.[[/highlight]]

    return new Scaffold(
      appBar: new AppBar(
        title: const Text('Basic Hero Animation'),
      ),
      body: new Center(
        [[highlight]]child: new PhotoHero([[/highlight]]
          photo: 'images/flippers-alpha.png',
          width: 300.0,
          [[highlight]]onTap: ()[[/highlight]] {
            [[highlight]]Navigator.of(context).push(new MaterialPageRoute<Null>([[/highlight]]
              [[highlight]]builder: (BuildContext context)[[/highlight]] {
                return new Scaffold(
                  appBar: new AppBar(
                    title: const Text('Flippers Page'),
                  ),
                  body: new Container(
                    // The blue background emphasizes that it's a new route.
                    color: Colors.lightBlueAccent,
                    padding: const EdgeInsets.all(16.0),
                    alignment: Alignment.topLeft,
                    [[highlight]]child: new PhotoHero([[/highlight]]
                      photo: 'images/flippers-alpha.png',
                      width: 100.0,
                      [[highlight]]onTap: ()[[/highlight]] {
                        [[highlight]]Navigator.of(context).pop();[[/highlight]]
                      },
                    ),
                  ),
                );
              }
            ));
          },
        ),
      ),
    );
  }
}
{% endprettify %}

重要信息：

* 当用户点击包含源 hero 的InkWell时，代码将使用MaterialPageRoute创建目标路由。将目标路由push到导航栈会触发动画。
* 该Container将PhotoHero放置在目标路由AppBar下方的左上角。
* 目标PhotoHero 的`onTap()`方法会pop出导航栈(返回)，触发将Hero飞回原路由的动画。
* 在调试时使用`timeDilation`属性来减缓动画。

---

## Radial hero 动画

<div class="whats-the-point" markdown="1">

<b> <a id="whats-the-point" class="anchor" href="#whats-the-point" aria-hidden="true"><span class="octicon octicon-link"></span>**重点是什么？**</a></b>

*  *径向变换* 动画会将圆形转换为方形。
*  在将 hero 从源路由“飞行”到目路由时，径向* hero *动画执行径向变换
*  MaterialRectCenterArcTween定义补间动画。
*  使用PageRouteBuilder构建目标路由。
  </div>

将 hero 从一个路由转移到另一个路由时，将其从圆形转换为矩形形状是平滑的效果，您可以使用 hero  widget实现。为了实现这一点，代码为两个剪辑形状：圆形和方形的**相交部分**提供动画。在整个动画中，圆形剪辑（和图片）从`minRadius`放大到`maxRadius`，而方形剪辑大小保持不变。同时，图片从源路由中的位置飞到目标路由中的位置。有关此过渡的视觉示例，请参阅Material motion规范中的[径向变换](https://material.io/guidelines/motion/transforming-material.html#transforming-material-radial-transformation)。

这个动画可能看起来很复杂（而且确实如此），但您可以 **根据需要来自定义这些示例**，它们会为您完成这些繁重的工作。

<a name="radial-hero-animation-code"></a>
<aside class="alert alert-info" markdown="1">
**Radial hero 动画代码**<br>

以下每个示例都演示了径向 hero 动画。本指南将介绍第一个示例。<br>

[radial_hero_animation](https://github.com/flutter/website/tree/master/_includes/code/animation/radial_hero_animation)
: Material motion 规范中描述的径向 hero 动画。

[basic_radial_hero_animation](https://github.com/flutter/website/tree/master/_includes/code/animation/basic_radial_hero_transition)
: 径向 hero 动画最简单的例子。目标页面没有Scaffold、 Card、Column或Text。本指南中未描述供参考的基本示例。

[radial_hero_animation_animate_rectclip](https://github.com/flutter/website/tree/master/_includes/code/animation/radial_hero_animation_animate_rectclip)
: 通过添加剪辑矩形大小的动画来扩展radial_hero_animaton示例。本指南中未介绍此更高级的示例，仅供您参考。

</aside>

<aside class="alert alert-info" markdown="1">
**提示:**径向 hero 动画涉及圆形与正方形的相交。即使通过`timeDilation`放慢动画速度，也很难看清楚，因此您可以考虑 在开发过程中启用Flutter的[可视化调试模式](/debugging/#visual-debugging)。
</aside>

### 发生了什么?

下图显示了动画开始时（`t = 0.0`）和结束时（`t = 1.0`）的剪辑图片。

<img src="images/radial-hero-animation.png" alt="visual diagram of
radial transformation from beginning to end">

蓝色渐变（表示图片）表示剪辑形状相交的位置。在转换开始时，相交的部分是一个圆形剪辑（[ClipOval](https://docs.flutter.io/flutter/widgets/ClipOval-class.html)）。在转换过程中，ClipOval从`minRadius`过渡到`maxRadius`，而[ClipRect](https://docs.flutter.io/flutter/widgets/ClipRect-class.html) 保持固定大小。在转换结束时，圆形和矩形剪辑的交集产生与 hero  widget大小相同的矩形。换句话说，在过渡结束时，图片不再被剪切。

参照[创建一个新的Flutter示例](/get-started/test-drive/)并使用[GitHub目录中](https://github.com/flutter/website/tree/master/_includes/code/animation/radial_hero_animation)的文件进行更新运行。


运行示例：

- 点击三个圆形缩略图中的一个，将图片作为新路由中间的大正方形，然后遮蔽原始路由。
- 通过点击图片或使用设备的返回键，返回到前一路由。
- 您可以使用`timeDilation` 属性减缓过渡。

### Photo 类

Photo类构建保存图片的 widget树：

<!-- skip -->
{% prettify dart %}
class Photo extends StatelessWidget {
  Photo({ Key key, this.photo, this.color, this.onTap }) : super(key: key);

  final String photo;
  final Color color;
  final VoidCallback onTap;

  Widget build(BuildContext context) {
    return [[highlight]]new Material([[/highlight]]
      // Slightly opaque color appears where the image has transparency.
      [[highlight]]color: Theme.of(context).primaryColor.withOpacity(0.25),[[/highlight]]
      child: [[highlight]]new InkWell([[/highlight]]
        onTap: [[highlight]]onTap,[[/highlight]]
        child: [[highlight]]new Image.asset([[/highlight]]
            photo,
            fit: BoxFit.contain,
          )
      ),
    );
  }
}
{% endprettify %}

重要信息：

- Inkwell捕获点击手势，在“飞行”过程中，InkWell在其第一个Material上产生点击水波效果。
- Material widget有颜色(主题色，不透透明度为0.25)，因此图片的透明部分会用此颜色呈现。这确保了即使对于具有透明度的图片也很容易看到圆形到正方形的过渡。
- Photo类在它的 widget树中不包括Hero。为了使动画起作用， hero 包装RadialExpansion widget，该 widget包装 hero 。

### RadialExpansion类

RadialExpansion widget，作为该示例的核心，构建了在转换过程中剪切图片的 widget树。剪切后的形状是由一个圆形剪辑（“飞行”期间放大）与一个矩形剪辑（大小始终保持不变）相交而成的。

它构建了以下 widget树:

<img src="images/radial-expansion-class.png" alt="widget tree for the RadialExpansion widget">

代码如下：

<!-- skip -->
{% prettify dart %}
class RadialExpansion extends StatelessWidget {
  RadialExpansion({
    Key key,
    this.maxRadius,
    this.child,
  }) : [[highlight]]clipRectSize = 2.0 * (maxRadius / math.SQRT2),[[/highlight]]
       super(key: key);

  final double maxRadius;
  final clipRectSize;
  final Widget child;

  @override
  Widget build(BuildContext context) {[[/highlight]]
    return [[highlight]]new ClipOval([[/highlight]]
      child: [[highlight]]new Center([[/highlight]]
        child: [[highlight]]new SizedBox([[/highlight]]
          width: clipRectSize,
          height: clipRectSize,
          child: [[highlight]]new ClipRect([[/highlight]]
            child: [[highlight]]child,[[/highlight]]  // Photo
          ),
        ),
      ),
    );
  }
}
{% endprettify %}

重要信息：

-  hero 包装RadialExpansion widget。

- 当 hero “飞行”时，其大小发生变化，并且由于它限制了其孩子的大小，RadialExpansion widget更改其自身大小以匹配。

- RadialExpansion动画由两个重叠的剪辑创建。

- 该示例使用[MaterialRectCenterArcTween](https://docs.flutter.io/flutter/material/MaterialRectCenterArcTween-class.html)定义了Tween。 hero 动画的默认“飞行”路径使用 hero 的边角作为插值(过渡的属性)。该方法会影响 hero 在径向转换过程中的长宽比，因此新的“飞行”路径使用MaterialRectCenterArcTween来使用每个 hero 的中心点作为Tween的插值。

  代码如下：

  ```dart
  static RectTween _createRectTween(Rect begin, Rect end) {
    return new MaterialRectCenterArcTween(begin: begin, end: end);
  }
  ```

   hero 的“飞行”路径仍然沿着弧线，但图片的长宽比保持不变。

---

## 资源

编写动画时以下资源可能会有所帮助：

[Flutter动画](/animations/)
: 列出了Flutter动画的可用文档。如果您还不了解补间动画，请查看 [Animations tutorial](/tutorials/animation/).

[Flutter API文档](https://docs.flutter.io/)
: 所有Flutter库的参考文档。详情请参阅[动画库](https://docs.flutter.io/flutter/animation/animation-library.html) 。

[Flutter Gallery](https://github.com/flutter/flutter/tree/master/examples/flutter_gallery)
: Flutter演示程序。展示了许多Material Design widget和其他Flutter功能。其中的[Shrine示例](https://github.com/flutter/flutter/tree/master/examples/flutter_gallery/lib/demo/shrine) 实现了hero动画。

[Material motion 规范](https://material.io/guidelines/motion/)
: 描述Material Design应用程序的动作效果。