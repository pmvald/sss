
SSS: A css framework to make life easy
=====================================

Overview
------------------
This a design system (css) I used in my previous projects and here I try to formalize it ... It is supposed to be simple and very practical, without creating unnecessary limitations or opinionated designs. 

This is a work in progress (I work on it on and off here). Ultimately it will emerge as a css/sass framework. Your contribution is welcome! 

This doc contains a summary of the design system itself, and the present repository contains one example of such a design system. Note that this example needs some cleanup; for one thing, it is still using some features from Bootstrap 5 (note the cloned bootstrap folder in the source code) but gradually it has to be replaced by our own scss. 

Now a summary of what this css architecture is about:

The framework
-----------------------------
This framework uses BEM convention for calss naming, also inspired by SMACSS we divide our css into three files (or layers):

- **Layer 0:** layouts.scss: universal classes to build basic layouts. The main goal of these classes is to allow you to "place" elements in the right place (such as arranging elements as a horizontal menu) This file is 100% un-opinionated, and is to be used in each project and each device (desktop/tablet/mobile) WITHOUT any modification (at the moment most part of this layer comes from bootstrap 5; i.e., those familiar classes `row`, `col-sm-6`, `d-flex`, etc.)
  

- **Layer 1:** styles.css: This are the opinionated styles (i.e., colors, font-sizes, etc) for the common components used in every project, like menus, dropdowns, etc. The theme and its variables are defined in this file. This layer either extends the class in layer 0 or creates new classes. These new classes should start with *s-* to specify they are opinionated *Style classes* (e.g., `s-btn` is the style class for a button). This file is to be used in all projects WITH modifications (to reflect the theme)
  

- **Layer 2:** project.scss: the project specific styles. These are components that are only used in the current project. Example: property search card in a real-estate project. These class names start with double s: ss- (meaning *Specific Style*) This file is NOT to be used in other projects.

Below comes a more detailed introduction to these three layers with examples.


### Layer 0: Bare-bone layout

These classes are defined in layouts.scss and their names do not start with s- or ss-. At the moment these classes are mostly taken from the layout/grid classes of Bootstrap 5.0 source code, which is to be found in the folder `boostrap5`. You may notice that certain css files of bootstrap5 folder are included in the layouts.scss such as grid, container, flex utilities, etc. 

The classes in layer 0 deal solely with positioning and spacing. They allow us to put elements nicely beside each other or create columns, padding, margins, etc. They have nothing to do with color, font-family, etc, they do not change the "appearance" of the html elements in anyway, all elements will have their original browser-native look when using layer 0 classes. Here are the styles that layer 0 is concern about:

- Grid layouts for horizontal positioning (example classes: row, col-3, col-sm-2, d-flex,)
  

- helper classes for margin, padding, spacers (example classes: mb-3, pt-2, m-2, etc)- responsive classes to show/hide stuffs based on breakpoints (example classes: d-md-none, d-sm-block, etc.)
  

- classes for very basic and common structures that browsers do not support natively but needed in any website/app, e.g., menu (defined in bootstrap5/_nav.scss), dropdown, modal, etc.
  

- unifying basic styles of elements cross browsers (-> styles in bootstrap5/_roboot.scss)

> **Note:** We have modified/simplified some of bootstrap5 files that are imported into layouts.scss. So the folder bootstrap5 is in fact a fork of bootstrap5 project and not exactly a clone. We use bootstrap 5 as starting point for our layer 0 styles and continue to modify this source to scrape its classes that are fit for a layer 0 stylesheet. Note that bootstrap contains a lot of opinionated styles that do not belong to layer 0 and should be done away. Eventually we will get rid of bootstrap and stand on our feet.

Independence from styles: The styles in layer 0 must be totally independent from upper layers. For example, your grid (which you made with layer 0 classes) should look nice and aligned no matter what font-size is used for the elements. One of the most useful boostrap5 classes (included in layouts.scss) for this purpose is `d-flex`, specially when combined with `align-items-center` which allow us to put elements side by side and align them by center no matter what the height or font ize of each element is. This is how layout 0 should operate. 

If your style breaks when elements become big or small then it is not independent from the style and must be redesigned. Layer 0 styles must be standalone and style-independent. They are not really styles to change the look and appearance, but more like *extensions of the browser* which enable us to place and position elements more easily (browser only allows us to stack elements on top of each other, no native support for grid, menu, etc. without using css classes)


### Layer 1: Opinionated styles

These are opinionated styles containing font-size, color, background-color, etc. They are defined in styles.scss and the class names begin with "s-". These styles are meant to be "general purpose" and contain "common structures used in many project". Here are the areas that these styles are concern about:

- Defining theme variables ($primary-color, $secondary-color, $body-color, $body-font, etc.)
  

- Applying styles to individual elements (like converting the color of `<a>` element to the primary color, defining class s-btn for buttons)
  

- Adding additional styles (color, font-size, etc) to the styles defined in layer 0 (like styling the menu, dropdown, modal, etc.)
  

- Styling the common components used in many websites, such as main header, landing page banner, footer, login forms, alert boxes

### Layer 2: Project specific styles

They project specific styles. They are defined in project.scss and the class names begin with the prefix `ss-`. They are to define specific components for the project, like the product summary card, search bar (with special controls), etc. These styles are mainly "blocks" (in the BEM model). Each such block should be defined in a separate scss file in the "components" folder and then imported into project.scss.  

### Portability

When you move to your next project:

1. Layer 0 is transferred exactly as it is, no change (universal layout concepts are the same cross projects)

