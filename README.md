# [textarea.my](https://textarea.my)

A _minimalist_ text editor that lives entirely in your browser and stores everything in the URL hash.

<p align="center">
  <a href="https://textarea.my/#VZA9TgMxEIV7n2KkdJZIejoURIGgYaGgnGSHeITXXtmzG0zFAZDSREoTlIZDIHGbXIAcAXsTClrP-_meRyD0IhgIx01S6gJ0w44btBxFDzegmsUHEIMClnuKQE44kE3ADpLvAsyCX0YKgK6GmMVF01NIYtgtikoMwcPdDRiMZqyUEWnj-WTyr3s0gitC6bJbqTM47Dbbn68VaD31TZsfI3unNezf1vBYSge4BUmE-UlANSxZDNT0ZFFoCFlvcsJf9clemVx6JHdeMuws5Yw2FVgsnNm5374fdqvvbK4kWTo5p12e1_ArDYus98_HxmlVQc8Il9Tfe2_zAKVvsabjdf_xWZaU37nGHqt54FZ01vwC">  
    <img src=".github/textarea-my-screenshot.webp" alt="Screenshot of textarea.my" width="550" height="386">
  </a>
</p>

## Features

- 🗜️ **Compression** – Your text gets compressed with deflate
- 🔗 **URL hash** – Share your notes by copying a URL
- 💕 **Style** – Customize the look with CSS via DevTools

## How It Works

### Core Architecture

**ContentEditable Element**
- Uses an `<article contenteditable="plaintext-only">` element
- User types directly into this element
- `plaintext-only` prevents rich HTML formatting from paste operations

**Live Syntax Highlighting**
- After each keystroke (debounced by 30ms), the editor:
  1. Saves cursor position
  2. Extracts plain text content
  3. Parses it with regex matchers to find markdown patterns
  4. Replaces content with styled HTML fragments
  5. Restores cursor position

**Supported Markdown Syntax**
- Headers: `# Title`, `## Title`, etc.
- Bold: `**text**` or `__text__`
- Italic: `*text*` or `_text_`
- Strikethrough: `~~text~~`
- Inline code: `` `code` ``
- Code blocks: ` ```code``` `
- URLs: Auto-detected HTTP(S) links

**Custom Editor**
- Manages contenteditable behavior with cursor preservation
- Custom undo/redo with history stack (up to 10,000 entries)
- Complex logic to maintain cursor position through DOM transformations

**Data Storage**
- Stores plain text (not HTML) in URL hash
- Uses deflate compression via `CompressionStream` API
- Encodes to base64url for URL-safe sharing
- Also saves to localStorage as backup
- Format: `#<compressed-data>` optionally with `\x00<style>` for custom CSS

**Key Features**
- Real-time rendering: Type markdown, see formatted output
- URL-based storage: Entire document lives in URL hash
- No server needed: Everything runs client-side
- Shareable: Copy URL to share document

**Current Limitations**
- No lists or tables: Regex patterns don't recognize `- item` or `| table |` syntax
- Regex-based parsing: Not a full markdown parser, edge cases may not work
- Complex cursor management: Save/restore logic can be fragile with pattern conflicts

## Pro tips

- Start your document with `# Title` to set a custom page title
- Your data lives in localStorage AND the URL. Double the fun!
- Add a `style` attribute to the `<article>` tag via DevTools. It'll be saved in the URL too!
- Add [`/qr`](https://textarea.my/qr#c0_NSy1KLElVSFQIDFJIzk9JVUjLL1KozC8tUsjLL0ktVgQA) to get a QR code for the current page

## Examples

<!-- - [Crime and Punishment (by Fyodor Dostoevsky)](https://medv.io/goto/crime-and-punishment-by-fyodor-dostoevsky.html) -->
- [A Markdown Example](https://textarea.my/#Xc9NT8JAEAbg-_6KV7hAQyB86AFCjDdMiAej9y7dobuy7NTdqWKM_e2mhVOP88z3EDvSxoUSe_oij7lSw2HPFq31cdlhX1dX7fP9jfv-oNQLx7P2qHTUZdSVhdBFcKAjR4IL3gXCsa0RcaGcKvVmXULHBQfRLiRk2YG96TqzbILMifauuMYT6GDQNEmiO5HYyHV5XdI00N5DuCSxFKdKPYfbXEOgiz5XntbICw5JcMEWq8UmV-oJ7697JMu1NzgQDAkVQqZbJHUMZOCCMHR75mkNK1Kl9Wx2Gzkt-DyrtNjHz5riz3a-WA6tTlapPM8_kjrWoRDHAZa859EYvwrtr4k9TT2Xo8GuTUzwzdGbu8F4o_7a1n8=)
- [An Ode to Comic Sans](https://textarea.my/#TVM9j9w2EE3NX_FwaRJAtwe4SHGu7g5I4MII4HNgpByRI4lZiqPMjFbeVPsj0hiI_9z-koCyDV9FznA-33v8EQ8VvyeGC55kzhHPVC2E73eQaj6xYcs-yeogO-Y6duEBo-aKXOHnRUalZcoRg-jchedJuFBkw1o9c-pghblVUSmFE9alC09KNuU6widGT_XvlR0ywFjzAKoJVXQ-hPAoWlFlf5xJ-8KYqBSDKApt6DXzYF14XB1JJBW2DrYwxwn92ve7TZioptuktFUshakLb7xF0dH2JSbGSXLk1qUZsZCZisxYxJz1Plwv_z0yjrmm7nr5jGbSkWFU-JvjVQMAgzIfrpfPIbyf-Ds8rAaLMgwdKP21mrc-WXFkrTukT2X1OOE3UpqlJpR8ZCxMWgzkSLTVQ_iT_QVV2OjU1lsr9WQTpy48Tcw6rKWcsanUsYP52veitZwh9RDa4ptmZ0Of1adEZ0TSZDvoRcxvk4wYypnVWr1V44TKmxV2bz5YHqvBaeGEqCJHTk1CSUTt0OonYds526ZsCysWZfM8ciNtkY31evl3Z6CJyroG3jsu9PGAD3y9fDoxjLliE7WvWL6VeGxSmnluBztrpbZlX2jmFH4VRdQ8syE7Kp9YQUUqI8o8Z_cGzhtHz6S2U7xxHqddVtM6U4WTOYcP2ScQbM6F4RM5ItXr5VPLBKfsnA4hPNQE81xKa7awWjb_KrNlOYNizImrd-GPGkVKY6gXn1jb7GtVHnJtA73jOdf07RtsoiV1TY8qa03ceqgc2brwvk3SEKVSWlwyVP4C-8akIHjmww8BAHqKx3GvcI9Vy093dzOn0yHLHZmx2505qR3GPPz8ek8YpPrtQHMu53vcvJDX2-eb7qXjpkNc1fKJXyRa_ofv8eqX5eMXZ5Qiet-od34d_gc=)
