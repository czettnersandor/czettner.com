---
layout: post
title: Spring MVC with layout templates
created: 1318089658
comments: true
categories: [java]
---
If you have used other MVC frameworks (Rails or Grails for example) you will miss layout templates from Spring. I found an easy-to use solution which is also used in Grails under the hood. If you use Maven in your project, simply add it to the pom.xml file under the dependencies:

{% highlight xml %}
<dependency>
    <groupId>opensymphony</groupId>
    <artifactId>sitemesh</artifactId>
    <version>2.4.2</version>
</dependency>
{% endhighlight %}

Next, add the following to your web.xml:
{% highlight xml %}
<filter>
    <filter-name>sitemesh</filter-name>
    <filter-class>com.opensymphony.module.sitemesh.filter.PageFilter</filter-class>
</filter>
<filter-mapping>
    <filter-name>sitemesh</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
{% endhighlight %}

Add a new decorators.xml file under the WEB-INF directory:
{% highlight xml %}
<decorators defaultdir="/WEB-INF/layouts">
    <decorator name="application" page="application.jsp">
        <pattern>/*</pattern>
    </decorator>
</decorators>
{% endhighlight %}

This means /WEB-INF/layouts/application.jsp will be used for every url your application catch. You can specify others, for example /admin/* or /user/*:

{% highlight xml %}
    <decorator name="admin" page="admin.jsp">
        <pattern>/admin/*</pattern>
    </decorator>
    <decorator name="user" page="login.jsp">
        <pattern>/user/*</pattern>
    </decorator>
{% endhighlight %}

Now. you need to create the layout file(s) under /WEB-INF/layouts directory. Use the following code in it:
{% highlight xml %}
<%@ taglib prefix="decorator" uri="http://www.opensymphony.com/sitemesh/decorator" %>
...html, head, title, etc...
<nav>
<decorator:navigation/>
</nav>
<decorator:body/>
{% endhighlight %}

<decorator:body/> will be replaced as specified in the views:
{% highlight html %}
<body>
   <h1>The title of an article</h1>
   <p>The body of the article</p>
</body>
{% endhighlight %}

Visit the official SiteMesh website for more specified examples: http://www.opensymphony.com/sitemesh/
