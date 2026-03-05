# Manuale `<web-requester>` (Italiano)

Web Component (Custom Element) che mostra un dialog modale di conferma e restituisce un risultato asincrono (`Promise<boolean>`).

- Sorgente: `../js/web-requester.js`
- Manuale HTML: `./web-requester-manual.html`

## Avvio rapido

### 1) Carica lo script del componente

```html
<script src="/modules/mpexportdocuments/views/asset/js/web-requester.js"></script>
```

### 2) Usa il singleton (consigliato)

```js
const ok = await WebRequester.confirm({
  title: "Conferma azione",
  message: "Sei sicuro?",
  type: "warning"
});

if (!ok) return;
```

### 3) Oppure crea un’istanza manuale

```js
const wr = document.createElement("web-requester");
document.body.appendChild(wr);

const ok = await wr.confirm({
  title: "Conferma",
  message: "Procedo?",
  type: "info"
});
```

## Tema (palette)

Puoi selezionare la palette globale tramite attributo/proprietà `theme`.

Valori ammessi:

- `soft`
- `bold`
- `pastel`
- `elegant` (default)
- `glass`

Esempi:

```html
<web-requester theme="soft"></web-requester>
```

```js
WebRequester._getSingleton().setTheme("soft");
// oppure
wr.theme = "soft";
```

Puoi anche forzare il tema per una singola chiamata:

```js
await WebRequester.confirm({
  title: "Conferma",
  message: "Procedo?",
  type: "warning",
  theme: "glass"
});
```

## API

### Metodi d’istanza

- `wr.confirm(options): Promise<boolean>`
- `wr.info(options | message): Promise<boolean>`
- `wr.success(options | message): Promise<boolean>`
- `wr.warning(options | message): Promise<boolean>`
- `wr.danger(options | message): Promise<boolean>`
- `wr.setTheme(theme): void`

### Metodi statici (singleton)

- `WebRequester.confirm(options): Promise<boolean>`
- `WebRequester.info(options | message): Promise<boolean>`
- `WebRequester.success(options | message): Promise<boolean>`
- `WebRequester.warning(options | message): Promise<boolean>`
- `WebRequester.danger(options | message): Promise<boolean>`

### `options`

| campo | tipo | default | note |
|---|---|---:|---|
| `title` | `string` | `"Conferma"` | Mostrato nella barra superiore |
| `message` | `string` | `""` | Testo messaggio (supporta a-capo) |
| `type` | `"info" \| "success" \| "warning" \| "danger"` | `"info"` | Colore top bar + bottone “Si” |
| `theme` | `"soft" \| "bold" \| "pastel" \| "elegant" \| "glass"` | `undefined` | Override palette per singola chiamata |

## Mappatura del risultato

- Click su **Si** => `true`
- Click su **No** => `false`
- Tasto **Esc** / chiusura dialog => `false`

## Note

- Il componente impedisce richieste concorrenti: se chiami `confirm()` mentre un’altra è pendente, la Promise viene rigettata.
