---
title: Allied Security Trust
subtitle: Patent holding company that helps protect members from patent infringement lawsuits by non-practicing entities
date: '2019-01-05'
thumb_image: images/portfolio/ast.jpg
image: images/portfolio/ast.jpg
layout: project
---

<div class="block-header inner-sm" style="margin-top: 1.5em; margin-bottom: 1.5em">
  <h2 class="block-title line-top">Contributions</h2>
</div>

At Allied Security Trust, I was brought in during the very early stages of developing their MVP. One of my first tasks was to migrate their old MVP codebase to a new application architecture, update their UI from Angular Material to Clarity Design System, and migrate their old GraphQL queries to Hasura.

Within the data and document heavy application, I also had the opportunity to leverage ag-Grid to present this information in a digestible way with the freedom for users to customize and save their UI preferences.

Given the amount of data manipulation being done within these features, managing state when users are switching back and forth between tasks was a necessity. I was responsible for implementing a state management system with our Elasticsearch driven patent search page.

I also implemented an app wide alert service to integrate the two different alert styles used for desktop and mobile users. This service automated displaying alerts based on screen size with an option to preserve alerts on page redirect.

<div class="block-header inner-sm" style="margin-bottom: 1.5em">
  <h2 class="block-title line-top">Technology</h2>
</div>

This isn't a comprehensive list of all technologies used for this project but some of the primary ones are listed below:

- Angular 9+
- NestJS
- Hasura/GraphQL
- Firebase
- Elasticsearch
- ag-Grid
- Clarity Design System
- Ant Design