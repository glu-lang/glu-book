---
icon: fas fa-stream
title: The Book
order: 1
---

Bellow, every chapters of the Glu Programming Language Book.

### The Glu Basics:

{% for doc in site.docs -%}
{% if doc.category == "The Glu Basics" -%}
- [{{doc.title}}]({{ doc.url }})

{% endif %}
{%- endfor -%}


### Common Concepts:

{% for doc in site.docs -%}
{% if doc.category == "Common Concepts" -%}
- [{{doc.title}}]({{ doc.url }})

{% endif %}
{%- endfor -%}


### Memory & Management:

{% for doc in site.docs -%}
{% if doc.category == "Memory & Management" -%}
- [{{doc.title}}]({{ doc.url }})

{% endif %}
{%- endfor -%}

### Structure related data using Structs:

{% for doc in site.docs -%}
{% if doc.category == "Structure related data using Structs" -%}
- [{{doc.title}}]({{ doc.url }})

{% endif %}
{%- endfor -%}

### Build complex Data Types:

{% for doc in site.docs -%}
{% if doc.category == "Build complex Data Types" -%}
- [{{doc.title}}]({{ doc.url }})

{% endif %}
{%- endfor -%}

### Generic Types and Templates:

{% for doc in site.docs -%}
{% if doc.category == "Generic Types and Templates" -%}
- [{{doc.title}}]({{ doc.url }})
{% endif %}
{%- endfor -%}

### Understanding Imports and Namespaces:

{% for doc in site.docs -%}
{% if doc.category == "Understanding Imports and Namespaces" -%}
- [{{doc.title}}]({{ doc.url }})
{% endif %}
{%- endfor -%}

### Appendix:

{% for doc in site.docs -%}
{% if doc.category == "Appendix" -%}
- [{{doc.title}}]({{ doc.url }})
{% endif %}
{%- endfor -%}

