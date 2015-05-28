{% include 'portal/aviso.md' %}

# Filtros

### Advanced output: Filters

Output markup takes filters.  Filters are simple methods.  The first parameter
is always the output of the left side of the filter.  The return value of the
filter will be the new left value when the next filter is run.  When there are
no more filters, the template will receive the resulting string.

```
Hello {{ 'tobi' | upcase }}
Hello tobi has {{ 'tobi' | size }} letters!
Hello {{ '*tobi*' | textilize | upcase }}
Hello {{ 'now' | date: "%Y %h" }}
```

### Standard Filters


* `date` - reformat a date ([syntax reference](http://docs.shopify.com/themes/liquid-documentation/filters/additional-filters#date))
* `capitalize` - capitalize words in the input sentence
* `downcase` - convert an input string to lowercase
* `upcase` - convert an input string to uppercase
* `first` - get the first element of the passed in array
* `last` - get the last element of the passed in array
* `join` - join elements of the array with certain character between them
* `sort` - sort elements of the array
* `map` - map/collect an array on a given property
* `size` - return the size of an array or string
* `escape` - escape a string
* `escape_once` - returns an escaped version of html without affecting existing escaped entities
* `strip_html` - strip html from string
* `strip_newlines` - strip all newlines (\n) from string
* `newline_to_br` - replace each newline (\n) with html break
* `replace` - replace each occurrence *e.g.* {% raw %}<code>{{ 'foofoo' | replace:'foo','bar' }}</code>{% endraw %}` #=> 'barbar'`
* `replace_first` - replace the first occurrence *e.g.* {% raw %}<code>{{ 'barbar' | replace_first:'bar','foo' }} #=> 'foobar'</code>{% endraw %}
* `remove` - remove each occurrence *e.g.* {% raw %}<code>{{ 'foobarfoobar' | remove:'foo' }} #=> 'barbar'</code>{% endraw %}
* `remove_first` - remove the first occurrence *e.g.* {% raw %}<code>{{ 'barbar' | remove_first:'bar' }} #=> 'bar'</code>{% endraw %}
* `truncate` - truncate a string down to x characters. It also accepts a second parameter that will append to the string *e.g.* {% raw %}<code>{{ 'foobarfoobar' | truncate: 5, '.' }} #=> 'foob.'</code>{% endraw %}
* `truncatewords` - truncate a string down to x words
* `prepend` - prepend a string *e.g.* {% raw %}<code>{{ 'bar' | prepend:'foo' }} #=> 'foobar'</code>{% endraw %}
* `append` - append a string *e.g.* {% raw %}<code>{{ 'foo' | append:'bar' }} #=> 'foobar'</code>{% endraw %}
* `slice` - slice a string. Takes an offset and length, *e.g.* {% raw %}<code>{{ "hello" | slice: -3, 3 }} #=> llo</code>{% endraw %}
* `minus` - subtraction *e.g.*  {% raw %}<code>{{ 4 | minus:2 }} #=> 2</code></code>{% endraw %}
* `plus` - addition *e.g.*  {% raw %}<code>{{ '1' | plus:'1' }} #=> 2</code>{% endraw %}, {% raw %}<code>{{ 1 | plus:1 }} #=> 2</code>{% endraw %}
* `times` - multiplication  *e.g* {% raw %}<code>{{ 5 | times:4 }} #=> 20</code>{% endraw %}
* `dividedBy` - integer division *e.g.* {% raw %}<code>{{ 10 | dividedBy:3 }} #=> 3</code>{% endraw %}
* `split` - split a string on a matching pattern *e.g.* {% raw %}<code>{{ "a~b" | split:"~" }} #=> ['a','b']</code>{% endraw %}
* `modulo` - remainder, *e.g.* {% raw %}<code>{{ 3 | modulo:2 }} #=> 1</code>{% endraw %}

