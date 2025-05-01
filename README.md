# 🔍 HackerOne Scope Domain Extractor

A simple, browser-based JavaScript tool to extract **all scoped domains** from any HackerOne program page — with just a few clicks in your browser's Developer Console!

![Preview](https://img.shields.io/badge/Bug%20Bounty-HackerOne-blueviolet?style=flat-square)
![JavaScript](https://img.shields.io/badge/Language-JavaScript-yellow?style=flat-square)
![Browser](https://img.shields.io/badge/Run%20in-Browser%20Console-green?style=flat-square)

---

## 💡 Features

- ✅ Extracts all scoped domains (`title` attributes) from a HackerOne program page.
- ✅ Works on **paginated pages** — just scroll to the bottom to load all items before running.
- ✅ Automatically downloads `domains.txt` with all collected domains.
- ✅ Runs directly in your browser — **no installation needed**.
- ✅ Clean and simple — perfect for recon or bug bounty reports.

---

## ⚙️ How to Use

1. Open any [HackerOne](https://hackerone.com) program page (e.g., [`https://hackerone.com/bournvita.ng`](https://hackerone.com/bournvita.ng))
2. Scroll down to **load all in-scope domains**
3. Right-click → `Inspect` → go to the `Console` tab
4. Paste the following script and press `Enter`:

```javascript
(() => {
  const scopeElements = [...document.querySelectorAll('[title]')];
  const domains = scopeElements
    .map(el => el.getAttribute('title'))
    .filter(url => url.includes('.'));

  const uniqueDomains = [...new Set(domains)];

  const blob = new Blob([uniqueDomains.join('\n')], { type: 'text/plain' });
  const link = document.createElement('a');
  link.href = URL.createObjectURL(blob);
  link.download = 'domains.txt';
  link.click();

  console.log(`[+] Extracted ${uniqueDomains.length} domains`);
})();
