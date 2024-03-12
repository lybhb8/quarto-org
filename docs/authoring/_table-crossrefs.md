

对于 markdown 表格，请在表格下方添加标题，然后在标题末尾用大括号包含一个 `#tbl-` 标签。例如：

``` markdown
| Col1 | Col2 | Col3 |
|------|------|------|
| A    | B    | C    |
| E    | F    | G    |
| A    | G    | G    |

: My Caption {#tbl-letters}

See @tbl-letters.
```

渲染成 HTML 是这样的：

![](images/crossref-table.png){fig-alt="A table with 3 columns and four rows. The text 'Table 1: My Caption' is above the header column. The text 'See tbl. 1' is aligned to the left underneath the last column." width="500"}
