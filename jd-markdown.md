# jd's markdown reference

This file was based primarily on <https://markdownguide.org>, with reference to...
 - John Gruber’s spec: <https://daringfireball.net/projects/markdown/syntax>
 - The CommonMark precisification: <https://spec.commonmark.org/0.30/>
 - Basic syntax guide: <https://www.markdownguide.org/basic-syntax>
 - Extended syntax guide: <https://www.markdownguide.org/extended-syntax>
 - GitHub Flavored Markdown (GFM), based on CommonMark: <https://github.github.com/gfm/>

(In VSCode, shift-command-V will render)

# Basic Syntax

The elements outlined in John Gruber’s original design. All Markdown
applications should support these elements.

# H1

H1 alternate syntax
===================

## H2

H2 alternate syntax
-------------------

### H3

#### H4

##### H5

###### H6

---

### Basic formatting

*italicized text*

**bold text**

> blockquote
>
> text
>> with a nested
>> blockquote

`code, and you can use `` in your string`

``also code, and you can use ` in your string``

    codeblock, if we are indented by at least
    four spaces -- see fenced codeblocks in
    extended syntax for an alternate syntax

### Links

[Links with alt text](https://www.markdownguide.org "optional tool tip")

Autolink syntax with <>: <https://www.markdownguide.org>, <fake@example.com>

Links with reference-style syntax, to keep text more human-readable:  
[text for the link][1]

Then, anywhere else in the document, define that reference. The following line
won't be rendered, even though it's nicely human readable as text

[1]: https://www.markdownguide.org "optional tool tip"

### Images

Same as Link syntax, just with a leading !:
![Image with alt text](https://jeremydolan.net/media/Markdown.png "tool tip")

### Linked Images

[ to start the link, then ![ to start the image gives us [![ an exclam sandwich
[![An old rock in the desert](https://www.markdownguide.org/assets/images/shiprock.jpg "Shiprock, New Mexico by Beau Rogers")][label]

[label]: https://www.flickr.com/photos/beaurogers/31833779864/in/photolist-Qv3rFw-34mt9F-a9Cmfy-5Ha3Zi-9msKdv-o3hgjr-hWpUte-4WMsJ1-KUQ8N-deshUb-vssBD-6CQci6-8AFCiD-zsJWT-nNfsgB-dPDwZJ-bn9JGn-5HtSXY-6CUhAL-a4UTXB-ugPum-KUPSo-fBLNm-6CUmpy-4WMsc9-8a7D3T-83KJev-6CQ2bK-nNusHJ-a78rQH-nw3NvT-7aq2qf-8wwBso-3nNceh-ugSKP-4mh4kh-bbeeqH-a7biME-q3PtTf-brFpgb-cg38zw-bXMZc-nJPELD-f58Lmo-bXMYG-bz8AAi-bxNtNT-bXMYi-bXMY6-bXMYv "using a link label to isolate out this long link"

### Ordered Lists

1. First item
2. Second item
1. Third item (any number will do!)
1\. escape . to start lines with [number][period]

### Unordered Lists

- with a dash (-)
- with an asterisk (*)
- with a plus sign (+)
    - nest lists by indenting
    \- escape - to start lines with -

### Horizontal Rule

Three or more asterisks (***), dashes (---), or underscores (___)

---

### Line breaks

Use two or more spaces  
at the end of a line for a line break    
like this



# Extended Syntax

Not all Markdown applications support these elements.

~~struck~~ text


### Table

Three or more hyphens to create each column’s header. Optionally, colons in the
hyphen sequence specify alignment for the column. This can help:
<https://www.tablesgenerator.com/markdown_tables>

| Syntax      | Description | Test Text     |
| :---        |    :----:   |          ---: |
| Header      | Title       | Here's this   |
| Paragraph   | Text        | And more      |

### Fenced Code Block with optional syntax highlighting

```JSON
{
  "firstName": "John",
  "lastName": "Smith",
  "age": 25
}
```

```python
# python syntax detection seems mediocre
if self.regions.get(i):
    region_length = 3 + len(max(self.regions.values(), key=len, default=''))
```

### Footnote

Here's a sentence with a footnote. [^1]

[^1]: This isn't supported by VSCode.

### Heading_ID / anchor links

Many Markdown viewers automatically generate anchor names at headings,
with the header name transformed into an anchor name with something like
tolower() + sub(' ', '-') + sub('/', '') and probably some other stuff. VSCode
at least auto-completes the name to let you find it. I think I saw an algorithm 
in one of the references but can't for the life of me find it now.

Jumping to anchors at headers:  
- [Back to Tables section](#table)
- [or to the top of this section](#heading_id--anchor-links)

### Definition List

Not supported in VS Code:

term
: de`fi`nition

### Task List

Not supported in VS Code:

- [x] Write the press release
- [ ] Update the website
- [ ] Contact the media

### Emoji

Not supported in VS Code: :joy:

### Highlight

Not supported in VS Code: ==very important words==

### Subscript

Not supported in VS Code: H~2~O

### Superscript

Not supported in VS Code: X^2^
