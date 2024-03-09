您可能希望创建一个由多个子表格组成的组合。为此，请创建一个带有主标识符的 div，然后将子标识符（以及可选的标题文本）应用到所包含的表格中。例如

``` markdown
::: {#tbl-panel layout-ncol=2}
| Col1 | Col2 | Col3 |
|------|------|------|
| A    | B    | C    |
| E    | F    | G    |
| A    | G    | G    |

: First Table {#tbl-first}

| Col1 | Col2 | Col3 |
|------|------|------|
| A    | B    | C    |
| E    | F    | G    |
| A    | G    | G    |

: Second Table {#tbl-second}

Main Caption
:::

See @tbl-panel for details, especially @tbl-second.
```

渲染成 HTML 时看起来是这样的：

![](/docs/authoring/images/crossref-subtable.png){fig-alt="Two tables side-by-side. Both tables have 3 columns and 4 rows. The table on the left is titled '(a) First table'. The table on the right is titled '(b) Second Table'. Centered underneath both tables is the text 'Table 1: Main Caption'. The text 'See tbl. 2 for details, especially tbl. 2 (b)' is aligned to the left underneath that."}

请注意，表格的 "主标题 "是作为包含 div 的最后一个块提供的。
