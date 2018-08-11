# zisui

A fast and simple CLI to screenshot your Storybook.

## Install

```sh
$ npm install zisui
```

## Usage

If your Storybook server is already started, simply run:

```sh
$ zisui <storybook_url>
```

You can launch your storybook server within `zisui` via the following example:

```
$ zisui --serverCmd "start-storybook -p 3001" http://localhost:3001
```

## Configure

First, you need to register this as a Storybook addon.

```js
/* .storybook/addons.js */

import 'zisui/register';
```

Second, using `withScreenshot` decorator to tell how *zisui* captures your stories.

```js
/* .storybook/config.js */
import { addDecorator } from '@storybook/react';
import { withScreenshot } from 'zisui';

addDecorator(withScreenshot());
```

Also you can decorate some specific stories.

```js
storiesOf('SomeKind', module)
.addDecorator(withScreenshot({
  viewPort: {
    width: 600,
    height: 400,
  },
 }))
.add('a story', () => /* your story component */);
```

### CLI options

Run `zisui --help` .

### API

#### function `withScreenshot`

```typescript
withScreenshot(opt?: ScreenShotOptions): Function;
```

A Storybook decorator to notify *zisui* to screenshot stories.

#### type `ScreenShotOptions`

```
type ScreenShotOptions = {
  waitImages?: boolean,   // default true
  waitFor?: string,       // default ""
  viewPort?: {
    width: number,        // default 800
    height: number,       // default 600
  },
  fullPage?: boolean,     // default true
}
```

- `waitFor` : Sometimes you want control the timing to screenshot. If you set a name of a function to return `Promise`, *zisui* waits the promise is resolved. 

```html
<!-- .storybook/preview-head.html -->
<script>
  function myWait() {
    return new Promise(res => setTimeout(res, 5000));
  }
</script>
```

```js
  withScreenshot({ waitFor: 'myWait' }) // wait for 5 seconds.
```

## License
MIT