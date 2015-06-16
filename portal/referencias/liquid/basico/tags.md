{% include 'portal/aviso.md' %}

# Tags

Tags are used for the logic in your template. New tags are very easy to code,
so I hope to get many contributions to the standard tag library after releasing
this code.

Here is a list of currently supported tags:

* **assign** - Assigns some value to a variable
* **capture** - Block tag that captures text into a variable
* **case** - Block tag, its the standard case...when block
* **comment** - Block tag, comments out the text in the block
* **cycle** - Cycle is usually used within a loop to alternate between values, like colors or DOM classes.
* **for** - For loop
* **break** - Exits a for loop
* **continue** Skips the remaining code in the current for loop and continues with the next loop
* **if** - Standard if/else block
* **include** - Includes another template; useful for partials
* **raw** - temporarily disable tag processing to avoid syntax conflicts.
* **unless** - Mirror of if statement

### Comments

Any content that you put between `{% raw %}{% comment %}{% endraw %}` and `{% raw %}{% endcomment %}{% endraw %}` tags is turned into a comment. 

```
We made 1 million dollars {% raw %}{% comment %}{% endraw %} in losses {% raw %}{% endcomment %}{% endraw %} this year
```

### If / Else

`if / else` should be well-known from any other programming language.
Liquid allows you to write simple expressions in the `if` or `unless` (and
optionally, `elsif` and `else`) clause:

```
{% if user %}
  Hello {{ user.name }}
{% endif %}
```

```
# Same as above
{% if user != null %}
  Hello {{ user.name }}
{% endif %}
```

```
{% raw %}
{% if user.name == 'tobi' %}
  Hello tobi
{% elsif user.name == 'bob' %}
  Hello bob
{% endif %}
{% endraw %}
```

```
{% raw %}
{% if user.name == 'tobi' or user.name == 'bob' %}
  Hello tobi or bob
{% endif %}
{% endraw %}
```

```
{% raw %}
{% if user.name == 'bob' and user.age > 45 %}
  Hello old bob
{% endif %}
{% endraw %}
```

```
{% raw %}
{% if user.name != 'tobi' %}
  Hello non-tobi
{% endif %}
{% endraw %}
```

```
{% raw %}
# Same as above
{% unless user.name == 'tobi' %}
  Hello non-tobi
{% endunless %}
{% endraw %}
```

```
{% raw %}
# Check for the size of an array
{% if user.payments == empty %}
   you never paid !
{% endif %}

{% if user.payments.size > 0  %}
   you paid !
{% endif %}
{% endraw %}
```

```
{% raw %}
{% if user.age > 18 %}
   Login here
{% else %}
   Sorry, you are too young
{% endif %}
{% endraw %}
```

```
{% raw %}
# array = 1,2,3
{% if array contains 2 %}
   array includes 2
{% endif %}
{% endraw %}
```

```
{% raw %}
# string = 'hello world'
{% if string contains 'hello' %}
   string includes 'hello'
{% endif %}
{% endraw %}
```

### Case Statement

If you need more conditions, you can use the `case` statement:

```
{% raw %}
{% case condition %}
{% when 1 %}
hit 1
{% when 2 or 3 %}
hit 2 or 3
{% else %}
... else ...
{% endcase %}
{% endraw %}
```

*Example:*

```
{% raw %}
{% case template %}

{% when 'label' %}
     // {{ label.title }}
{% when 'product' %}
     // {{ product.vendor | link_to_vendor }} / {{ product.title }}
{% else %}
     // {{page_title}}
{% endcase %}
{% endraw %}
```

### Cycle

Often you have to alternate between different colors or similar tasks.  Liquid
has built-in support for such operations, using the `cycle` tag.

```
{% raw %}
{% cycle 'one', 'two', 'three' %}
{% cycle 'one', 'two', 'three' %}
{% cycle 'one', 'two', 'three' %}
{% cycle 'one', 'two', 'three' %}
{% endraw %}
```

will result in

```
one
two
three
one
```

If no name is supplied for the cycle group, then it's assumed that multiple
calls with the same parameters are one group.

If you want to have total control over cycle groups, you can optionally specify
the name of the group.  This can even be a variable.

```
{% raw %}
{% cycle 'group 1': 'one', 'two', 'three' %}
{% cycle 'group 1': 'one', 'two', 'three' %}
{% cycle 'group 2': 'one', 'two', 'three' %}
{% cycle 'group 2': 'one', 'two', 'three' %}
{% endraw %}
```

will result in

```
one
two
one
two
```

### For loops

Liquid allows `for` loops over collections:

```
{% raw %}
{% for item in array %}
  {{ item }}
{% endfor %}
{% endraw %}
```

When iterating a hash, `item[0]` contains the key, and `item[1]` contains the value:

```
{% raw %}
{% for item in hash %}
  {{ item[0] }}: {{ item[1] }}
{% endfor %}
{% endraw %}
```

During every `for` loop, the following helper variables are available for extra
styling needs:

```
forloop.length      # => length of the entire for loop
forloop.index       # => index of the current iteration
forloop.index0      # => index of the current iteration (zero based)
forloop.rindex      # => how many items are still left?
forloop.rindex0     # => how many items are still left? (zero based)
forloop.first       # => is this the first iteration?
forloop.last        # => is this the last iteration?
```

There are several attributes you can use to influence which items you receive in
your loop

`limit:int` lets you restrict how many items you get.
`offset:int` lets you start the collection with the nth item.

```
{% raw %}
# array = [1,2,3,4,5,6]
{% for item in array limit:2 offset:2 %}
  {{ item }}
{% endfor %}
# results in 3,4
{% endraw %}
```

Reversing the loop

```
{% raw %}
{% for item in collection reversed %} {{item}} {% endfor %}
{% endraw %}
```

Instead of looping over an existing collection, you can define a range of
numbers to loop through.  The range can be defined by both literal and variable
numbers:

```
{% raw %}
# if item.quantity is 4...
{% for i in (1..item.quantity) %}
  {{ i }}
{% endfor %}
# results in 1,2,3,4
{% endraw %}
```

### Variable Assignment

You can store data in your own variables, to be used in output or other tags as
desired.  The simplest way to create a variable is with the `assign` tag, which
has a pretty straightforward syntax:

```
{% raw %}
{% assign name = 'freestyle' %}

{% for t in collections.tags %}{% if t == name %}
  <p>Freestyle!</p>
{% endif %}{% endfor %}
{% endraw %}
```

Another way of doing this would be to assign `true / false` values to the
variable:

```
{% raw %}
{% assign freestyle = false %}

{% for t in collections.tags %}{% if t == 'freestyle' %}
  {% assign freestyle = true %}
{% endif %}{% endfor %}

{% if freestyle %}
  <p>Freestyle!</p>
{% endif %}
{% endraw %}
```

If you want to combine a number of strings into a single string and save it to
a variable, you can do that with the `capture` tag. This tag is a block which
"captures" whatever is rendered inside it, then assigns the captured value to
the given variable instead of rendering it to the screen.

```
{% raw %}
{% capture attribute_name %}{{ item.title | handleize }}-{{ i }}-color{% endcapture %}

<label for="{{ attribute_name }}">Color:</label>
  <select name="attributes[{{ attribute_name }}]" id="{{ attribute_name }}">
  <option value="red">Red</option>
  <option value="green">Green</option>
  <option value="blue">Blue</option>
</select>
{% endraw %}
```