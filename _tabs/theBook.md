---
icon: fas fa-stream
title: The Book
order: 0
---

Below, every chapters of the Glu Programming Language Book.


{% for category in site.docs[0].categories -%}

### {{ category }}:

{% for doc in site.docs -%}
{% if doc.category == category -%}
- [{{doc.title}}]({{ doc.url }})

{% endif %}
{%- endfor -%}

{%- endfor -%}
