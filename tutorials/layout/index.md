---
layout: tutorial
title: "Building Layouts in Flutter"
sidebar: home_sidebar
permalink: /tutorials/layout/
---

## Introduction

This is a guide to building layouts in Flutter.
We start by explaining Flutter's approach to layout, and
show you how to place single widgets on the screen.
We'll discuss how to to lay widgets out horizontally and vertically,
and then cover some of the most common layout widgets.
Finally, we'll walk through the process of creating a layout for this app:

<img src="images/lakes.png" style="border:1px solid black" alt="finished lakes app that we'll build in 'Building a Layout'">

### Contents

* [Flutter's approach to layout](#approach)
* [Lay out a widget](#lay-out-a-widget)
* [Lay out multiple widgets vertically and horizontally](#rows-and-columns)
  * [Aligning widgets](#alignment)
  * [Sizing widgets](#sizing)
  * [Packing widgets](#packing)
  * [Nesting rows and columns](#nesting)
* [Common layout widgets](#common-layout-widgets)
  * [Standard widgets](#standard-widgets)
  * [Material widgets](#material-widgets)
* [Building a layout](#building)
  * [Step 1: Diagram the layout](#step-1)
  * [Step 2: Implement the title row](#step-2)
  * [Step 3: Implement the button row](#step-3)
  * [Step 4: Implement the text section](#step-4)
  * [Step 5: Implement the image section](#step-5)
  * [Step 6: Put it together](#step-6)
* [Resources](#resources)

<a name="approach"></a>
## Flutter's approach to layout

<div class="panel" markdown="1">

<b> <a id="whats-the-point" class="anchor" href="#whats-the-point" aria-hidden="true"><span class="octicon octicon-link"></span></a>What's the point?</b>

* Widgets are classes used to build UIs.
* Widgets are used for both layout and UI elements.
* Compose simple widgets to build complex widgets.

</div>

The core of Flutter's layout mechanism is widgets. In Flutter, most
everything is a widget&mdash;even layout models are widgets.
The images, icons, and text that you see in a Flutter app  are all widgets.
But things you don't see are also widgets, such as the rows, columns,
and grids that arrange, constrain, and align the visible widgets.

You create a layout by composing widgets to build more complex widgets.
For example, the screenshot on the left shows 3 icons with a label under
each one:

<img src="images/lakes-icons.png" style="border:1px solid black" alt="sample layout">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<img src="images/lakes-icons-visual.png" style="border:1px solid black" alt="sample sample layout with visual debugging turned on">

The second screenshot displays the visual layout, showing a row of
3 columns where each column contains an icon and a label.

<aside class="alert alert-info" markdown="1">
**Note:** Most of the screenshots in this tutorial are displayed with
`debugPaintSizeEnabled` set to true so you can see the visual layout.
For more information, see
[Visual debugging](https://flutter.io/debugging/#visual-debugging), a section in
[Debugging Flutter Apps](https://flutter.io/debugging/#visual-debugging).
</aside>

Here's a diagram of the widget tree for this UI:

<img src="images/sample-flutter-layout.png" alt="node tree representing the sample layout">

Most of this should look as you might expect, but you might be wondering
about the Containers (shown in pink). Container is a widget that allows
you to customize its child widget. Use a Container when you want to
add padding, margins, borders, or background color, to name some of its
capabilities.

In this example, each Text widget is placed in a Container to add margins.
The entire Row is also placed in a Container to add padding around the row.

The rest of the UI in this example is controlled by properties.
Set an Icon's color using its `color` property.
Use Text's `style` property to set the font, its color, weight, and so on.
Columns and Rows have properties that allow you to specify how their
children are aligned vertically or horizontally, and how much space
the children should occupy.

<hr>

<a name="lay-out-a-widget"></a>
## Lay out a widget

<div class="panel" markdown="1">

<b> <a id="whats-the-point" class="anchor" href="#whats-the-point" aria-hidden="true"><span class="octicon octicon-link"></span></a>What's the point?</b>

{% comment %}
* Create an [Image](https://docs.flutter.io/flutter/widgets/Image-class.html),
  [Icon](https://docs.flutter.io/flutter/material/Icon-class.html),
  or [Text](https://docs.flutter.io/flutter/widgets/Text-class.html) widget.
* Add it to a layout widget, such as
  [Center](https://docs.flutter.io/flutter/widgets/Center-class.html),
  [Align](https://docs.flutter.io/flutter/widgets/Align-class.html),
  [SizedBox](https://docs.flutter.io/flutter/widgets/SizedBox-class.html),
  or [ListView](https://docs.flutter.io/flutter/widgets/ListView-class.html),
  to name a few.
* Add the layout widget to the root of the widget tree.
{% endcomment %}
* Even the app itself is a widget.
* It's easy to create a widget and add it to a layout widget.
* To display the widget on the device, add the layout widget to the app widget.
* It's easiest to use
  [Scaffold](https://docs.flutter.io/flutter/material/Scaffold-class.html),
  a widget from the material library, which provides a default banner,
  background color, and has API for adding drawers, snack bars,
  and bottom sheets.
* If you prefer, you can build an app that only uses standard widgets
  from the widgets library.

</div>

How do you layout a single widget in Flutter?
This section shows how to create a simple widget and display it on screen.
It also shows the entire code for a simple Hello World app.

In Flutter,
it takes only a few steps to put text, an icon, or an image on the screen.

<ol markdown="1">

<li markdown="1"> Select a layout widget to hold the object.<br>
    Choose from a variety of [layout widgets](/widgets/) based
    on how you want to align or constrain the visible widget,
    as these characteristics are typically passed on to the
    contained widget.
    This example uses Center which centers its content
    horizontally and vertically.
</li>

<li markdown="1"> Create a widget to hold the visible object.<br>

<aside class="alert alert-info" markdown="1">
**Note:**
Flutter apps are written in the [Dart language](https://www.dartlang.org/).
If you know Java or similar object-oriented coding languages, Dart
will feel very familiar. If not, you might try
[DartPad](https://www.dartlang.org/tools/dartpad), an interactive Dart
playground you can use from any browser. The
[Language Tour](https://www.dartlang.org/guides/language) provides an
overview of the features of the Dart Language.
</aside>

For example, create a Text widget:

<!-- skip -->
{% prettify dart %}
new Text('Hello World', style: new TextStyle(fontSize: 32.0))
{% endprettify %}

Create an Image widget:

<!-- skip -->
{% prettify dart %}
new Image.asset('images/myPic.jpg', fit: ImageFit.cover)
{% endprettify %}

Create an Icon widget:

<!-- skip -->
{% prettify dart %}
new Icon(Icons.star, color: Colors.red[500])
{% endprettify %}

</li>

<li markdown="1"> Add the visible widget to the layout widget.<br>
    All layout widgets have a `child` property if they take a single
    child (for example, Center or Container),
    or a `children` property if they take a list of widgets (for example,
    Row, Column, ListView, or Stack).

Add the Text widget to the Center widget:

<!-- skip -->
{% prettify dart %}
new Center(
  child: new Text('Hello World', style: new TextStyle(fontSize: 32.0))
{% endprettify %}

</li>

<li markdown="1"> Add the layout widget to the page.<br>
   A Flutter app is, itself, a widget and most widgets have a
   [build()](https://docs.flutter.io/flutter/widgets/StatelessWidget/build.html)
   method. Declaring the widget in the app's build method displays the widget
   on the device.

   For a material app, you can add the Center widget directly to the
  `body` property for the home page.

<!-- _code/hello-world/main.dart -->
<!-- skip -->
{% prettify dart %}
class _MyHomePageState extends State<MyHomePage> {
  @override
  Widget build(BuildContext context) {
    return new Scaffold(
      appBar: new AppBar(
        title: new Text(config.title),
      ),
      [[highlight]]body: new Center([[/highlight]]
        [[highlight]]child: new Text('Hello World', style: new TextStyle(fontSize: 32.0)),[[/highlight]]
      ),
    );
  }
}
{% endprettify %}

<aside class="alert alert-info" markdown="1">
**Note:**
The material library implements widgets that follow
[material design principles](https://material.io/guidelines/).
When designing your UI, you can exclusively use widgets from the standard
[widgets library](https://docs.flutter.io/flutter/widgets/widgets-library.html),
or you can use widgets from the [material
library](https://docs.flutter.io/flutter/material/material-library.html).
You can mix widgets from both libraries (resulting in a material app),
you can customize existing widgets,
or you can build your own set of custom widgets.
</aside>

For a non-material app, you can add the Center widget to the app's `build`
method:

<!-- _code/widgets-only/main.dart -->
<!-- skip -->
{% prettify dart %}
// This app doesn't use any material widgets, such as Scaffold.
// Normally, an app that doesn't use Scaffold has a black background
// and the default text color is black. This app changes its background
// to white and its text color to dark grey to mimic a material app.
import 'package:flutter/material.dart';

void main() {
  runApp(new MyApp());
}

class MyApp extends StatelessWidget {
  @override
  [[highlight]]Widget build(BuildContext context)[[/highlight]] {
    return new Container(
      decoration: new BoxDecoration(backgroundColor: Colors.white),
      child: new Center(
        child: [[highlight]]new Text('Hello World',[[/highlight]]
            style: new TextStyle(fontSize: 40.0, color: Colors.black87)),
      ),
    );
  }
}
{% endprettify %}

Note that, by default, the non-material app doesn't include an AppBar, title,
or background color. If you want these features in a non-material app,
you have to build them yourself. This app changes the background color to
white and the text to dark grey to mimic a material app.

</li>

</ol>

That's it! When you run the app, you should see:

<img src="images/hello-world.png" style="border:1px solid black" alt="screenshot of a white background with grey 'Hello World' text.">

**Dart code** (material app): [main.dart](https://raw.githubusercontent.com/flutter/website/master/_includes/_code/hello-world/main.dart)<br>
**Dart code** (widgets-only app): [main.dart](https://raw.githubusercontent.com/flutter/website/master/_includes/_code/widgets-only/main.dart)

<aside class="alert alert-success" markdown="1">
<i class="fa fa-lightbulb-o"> </i> **Tip:**
For a faster development experience, try Flutter's hot reload feature.
Hot reload allows you to modify your code and see the changes without
fully restarting the app. The Flutter plugins for IntelliJ and Atom support hot
reload, or you can trigger it when running apps from the command line.
For more information, see [Hot Reloads vs. Full Application
Restarts](https://flutter.io/intellij-ide/#hot-reloads-vs-full-application-restarts).
</aside>

<hr>

<a name="rows-and-columns"></a>
## Lay out multiple widgets vertically and horizontally

One of the most common layout patterns is to arrange widgets vertically
or horizontally. You can use a Row widget to arrange widgets horizontally,
and a Column widget to arrange widgets vertically.

<div class="panel" markdown="1">

<b> <a id="whats-the-point" class="anchor" href="#whats-the-point" aria-hidden="true"><span class="octicon octicon-link"></span></a>What's the point?</b>

* Row and Column are two of the most commonly used layout patterns.
* Row and Column each take a list of child widgets.
* A child widget can itself be a Row, Column, or other complex widget.
* You can specify how a Row or Column aligns its children, both vertically
  and horizontally.
* You can stretch or constrain specific child widgets.
* You can specify how child widgets use the Row's or Column's available space.

</div>

### Contents

* [Aligning widgets](#alignment)
* [Sizing widgets](#sizing)
* [Packing widgets](#packing)
* [Nesting rows and columns](#nesting)

To create a row or column in Flutter, you add a list of children widgets to a
[Row](https://docs.flutter.io/flutter/widgets/Row-class.html) or
[Column](https://docs.flutter.io/flutter/widgets/Column-class.html) widget.
In turn, each child can itself be a row or column, and so on.
The following example shows how it is possible to nest rows or columns inside
of rows or columns.

This layout is organized as a Row. The row contains two children:
a column on the left, and an image on the right:


<center><img src="images/pavlova-diagram.png" alt="screenshot with callouts showing the row containing two children: a column and an image."></center><br>

The left column's widget tree nests rows and columns.

<center><img src="images/pavlova-left-column-diagram.png" alt="diagram showing a left column broken down to its sub-rows and sub-columns"></center><br>

You'll implement some of Pavlova's layout code in
[Nesting rows and columns](#nesting).

<aside class="alert alert-info" markdown="1">
**Note:** Row and Column are basic primitive widgets for horizontal
and vertical lay outs&mdash;these low-level widgets allow for maximum
customization. Flutter also offers specialized, higher level widgets
that might be sufficient for your needs. For example, instead of Row
you might prefer
[ListItem](https://docs.flutter.io/flutter/material/ListItem-class.html),
an easy-to-use widget with properties for leading and trailing icons,
and up to 3 lines of text.  Instead of Column, you might prefer
[ListView](https://docs.flutter.io/flutter/widgets/ListView-class.html),
a column-like layout that automatically scrolls if its content is too long
to fit the available space.  For more information,
see [Common layout widgets](#common-layout-widgets).
</aside>

<a name="alignment"></a>
### Aligning widgets

You control how a row or column aligns its children using the
`mainAxisAlignment` and `crossAxisAlignment` properties.
For a row, the main axis runs horizontally and the cross axis runs
vertically. For a column, the main axis runs vertically and the cross
axis runs horizontally.

<div class="row"> <div class="col-md-6" markdown="1">

<p></p>
<img src="images/row-diagram.png" alt="diagram showing the main axis and cross axis for a row">

</div> <div class="col-md-6" markdown="1">

<img src="images/column-diagram.png" alt="diagram showing the main axis and cross axis for a column">

</div> </div>

The [MainAxisAlignment](https://docs.flutter.io/flutter/rendering/MainAxisAlignment-class.html)
and [CrossAxisAlignment](https://docs.flutter.io/flutter/rendering/CrossAxisAlignment-class.html)
classes offer a variety of constants for controlling alignment.

<aside class="alert alert-info" markdown="1">
**Note:** When you add images to your project,
you need to update the pubspec file to access them&mdash;this
example uses `Image.asset` to display the images.  For more information,
see this example's [pubspec.yaml
file](https://raw.githubusercontent.com/flutter/website/master/_includes/_code/row/pubspec.yaml),
or [Adding Assets and Images in Flutter](/assets-and-images).
You don't need to do this if you're referencing online images using
`Image.network`.
</aside>

In the following example, each of the 3 images is 100 pixels wide.
The render box (in this case, the entire screen) is more than 300 pixels wide,
so setting the main axis alignment to `spaceEvenly` divides the free
horizontal space evenly between, before, and after each image.

<div class="row"> <div class="col-md-8" markdown="1">

{% include includelines filename="_code/row/main.dart" start=40 count=8 %}

</div> <div class="col-md-3" markdown="1">

<center><img src="images/row-spaceevenly-visual.png" style="border:1px solid black" alt="a row showing 3 images spaced evenly in the row"></center>

**Dart code:** [main.dart](https://raw.githubusercontent.com/flutter/website/master/_includes/_code/row/main.dart)<br>
**Images:** [images](https://github.com/flutter/website/tree/master/_includes/_code/row/images)<br>
**Pubspec:** [pubspec.yaml](https://raw.githubusercontent.com/flutter/website/master/_includes/_code/row/pubspec.yaml)

</div> </div>

Columns work the same way as rows. The following example shows a column
of 3 images, each is 100 pixels high. The height of the render box
(in this case, the entire screen) is more than 300 pixels, so
setting the main axis alignment to `spaceEvenly` divides the free vertical
space evenly between, above, and below each image.

<div class="row"> <div class="col-md-8" markdown="1">

{% include includelines filename="_code/column/main.dart" start=40 count=8 %}

**Dart code:** [main.dart](https://raw.githubusercontent.com/flutter/website/master/_includes/_code/column/main.dart)<br>
**Images:** [images](https://github.com/flutter/website/tree/master/_includes/_code/column/images)<br>
**Pubspec:** [pubspec.yaml](https://raw.githubusercontent.com/flutter/website/master/_includes/_code/column/pubspec.yaml)

</div> <div class="col-md-3" markdown="1">

<img src="images/column-visual.png" style="border:1px solid black" alt="a column showing 3 images spaced evenly in the column">

</div> </div>

<aside class="alert alert-info" markdown="1">
**Note:**
When a layout is too large to fit the device, a red strip appears along the
affected edge. For example, the row in the following screenshot is too
wide for the device's screen:

<center><img src="images/layout-too-large.png" style="border:1px solid black" alt="a row that is too wide, showing a red string along the right edge"></center>

Widgets can be sized to fit within a row or column by using an Expanded widget,
which is described in the [Sizing widgets](#sizing) section below.
</aside>

<a name="sizing"></a>
### Sizing widgets

Perhaps you want a widget to occupy twice as much space as its siblings.
You can place the child of a row or column in an
[Expanded](https://docs.flutter.io/flutter/widgets/Expanded-class.html)
widget to control widget sizing along the main axis.
The Expanded widget has a `flex` property, an integer that determines
the flex factor for a widget. The default flex factor for an Expanded
widget is 1.

For example, to create a row of three widgets where the middle widget is twice
as wide as the other two widgets, set the flex factor on the middle widget to 2:

<div class="row"> <div class="col-md-8" markdown="1">

{% include includelines filename="_code/row-expanded/main.dart" start=40 count=15 %}

</div> <div class="col-md-3" markdown="1">

<img src="images/row-expanded-visual.png" style="border:1px solid black" alt="a row of 3 images with the middle image twice as wide as the others">

**Dart code:** [main.dart](https://raw.githubusercontent.com/flutter/website/master/_includes/_code/row-expanded/main.dart)<br>
**Images:** [images](https://github.com/flutter/website/tree/master/_includes/_code/row-expanded/images)<br>
**Pubspec:** [pubspec.yaml](https://raw.githubusercontent.com/flutter/website/master/_includes/_code/row-expanded/pubspec.yaml)

</div> </div>

To fix the example in the previous section where the row of 3 images was
too wide for its render box, and resulted in the red strip,
wrap each widget with an Expanded widget.
By default, each widget has a flex factor of 1, assigning one-third of
the row to each widget.

<div class="row"> <div class="col-md-8" markdown="1">

{% include includelines filename="_code/row-expanded-2/main.dart" start=40 count=14 %}

</div> <div class="col-md-3" markdown="1">

<img src="images/row-expanded-2-visual.png" style="border:1px solid black" alt="a row of 3 images that are too wide, but each is constrained to take only 1/3 of the row's available space">

**Dart code:** [main.dart](https://raw.githubusercontent.com/flutter/website/master/_includes/_code/row-expanded-2/main.dart)<br>
**Images:** [images](https://github.com/flutter/website/tree/master/_includes/_code/row-expanded-2/images)<br>
**Pubspec:** [pubspec.yaml](https://raw.githubusercontent.com/flutter/website/master/_includes/_code/row-expanded-2/pubspec.yaml)

</div> </div>

<a name="packing"></a>
### Packing widgets

By default, a row or column occupies as much space along its main axis
as possible, but if you want to pack the children closely together,
set its `mainAxisSize` to `MainAxisSize.min`. The following example
uses this property to pack the star icons together.

<div class="row"> <div class="col-md-8" markdown="1">

<!-- _code/packed/main.dart -->
<!-- skip -->
{% prettify dart %}
class _MyHomePageState extends State<MyHomePage> {
  @override
  Widget build(BuildContext context) {
    [[highlight]]var packedRow = new Row[[/highlight]](
      [[highlight]]mainAxisSize: MainAxisSize.min,[[/highlight]]
      children: [
        new Icon(Icons.star, color: Colors.green[500]),
        new Icon(Icons.star, color: Colors.green[500]),
        new Icon(Icons.star, color: Colors.green[500]),
        new Icon(Icons.star, color: Colors.black),
        new Icon(Icons.star, color: Colors.black),
      ],
    );

  // ...
}
{% endprettify %}

</div> <div class="col-md-3" markdown="1">

<img src="images/packed.png" style="border:1px solid black" alt="a row of 5 stars, packed together in the middle of the row">

**Dart code:** [main.dart](https://raw.githubusercontent.com/flutter/website/master/_includes/_code/packed/main.dart)<br>
**Icons:** [Icons class](https://docs.flutter.io/flutter/material/Icons-class.html)<br>
**Pubspec:** [pubspec.yaml](https://raw.githubusercontent.com/flutter/website/master/_includes/_code/packed/pubspec.yaml)

</div> </div>

<a name="nesting"></a>
### Nesting rows and columns

The layout framework allows you to nest rows and columns inside of rows
and columns as deeply as you need. Let's look the code for the outlined section
of the following layout:

<img src="images/pavlova-large-annotated.png" style="border:1px solid black" alt="a screenshot of the pavlova app, with the ratings and icon rows outlined in red">

The outlined section is implemented as two rows. The ratings row contains
five stars and the number of reviews. The icons row contains three
columns of icons and text.

The widget tree for the ratings row:

<center><img src="images/widget-tree-pavlova-rating-row.png" alt="a node tree showing the widgets in the ratings row"></center><br>

The `ratings` variable creates a row containing a smaller row of 5 star icons,
and text:

<!-- _code/pavlova/main.dart -->
<!-- skip -->
{% prettify dart %}
class _MyHomePageState extends State<MyHomePage> {
  @override
  Widget build(BuildContext context) {
    //...

    [[highlight]]var ratings = new Container[[/highlight]](
      padding: new EdgeInsets.all(20.0),
      child: [[highlight]]new Row[[/highlight]](
        mainAxisAlignment: MainAxisAlignment.spaceEvenly,
        children: [
          [[highlight]]new Row[[/highlight]](
            mainAxisSize: MainAxisSize.min,
            children: [
              [[highlight]]new Icon[[/highlight]](Icons.star, color: Colors.black),
              [[highlight]]new Icon[[/highlight]](Icons.star, color: Colors.black),
              [[highlight]]new Icon[[/highlight]](Icons.star, color: Colors.black),
              [[highlight]]new Icon[[/highlight]](Icons.star, color: Colors.black),
              [[highlight]]new Icon[[/highlight]](Icons.star, color: Colors.black),
            ],
          ),
          [[highlight]]new Text[[/highlight]](
            '170 Reviews',
            style: new TextStyle(
              color: Colors.black,
              fontWeight: FontWeight.w800,
              fontFamily: 'Roboto',
              letterSpacing: 0.5,
              fontSize: 20.0,
            ),
          ),
        ],
      ),
    );
    //...
  }
}
{% endprettify %}

<aside class="alert alert-success" markdown="1">
<i class="fa fa-lightbulb-o"> </i> **Tip:**
To minimize the visual confusion that can result from heavily nested layout
code, implement pieces of the UI in variables and functions.
</aside>

The icons row, below the ratings row, contains 3 columns; each column contains
an icon and two lines of text, as you can see in its widget tree:

<img src="images/widget-tree-pavlova-icon-row.png" alt="a node tree for the widets in the icons row">

The `iconList` variable defines the icons row:

<!-- _code/pavlova/main.dart -->
<!-- skip -->
{% prettify dart %}
class _MyHomePageState extends State<MyHomePage> {
  @override
  Widget build(BuildContext context) {
    // ...

    var descTextStyle = new TextStyle(
      color: Colors.black,
      fontWeight: FontWeight.w800,
      fontFamily: 'Roboto',
      letterSpacing: 0.5,
      fontSize: 18.0,
      height: 2.0,
    );

    // DefaultTextStyle.merge allows you to create a default text
    // style that is inherited by its child and all subsequent children.
    [[highlight]]var iconList = new DefaultTextStyle.merge[[/highlight]](
      context: context,
      style: descTextStyle,
      child: new Container(
        padding: new EdgeInsets.all(20.0),
        child: [[highlight]]new Row[[/highlight]](
          mainAxisAlignment: MainAxisAlignment.spaceEvenly,
          children: [
            [[highlight]]new Column[[/highlight]](
              children: [
                new Icon(Icons.kitchen, color: Colors.green[500]),
                new Text('PREP:'),
                new Text('25 min'),
              ],
            ),
            [[highlight]]new Column[[/highlight]](
              children: [
                new Icon(Icons.timer, color: Colors.green[500]),
                new Text('COOK:'),
                new Text('1 hr'),
              ],
            ),
            [[highlight]]new Column[[/highlight]](
              children: [
                new Icon(Icons.restaurant, color: Colors.green[500]),
                new Text('FEEDS:'),
                new Text('4-6'),
              ],
            ),
          ],
        ),
      ),
    );
    // ...
  }
}
{% endprettify %}

The `leftColumn` variable contains the ratings and icons rows, as well as
the title and text that describes the Pavlova:

<!-- _code/pavlova/main.dart -->
<!-- skip -->
{% prettify dart %}
class _MyHomePageState extends State<MyHomePage> {
  @override
  Widget build(BuildContext context) {
    //...

    [[highlight]]var leftColumn = new Container[[/highlight]](
      padding: new EdgeInsets.fromLTRB(20.0, 30.0, 20.0, 20.0),
      child: new Column(
        [[highlight]]children:[[/highlight]] [
          [[highlight]]titleText[[/highlight]],
          [[highlight]]subTitle[[/highlight]],
          [[highlight]]ratings[[/highlight]],
          [[highlight]]iconList[[/highlight]],
        ],
      ),
    );
    //...
  }
}
{% endprettify %}

The left column is placed in a Container to constrain its width.
Finally, the UI is constructed with the entire row (containing the
left column and the image) inside a Card.

<a name="adding-images"></a>
The Pavlova image is from
[Pixabay](https://pixabay.com/en/photos/?q=pavlova&image_type=&cat=&min_width=&min_height=)
and is available under the Creative Commons license.
You can embed an image from the net using `Image.network` but,
for this example, the image is saved to an images directory in the project,
added to the [pubspec
file,](https://raw.githubusercontent.com/flutter/website/master/_includes/_code/pavlova/pubspec.yaml)
and accessed using `Images.asset`. For more information, see
[Adding Assets and Images in Flutter](/assets-and-images).

<!-- _code/pavlova/main.dart -->
<!-- skip -->
{% prettify dart %}
body: new Center(
  child: [[highlight]]new Container[[/highlight]](
    margin: new EdgeInsets.fromLTRB(0.0, 40.0, 0.0, 30.0),
    height: 600.0,
    child: [[highlight]]new Card[[/highlight]](
      child: [[highlight]]new Row[[/highlight]](
        crossAxisAlignment: CrossAxisAlignment.start,
        children: [
          [[highlight]]new Container[[/highlight]](
            width: 440.0,
            child: [[highlight]]leftColumn[[/highlight]],
          ),
          [[highlight]]mainImage[[/highlight]],
        ],
      ),
    ),
  ),
),
{% endprettify %}

<div class="row"> <div class="col-md-3" markdown="1">

**Dart code:** [main.dart](https://raw.githubusercontent.com/flutter/website/master/_includes/_code/pavlova/main.dart)<br>
**Images:** [images](https://github.com/flutter/website/tree/master/_includes/_code/pavlova/images)<br>
**Pubspec:** [pubspec.yaml](https://raw.githubusercontent.com/flutter/website/master/_includes/_code/pavlova/pubspec.yaml)

</div> <div class="col-md-9" markdown="1">

<aside class="alert alert-success" markdown="1">
<i class="fa fa-lightbulb-o"> </i> **Tip:**
The Pavlova example runs best horizontally on a wide device, such as a tablet.
If you are running this example in the iOS simulator, you can select a
different device using the **Hardware > Device** menu. For this example, we
recommend the iPad Pro. You can change its orientation to landscape mode using
**Hardware > Rotate**. You can also change the size of the simulator window
(without changing the number of logical pixels) using **Window > Scale**.
</aside>

</div> </div>

<hr>

<a name="common-layout-widgets"></a>
## Common layout widgets

Flutter has a rich library of layout widgets, but here a few of those most
commonly used. The intent is to get you up and running as quickly as possible,
rather than overwhelm you with a complete list.  For information on other
available widgets, refer to the [Widget Overview](https://flutter.io/widgets/),
or use the Search box in the the [API reference docs](https://docs.flutter.io/).
Also, the widget pages in the API docs often make suggestions
about similar widgets that might better suit your needs.

The following widgets fall into two categories: standard widgets from the
[widgets library,](https://docs.flutter.io/flutter/widgets/widgets-library.html)
and specialized widgets from the
[material library](https://docs.flutter.io/flutter/material/material-library.html).
Any app can use the widgets library but only material apps can use the
material library.

### Standard widgets

* [Container](#container)
: Adds padding, margins, borders, background color,
  or other decorations to a widget.
* [GridView](#gridview)
: Lays widgets out as a scrollable grid.
* [ListView](#listview)
: Lays widgets out as a scrollable list.
* [Stack](#stack)
: Overlaps a widget on top of another.

### Material widgets

* [Card](#card)
: Organizes related info into a box with rounded corners and a drop shadow.

* [ListItem](#listitem)
: Organizes up to 3 lines of text, and optional leading and training icons,
  into a row.

### Container

Many layouts make liberal use of Containers to separate widgets with padding,
or to add borders or margins. You can change the device's background by
placing the entire layout into a Container and changing its background color
or image.

<div class="row"> <div class="col-md-6" markdown="1">

#### Container summary:

* Add padding, margins, borders
* Change background color or image
* Contains a single child widget, but that child can be a Row, Column,
  or even the root of a widget tree

</div> <div class="col-md-6" markdown="1">

<img src="images/margin-padding-border.png" alt="a diagram showing that margins, borders, and padding, that surround content in a container">

</div> </div>

#### Container examples:

In addition to the example below,
many examples in this tutorial use Container. You can also find more
Container examples in the [Flutter
Gallery](https://github.com/flutter/flutter/tree/master/examples/flutter_gallery).

<div class="row"> <div class="col-md-6" markdown="1">

This layout consists of a column of two rows, each containing 2 images.
Each image uses a Container to add a rounded grey border and margins.
The Column, which contains the rows of images,
uses a Container to change the background color to a lighter grey.

**Dart code:** [main.dart](https://raw.githubusercontent.com/flutter/website/master/_includes/_code/container/main.dart), snippet below<br>
**Images:** [images](https://github.com/flutter/website/tree/master/_includes/_code/container/images)<br>
**Pubspec:** [pubspec.yaml](https://raw.githubusercontent.com/flutter/website/master/_includes/_code/container/pubspec.yaml)

</div> <div class="col-md-6" markdown="1">

<img src="images/container.png" alt="a screenshot showing 2 rows, each containing 2 images; the images have grey rounded borders, and the background is a lighter grey">

</div> </div>

<!-- _code/container/main.dart -->
<!-- skip -->
{% prettify dart %}
class _MyHomePageState extends State<MyHomePage> {
  @override
  Widget build(BuildContext context) {

    [[highlight]]var container = new Container[[/highlight]](
      decoration: new BoxDecoration(
        backgroundColor: Colors.black26,
      ),
      child: [[highlight]]new Column[[/highlight]](
        children: [
          new Row(
            children: [
              new Expanded(
                child: [[highlight]]new Container[[/highlight]](
                  decoration: new BoxDecoration(
                    border: new Border.all(width: 10.0, color: Colors.black38),
                    borderRadius:
                        const BorderRadius.all(const Radius.circular(8.0)),
                  ),
                  margin: const EdgeInsets.all(4.0),
                  child: new Image.asset('images/pic1.jpg'),
                ),
              ),
              new Expanded(
                child: [[highlight]]new Container[[/highlight]](
                  decoration: new BoxDecoration(
                    border: new Border.all(width: 10.0, color: Colors.black38),
                    borderRadius:
                        const BorderRadius.all(const Radius.circular(8.0)),
                  ),
                  margin: const EdgeInsets.all(4.0),
                  child: new Image.asset('images/pic2.jpg'),
                ),
              ),
            ],
          ),
          // ...
          // [[highlight]]See the definition for the second row on GitHub:[[/highlight]]
          // [[highlight]]https://raw.githubusercontent.com/flutter/website/master/_includes/_code/container/main.dart[[/highlight]]
        ],
      ),
    );
    //...
  }
}
{% endprettify %}

<hr>

### GridView

Use [GridView](https://docs.flutter.io/flutter/widgets/GridView-class.html)
to lay widgets out as a two-dimensional list. GridView provides two
pre-fabricated lists, or you can build your own custom grid.
When a GridView detects that its contents are too long to fit the render box,
it automatically scrolls.

#### GridView summary:

* Lays widgets out in a grid
* Detects when the column content exceeds the render box and automatically
  provides scrolling
* Build your own custom grid, or use one of the provided grids:
  * `GridView.count` allows you to specify the number of columns
  * `GridView.extent` allows you to specify the maximum pixel width of a tile
{% comment %}
* Use `MediaQuery.of(context).orientation` to create a grid that changes
  its layout depending on whether the device is in landscape or portrait mode.
{% endcomment %}

<aside class="alert alert-info" markdown="1">
**Note:** When displaying a two-dimensional list where it's important which
row and column a cell occupies (for example,
it's the entry in the "calorie" column for the "avocado" row), use
[Table](https://docs.flutter.io/flutter/widgets/Table-class.html) or
[DataTable](https://docs.flutter.io/flutter/material/DataTable-class.html).
</aside>

#### GridView examples:

<div class="row"> <div class="col-md-6" markdown="1">

<img src="images/gridview-extent.png" style="border:1px solid black" alt="a 3-column grid of photos">

Uses `GridView.extent` to create a grid with tiles a maximum 150 pixels wide.<br>
**Dart code:** [main.dart](https://raw.githubusercontent.com/flutter/website/master/_includes/_code/grid/main.dart), snippet below<br>
**Images:** [images](https://github.com/flutter/website/tree/master/_includes/_code/grid/images)<br>
**Pubspec:** [pubspec.yaml](https://raw.githubusercontent.com/flutter/website/master/_includes/_code/grid/pubspec.yaml)

</div> <div class="col-md-6" markdown="1">

<img src="images/gridview-count-flutter-gallery.png" style="border:1px solid black" alt="a 2 column grid with footers containing titles on a partially translucent background">

Uses `GridView.count` to create a grid that's 2 tiles wide in portrait mode,
and 3 tiles wide in landscape mode. The titles are created by setting the
`footer` property for each GridTile.<br>
**Dart code:** [grid_list_demo.dart](https://github.com/flutter/flutter/blob/master/examples/flutter_gallery/lib/demo/material/grid_list_demo.dart)
from the [Flutter
Gallery](https://github.com/flutter/flutter/tree/master/examples/flutter_gallery)

</div> </div>

<!-- _code/grid/main.dart -->
<!-- skip -->
{% prettify dart %}
// The images are saved with names pic1.jpg, pic2.jpg...pic30.jpg.
// The List.generate constructor allows an easy way to create
// a list when objects have a predictable naming pattern.
[[highlight]]List<Container> _buildGridTileList(int count)[[/highlight]] {

  [[highlight]]return new List<Container>.generate[[/highlight]](
      count,
      (int index) =>
          new Container(child: new Image.asset('images/pic${index+1}.jpg')));
}

[[highlight]]Widget buildGrid()[[/highlight]] {
  return new GridView.extent(
      maxCrossAxisExtent: 150.0,
      padding: const EdgeInsets.all(4.0),
      mainAxisSpacing: 4.0,
      crossAxisSpacing: 4.0,
      children: [[highlight]]_buildGridTileList(30));[[/highlight]]
}

class _MyHomePageState extends State<MyHomePage> {
  @override
  Widget build(BuildContext context) {
    return new Scaffold(
      appBar: new AppBar(
        title: new Text(config.title),
      ),
      body: new Center(
        child: [[highlight]]buildGrid()[[/highlight]],
      ),
    );
  }
}
{% endprettify %}


<hr>

### ListView

[ListView](https://docs.flutter.io/flutter/widgets/ListView-class.html),
a column-like widget, automatically provides scrolling when
its content is too long for its render box.

#### ListView summary:

* A specialized Column for organizing a list of boxes
* Can be laid out horizontally or vertically
* Detects when its content won't fit and provides scrolling
* Less configurable than Column, but easier to use and supports scrolling

#### ListView examples:

<div class="row"> <div class="col-md-6" markdown="1">

<img src="images/listview.png" style="border:1px solid black" alt="a ListView containing movie theaters and restaurants">

Uses ListView to display a list of businesses using ListItems.
A Divider separates the theaters from the restaurants.<br>
**Dart code:** [main.dart](https://raw.githubusercontent.com/flutter/website/master/_includes/_code/listview/main.dart), snippet below<br>
**Icons:** [Icons class](https://docs.flutter.io/flutter/material/Icons-class.html)<br>
**Pubspec:** [pubspec.yaml](https://raw.githubusercontent.com/flutter/website/master/_includes/_code/listview/pubspec.yaml)

</div> <div class="col-md-6" markdown="1">

<img src="images/listview-flutter-gallery.png" style="border:1px solid black" alt="a ListView containing shades of blue from the material design color palette">

Uses ListView to display the
[Colors](https://docs.flutter.io/flutter/material/Colors-class.html)
from the
[material design palette](https://material.io/guidelines/style/color.html)
for a particular color family.<br>
**Dart code:** [colors_demo.dart](https://github.com/flutter/flutter/blob/master/examples/flutter_gallery/lib/demo/colors_demo.dart)
from the [Flutter
Gallery](https://github.com/flutter/flutter/tree/master/examples/flutter_gallery)

</div> </div>

<!-- _code/listview/main.dart -->
<!-- skip -->
{% prettify dart %}
[[highlight]]List<Widget> list = <Widget>[[/highlight]][
  [[highlight]]new ListItem[[/highlight]](
    title: new Text('CineArts at the Empire',
        style: new TextStyle(fontWeight: FontWeight.w500, fontSize: 20.0)),
    subtitle: new Text('85 W Portal Ave'),
    leading: new Icon(
      Icons.theaters,
      color: Colors.blue[500],
    ),
  ),
  [[highlight]]new ListItem[[/highlight]](
    title: new Text('The Castro Theater',
        style: new TextStyle(fontWeight: FontWeight.w500, fontSize: 20.0)),
    subtitle: new Text('429 Castro St'),
    leading: new Icon(
      Icons.theaters,
      color: Colors.blue[500],
    ),
  ),
  // ...
  // [[highlight]]See the rest of the column defined on GitHub:[[/highlight]]
  // [[highlight]]https://raw.githubusercontent.com/flutter/website/master/_includes/_code/listview/main.dart[[/highlight]]
];

class _MyHomePageState extends State<MyHomePage> {
  @override
  Widget build(BuildContext context) {
    return new Scaffold(
      // ...
      body: new Center(
        child: [[highlight]]new ListView[[/highlight]](
          children: [[highlight]]list,[[/highlight]]
        ),
      ),
    );
  }
}
{% endprettify %}

<hr>

### Stack

Use [Stack](https://docs.flutter.io/flutter/widgets/Stack-class.html)
to arrange widgets on top of a base widget&mdash;often an image.
The widgets can completely or partially overlap the base widget.

#### Stack summary:

* Use for widgets that overlap another widget
* The first widget in the list of children is the base widget;
  subsequent children are overlaid on top of that base widget
* A Stack's content can't scroll
* You can choose to clip children that exceed the render box

#### Stack examples:

<div class="row"> <div class="col-md-6" markdown="1">

<img src="images/stack.png" style="border:1px solid black" alt="a circular avatar containing the label 'Mia B' in the lower right portion of the circle">

Uses Stack to overlay a Container (that displays its Text on a translucent
black background) on top of a Circle Avatar.
The Stack offsets the text using the `alignment` property and
FractionalOffsets.<br>
**Dart code:** [main.dart](https://raw.githubusercontent.com/flutter/website/master/_includes/_code/stack/main.dart), snippet below<br>
**Image:** [images](https://github.com/flutter/website/tree/master/_includes/_code/stack/images)<br>
**Pubspec:** [pubspec.yaml](https://raw.githubusercontent.com/flutter/website/master/_includes/_code/stack/pubspec.yaml)


</div> <div class="col-md-6" markdown="1">

<img src="images/stack-flutter-gallery.png" style="border:1px solid black" alt="an image with a grey gradient across the top; on top of the gradient is tools painted in white">

Uses Stack to overlay a gradient to the top of the image. The gradient
ensures that the toolbar's icons are distinct against the image.<br>
**Dart code:** [contacts_demo.dart](https://github.com/flutter/flutter/blob/master/examples/flutter_gallery/lib/demo/contacts_demo.dart)
from the [Flutter
Gallery](https://github.com/flutter/flutter/tree/master/examples/flutter_gallery)

</div> </div>

<!-- _code/stack/main.dart -->
<!-- skip -->
{% prettify dart %}
class _MyHomePageState extends State<MyHomePage> {
  @override
  Widget build(BuildContext context) {
    [[highlight]]var stack = new Stack([[/highlight]]
      [[highlight]]alignment: const FractionalOffset(0.8, 0.8),[[/highlight]]
      children: [
        [[highlight]]new CircleAvatar[[/highlight]](
          backgroundImage: new AssetImage('images/pic.jpg'),
          radius: 100.0,
        ),
        [[highlight]]new Container[[/highlight]](
          decoration: new BoxDecoration(
            backgroundColor: Colors.black45,
          ),
          child: [[highlight]]new Text[[/highlight]](
            'Mia B',
            style: new TextStyle(
              fontSize: 20.0,
              fontWeight: FontWeight.bold,
              color: Colors.white,
            ),
          ),
        ),
      ],
    );
    // ...
  }
}
{% endprettify %}

<hr>

### Card

A Card, from the material library, contains related nuggets of information
and can be composed from most any widget, but is often used with ListItem.
Card has a single child, but its child can be a column, row, list, grid,
or other widget that supports multiple children. By default, Card shrinks
to 0 by 0 pixels. You can use
[SizedBox](https://docs.flutter.io/flutter/widgets/SizedBox-class.html) to
constrain the size of a card.

In Flutter, a Card features slightly rounded corners
and a drop shadow, giving it a 3D effect.
Changing a Card's `elevation`
property allows you to control the drop shadow effect.
Setting the elevation to 24, for example, visually lifts the Card further
from the surface and causes the shadow to become more dispersed.
For a list of supported elevation values, see
[Elevation and
Shadows](https://material.io/guidelines/material-design/elevation-shadows.html)
in the [material guidelines](https://material.io/guidelines/).
Specifying an unsupported value disables the drop shadow entirely.

#### Card summary:

* Implements a [material design
  card](https://material.io/guidelines/components/cards.html)
* Used for presenting related nuggets of information
* Accepts a single child, but that child can be a Row, Column, or other
  widget that holds a list of children
* Displayed with rounded corners and a drop shadow
* A Card's content can't scroll
* From the material library

#### Card examples:

<div class="row"> <div class="col-md-6" markdown="1">

<img src="images/card.png" style="border:1px solid black" alt="a Card containing 3 ListItems">

A Card containing 3 ListItems and sized by wrapping it with a
SizedBox. A Divider separates the first and second ListItems.

**Dart code:** [main.dart](https://raw.githubusercontent.com/flutter/website/master/_includes/_code/card/main.dart), snippet below<br>
**Icons:** [Icons class](https://docs.flutter.io/flutter/material/Icons-class.html)<br>
**Pubspec:** [pubspec.yaml](https://raw.githubusercontent.com/flutter/website/master/_includes/_code/card/pubspec.yaml)

</div> <div class="col-md-6" markdown="1">

<img src="images/card-flutter-gallery.png" style="border:1px solid black" alt="a Card containing an image and text and buttons under the image">

A Card containing an image and text.<br>
**Dart code:** [cards_demo.dart](https://github.com/flutter/flutter/blob/master/examples/flutter_gallery/lib/demo/material/cards_demo.dart)
from the [Flutter
Gallery](https://github.com/flutter/flutter/tree/master/examples/flutter_gallery)

</div> </div>

<!-- _code/card/main.dart -->
<!-- skip -->
{% prettify dart %}
class _MyHomePageState extends State<MyHomePage> {
  @override
  Widget build(BuildContext context) {
    [[highlight]]var card = new SizedBox[[/highlight]](
      height: 210.0,
      [[highlight]]child: new Card[[/highlight]](
        child: new Column(
          children: [
            [[highlight]]new ListItem[[/highlight]](
              title: new Text('1625 Main Street',
                  style: new TextStyle(fontWeight: FontWeight.w500)),
              subtitle: new Text('My City, CA 99984'),
              leading: new Icon(
                Icons.restaurant_menu,
                color: Colors.blue[500],
              ),
            ),
            [[highlight]]new Divider()[[/highlight]],
            [[highlight]]new ListItem[[/highlight]](
              title: new Text('(408) 555-1212',
                  style: new TextStyle(fontWeight: FontWeight.w500)),
              leading: new Icon(
                Icons.contact_phone,
                color: Colors.blue[500],
              ),
            ),
            [[highlight]]new ListItem[[/highlight]](
              title: new Text('costa@example.com'),
              leading: new Icon(
                Icons.contact_mail,
                color: Colors.blue[500],
              ),
            ),
          ],
        ),
      ),
    );
  //...
}
{% endprettify %}

<hr>

### ListItem

Use
ListItem, a specialized row widget from the material library, for an easy
way to create a row containing up to 3 lines of text and optional leading
and trailing icons. ListItem is most commonly used in Card or ListView,
but can be used elsewhere.

#### ListItem summary:

* A specialized row that contains up to 3 lines of text and optional icons
* Less configurable than Row, but easier to use
* From the material library

#### ListItem examples:

<div class="row"> <div class="col-md-6" markdown="1">

<img src="images/card.png" style="border:1px solid black" alt="a Card containing 3 ListItems">

A Card containing 3 ListItems.<br>
**Dart code:** See [Card examples](#card-examples).

</div> <div class="col-md-6" markdown="1">

<img src="images/listitem-flutter-gallery.png" style="border:1px solid black" alt="3 ListItems, each containing a pull-down button">

Uses ListItem to list 3 drop down button types.<br>
**Dart code:** [buttons_demo.dart](https://github.com/flutter/flutter/blob/master/examples/flutter_gallery/lib/demo/material/buttons_demo.dart)
from the [Flutter
Gallery](https://github.com/flutter/flutter/tree/master/examples/flutter_gallery)

</div> </div>

<hr>

<a name="building"></a>
## Building a layout

The previous sections describe Flutter's approach to creating a layout.
How do you use this knowledge to build an entire layout?
Let's walk through the layout for the following screenshot:

<img src="images/lakes.png" style="border:1px solid black" alt="A screenshot of the lakes app that we will build in this section">

### Contents

* [Step 1: Diagram the layout](#step-1)
* [Step 2: Implement the title row](#step-2)
* [Step 3: Implement the button row](#step-3)
* [Step 4: Implement the text section](#step-4)
* [Step 5: Implement the image section](#step-5)
* [Step 6: Put it together](#step-6)

<a name="step-1"></a>
### Step 1: Diagram the layout

The first step is to break the layout down to its basic elements:

* Identify the rows and columns.
* Does the layout include a grid?
* Overlapping elements?
* Tabs?
* Notice areas that require alignment, padding, or borders.

First, identify the larger elements. In this example, four elements are
arranged as a Column: an image, two rows, and a block of text.

<img src="images/lakes-diagram.png" alt="diagramming the rows in the lakes screenshot">

Next, diagram each row. The first row, which we call the "Title
Section", has 3 children: 1) a column of text, 2) a star icon,
and 3) a number. Its first child, the column, contains 2 lines of text.
That first column takes a lot of space, so it must be wrapped in an
Expanded widget.

<img src="images/title-section-diagram.png" alt="diagramming the widgets in the title section">

The second row, which we call the "Button Section", also has
3 children&mdash;each child is a column that contains an icon and text.

<img src="images/button-section-diagram.png" alt="diagramming the widgets in the button section">

Once the layout has been diagrammed, it's easiest to take a bottom up
approach to implementing it. In order to minimize the visual
confusion of deeply nested layout code, place some of the implementation
in variables and functions.

<a name="step-2"></a>
### Step 2: Implement the title row

First, let's build the left column in the title section. The Column is
placed inside an Expanded widget to stretch it to use all remaining free
space in the row. Setting the `crossAxisAlignment` property to
`CrossAxisAlignment.start` positions the column to the beginning of the row.

The first row of text is placed inside a Container to add padding and the
text is styled to use a bold font weight. The second child in the Column,
also text, is painted in grey.

The last two items in the title row are a star icon, painted red,
and the text, '41'. The entire row is placed in a Container and padded
along each edge with 32 pixels.


<!-- _code/lakes/main.dart -->
<!-- skip -->
{% prettify dart %}
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    [[highlight]]Widget titleSection = new Container[[/highlight]](
      padding: const EdgeInsets.all(32.0),
      child: [[highlight]]new Row[[/highlight]](
        children: [
          [[highlight]]new Expanded[[/highlight]](
            child: [[highlight]]new Column[[/highlight]](
              crossAxisAlignment: CrossAxisAlignment.start,
              children: [
                [[highlight]]new Container[[/highlight]](
                  padding: const EdgeInsets.only(bottom: 8.0),
                  child: [[highlight]]new Text[[/highlight]](
                    'Oeschinen Lake Campground',
                    style: new TextStyle(
                      fontWeight: FontWeight.bold,
                    ),
                  ),
                ),
                [[highlight]]new Text[[/highlight]](
                  'Kandersteg, Switzerland',
                  style: new TextStyle(
                    color: Colors.grey[500],
                  ),
                ),
              ],
            ),
          ),
          [[highlight]]new Icon[[/highlight]](
            Icons.star,
            color: Colors.red[500],
          ),
          [[highlight]]new Text[[/highlight]]('41'),
        ],
      ),
    );
  //...
}
{% endprettify %}

<a name="step-3"></a>
### Step 3: Implement the button row

The button section contains 3 columns that use the same layout&mdash;an
icon over a row of text. The columns in this row are evenly spaced,
and the text and icons are painted with the primary color,
which was set to blue in the app's `build` method:

<!-- _code/lakes/main.dart -->
<!-- skip -->
{% prettify dart %}
class MyApp extends StatelessWidget {
  @override
  [[highlight]]Widget build(BuildContext context)[[/highlight]] {
    //...

    return new MaterialApp(
      title: 'Flutter Demo',
      [[highlight]]theme: new ThemeData[[/highlight]](
        [[highlight]]primarySwatch: Colors.blue,[[/highlight]]
      ),

    //...
}
{% endprettify %}

Since the code for building each row would be almost identical,
it's most efficient to create a nested function, such as `buildButtonColumn`,
that takes an Icon and Text, and returns a column with its widgets
painted in the primary color.

<!-- _code/lakes/main.dart -->
<!-- skip -->
{% prettify dart %}
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    //...

    [[highlight]]Column buildButtonColumn(IconData icon, String label)[[/highlight]] {
      List<Widget> widgets = new List(2);
      [[highlight]]Color color = Theme.of(context).primaryColor;[[/highlight]]

      widgets[0] = new Icon(icon, color: color);
      widgets[1] = new Container(
        margin: const EdgeInsets.only(top: 8.0),
        child: new Text(
          label,
          style: new TextStyle(
            fontSize: 12.0,
            fontWeight: FontWeight.w400,
            color: color,
          ),
        ),
      );

      [[highlight]]return new Column[[/highlight]](
        mainAxisSize: MainAxisSize.min,
        mainAxisAlignment: MainAxisAlignment.center,
        children: widgets,
      );
    }
  //...
}
{% endprettify %}

In the build function, the icon is added directly to the column. The
text is placed into a Container to add padding to the top of the text,
separating it from the icon.

The row containing these columns is built by calling the function and
passing the [icon](https://docs.flutter.io/flutter/material/Icons-class.html)
and text specific to that column. The columns are aligned along the
main axis using `MainAxisAlignment.spaceEvenly` to arrange the free
space evenly before, between, and after each column.

<!-- _code/lakes/main.dart -->
<!-- skip -->
{% prettify dart %}
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    //...

    [[highlight]]Widget buttonSection = new Container([[/highlight]]
      child: new Row(
        mainAxisAlignment: MainAxisAlignment.spaceEvenly,
        children: [
          [[highlight]]buildButtonColumn(Icons.call, 'CALL'),[[/highlight]]
          [[highlight]]buildButtonColumn(Icons.near_me, 'ROUTE'),[[/highlight]]
          [[highlight]]buildButtonColumn(Icons.share, 'SHARE'),[[/highlight]]
        ],
      ),
    );
  //...
}
{% endprettify %}

<a name="step-4"></a>
### Step 4: Implement the text section

The text section is fairly wordy, so it's also defined as a variable.
The text is placed in a Container in order to add 32 pixels of padding on
each edge. The `softwrap` property indicates whether the text should break
on soft line breaks, such as periods or commas.

<!-- _code/lakes/main.dart -->
<!-- skip -->
{% prettify dart %}
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    //...

    [[highlight]]Widget textSection = new Container[[/highlight]](
      padding: const EdgeInsets.all(32.0),
      child: new Text(
        '''
Lake Oeschinen lies at the foot of the Blüemlisalp in the Bernese Alps. Situated 1,578 meters above sea level, it is one of the larger Alpine Lakes. A gondola ride from Kandersteg, followed by a half-hour walk through pastures and pine forest, leads you to the lake, which warms to 20 degrees Celsius in the summer. Activities enjoyed here include rowing, and riding the summer toboggan run.
        ''',
        softWrap: true,
      ),
    );
  //...
}
{% endprettify %}

<a name="step-5"></a>
### Step 5: Implement the image section

Three of the four Column elements are now complete, leaving only the image.
This image is [available
online](https://images.unsplash.com/photo-1471115853179-bb1d604434e0?dpr=1&amp;auto=format&amp;fit=crop&amp;w=767&amp;h=583&amp;q=80&amp;cs=tinysrgb&amp;crop=)
under the Creative Commons license, however it's quite large and slow to fetch.
As with the [Pavlova example](#adding-images), we've included the image
in the project, which you can see in the [pubspec
file.](https://raw.githubusercontent.com/flutter/website/master/_includes/_code/lakes/pubspec.yaml)

<!-- _code/lakes/main.dart -->
<!-- skip -->
{% prettify dart %}
[[highlight]]new Image.asset[[/highlight]](
  'images/lake.jpg',
  width: 600.0,
  height: 240.0,
  [[highlight]]fit: ImageFit.cover,[[/highlight]]
),
{% endprettify %}

`ImageFit.cover` tells the framework that the image should be as small as
possible but cover its entire render box.

<a name="step-6"></a>
### Step 6: Put it together

The final step assembles the pieces together. At this point, you can tweak
the layout as needed.  Here is the final Column:

<!-- skip -->
<!-- _code/lakes/main.dart -->
{% prettify dart %}
//...
body: new Column(
  children: [
    new Image.asset(
      'images/lake.jpg',
      width: 600.0,
      height: 240.0,
      fit: ImageFit.cover,
    ),
    titleSection,
    buttonSection,
    textSection,
  ],
),
//...
{% endprettify %}

**Dart code:** [main.dart](https://raw.githubusercontent.com/flutter/website/master/_includes/_code/lakes/main.dart)<br>
**Image:** [images](https://github.com/flutter/website/tree/master/_includes/_code/lakes/images)<br>
**Pubspec:** [pubspec.yaml](https://raw.githubusercontent.com/flutter/website/master/_includes/_code/lakes/pubspec.yaml)

<hr>

<a name="resources"></a>
## Resources

The following resources may help when writing layout code.

* [Widget Overview](/widgets)<br>
  Describes many of the widgets available in Flutter.
* [HTML/CSS Analogs in Flutter](/web-analogs)<br>
  For those familiar with web programming, this page maps HTML/CSS functionality
  to Flutter features.
* [Flutter
  Gallery](https://github.com/flutter/flutter/tree/master/examples/flutter_gallery)<br>
  Demo app showcasing many material design widgets and other Flutter features.
* [Flutter API documentation](https://docs.flutter.io/)<br>
  Reference documentation for all of the Flutter libraries.
* [Dealing with Box Constraints in Flutter](/layout)<br>
  Discusses how widgets are constrained by their render boxes.
* [Adding Assets and Images in Flutter](/assets-and-images)<br>
  Explains how to add images and other assets to your app's package.
* [Zero to One with
  Flutter](https://medium.com/@mravn/zero-to-one-with-flutter-43b13fd7b354#.z86tsq4ld)<br>
  One person's experience writing his first Flutter app.