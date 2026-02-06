# MG's Snippets

## tools

check your ip

[https://checkip.amazonaws.com/](https://checkip.amazonaws.com/)


## Python

```python title="read csv to row"
import csv
with open('eggs.csv', newline='') as csvfile:
    spamreader = csv.reader(csvfile)
    for row in spamreader:
        print(', '.join(row))
```

```python title="read csv to row dict"
with open('names.csv', newline='') as csvfile:
    reader = csv.DictReader(csvfile)
    for row in reader:
        print(row['first_name'], row['last_name'])
```

```python title="write csv"
import csv
with open('eggs.csv', 'w', newline='') as csvfile:
    spamwriter = csv.writer(csvfile)
    spamwriter.writerow(['Spam'] * 5 + ['Baked Beans'])
    spamwriter.writerow(['Spam', 'Lovely Spam', 'Wonderful Spam'])
```

```python title="write csv dict"
import csv
with open('eggs.csv', 'w', newline='') as csvfile:
    spamwriter = csv.DictWriter(csvfile, fieldnames=['foo', 'bar', 'value'])
    spamwriter.writeheader()
    spamwriter.writerow({
      'foo': 'x',
      'bar': 'y',
      'value': 'abc'
    })
```

Python app with csv in/out file

```python
import csv
import argparse
import sys

def main(args):
    print("--- Script starting ---")
    data = {}
    fieldnames = [args.field]
    reader = csv.DictReader(args.infile)
    stats = {
        'empty': 0,
        'ignore': 0,
        'total': 0,
    }

    if args.verbose:
        print(f'stats: {stats}')

    print("--- Script finished ---")
    return 0


if __name__ == '__main__':
    parser = argparse.ArgumentParser(description='raw parser')
    parser.add_argument(
        'infile',
        type=argparse.FileType('r'),
        default=sys.stdin)

    parser.add_argument(
        'field',
        type=str,
        help='field name'
    )
    parser.add_argument(
        '--relate',
        nargs='?',
        type=str,
        help='related field name'
    )
    parser.add_argument(
        '--out',
        nargs='?',
        type=argparse.FileType('w')
    )

    parser.add_argument(
        "-v", "--verbose",
        action="store_true",  # This makes it a flag
        help="Enable verbose output."
    )

    parsed_args = parser.parse_args()

    sys.exit(main(parsed_args))
```
## JavaScript

```javascript title="IIFE (Immediately Invoked Function Expression)"
(function() {
  'use strict';
  // ...
})();
```
[IIFE - MDN Web Docs Glossary: Definitions of Web-related terms | MDN](https://developer.mozilla.org/en-US/docs/Glossary/IIFE)

```javascript title="iterate object entries"
for (const [key, value] of Object.entries(object1)) {
  console.log(`${key}: ${value}`);
}
```

```javascript title="fetch many urls and wait all promises"
  const fetchData = async (url) => {
    try {
      let response = await fetch(url);
      return await response.json();
    } catch(error) {
      console.error(`fetch error) ${error} | ${url}`);
      return error.message;
    }
  };

  const fetchURLs = async (urls) => {
    return await Promise.all(urls.map( async ([key, url]) => {
      let res = await fetchData(url);
      return [key, res];
    }));
  };

  fetchURLs(urls)
    .then(data => {
      console.log('Fetched data:', data)})
    .catch(error => {
      console.error('Error fetching data:', error)
    });
```
## HTML

```html title="usual layout structure"
<html>
  <head>
  </head>
  <body>
    <main>
      <header> </header>
      <section> </section>
      <section> </section>
      <section> </section>
      <footer> </footer>
    </main>
  </body>
</html>
```

```html title="link no blank tab"
target="_blank" rel="noreferrer noopener
```

## Document Templates

MkDocs Blog

```markdown
---
date: 2026-01-15
categories:
  - Journal
tags:
  - tag1
  - tag2
---

# Header1

![foo](../../assets/blog/bar)

<!-- more -->

```

Make code block wrap

```html
<style>
pre code {
  white-space: pre-wrap !important;
  word-wrap: break-word !important;
}
</style>
```

reST (ReStructuredText)

```text
###
Parts
###

***
Chapters (filename)
***

Section
===
or 
# SECTION (all Upper case)

Subsection
-----------
or 
## Subsection (capitalize)
```


## Bookmarklet

```javascript title="markdown link"
javascript:(function()%7Bt='%5B'+decodeURIComponent(document.title)+'%5D('+decodeURIComponent(window.location.href)+')';win=window.open('','_new','location=no,links=no,scrollbars=no,toolbar=no,width=550,height=150');win.document.write('%3Cform%3E%3Ctextarea%20name=%22a%22%20rows=%225%22%20cols=%2250%22%20onClick=%22javascript:this.form.a.focus();this.form.a.select();%22%3E'+t+'%3C/textarea%3E%3C/form%3E');%7D)()
```

```javascript title="markdown link (strip facebook ads)"
javascript:(function()%7Bt='%5B'+decodeURIComponent(document.title)+'%5D('+decodeURIComponent(window.location.href.split('?fbclid=')[0])+')';win=window.open('','_new','location=no,links=no,scrollbars=no,toolbar=no,width=550,height=150');win.document.write('%3Cform%3E%3Ctextarea%20name=%22a%22%20rows=%225%22%20cols=%2250%22%20onClick=%22javascript:this.form.a.focus();this.form.a.select();%22%3E'+t+'%3C/textarea%3E%3C/form%3E');%7D)()
```

```javascript title="open textarea"
javascript:(function()%7Bwin=window.open('','_new','location=no,links=no,scrollbars=no,toolbar=no,width=800,height=600');win.document.write('%3Cform%3E%3Ctextarea%20rows=%2230%22%20cols=%2280%22%3E%3C/textarea%3E%3C/form%3E');%7D)()
```
