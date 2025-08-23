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
<div class="committee-list">
  {% for p in bucket.items %}
    {% include committee_card.html name=p.Name affiliation=p.Affiliation photo=p.Photo url=p.Scholar %}
  {% endfor %}
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
<div class="committee-list">
  {% for p in g.items %}
    {% include committee_card.html name=p.Name affiliation=p.Affiliation photo=p.Photo url=p.Scholar %}
  {% endfor %}
</div>
  {% endunless %}
{% endfor %}
