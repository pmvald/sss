A summary of BEM
==========================

For a quick overview of BEM refer to [this](https://css-tricks.com/building-a-scalable-css-architecture-with-bem-and-utility-classes/).

BEM: Consists of Block, Element and Modifier.

*Block:* Standalone entity that is meaningful on its own. Examples: header, menu, input

*Element:* A part of a block that has no standalone meaning and is semantically tied to its block. Examples: header title, menu item, list item

*Modifier:* A flag on a block or element. Use them to change appearance or behaviour. Examples: disabled, checked, size big, color primary

*Naming rules:*
- Block: Block names may consist of Latin letters, digits, and dashes. To form a CSS class, add a short prefix for namespacing: `.menu`

*Element:* Element names may consist of Latin letters, digits, dashes and underscores. CSS class is formed as block name plus two underscores plus element name: `.menu__list`

*Modifier:* Modifier names may consist of Latin letters, digits, dashes and underscores. CSS class is formed as block’s or element’s name plus two dashes: `.menu__list--active`, `.button--disabled`, `.button--color-red` (Spaces in complicated modifiers are replaced by dash.)
