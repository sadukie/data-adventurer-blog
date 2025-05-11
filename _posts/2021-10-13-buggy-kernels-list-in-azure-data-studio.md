---
title: Buggy Kernels List in Azure Data Studio
tags: azure-data-studio r jupyter-notebooks
---

This is a follow-up to [Getting an R Jupyter Kernel in Azure Data Studio]({{site.baseurl}}/2021/10/13/getting-an-r-jupyer-kernel-in-azure-data-studio). After getting the R kernel set, I was able to see the R kernel in my dropdown for an existing notebook. However, things are weird for a new notebook. Let's take a look at what I'm doing.

## Creating a New Notebook

I've created a new notebook in Azure Data Studio by going to the File menu and selecting New Notebook.

My kernel dropdown is back to the default kernels:

![Azure Data Studio with the default kernels list]({{site.baseurl}}/assets/images/posts/2021-10-13/ads-kernels-default-list.png)

**Default Azure Data Studio kernels**: SQL, PySpark, `Spark | Scala`, `Spark | R`, Python 3, and PowerShell

Where did my R go?

Hmm… being a software developer, I suspect they are loading default values in the kernel dropdown and aren't actually using the Jupyter kernelspec list.

## Reloading the Kernel List

Let's force a dropdown kernel reload, as we know that “Changing kernel…” appears when we select a new value. I'm changing this to PowerShell. (Why PowerShell? Because I wrote a book on it long ago! It still holds a special place in my heart!)

Ah-ha! There's my R kernel!

![Azure Data Studio kernels – now with the kernels from the Jupyter kernelspec list]({{site.baseurl}}/assets/images/posts/2021-10-13/ads-kernels-updated.png)

So if you run into issues where your added kernel isn't in the default list, change the option in the dropdown so that it is forced to refresh the kernel list.

Time to check if Azure Data Studio has a bug already reported or file a new one! Did you know they're on GitHub? Check out [the Azure Data Studio repo](https://github.com/Microsoft/azuredatastudio/)!

Edited:

As for this problem, I submitted an issue: [Notebook kernel list: not showing kernels in kernelspec for new notebooks · Issue #17351 · microsoft/azuredatastudio (github.com)](https://github.com/microsoft/azuredatastudio/issues/17351)