---
layout: archive
title: "Publications"
permalink: /publications/
author_profile: true
---
## Journal Article
**C. Qu**, J. He, J. Li, X. Duan and Y. Mo, "Optimal Control for Mobile Agents Considering State Unpredictability", IEEE Transactions on Automatic Control, Jun, 2024. (Early access) \[[paper](https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=10363359)\]

## Conference Papers
**C. Qu**, J. He, X. Duan and S. Wu, "Control Input Inference of Mobile Agents under Unknown Objective", IFAC World Congress 2023, Yokohama, Japan, 2023.  
**C. Qu**, J. He and J. Li, "Multi-period Optimal Control for Mobile Agents Considering State Unpredictability", 2022 IEEE 96th Vehicular Technology Conference (VTC2022-Fall), London, United Kingdom, 2022.  
**C. Qu**, J. He, J. Li, C. Fang and Y. Mo, "Moving Target Interception Considering Dynamic Environment", 2022 American Control Conference (ACC), Atlanta, GA, USA, 2022.

## Preprints
**C. Qu**, J. He, X. Duan and J. Chen, "Inverse Reinforcement Learning with Unknown Reward Model based on Structural Risk Minimization", 2023. \[[paper](https://arxiv.org/abs/2312.16566)\]  
**C. Qu**, J. He and X. Duan, "Observation-based Optimal Control Law Learning with LQR Reconstruction", 2023, submitted to TAC. \[[paper](https://arxiv.org/abs/2312.16572)\]


{% if author.googlescholar %}
  You can also find my articles on <u><a href="{{author.googlescholar}}">my Google Scholar profile</a>.</u>
{% endif %}

{% include base_path %}

{% for post in site.publications reversed %}
  {% include archive-single.html %}
{% endfor %}
