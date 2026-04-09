# Introduction to Pug — Presentation Notes

---

## Slide 01 — Title

**Introduction to Pug**

Indentation-Based Templates — Clean Markup Without the Angle Brackets

19 slides · syntax, inheritance, mixins, filters, security, performance · 2026

---

## Slide 02 — Agenda

### Foundations
- What Is Pug?
- Basic Syntax — Tags, Text, Attributes
- Classes, IDs, and Inline Styles
- Interpolation & Escaping
- Control Flow — Conditionals

### Composition
- Control Flow — Iteration
- Mixins
- Template Inheritance
- Includes
- Filters

### Express & Data
- Express Integration
- Passing Data to Templates
- Security — XSS Prevention
- Performance & Caching

### Production
- Pug vs Other Engines
- Real-World Project Structure
- Summary & Next Steps

---

## Slide 03 — What Is Pug?

### History: From Jade to Pug

Pug was originally called **Jade**, created by TJ Holowaychuk in 2010. Renamed to **Pug** in 2016 due to a trademark conflict. The philosophy: write less markup, produce clean HTML through indentation-based syntax — no closing tags, no angle brackets.

### Key Characteristics

- **Whitespace-significant** — indentation defines nesting
- **No closing tags** — cleaner, more readable templates
- **Built-in inheritance** — extends & block without plugins
- **Mixins** — reusable parameterised components
- **~1.5M weekly npm downloads** — established ecosystem
- **Full JS expressions** — any valid JS works inline

### Install & Basic Use

```bash
npm install pug
```

```javascript
const pug = require('pug');

// Compile a template string
const fn = pug.compile('h1= title');
const html = fn({ title: 'Hello Pug' });
// <h1>Hello Pug</h1>

// Render a file directly
const html2 = pug.renderFile(
  './views/index.pug',
  { users: [...] }
);
```

### File Extension

Convention: `.pug` files (previously `.jade`). If upgrading from Jade, rename files and update `require('jade')` to `require('pug')`.

---

## Slide 04 — Basic Syntax — Tags, Text, Attributes

### Tags & Nesting

```pug
//- Pug
div
  h1 Welcome
  p This is a paragraph.
  ul
    li Item one
    li Item two
```

```html
<!-- HTML Output -->
<div>
  <h1>Welcome</h1>
  <p>This is a paragraph.</p>
  <ul>
    <li>Item one</li>
    <li>Item two</li>
  </ul>
</div>
```

### Inline & Block Text

```pug
//- Inline text after tag
p Hello world

//- Multi-line with pipe |
p
  | This is line one.
  | This is line two.

//- Block text with dot .
script.
  console.log('hello');
  alert('world');
```

### Attributes

```pug
//- Parenthesised attributes
a(href="/about", target="_blank") About

//- Boolean attributes
input(type="checkbox", checked)
input(type="text", disabled)

//- Multi-line attributes
input(
  type="text"
  name="email"
  placeholder="you@example.com"
  required
)
```

### Self-Closing Tags

```pug
//- Void elements auto-close
img(src="/logo.png", alt="Logo")
br
hr
input(type="hidden", name="_csrf")
```

Pug knows which HTML elements are void and self-closes them automatically — no trailing `/` needed.

---

## Slide 05 — Classes, IDs, and Inline Styles

### CSS-Selector Shorthand

```pug
//- Classes with .
div.container
  p.lead.text-center Hello

//- IDs with #
div#app
  h1#title My App

//- Combined
div#main.wrapper.dark

//- Implicit div (omit tag name)
.container
  #sidebar
    p Content
```

### Dynamic Classes & IDs

```pug
//- Expression in attribute
div(class=isActive ? 'on' : 'off')

//- Array of classes
div(class=['btn', 'btn-' + size])

//- Object syntax (truthy = included)
div(class={active: isActive, hidden: !show})

//- Mix shorthand + dynamic
a.nav-link(class={active: current})
```

### Inline Styles

```pug
//- String style
div(style="color: red; font-size: 14px;")

//- Object style
div(style={color: 'red', fontSize: '14px'})
```

### The &attributes Syntax

```pug
//- Spread attributes from an object
- var attrs = {id: 'box', class: 'wide'}
div&attributes(attrs) Hello
```

Useful for passing dynamic attribute sets into mixins or components.

---

## Slide 06 — Interpolation & Escaping

### `#{}` — Escaped (Safe)

```pug
//- Escaped interpolation (default)
p Welcome, #{user.name}!
p You have #{count} messages.

//- Equivalent buffered output
p= 'Welcome, ' + user.name + '!'
```

