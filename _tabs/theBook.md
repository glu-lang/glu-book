---
icon: fas fa-stream
title: The Book
order: 0
categories: ['The Glu Basics', 'Common Concepts', 'Memory & Management', 'Structure related data using Structs', 'Build complex Data Types', 'Generic Types and Templates', 'Understanding Imports and Namespaces', 'Appendix']
---

Below, every chapter of the Glu Programming Language Book.


{% for category in page.categories -%}

### {{ category }}:

{% for doc in site.docs -%}
{% if doc.category == category -%}
- [{{doc.title}}]({{ doc.url }})

{% endif %}
{%- endfor -%}

{%- endfor -%}
