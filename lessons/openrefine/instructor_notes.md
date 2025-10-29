# 2025-11-04-strathclyde OpenRefine lesson instructor notes

## Summary and Setup

**[SLIDE HERE: Links/QR codes]**

- Lesson site: [https://swcarpentry.github.io/shell-novice/](https://swcarpentry.github.io/shell-novice/)

### Summary

- Before you can analyse your data, you normally need to *clean it**
- What is "data cleaning"?
  - Identification (and maybe correction) of errors in the data
  - Formatting data for consistency
- **If your data is not clean, your analysis results may be false and/or not reproducible**

- OpenRefine is a powerful, free, and open source tool for working with messy data
  - It can clean data and transform it from one format to another
  - It can also generate reproducible data cleaning workflows or pipelines that track the changes you make
  - The scripts can also be applied to **undo** changes that were made to the raw data

-  Data cleaning steps often need to be applied to more than one file
   -  The pipelines OpenRefine makes allow you to reapply the processes to new datasets

## Introduction

- OpenRefine is a Java program that runs on your own machine
  - It is a desktop application that uses your web browser as an interface
  - No internet connection is required and no data or commands are sent to any server: **it can be used on confidential data**

- OpenRefine **does not modify your original dataset** - this is best practice
  - You must save the cleaned data in a different file

- All OpenRefine actions can be reversed

- OpenRefine saves as you go - you can leave or pick up a data cleaning proejct at any point

- OpenRefine can import your data in many formats, including
  - Comma-separated (CSV) or tab-separated (TSV)
  - text files
  - fixed-width columns
  - JSON
  - XML
  - OpenDocument Spreadsheet (ODS)
  - Excel spreadsheet (XLS/XLSX)
  - RDF data

## Importing data

**[OPENREFINE DEMO]**

- Let's get started with OpenRefine, by creating a project
- Start OpenRefine
  - This will open in a web browser at [http://127.0.0.1:3333](http://127.0.0.1:3333)
  - In the left margin you'll see options: `Create Project`, `Open Project` and so on
- Click `Create Project`
  - The window "Create a project by importing data" should be visible
- Select `This Computer` as you want to use a local file
- Click `Browse`
  - Navigate to where you downloaded `Portal_rodents_19772002_simplified.csv`
  - Select that file and click `Open`
  - The filename should be visible
- Click `Next` to upload the data to OpenRefine
  - Once it's loaded, OpenRefine will present a preview of your data

- **Any obvious errors (like file formatting problems) should be visible at this point**

- In the middle of the page there should be a set of options including `Character Encoding`
  - Ensure that the `Trim leading and trailing whitespace from strings` option is **not** checked
- Everything should ook OK
- Click `Create project`

- OpenRefine should present you with a table of 790 rows
  - All data is imported as text - **even numerical data**

## Exploring data

### Exploring data with facets

- **Facets** are a useful concept in OpenRefine
  - **Faceting** is the process of exploring data using filters, to explore its composition
  - Faceting allows us to identify a subset of data to change in bulk
- **Faceting groups all like values that appear in a column**
  - It allows filtering by those values
- This is easier to understand through practice

- Find the `scientificName` column in the dataset
- Click the down arrow/triangle next to the header
- Choose `Facet -> Text facet`

- In the left panel you will see a box representing each unique value in the `scientificName` column
  - The number of times each entry appears is also shown
- Sort the facet by `name` and `count`
  - What problems do you see with the data?

- Hover your mouse over a name in the facet
  - You should see an `edit` and an `include` link
  - If you click on `edit` you will see options to repair errors immediately - **don't do this yet**

**[SHOW SLIDES: Challenge (faceting)]**

**[OPENREFINE DEMO]**

- By default, all imported columns are treated as text
- We can convert data we know to be a different data type (e.g. number or date) using `Edit cells -> Common transforms`
  - **Sometimes the data has errors that prevent us treating it as numeric** so we explore it with a facet first

- Click on the down arrow/triangle for `yr`
- Select `Facet -> Numeric facet`
  - This will generate a facet that says there is no numeric data present (the data is in text format)
- Click on the down arrow/triangle for `yr`
- Select `Edit cells -> Common transforms... -> To number`
  - The data shifts to right-justified and turns green
  - The facet shows a histogram

- Transform `period`, `plot`, and `recordID` from text to numbers

- Remove the facets by clicking the `X`

### Using scatterplots

- We can see how numeric columns relate to each other using the **scatterplot facet**
- Click on the down arrow/triangle for `recordID`
  - Select `Facet -> Scatterplot facet`
  - A new window called `Scatterplot matrix` appears
  - This plots each numeric column against the others
- Select the plot of `recordID` against `yr`
  - A facet of the comparison appears
- Click in the scatterplot and drag to highlight a rectangle
  - **What happens to the data**? It is selected/subsetted in the table
  - **We can use this approach to identify outliers in a dataset**
- Click anywhere in the scatterplot to deselect

## Transforming data

### Data splitting

- We can split data from one column into multiple columns, if we can identify a common separator (like a comma or space)

- Let's split the `scientificName` into two columns: one for genus, one for species
- Click on the down arrow/triangle for `scientificName`
  - Select `Edit Column -> Split into several columns...`
  - In the `Separator box` replace the comma with a space
  - **Uncheck the `Remove this column` box**
  - Click `OK`
- You should now see several columns: `scientificName1`, `scientificName2`, etc.
  - We can see that some of the data was not well-formatted, in the sense it wasn't simple _Genus species_ data
  - We're going to have to fix this

### Undoing/redoing actions

- But first let's undo what we just did with the split
  - This is quite a common requirement when cleaning data

- Click `Undo/Redo` in the left margin
  - You should see all the changes you made to the data in that panel
  - The current stage is highlighted
  - We can click on any stage in the processing to jump to that version of the data

- Click on the appropriate step just before splitting `scientificName`
- We could click on the last stage and redo the change, but let's not do that.

### Trimming leading/trailing whitespace

- It's quite common for whitespace to be added at the start/end of text in datasets
  - But it's difficult to spot this by eye
- OpenRefine can remove these characters for us automatically

- Click on the down arrow/triangle for `scientificName`
  - Select `Edit cells -> Common transforms -> Trim leading and trailing whitespace`
  - OpenRefine will warn us about deleting project history (taking a different path), but click `Apply anyway`

- Let's split the `scientificName` into columns again: one for genus, one for species
- Click on the down arrow/triangle for `scientificName`
  - Select `Edit Column -> Split into several columns...`
  - In the `Separator box` replace the comma with a space
  - **Uncheck the `Remove this column` box**
  - Click `OK`
- You should now see two columns: `scientificName1`, `scientificName2`

### Renaming columns

- We have Genus and species separated into two columns, but they have generic names
  - We want to call the columns `genus` and `species`

- Click on the down arrow/triangle for `scientificName1`
  - Select `Edit Column -> Rename column...`
  - Enter `genus` into the field
- Click on the down arrow/triangle for `scientificName2`
  - Select `Edit Column -> Rename column...`
  - Enter `species` into the field
  - **You will not be allowed to rename the column**
  - **Change the name of the `species` column to `species_abbreviation`**
  - Rename the `scientificName2` column to `species`

### Combine columns

- The date for each row is split in three columns, for month, day, and year
  - **We want to create a single column which contains the date**

- Click on the menu for the `yr` column
  - Select `Edit column -> Join columns...`
  - Check the boxes next to `mo` and `dy`
  - Enter `-` as a separator
  - Select the option `Write result in new column named` and write `date` in the field
  - Click `OK`

- Click on the menu for the new `date` column
  - Select `Edit cells -> Common transforms -> To date`

- Now we can use a **Timeline facet** to inspect the data
- Click on the menu for the new `date` column
  - Select `Facet -> Timeline facet`
  - A histogram view of sampled dates should appear in the left margin

### Data clustering

- Clustering allows us to find groups of entries that are not identical, but that are similar
- These may be alternative representations of the same thing - possibly even typos

- Make a text facet for `scientificName`
- In `scientificName` there are several near-identical entries
  - For example a misspelling of _Ammospermophilus harrisii_
  - Click the `Cluster` button
    - Try some options for `Method` and `Keying function` and click the `Cluster` button
- Choose `Key collision` and `Metaphone3`
  - This identifies a cluster of options for _Ammospermophilus harrisii_
  - It also suggests a new cell value to replace the likely misspellings
- Tick the `Merge` checkbox
- Click `Merge selected and close`
- Note that the facet has updated so that there are no more misspellings of _Ammospermophilus harrisii_