If `user.name` = `<b>Jo</b>`, output is: `<p>Welcome, &lt;b&gt;Jo&lt;/b&gt;!</p>`

Characters escaped: `&` `<` `>` `"` `'`

### Buffered vs Unbuffered Code

```pug
//- Buffered (outputs escaped)
p= user.name

//- Unbuffered (no output, runs JS)
- var x = 10
- var items = ['a', 'b', 'c']

//- Unescaped buffered
p!= richContent
```

### `!{}` — Unescaped (Dangerous)

```pug
//- Unescaped interpolation
div !{htmlContent}

//- Unescaped buffered output
div!= htmlContent
```

HTML renders as-is. **Dangerous with user input!**

### Attribute Interpolation

```pug
//- JS expressions in attributes
a(href='/users/' + user.id)= user.name

//- ES6 template literals
a(href=`/users/${user.id}`) Profile

//- Escaped by default in attributes
input(value=userInput)
```

### When To Use Unescaped

- Pre-sanitised CMS / Markdown HTML
- Trusted SVG or icon markup
- **Never** with raw user input

---

## Slide 07 — Control Flow — Conditionals

### if / else if / else

```pug
if user.role === 'admin'
  .badge.admin Admin
else if user.role === 'editor'
  .badge.editor Editor
else
  .badge.member Member
```

No braces, no parentheses — indentation defines scope.

### unless (Negated if)

```pug
//- unless = if NOT
unless user.isVerified
  .alert Please verify your email.

//- Equivalent to:
if !user.isVerified
  .alert Please verify your email.
```

### case / when (Switch)

```pug
case user.role
  when 'admin'
    p Full access granted
  when 'editor'
    p Edit access granted
  when 'viewer'
    p Read-only access
  default
    p No special permissions
```

### Inline Conditionals

```pug
//- Ternary in attributes
a(class=active ? 'on' : 'off') Link

//- Conditional attributes
input(type="text", disabled=isLocked)
//- If isLocked is false, attribute is omitted entirely

//- Ternary in interpolation
p Status: #{active ? 'Active' : 'Inactive'}
```

**Tip: Keep Logic Thin** — Move complex conditions into helper functions or compute them in the route handler. Templates should branch, not compute.

---

## Slide 08 — Control Flow — Iteration

### each (Most Common)

```pug
ul
  each user in users
    li #{user.name} — #{user.email}

//- With index
ul
  each item, i in items
    li #{i + 1}. #{item.name}
```

### each ... else (Empty State)

```pug
each product in products
  .product
    h3= product.name
    p= product.price
else
  p.empty No products found.
```

The `else` block runs when the array is empty. No separate `if` check needed.

### Iterating Objects

```pug
//- each val, key in object
each val, key in settings
  p #{key}: #{val}
```

### while Loop

```pug
- var n = 0
ul
  while n < 5
    li= n++
```

### each vs for

`each` and `for` are interchangeable in Pug. Convention favours `each`.

### Nested Iteration

```pug
each category in categories
  h2= category.name
  ul
    each item in category.items
      li= item.title
```

---

## Slide 09 — Mixins

### Defining & Calling

```pug
//- Define a mixin
mixin card(title, text)
  .card
    h3= title
    p= text

//- Call the mixin
+card('Hello', 'World')
+card('Pug', 'Template engine')
```

### Rest Arguments

```pug
mixin list(id, ...items)
  ul(id=id)
    each item in items
      li= item

+list('fruits', 'Apple', 'Banana', 'Cherry')
```

### Passing Blocks

```pug
//- Mixin with block content
mixin panel(title)
  .panel
    .panel-header= title
    .panel-body
      if block
        block

//- Usage: nested content as block
+panel('Settings')
  p Adjust your preferences.
  button Save Changes
```

### Mixin Attributes

```pug
//- attributes object is implicit
mixin btn(label)
  button.btn&attributes(attributes)= label

+btn('Submit')(type='submit', class='primary')
+btn('Cancel')(type='button', class='ghost')
```

The `&attributes` syntax spreads caller attributes onto the mixin's root element.

### Best Practices

- Keep mixins small and focused
- Store shared mixins in a separate file and `include` them
- Use `block` for flexible content injection
- Use `&attributes` for pass-through HTML attributes

---

## Slide 10 — Template Inheritance

### extends & block

