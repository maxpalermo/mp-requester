# `<web-requester>` Manual (English)

A lightweight Web Component (Custom Element) that shows a modal confirmation dialog and returns an asynchronous boolean result via `Promise<boolean>`.

- Source: `../js/web-requester.js`
- HTML manual: `./web-requester-manual.html`

## Quick start

### 1) Load the component script

```html
<script src="asset/js/web-requester.js"></script>
```

### 2) Use the singleton (recommended)

```js
const ok = await WebRequester.confirm({
  title: "Confirm action",
  message: "Are you sure?",
  type: "warning"
});

if (!ok) return;
```

### 3) Or create a manual instance

```js
const wr = document.createElement("web-requester");
document.body.appendChild(wr);

const ok = await wr.confirm({
  title: "Confirm",
  message: "Proceed?",
  type: "info"
});
```

## Screenshot / example

![`<web-requester>` example](image/README/1772711619108.png)

## Themes (palettes)

You can select a global palette with the `theme` attribute/property.

Allowed values:

- `soft`
- `bold`
- `pastel`
- `elegant` (default)
- `glass`

Examples:

```html
<web-requester theme="soft"></web-requester>
```

```js
WebRequester._getSingleton().setTheme("soft");
// or
wr.theme = "soft";
```

You can also override the theme for a single call:

```js
await WebRequester.confirm({
  title: "Confirm",
  message: "Proceed?",
  type: "warning",
  theme: "glass"
});
```

## API

### Instance methods

- `wr.confirm(options): Promise<boolean>`
- `wr.info(options | message): Promise<boolean>`
- `wr.success(options | message): Promise<boolean>`
- `wr.warning(options | message): Promise<boolean>`
- `wr.danger(options | message): Promise<boolean>`
- `wr.setTheme(theme): void`

### Static (singleton) methods

- `WebRequester.confirm(options): Promise<boolean>`
- `WebRequester.info(options | message): Promise<boolean>`
- `WebRequester.success(options | message): Promise<boolean>`
- `WebRequester.warning(options | message): Promise<boolean>`
- `WebRequester.danger(options | message): Promise<boolean>`

### `options`

| field       | type                                                 |        default | notes                                   |
| ----------- | ---------------------------------------------------- | -------------: | --------------------------------------- |
| `title`   | `string`                                           | `"Conferma"` | Shown in the top bar                    |
| `message` | `string`                                           |         `""` | Dialog body text (supports new lines)   |
| `type`    | `"info" \| "success" \| "warning" \| "danger"`        |     `"info"` | Controls top bar + “Yes” button style |
| `theme`   | `"soft" \| "bold" \| "pastel" \| "elegant" \| "glass"` |  `undefined` | Optional per-call palette override      |

## Result mapping

- Clicking **Yes** resolves `true`
- Clicking **No** resolves `false`
- Pressing **Esc** / closing the dialog resolves `false`

## Notes

- The component rejects concurrent requests: calling `confirm()` while another one is pending returns a rejected Promise.
