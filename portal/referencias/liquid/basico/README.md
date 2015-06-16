{% include 'portal/aviso.md' %}

# Guia básico de Liquid

There are two types of markup in Liquid: Output and Tag.

* Output markup (which may resolve to text) is surrounded by

```
{% raw %}
{{ matched pairs of curly brackets (ie, braces) }}
{% endraw %}
```

* Tag markup (which cannot resolve to text) is surrounded by

```
{% raw %}
{% matched pairs of curly brackets and percent signs %}
{% endraw %}
```

## Output

Here is a simple example of Output:

```
Hello {{name}}
Hello {{user.name}}
Hello {{ 'tobi' }}
```
