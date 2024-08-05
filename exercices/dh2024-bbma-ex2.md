## OpenRefine reconciliation with Biblissima - Tutorial 2 

<br />

This tutorial will walk you through some possible steps to improve the results of a first iteration in a reconciliation process with OpenRefine. 

This  work is licenced under a [Creative Commons Attribution 4.0 International](http://creativecommons.org/licenses/by/4.0) Licence.

**Context**
This tutorial was written by Emmanuelle Morlock, with the collaboration of the technical Team of the projet Biblissima (Kevin Bois, Eduard Frunzeanu and Régis Robineau) to serve as a handout during this [DH2024 workshop](https://dh2024.adho.org/program/workshops/). 
It's the second tutorial of the series of 3.

**OpenRefine** is a powerfull and user-friendly opensource software for cleaning or transforming data and extending it with web services. 

**Reconciliation services** allow you to look up terms in a local dataset against external services and reuse the data from these external datasets in combination with your data. The exercices in this tutorial will be using the reconciliation service of **data.biblissima**, a database of  semantic authorities in the domain of ancient written heritage. Since **data.biblissima** uses the same database management system as Wikidata (Wikibase) it offers a very similar reconciliation service (more information on the service via this list of reconciliations services presented on the Openrefine online documentation).

**In this Tutorial n°2, you will learn**:
- How to improve the result of a previous reconciliation iteration with the data.biblissima's reconciliation service.

**Requirements for this tutorial**:
- Install **OpenRefine** on your operating system from the [official website](https://openrefine.org/download.html) then proceed to installation following the []()[]()[instructions](https://openrefine.org/docs/manual/installing). The Java Runtime Environment (JRE) is required.
- This tutorial was designed using OpenRefine 3.8.2 but it should be compatible with other supported versions.
- As input file we use an CSV (comma separated value) file called `biblissima_cleaned.csv'.

<br />
<br />

## Step 1 - Create or open a project

For this tutorial you will either open the project created with tutorial n°1 and create a new project with the file called `biblissima-cleaned.csv'.

The following steps present different ways you can try improving the reconciliation results, starting with the most simple. But there is no obligation to apply them in that order. 

<br />
<br />

## Step 2 - Reconcile a column againts a specific type 

As our data mixes different category of entities (persons, places, descriptors and shelfmarks), we chose the option "Reconcile against no particular type" for the first iteration. Now we will try to use facets to reconcile for each type of name.

1. Start with duplicating the "Name" column to keep the original data, as we did in the first tutorial

2. Use a textual facet to group the rows by type and reconcile each type one after the other:
   
> Create a text facet on the 'Type' column

> Display the Facet / Filter tab in the left part of the screen

> Click on an item in the Type facet (e.g. "Descripteur" – "Descriptor" in English)

> Reconcile against the corresponding type.

For comparison purposes, you will dedicate a specific column for each reconciliation by type.In particular, see how the 'cardinal' value was processed with different types. For example, compare the results with a reconciliation as a descriptor (Q304387) or as a human being (Q168).

The page about entity types [Project:types d'entités](https://data.biblissima.fr/w/Project:Types_d%27entités) on data.biblissima gives you the list of available entity types. Hover on the labels to get the QID if the auto-completion is not sufficient.

Note: 

> you don't have to limit yourself to the types detected by OpenRefine. You can indicate any other category existing in the schema. To reconcile against another category, enter its name or QID in the field "Reconcile against type:"

<br />
<br />

## Step 3 - Reconcile with indication of additional columns in the source dataset

When your dataset contains pieces of information that can help disambiguate the entities, it's recommended to have OpenRefine take them into account in reconciliation process. You just need to map the other columns with the schema of the reconciliation service. 

Typically, the birth and death dates are very helpful to reconcile person names. 

Test it on our example dataset:

> Use the type facet 'person' to navigate to the list of person names

> Duplicate the label column and name it "persons with dates" for example

> Start a new reconciliation iteration

> Type "date de naissance " in the field next to the column name "YearBirth" in the right halft of the window and select suggested value (the one with the "P57" property code)

> Do the same for the death date and map the column "YearDeath" with the value "Date de mort P62"

> Start the reconciliation

> Observe that all the names with birth and death dates were automatically matched this time

<br />
<br />

## Step 4 - Correct or amend the data

Sometimes the most efficient thing to do is to modify the entity names you want to reconcile. 

1. This can be done manually in the dialog box that opens when you click on the "edit" button displayed when you hover on the cell. For example, if you corrected the label "Christine de Piza, 1364?-1430?" with the string ("Christine de Pizan"), it will be reconciled with a score of 100.
   
2. The modification can be done on the fly for the entire dataset with an expression. Expressions can be written in Openrefine's own GREL language (_Google Refine Expression Language, or General Refine Expression Language_), in Jython (Python's implementation in Java) or in Clojure. GREL is easier to use for simple use cases. 

For example, if you want to delete the dates in the labels, you can use a regular expression in a cell transformation of the label column:

> Edit cells ---> Transform...

> Enter the regular expresion: `value.replace(/\((?<=\()[^)]+(?=\))\)/,'')`

> Click OK

> Edit cells ---> Common transforms ---> Trim leading and trailing whitespace

> Reconcile ---> Start reconciling…
   
Alternatively, you could proceed in steps with the menu commands:

> Edit columns ---> Split into several columns

> Type `(` as a separator

> This creates two colums. Delete the second one and transform the first with:

>  Edit cells ---> Common transforms ---> Trim leading and trailing whitespace

> Then reconcile again.

<br />
<br />

## Step 5 - Alternative methods for checking and validating the results

This manual way of processing to validate the results of a reconciliation iteration is only possible when you deal with a small dataset. For obvious reasons, if you have to deal with a dataset of hundreds or thousands or rows, it's not the way to go. 

OpenRefine offers you alternative commands to perform bulk changes. These are accessible in the submenu "Action" of the "Reconcile" menu entry. 

To experiment with this functionality on a larger dataset, you might want to "Match each cell with its best candidate" or using facets, decide that only cells with a score over 80 % can be matched automatically, the rest having to be checked and validated manually. 

Do do that you will use the slider in the facets tab that lets you target the threshold.

