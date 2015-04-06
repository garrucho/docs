# Partials

Uma `partial` é um trecho de template que pode ser reutilizado.

Exemplo:

Partial `header.html`:
```html
<html>
<head>
	<title>{{ accountName }}</title>
</head>
<body>	
```

Partial `footer.html`:
```html
</body>
</html>
```

Página `home.html`:
```html
{% include 'header' %}

Olá, mundo!

{% include 'footer' %}
```

Note que ao usar a tag Liquid `include` omitimos a extensão do arquivo (.html) ([leia mais sobre Liquid](../../referencias/liquid/index.html)).
