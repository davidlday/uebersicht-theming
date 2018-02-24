# A Guide to Externalizing Übersicht Widget Styles

[Übersicht](http://tracesof.net/uebersicht/) is great, but its current design implements logic and themes all in the same file. [Felix](https://github.com/felixhageloh), creator of [Übersicht](http://tracesof.net/uebersicht/), encourages users to [submit pull requests for widgets over creating new ones](https://github.com/felixhageloh/uebersicht-widgets#note):

> Since tweaking the look of a widget is fun and easy, the widget gallery is probably the most useful if it serves as starting point instead of being a comprehensive list of all possible widgets. For this reason we try to keep the widget gallery as focused as possible. This means, instead of adding new widgets that are very similar to an existing widget, we try to encourage people to make a pull requests with their improvements (these could be technical improvements or style/design improvements).

This is how I managed to put all styling in an external file. This should work for any widget, but it also imposes the following constratints:

1. The widget should have only one script file (.coffee, .js).
2. The widget can only use [CSS](https://www.w3.org/Style/CSS/) for styling.
    * You can still use [Stylus](http://stylus-lang.com/), but you'll have to compile styles to CSS before publishing.
    * Actually, you can use any CSS generator, but have to manually compile them to CSS before publishing.

## How To Create a Widget

Gory details on why come later. Here's how to make this happen.

### Package Structure

The sample assumes the widget is called ```<widget-name>.widget```, and that your repo follows this structure:

```
<reponame>/
    |-README.md
    |-<widget-name>.widget/
        |-styles/
        |   |-default.css
        |-<widget-name>.coffee
```

See my [Reminder Widget](https://github.com/davidlday/RemindersWidget) for a working example of this layout.

1. Create your repo can call it whatever you want.
2. Create a ```<widget-name>.widget/``` subdirectory for the widget files. This isn't a must, but I found it easier to zip the whole thing up at the end using this structure. See [widget-name.widget.zip](https://github.com/felixhageloh/uebersicht-widgets#widget-namewidgetzip) for details on why.
3. Create ```<widget-name.widget>/styles``` subdirectory. Your CSS themes will go here.
4. Create your widget script as ```<widget-name>.widget/<widget-name>.coffee``` or ```<widget-name>.widget/<widget-name>.js```. The rest of this document assumes you're using [CoffeeScript](http://coffeescript.org/).

### Create the Default Style

Create a file called ```<widget-name>.widget/styles/default.css```. This will be the widget's default theme.

### Set the Default Style

The [Übersicht Style Documentation](https://github.com/felixhageloh/uebersicht#style) shows how to implement a style using a string directly in the widget's script (```<widget-name>.coffee``` in our example). However, this means anytime someone wants to alter the style, they have to do so in the main script. If you update and make a new release, you're going to obliterate their custom style.

Instead, we'll leverage CSS's [@import](https://www.w3.org/TR/css3-cascade/#at-ruledef-import) feature. In your widget's script file, ensure the Übersicht settings **come last**. One of them will implement the import statement, as such:

```coffee
#############################
# CSS styling for the widget
style: """
    @import url(reminders.widget/styles/""" + settings.styleFilename + """);
"""
```

Then, at the top, add widget specific settings, one of which will be the style filename:

```coffee
#############################
# Widget Settings (at the top of the file)
settings =
# File containing widget style:
# - default.css (original)
# - sidebar.css (sidebar)
  styleFilename: 'default.css'
# Set the refresh frequency (milliseconds).
  refreshFrequency: '1m'
#############################
```

I also implelement the refresh ettings here to keep all the configurable stuff in one place. Here's how it looks together:

```coffee
#############################
# Widget Settings (at the top of the file)
settings =
# File containing widget style:
# - default.css (original)
# - sidebar.css (sidebar)
  styleFilename: 'default.css'
# Set the refresh frequency (milliseconds).
  refreshFrequency: '1m'
#############################

#############################
# Do not alter below here.
#############################

#############################
# CSS styling for the widget
style: """
    @import url(reminders.widget/styles/""" + settings.styleFilename + """);
"""
```

Now you can provide multiple styles, or allow users to implement their own, with minimal changes to the main script.

## Details

Here are some explanations as to why this works, and why the widget has to be implemented in a specific way.

### CSS vs. Stylus

Per the [Writing Widgets](https://github.com/felixhageloh/uebersicht#writing-widgets) guide, [Übersicht](http://tracesof.net/uebersicht/) uses Stylus to process the ```style``` string. However, if you choose to externalize your styles to allow for theming or easy customization, you're going to have to stick with CSS because the style will be pulled in via [@import](https://www.w3.org/TR/css3-cascade/#at-ruledef-import) and bypass the stylus preprocessor. You can still create .styl files, but you'll need to process them into .css files on your own.

### Widget Structure


