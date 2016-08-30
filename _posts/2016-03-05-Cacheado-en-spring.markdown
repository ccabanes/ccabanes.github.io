---
layout: post
disqus: "true"
title:  "Aplicar caché en Spring"
date:   2016-03-05
categories: "java Spring cache"
urlshort: http://bit.ly/1nmZhbx
---

Desde hace ya unas versiones [Spring](https://spring.io/) facilita el uso de la caché de forma fácil para el desarrollador. Mediante la anotación @Cacheable se puede aplicar caché a cualquier método que se quiera dentro de una aplicación spring.

Si no estás familiarizado con este entorno, te recomiendo que eches un vistazo a [gvNix](http://www.gvnix.org/), una de las herramientas más populares de desarrollo rápido de aplicaciones, con la que podrás en unos pocos minutos crear un nuevo proyecto basado en esta tecnología.

###Cómo configurar la caché en Spring?
Para poder empezar a usar la caché en Spring, primero hay que configurarla. En este caso, voy a usar como proveedor de caché [ehCache](http://www.ehcache.org/). Para ello, lo primero es incluir la dependencia en el proyecto:

######Pom.xml
{% highlight xml%}
<dependency>
    <groupId>net.sf.ehcache</groupId>
    <artifactId>ehcache</artifactId>
    <version>2.9.0</version>
</dependency>
{% endhighlight %}

En el fichero ```ApplicationContext.xml``` incluimos lo siguiente:

######ApplicationContext.xml
{% highlight xml%}
  <beans  xmlns:cache="http://www.springframework.org/schema/cache"  xsi:schemaLocation="http://www.springframework.org/schema/cache http://www.springframework.org/schema/cache/spring-cache.xsd">
    ...
    <cache:annotation-driven mode="aspectj" />
     <bean id="cacheManager" class="org.springframework.cache.ehcache.EhCacheCacheManager">
    <property name="cacheManager" ref="ehcache" />
    </bean>
    <bean id="ehcache" class="org.springframework.cache.ehcache.EhCacheManagerFactoryBean">
        <property name="configLocation" value="classpath:ehcache.xml"/>
    </bean>

{% endhighlight %}

En la configuración se especifica el fichero donde están anotadas las diferentes cachés mediante _classpath:ehcache.xml_. Ahora creamos dicho fichero en _src/main/resources_:

######ehcache.xml
En este fichero es donde se definen las diferentes cachés que usará la aplicación. Existen multitud de parámetros para configurar cada una de ellas. [Aquí](http://www.ehcache.org/ehcache.xml) te dejo un ejemplo.

{% highlight xml%}
<ehcache xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:noNamespaceSchemaLocation="ehcache.xsd"
    updateCheck="false"
    monitoring="off"
    dynamicConfig="true"
    maxBytesLocalHeap="100M">

    <!--diskStore path="java.io.tmpdir" /-->
    <cache name="demoCache"
           maxEntriesLocalHeap="10000"
           eternal="false"
           timeToLiveSeconds="600"
           overflowToOffHeap="true"
           maxBytesLocalOffHeap="4g"
           diskExpiryThreadIntervalSeconds="1">
        <persistence strategy="localTempSwap"/>
     </cache>
</ehcache>
{% endhighlight %}

Finalmente tan solo queda por aplicar la caché al método deseado.

####Anotación @Cacheable
Para aplicar la caché sobre un método se usa la anotación ```@Cacheable```:

{% highlight java%}
@Cacheable(value="demoCache", key="#isbn"
public Book findBook(ISBN isbn, boolean checkWarehouse, boolean includeUsed)
@Cacheable(value="demoCache", key="#isbn.rawNumber")
public Book findBook(ISBN isbn, boolean checkWarehouse, boolean includeUsed)
@Cacheable(value="demoCache", key="T(someType).hash(#isbn)")
public Book findBook(ISBN isbn, boolean checkWarehouse, boolean includeUsed)
{% endhighlight %}

La anotación ```@Cacheable``` internamente usa un ```Map<key,value>``` para almacenar las cachés según el valor del parámetro ```key```. En el caso del ejemplo, usará el valor de ```isbn``` o ```isbn.rawNumber``` para saber si tiene que ejecutar el método o bien obterner dichos valores del ```Map```, teniendo también en cuenta los parámetros de configuración de la caché descrita anteriormente en el fichero ```ehcache.xml```.

Como se aprecia en el tercer ejemplo, es posible también utilizar expresiones [SpEL](http://docs.spring.io/spring/docs/3.2.13.RELEASE/spring-framework-reference/htmlsingle/#expressions) como criterio dentro del ```key```.