```pug
//- views/layout.pug (base template)
doctype html
html(lang="en")
  head
    meta(charset="UTF-8")
    title #{title} | My App
    block styles
      link(rel="stylesheet", href="/css/main.css")
  body
    include partials/nav
    main.container
      block content
    include partials/footer
    block scripts
```

### Child Template

```pug
//- views/home.pug
extends layout

block content
  h1 Welcome Home
  p This replaces the content block.

block scripts
  script(src="/js/home.js")
```

The child **replaces** each named block. Unspecified blocks keep the parent's default content.

### block append & block prepend

```pug
//- Add to a block instead of replacing
extends layout

block append styles
  link(rel="stylesheet", href="/css/about.css")

block prepend scripts
  script(src="/js/vendor/chart.js")

block content
  h1 About Us
```

**append** adds after the parent block. **prepend** adds before. Both preserve the parent's content.

### Multi-Level Inheritance

layout.pug → admin-layout.pug → dashboard.pug. Each level can extend the previous and override or append to blocks.

### Inheritance vs Includes

- **extends/block** — define a page skeleton, override sections
- **include** — insert a reusable fragment (nav, footer)
- Use both together: layout uses includes, pages extend the layout

---

## Slide 11 — Includes

### Basic Include Syntax

```pug
//- Include a Pug file (no extension needed)
include partials/nav

//- Include with a relative path
include ./components/sidebar

//- Include a plain-text file
include:css styles/inline.css
include plaintext.txt
```

Pug includes are resolved relative to the current file. The included file is compiled as Pug by default.

### Reusable Partial Example

```pug
//- partials/card.pug
.card
  if image
    img(src=image, alt=title)
  h3= title
  p.price $#{price.toFixed(2)}
```

Variables from the parent scope are accessible inside the include — no explicit passing needed.

### Include with Filters

```pug
//- Include and process through a filter
include:markdown-it docs/intro.md

//- Include raw HTML
include:html legacy/widget.html
```

### Inheritance vs Includes

| Feature | extends/block | include |
|---------|---------------|---------|
| Purpose | Page skeleton | Reusable fragment |
| Content flow | Child overrides parent | Fragment inserted in place |
| Data access | Shared scope | Shared scope |
| Typical use | Layouts | Nav, footer, widgets |

### Path Resolution

Paths are relative to the including file. Use `basedir` option for absolute paths: `include /partials/nav` resolves from `basedir`.

---

## Slide 12 — Filters

### What Are Filters?

Filters let you embed other languages inside Pug templates. The content is processed by an external module at compile time and the result is inserted into the HTML output.

```pug
:filterName
  Content processed by filterName
```

### :markdown-it

```pug
//- Requires: npm install jstransformer-markdown-it
:markdown-it
  # Hello World
  This is **bold** and _italic_.
  - List item one
  - List item two
```

### :babel (ES6+ in Browser)

```pug
//- Requires: npm install jstransformer-babel
script
  :babel
    const greet = (name) => {
      console.log(`Hello, ${name}!`);
    };
    greet('Pug');
```

### Custom Filters

```javascript
// Register via options
const pug = require('pug');
const html = pug.renderFile('tpl.pug', {
  filters: {
    'upper': (text) => text.toUpperCase(),
    'wrap-div': (text, opts) =>
      `<div class="${opts.class}">${text}</div>`
  }
});
```

### Available Filters

Pug uses the **jstransformer** ecosystem. Install `jstransformer-*` packages: `markdown-it`, `babel`, `scss`, `coffee-script`, `uglify-js`, and many more.

---

## Slide 13 — Express Integration

### Basic Setup

```javascript
const express = require('express');
const path = require('path');
const app = express();

// Set Pug as the view engine
app.set('view engine', 'pug');

// Set the views directory
app.set('views', path.join(__dirname, 'views'));

// Render a template
app.get('/', (req, res) => {
  res.render('index', { title: 'Home' });
});
```

Express calls `pug.__express` under the hood — no extra middleware needed.

### No Layout Plugin Needed

Unlike EJS, Pug has built-in extends/block — no plugin required.

### res.render() in Detail

```javascript
// Signature
res.render(view, [locals], [callback]);

// Examples
res.render('dashboard');                // no data
res.render('dashboard', { title: 'Dashboard', user: req.user });  // with data
res.render('report', data, (err, html) => {
  if (err) return next(err);
  res.send(html);                      // manual send
});
```

### Pug Options via Express

```javascript
// Pretty-print HTML in development
app.locals.pretty = true;

// Set basedir for absolute includes
app.locals.basedir = path.join(__dirname, 'views');

// Enable caching in production
app.set('view cache', true);
```

