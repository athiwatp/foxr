# foxr

[![npm](https://flat.badgen.net/npm/v/foxr)](https://www.npmjs.com/package/foxr) [![install size](https://flat.badgen.net/packagephobia/install/foxr)](https://packagephobia.now.sh/result?p=foxr) [![tests](https://flat.badgen.net/travis/deepsweet/foxr/master?label=tests)](https://travis-ci.org/deepsweet/foxr) [![coverage](https://flat.badgen.net/codecov/c/github/deepsweet/foxr/master)](https://codecov.io/github/deepsweet/foxr)

Node.js API to control Firefox.

<img src="logo.svg" width="110" height="110" align="right" alt="logo"/>

* uses a built-in [Marionette](https://vakila.github.io/blog/marionette-act-i-automation/) through [remote protocol](https://firefox-source-docs.mozilla.org/testing/marionette/marionette/index.html)
* no [Selenium WebDriver](https://github.com/SeleniumHQ/selenium/wiki/FirefoxDriver) is needed
* works with [Headless mode](https://developer.mozilla.org/en-US/docs/Mozilla/Firefox/Headless_mode)
* compatible subset of [Puppeteer](https://github.com/GoogleChrome/puppeteer) API

At this point Foxr is more a proof of concept, [work is pretty much in progress](https://github.com/deepsweet/foxr/issues?q=is%3Aissue+is%3Aopen+sort%3Aupdated-desc+label%3Aenhancement).

## Example

Run a locally installed Firefox:

```sh
/path/to/firefox -headless -marionette -safe-mode
```

Or a [dockerized version](https://github.com/deepsweet/firefox-headless-remote):

```sh
docker run -it --rm --shm-size 2g -p 2828:2828 deepsweet/firefox-headless-remote:61
```

```js
import foxr from 'foxr'
// const foxr = require('foxr').default

(async () => {
  try {
    const browser = await foxr.connect()
    const page = await browser.newPage()

    await page.goto('https://example.com')
    await page.screenshot({ path: 'example.png' })
    await browser.close()
  } catch (error) {
    console.error(error)
  }
})()
```

## Install

```sh
yarn add --dev foxr
# or
npm install --save-dev foxr
```

## API

### Foxr

#### `connect`

Connect to the Marionette endpoint.

```ts
foxr.connect(options?: { host?: string, port?: number }): Promise<TBrowser>
```

* `host` – `'localhost'` by default
* `port` – `2828` by default

### Browser

#### `close`

```ts
browser.close(): Promise<void>
```

#### `disconnect`

```ts
browser.disconnect(): Promise<void>
```

#### `newPage`

```ts
browser.newPage(): Promise<TPage>
```

#### `pages`

```ts
browser.pages(): Promise<TPage[]>
```

### Page

#### `$`

```ts
page.$(selector: string): Promise<TElement | null>
```

#### `$$`

```ts
page.$$(selector: string): Promise<TElement[]>
```

#### `bringToFront`

```ts
page.bringToFront(): Promise<void>
```

#### `browser`

```ts
page.browser(): TBrowser
```

#### `close`

```ts
page.close(): Promise<void>
```

#### `content`

```ts
page.content(): Promise<string>
```

#### `evaluate`

```ts
page.evaluate(target: string): Promise<TJsonValue>
page.evaluate(target: TSerializableFunction, ...args: TJsonValue[]): Promise<TJsonValue>
```

#### `goto`

```ts
page.goto(url: string): Promise<void>
```

#### `screenshot`

```ts
page.screenshot(options?: { path?: string }): Promise<Buffer>
```

#### `setContent`

```ts
page.setContent(html: string): Promise<void>
```

#### `title`

```ts
page.title(): Promise<string>
```

#### `url`

```ts
page.url(): Promise<string>
```

### Element

#### `$`

```ts
element.$(selector: string): Promise<TElement>
```

#### `$$`

```ts
element.$$(selector: string): Promise<TElement[]>
```

#### `screenshot`

```ts
element.screenshot(options?: { path?: string }): Promise<Buffer>
```

## Development

See [my Start task runner preset](https://github.com/deepsweet/_/tree/master/packages/start-preset-node-ts-lib) for details.

## References

* Python Client: [API](https://marionette-client.readthedocs.io/en/latest/reference.html), [source](https://searchfox.org/mozilla-central/source/testing/marionette/client/)
* Perl Client: [API](https://metacpan.org/pod/Firefox::Marionette), [source](https://metacpan.org/source/DDICK/Firefox-Marionette-0.57/lib/Firefox)
* Node.js client (outdated): [source](https://github.com/mozilla-b2g/gaia/tree/master/tests/jsmarionette/client)
* [Marionette Google Group](https://groups.google.com/forum/#!forum/mozilla.tools.marionette)
