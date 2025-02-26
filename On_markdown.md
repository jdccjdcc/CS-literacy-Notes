# On markdown syntax

links: 
[md tutorial](https://www.jetbrains.com/help/hub/markdown-syntax.html#quick-notes-markdown-quotes)

forenote: to view the rendered Markdown, open Markdown Preview:
> Ctrl + Shift + V (Windows/Linux) or Cmd + Shift + V (macOS). 
---
---
### Hide text 
<details>
<summary>clica aqui</summary>

1. hidingfoo
2. hidingbar

</details>

### Potpourri
- #### Preformatted Text with HTML `<pre>` Tags
Using HTML `<pre>` tags ensures that both spaces and line breaks are preserved exactly as you type them.
<pre>
This    is    text    with    spaces.
</pre>

### Headings
 + use `#` before the title. One to six cardinals (decreasing order of size)

### Paragraphs and line breaks

- To start a new **paragraph**, leave a **blank line** between lines of text.
- To start a **new line** inside a paragraph, enter **two trailing spaces** at the end of the line of text.

The **underscores** 
___
----
****

### Block quote
> Use quote blocks to emulate reply text.
> This line is part of the same quote.

This line is not formatted and does not belong to the quote block.

> This block spans multiple paragraphs.
>
> The second paragraph is grouped with the previous paragraph in the same quote block.
> Character formatting is _also_ supported inside the **quote block**.

> Quote blocks can also be nested.
>> When you start a new line with additional > characters,
>>> it simulates a threaded conversation.

### Code block

```python
print("Hello")
```

### Lists

To create an unordered list, start the line with a dash (-), asterisk (*), or plus sign (+).

To nest an unordered list inside an unordered or ordered list, indent the line with two spaces.

1. a
2. b
- foo
  - bar
    - foo
### Tables

 Month    | Assignee | Backup |
| -------- | -------- | ------ |
| January  | Dave     | Steve  |
| February | Gregg    | Karen  |
| March    | Diane    | Jorge  |

Here's the same text with character formatting.
+ The text in the first column is flush right.
+ The text in the second column is centered.

Month | Assignee | Backup
---:|:---:| ---
**January** | Dave | _Steve_
**February** | Gregg | _Karen_
**March** | Diane | _Jorge_

### Images

Inline:
![Markdown logo](https://upload.wikimedia.org/wikipedia/commons/thumb/4/48/Markdown-mark.svg/208px-Markdown-mark.svg.png "Markdown")

Reference style:
![Markdown logo][logo]

[logo]: https://upload.wikimedia.org/wikipedia/commons/thumb/4/48/Markdown-mark.svg/208px-Markdown-mark.svg.png "Markdown"
---
+ The video ID is the unique identifier that comes after v= in a YouTube URL
+ YouTube provides a few standard thumbnail sizes. You can replace VIDEO_ID with the actual video ID in these URLs:  
    1. default thumbnail(120*90 pixels):
    ```https://img.youtube.com/vi/VIDEO_ID/default.jpg```
    2. standard (640*480 pixels):
    ```https://img.youtube.com/vi/VIDEO_ID/sddefault.jpg```  
    3. etc.

[![Youtube - Joao Cesar Monteiro chillin](https://img.youtube.com/vi/cEsSPAxDPHY/sddefault.jpg)](https://www.youtube.com/watch?v=cEsSPAxDPHY)

### Backlash Escapes

- When you have characters that are parsed as Markdown that you want to show as written, you can escape the character with the backslash (\).  
- Backslashes before non-markup characters are shown as backslash characters.
- Escaped characters are treated as regular characters. Their usual meaning in Markdown syntax is ignored.
- Backslash escapes do not work in fenced code blocks, inline code spans, or autolinks.

Here are a few examples of backslash escapes:

- \*not emphasis*
- \`not an inline code span`
- 1\. not an ordered list
- \* not an unordered list
- \# not a heading

