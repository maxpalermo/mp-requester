# Manuale `<web-requester>` (Italiano)

Web Component (Custom Element) che mostra un dialog modale di conferma e restituisce un risultato asincrono (`Promise<boolean>`).

- Sorgente: `../js/web-requester.js`
- Manuale HTML: `./web-requester-manual.html`

![Esempio messaggio HTML `<web-requester>`](warning.png)

## Avvio rapido

### 1) Carica lo script del componente

```html
<script src="asset/js/web-requester.js"></script>
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

## Screenshot / esempio

![Esempio messaggio HTML `<web-requester>`](html.png)

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
- `wr.error(options | message): Promise<boolean>`
- `wr.ok(options): Promise<void>`
- `wr.doWait(title="Attendere", message="Operazione in corso...", type="info", html=false, theme="soft"): Promise<void>`
- `wr.hideWait(): void`
- `wr.setTheme(theme): void`

### Metodi statici (singleton)

- `WebRequester.confirm(options): Promise<boolean>`
- `WebRequester.info(options | message): Promise<boolean>`
- `WebRequester.success(options | message): Promise<boolean>`
- `WebRequester.warning(options | message): Promise<boolean>`
- `WebRequester.danger(options | message): Promise<boolean>`
- `WebRequester.error(options | message): Promise<boolean>`
- `WebRequester.ok(options): Promise<void>`
- `WebRequester.doWait(title="Attendere", message="Operazione in corso...", type="info", html=false, theme="soft"): Promise<void>`
- `WebRequester.hideWait(): void`

### `options`

| campo       | tipo                                                 |        default | note                                                              |
| ----------- | ---------------------------------------------------- | -------------: | ----------------------------------------------------------------- |
| `title`   | `string`                                           | `"Conferma"` | Mostrato nella barra superiore                                    |
| `message` | `string`                                           |         `""` | Testo messaggio (supporta a-capo)                                 |
| `type`    | `"info" \| "success" \| "warning" \| "danger"`        |     `"info"` | Colore top bar + bottone “Si”                                   |
| `theme`   | `"soft" \| "bold" \| "pastel" \| "elegant" \| "glass"` |  `undefined` | Override palette per singola chiamata                             |
| `html`    | `boolean`                                          |      `false` | Se `true`, `message` viene inserito come HTML (`innerHTML`) |

## Avviso con solo OK

Se ti serve solo avvisare l’utente (senza Si/No), usa `ok()`.

```js
await WebRequester.ok({
  title: "Avviso",
  message: "Export completato correttamente",
  type: "success",
  theme: "soft",
});
```

## Attesa / caricamento (spinner)

Per mostrare una schermata modale di attesa senza bottoni, usa `doWait()` e chiudi con `hideWait()`.

```js
WebRequester.doWait("Attendere", "Esportazione in corso...", "info", false, "soft");

try {
  // operazione lunga
} finally {
  WebRequester.hideWait();
}
```

## Messaggi HTML

Di default, `message` viene trattato come testo. Se ti serve formattazione, passa `html: true`.

```js
await WebRequester.confirm({
  title: "- EXPORT ORDER -",
  message: `
    <div style="display:grid;gap:6px;">
      <b>System Info</b>
      <div>Stai per eseguire <code style="color:#a04040;font-weight:bold;">exportOrder</code>.</div>
      <div style="color:#64748b;font-size:12px;">Questa operazione è irreversibile.</div>
      <div style="color:#64748b;font-size:12px;">Continuare?</div>
    </div>
  `,
  type: "info",
  theme: "soft",
  html: true,
});
```

## Mappatura del risultato

- Click su **Si** => `true`
- Click su **No** => `false`
- Tasto **Esc** / chiusura dialog => `false`

## Note

- Il componente impedisce richieste concorrenti: se chiami `confirm()` mentre un’altra è pendente, la Promise viene rigettata.
