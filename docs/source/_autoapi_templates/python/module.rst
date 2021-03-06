.. |nbsp| unicode:: 0xA0
    :trim:

.. role:: raw-html(raw)
    :format: html

.. |br| raw:: html

    <br/>

.. |default| raw:: html

    <div class="default-value-section"><span class="default-value-label">Default:</span></div>

.. |decorated| raw:: html

    <p class="decorated">

.. |fqp| raw:: html

    <p class="decorated">

{% if not obj.display %}
:orphan:

{% endif %}

{% if obj.top_level_object %}
Databay API Reference
=====================
{% else %}
{{ obj.name }}
======={{ "=" * obj.name|length }}
{% endif %}

.. py:module:: {{ obj.name }}

{% if obj.docstring %}
.. autoapi-nested-parse::

  {{ obj.docstring|prepare_docstring|indent(3) }}

{% endif %}

{% block subpackages %}
{% set visible_subpackages = obj.subpackages|selectattr("display")|list %}
{% if visible_subpackages %}

.. toctree::
  :maxdepth: 2

{% for subpackage in visible_subpackages %}
   - {{ subpackage.short_name|replace("databay.","") }} <{{ subpackage.short_name }}/index.rst>

{% endfor %}
{% endif %}
{% endblock %}
{% block submodules %}
{% set visible_submodules = obj.submodules|selectattr("display")|list %}
{% if visible_submodules %}

.. toctree::
  :maxdepth: 1

{% for submodule in visible_submodules %}
  {{ submodule.short_name|replace("databay.","") }} <{{ submodule.short_name }}/index.rst>

{% endfor %}

{% endif %}
{% endblock %}
{% block content %}
{% if obj.all is not none %}
{% set visible_children = obj.children|selectattr("short_name", "in", obj.all)|list %}
{% elif obj.type is equalto("package") %}
{% set visible_children = obj.children|selectattr("display")|list %}
{% else %}
{% set visible_children = obj.children|selectattr("display")|rejectattr("imported")|list %}
{% endif %}
{% if visible_children %}

..
   {{ obj.type|title }} Contents

   {{ "-" * obj.type|length }}---------

{% set visible_classes = visible_children|selectattr("type", "equalto", "class")|list %}
{% set visible_functions = visible_children|selectattr("type", "equalto", "function")|list %}

{% if "show-module-summary" in autoapi_options and visible_children|length > 1 %}

{% block classes scoped %}

.. rst-class:: mb-l mt-xxxx

    Contents:

.. autoapisummary::
   :nosignatures:

{% for child in visible_children %}
   {{ child.id }}

{% endfor %}


{% endblock %}
{% endif %}

{% for obj_item in visible_children %}
{{ obj_item.rendered|indent(0) }}
{% endfor %}
{% endif %}
{% endblock %}