### File Resolution

Express resolves `res.render('dashboard')` to `views/dashboard.pug` automatically. Subdirectories work too: `res.render('admin/users')`.

---

## Slide 14 — Passing Data to Templates

### res.render() Data Object

```javascript
// Route handler
app.get('/dashboard', async (req, res) => {
  const user = await User.findById(req.user.id);
  const stats = await getStats(user.id);

  res.render('dashboard', {
    title: 'Dashboard',
    user,
    stats,
    isAdmin: user.role === 'admin',
    formatDate: (d) => d.toLocaleDateString('en-GB')
  });
});
```

### res.locals — Per-Request Data

```javascript
// Middleware sets data for all templates
app.use((req, res, next) => {
  res.locals.currentUser = req.user;
  res.locals.flash = req.flash();
  next();
});

// Available in ALL templates for this request
// — no need to pass explicitly in res.render()
```

### app.locals — Global Data

```javascript
// Set once at startup
app.locals.siteName = 'My App';
app.locals.version = '2.1.0';
app.locals.nav = [
  { href: '/', label: 'Home' },
  { href: '/about', label: 'About' },
  { href: '/contact', label: 'Contact' },
];

// Accessible in every template:
// h1= siteName
// span= version
```

### Data Merge Order

Express merges data in this priority (highest wins):

1. `res.render()` data object
2. `res.locals`
3. `app.locals`

This means route-specific data overrides middleware-set data, which overrides global data.

---

## Slide 15 — Security — XSS Prevention

### The Danger: Cross-Site Scripting

```pug
//- VULNERABLE: unescaped user input
div!= userComment

//- SAFE: escaped output (default)
div= userComment
p Welcome, #{userComment}
```

### Rule of Thumb

| Scenario | Syntax |
|----------|--------|
| User-supplied text | `= val` or `#{val}` always |
| Trusted HTML content | `!= val` or `!{val}` |
| CMS / Markdown HTML | `!= val` after sanitising |
| JSON in script tag | `!= JSON.stringify(data)` |

### Sanitisation for Rich Content

```javascript
const createDOMPurify = require('dompurify');
const { JSDOM } = require('jsdom');
const window = new JSDOM('').window;
const DOMPurify = createDOMPurify(window);

// In route handler
const clean = DOMPurify.sanitize(userHtml, {
  ALLOWED_TAGS: ['b','i','p','a','ul','li'],
  ALLOWED_ATTR: ['href']
});
res.render('post', { content: clean });
```

```pug
//- Template — safe after sanitisation
article!= content
```

### Embedding Data in JS Safely

```pug
script.
  // SAFE — JSON.stringify escapes angle brackets in strings
  const data = !{JSON.stringify(data)};
```

Avoids string interpolation XSS in inline scripts.

---

## Slide 16 — Performance & Caching

### How Pug Compiles Templates

`.pug file` → (lex + parse) → `JS function` → (compile, cached) → `HTML string` → (execute)

Pug uses a 3-stage pipeline: **lexer** (tokenise), **parser** (build AST), **code generator** (emit JS function). The compiled function is cached and reused.

### Express View Cache

```javascript
// Auto-enabled in production
app.set('view cache', true);

// Or via NODE_ENV
// NODE_ENV=production node app.js
// Express enables view cache automatically when NODE_ENV === 'production'
```

In dev, templates are re-read and re-compiled on every request (useful for live editing). In production, they compile once.

### Pug Compilation Options

```javascript
const pug = require('pug');

// Compile to a reusable function
const fn = pug.compileFile('tpl.pug', {
  cache: true,       // enable caching
  pretty: false      // minified output
});
const html = fn(data);

// Precompile for client-side use
const clientFn = pug.compileFileClient(
  'tpl.pug',
  { name: 'myTemplate' }
);
// Returns a string of JS code
```

### Performance Tips

- **Always** enable view cache in production
- Keep includes small — each is compiled separately
- Avoid deeply nested inheritance chains (3+ levels)
- Move heavy computation to route handlers, not templates
- Precompile templates at build time for serverless / edge
- Set `pretty: false` in production for smaller output

### Benchmarks (Approximate)

| Engine | Renders/sec |
|--------|-------------|
| Pug (cached) | ~22,000 |
| EJS (cached) | ~28,000 |
| Handlebars (precompiled) | ~35,000 |
| Nunjucks (cached) | ~18,000 |

---

## Slide 17 — Pug vs Other Template Engines

