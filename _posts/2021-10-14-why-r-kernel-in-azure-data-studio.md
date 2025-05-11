---
title: Why R Kernel in Azure Data Studio
tags: azure-data-studio r jupyter-notebooks
---

Yesterday, I posted about [getting the R kernel in Azure Data Studio]({{site.baseurl}}/2021/10/13/getting-an-r-jupyter-kernel-in-azure-data-studio). However, I didn’t explain the “why”. Let’s talk about that.

## Why Azure Data Studio?

Over the past couple years, I’ve been working with Azure Data Studio as my primary editor for writing data curriculum. The curriculum was written in Markdown, and I knew I could write Markdown files within Azure Data Studio. I could also use it for connecting to my SQL resources and for my Python notebooks for the Python part of the curriculum. It had just about everything I needed for my data curriculum. To have as much of my data environment in one application – Azure Data Studio just makes sense.

## Why R?

I’m a polyglot programmer, one who would appear on an A&E episode of “Hoarders” if there was an episode on hoarding programming languages. I have always had an interest in programming paradigms and languages from a practical perspective. In 2018, I took courses through DataCamp for their Python and R programmer tracks – Python to refresh my skills and R to really solidify yet another language – yet another tool in my data toolbelt.

I also have an engineering background. My degree is in Computer Science and Engineering Technology. I started in the theory-based Computer Science and Engineering and transitioned to the practical-focused Computer Science and Engineering Technology program. My mind is very much engineering-oriented – analytical, curious, and creative. When I first saw R, it reminded me of stats class and memories of MATLAB and Maple. My curiosity led me to R.

## Why not the `PySpark | R` kernel?

Azure Data Studio comes with a R kernel known as `PySpark | R`. While this supports R, this also requires a connection to a server. In my case, all the data I work with is either built into R, in R packages, or in local files. I don’t need it for Big Data processing in this case. So the `PySpark | R` kernel doesn’t fit my needs.

## Why R Kernel in Azure Data Studio?

So… why do I want to use the R kernel within Azure Data Studio? Here are my reasons:

1. Azure Data Studio’s notebook experience is simplistic – not complicated – and user-friendly.
1. Azure Data Studio is a familiar interface. For developer types like me who are familiar with Visual Studio Code, Azure Data Studio makes sense because it is built on top of Code.
1. I’m refreshing my R skills by going through more DataCamp courses. I’d like to save my notes in a Jupyter notebook, and I’d like to use a friendly interface.
1. I am working with smaller datasets, built-in datasets, and local data, so the `PySpark | R` kernel doesn’t fit my needs.

This might not be a common use case, but this is my story and how I enjoy using Azure Data Studio for my data engineering environment.