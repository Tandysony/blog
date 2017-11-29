---
layout: post
title: Markdown and HTML
tags: [Jekyll, LaTex, Markdown]
---

[Jekyll](https://jekyllrb.com/) supports the use of Markdown with inline HTML tags which makes it easier to quickly write posts with Jekyll, without having to worry too much about text formatting. Pleaser refer to [GitHub flavoured Markdown tutorial](https://guides.github.com/features/mastering-markdown/) for details. A set of table, figure, code and LaTax equation examples are give below to demonstrate how to use Markdown in Jekyll.

  * Tables have also been extended from Markdown:

  First Header  | Second Header
  ------------- | -------------
  Content Cell  | Content Cell
  Content Cell  | Content Cell

  * Here's an example of an image, which is included using Markdown:

  ![Geometric pattern with fading gradient]({{ site.baseurl }}/assets/img/thumbnails/0001-scrabble-blog.jpeg)

  * Highlighting for code in Jekyll is done using Pygments or Rouge. This theme makes use of Rouge by default.

    {% highlight js %}
    // count to ten
    for (var i = 1; i <= 10; i++) {
        console.log(i);
    }

    // count to twenty
    var j = 0;
    while (j < 20) {
        j++;
        console.log(j);
    }
    {% endhighlight %}

  * Type on Strap uses KaTeX to display maths. Equations such as $$S_n = a \times \frac{1-r^n}{1-r}$$ can be displayed inline. Alternatively, they can be shown on a new line:

$$ f(x) = \int \frac{2x^2+4x+6}{x-2} $$
