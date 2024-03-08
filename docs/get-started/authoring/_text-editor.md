## 概述

在本教程中，我们将探索 Quarto 的更多编写功能。我们将介绍以多种格式呈现文档的方法，并演示如何添加目录、方程式、引文、交叉引用等组件。


## 输出格式


Quarto 支持将笔记本渲染为数十种不同的[output formats输出格式]（/docs/output-formats/all-formats.qmd "输出格式"）。默认情况下使用 `html` 格式，但你也可以在文档选项中指定另一种（或多种）格式。

### 格式选择

让我们创建一个新文件（"authoring.qmd"），并定义要呈现的各种格式，为每种格式添加一些选项。需要提醒的是，文件选项是在源文件的开头以 YAML 格式指定的。

```yaml
---
title: "Quarto Document"
author: "Norah Jones"
format: pdf
---
```

我们指定 `pdf` 为默认输出格式（如果不使用 `format ` 选项，则默认为 `html`）。

让我们添加一些选项来控制 PDF 输出。

```yaml
---
title: "Quarto Document"
author: "Norah Jones"
format: 
  pdf: 
    toc: true
    number-sections: true
---
```

### 多种格式

您创建的某些文档只有一种输出格式，但在许多情况下，我们需要支持多种格式。让我们在文档中添加 `html` 和 `docx` 格式。

```yaml
---
title: "Quarto Document"
author: "Norah Jones"
toc: true
number-sections: true
highlight-style: pygments
format: 
  html: 
    code-fold: true
    html-math-method: katex
  pdf: 
    geometry: 
      - top=30mm
      - left=20mm
  docx: default
---
```

这里有很多东西需要了解！让我们来细分一下。前两行是与输出格式完全无关的通用文档元数据。

```yaml
title: "Quarto Document"
author: "Norah Jones"
```

接下来的三行是*适用于所有格式*的文档格式选项，这也是在根级别指定这些选项的原因。

```yaml
toc: true
number-sections: true
highlight-style: pygments
```

接下来是 `format`（格式）选项，提供特定格式。

```yaml
format:
  html: 
    code-fold: true
    html-math-method: katex
  pdf:
    geometry: 
      - top=30mm
      - left=20mm
  docx: default
```

`html `和 `pdf `格式各提供一两个选项。例如，对于 HTML 输出，我们希望用户可以控制显示或隐藏代码（`code-fold: true`），并使用 `katex`来处理数学文本。对于 PDF，我们定义了一些页边距。docx "格式有点不同--它指定了 `docx: default`。这意味着只需使用该格式的所有默认选项。

## 重新渲染

文档选项中指定的格式定义了默认渲染的格式。如果我们使用上述所有选项渲染文档，则格式如下。

```{.bash
quarto render authoring.qmd
```

然后会创建以下文件。

- `authoring.html`
- `authoring.pdf`
- `authoring.docx`

我们可以使用 `--to` 选项选择一种或多种格式。

```{.bash
quarto render authoring.qmd --to docx
quarto render authoring.qmd --to docx,pdf
```

请注意，目标文件（本例中为 `authoring.qmd`）应始终是第一个命令行参数。

如果需要，我们还可以呈现未在文档选项中指定的格式。

```{.bash
quarto render authoring.qmd --to odt
```

由于文档选项中不包含 `odt` 格式，因此将使用该格式的默认选项。

## 章节

您可以使用目录和/或章节编号来方便读者浏览文档。方法是在文档选项中添加 `toc` 和/或 `number-sections` 选项。请注意，这些选项通常在根级别指定，因为它们在所有格式中共享。

```markdown
---
title: Quarto Basics
author: Norah Jones
date: 'May 22, 2021'
toc: true
number-sections: true
---

## Colors

- Red
- Green 
- Blue

## Shapes

- Square
- Circle
- Triangle

## Textures

- Smooth
- Bumpy
- Fuzzy
```

下面是该文档渲染成 HTML 后的效果。

![](images/sections-render.png){.border}

有很多选项可用于控制目录和章节编号的行为方式。请参阅输出格式文档（例如 [HTML](/docs/output-formats/html-basics.qmd)、[PDF](/docs/output-formats/pdf-basics.qmd)、[MS Word](/docs/output-formats/ms-word.qmd)）了解更多详情。

## 公式

您可以在 markdown 中使用 LaTeX 方程。

```markdown
爱因斯坦的特殊相对理论，表达了质量和能量的等价性：

$E = mc^{2}$
```

渲染后效果如下。

::: {.card style="padding: 10px;"}
爱因斯坦的特殊相对理论，表达了质量和能量的等价性：

$E = mc^{2}$
:::

