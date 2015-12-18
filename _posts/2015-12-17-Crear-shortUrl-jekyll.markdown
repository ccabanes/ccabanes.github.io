---
layout: post
comments: true
title:  "Crear URLs cortas con jekyll"
date:   2015-12-17 20:36:00
categories: "blog"
tag: "jekyll,blog"
urlshort: http://bit.ly/1NrQAWi
---
Una forma sencilla de crear url cortas en un blog con jekyll es incluir una nueva variable a nivel de post que podamos evaluar dentro de la plantilla que gestiona los enlaces de las redes sociales. 

En mi caso, he creado una nueva variable en mi cabecera del post que he llamado 'urlshort' con el valor del link. La variable debe de tener el valor del link ya calculado. En este caso, he usado [bitly](https://bitly.com/).

```javascript
---
layout: post
title:  "Crar URLs cortas con jekyll"
date:   2015-12-17 20:36:00
categories: jekyll, blog
urlshort: http://bit.ly/1NrQAWi
---
```

Ahora se puede usar esta variable por ejemplo dentro de la plantilla 'share.html' ya que está en el propio contexto. Se puede componer los enlaces en función si extá asignada o no la variable creada o bien dejando un enlace largo uniendo el dominio y la 'page.url' del post.


```html
<section class="share">
    <h4>Share this post</h4>
    {% raw %}
        {% if page.urlshort != null %}
        {% assign url = page.urlshort %}
        {% else %}    
        {% capture url%}{{site.domain_name}}{{page.url}}{%endcapture%}        
        {% endif %}
    {% endraw %} 
<a class="icon-twitter" href="http://twitter.com/share?text={{page.title}}&amp;url={% raw %}{{url}}{% endraw %}"
        onclick="window.open(this.href, 'twitter-share', 'width=550,height=235');return false;">
        <span class="hidden">Twitter</span>
    </a>
    <a class="icon-facebook" href="https://www.facebook.com/sharer/sharer.php?u={% raw %}{{url}}{% endraw %}"
        onclick="window.open(this.href, 'facebook-share','width=580,height=296');return false;">
        <span class="hidden">Facebook</span>
    </a>
    <a class="icon-google-plus" href="https://plus.google.com/share?url={% raw %}{{url}}{% endraw %}"
       onclick="window.open(this.href, 'google-plus-share', 'width=490,height=530');return false;">
        <span class="hidden">Google+</span>
    </a>
</section>
```

Ahora cuando el post tenga `urlshort` asignará en `{% raw %}{{url}}{% endraw %}` el valor corresponiente y en caso contrario, compondrá la url larga.

Fácil, verdad?
