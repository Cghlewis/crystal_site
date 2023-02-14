---
title: "Creating a data cleaning workflow"
subtitle: 
excerpt: "Part 2 in a series on creating a data cleaning workflow"
author: "Crystal Lewis"
date: "2023-02-15"
draft: true
# layout options: single, single-sidebar
layout: single
categories:
- tutorials
tags:
- data cleaning
- rstats
- data management
- data sharing
---


## Data cleaning workflow

Data cleaning is the process of organizing and transforming raw data into a format that can be easily accessed and analyzed. In education research, we are often cleaning data collected in the field from methods such as surveys, assessments, tests, or forms. 

Even despite our best efforts to collect organized data, there is most likely at least **some** cleaning that is needed before you have a dataset that is ready to be shared within or outside of your team. And oftentimes, we want our cleaning process to be standardized, reproducible, and reliable. Yet, despite our best intentions, many times we (myself included) end up cleaning data in a somewhat haphazard way, leading to more work after the cleaning process, having to organize our messy work so that others can understand what we did. So how do we create data cleaning workflows that are standardized, reproducible, and produce reliable data? I have a few ideas that can help and I will share those ideas in this post.

## Preliminary steps for standardization

Before you dive into data cleaning, there are several preliminary steps you need to take. All of these steps will help standardize your process as well as your output. Those steps are as follows:

1. Have your data dictionary created  
    - Have your variables named and coded according to your style guide  
2. Have your data cleaning plan written  
    - Review the plan with your team  
3. Review any readme files that have been provided to you  
    - Incorporate those into your data cleaning plan  
4. Have your folder structures set up according to your style guide  
5. Have your file names set up according to your style guide  

So let's briefly review the documents mentioned in these steps.

### Data dictionary

