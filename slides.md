---
# You can also start simply with 'default'
theme: default
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: ./QTbackground3.png
# some information about your slides (markdown enabled)
title: Webpage script testing
# apply unocss classes to the current slide
class: text-center
# https://sli.dev/custom/highlighters.html
highlighter: shiki
# https://sli.dev/guide/drawing
drawings:
  persist: false
# slide transition: https://sli.dev/guide/animations#slide-transitions
transition: slide-left
# enable MDC Syntax: https://sli.dev/guide/syntax#mdc-syntax
mdc: true
---

# Webpage script testing

by Arindam Kalita

<!--
Use branch 'demo-qt' for demo
-->

---
layout: center
align: left
---


## Background

* Often marketing and analytics teams implement different scripts and tags in websites which runs in the background

<br>
<br>

### Example:
* A canonical tag is a tag in the source code of a page that indicates to search engines that a master copy of the page exists. 
* Canonical tags are used in SEO to help search engines index the correct URL and avoid duplicate content.

<br>
<br>

_Example of wikipedia:_
```html
<link rel="canonical" href="https://en.wikipedia.org/wiki/Main_Page">
```

---
layout: center
align: left
---
### Step 1:

Go to home URL. Open Dev tools and go to console.

---
layout: center
align: left
transition: slide-up
level: 2
---

### Step 2:

Run the following javascript code in console:

> The goal of the code is to extract information from `<a>` tags (links) in a webpage, filter out duplicates based on URL, and generate a CSV output where each row represents a link that is not an external link (`External Link = FALSE`).

---


```js {all|1-2|4-13|15-23|25|all} twoslash
const results = [['URL', 'Anchor Text', 'External Link']];
const seenUrls = new Set(); // Set to keep track of unique URLs
// Processing Each `<a>` Tag
const urls = document.getElementsByTagName('a');
for (let urlIndex = 0; urlIndex < urls.length; urlIndex++) {
    const url = urls[urlIndex];
    const externalLink = url.host !== window.location.host;

    if (url.href && url.href.indexOf('://') !== -1 && !seenUrls.has(url.href) && !externalLink) {
        seenUrls.add(url.href); // Add URL to the set
        results.push([url.href, url.innerText.trim(), 'FALSE']);
    }
}
// CSV Content Generation
const csvContent = results.map((line) => {
    return line.map((cell) => {
        if (typeof(cell) === 'boolean') {return cell ? 'TRUE' : 'FALSE';}
        if (!cell) {return '';}
        let value = cell.replace(/[\f\n\v]*\n\s*/g, "\n").replace(/[\t\f ]+/g, ' ');
        value = value.replace(/\t/g, ' ').trim();
        return `"${value}"`;
    }).join('\t');
}).join("\n");
// Logging CSV Content
console.log(csvContent)
```


---
layout: center
align: left
---

### Step 3:
Copy the CSV output in an Excel or feature file. 
<!-- * Remove duplicates by: Go to the Data tab > Click the Remove Duplicates button.
* Remove external links -->

---
layout: center
align: left
---

### Step 4:

Implement in cucumber:

```gherkin
Feature: Canonical testing

Scenario Outline: Verify canonical tagging in Wikipedia
Given User clicks on URL <URL>
Then verify that canonical tagging is present in page

Examples:
    |URL|Anchor Text|External|
    |https://en.wikipedia.org/wiki/Main_Page|Wikipedia|FALSE|
```

---
layout: center
align: left
---

### Thank you!