# CodeByMasood Datepicker

Hey! 👋  
This is my personal fork of Pardis Jalali Datepicker — a modern, zero-dependency Persian (Jalali/Shamsi) datepicker. Supports multiple instances, inline mode, range selection, input masking, and three cool themes.

---

---

## Features

- **Zero dependencies** — pure vanilla JS, no external libraries
- **Headless architecture** — engine, renderer, and input mask are fully decoupled
- **Multi-instance** — any number of independent pickers on one page
- **Inline mode** — always-visible calendar without an input field
- **Range selection** — pick a start and end date with hover preview
- **Input masking** — auto-formats Persian digits with slash separators
- **Three themes** — Modern, Glassmorphism, Classic/Dark
- **Dual output** — returns both Jalali and Gregorian date data simultaneously
- **Precise conversion** — correct Jalaali ↔ Gregorian algorithm (integer division, not `Math.floor`)
- **RTL** — fully right-to-left layout

---

## Project Structure

```
codebymasood-datepicker/
├── lib/
│   ├── codebymasood-datepicker.js   # Library — classes only
│   └── codebymasood-datepicker.css  # Library — CSS variables, themes, component styles
├── demo/
│   ├── demo.js                        # Demo page script
│   └── demo.css                       # Demo page styles
└── index.html                         # Interactive demo page
```

---

## Quick Start

Include the library files and create a datepicker on any `<input>`:

```html
<link rel="stylesheet" href="lib/codebymasood-datepicker.css" />

<input id="myInput" class="cbm-input" type="text" placeholder="۱۴۰۴/۰۱/۰۱" />

<script src="lib/codebymasood-datepicker.js"></script>
<script>
  const dp = new CbmDatepicker("#myInput", {
    outputFormat: "both",
    onChange: (payload) => {
      console.log(payload.jalali.formatted); // '1404/01/01'
      console.log(payload.gregorian.formatted); // '2025-03-21'
    },
  });
</script>
```

---

## Installation

### npm

```bash
npm install codebymasood-datepicker
```

### Manual

Copy `lib/codebymasood-datepicker.js` and `lib/codebymasood-datepicker.css` into your project and include them directly.

> **Note:** The library uses the `Vazirmatn` font by default (via CSS variable `--cbm-font`). Load it yourself (e.g. from Google Fonts) or override the variable with your preferred font.

---

## API Reference

### `new CbmDatepicker(target, options)`

| Option          | Type                                    | Default       | Description                                                                                                        |
| --------------- | --------------------------------------- | ------------- | ------------------------------------------------------------------------------------------------------------------ |
| `inline`        | `boolean`                               | `false`       | Render calendar always-visible inside the target element (no input needed)                                         |
| `rangeMode`     | `boolean`                               | `false`       | Enable range selection (start + end date)                                                                          |
| `outputFormat`  | `'both'` \| `'jalali'` \| `'gregorian'` | `'both'`      | Shape of the payload passed to callbacks                                                                           |
| `initialYear`   | `number`                                | current year  | Jalali year to display on first render                                                                             |
| `initialMonth`  | `number`                                | current month | Jalali month (1–12) to display on first render                                                                     |
| `minDate`       | `{jy, jm, jd}`                          | `null`        | Earliest selectable date                                                                                           |
| `maxDate`       | `{jy, jm, jd}`                          | `null`        | Latest selectable date                                                                                             |
| `onChange`      | `function`                              | `null`        | Called when a single date is selected. Receives a [date payload](#date-payload)                                    |
| `onRangeStart`  | `function`                              | `null`        | Called when the first date of a range is picked. Receives a [date payload](#date-payload)                          |
| `onRangeSelect` | `function`                              | `null`        | Called when both range dates are selected. Receives `{ start, end }` where each is a [date payload](#date-payload) |
| `onClear`       | `function`                              | `null`        | Called when the selection is cleared                                                                               |

---

### Methods

| Method                     | Description                                                                                  |
| -------------------------- | -------------------------------------------------------------------------------------------- |
| `dp.open()`                | Open the popover (no-op in inline mode)                                                      |
| `dp.close()`               | Close the popover (no-op in inline mode)                                                     |
| `dp.getValue()`            | Returns the current date payload, or `null` if nothing is selected                           |
| `dp.setValue(jy, jm, jd)`  | Programmatically select a Jalali date                                                        |
| `dp.clear()`               | Clear the current selection                                                                  |
| `dp.setOption(key, value)` | Update an option after construction (currently supports `rangeMode` and `outputFormat` only) |
| `dp.destroy()`             | Remove all event listeners and DOM elements created by this instance                         |

Access the underlying engine directly via `dp.engine` for advanced use.

---

### Date Payload

When `outputFormat: 'both'` (default), callbacks receive:

```js
{
  jalali: {
    year,             // 1404
    month,            // 1
    day,              // 1
    monthName,        // 'فروردین'
    formatted,        // '1404/01/01'
    formattedPersian, // '۱۴۰۴/۰۱/۰۱'
    timestamp         // Unix ms
  },
  gregorian: {
    year,             // 2025
    month,            // 3
    day,              // 21
    monthName,        // 'March'
    formatted,        // '2025-03-21'
    date,             // native Date object
    timestamp         // Unix ms
  },
  iso,                // '2025-03-21'
  timestamp           // Unix ms
}
```

When `outputFormat: 'jalali'`, the Jalali fields are returned directly (no nesting).  
When `outputFormat: 'gregorian'`, the Gregorian fields are returned directly.

---

---

## Themes

Apply a theme by setting `data-cbm-theme` on `<html>` and a body class:

| Theme            | `data-cbm-theme`     | `<body>` class  |
| ---------------- | -------------------- | --------------- |
| Modern (default) | _(remove attribute)_ | `theme-modern`  |
| Glassmorphism    | `glass`              | `theme-glass`   |
| Classic / Dark   | `classic`            | `theme-classic` |

```js
// Switch to glass theme
document.documentElement.setAttribute("data-cbm-theme", "glass");
document.body.className = "theme-glass";

// Switch back to modern
document.documentElement.removeAttribute("data-cbm-theme");
document.body.className = "theme-modern";
```

---

## Input Styling

Add the `cbm-input` class to your `<input>` for the built-in styled input:

```html
<div class="cbm-input-wrapper">
  <input
    class="cbm-input"
    id="myInput"
    type="text"
    placeholder="۱۴۰۴/۰۱/۰۱"
    autocomplete="off"
  />
  <span class="cbm-input-icon">📅</span>
</div>
```

input wrapper is created automatically by `CbmDatepicker` if it does not already exist. You can also wrap it yourself for custom layouts.

## Browser Support

Works in all modern browsers (Chrome, Firefox, Safari, Edge). No polyfills required.

---

## License

MIT

## Support

If you like this project and want to support me, why not buy me a coffee? ☕  
Your support keeps me coding and improving this library!

[![Buy me a coffee](https://img.shields.io/badge/Buy%20Me%20a%20Coffee-❤️%20Support-orange?style=for-the-badge)](https://reymit.ir/codebymasood/?hosted_button_id=YOUR_BUTTON_ID)
