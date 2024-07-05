---
icon: fas fa-book
title: The Book
order: 0
categories: ['Basics', 'Common Concepts', 'Data Types', 'Polymorphism', 'Imports and Modules', 'Appendix']
---

Below, every chapter of the Glu Programming Language Book.

{% for category in page.categories -%}

## {{ category }}

{% for doc in site.docs -%}
{% if doc.category == category -%}
- [{{doc.title}}]({{ doc.url }})

{% endif %}
{%- endfor -%}

{%- endfor -%}
