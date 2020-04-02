[//]: # 'AUTO INSERT HEADER PREPUBLISH'

# Tooltip

`lion-tooltip` is a component used for basic popups on hover.
Its purpose is to show content appearing when the user hovers over an invoker element with the cursor or with the keyboard, or if th
e invoker element is focused.

```js script
import { css, html } from '@lion/core';
import { LionTooltipArrow } from './index.js';
import './lion-tooltip.js';
import './lion-tooltip-arrow.js';

export default {
  title: 'Overlays/Tooltip',
  // parameters: {
  //   component: 'lion-tooltip',
  //   options: {
  //     selectedPanel: 'storybookjs/knobs/panel'
  //   }
  // }
  // decorators: [withKnobs]
};

const tooltipDemoStyles = css`
  .demo-tooltip-invoker {
    margin: 50px;
  }

  .demo-tooltip-content {
    display: block;
    font-size: 16px;
    color: white;
    background-color: black;
    border-radius: 4px;
    padding: 8px;
  }

  .demo-box-placements {
    display: flex;
    flex-direction: column;
    margin: 40px 0 0 200px;
  }

  .demo-box-placements lion-tooltip {
    margin: 20px;
  }
`;
```

```js preview-story
export const main = () => html`
    <style>${tooltipDemoStyles}</style>
    <lion-tooltip>
      <button slot="invoker" class="demo-tooltip-invoker">Hover me</button>
      <div slot="content" class="demo-tooltip-content">This is a tooltip<div>
    </lion-tooltip>
  `;
```

## Live Demo/Documentation

> See our [storybook](http://lion-web-components.netlify.com/?path=/docs/overlays-specific-wc-tooltip) for a live demo and API documentation

## Features

- Show content when hovering the invoker
- Show content when the invoker is focused
- Uses Popper.js under the hood, to have the content pop up relative to the invoker
- Use `.config` to override the overlay configuration
- Config has `popperConfig` property that has a one to one relation with Popper.js configuration API.
- Comes with a default `lion-tooltip-arrow` for an arrow.

## How to use

### Installation

```sh
npm i --save @lion/tooltip
```

```js
import { LionTooltip } from '@lion/tooltip';
// or
import '@lion/tooltip/lion-tooltip.js';
```

## Examples

### Placements

You can easily change the placement of the content node relative to the invoker.

```js preview-story
export const placements = () => html`
  <style>
    ${tooltipDemoStyles}
  </style>
  <div class="demo-box-placements">
    <lion-tooltip .config=${{ popperConfig: { placement: 'top' } }}>
      <button slot="invoker">Top</button>
      <div slot="content" class="demo-tooltip-content">Its top placement</div>
      <lion-tooltip-arrow slot="arrow"></lion-tooltip-arrow>
    </lion-tooltip>
    <lion-tooltip .config=${{ popperConfig: { placement: 'right' } }}>
      <button slot="invoker">Right</button>
      <div slot="content" class="demo-tooltip-content">Its right placement</div>
      <lion-tooltip-arrow slot="arrow"></lion-tooltip-arrow>
    </lion-tooltip>
    <lion-tooltip .config=${{ popperConfig: { placement: 'bottom' } }}>
      <button slot="invoker">Bottom</button>
      <div slot="content" class="demo-tooltip-content">Its bottom placement</div>
      <lion-tooltip-arrow slot="arrow"></lion-tooltip-arrow>
    </lion-tooltip>
    <lion-tooltip .config=${{ popperConfig: { placement: 'left' } }}>
      <button slot="invoker">Left</button>
      <div slot="content" class="demo-tooltip-content">Its left placement</div>
      <lion-tooltip-arrow slot="arrow"></lion-tooltip-arrow>
    </lion-tooltip>
  </div>
`;
```

### Override Popper configuration

You can override the Popper configuration, the API is one to one with Popper's API, and includes modifiers.

```js preview-story
export const overridePopperConfig = () => html`
    <style>${tooltipDemoStyles}</style>
    <lion-tooltip .config=${{
      popperConfig: {
        placement: 'bottom-start',
        positionFixed: true,
        modifiers: {
          keepTogether: {
            enabled: true,
          },
          preventOverflow: {
            enabled: false,
            boundariesElement: 'viewport',
            padding: 16,
          },
          flip: {
            enabled: true,
            boundariesElement: 'viewport',
            padding: 4,
          },
          offset: {
            enabled: true,
            offset: `0, 4px`,
          },
        },
      },
    }}>
      <button slot="invoker" class="demo-tooltip-invoker">Hover me</button>
      <div slot="content" class="demo-tooltip-content">This is a tooltip<div>
    </lion-tooltip>
  `;
```

Modifier explanations:

- keepTogether: prevents detachment of content element from reference element, which could happen if the invoker is not in the viewport anymore. If this is disabled, the content will always be in view.
- preventOverflow: enables shifting and sliding behavior on the secondary axis (e.g. if top placement, this is horizontal shifting and sliding). The padding property defines the margin with the boundariesElement, which is usually the viewport.
- flip: enables flipping behavior on the primary axis (e.g. if top placement, flipping to bottom if there is not enough space on the top). The padding property defines the margin with the boundariesElement, which is usually the viewport.
- offset: enables an offset between the content node and the invoker node. First argument is horizontal marign, second argument is vertical margin.

### Arrow

Popper also comes with an arrow modifier.
In our tooltip you can pass an arrow element (e.g. an SVG Element) through the `slot="arrow"`.

We export a `lion-tooltip-arrow` that you can use by default for this.

```js preview-story
export const arrow = () => html`
    <style>${tooltipDemoStyles}</style>
    <lion-tooltip>
      <button slot="invoker" class="demo-tooltip-invoker">Hover me</button>
      <div slot="content" class="demo-tooltip-content">This is a tooltip<div>
      <lion-tooltip-arrow slot="arrow"></lion-tooltip-arrow>
    </lion-tooltip>
  `;
```

#### Use a custom arrow

If you plan on passing your own arrow element, you can extend the `lion-tooltip-arrow`.

All you need to do is override the `render` method to pass your own SVG, and extend the styles to pass the proper dimensions of your arrow.
The rest of the work is done by Popper.js (for positioning) and the `lion-tooltip-arrow` (arrow dimensions, rotation, etc.).

```js preview-story
export const customArrow = () => {
  if (!customElements.get('custom-tooltip-arrow')) {
    customElements.define(
      'custom-tooltip-arrow',
      class extends LionTooltipArrow {
        static get styles() {
          return [
            super.styles,
            css`
              :host {
                --tooltip-arrow-width: 20px;
                --tooltip-arrow-height: 8px;
              }
            `,
          ];
        }
        render() {
          return html`
            <svg viewBox="0 0 20 8">
              <path d="M 0,0 h 20 L 10,8 z"></path>
            </svg>
          `;
        }
      },
    );
  }
  return html`
      <style>${tooltipDemoStyles}</style>
      <lion-tooltip>
        <button slot="invoker" class="demo-tooltip-invoker">Hover me</button>
        <div slot="content" class="demo-tooltip-content">This is a tooltip<div>
        <custom-tooltip-arrow slot="arrow"></custom-tooltip-arrow>
      </lion-tooltip>
    `;
};
```

#### Rationale

**Why a Web Component for the Arrow?**

Our preferred API is to pass the arrow as a slot.
Popper.JS however, necessitates that the arrow element is **inside** the content node,
otherwise it cannot compute the positioning of the Popper element and the arrow element, so we move the arrow node inside the content node.

This means that we can no longer style the arrow through the `lion-tooltip` with the `::slotted` selector,
because the arrow element is no longer a direct child of `lion-tooltip`.

Additionally we now also need to sync the popper placement state to the arrow element, to style it properly.
For all this logic + style encapsulation, we decided a Web Component was the only reasonable solution for the arrow element.
