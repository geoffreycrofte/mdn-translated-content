---
title: "ARIA: le role tab"
slug: Web/Accessibility/ARIA/Roles/tab_role
tags:
  - ARIA
  - ARIA Role
  - ARIA Tab
  - ARIA widget
  - Référence
spec-urls:
  - https://w3c.github.io/aria/#tab
  - https://w3c.github.io/aria-practices/#tabpanel
---

Le role ARIA `tab` indique un élément interactif à l'intérieur d'une `tablist` qui, quand il est activé, affiche le `tabpanel` qui lui est associé.

```html
<button role="tab" aria-selected="true" aria-controls="tabpanel-id" id="tab-id">
  Libellé d'une Tab
</button>
```

## Description

Un élément avec le rôle `tab` contrôle la visibilité d'un élément qui lui est associé grâce au rôle [`tabpanel`](/fr/docs/Web/Accessibility/ARIA/Roles/tabpanel_role). L'expérience utilisateur commune pour ce genre de modèle est un regroupement visuel d'onglets au-dessus ou sur le côté d'une zone de contenu, et sélectionner un onglet différent change le contenu de cette zone. Un aspect visuel différent permet de mettre en évidence l'onglet sélectionné.

Les éléments ayant le rôle `tab` _doivent_ soit être un enfant d'un élément ayant le rôle `tablist`, soit avoir leu `i` dans la propriété [`aria-owns`](/fr/docs/Web/Accessibility/ARIA/Attributes/aria-owns) d'une `tablist`. Cette combinaison indique à la technologie d'assistance que l'élément fait partie d'un groupe d'éléments connexes. Certaines technologies d'assistance fournissent un compte du nombre d'éléments de rôle `tab` à l'intérieur d'une `tablist` et informent les utilisateurs de la `tab` qu'ils ciblent actuellement. En outre, un élément doté du rôle `tab` _doit_ contenir la propriété `aria-controls` identifiant un `tabpanel` correspondant (qui a un rôle de `tabpanel`) par l'`id` de cet élément. Lorsqu'un élément avec le rôle `tabpanel` a le focus, ou qu'un de ses enfants a le focus, cela indique que l'élément connecté avec le rôle `tab` est l'onglet actif dans une liste d'onglets.

Lorsque des éléments ayant le rôle `tab` sont sélectionnés ou actifs, leur attribut `aria-selected` doit avoir la valeur `true`. Dans le cas contraire, leur attribut `aria-selected` doit avoir la valeur `false`. Lorsqu'une `tablist` à sélection unique est sélectionnée ou active, l'attribut `hidden` des autres panneaux d'onglets doit être défini sur `true` jusqu'à ce que l'utilisateur sélectionne l'onglet associé à ce panneau d'onglets. Lorsqu'une `tablist` à sélection multiple est sélectionnée ou active, l'attribut [`aria-expanded`](/fr/docs/Web/Accessibility/ARIA/Attributes/aria-expanded) de la `tablist` contrôlée correspondant doit être défini sur `true` et son attribut `hidden` sur `false`, sinon c'est l'inverse.

### Tous les descendants sont présentationnelles

There are some types of user interface components that, when represented in a platform accessibility API, can only contain text. Accessibility APIs do not have a way of representing semantic elements contained in a `tab`. To deal with this limitation, browsers, automatically apply role [`presentation`](/en-US/docs/Web/Accessibility/ARIA/Roles/presentation_role) to all descendant elements of any `tab` element as it is a role that does not support semantic children.

For example, consider the following `tab` element, which contains a heading.

```html
<div role="tab"><h3>Title of my tab</h3></div>
```

Because descendants of `tab` are presentational, the following code is equivalent:

```html
<div role="tab"><h3 role="presentation">Title of my tab</h3></div>
```

From the assistive technology user's perspective, the heading does not exist since the previous code snippets are equivalent to the following in the [accessibility tree](/en-US/docs/Glossary/Accessibility_tree):

```html
<div role="tab">Title of my tab</div>
```

### Associated roles and attributes

- [`aria-selected`](/en-US/docs/Web/Accessibility/ARIA/Attributes/aria-selected)
  - : boolean
