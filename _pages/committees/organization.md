---
title: Organizing Committee
layout: single
excerpt: "EACL 2026 organizing committee."
permalink: /committees/organization/
sidebar:
  nav: committees
---
<style>
.committee-list{display:grid;grid-template-columns:repeat(auto-fill,minmax(420px,1fr));gap:1.25rem;margin-bottom:2rem;}
.committee-card{display:flex;align-items:center;gap:.75rem;}
.committee-card img{width:64px;height:64px;border-radius:9999px;object-fit:cover;}
.committee-card__text{flex:1 1 auto;}
.committee-card__name{font-weight:700;font-size:1.125rem;line-height:1.2;white-space:normal;overflow:visible;text-overflow:unset;}
.committee-card__affil{white-space:normal;overflow:visible;text-overflow:unset;line-height:1.35;margin-top:.125rem;}
/* remove any residual dividers inside cards */
.committee-card, .committee-card * { border: 0 !important; box-shadow: none !important; }

/* grey line just under each section title (applies only when a .committee-list follows) */
h3 + .committee-list {
  border-top: 1px solid #e5e7eb;  /* gray-200 */
  padding-top: .75rem;            /* space between line and cards */
  margin-top: .25rem;             /* space between title and line */
}

/* optional: a little space above each title */
.page__content h3 { margin-top: 2rem; }

/* contact line & thin divider (for ED&I section) */
.committee-contact{margin:.25rem 0 .5rem;color:#4b5563;font-size:.95rem;}
.heading-divider{border-top:1px solid #e5e7eb;margin:.25rem 0 1rem;}
</style>

{% comment %} Map short role categories -> display headings {% endcomment %}
{% assign role_headings =  {
  "General": "General Chair",
  "Program": "Program Chair",
  "Local Organization": "Local Organization Chair",
  "Workshop": "Workshop Chair",
  "Tutorial": "Tutorial Chair",
  "Internal Communications": "Internal Communications Chair",

  "Demo": "Demonstration Chair",
  "SRW": "Student Research Workshop Chair",
  "Faculty SRW": "Faculty Advisor to the Student Research Workshop",
  "Industry": "Industry Chair",
  "Sponsorship": "Sponsorship Chair",
  "Publication": "Publication Chair",
  "Publicity": "Publicity Chair",
  "Communications": "Internal Communications Chair",  <!-- fallback if CSV uses "Communications" -->
  "Website": "Website Chair",
  "Local": "Local Arrangements Chair",
  "ED&I": "Diversity and Inclusion Chair"
} %}

{% assign preferred = "General|Program|Local Organization|Workshop|Tutorial|Internal Communications" | split:"|" %}
{% assign groups = site.data.organizers | group_by: "Role" %}

{% comment %} 1) Print preferred roles in this exact order {% endcomment %}
{% for r in preferred %}
  {% assign bucket = groups | where: "name", r | first %}
  {% if bucket and bucket.items.size > 0 %}

    {% comment %} Get base heading; avoid double-adding "Chair" if role already ends with it {% endcomment %}
    {% assign _mapped = role_headings[r] %}
    {% if _mapped %}
      {% assign heading = _mapped %}
    {% else %}
      {% assign _last = r | split: ' ' | last %}
      {% if _last == "Chair" or _last == "Chairs" %}
        {% assign heading = r %}
      {% else %}
        {% assign heading = r | append: " Chair" %}
      {% endif %}
    {% endif %}

    {% comment %} Pluralize when more than one person {% endcomment %}
    {% if bucket.items.size > 1 %}
      {% assign _last_word = heading | split: ' ' | last %}
      {% if _last_word == "Chair" %}
        {% assign heading = heading | append: "s" %}
      {% endif %}
    {% endif %}

### {{ heading }}
{% comment %} ED&I contact line (preferred loop) {% endcomment %}
{% if r == "ED&I" or r == "Diversity and Inclusion" %}
<p class="committee-contact">
  Contact: <a href="mailto:eacl2026deichairs@gmail.com">eacl2026deichairs@gmail.com</a>
</p>
<div class="heading-divider"></div>
{% endif %}
<div class="committee-list">
  {%- assign _annot = "" -%}
  {%- for p in bucket.items -%}
    {%- assign _name = p.Name | strip -%}
    {%- comment -%} take last token as surname; strip parentheses like "(May)" {%- endcomment -%}
    {%- assign _surname = _name | replace: "(", " " | replace: ")", " " | split: " " | last | downcase -%}
    {%- assign _annot = _annot
        | append: _surname | append: "§" | append: forloop.index0
        | append: "¶" -%}
  {%- endfor -%}
  {%- assign _rows = _annot | split: "¶" | sort_natural -%}
  {%- for row in _rows -%}
    {%- unless row == "" -%}
      {%- assign _parts = row | split: "§" -%}
      {%- assign _idx = _parts[1] | plus: 0 -%}
      {%- assign person = bucket.items[_idx] -%}
      {% include committee_card.html
           name=person.Name
           affiliation=person.Affiliation
           photo=person.Photo
           url=person.Scholar %}
    {%- endunless -%}
  {%- endfor -%}
</div>

  {% endif %}
{% endfor %}

{% comment %} 2) Then print any remaining roles not listed above {% endcomment %}
{% for g in groups %}
  {% unless preferred contains g.name %}

    {% comment %} Get base heading; avoid double-adding "Chair" {% endcomment %}
    {% assign _mapped = role_headings[g.name] %}
    {% if _mapped %}
      {% assign heading = _mapped %}
    {% else %}
      {% assign _last = g.name | split: ' ' | last %}
      {% if _last == "Chair" or _last == "Chairs" %}
        {% assign heading = g.name %}
      {% else %}
        {% assign heading = g.name | append: " Chair" %}
      {% endif %}
    {% endif %}

    {% if g.items.size > 1 %}
      {% assign _last_word = heading | split: ' ' | last %}
      {% if _last_word == "Chair" %}
        {% assign heading = heading | append: "s" %}
      {% endif %}
    {% endif %}

