---
layout: default
title: About
description: 
keywords: 
comments: true
menu: 关于
permalink: /about/
---

<section class="collection-head small geopattern" data-pattern-id="{{ page.title | truncate: 15}}">
    <div class="container">
        <div class="collection-title">
            <h1 class="collection-header">{{ page.title }}</h1>
        </div>
    </div>
</section>

{% if site.github.contributors != null %}

<section class="container">

这里是 ELC Bell Net Group 技术博客，在一群鸽子手下，能用下去就谢天谢地了

<hr style="height: 100px"/>
    <h2>Contributors</h2>
    <div class="repo-list">
        <!-- Check here for github metadata -->
        <!-- https://help.github.com/articles/repository-metadata-on-github-pages/ -->
        <table style="width:100%">
            {%- for con in site.github.contributors -%}
            <tr>
                <th><img src="{{ con.avatar_url }}" style="max-width:100px"/></th>
                <th style="text-align:right">
                    <a href="{{ con.html_url }}" target="_blank">{{ con.login }}</a>
                    <br/>
                    {% if site.data.contributors %}
                        {% for signed in site.data.contributors %}
                            {%- if signed.name == con.login -%}
                                <p class="card-text">{{ signed.intro }}</p>
                            {%- endif -%}
                        {% endfor %}
                    {% endif %}
                </th>
            </tr>
            {% endfor %}
        </table>
    </div>
</section>

{% endif %}