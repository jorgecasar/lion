[//]: # 'AUTO INSERT HEADER PREPUBLISH'

# Fieldset

`lion-fieldset` groups multiple input fields or other fieldsets together.

```js script
import { html } from 'lit-html';
import '@lion/input/lion-input.js';
import { localize } from '@lion/localize';
import { loadDefaultFeedbackMessages, MinLength, Validator, Required } from '@lion/validate';
import './lion-fieldset.js';
import './docs/helpers/demo-fieldset-child.js';

export default {
  title: 'Forms/Fieldset/Overview',
};
```

We have three specific fieldset implementations:

- [lion-form](?path=/docs/forms-form-overview--page)
- [lion-checkbox-group](?path=/docs/forms-checkbox-group--default-story)
- [lion-radio-group](?path=/docs/forms-radio-group--default-story)

```js story
export const main = () => html`
  <lion-fieldset name="nameGroup" label="Name">
    <lion-input name="FirstName" label="First Name"></lion-input>
    <lion-input name="LastName" label="Last Name"></lion-input>
  </lion-fieldset>
`;
```

A native fieldset element should always have a legend-element for a11y purposes.
However, our fieldset element is not native and should not have a legend-element.
Our fieldset instead has a label attribute or you can add a label with a div- or heading-element using `slot="label"`.

## Live Demo/Documentation

> See our [storybook](http://lion-web-components.netlify.com/?path=/docs/forms-fieldset-overview--page) for a live demo and documentation

## Features

- Easy retrieval of form data based on field names
- Advanced user interaction scenarios via [interaction states](?path=/docs/forms-system-interaction-states--interaction-states)
- Can have [validate](?path=/docs/forms-validation-overview--page) on fieldset level and shows the validation feedback below the fieldset
- Can disable input fields on fieldset level
- Accessible out of the box

## How to use

### Installation

```sh
npm i --save @lion/fieldset
```

```js
import { LionFieldset } from '@lion/fieldset';
// or
import '@lion/fieldset/lion-fieldset.js';
```

### Example

```html
<lion-fieldset name="personalia" label="personalia">
  <lion-input name="title" label="Title"></lion-input>
</lion-fieldset>
```

For more examples please look at [Fieldset Examples](?path=/docs/forms-fieldset-examples--default-story).
