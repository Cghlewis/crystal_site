---
title: "Building a simple relational database in Excel"
subtitle: 
excerpt: "Beginner guidance for setting up a simple participant tracking database in Microsoft Excel."
author: "Crystal Lewis"
date: "2024-11-04"
draft: true
# layout options: single, single-sidebar
layout: single
categories:
- tutorials
tags:
- data management
- Excel
---

When running a research study that involves data collection from human participants, or any data that is collected in the field, it's vitally important to find some way to track incoming information on participants and their data. Oftentimes, researchers think they can simply keep track of incoming data in their head, but time and time again, I've seen researchers lose data because of this method (they think they had a form, but then figure out later it was never collected). Instead, it's important to build an internal system where, on an ongoing basis, you record necessary participant information (e.g., name, contact information, assigned unique study identifier), as well as track all necessary information (e.g., consent status, data collection forms collected, participant payment sent, withdrawn status). Building this kind of formal system for tracking incoming information has many benefits including:

- Acting as a single source of truth for participant record keeping
- Tracking and reporting ongoing data collection efforts (reducing chances of lost data)
- Serving as a linking key between participant identifiable information and their true identity

For anyone who has read my [book](https://datamgmtinedresearch.com/), I devote almost an entire [chapter](https://datamgmtinedresearch.com/track) to discussing the benefits of keeping a participant database and reasons why tools that allow you to build relational databases (e.g., Microsoft Access, FileMaker), as opposed to spreadsheet tools like Microsoft Excel, are often the best tool to use for this tracking system. I list reasons such as the ability to link information across tables reducing duplication, the ability to set up data validation to reduce data entry errors, as well as the ability to query data as needed for various reporting purposes. However, there are times where a research team may not have access to database tools, they may not have the expertise to build relational databases, or a study is small enough where it doesn't necessitate the time it takes to build up a sophisticated database. In these cases, researchers often resort to using a spreadsheet tool like Microsoft Excel for participant tracking, and that's okay! In this blog post I hope to share ways that you can build a simple participant tracking database in Excel, and still get many of the same benefits you can get from a database.

## The Excel workbook

Let's say we have a hypothetical research study where we are collecting data from students, teachers, and schools. In the case of an Excel workbook, I am going to set up one tab, or sheet, for each of these entities. This study is going to be very simple and we are only collecting data from our participants once. We are collecting a consent form and an observation from students, and we are collecting a consent form and a survey from teachers. I also need to assign unique study identifiers to schools, teachers, and students for confidentiality purposes. Each tab in our workbook will look something like Figure 1, which shows what fields will be entered on each sheet.


<div class="figure" style="text-align: center">
<img src="img/fig1.PNG" alt="The fields being collected in each sheet of our workbook" width="533" />
<p class="caption">Figure 1: The fields being collected in each sheet of our workbook</p>
</div>

In the next three sections, we are going to review three ways we can improve the use of this tracking system by adding data validation, relating information across sheets, and creating a reporting system, similar to what we might see when using a database tool.

## Entering data

As you begin receiving information about your participants, it's important for team members to have a system for adding this information into your participant tracking system, allowing you to have a single source of truth about who is in your study and what data has been collected on them. Sometimes that responsibility may be designated to one person, othertimes to several people. No matter what, it's important to reduce data entry errors to ensure that you have accurate, usable information.

One of the nice things about many database systems is the ability to set up thorough data validation, as well as the ability to enter data into forms, rather than spreadsheet view, reducing data entry errors. In this post I'm not going to cover entering data into forms, although you can certainly set this up with programs like Google forms and Google sheets, and most likely you are able to do something similar with Microsoft tools as well. For now, we are going to keep things simple and just focus on how we can reduce data entry errors in Excel through validation.

By validation I mean restricting data entry to an allowable set of values. [Simple data validation](https://support.microsoft.com/en-us/office/apply-data-validation-to-cells-29fecbcc-d1b9-42c1-9d76-eff3ce5f7249) is fairly easy to set up in Excel. First select the column you want to apply your rules to. Then, go to "Data" -> "Data Validation" -> "Data Validation".

<div class="figure" style="text-align: center">
<img src="img/fig2.PNG" alt="Data validation in Excel" width="597" />
<p class="caption">Figure 2: Data validation in Excel</p>
</div>

Then, you can start applying rules for each variable. For instance, if I chose `stu_id`, I know that these values should be numeric and should fall within the range of 1400 -- 1500. So I will apply those rules to this column.

<div class="figure" style="text-align: center">
<img src="img/fig3.PNG" alt="Applying data validation rules to the stu_id column" width="520" />
<p class="caption">Figure 3: Applying data validation rules to the stu_id column</p>
</div>

Once rules are applied, if someone tries to enter a value outside of that range, or a value that is not numeric, they will get an error. 

<div class="figure" style="text-align: center">
<img src="img/fig4.PNG" alt="Getting an error when entering an unacceptable value" width="557" />
<p class="caption">Figure 4: Getting an error when entering an unacceptable value</p>
</div>

Let's apply validation using a list of values. We can do this for our `s_obs_complete` field.

Here we select "List" as our "Allow" value, and we type our options in the "Source" box, with each option separated by a comma.

<div class="figure" style="text-align: center">
<img src="img/fig5.PNG" alt="Applying a validation list of values" width="562" />
<p class="caption">Figure 5: Applying a validation list of values</p>
</div>

Once you are done, you are left with a drop-down option for this column. This reduces any chances that the word "yes" is written as "YES", or "Yes", or "Y", reducing your ability to categorize information. 

<div class="figure" style="text-align: center">
<img src="img/fig6.PNG" alt="A drop-down list of values" width="390" />
<p class="caption">Figure 6: A drop-down list of values</p>
</div>


## Linking sheets

One of the biggest pros about building relational databases is that you are able to link data across tables through [primary and foreign keys](https://datamgmtinedresearch.com/structure#structure-link) (see Figure 7 where primary keys are in rectangles and foreign keys are in ovals). Relating tables in this way, allows us to pull information across tables, reducing duplication. In Figure 7, you can see that in the "student" tab, we're only entering student information (with exception of `tch_id`). However, if I wanted my team to see the teacher first and last name while looking at the student tab, not just the `tch_id`, I could simply generate a query to pull that information from the teacher table through the `tch_id` key. This relation removes the need for us to have to type the teacher first and last name over and over for every student in the teacher table, reducing both effort and error.

<div class="figure" style="text-align: center">
<img src="img/fig7.PNG" alt="Participant database built using a relational model" width="532" />
<p class="caption">Figure 7: Participant database built using a relational model</p>
</div>


While a relational database is built for this type of linking, the [XLOOKUP function](https://support.microsoft.com/en-us/office/xlookup-function-b7fd680e-6d10-43e6-84f9-88eae8bf5929) in Excel can help us emulate this behavior. 

Say for instance, I want to be able to see the school name alongside the `sch_id` in our "school" sheet. See our example sheets in Figures 8 and 9.

<div class="figure" style="text-align: center">
<img src="img/fig8.PNG" alt="Teacher tab in Excel" width="378" />
<p class="caption">Figure 8: Teacher tab in Excel</p>
</div>

<div class="figure" style="text-align: center">
<img src="img/fig9.PNG" alt="School tab in Excel" width="450" />
<p class="caption">Figure 9: School tab in Excel</p>
</div>

Rather than re-entering the school name on the "teacher" tab, I can use `XLOOKUP` to pull this information from the "school" tab into the "teacher" tab.

When we enter `=XLOOKUP()` in to the first cell, a series of arguments will pop up. The arguments are:

- lookup_value (In this case, choose the first `sch_id` value on the "teacher" tab)
- lookup_array (In this case, the `sch_id` values on the "school" tab)
- return_array (In this case, the `sch_name` column on the "teacher" tab)
- There are 3 other optional arguments that we will not worry about right now.

<div class="figure" style="text-align: center">
<img src="img/fig10.PNG" alt="Using XLOOKUP in Excel" width="540" />
<p class="caption">Figure 10: Using XLOOKUP in Excel</p>
</div>

If we fill in the arguments in the first cell as such,

`=XLOOKUP(D2,school!A2:A3,school!B2:B3)`

We should get the correct value returned from the school table.

<div class="figure" style="text-align: center">
<img src="img/fig11.PNG" alt="School name returned using XLOOKUP" width="435" />
<p class="caption">Figure 11: School name returned using XLOOKUP</p>
</div>

We can now [copy this formula by dragging it down through the remaining cells](https://support.microsoft.com/en-us/office/copy-a-formula-by-dragging-the-fill-handle-in-excel-for-mac-dd928259-622b-473f-9a33-83aa1a63e218), but first we need to tell Excel which values need to stay consistent, and we can do this using `$`.

`=XLOOKUP(D2,school!$A$2:$A$3,school!$B$2:$B$3)`

We could then use this same functionality to do things like bring teacher names and grade level into the "student" tab using the `tch_id`. What's great is, if a teacher name is changed at any point, when you change it in the teacher tab, it will then be updated in the "student" tab as well. While this was a fairly simple example that we could have easily manually typed across tabs, you can see how using this functionality can save us tons of time if there was a much larger sample. 

No matter the size of the sample though, this linking of information still reduces duplication and keeps one single source of truth. Imagine a scenario where you manually type a teacher name wrong in one of the tabs. How do you know which spelling is correct? There's no longer one source of truth, there are multiple.

**Note:** I originally assumed we could do this kind of linking using `VLOOKUP` as well, but it turns out the `VLOOKUP` is less flexible, requiring a specific ordering to your variables, as well as specific sorting. Yet, when working with a participant tracking database, the order of variables and the sorting of variables may change, so `VLOOKUP` will tend to break, `XLOOKUP` a more reliable option.


## Pulling reports

Another benefit of working with relational databases is that you have tons of flexibility in how you can combine and summarize information across tables. While that full range of flexibility may not be possible in Excel, [pivot tables](https://support.microsoft.com/en-us/office/create-a-pivottable-to-analyze-worksheet-data-a9a84538-bfe9-40a9-a8e9-f99134456576) still provide an excellent way to obtain necessary information for project coordination and reporting purposes. Once you've linked relevant information across sheets, you can now select a sheet to do a pivot table on. For our example, we will do a pivot table on the "student" sheet.


<div class="figure" style="text-align: center">
<img src="img/fig12.PNG" alt="Starting a pivot table" width="414" />
<p class="caption">Figure 12: Starting a pivot table</p>
</div>

Select the range of your data and select "New Worksheet".

What's nice about creating pivot tables in other worksheets is that it reduces errors that might happen if you are sorting and filtering directly in your tracking sheets. 

<div class="figure" style="text-align: center">
<img src="img/fig13.PNG" alt="Selecting pivot table options" width="703" />
<p class="caption">Figure 13: Selecting pivot table options</p>
</div>

One you select "OK", you can begin to build your query. You will first want to drag your primary key (unique identifier) into the "Values" field. Make sure to change the Value Field Settings to "Count" rather than "Sum" (see Figure 14). 

<div class="figure" style="text-align: center">
<img src="img/fig14.PNG" alt="Changing Value Field Settings" width="418" />
<p class="caption">Figure 14: Changing Value Field Settings</p>
</div>

From there you can begin dragging other variables of interest to Columns, Rows, or Filters. For instance, I want to see the status of observation completion per classroom. But I only want to know this for students who have not withdrawn from the study. To get this I could drag `tch_id` to Columns, `s_obs_complete` to Rows, and `s_withdrawn` to Filters. Then filter to "no" on `s_withdrawn` and I would get something like Figure 15.

<div class="figure" style="text-align: center">
<img src="img/fig15.PNG" alt="A pivot table to show the completion of observations by classroom" width="584" />
<p class="caption">Figure 15: A pivot table to show the completion of observations by classroom</p>
</div>


## Final thoughts

First, I want to reiterate that I still think building this type of tracking system in a relational database is often the most efficient way, especially the larger your study gets. Relational database systems are typically more intuitive and more flexible for building these types of relationships. While we can kind of hack our way to something similar in Excel (or other spreadsheet systems), it's not quite the same.

Second, this is not a full manual for building a perfect data entry and tracking system in Excel. If you plan to build this kind of system in Excel, this blog is here to get you thinking about ways you can hopefully make that system less error-prone, more efficient, and more useful. From there, continue adding on any other elements that may help you reduce errors or make this a more useful system. Consider things like how you will secure this database, limiting access to the private information contained in it. You will also want to consider how will reduce accidental deletion or overwriting of information by users (Eeeek!).

Last, I am not an Excel or database expert. Building these types of tools is only a small part of what I do in my work. I am just trying to pass along tips that I've learned along the way that might help researchers who are new to building these types of systems. If you are an Excel or database expert reading this, I would love to hear your comments and ideas for improving upon what I've suggested or other ways to make a more efficient system for recording this type of information.


