# ctrovalidate-browser

**DOM-first validation for vanilla JavaScript and HTML.**

`ctrovalidate-browser` provides a `Ctrovalidate` controller class that bridges [`ctrovalidate-core`](https://www.npmjs.com/package/ctrovalidate-core) validation logic to the DOM. Discover fields via data attributes, manage event listeners, display errors, and handle ARIA accessibility — all without a framework.

[![npm version](https://img.shields.io/npm/v/ctrovalidate-browser.svg)](https://www.npmjs.com/package/ctrovalidate-browser)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)

---

## Installation

```bash
npm install ctrovalidate-browser ctrovalidate-core
```

---

## Quick Start

### HTML

```html
<form id="signupForm" novalidate>
  <div>
    <input
      type="email"
      name="email"
      data-ctrovalidate-rules="required|email"
    />
    <div class="error-message"></div>
  </div>

  <div>
    <input
      type="password"
      name="password"
      data-ctrovalidate-rules="required|minLength:8|strongPassword"
    />
    <div class="error-message"></div>
  </div>

  <button type="submit">Sign Up</button>
</form>
```

### JavaScript

```javascript
import { Ctrovalidate } from 'ctrovalidate-browser';

const form = document.querySelector('#signupForm');
const validator = new Ctrovalidate(form, {
  errorClass: 'is-invalid',
  errorMessageClass: 'error-message',
});

form.addEventListener('submit', async (e) => {
  e.preventDefault();
  const isValid = await validator.validate();
  if (isValid) {
    // Submit
  }
});
```

---

## Data Attributes

| Attribute | Description |
|-----------|-------------|
| `data-ctrovalidate-rules` | Pipe-delimited rule string (e.g. `required\|email\|minLength:8`) |
| `data-ctrovalidate-if` | Conditional validation dependency (e.g. `agreeTerms:checked`, `country:value=USA`) |
| `data-ctrovalidate-message` | Catch-all error message for the field |
| `data-ctrovalidate-{ruleName}-message` | Rule-specific error message |

---

## Configuration

```javascript
const validator = new Ctrovalidate(form, {
  realTime: true,                          // Enable blur/input listeners
  errorClass: 'is-invalid border-red-500', // Supports Tailwind space-separated
  errorMessageClass: 'error-message',      // Also space-separated
  pendingClass: 'is-validating',           // Added during async validation
  logLevel: Ctrovalidate.LogLevel.NONE,    // Logging verbosity
  schema: { email: 'required|email' },     // Programmatic rules (merged with HTML)
  aliases: { username: 'required|alphaDash|minLength:3' }, // Instance aliases
});
```

---

## Instance Methods

```javascript
await validator.validate();            // Validate all fields
validator.addField(element);           // Register a field
validator.removeField(element);         // Unregister a field
validator.refresh();                    // Re-discover fields
validator.isDirty('email');             // Check if field was interacted with
validator.getError('email');            // Get current error message
validator.reset();                      // Clear all validation state
validator.destroy();                    // Full cleanup
```

---

## Static Methods

```javascript
Ctrovalidate.defineAlias('password', 'required|minLength:8|strongPassword');
Ctrovalidate.addRule('isEven', (v) => Number(v) % 2 === 0, 'Must be even.');
Ctrovalidate.addAsyncRule('checkEmail', async (v, p, el, signal) => { ... });
Ctrovalidate.setCustomMessages({ required: 'Cannot be empty.' });
```

---

## Accessibility

The controller automatically manages:

- `aria-invalid="true"` on validation failure
- `aria-describedby` linking to the error container
- `role="status"` and `aria-live="polite"` on fallback error containers

---

## Related Packages

- **[ctrovalidate-core](https://www.npmjs.com/package/ctrovalidate-core)** — Validation engine
- **[ctrovalidate-react](https://www.npmjs.com/package/ctrovalidate-react)** — React hook
- **[ctrovalidate-vue](https://www.npmjs.com/package/ctrovalidate-vue)** — Vue composable
- **[ctrovalidate-svelte](https://www.npmjs.com/package/ctrovalidate-svelte)** — Svelte stores
- **[ctrovalidate-next](https://www.npmjs.com/package/ctrovalidate-next)** — Next.js server actions

---

## License

MIT © [Ctrotech](https://github.com/ctrotech-tutor)

Full documentation: https://ctrovalidate.vercel.app
