# jd's markdown reference

This file was based primarily on <https://markdownguide.org>, with reference
to...
 - John Gruber’s spec: <https://daringfireball.net/projects/markdown/syntax>
 - The CommonMark precisification: <https://spec.commonmark.org/0.30/>
 - Basic syntax guide: <https://www.markdownguide.org/basic-syntax>
 - Extended syntax guide: <https://www.markdownguide.org/extended-syntax>
 - GitHub Flavored Markdown (GFM), based on CommonMark:
   <https://github.github.com/gfm/>

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

*\*italicized text\**

**\*\*bold text\*\***

> blockquoted text
> prefaced with >
>> with a nested
>> blockquote
>>> we can keep going


inline `code with one backtick, and you can use `` in your` string

also inline ``code with double backtick, but you can use ` in your`` string

    codeblock, by uniformly indenting at least
    four spaces -- see also fenced codeblocks in
    extended syntax for an alternative

---

### Links

1. Inline [link](https://www.example.com "optional tool tip") syntax:
`[text](url)`

2. 'Autolinks' when your link name is just the url itself, with `<url>`:
<http://example.com>, <user@example.com>

3. Reference-style [links][1] will keep the source text more human-readable.
The in-line markup is simply:
> `[text][<ref_name>]`

Then, anywhere else in the document, define that reference:  
> `[<ref_name>]: url "tool tip"`

That line won't be rendered in a Markdown viewer, but will look clean when
viewing the source text.

[1]: https://www.markdownguide.org "optional tool tip"

---

### Images

Similar to link syntax, but with a leading !: `![alt text](url.png "tool tip")`
![Image with alt text](https://jeremydolan.net/media/Markdown.png "tool tip")

---

### Linked Images

[ to start the link then ![ to start the image gives us [![ an exclam sandwich:  
`[![alt text if image doesn't load](image URL "image tool tip")](link url)`  
Or see source below for an example using a reference-style link.
[![An old rock in the desert](
    https://www.markdownguide.org/assets/images/shiprock.jpg
    "Shiprock, New Mexico by Beau Rogers") extra text][link2]

[link2]: https://www.flickr.com/photos/beaurogers/31833779864/
   "using a link label to isolate out this long link"

---

### Ordered Lists

1. First item
2. Second item
1. Third item (any number will do!)
1\. escape the . to *prevent* recognition of text as a list item

---

### Unordered Lists

- with a dash (-)
- with an asterisk (*)
- with a plus sign (+)
    - nest lists by indenting
    \- escape the - to start lines with -

---

### Horizontal Rule

Three or more asterisks (***), dashes (---), or underscores (___)

***

### Line breaks

Two or more spaces at the end of a line yield a line break.  
It is  
mildly  
frowned upon

---

# Extended Syntax

DANGER! Not all Markdown applications support these elements.

### Struck text

~~struck text~~ - Both VSCode and GitHub render text bracketed by \~\~ as
struck. (GitHub also strikes text backeted by single tildes.)

✅ GitHub  
✅ VSCode

---

### Tables and column alignment

Three or more hyphens to create each column’s header. Optionally, colons in the
hyphen sequence specify alignment for the column. This can help:
<https://www.tablesgenerator.com/markdown_tables>

| Syntax      | Description | Test Text     |
| :---        |    :----:   |          ---: |
| Header      | Title       | Here's this   |
| Paragraph   | Text        | And more      |

✅ GitHub  
✅ VSCode

---

### Fenced code block, with syntax highlighting

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

✅ GitHub  
✅ VSCode

---

### Footnote

Here's a sentence with a footnote.[^1]  
✅ GitHub  
❌ VSCode

[^1]: This isn't supported by VSCode.

---

### Heading_ID / anchor links

Many Markdown viewers automatically generate anchor names at headings,
with the header name transformed into an anchor name with something like
tolower() + sub(' ', '-') + sub('/', '') and probably some other stuff. VSCode
at least auto-completes the name to let you find it. I think I saw an algorithm 
in one of the references but can't for the life of me find it now. TODO.

Jumping to header anchors:
- [Back to Tables section](#tables-and-column-alignment)
- [or to the top of this section](#heading_id--anchor-links)

✅ GitHub  
✅ VSCode

---

### Task List

- [x] Do the thing
- [ ] ???
- [ ] PROFIT!

✅ GitHub  
❌ VSCode

---

### Emojis by name

:joy: :see_no_evil: 

✅ GitHub  
❌ VSCode

---

### Subscripts and Superscripts

Both GitHub and VSCode Markdown views will render the HTML tags sub
(H<sub>2</sub>O) and sup (X<sup>2</sup>).