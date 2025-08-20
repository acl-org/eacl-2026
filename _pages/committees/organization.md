---
title: Organizing Committee
layout: single
excerpt: "EACL 2026 organizing committee."
permalink: /committees/organization/
sidebar:
  nav: committees
---

{% assign role_headings =  {
  "General": "General Chair",
  "Program": "Program Chair",
  "Workshop": "Workshop Chair",
  "Tutorial": "Tutorial Chair",
  "Demo": "Demonstration Chair",
  "SRW": "Student Research Workshop Chair",
  "Facutly SRW": "Faculty Advisor to the Student Research Workshop",
  "Faculty SRW": "Faculty Advisor to the Student Research Workshop",
  "Industry": "Industry Chair",
  "Sponsorship": "Sponsorship Chair",
  "Publication": "Publication Chair",
  "Publicity": "Publicity Chair",
  "Communications": "Internal Communication Chair",
  "Website": "Website Chair",
  "Local Chair": "Local Chair",
  "Local Organization": "Local Organization Chair",
  "ED&I": "Diversity and Inclusion Chair"
} %}

{% assign preferred = "General|Program|Workshop|Tutorial|Demo|SRW|Facutly SRW|Faculty SRW|Industry|Sponsorship|Publication|Publicity|Communications|Website|Local Chair|Local Organization|ED&I" | split:"|" %}
{% assign groups = site.data.organizers | group_by: "Role" %}

{% for r in preferred %}
  {% assign bucket = groups | where: "name", r | first %}
  {% if bucket and bucket.items and bucket.items.size > 0 %}
    {% assign base = role_headings[r] | default:r | append:" Chair" %}
    {% assign heading = bucket.items.size > 1 | default:false | if: base | replace:"Chair","Chairs" %}
    {% if bucket.items.size == 1 %}{% assign heading = base %}{% endif %}

### {{ heading }}
<div class="committee-list">
  {% for p in bucket.items %}
  {% include committee_card.html name=p.Name affiliation=p.Affiliation photo=p.Photo url=p.Scholar %}
  {% endfor %}
</div>

  {% endif %}
{% endfor %}

{% for g in groups %}
  {% unless preferred contains g.name %}
    {% assign base = role_headings[g.name] | default:g.name | append:" Chair" %}
    {% assign heading = g.items.size > 1 | default:false | if: base | replace:"Chair","Chairs" %}
    {% if g.items.size == 1 %}{% assign heading = base %}{% endif %}

### {{ heading }}
<div class="committee-list">
  {% for p in g.items %}
  {% include committee_card.html name=p.Name affiliation=p.Affiliation photo=p.Photo url=p.Scholar %}
  {% endfor %}
</div>

  {% endunless %}
{% endfor %}

