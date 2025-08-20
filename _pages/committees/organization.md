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

{% assign ordered_roles = "General|Program|Workshop|Tutorial|Demo|SRW|Faculty SRW|Industry|Sponsorship|Publication|Publicity|Communications|Website|Local Chair|Local Organization|ED&I" | split:"|" %}

{% for role in ordered_roles %}
  {% assign people = site.data.organizers | where: "Role", role %}
  {% if people and people.size > 0 %}
    {% assign base_heading = role_headings[role] | default: role | append: " Chair" %}
    {% if people.size > 1 %}
      {% assign heading = base_heading | replace: "Chair", "Chairs" %}
    {% else %}
      {% assign heading = base_heading %}
    {% endif %}

### {{ heading }}
<div class="committee-list">
  {% for p in people %}
  {% include committee_card.html
     name=p.Name
     affiliation=p.Affiliation
     photo=p.Photo
     url=p.Scholar %}
  {% endfor %}
</div>

  {% endif %}
{% endfor %}
