# OpenRefine reconciliation with Biblissima - Tutorial 1

<br />

This tutorial will walk you through the basic steps of a reconciliation process with OpenRefine. It was written by Emmanuelle Morlock, with the collaboration of the technical Team of the projet Biblissima (Kevin Bois, Eduard Frunzeanu and Régis Robineau) to serve as a handout during this [DH2024 workshop](https://dh2024.adho.org/program/workshops/). 

It's the first tutorial in a series of 3.

This  work is licenced under a [Creative Commons Attribution 4.0 International](http://creativecommons.org/licenses/by/4.0) Licence.

**OpenRefine** is a powerfull and user-friendly opensource software for cleaning or transforming data and extending it with web services. 

**Reconciliation services** allow you to look up terms in a local dataset against external services and reuse the data from these external datasets in combination with your data. The exercices in this tutorial will be using the reconciliation service of **data.biblissima**, a database of  semantic authorities in the domain of ancient written heritage. Since **data.biblissima** uses the same database management system as Wikidata (Wikibase) it offers a very similar reconciliation service (more information on the service via this list of reconciliations services presented on the Openrefine online documentation).

**In this Tutorial n° 1, you will learn**:
- Import a dataset in OpenRefine and create a project;
- Use "facets" to explore a dataset and get global view on the data from specific criterias;
- Use some pre-defined menu commands for cleaning and normalising data;
- Reconcile a given dataset with the data.biblissima's reconciliation service.

**Requirements for this tutorial**:
- Install **OpenRefine** on your operating system from the [official website](https://openrefine.org/download.html) then proceed to installation following the []()[]()[instructions](https://openrefine.org/docs/manual/installing). The Java Runtime Environment (JRE) is required.
- This tutorial was designed using OpenRefine 3.8.2 but it should be compatible with other supported versions.
- As input file we use an CSV (comma separated value) file called `biblissima.csv`.

<br />
<br />

## Step 1 – Create a project in OpenRefine

The fist step is to create a projet with a file called `biblissima.csv`that you can download from this Github project. As using data from external services can be relatively slow, we deliberately chose a very small dataset of 16 rows. 

1. Start OpenRefine: its interface loads automatically in a new window of your default web browser. You can also point at the home screen with: *http://localhost:3333*.
2. In the *Create project tab*, select the file `biblissima.csv` and click on `Next`.
3. Ask OpenRefine to parse data as `CSV / TSV/ separator-based files` if it didn't recognize the format and check in the preview that the values were recognized properly.

5. Change the project name if you wish (you can do that later).
6. Then click on '`Create project`.

The first 10 rows are displayed on the screen.
Now we will try to prepare the data so that we can get the best possible results in the reconciliation process. In particular, we will pay attention on names as they correspond to the category of data we will use to align or dataset with data.biblissima. 

<br />
<br />

## Step 2 – Prepare the data


Browsing through the pages in a small dataset of less than 25 rows might be enough to get a global view on the data and some spot some things that might be done to clean inconsistent or corrupted data. But you can also create "facets"grouping togther rows corresponding to a common criteria and use it to navigate very quickly through the dataset. Please note the facets in OpenRefine have many other advanced uses. For now, will just experiment with them using the dropdown menu of the "type" column. 

1.  Click on the arrow near the column named "Type" and select:
> Facets > Text facet

OpenRefine opens the tab called 'Facets/Filter' on the left on the screen. Each distinct value in the column is presented with a link and the number of corresponding rows. Our biblissima dataset contains 3 descriptors (or keywords), 9 persons, 2 places and 2 shelfmarks. If you click on one, the center of the screen will only display the rows with this value in the column type. 

Doing that we notice several errors or typos we'll try to correct:
- Add a title for the column of names
- Normalize whitespace (before, or inside the name values)
- Correct the type for "Paris, France

We will use the dropdown menu of the corresponding columns (click on the little left arrow near a column title to open it). 

2. Give a proper title to the column containing names:
   
> Edit column > Rename this column…
> Enter the new title in the dialog box (e.g. "Names")

3. To remove any leading or trailing whitespace in the same column, use 2 menu commands one after the other:
   
> Edit cells ---> Common transforms ---> Trim leading and trailing whitespace

> Edit cells ---> Common transforms ---> Collapse consecutive whitespace

4. You might want to sort the "Names" column by alphabetic order. Use the menu command called "Sort...". You can see on the left that the row number is unchanged. If you want to keep the sorted order for further processing, make it permanent with the "Sort" menu button that appeared at the top of the grid and select:
   
> Sort   ---> Reorder rows permanently

5. Like the wrong type for the location name "Paris, France", some corrections can only be done manually at the cell level. Just hover it and click on the "edit" button that is displayed  at the right of the cell. Then type the correct value  in the dialog box.

6. It's good practice to start by copying the column you want to reconcile. That way you keep a view on the original data without overwritten it.  Use the dropdown menu to duplicate the "Names" column and give it a name (e.g. "Names to reconcile")

> Edit column --> Add column based on this column…

> Give the new column a name (e.g. "Names to reconcile")

> Leave the term "value" in the "expression field and click "OK"

We now just need to configure the data.biblissima service before performing our first reconciliation iteration…

<br />
<br />

### Step 3 – Configure the reconciliation service

Only Wikidata in English is pre-packaged with OpenRefine. If it's the first time you use the data.biblissima reconciliation service, you need to configure it by giving the URL of its endpoint to OpenRefine.

In the dropdown menu of the duplicated column ("Names to reconcile" if you chose that title) select:

> Reconcile --> Start reconciling

> Add a standard service

> Enter the URL of the endpoint: `https://data.biblissima.fr/reconcile/fr/api`
   
The service is set up and can be selected. 

Note that you could add other  [reconciliation services working with OpenRefine](https://reconciliation-api.github.io/testbench/#/) or Wikidata in another language. 

<br />
<br />

### Step 4 – Reconcile with data.biblissima


1. Choose the Biblissima reconciliation service and click "Next"

2. Like Wikidata, the Biblissima reconciliation service tries identify the most appropriate category of data to reconcilie against. This can help you get better results and avoid errors due to homonymy (when the same term can occur in two different context like "Virginia" which can stand for a female's first name or a state in the american continent. Since our data mixes different categories, we will select the option: "Reconcile against no particular type". 

3. The window also and lets choose whether you accept automatic match or prefer to validate every match manually, even if the score is high. It's generally recommended to use auto-matching with care, but for this exercise, we will choose to keep the option "Auto-match candidates with high confidence" checked. Then click "Start reconciliation".

4. Once the reconciliation is completed, each (non blank cell) cell in the column will display the result of the matching process.

- If the auto-match was successful, it displays a single blue line.
- If there is no high confidence for the match, the cell will display all one or more possible candidates, together with a score inside parentheses.
- If there is zero match, the cells suggest the creation of a new item.

5. The new values are in fact hyperlinks pointing to data.biblissima. When the auto-match was not performed, you will need to select manually the correct one. You can also search for a better one if no one in the list seems relevant.

- A click on the button with one checkmark will match this cell only.
- A click on the button with 2 checkmarks will match all identical cells.
- To help you in the validation process, you can hover a hyperlink get some related information about it, depending on how the reconciliation service implemented the feature. Biblissima shows at the identifier (QID) and a link to the record in its Wikibase instance.
- Don't forget to click on "See more" to display the complete list of candidates when there are more than 3 candidates.

> For example for the label "Augustine", you might want to choose "Augustin (saint, 0354-0430)"

4. When no candidate is suggested, or if the candidates or the matches are all wrong,  you can do a search in the database. You can either use the autocompletion functionality or enter the identifier of an entity you know. 

For example the descriptor name "chevaliers" (Knights) was matched with the term 'Cavaliers'. If you click on the link to the database, you see that it is not at all a descriptor the title of a work (a comedy written by Aristophane). 

> You can click on the button "Choose new match": you'll see  a list of candidates appear in the cell. But as no one corresponds to a descriptor, you can do click on the button "Search for match" and use autocompletion or the entry of the QID identifier you found in a more efficient search in the Biblissima Portal: e.g. "Q295000" .
   
Of course, the notion what is a "right" or "wrong" match cannot is relative. It depends on the faculty of the editor to interpret the data and the accuracy rate she wishes to obtain in the matching. For example, the descriptor "jeux olympiques" (olympic games) was aligned to the Biblissima descriptor "Jeux olympiques antiques" (antic olympic games). Depending on the task and your understanding of the data, you might want to accept or reject this match...