2. Layer 1 is transferred with modifications, mainly to its theme variables; but in a more customized project you may need to do surgery on other components as well.

3. Later 2 is NOT transferred per se. It is a collection of project specific components and normally not relevant to another project.


### bundle.css

This file imports all three layers into one css file. Thus its source code is:

```scss
@import "_layouts.scss";
@import "_styles.scss";
@import "_project.scss";
```

This is the file that we will include in our html files.


Design guidelines
------------------------
- When designing a component, first design its layout with layer 0 classes only and make sure everything is placed and aligned correctly. And then add layer 1 & 2 for further styling


- Minimize using layer 2 styles. Try to build thing by later 0 and 1 as much as possible. Use layer 2 only if the component is very special.


- The theme is defined in styles.scss variables. Do not defined theme variables in layer 2 (project.scss)


- Keep style.scss styles to "common" elements (menus, headers, footers, general forms, etc) Do not define project specific components there (like product card, checkout list, etc.)


Example:
---------------------------
Suppose you are tasked to make a page title bar like the following picture:

![alt text](/guides/img/design-guide-1.png "Logo Title Text 1")

**Step 1:** Making the base layout using layer 0 classes. Note that the component can be divided into two parts: 1- the left section (title section) and the right section (menu section). So the base html along with layer 0 layouting would be:

```html

<!-- making a flexbox to arrange the two section of title and menu horizontally with their vertical position aligned in the center. We use the famous boostrap combination of  "d-flex align-items-center" --> 
<header class="d-flex align-items-center">
    
    <!-- The title section -->
    <section>
        <span>List of Condos</span>
        <a href="/guides/what-is-condo">(What is Condo?)</a>        
    </section>
    
    <!-- menu section - To push this section to right we set the left margin to "auto" using the bootstrap5 class "ms-auto". Please note using <nav> tag since this menu is part of page navigation as stated in the first section of this guide (web standards) -->
    <nav class="ms-auto">
        <menu>
            <li><a class="active">Condo</a></li>
            <li><a>Townhouse</a></li>
            <li><a>House</a></li>
            <li><a>Land</a></li>
        </menu>
    </nav>
    
</header>
```

In this particular example, the layer 0 classes used above are almost enough. The styles in layer 1 will do the rest for us, in particular the ```<menu>``` item has styles defined in layer 1 which makes it like the picture. So all we needed to do for this component could be done with layer 0 and the rest are default styles defined in layer 1. 

But now suppose we want to further customize this component to:
- have light blue color as background
- Have shadow text for the title text ("List of Condos")

**Step 2:** Since these styles are very specific to this particular component, we will need to define them in layer 2 (project specific). So we define a layer 2 class called *ss-page-bar*, and using BEM model, we also include an elemental class *ss-page-bar__title* to add shadow to the title text:

```html
<header class="d-flex align-items-center ss-page-bar">
    
    <!-- The title section -->
    <section>
        <span class="ss-page-bar__title">List of Condos</span>
        <a href="/guides/what-is-condo">(What is Condo?)</a>        
    </section>
    
    <!-- menu section -->
    <nav class="ms-auto">
        <menu>
            <li><a class="active">Condo</a></li>
            <li><a>Townhouse</a></li>
            <li><a>House</a></li>
            <li><a>Land</a></li>
        </menu>
    </nav>
    
</header>
```

```scss

// main "block" class
.ss-page-title {
  
  // light blue background for the component
  background-color: $primary-light-bg;
  
  // in scss & is the parent name, so the expression "&__title" is equivalent to the css class expression ".ss-page-bar__title". This class is used to add shadow to the title text
  &__title {
    text-shadow: 2px 2px #ff0000;
  }
}
```

Other useful hints:
-------------------------

### Margin vs padding

Padding is to *separate* a *wrapper* from its *content*. To add vertical space *between* blocks margin is the standard choice, or you can just add a vertical space using a spacer block, like this one which we included in our project:

```html
<div class="spacer-2"></div>
```

The above block adds a vertical space of size 2-rem (currently we have spacers from 1 to 4 rems).

Do NOT use padding to add space between components. Padding is usually used to separate a wrapper from its contents:

```html
<!-- wrapper block, with possibly a border -->
<div style="padding:4px; border: 1px solid grey"> 
  ... lot of content
</div>
```

The padding for wrapper is usually a full padding (=> includes all four directions) and mostly has the same value for all directions (like padding: 4px). The wrapper also usually has a border around it, so it basically adds a border with a nice padding around your content and allow you to group and organize your contents.

### Using Bootstrap icons

The bootstrap icons are downloaded in the folder `/font`. To include them in your html files just include the file `bootstrap-icons.css` in the header:

```html
<link rel="stylesheet" href="../../font/bootstrap-icons.css">
```

And then use the icons, like:

```html
<i class="bi bi-alarm"></i>
```

### Using SCSS

For a more detailed introduction to SCSS/SASS you may refer to their website, particularly [this link](https://sass-lang.com/guide) is a good place to start.

You need to have sass installed globally:

```
sudo npm install sass -g
```

Now run the script *sass-start* in the project root to begin a sass watch.

```
// from project root:
./sass-start
```

Then every time you make a change to a .scss file in the -sass folder the result will automatically be compiled to css file (bundle.css) in the css folder. If you got *permission deny error* while trying to run this script, it is probably because you need to add *execution permission* to this script:

```
sudo chmod +x sass-start
```

Now you can start editing the scss files and see the results live by including the `css/bundle` file in a html file.