- [`aria-controls`](/en-US/docs/Web/Accessibility/ARIA/Attributes/aria-controls)
  - : `id` of element with `tabpanel` role
- {{htmlattrxref("id")}}
  - : content

### Keyboard interaction

| Key               | Action                                                                                                                                                                                                           |
| ----------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <kbd>Tab</kbd>    | When focus is outside of the `tablist` moves focus to the active tab. If focus is on the active tab moves focus to the next element in the keyboard focus order, ideally the active tab's associated `tabpanel`. |
| <kbd>→</kbd>      | Focuses and optionally activates the next tab in the tab list. If the current tab is the last tab in the tab list it activates the first tab.                                                                    |
| <kbd>←</kbd>      | Focuses and optionally activates the previous tab in the tab list. If the current tab is the first tab in the tab list it activates the last tab.                                                                |
| <kbd>Delete</kbd> | When allowed removes the currently selected tab from the tab list.                                                                                                                                               |

### Required JavaScript features

> **Note:** While there are ways to build tab-like functionality without JavaScript, there is no substitute combination using only HTML and CSS that will provide the same set of functionality that's required above for accessible tabs with content.

## Example

This example combines the role `tab` with `tablist` and elements with `tabpanel` to create an interactive group of tabbed content. Here we are enclosing our group of content in a `div`, with our `tablist` having an `aria-label` which labels it for assistive technology. Each `tab` is a `button` with the attributes previously mentioned. The first `tab` has both `tabindex="0"` and `aria-selected="true"` applied. These two attributes must always be coordinated as such—so when another tab is selected, it will then have `tabindex="0"` and `aria-selected="true"` applied. All unselected tabs must have `aria-selected="false"` and `tabindex="-1"`.

All of the `tabpanel` elements have `tabindex="0"` to make them tabbable, and all but the currently active one have the `hidden` attribute. The `hidden` attribute will be removed when a `tabpanel` becomes visible with JavaScript. There is some basic styling applied that restyles the buttons and changes the [`z-index`](/en-US/docs/Web/CSS/z-index) of `tab` elements to give the illusion of it connecting to the `tabpanel` for active elements, and the illusion that inactive elements are behind the active `tabpanel`.

```html
<div class="tabs">
  <div role="tablist" aria-label="Sample Tabs">
    <button
      role="tab"
      aria-selected="true"
      aria-controls="panel-1"
      id="tab-1"
      tabindex="0">
      First Tab
    </button>
    <button
      role="tab"
      aria-selected="false"
      aria-controls="panel-2"
      id="tab-2"
      tabindex="-1">
      Second Tab
    </button>
    <button
      role="tab"
      aria-selected="false"
      aria-controls="panel-3"
      id="tab-3"
      tabindex="-1">
      Third Tab
    </button>
  </div>
  <div id="panel-1" role="tabpanel" tabindex="0" aria-labelledby="tab-1">
    <p>Content for the first panel</p>
  </div>
  <div id="panel-2" role="tabpanel" tabindex="0" aria-labelledby="tab-2" hidden>
    <p>Content for the second panel</p>
  </div>
  <div id="panel-3" role="tabpanel" tabindex="0" aria-labelledby="tab-3" hidden>
    <p>Content for the third panel</p>
  </div>
</div>
```

```css hidden
.tabs {
  padding: 1em;
}

[role="tablist"] {
  margin-bottom: -1px;
}

[role="tab"] {
  position: relative;
  z-index: 1;
  background: white;
  border-radius: 5px 5px 0 0;
  border: 1px solid grey;
  border-bottom: 0;
  padding: 0.2em;
}

[role="tab"][aria-selected="true"] {
  z-index: 3;
}

[role="tabpanel"] {
  position: relative;
  padding: 0 0.5em 0.5em 0.7em;
  border: 1px solid grey;
  border-radius: 0 0 5px 5px;
  background: white;
  z-index: 2;
}

[role="tabpanel"]:focus {
  border-color: orange;
  outline: 1px solid orange;
}
```

There are two things we need to do with JavaScript: we need to change focus and tab index of our `tab` elements with the right and left arrows, and we need to change the active `tab` and `tabpanel` when we click on a `tab`.

