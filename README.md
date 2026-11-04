# template

Templates for [Moth](https://github.com/samyyy2423/moth): text with `{{ name }}` in it, filled in from a
record or a dictionary, as [Mustache](https://mustache.github.io/) fills them. Values, fields of fields,
parts that repeat for each item of a list or show only when a value is there, other templates by name, and
HTML kept safe. In Moth alone, so it works everywhere Moth does.

```text
moth add template
```

```
use the package "template" as template

say template's fill("Hello, {{ name }}!", record with name "Sam")      # Hello, Sam!

set people to list of (record with name "Ana", age 31), (record with name "Bo", age 7)
say template's fill("{{# people }}{{ name }} is {{ age }}\n{{/ people }}", record with people people)
# Ana is 31
# Bo is 7
```

For a web page, `fill_html` makes each value safe to put in HTML, so a name like `<script>` shows as text:

```html
<!-- page.html -->
<h1>{{ title }}</h1>
<ul>
  {{# notes }}
  <li>{{ text }} <small>by {{ author.name }}</small></li>
  {{/ notes }}
</ul>
{{^ notes }}
<p>No notes yet.</p>
{{/ notes }}
```

```
set page to template's fill_html(read "page.html", record with title "Notes", notes notes)
```

## What goes between `{{` and `}}`

| | |
|---|---|
| `{{ name }}` | the value, as `say` shows it; `nothing`, or a name that isn't there, is "" |
| `{{ user.name }}` | a field of a field |
| `{{# name }} ... {{/ name }}` | the part between: once for each item of a list, with the item's fields (and the fields around it); once for a record or dictionary, with its fields; once for any other value; not at all for `nothing`, `no`, an empty list or "" |
| `{{ . }}` | the item a `{{# list }}` is on, such as a number or text |
| `{{^ name }} ... {{/ name }}` | the part between only when `{{# name }}` wouldn't show it |
| `{{! a note }}` | left out |
| `{{> name }}` | another template, from the partials given: `fill(words, values, partials)` |
| `{{{ name }}}` or `{{& name }}` | in `fill_html`, the value as it is |

A line with only a `{{# }}`, `{{^ }}`, `{{/ }}`, `{{! }}` or `{{> }}` on it is left out of what's made, so a
template can put each on a line of its own, as above. A mistake in a template, such as a `{{# }}` with no
`{{/ }}`, fails with its line: `line 3: the {{# notes }} has no {{/ notes }}`.

`html_safe(words)` makes any text safe to put in HTML, and `names_in(words)` lists the names a template uses.

A record's fields are read through JSON, so a record that holds a function (or a kind's record) can't be
filled from: give it one of just the values.

In Moth's own text, a `{` before a letter starts a value, so write a tag in a Moth program with a space after
its `{{`, as these do, or as `{\{name}}`. A template read from a file needs nothing.

## Tests

```text
moth test template.test.moth
```

## License

Apache License 2.0, as Moth is: see [LICENSE](LICENSE).
