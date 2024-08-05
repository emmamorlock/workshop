# 3 OpenRefine reconciliation with Biblissima - Tutorial 3 
This tutorial will walk you through some possible steps to use enrich a TEI file with the Biblissima reconciliaion service for OpenRefine. 

This  work is licenced under a [Creative Commons Attribution 4.0 International](http://creativecommons.org/licenses/by/4.0) Licence.

**Context**
This tutorial was written by Emmanuelle Morlock, with the collaboration of the technical Team of the projet Biblissima (Kevin Bois, Eduard Frunzeanu and Régis Robineau) to serve as a handout during this [DH2024 workshop](https://dh2024.adho.org/program/workshops/). 
It's the second tutorial of the series of 3.

**OpenRefine** is a powerfull and user-friendly opensource software for cleaning or transforming data and extending it with web services. 

**Reconciliation services** allow you to look up terms in a local dataset against external services and reuse the data from these external datasets in combination with your data. The exercices in this tutorial will be using the reconciliation service of **data.biblissima**, a database of  semantic authorities in the domain of ancient written heritage. Since **data.biblissima** uses the same database management system as Wikidata (Wikibase) it offers a very similar reconciliation service (more information on the service via this list of reconciliations services presented on the Openrefine online documentation).

**In this Tutorial n°3, you will learn**:
- Import directly an XML file
- Align named TEI tagged name entities with data.biblissima
- Export the data with the TEI pattern of your choice 
- Re-inject the data in the TEI file and visualize the result in a TEI Publisher instance.

**Requirements for this tutorial**:
- Install **OpenRefine** on your operating system from the [official website](https://openrefine.org/download.html) then proceed to installation following the []()[]()[instructions](https://openrefine.org/docs/manual/installing). The Java Runtime Environment (JRE) is required.
- This tutorial was designed using OpenRefine 3.8.2 but it should be compatible with other supported versions.
- As input file we use a TEI / XML  encoded  file called `biblissima-tei.xml'.


## Step 1 - Start OpenRefine and import data

1. Start OpenRefine: its interface loads automatically in a new window of your default web browser. You can also point at the home screen with: *http://localhost:3333*. In the *Create project screen*, import the XML/TEI file: `biblissima-tei.xml`.


OpenRefine detects the XML input file format automatically_ and suggests you to indicate the first XML element corresponding to the first record to load by clicking on it on the preview displayed in the main window.

> Click on the first `<name>` element: 
`<name type="person" ref="#name_gzw_vfw_2cc">John, Duke of Berry</name>`

A preview of the data extraction is displayed on the screen. 
> Click on the button 'Create project' if you are happy with it, otherwise click "Start over"


## Step 2 - Prepare data for reconciliation

First, we want to clean the data in the column containing the names to be reconciled. We will use the transform menu with pre-defined string transformations.

1. To remove any leading or trailing whitespace in the column "name", click on the arrow near the column title and select:
> Edit cells --> Common transforms --> Trim leading and trailing whitespace

2. To remove any whitespace inside the name string, select:
> Edit cells --> Common transforms --> Collapse consecutive whitespace

3. Sort the rows
    - click on the drop-down menu for the column you want to sort on, and choose `Sort`
    - a new “Sort” button appears at the top of the grid
    - as in OpenRefine are temporary (if you remove the `Sort`, the data will go back to its original ‘unordered’ state), to amend the original sort, choose Reorder Rows Permanently from the Sort drop-down menu.

4. Create text facets on the column 'name - type' to reconcile in 2 steps with types:
- 'Personne physique', 'être humain', 'human' - Q168
- 'Noms geographiques', 'Geographical location' - Q26719
- Note: You might need to connect configure the Biblissima web service first with "Add reconciliation service" See tutorial 1.

5. Terminate by matching cells "manually". Example tasks you can perform:
   > Validate the relevant propositions (use the dates in the source text to desambiguate eg Duc d'Aumale)
   > Search for new match - E.g. "Limbourg brothers Paul, Johan and Herman.": delete the first names and then validate '[De Limbourg (frères)](https://data.biblissima.fr/w/Item:Q23045) (Q23045)' 
     > Duke of Savoye : try with "Charles I" and use the dates in the text (1485-89)

6. Suppress duplicates
There are two occurences of the Limbourg brothers in the text. You can delete one using the facets after starting it.
> Star the row to delete
> All — > Facet — > Facet by star
> In the box "Star rows", choose "true"
> In the screen with only the starred row:  All — > Edit rows — > Remove matching row


## Step 3 - Enrich the dataset

First we will use the reconciliation drop down menu to extract some information collected by the reconciliation process and store it in new colums. On the reconciled column "name for reconciliation" select:

> Reconcile — > Add entity identifiers column… will create a columns with data.biblissima QIDs
> Reconcile — > Add columns with URL of matched entities… will create a direct link to the entity in data.biblissima

The real exploitation of data.biblissima will start now. 

1. Add new colums from the reconciled column with:

   > Edit column  — > Add columns from reconciled values' . Note that this menu command works only if the reconciliation service supports the data extention functionality (as data.biblissima is based on an Wikibase instance, this is the case) )

You need to search the the property you want to add, as the interface only suggests "QID". To get "inspired", just click on the link of one or two reconciled names to look at the data.biblissima entire entry. You will be able to identify interesting properties to add to your dataset. When you do that, take note of the property code of the ones you will want to get in the next step. You can also use the properties suggested below...

In that dialog box, you can either use the search field or enter the property name or code directly (to overcome the limitations of this search):
- Labels in other languages ('Lfr', 'Len')
- Alternative labels (Afr, Aen, Ade, ect.)
- Wikipedia identifier (P112)
- Birth date (P75@isodate or P57@year)
- Death date (P62@isodate or P62@year)
- Biblissima portal id (P129)
  

Now we can also extract information from the other sources these entitites are aligned to in data.biblissima.

For example, we can get the Wikipedia QID from data.biblissima. 
We'll use that identifier to reconcile against Wikipedia then. This way, we will get better results than if we had reconciled directly with Wikipedia. You can test that. 
Of course the method is replicable with any other reconciliation service data.biblissima records alignments with (ULAN, VIAF, Geonames, etc.)

1. Get the Wikidata Qid (P112) with the same method as before
   
2. Use the wikidata QId to bounce another reconciliation with Wididata:

> Reconcile --> Use values as identifiers…
> Choose the **Wikipedia** reconciliation service this time.

3. You can add several columns at the same time. For example, ask for:
- Labels in other languages (Lfr, Lde, etc.)
- images (P18)

4. Repeat the operation for places. Columns to add from data.biblissima:
       - latitude (P176)
       - longitude (P177)


## Step 4 - Prepare the data export it and insert it back in the TEI source file

First we want to create a link to the Biblissima portal by concatenating the url and the Biblissima portal identifier. On the portal identifier column, select:

> Edit column --> Add column based on this column

Then replace 'value' with the text:
> `'https://portail.biblissima.fr/ark:/43093/' + value`

2. Transform the wikipedia image column to the file link to be used in a web page

> pattern: https://upload.wikimedia.org/wikipedia/commons/f/fd/Franz%C3%B6sischer_Meister_um_1500_001.jpg (see [FAQ on wikimedia commons file paths](https://commons.wikimedia.org/wiki/Commons:FAQ#What_are_the_strangely_named_components_in_file_paths.3F)). The 'f' and 'fd' subdirectories are calculated from the file name (they correspond to the first and the first two characters of the [MD5](https://en.wikipedia.org/wiki/MD5 "en:MD5") hash of the filename – blanks replaced by underscores).

> Edit column --> Add column based on this column

> 'https://upload.wikimedia.org/wikipedia/commons/'+ substring(md5(cells["image"].value.replace(' ','_')),0,1) +'/' + substring(md5(cells["image"].value.replace(' ','_')),0,2) + '/' + cells["image"].value.escape('url').replace('+','_')

3. Test the links and verify the data
   - you'll notice that the reconciliation operation found two images for the duc d'Aumale. Choose one and delete so the data model remains simple for the exercice. Use the '_blank' facet (as it has no name type)
> All → Edit rows → Remove matching rows

The last step is to prepare an xml:id for each entity. We will of course use the data coming from the `@ref`attribute coming from the `biblissima-tei.xml`file.
We just need to delete the '#' character in the string in the "name - ref" colum:

>  Edit columns… — > Add colum based on this column…
> name it 'xml:id' for example
> Enter `value.replace('#', '')`in the expression field



## Step 5 - Export the data to paste in the XML source file
The goal is to create 2 lists of places and persons, and paste them in the TEI Header. 
We could choose to do that in the `<standoff>` or in the `<profileDesc>`element, depending on the choices of your publication system. 
We'll choose the latter to be compatible with an existing template in TEI Publisher (graves letter)

We want to insert a simple hierarchy of places and persons.

```
<teiHeder>
(...)
<profileDesc>

   <particDesc>

      <listPerson>
         <person xml:id="n211">
            <persName>duc d'Aumale</persName>
            <idno type="biblissima">https://data.biblissima.fr/w/Item:Q6757</idno>
           <figure>
                     <graphic url="https://upload.wikimedia.org/wikipedia/commons/4/4e/Prince_Henri%2C_Duke_of_Aumale.jpg"/>
           <ab type="caption">Prince Henri, Duke of Aumale.jpg</ab>
           </figure>
          </person>
      </listPerson>
   </particDesc>

   <settingDesc>

      <listPlace>
         <place xml:id="n51">
            <placeName xml:id="en">Chantilly</placeName>
               <location>
                  <geo>49.19461 2.47124</geo>
               </location>
         </place>
      </listPlace>

   </settingDesc>

</profileDesc>

</teiHeader>
```

OpenRefine offers several export options. 
Many of them will export data with the current view with the filters and facets applied. 
To produce the two lists structured as the example above, we will proceed in two steps using the facet on the column 'name type' as we did before. 

1. Filter the entry list for places with the textual facet on the column 'name type'

2. To format our data in XML according to the listPlace and listPerson trees, we will use the OpenRefine's **Templating exporter** from the export dropdown menu. 

> Export --> Templating…


3. The templating Export Windows suggests an exporting pattern in the json format. Start by emptying all fields to insert the  xml tags. Lets start with the list of places:
   - Prefix: enter the opening tag `<listPlace> `
   - Suffix: enter the closing tag `</listPlace>`
   - Row separator: leave empty
   - Row template: copy and paste the the pattern and add the reference to the column row with that pattern: `${name-of-the-column}`

```
 <place xml:id="${xml:id}">
        <placeName>${name}</placeName>
        <location>
          <geo>${latitude} ${longitude}</geo>
        </location>
        <idno type="wikidata">${Wikidata identifier}</idno></place>
```
The templating exporter window displays a preview of the result on the right part of it. If the preview seems ok with you, click on the 'export' button.

```
<div align="center">
<img src="/img/export-listplaces" alt="Export a list of places" width="100%">
</div>
```
The result of the export operation is presented in a window of your default text editor. 
Copy the result and paste it in your biblissima.xml source file in a new standoff element, as a direct child of the root element TEI.

Do the same thing for persons

```
<person corresp="${xml:id}">
        <persName>${name}</persName>
        <idno type="data.biblissima">https://data.biblissima.fr/w/Item:${bbma-QID}</idno>
        <idno type="portail.biblissima">${Link to bbma portal}</idno>
        <figure>
            <graphic url="${image file url}"/>
        </figure>
</person>
```


## Step 6 - Visualise with TEI Publisher

This needs some tweaking. 
To do later.


