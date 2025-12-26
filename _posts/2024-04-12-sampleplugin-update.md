---
title: Update to methods for adding dishes in SamplePlugin
layout: default
---

In [SamplePlugin](https://github.com/syrve/front.api.sdk/tree/master/sample/v8), the methods [AddProduct](https://github.com/syrve/front.api.sdk/blob/7909d838587ad7d0408b9bbe5881d566055b5ffc/sample/v8/Resto. Front.Api.SamplePlugin/EditorTester.cs#L364) and [AddCompoundItem](https://github.com/syrve/front.api.sdk/blob/7909d838587ad7d0408b9bbe5881d566055b5ffc/sample/v8/Resto. Front.Api.SamplePlugin/EditorTester.cs#L411), used to add regular and compound dishes to an order. These methods demonstrate the mechanism for adding dishes to an order that require certain fields to be specified (e.g., mandatory modifiers, size) exclusively within the current editing session. For more details, see [Data editing]({{ site.baseurl }}/v6/en/Data editing.html).