A [data dictionary](https://datamgmtinedresearch.com/documentation.html#data-dictionary) is a rectangular format collection of names, definitions, and attributes about variables in a dataset and it is the document where you lay out exactly how you expect your data to be structured. This document should tell you what values variables are allowed to have, their assigned variable names, allowable variable types and more. It will inform your entire data cleaning process.

### Data cleaning plan

I have written a lot about data cleaning plans in many other [places](https://datamgmtinedresearch.com/documentation.html#data-cleaning-plan), but in a nutshell, a data cleaning plan is a written proposal outlining how you plan to transform your raw data into clean, usable data. This document contains no code and is not technical skills dependent. It is helpful to create these before you ever clean your data so that you can share these with collaborators and get consensus on planned transformations.

You may be wondering at this point, what cleaning steps should I write into my data cleaning plan then? Of course, that is going to depend on the actual data you are working with. The first thing you should do before writing a data cleaning plan is:

1. Export sample raw data
    - You need to see what variables exist, how they are named, what format they are in, what values are exported  
2. Compare the raw data to your data dictionary
    - Remember from a previous [blog post](https://cghlewis.com/blog/survey_data/), hopefully you designed your data collection instrument according to your data dictionary. If you did, very little transformation should be required. However, if you did not, start to compare the differences between what you expect your variables to look like and what the actual raw data looks like. This will be the foundation for writing your data cleaning plan.
  
After this, you can then use the standardized [data cleaning checklist]() we discussed in the previous post, to guide you as you think through what transformations are required to create a clean dataset for the purposes of general data sharing. Add the steps that are relevant to your data and remove the steps that are not relevant. Remember, the order of the steps are fluid and can be reorganized as needed.

### Readme files

[Readme files](https://datamgmtinedresearch.com/documentation.html#readme) are also plain text files that collaborators may store in a folder, alongside a raw data file. These files contain notes that may be relevant to your data cleaning process. Maybe there was an error that occurred during data collection that they need you to fix in the cleaning process. For instance, a collaborator may note that "ID 1234 should actually be ID 1235". 

### Style guide

Last, a [style guide](https://cghlewis.github.io/mpsi-data-training/training_3.html) is a set of standards for how your team should organize data for your project. This document should tell you exactly how to structure project folders and name project files in a way that promotes reproducibility in your work. Naming files and folders in a consistent way allows your process to be more reproducible. For example, always naming raw files with the suffix "_raw" and clean files with the suffix "_clean" allows you to programmatically grab files based on file metadata.

This guide also informs how you name and code variables and will be used as you construct your data dictionary.

## Cleaning data using reproducible practices

Once you are ready to begin actually cleaning your data, there are several steps you can take to improve the reproducibility of your work.

1. Use code
    - While you **can** clean data through a point and click method in a program like SPSS or Excel, cleaning data manually is typically not reproducible, leads to errors, and is time consuming. 
    - My recommendation is to clean your data, no matter how small the task, with syntax. Don't do **any** transformations outside of code (even if you think it is something small). Once you do, your chain of processing is lost and your process is no longer reproducible.
    - Syntax files allow you to have a record of every transformation you make from the raw data to your clean data. While writing syntax may seem time consuming up front, it helps you to be more thoughtful in your data cleaning process and it can save you an enormous amount of time in the future if you plan to clean data for the same form multiple times (in say a longitudinal study).
2. Use comments
    - Code comments help you to organize and communicate your thought process. While your syntax may seem intuitive to you, it is not necessarily clear to others. As you clean your data according to your data cleaning plan, comment every step in your syntax, explaining what that specific line of code is doing.
3. Follow a coding style guide
    - Creating a [code style guide](http://adv-r.had.co.nz/Style.html) for your team/project ensures that all team members are setting up their files in a consistent manner. This reduces the variation across code files and allows your code to be more usable by others. It is even possible to create code [templates](https://timfarewell.co.uk/my-r-script-header-template/) that team members can use to create even more standardization.
4. Use relative file paths
    - Setting absolute file paths in our syntax reduces the reproducibility of our code because future users will have a different file path. Set your file paths relative the directory you are working in.
5. Don't do anything random
    - Everything in your syntax must be replicable. Yet, there are a few scenarios where, without much thought, you could be producing random results.
      1. If you randomly generate any numbers in your data (like study IDs) then you need to set a seed. For instance, in R, using the `set.seed()` function in your syntax means when you or someone else runs your syntax again to randomly generate your numbers, they will get the same set of random numbers. Otherwise, you will get a new random set of IDs each time your syntax is run.
      2. Another example is when you are removing duplicates. Be purposeful about how you remove those duplicates and order your data in a specific way first. If someone at some point shuffles your raw data around, and you re-run your syntax, you may end up dropping a different duplicate if you did not arrange your data first.
6. Record your session info
    - In a program like R, you can record your session information in your syntax, or into a plain text file, so that future users can review the requirements needed for running your code. Information about package versions and operating system used are recorded. If users run into errors running your code, this information may help them troubleshoot. 
    
## Cleaning data using reliable practices

There are several practices you can implement that will help you have more confidence in your data cleaning process.

1. Review your data upon import
    - We talked about ways to validate data in our previous [post](). And especially when you are reusing a syntax to clean data collected multiple times (for instance, in a longitudinal study), it can be very helpful to validate that your incoming raw data is still as you expect it to be. I've been a part of many studies where I assumed that forms did not change over time, but found out later on in the cleaning process that items had been added, variable names changed, or even the allowable values had been shifted. It's best to find this out before you start the cleaning process so you can adjust your code as needed. Otherwise you may be running a script that is incorrectly cleaning your data.
2. Write functions for repeatable tasks
    - As the sayings go, "Don't repeat yourself" or "Never write the same code twice". This can be a really difficult habit to break, especially for people who are fairly new to code writing. But there are reasons that, once you are comfortable with writing some basic functions, to move your 10 lines of code that you multiple times throughout your script, into a one line function that you can call throughout your script.
    - Not only does it make your script more readable, but it reduces the errors that might be created through things like copy and paste.
    - Similarly, find ways to automate some of your tasks. Rather than renaming all of your variables by hand, use your data dictionary to [automate tasks]() like this. This not only increases efficiency but also reduces mistake you might make when typing out variable names.
3. Check each transformation
  - While it is very important to validate your data before you export it, it is just as important to check your work along the way as well. For each transformation in your data, consider the following:
    1. Review your data/variables before and after the transformations
    2. Review all errors/warning codes
      - Some warnings may be innocuous (just messages)
      - Some errors are telling you that your code did not run, you need to fix something
      - Other warnings are telling you that your code **did** run but it did not run as you expected it to. These are the scariest to me. If you don't pay attention to these warnings, you may end up with unexpected results.
4. Validate your data before exporting 
    - As we discussed in the previous [post](), this is when you will want to run through your final list of all checks to make sure no mistakes exist in the data before you export it.
5. Version your data AND code and keep change logs
    - It is inevitable that at some point you will find an error in your data and/or your code. However, once you've shared your data and code with others, it will be imperative that you do not save over existing versions of those files. Versioning your files and keeping track of those different versions in a [changelog](https://datamgmtinedresearch.com/documentation.html#changelog) allow you to track data lineage, allowing users to understand where the data originated as well as all transformations made to the data.
6. Do code review
    - If you have the luxury of having more than one person on your team who understands code, I highly recommend to incorporate [code review](https://docs.google.com/document/d/1cqrRDEYkhtZT9QRruZmUnPPN00VEo0fKV7cEyoluMnU/mobilebasic) into your workflow. This is the process of having someone other than yourself, review your code for things such as the readability, usability, and efficiency of code. Through code review it's possible to catch errors you were not aware of, to make your code more efficient, and to improve the usability of your code.
    
Similar to what I mentioned in the previous post, this is not a catch-all list of practices. These are practices that I have found, through my own work and by learning from others, has helped me improve my data cleaning workflow in a way that produces data in a more standardized, reproducible, reliable way. As always, I am open to comments and feedback! In the last post of this series, we will use the lessons from these last two posts to implement a data cleaning workflow for an example education research dataset to see how this might work in a real scenario.



