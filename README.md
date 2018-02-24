# A Guide to Externalizing Übersicht Widget Styles

[Übersicht](http://tracesof.net/uebersicht/) is great, but its current design implements logic and themes all in the same file. [Felix](https://github.com/felixhageloh), creator of [Übersicht](http://tracesof.net/uebersicht/), encourages users to [submit pull requests for widgets over creating new ones](https://github.com/felixhageloh/uebersicht-widgets#note):


> Since tweaking the look of a widget is fun and easy, the widget gallery is probably the most useful if it serves as starting point instead of being a comprehensive list of all possible widgets. For this reason we try to keep the widget gallery as focused as possible. This means, instead of adding new widgets that are very similar to an existing widget, we try to encourage people to make a pull requests with their improvements (these could be technical improvements or style/design improvements).

## CSS vs. Stylus

Per the [Writing Widgets](https://github.com/felixhageloh/uebersicht#writing-widgets) guide, [Übersicht](http://tracesof.net/uebersicht/) uses Stylus to process the ```style``` string. However, if you choose to externalize your styles to allow for theming or easy customization, you're going to have to stick with CSS because the style will be pulled in via [@import](https://www.w3.org/TR/css3-cascade/#at-ruledef-import) and bypass the stylus preprocessor. You can still create .styl files, but you'll need to process them into .css files on your own.

## Widget Structure


