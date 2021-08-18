---
title: "Draft - Link to Samples folder"
linkTitle: "Draft - Link to Samples folder"
weight: 100
date: 2021-08-18
description: >
  Link to Samples folder - test for Zoomin
---

This page is **not** available in the Axway docs production portal, but it needs to be available on **staging** so Zoomin can see it.

There's no way to hide it on Netlify and, at the same time, make it available on Zoomin.

{{< alert title="Link using absolute path" color="warning" >}}
{{< /alert >}}

This is a link using absolute path, which we **don't** want -> Click to download [create_environments.json](https://axway-open-docs.netlify.app/samples/central/create_environments.json).

The code of the link is:

```
[create_environments.json](https://axway-open-docs.netlify.app/samples/central/create_environments.json)
```

{{< alert title="Link using relative path" color="primary" >}}
{{< /alert >}}

This is a link using **relative** path, which we want -> Click to download [create_environments.json](/samples/central/create_environments.json).

The code of the link is:

```
[create_environments.json](/samples/central/create_environments.json)
```

This relative link works on Netlify but not on Zoomin