内联方程用 `$...$` 分隔。要在新行内创建等式（显示等式），请使用 `$$...$$`。更多详情，请参阅 [markdown 公式](/docs/authoring/markdown-basics.html#equations)。

## 引用

在 Quarto 文档中引用其他作品。首先以支持的格式（BibTeX 或 CSL）创建一个书目文件。然后，使用 `bibliography` YAML 元数据选项将书目链接到文档。

下面是一份包含书目和单一引文的文档。

````markdown
---
title: Quarto Basics
format: html
bibliography: references.bib
jupyter: python3
---

## Overview

Knuth says always be literate [@knuth1984].

```{{python}}
1 + 1
```

## References
````

请注意，书目中的项目使用 `@citeid` 语法引用。

```markdown
 Knuth says always be literate [@knuth1984].
```

参考资料将包含在文档的末尾，因此我们在源文件的底部加入了一个 `## 参考资料 ` 标题。

下面是该文档渲染后的样子。

![](/docs/get-started/authoring/images/citations-render.png){.border width="600" fig-alt="Rendered document with references section at the bottom the content of which reads 'Knuth, Donald E. 1984. Literate Programming. The Computer Journal 27 (2): 97-111.'"}


`@` 引用语法非常灵活，支持前缀、后缀、定位器和文中引用。如需了解更多信息，请参阅[引文和脚注](/docs/authoring/footnotes-and-citations.qmd "Citations and Footnotes")文档。

## 交叉引用

交叉引用可以为图、表、方程式和章节提供编号引用和超链接，从而方便读者浏览文档。可交叉引用的实体一般需要一个标签（唯一标识符）和一个标题。

本例说明了各类实体的交叉引用。

````markdown
---
title: Quarto Crossrefs
format: html
jupyter: python3
---

## Overview

See @fig-simple in @sec-plot for a demonstration of a simple plot. 

See @eq-stddev to better understand standard deviation.

## Plot {#sec-plot}

```{{python}}
#| label: fig-simple
#| fig-cap: "Simple Plot"
import matplotlib.pyplot as plt
plt.plot([1,23,2,4])
plt.show()
```

## Equation {#sec-equation}

$$
s = \sqrt{\frac{1}{N-1} \sum_{i=1}^N (x_i - \overline{x})^2}
$$ {#eq-stddev}
````

我们交叉引用了章节、图表和方程式。下表显示了我们是如何表达这些内容的.

+----------+---------------+----------------------------------+
| Entity   | Reference     | Label / Caption                  |
+==========+===============+==================================+
| Section  | `@sec-plot`   | ID added to heading:             |
|          |               |                                  |
|          |               | ``{.default code-copy="false"} | |          |               | # Plot {#sec-plot}               | |          |               |``                              |
+----------+---------------+----------------------------------+
| Figure   | `@fig-simple` | YAML options in code cell:       |
|          |               |                                  |
|          |               | ``{.default code-copy="false"} | |          |               | #| label: fig-simple             | |          |               | #| fig-cap: "Simple Plot"        | |          |               |``                              |
+----------+---------------+----------------------------------+
| Equation | `@eq-stddev`  | At end of display equation:      |
|          |               |                                  |
|          |               | ``default                      | |          |               | $$ {#eq-stddev}                  | |          |               |``                              |
+----------+---------------+----------------------------------+

: {tbl-colwidths=\[20,30,50\]}

最后，下面是该文档的渲染效果。

![](/docs/get-started/authoring/images/crossref-render.png){.border width="600" fig-alt="Rendered page with linked cross references to figures and equations."}

请参阅[交叉引用](/docs/authoring/cross-references.qmd "Cross References")一文了解更多信息，包括如何自定义标题和参考文献文本（例如，使用 "Fig. "而不是 "Figure"）。

## 标示

标注是一种极好的方法，可以引起人们对某些概念的额外关注，或更清楚地表明某些内容是补充性的或只适用于某些情况。

标注是具有特殊标注属性的标记符 div。要在标记符单元格中创建标示，请在文档中键入以下内容。

```markdown
::: {.callout-note}
请注意，有五种类型的标注，包括:
`note`, `tip`, `warning`, `caution`, and `important`.
:::
```

显示效果如下

::: callout-note
请注意，有五种类型的标注，包括 `note`, `tip`, `warning`, `caution`, and `important`.
:::

您可以在 [标示](/docs/authoring/callouts.qmd "Callouts") 文档中进一步了解不同类型的标注及其外观选项。

## 文章布局

Quarto 文章正文的默认宽度约为 700 像素。选择这种宽度是为了[optimize readability](https://medium.com/ben-shoemate/optimum-web-readability-max-and-min-width-for-page-text-dee9987a27a0 “优化可读性”)。这通常会在文档页边空白处留出一些可用空间，有几种方法可以利用这些空间。

在本例中，我们使用 "reference-location "选项来表示我们希望将脚注放在右边距。

我们还使用了 "column: screen-inset "单元格选项，表示我们希望我们的数字占据整个屏幕的宽度，并有一个小的嵌入。

````markdown
---
title: Quarto Layout
format: html
reference-location: margin
jupyter: python3
---

## Placing Colorbars

彩条表示图像数据的定量范围。
在图中放置彩条并非易事，因为需要为它们留出空间。
空间。最简单的方法是在每个坐标轴上附加一个 
色条：请参阅 [Matplotlib 图库](https://matplotlib.org/stable/gallery/subplots_axes_and_figures/colorbar_placement.html)进一步了解色条。


```{{python}}
#| code-fold: true
#| column: screen-inset
import matplotlib.pyplot as plt
import numpy as np

fig, axs = plt.subplots(2, 2)
fig.set_size_inches(20, 8)
cmaps = ['RdBu_r', 'viridis']
for col in range(2):
    for row in range(2):
        ax = axs[row, col]
        pcm = ax.pcolormesh(
          np.random.random((20, 20)) * (col + 1),
          cmap=cmaps[col]
        )
        fig.colorbar(pcm, ax=ax)
plt.show()
```
````

下面是该文件的渲染效果。

![](images/layout-render.png){.border fig-alt="Document with Quarto Layout title at the top followed by Placing Colorbars header with text below it. Next to the text is a footnote in the page margin. Below the text is a toggleable code widget to hide/reveal the code followed by four plots displayed in two rows and two columns."}

您可以在页边空白处查找引文、脚注和 [旁注](https://quarto.org/docs/authoring/article-layout.html#asides)。您还可以为数字、表格或其他内容定义自定义列距。更多详情，请参阅[文章布局](/docs/authoring/article-layout.qmd)文档。

{{< include _footer.md >}}


