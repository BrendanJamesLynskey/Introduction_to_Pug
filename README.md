# ◇ Introduction to Pug

An interactive Reveal.js presentation covering Pug — from indentation-based syntax and template inheritance through to mixins, filters, security, performance, and Express integration.

## ▶ [Open the Presentation](https://brendanjameslynskey.github.io/Introduction_to_Pug/)

## 📄 [Markdown Version](https://github.com/BrendanJamesLynskey/Introduction_to_Pug/blob/main/presentation.md)

---

## Contents

| # | Topic | Description |
|---|-------|-------------|
| 01 | Title | Introduction to Pug — Indentation-Based Templates |
| 02 | Agenda | Overview of all topics covered |
| 03 | What Is Pug? | History as Jade, philosophy, indentation-based syntax, install & basic use |
| 04 | Basic Syntax — Tags, Text, Attributes | Tag nesting, inline/block text, parenthesised attributes |
| 05 | Classes, IDs, and Inline Styles | CSS-selector shorthand, dynamic classes, &attributes syntax |
| 06 | Interpolation & Escaping | Escaped `#{}`, unescaped `!{}`, buffered output, attribute interpolation |
| 07 | Control Flow — Conditionals | if/else, unless, case/when, inline conditionals |
| 08 | Control Flow — Iteration | each, while, each...else, iterating objects, nested iteration |
| 09 | Mixins | Defining, calling, passing blocks, rest args, &attributes |
| 10 | Template Inheritance | extends, block, block append/prepend, multi-level inheritance |
| 11 | Includes | Including files, filters, inheritance vs includes, path resolution |
| 12 | Filters | :markdown-it, :babel, custom filters, jstransformer ecosystem |
| 13 | Express Integration | app.set view engine, res.render, Pug options, no layout plugin needed |
| 14 | Passing Data to Templates | res.render data, res.locals, app.locals, merge order |
| 15 | Security — XSS Prevention | Auto-escaping, raw output, sanitisation with DOMPurify |
| 16 | Performance & Caching | Compilation pipeline, view cache, precompilation, benchmarks |
| 17 | Pug vs Other Engines | Comparison table with EJS, Handlebars, Nunjucks |
| 18 | Real-World Project Structure | Directory layout, naming conventions, app.js setup |
| 19 | Summary & Next Steps | Core takeaways, best practices, resources, key packages |

---

## Slide Controls

| Action | Key |
|--------|-----|
| Next / Previous | `→` `←` or swipe |
| Overview | `Esc` |
| Fullscreen | `F` |
| Export to PDF | Append `?print-pdf` to URL, then print |

## Technology

[Reveal.js 4.6](https://revealjs.com) · [highlight.js](https://highlightjs.org) · Playfair Display + DM Sans + JetBrains Mono

Single self-contained `index.html` — no build step, no npm, no dependencies to install.

## References

- [Pug Official Site](https://pugjs.org) — documentation and API reference
- [Pug GitHub Repository](https://github.com/pugjs/pug) — source code and issues
- [Express.js Template Engines Guide](https://expressjs.com/en/guide/using-template-engines.html) — official Express integration docs
- [jstransformer](https://github.com/jstransformers) — filter ecosystem for Pug
- [MDN Web Docs: Cross-Site Scripting (XSS)](https://developer.mozilla.org/en-US/docs/Glossary/Cross-site_scripting) — security background
- [DOMPurify](https://github.com/cure53/DOMPurify) — HTML sanitisation library
- [express-validator](https://express-validator.github.io/) — form validation middleware

## License

Educational use. Code examples provided as-is.