| Feature | Pug | EJS | Handlebars | Nunjucks |
|---------|-----|-----|------------|----------|
| Syntax | Indentation-based | HTML + JS tags | Mustache `{{ }}` | Jinja2-style `{% %}` |
| Learning Curve | Moderate | **Minimal** | Low | Low-Moderate |
| JS in Templates | Full JS access | Full JS access | **No** (logic-less) | Limited expressions |
| Layouts | **Built-in** extends/block | Via plugin/includes | Via partials | **Built-in** extends |
| Mixins | **Built-in** | No (use functions) | Partials + helpers | Macros |
| Auto-Escape | Default escaped | `<%= %>` escapes | Default escaped | Default escaped |
| npm Weekly DLs | ~1.5M | **~4.7M** | ~3.2M | ~0.9M |
| Best For | Clean markup fans | JS devs, rapid dev | Logic separation | Python devs, i18n |

**When to choose Pug:** You want concise, indentation-based templates with built-in inheritance, mixins, and filters. **When not to choose Pug:** Your team prefers plain HTML, you need minimal learning curve, or IDE support is a priority.

---

## Slide 18 — Real-World Express + Pug Project Structure

### Directory Layout

```
my-app/
├── app.js
├── package.json
├── .env
├── config/
│   └── db.js
├── routes/
│   ├── index.js
│   ├── auth.js
│   └── products.js
├── controllers/
│   ├── authController.js
│   └── productController.js
├── models/
│   ├── User.js
│   └── Product.js
├── middleware/
│   ├── auth.js
│   └── validate.js
├── views/
│   ├── layout.pug
│   ├── mixins/
│   │   ├── card.pug
│   │   └── form-field.pug
│   ├── partials/
│   │   ├── nav.pug
│   │   ├── flash.pug
│   │   └── footer.pug
│   ├── pages/
│   │   ├── home.pug
│   │   ├── about.pug
│   │   └── contact.pug
│   ├── auth/
│   │   ├── login.pug
│   │   └── register.pug
│   ├── products/
│   │   ├── index.pug
│   │   ├── show.pug
│   │   └── edit.pug
│   └── error.pug
└── public/
    ├── css/
    │   └── style.css
    ├── js/
    │   └── main.js
    └── images/
```

### Key Principles

- **views/layout.pug** — base template with extends/block
- **views/mixins/** — reusable components (card, form-field)
- **views/partials/** — included fragments (nav, footer, flash)
- **views/pages/** — static-ish content pages
- **views/{resource}/** — CRUD views per model
- **public/** — static assets served by Express

### app.js Setup

```javascript
const express = require('express');
const path = require('path');

const app = express();

app.set('view engine', 'pug');
app.set('views', path.join(__dirname, 'views'));

// No layout plugin needed — Pug has
// built-in extends/block inheritance

app.locals.siteName = 'My App';
app.locals.basedir = path.join(__dirname, 'views');

app.use(express.static(path.join(__dirname, 'public')));
app.use(express.urlencoded({ extended: true }));
```

### Naming Conventions

Use `index.pug` for list views, `show.pug` for detail, `edit.pug` / `new.pug` for forms. Store shared mixins in `mixins/` and include them where needed.

---

## Slide 19 — Summary & Next Steps

### Core Takeaways

- Pug = indentation-based templates, no closing tags
- `=` / `#{}` for escaped, `!=` / `!{}` for raw
- Built-in inheritance with `extends` and `block`
- Mixins for reusable, parameterised components
- Auto-escaping prevents XSS by default

### Best Practices

- Always use `=` / `#{}` for user data
- Keep templates thin — compute in handlers
- Use mixins for repeated markup patterns
- Use `extends/block` for layouts, `include` for fragments
- Enable view cache in production
- Set `basedir` for absolute include paths

### Next Steps

- Build a CRUD app with Express + Pug
- Add authentication (Passport.js + Pug forms)
- Create a mixin library for your UI components
- Integrate flash messages (`connect-flash`)
- Explore filters for Markdown / SCSS embedding
- Try HTMX + Pug for dynamic partial updates

### Essential Resources

- **Official Docs:** pugjs.org
- **GitHub:** github.com/pugjs/pug
- **npm:** npmjs.com/package/pug
- **Express Guide:** expressjs.com/en/guide/using-template-engines.html

### Key Packages

| Package | Purpose |
|---------|---------|
| `pug` | Template engine |
| `pug-cli` | Command-line compiler |
| `jstransformer-markdown-it` | Markdown filter |
| `dompurify` + `jsdom` | HTML sanitisation |
| `express-validator` | Form validation |
