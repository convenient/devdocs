---
layout: default
group: ext-best-practices
subgroup: 02_Extension-Coding
title: Extension Coding
menu_title: Extension Coding
menu_order: 1
menu_node: parent
version: 2.0
tabgroup: best-practices
tablabel: Coding
tabweight: 10
github_link: ext-best-practices/extension-coding/coding-best-practices.md
---

The coding best practices presented in this section should be known and understood by you the developer when creating or maintaining your extensions. This ensures that the {% glossarytooltip 55774db9-bf9d-40f3-83db-b10cc5ae3b68 %}extension{% endglossarytooltip %} you develop behaves and functions correctly within the Magento application architecture. This guide is not only meant to educate you about coding best practices but to also highlight some pitfalls we have seen other extension developers fall into so that you may avoid them.

For in depth content about creating extensions, see the [PHP Developer Guide]({{page.baseurl}}extension-dev-guide/bk-extension-dev-guide.html).

### Articles

{% assign subgroup = site.pages | where: "guide_version", page.guide_version | where:"group","ext-best-practices" | where: "subgroup","02_Extension-Coding" | where: "menu_node",null | sort: "menu_order" %}

{% for node in subgroup %}
*  [{{ node.menu_title }}]({{page.baseurl}}{{ node.github_link | replace: ".md",".html" }})
{% endfor %}
