---
title: Getting an R Jupyter Kernel in Azure Data Studio
---

Over the years, I've been hooked on Jupyter Notebooks. I've spoken on Azure Notebooks (rest in peace), and have had a lot of fun writing data curriculum using Python notebooks in Azure Data Studio. Now that I'm back to working for myself, I'm taking time to get more learning in and resharpen some of my skills I haven't used in awhile.

## Installed Tools

On my machine, I have the following tools installed:

- [RStudio](https://www.rstudio.com/)
- [Azure Data Studio](https://azure.microsoft.com/en-us/services/developer-tools/data-studio/#overview)
- [Python 3.9 on Windows 10](https://www.microsoft.com/en-us/p/python-39/9p7qfqmjrfp7?activetab=pivot:overviewtab)
- [JupyterLab](https://jupyter.org/install)

## Checking Jupyter Kernels

I installed JupyterLab specifically because I didn't see any Jupyter kernel information in the settings in Azure Data Studio. Once I installed JupyterLab, I now had access to the jupyter commands.

To check which kernels are installed, run the following command:

`jupyter kernelspec list`

My output looked like this:

```
Available kernels: python3, powershell, pysparkkernel, sparkkernel, and sparkrkernel
Jupyter kernels installed: python3, powershell, pysparkkernel, sparkkernel, sparkrkernel
```

I was frustrated that there wasn't a simple R kernel for me to work with locally. But wait… there's hope.

## Installing the R Jupyter Kernel

In order to install the R Jupyter kernel, I had to open up RStudio. As long as you have access to an R console, you can follow along. The R Jupyter kernel is in the IRkernel package. You can install it from CRAN with the following command:

`install.packages('IRkernel')`

## Add the R Jupyter Kernel to Kernelspec

Once you install the R Jupyter kernel, then you can add it to the kernelspec with the following command – still in RStudio or at an R console:

`IRkernel::installspec(user = FALSE)`

## Checking the Kernelspec

Once the R Jupyter kernel is added to the kernelspec, open a terminal and run the following command:

`jupyter kernelspec list`

Now, R shows up in my list:

```
Jupyter kernels installed include: python3, powershell, pysparkkernel, sparkkernel, sparkrkernel, ir
Jupyter kernels installed: python3, powershell, pysparkkernel, sparkkernel, sparkrkernel, ir
```

## Checking the Kernels List in Azure Data Studio

Now, open a Jupyter notebook in Azure Data Studio. Click the dropdown next to kernel, and you should see the R kernel in the list:

![Screenshot of the Kernel dropdown in a Jupyter notebook in Azure Data Studio]({{site.baseurl}}/assets/images/posts/2021-10-13/azure-data-studio-kernel-list.png)

The Azure Data Studio Kernel dropdown – including the R kernel
And now I'm back off to DataCamp and working through some of their R courses!