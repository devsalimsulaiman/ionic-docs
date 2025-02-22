---
title: 'ion-router-link'
hide_table_of_contents: true
demoUrl: '/docs/demos/api/router-link/index.html'
demoSourceUrl: 'https://github.com/ionic-team/ionic-docs/tree/main/static/demos/api/router-link/index.html'
---

import TOCInline from '@theme/TOCInline';

import Props from '@ionic-internal/component-api/v6/router-link/props.md';
import Events from '@ionic-internal/component-api/v6/router-link/events.md';
import Methods from '@ionic-internal/component-api/v6/router-link/methods.md';
import Parts from '@ionic-internal/component-api/v6/router-link/parts.md';
import CustomProps from '@ionic-internal/component-api/v6/router-link/custom-props.md';
import Slots from '@ionic-internal/component-api/v6/router-link/slots.md';

<head>
  <title>Router Link | Navigating The ion-router-link Component</title>
  <meta
    name="description"
    content="Use the ion-router-link component to navigate to a specified link. The router link can accept an href for location and a direction for the transition animation."
  />
</head>

import EncapsulationPill from '@components/page/api/EncapsulationPill';

<EncapsulationPill type="shadow" />

<h2 className="table-of-contents__title">Contents</h2>

<TOCInline toc={toc} maxHeadingLevel={2} />

The router link component is used for navigating to a specified link. Similar to the browser's anchor tag, it can accept a href for the location, and a direction for the transition animation.

:::note
Note: this component should only be used with vanilla and Stencil JavaScript projects. For Angular projects, use an `<a>` and `routerLink` with the Angular router.
:::

## Properties

<Props />

## Events

<Events />

## Methods

<Methods />

## CSS Shadow Parts

<Parts />

## CSS Custom Properties

<CustomProps />

## Slots

<Slots />
