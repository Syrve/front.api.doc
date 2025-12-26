---
title: Ability to skip marking verification
layout: default
tags: v9preview3 v9
---

Starting from V9Preview3, it is possible to skip marking verification for dishes that have the "Allow skipping marking scan" checkbox enabled in their TN VED EAES code.
To do this, call the method
[`SkipScanningOrderItemMarkingCode`](https://syrve.github.io/front.api.sdk/v9/html/M_Resto_Front_Api_Editors_IEditSession_SkipScanningOrderItemMarkingCode.htm),
and the presence of a marking for the marked dish will not be verified.
To resume marking verification for the dish, call the method
[`ChangeOrderItemMarkingCode`](https://syrve.github.io/front.api.sdk/v9/html/M_Resto_Front_Api_Editors_IEditSession_ChangeOrderItemMarkingCode.htm).