To accomplish the first, we listen for the [`keydown`](/en-US/docs/Web/API/Element/keydown_event) event on the `tablist`. If the event's [`key`](/en-US/docs/Web/API/KeyboardEvent/key) is `ArrowRight` or `ArrowLeft`, we react to the event. We start by setting the `tabindex` of the current `tab` element to -1, making it no longer tabbable. Then, if the right arrow is being pressed, we increase our tab focus counter by one. If the counter is greater than the number of `tab` elements we have, we circle back to the first tab by setting that counter to 0. If the left arrow is being pressed, we decrease our tab focus counter by one, and if it is then less than 0, we set it to the number of `tab` elements minus one (to get to the last element). Finally, we set `focus` to the `tab` element whose index is equal to the tab focus counter, and set its `tabindex` to 0 to make it tabbable.

To handle changing the active `tab` and `tabpanel`, we have a function that takes in the event, gets the element that triggered the event, the triggering element's parent element, and its grandparent element. We then find all tabs with `aria-selected="true"` inside the parent element and sets it to `false`, then sets the triggering element's `aria-selected` to `true`. After that, we find all `tabpanel` elements in the grandparent element, make them all `hidden`, and finally select the element whose `id` is equal to the triggering `tab`'s `aria-controls` and removes the `hidden` attribute, making it visible.

```js
window.addEventListener("DOMContentLoaded", () => {
  const tabs = document.querySelectorAll('[role="tab"]');
  const tabList = document.querySelector('[role="tablist"]');

  // Add a click event handler to each tab
  tabs.forEach((tab) => {
    tab.addEventListener("click", changeTabs);
  });

  // Enable arrow navigation between tabs in the tab list
  let tabFocus = 0;

  tabList.addEventListener("keydown", (e) => {
    // Move right
    if (e.key === 'ArrowRight' || e.key === 'ArrowLeft') {
      tabs[tabFocus].setAttribute("tabindex", -1);
      if (e.key === 'ArrowRight') {
        tabFocus++;
        // If we're at the end, go to the start
        if (tabFocus >= tabs.length) {
          tabFocus = 0;
        }
        // Move left
      } else if (e.key === 'ArrowLeft') {
        tabFocus--;
        // If we're at the start, move to the end
        if (tabFocus < 0) {
          tabFocus = tabs.length - 1;
        }
      }

      tabs[tabFocus].setAttribute("tabindex", 0);
      tabs[tabFocus].focus();
    }
  });
});

function changeTabs(e) {
  const target = e.target;
  const parent = target.parentNode;
  const grandparent = parent.parentNode;

  // Remove all current selected tabs
  parent
    .querySelectorAll('[aria-selected="true"]')
    .forEach((t) => t.setAttribute("aria-selected", false));

  // Set this tab as selected
  target.setAttribute("aria-selected", true);

  // Hide all tab panels
  grandparent
    .querySelectorAll('[role="tabpanel"]')
    .forEach((p) => p.setAttribute("hidden", true));

  // Show the selected panel
  grandparent.parentNode
    .querySelector(`#${target.getAttribute("aria-controls")}`)
    .removeAttribute("hidden");
}
```

{{EmbedLiveSample("Example", 600, 130)}}

## Best practices

It is recommended to use a {{HTMLElement('button')}} element with the role `tab` for their built-in functional and accessible features instead, as opposed to needing to add them yourself. For controlling tab key functionality for elements with the role `tab`, it is recommended to set all non-active elements to `tabindex="-1"`, and to set the active element to `tabindex="0"`.

## Precedence order

What are the related properties, and in what order will this attribute or property be read, which property will take precedence over this one, and which property will be overwritten.

## Specifications

{{Specifications}}

## See also

- HTML {{HTMLElement('button')}} element
- [KeyboardEvent.key](/en-US/docs/Web/API/KeyboardEvent/key)
- [ARIA `tabpanel` role](/en-US/docs/Web/Accessibility/ARIA/Roles/tabpanel_role)

<section id="Quick_links">

1. [**WAI-ARIA roles**](/en-US/docs/Web/Accessibility/ARIA/Roles)

   {{ListSubpagesForSidebar("/en-US/docs/Web/Accessibility/ARIA/Roles")}}

</section>