### {{ heading }}
{% comment %} ED&I contact line (remaining-groups loop) {% endcomment %}
{% if g.name == "ED&I" or g.name == "Diversity and Inclusion" %}
<p class="committee-contact">
  Contact: <a href="mailto:eacl2026deichairs@gmail.com">eacl2026deichairs@gmail.com</a>
</p>
<div class="heading-divider"></div>
{% endif %}
<div class="committee-list">
  {%- assign _annot = "" -%}
  {%- for p in g.items -%}
    {%- assign _name = p.Name | strip -%}
    {%- assign _surname = _name | replace: "(", " " | replace: ")", " "
                              | split: " " | last | downcase -%}
    {%- assign _annot = _annot
        | append: _surname | append: "§" | append: forloop.index0
        | append: "¶" -%}
  {%- endfor -%}
  {%- assign _rows = _annot | split: "¶" | sort_natural -%}
  {%- for row in _rows -%}
    {%- unless row == "" -%}
      {%- assign _parts = row | split: "§" -%}
      {%- assign _idx = _parts[1] | plus: 0 -%}
      {%- assign person = g.items[_idx] -%}
      {% include committee_card.html
           name=person.Name
           affiliation=person.Affiliation
           photo=person.Photo
           url=person.Scholar %}
    {%- endunless -%}
  {%- endfor -%}
</div>

  {% endunless %}
{% endfor %}


## Contact

- **General Chair**: [Aline Villavicencio](https://sites.google.com/view/alinev)
- **Program Chairs**: [Vera Demberg](https://www.uni-saarland.de/lehrstuhl/demberg.html), [Kentaro Inui](https://kentaro-inui.github.io/), Lluís Marquez Villodre

For questions related to paper submission, email: <editors@aclrollingreview.org>  

For questions related to paper commitment and program, email: <eacl26-pcs@googlegroups.com>  

For all other questions, email: <eacl2026.contact@gmail.com>

