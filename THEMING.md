# Angular Material Themes

## Introduction

Material Design is a visual design language that supports innovative user experiences (UX) and user interface (UI) elements across multiple devices and multiple screen sizes. Similar to the Layout features' use of keylines and alignments to convey meaning, Material Design **Themes** are used to convey meaning through the use of color, tones, and contrasts.

Theme [**color palettes**](http://www.google.com/design/spec/style/color.html#color-ui-color-palette), alphas, and shadows deliver a consistent tone to your application and provide a unified feel for all Angular Material UI components and elements.

![screen shot 2014-10-28 at 3 04 49 pm](https://cloud.githubusercontent.com/assets/210413/4816236/bf7783dc-5edd-11e4-88ef-1f8b6e87e1d7.png)

Themes' support is an integral feature within each Angular Material component. And Theme construction is also an essential aspect of the SDK build processes. In fact, Themes are used within our [Online Documentation](http://material.angularjs.org).

> Currently, Themes only supports color and font-family styling. Additional theme options such as additional font stylings , padding, borders, and SVG support are planned for future releases.

- - -


This document is intended to provide clarity on three (3) important [Angular Material] Theme topics:

1. [Using *Themes*](#ut)
2. [Building Custom Themes](#ct)
3. [Exploring Themes](#tuth)


## 1) <a name="ut"></a> Using Themes

Only two (2) simple steps are required to use Themes within your own custom application:

**Step 1**:

Load the `default-theme.css` and one (1) or more custom theme CSS files in your main HTML page.
<br/><br/>Compiled themes can be found in both the `/dist/themes` directory and the [bower-material themes](https://github.com/angular/bower-material/themes) folder. 
> NOTE: The directory `/dist/themes` is updated after running `gulp build`.


Developers must add any theme's stylesheet that will be used in their application. For example, if the application also uses theme's `green` and `indigo`, you must install all those `*.css` files:<br/><br/>
```html
<!doctype html>
<html lang="en">
    <head>
        <title>My App</title>
        <script src="//ajax.googleapis.com/ajax/libs/angularjs/1.3.0/angular.js"></script>
        <script src="//ajax.googleapis.com/ajax/libs/angularjs/1.3.0/angular-animate.min.js"></script>
        <script src="//ajax.googleapis.com/ajax/libs/angularjs/1.3.0/angular-route.min.js"></script>
        <script src="//ajax.googleapis.com/ajax/libs/angularjs/1.3.0/angular-aria.min.js"></script>
    
        <script src="myApp.js"></script>
        
        <link rel="stylesheet" href="/themes/default-theme.css">
        <link rel="stylesheet" href="/themes/indigo-theme.css">
        <link rel="stylesheet" href="/themes/green-theme.css">
    </head>
    
    <body>...</body>
</html>
```

<br/>**Step 2:**

If no theme is specified, your application will be in the **default** Angular Material theme... no extra configuration is required.
<br/><br/>To **override** an area of your application with a custom theme, simply use the `md-theme` directive on specific view areas; which may be a container element or a specific element within your **ng-app**. <br/><br/>Angular Material component will always use the theme specified by its own **md-theme** setting or use the setting from the closest, theme-specified, parent container node. If a theme is **NOT** specified for a component or any of its parent, the `default` theme will again be used.
<br/><br/>
```html
<!doctype html>
<html lang="en">
	<head>... </head>
	
	<!-- Uses default theme for the `body` container and children -->
    <body ng-app="myApp" ng-controller="myAppController" layout="horizontal">
        
		<!-- Overrides with indigo theme -->
        <md-toolbar md-theme="indigo" class="md-medium-tall app-toolbar">
            
		<!-- Overrides div and children with green theme -->
        <div md-theme="green">
            <md-text-float ng-model="user.name" label="Name" />
        </div>
    
    </body>
</html>
```

<br/><br/>
The HTML snippet below shows how specific components can override the inherited theme:
```html
  <h4>Themes</h4>
  <md-progress-linear md-theme="red" mode="determinate" ng-value="determinateValue" ></md-progress-linear>
  <md-progress-linear md-theme="deep-orange" mode="buffer" value="{{determinateValue}}" secondaryValue="{{determinateValue2}}"></md-progress-linear>
  <md-progress-linear md-theme="yellow" mode="{{mode}}" value="{{determinateValue}}"></md-progress-linear>
  <md-progress-linear md-theme="green" mode="determinate" ng-value="determinateValue" ></md-progress-linear>
  <md-progress-linear md-theme="blue" mode="buffer" value="{{determinateValue}}" secondaryValue="{{determinateValue2}}"></md-progress-linear>
  <md-progress-linear md-theme="indigo" mode="{{mode}}" value="{{determinateValue}}"></md-progress-linear>
```

![screen shot 2014-10-29 at 7 02 58 am](https://cloud.githubusercontent.com/assets/210413/4825301/a45d735a-5f63-11e4-8597-60386f35fc68.png)

The Angular Material Online documentation also provides additional examples of theme-**overrides**: see [Buttons](http://localhost:8200/index.html#/demo/material.components.button), [Progress Bars](http://localhost:8200/index.html#/demo/material.components.progressLinear), and others...




## 2) <a name="ct"></a> Building Custom Themes

Normally you will use the *standard* themes provided by AngularJS Material and the **md-theme** directive. Sometimes, however, you need your own brand colors, etc, in which case you should build and compile your own theme.

To build your own theme, you must write a `scss` file that overrides only the specific variables that you want to customize. These will be used to override settings in the imported `default-theme` settings and generate your custom theme file.

For example, let's prepare a custome them file `themes/my-theme.scss`:

```scss
$theme-name: 'my-custom-theme';
$primary-color-palette: $color-indigo;
$background-color-base: #333;
$checkbox-color-palette: $color-pink;
```

Then run the shell command

```sh
gulp build-theme -t my-theme
```

to generate the theme file `dist/themes/my-theme.css`; which must be manually added to their project(s). 

The application can now be easily themed using the `md-theme` directive:

```html
<body ng-app="myApp" md-theme="custom">

</body>
```


## 3) <a name="tuth"></a>Exploring the Theme Implementation

This section will provide a high-level, under-the-hood exploration of Themes; to determine how its features are supported in both the CSS and the JavaScript implementations.

#### CSS Features
<br/>
**Color Palettes**

Angular Material has implemented the visual [color palettes](http://www.google.com/design/spec/style/color.html#color-ui-color-palette) as `scss maps` in [color-palette.scss](https://github.com/angular/material/blob/master/src/core/style/color-palette.scss). Using a new feature within SASS called `map-get()`, these colors can be easily accessed by using standard `scss` syntax.

```scss
body {
	background-color: map-get($color-red, '500');
}
```

**Global Style Variables**

Most Angular Material components' colors are derived from variables found in [variables.scss](https://github.com/angular/material/blob/master/src/core/style/variables.scss). By overriding these variables you can generate your own themes. Here are some guidelines regarding the variables found in `variables.scss`:

| Variable | Purpose |
|--------|--------|
| $theme-name | name of the theme which is required when using the `md-theme` directive to specify a theme choice |
| $dark-theme| boolean indicating whether or not the theme is a **dark** theme. Used for things like adding drop shadows to text, higher contrast colors, etc. |
| $foreground-color-palette | the color palette used for foreground colors (such as text, hints, and dividers) |
| $foreground-base-color | the from which foreground colors are derived |
| $foreground-primary-color | color used for text |
| $foreground-secondary-color | color used for secondary text and icons |
| $foreground-tertiary-color |  color used for hints |
| $foreground-quaternary-color | color used for dividers |
| $background-color-palette | the color palette used to determine the background color |
| $background-color-base | the background color |
| $primary-color-palette | the primary color used for things like buttons, spinners, etc. |
| $warn-color-palette | the color palette used for warnings within the app |
| $primary-color-palette-contrast-color | the color used for text with a `primary-color` as a background |

<br/>
**Component Styles**

Each component within Angular Material has custom styles specified in its `*-theme.scss` file; <br/>e.g. 
*  `textfield-theme.scss`,
*  `tabs-theme.scss`,
*  `slider-theme.scss`,
*  etc.

These component-specific styles are aggregated and compiled with the *variables.scss* and *color-palettes.scss* to generate the `/dist/themes/default-theme.scss` file. And for each custom theme override file listed in `/themse/*.scss`, these same aggregated styles, palettes, and variable overrides are also used to generate custom theme files deployed to `/dist/themes`

With this build process, it is now very easy to build custom Theme files [as described in [Section 2](#ct) above].

#### JavaScript Features
<br/>
Angular Material uses both a `md-theme` **directive** and a `$mdTheming` **service** to provide JS support for the Theming features.

`$mdTheming` is an internal service that registers a component as *themable*. The service is actually an overloaded function that can take arguments as `$mdTheming(scope, el)` or `$mdTheming(el)`. This allows it to be used as a **post-link** function in simple directives that don't otherwise need any other javascript logic. Eg.

```js
app.directive('simpleDirective', function($mdTheming) {
  return {
    template: '<h1> Hello world</h1>',
    link: $mdTheming
  }
});
```

`$mdTheming` will apply a class to the element for the appropriate theme. The class will be derived from the theme name, in the following format `md-custom-theme` where $theme-name == `custom`. It will use the closest `md-theme` to determine the theme to use. If no theme is found, it will use the `default` theme.

For example:

```html
<div md-theme="green">
  <simple-directive />
</div
```

`simple-directive` will have class `md-green-theme`.

Or

```html
<div md-theme="green">
  <simple-directive md-theme="yellow" />
</div
```

`simple-directive` will have class `md-yellow-theme`.

