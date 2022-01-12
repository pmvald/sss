CSS Architencture Introduction
======================================

Currently there are several well-known CSS architecture/model out there, all aiming at making it easy to write css for a *"big project"* as they claim. Here are links to three most famous ones:

- [BEM](http://getbem.com/)
- [OOCSS](https://github.com/stubbornella/oocss/wiki)
- [SMACSS](http://smacss.com/book/)

For a general comparison take a look [here](https://snipcart.com/blog/organize-css-modular-architecture)

In short, each of these architectures are basically a bunch of rules telling us how we should name the css classes in order to make it easy to udnerstand the role of each class and avoid name collision. For example, consider the following html:

```html
<ul>
    <li> some stuff 1 </li>
    <li> some stuff 2 </li>
    <li> some stuff 3 </li>
</ul>
```

Now in BEM framework, if you call the class for the main list (```<ul>```) *mylist*, then the class for each item should have the format like *mylist__item*.

```html
<ul class="mylist">
    <li class="mylist__item"> some stuff 1 </li>
    <li class="mylist__item"> some stuff 2 </li>
    <li class="mylist__item"> some stuff 3 </li>
</ul>
```

Here the notation double underscore (__) is used to mean that this element is the *"item"* (class) OF the *mylist* (class) ...

In our projects we mainly use BEM as the base architecture
