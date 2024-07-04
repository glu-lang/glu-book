---
icon: fas fa-stream
title: The Reference
order: 1
categories: ['The Architecture']
---

Below, every article explaining the architecture and the inner workings of the Glu Programming Language.

{% for category in page.categories -%}

### {{ category }}:

{% for doc in site.refs -%}
{% if doc.category == category -%}
- [{{doc.title}}]({{ doc.url }})

{% endif %}
{%- endfor -%}

{%- endfor -%}
