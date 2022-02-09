---
id: I99Yv6N68LmPukkyL8QFJ
title: animacje
desc: ''
updated: 1632331795540
created: 1631889353831
---

## Animacje pojawiania się i znikania

Nie można animować z **display: none** na **display: block**.
Trzeba animować z **opacity: 0; visibility: hidden;** na **opacity: 1; visibility: visible; **, a żeby było legit należałoby najpierw zmienić display z none na block a potem animować opacity - **elementy niewidoczne powinny mieć display: none;**.

## Performance
Only Animate CSS Opacity, Transforms and Filters

![](/assets/images/2021-09-17-16-37-45.png)
