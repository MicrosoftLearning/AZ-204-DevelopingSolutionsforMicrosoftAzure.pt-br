---
title: Instruções online hospedadas
permalink: index.html
layout: home
---

## Diretório de conteúdo

Hiperlinks para cada um dos laboratório estão listados abaixo.

## Laboratórios

{% assign labs = site.pages | where_exp: "page", "page.url contains '/Instructions/Labs'" %}
| Módulo | Laboratório |
| --- | --- |
{% for activity in labs %}{% if activity.lab.az204Module %}| {{ activity.lab.az204Module }} | [{{ activity.lab.az204Title }}]({{ site.github.url }}{{ activity.url }}) |
{% endif %}{% endfor %}

