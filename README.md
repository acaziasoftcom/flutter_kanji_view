# flutter_kanji_view [![Pub](https://img.shields.io/badge/pub-1.0.0-blue)](https://pub.dartlang.org/packages/drawing_animation)

A Flutter library to draw kanji character with animation.

# Introduction

This library is based on biocarl's [drawing_animation](https://github.com/biocarl/drawing_animation), but with some modifications to display kanji SVG from [KanjiVG](https://kanjivg.tagaini.net/).

Moreover, it exposes a third constructor: `.str`, to display the kanji SVG data downloaded as a string (very handy if you store those SVG on your server).

## 1. Add dependency into your `ppubspec.yaml`

```yml
dependencies:
  flutter_kanji_view: ^1.0.0
```

## 2. Provide the assets (optional, not recommended since it will increase the app bundle a lot. There are over 6000 SVG files in the package):

- Download KanjiVG pack from [here](https://github.com/KanjiVG/kanjivg/releases) , get the zip file.

- Add the path into `yml`:

```yml
assets:
  - assets/0ff10.svg
```

## 3. Use the widget: Same with drawing_animation, you can use it in two ways:

- Without controller: The animation will run once:

```dart
KanjiViewer.svg(
  "assets/0ff10.svg",
  run: this.run,
  duration: new Duration(seconds: 3),
)
```

- With controller: You will have more control over the widget.

```dart
class Playground extends StatefulWidget {
  @override
  _PlaygroundState createState() {
    return _PlaygroundState();
  }
}

class _PlaygroundState extends State<Playground>
    with SingleTickerProviderStateMixin {
  var run = true;
  AnimationController _controller;

  @override
  void initState() {
    super.initState();
    _controller =
        AnimationController(vsync: this, duration: Duration(seconds: 3));
  }

  @override
  Widget build(BuildContext context) {
    var char = getKanjiUnicode('雨');

    return Scaffold(
      body: Column(
        crossAxisAlignment: CrossAxisAlignment.center,
        mainAxisAlignment: MainAxisAlignment.center,
        children: <Widget>[
          Container(
            width: 400,
            height: 400,
            alignment: Alignment.center,
            padding: const EdgeInsets.all(6),
            color: Colors.black12,
            child: KanjiViewer.svg("assets/vectors/" + char + ".svg",
                scaleToViewport: true,
                duration: new Duration(seconds: 6)),
          ),
          RaisedButton(
              child: Text('Redraw'),
              onPressed: () {
                _controller.reset();
                _controller.forward();
              })
        ],
      ),
    );
  }
}
```

## 4. Advanced usage:

There are 3 mores for drawing SVG, mostly from the source they were from.

*** From SVG asset ***: This might be the most common use.

```dart
KanjiViewer.svg("<SVG Path>")
```

*** From SVG string ***

This can be convenient when you store your SVG data somewhere on the remote server, and retrieve via API:

```dart
KanjiViewer.str("<SVG string data>")
```

*** From Path data ***

```dart
// Of the List<Path>
KanjiViewer.paths(pathList)
```

You can use some methods in SVGHelper class (such as `getCoordinatesGroup` and `buildPath`) for Path parsing. I'm not going to document it any time soon because of lacking time though.

TODO:

Since the core library borrows a lot from drawing_animation, their TODOs are also considered this lib's TODOs. This lib, however, add some more priority tasks that I can foresee:

- Expose `Paint` object so users can customize the stroke color / width.
- Expose a `colorSeed` array for distinct strokes. It should be a `List` of `Color` object.
