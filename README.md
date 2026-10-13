# 🏗️ ServiceNow Asset Inventory Project 🎓

This project documents a complete, student-level ServiceNow exercise: preparing spreadsheet data, creating a custom table, importing the data through an import set and transform map, preventing duplicates with coalesce, and reporting on the result through a report and a dashboard.

The work is organised as four milestones under one epic. Each milestone has one or more Word documents, and the data work has an Excel workbook. Every document uses the same asset inventory as its running example, so the figures in one document can be checked against the others.

This README describes the whole project: what each deliverable is, how the pieces connect, the example dataset, the expected results, how to validate the work, and the limits of what was verified.

---

## 📚 Contents

1. Project overview
2. Epic and milestone map
3. Deliverables inventory
4. How the pieces connect
5. The example dataset
6. Milestone 1: Spreadsheet and table
7. Milestone 2: Import set and transform map
8. Milestone 3: Transform, validate and coalesce
9. Milestone 4: Reports and dashboards
10. Expected results and reconciliation
11. Validation checklist for the whole project
12. Suggested folder layout
13. How to read and use the documents
14. Known limitations and verification status
15. Glossary
16. Document conventions
17. Revision history

---

## 🚀 1. Project overview

### 🎯 1.1 Purpose

The purpose of the project is to show a complete, repeatable process for bringing business data into ServiceNow and making it useful. The process is the same whatever the dataset, so the example asset inventory is used as a model. A reader who follows the documents in order should be able to:

- prepare a spreadsheet that can be imported safely,
- create a custom table that matches the spreadsheet,
- load the spreadsheet into a staging table (an import set),
- map each column to the table with a transform map,
- transform the data and check the result,
- enable coalesce so that reloading the same data does not create duplicates,
- add new data without breaking what is already there,
- build a bar chart report that counts assets by category, and
- place that report on a dashboard for the asset team.

### 🧭 1.2 Scope

The project covers the administrator workflow on a ServiceNow developer instance. It does not cover integrations with other systems, scripted REST APIs, service catalogues, security roles beyond what is needed for the exercise, or production migration planning. Those topics would each need their own project.

### 👩‍🎓 1.3 Audience

The documents are written for a student or a junior administrator. They assume basic familiarity with navigating the platform and with spreadsheets. They explain each concept the first time it is used, so a reader with no prior ServiceNow experience can follow them.

### 📌 1.4 Status

All documents are complete and have been generated. The spreadsheet has been recalculated and its formulas return the expected values. The Word documents have been rendered to PDF to confirm their page counts and layout. The steps described in the documents have **not** been run against a live instance in this project. See Section 14.

---

## 🗺️ 2. Epic and milestone map

| Milestone | Epic title | Focus | Main outputs |
|---|---|---|---|
| 1 | Creation of Spreadsheet and Table | Prepare the source data and design the table | Spreadsheet guide; table creation guide |
| 2 | Creation of Import Set Table and Transform Map | Load data into staging and map it to the table | Import set guide; transform map guide |
| 3 | Transform Data & Validate and Enable Coalesce to Avoid Duplicate Records | Run the transform, check the results, prevent duplicates | Transform and validation guide; coalesce guide; new-data workbook |
| 4 | Creation of Reports & Dashboards | Build a report and place it on a dashboard | Report 1 guide (12 and 44 pages); dashboard guide |

Each milestone depends on the one before it. The data built in Milestone 1 is loaded in Milestone 2, validated and protected in Milestone 3, and reported on in Milestone 4. A reader who skips a milestone will find the next one difficult to follow.

---

## 📦 3. Deliverables inventory

The table below lists every file produced for the project, with its type, its length and what it is for. All files are held in the same output folder.

| File | Type | Length | Milestone | Purpose |
|---|---|---|---|---|
| Milestone1_Creation_of_Spreadsheet_and_Table.docx | Word | 12 pages | 1 | Preparing the spreadsheet and the first table guidance |
| Milestone1_Creation_of_Tables.docx | Word | 12 pages | 1 | Detailed guide to designing and creating custom tables |
| Milestone2_Import_Set_Table_and_Transform_Map.docx | Word | 12 pages | 2 | Load Data, import set structure, and an introduction to transform maps |
| Milestone2_Create_Transform_Map.docx | Word | 12 pages | 2 | Focused guide to creating and configuring the transform map |
| Milestone3_Transform_Data_Validate_Coalesce.docx | Word | 12 pages | 3 | Running the transform, validating results, and an introduction to duplicates |
| Milestone3_Enable_Coalesce_Avoid_Duplicates.docx | Word | 12 pages | 3 | Focused guide to enabling and testing coalesce |
| Inserting_New_Data.xlsx | Excel | 4 sheets | 3 | Workbook of new asset rows, with validation and checks |
| Milestone4_Report_1.docx | Word | 12 pages | 4 | Concise guide to creating Report 1 |
| Milestone4_Report_1_Full.docx | Word | 44 pages | 4 | Full guide to Report 1, with specification, tests and appendices |
| Milestone4_Adding_Reports_to_Dashboard.docx | Word | 12 pages | 4 | Creating the Asset Overview dashboard and adding Report 1 |
| README.md | Markdown | this file | All | Project overview and guide to the deliverables |

Where a milestone has two documents, the first is the broader guide and the second is a focused companion. Both are kept because they serve different readers: the broader guide gives context, and the focused guide gives step-by-step detail on one topic.

### 🔢 3.1 Summary counts

- Word documents: 10 (including the 44-page Report 1 guide)
- Excel workbooks: 1
- Markdown files: 1
- Total pages across all Word documents: approximately 190

---

## 🔗 4. How the pieces connect

The project follows a single pipeline. Each stage produces something the next stage reads.

```
Spreadsheet (.xlsx / .csv)
        |
        v
Load Data  ----->  Import set (staging table, one row per spreadsheet row)
                          |
                          v
                Transform map (field maps, coalesce, scripts)
                          |
                          v
          Custom table: u_asset_inventory (validated records)
                          |
                          v
          Report 1: Asset Count by Category (bar chart, count of Asset Tag)
                          |
                          v
          Dashboard: Asset Overview (holds a reference to Report 1)
```

### 📌 4.1 What each stage depends on

- The **spreadsheet** must have a header row, one record per row, and a unique key (Asset Tag).
- The **table** must have a column for each spreadsheet header, with matching choice lists.
- The **import set** is created automatically from the spreadsheet by Load Data.
- The **transform map** must point at the import set (source) and the table (target), with a field map for each column and coalesce on Asset Tag.
- The **report** reads the table. Its figures are only as good as the table's data.
- The **dashboard** holds a reference to the report. It does not store its own copy of the figures.

### 📌 4.2 Why this order matters

If the table is wrong, every later stage is wrong. If coalesce is missing, every reload duplicates records, and the report counts double. If the report is built before validation, its figures may be accepted without anyone checking them. Each milestone therefore ends with a check that the next milestone depends on.

---

## 🧪 5. The example dataset

The example uses ten assets. Four come from the original spreadsheet in Milestone 1, and six are new records in the insert workbook from Milestone 3. The dataset is used in every document, so the figures can be checked across the project.

### 🧱 5.1 Original spreadsheet (Milestone 1)

| Asset Tag | Asset Name | Category | Location | Purchase Date | Cost | Status |
|---|---|---|---|---|---|---|
| AST-1001 | Dell Latitude 5420 | Laptop | Madurai Lab 1 | 2025-06-12 | 58000 | In Use |
| AST-1002 | HP ProDesk 400 | Desktop | Madurai Lab 2 | 2025-07-03 | 42500 | In Use |
| AST-1003 | Cisco Catalyst 2960 | Network | Server Room | 2024-11-20 | 31000 | Active |
| AST-1004 | Epson EB-X51 Projector | Projector | Seminar Hall | 2025-01-15 | 47500 | In Storage |

### 🛡️ 5.2 New records (Inserting_New_Data.xlsx, Milestone 3)

| Asset Tag | Asset Name | Category | Location | Purchase Date | Cost | Status |
|---|---|---|---|---|---|---|
| AST-1005 | Lenovo ThinkPad T14 | Laptop | Madurai Lab 3 | 2025-08-18 | 62500 | In Use |
| AST-1006 | Dell OptiPlex 3090 | Desktop | Madurai Lab 3 | 2025-08-18 | 45800 | In Use |
| AST-1007 | Netgear GS308 Switch | Network | Server Room | 2025-09-02 | 6900 | Active |
| AST-1008 | BenQ MH560 Projector | Projector | Seminar Hall | 2025-09-10 | 52000 | In Storage |
| AST-1009 | HP LaserJet M404dn | Printer | Admin Office | 2025-10-01 | 28500 | In Use |
| AST-1010 | Apple iPad 10th Gen | Laptop | Library | 2025-10-06 | 38900 | In Storage |

### 🧷 5.3 Combined dataset

After both loads, the table holds ten records with unique Asset Tags from AST-1001 to AST-1010. The category and status counts for this combined dataset are in Section 10.

### 🔤 5.4 Field definitions

| Label | Column name | Type | Mandatory | Notes |
|---|---|---|---|---|
| Asset Tag | u_asset_tag | String (40) | Yes | Coalesce key; must be unique and trimmed |
| Asset Name | u_asset_name | String (100) | Yes | Descriptive name |
| Category | u_category | Choice | No | Laptop, Desktop, Network, Projector, Printer |
| Location | u_location | String (100) | No | Free text |
| Purchase Date | u_purchase_date | Date | No | Stored as a date, shown as yyyy-mm-dd |
| Cost | u_cost | Decimal | No | Plain number, no currency symbol |
| Status | u_status | Choice | No | In Use, Active, In Storage, Retired |
| Assigned to | u_assigned_to | Reference (User) | No | Optional; used in the table design, not in the example data |

The Status choice list includes Retired, which is not used by any example asset. It is included so that a later version of the data can record withdrawn equipment.

---

## 🧱 6. Milestone 1: Spreadsheet and table

### 🎯 6.1 Purpose

Milestone 1 prepares the source data and the destination table. Nothing can be imported until both exist and agree with each other.

### 📄 6.2 Spreadsheet guidance

The spreadsheet guidance covers the layout rules, the data quality checks, the choice of a unique key and the saving format.

Layout rules:

- The first row holds headers only.
- Each later row holds exactly one record.
- No merged cells, blank rows, totals or notes appear in the data area.
- Header names contain no slashes, percent signs or brackets.

Data quality checks:

- Dates use one format throughout.
- Numbers contain no currency symbols or thousands separators.
- Choice values are spelled identically in every row.
- The key column has no leading or trailing spaces.

Saving:

- Save as .xlsx with a clear name, for example M1_Asset_Inventory.xlsx.
- If CSV is used, choose UTF-8 encoding.
- Keep the original file for comparison during validation.

### 🧩 6.3 Table guidance

The table guidance covers the design decisions and the configuration steps.

Design decisions:

- Purpose: one table, one kind of record (an asset).
- Extension: standalone. The table does not extend Task, because assets are reference data rather than work items.
- Naming: label Asset Inventory, name u_asset_inventory.

Configuration steps:

1. Open the Tables module under System Definition.
2. Create a new table with the label and name above.
3. Tick Create module, so the table appears in the navigator.
4. Add one column for each spreadsheet header, with the type and mandatory setting in Section 5.4.
5. Add the choice values for Category and Status.
6. Set Asset Tag as the display field if the course asks for it, or Asset Name otherwise.
7. Review the default access control rules, and confirm the admin role can read and write.
8. Create one test record, check it, then delete it, so the table is empty before the import.

### 📤 6.4 Outputs

- Two Word guides (the spreadsheet and table guides are split across the two documents).
- The expected table design in the Milestone 1 table guide.

### 🧱 6.5 Checks before leaving Milestone 1

- The spreadsheet has the seven headers in Section 5.4 and ten rows of data (or four, for the original load).
- The table exists, is visible in the navigator, and has seven business columns.
- Both choice lists match the spreadsheet values exactly.
- The table is empty, or holds only the records intended for the next step.

---

## 🔄 7. Milestone 2: Import set and transform map

### 🎯 7.1 Purpose

Milestone 2 moves the spreadsheet into the platform in two stages. The first stage stores the raw rows in an import set. The second stage copies those rows into the table through a transform map. Keeping the stages apart means bad data can be found before it reaches the table.

### 📥 7.2 Import set

The import set is created by Load Data:

1. Open System Import Sets and select Load Data.
2. Name the data source, for example Asset Inventory Source.
3. Choose the file type (Excel or CSV) and attach the spreadsheet.
4. Set the import set table label, for example Asset Inventory Import.
5. Submit. The platform reads the headers, creates the staging table, and loads one row per data record.
6. Check the row count on the import set against the spreadsheet.

Each import set row carries standard columns. The most important are:

- State: Loaded, then Processed after the transform.
- Action: Insert, Update, Skip, Ignore or Error.
- Status message: the reason for a skip or error.

### 🔀 7.3 Transform map

The transform map links the staging table to the target table.

Creation:

1. Open the import set and select Create Transform Map, or create one from System Import Sets > Transform Maps.
2. Name it, for example Asset Inventory Transform.
3. Confirm the source is the import set table and the target is u_asset_inventory.
4. Save. Field maps are generated for columns whose names match.

Field maps:

- Review each generated field map and correct any wrong link.
- Add a manual field map for any column that did not match by name.
- Set Coalesce on the Asset Tag field map. (Milestone 3 covers this in detail.)

Scripts (optional):

- onStart runs once before the first row.
- onBefore runs for each row before it is written, and is the usual place to clean or check values.
- onAfter runs for each row after it is written.
- onComplete runs once at the end.

### 📤 7.4 Outputs

- Two Word guides: one on the import set and transform map overall, and one focused on creating the transform map.

### 🔄 7.5 Checks before leaving Milestone 2

- The import set row count equals the number of spreadsheet data rows.
- Every spreadsheet column has a field map.
- The transform map is active and targets u_asset_inventory.
- No field map points at the wrong target field.

---

## 🛡️ 8. Milestone 3: Transform, validate and coalesce

### 🎯 8.1 Purpose

Milestone 3 runs the transform, checks what it produced, and makes sure the process can be repeated without creating duplicates. It has three parts: running and validating, enabling coalesce, and adding new data.

### 📌 8.2 Running the transform

1. Open the import set record.
2. Select Transform.
3. Wait for the import set to show Processed.
4. Read the Import Set Rows list.

On a first run against an empty table, every valid row shows Insert, skipped rows show Skip with a reason, and no row shows Error.

### 📌 8.3 Validating the result

Validation checks that the records are correct, not just that they arrived.

Reconciliation:

- Spreadsheet data rows = import set rows.
- Import set rows = inserted + updated + skipped + ignored + errored.
- Target record count = inserted + updated (for an empty target at the start).

Value checks:

- No mandatory field is empty.
- Choice values show the agreed labels.
- Dates and costs match the spreadsheet for a sample of records.
- References point at the intended user.
- Key fields have no stray spaces.

Sampling:

- For a small table, check every record.
- For a larger table, check the first and last rows, one or two rows from each category, and any row changed by a script.

### 🔁 8.4 Coalesce

Coalesce is the setting that stops duplicates. When it is on for a field, the transform searches the target table for an existing record with the same value in that field before it inserts. If a match exists, the record is updated instead.

Enabling coalesce:

1. Open the Asset Inventory transform map.
2. Open the field map from asset_tag to u_asset_tag.
3. Tick Coalesce and save.
4. Add a line to the map description recording the key, the date and the reason.

Choosing the coalesce field:

- A good key is unique, stable, present on every row, and text.
- Asset Tag meets all four tests.
- Asset Name, Location, Purchase Date and Cost do not, because many assets can share them.

Keeping the key clean:

- Trim leading and trailing spaces.
- Use one case throughout.
- Check the spreadsheet for repeated keys before loading. Coalesce matches only against records already in the table, so two rows with the same key in one file are not caught.

### 🔁 8.5 Coalesce test scenarios

The coalesce guide defines seven scenarios. The core ones are:

| Scenario | Setup | Expected result |
|---|---|---|
| S1 First load | Empty table, full spreadsheet | All valid rows Insert |
| S2 Repeat load | Same spreadsheet loaded again | All rows Update; count unchanged |
| S3 Changed value | One location edited | One row Update; only that record changes |
| S4 New asset | One new Asset Tag appended | New row Insert; count rises by one |
| S5 Empty key | One blank Asset Tag | Row skipped with a reason |
| S6 Messy key | Trailing space or different case | Cleaned key matches; no new record |
| S7 Duplicate in source | Two rows share one key | Reported before loading |

The repeat-load test (S2) is the most important, because it is the case coalesce exists to handle.

### ➕ 8.6 Adding new data

The Inserting_New_Data.xlsx workbook holds six new assets (AST-1005 to AST-1010) in the same seven columns as the original spreadsheet.

Workbook sheets:

- Instructions: how to add rows, the colour legend, and the duplicate rules.
- Insert_Data: the six new records, with dropdowns for Category and Status, formatted to row 100.
- Lists: the allowed Category and Status values.
- Checks: formulas that count rows, find blank or repeated keys, detect spaces in keys, and flag values not on the lists. The overall status cell reads READY TO IMPORT when every check passes.

Process for new data:

1. Enter or paste rows on Insert_Data, starting in row 2.
2. Confirm the Checks sheet reads READY TO IMPORT.
3. Save as .xlsx.
4. Load through Load Data, then transform.
5. Expect Insert for every new row. If a row shows Update, its key already exists and must be investigated.

The workbook holds new records only. Changes to existing assets belong in a separate update file.

### 📤 8.7 Outputs

- Two Word guides: the transform and validation guide, and the coalesce guide.
- One Excel workbook for new data.

### 🛡️ 8.8 Checks before leaving Milestone 3

- The transform has run, and reconciliation passes.
- Coalesce is on for Asset Tag.
- A repeat load shows Update and an unchanged count.
- No duplicate Asset Tags remain in the table.
- The new-data workbook reads READY TO IMPORT.

---

## 📊 9. Milestone 4: Reports and dashboards

### 🎯 9.1 Purpose

Milestone 4 turns the validated table into information. Report 1 answers one question, and the dashboard presents that answer alongside room for later reports.

### 📑 9.2 Report 1: Asset Count by Category

Specification:

| Setting | Value |
|---|---|
| Data source | u_asset_inventory |
| Report type | Bar chart, vertical |
| Group by | Category |
| Aggregation | Count of Asset Tag |
| Sort | Count, descending |
| Filter | None in version 1 |
| Chart title | Assets by Category |
| Labels | On, whole numbers |
| Legend | Off |
| Colour | Single dark blue |
| Visibility | Asset team |
| Name | Asset Count by Category |

Build steps:

1. Open the Reports module and create a new report.
2. Select u_asset_inventory as the data source.
3. Select the Bar report type.
4. Set the group-by field to Category.
5. Set the aggregation to Count of Asset Tag.
6. Set the sort to count, descending.
7. Configure the title, labels, legend and colour.
8. Save with the name Asset Count by Category.
9. Write the description (purpose, data source, grouping, measure, sort, filter, owner, audience, date, last validation).
10. Set visibility to the asset team once validated.

Why a bar chart: the question compares counts across a few categories, and bar length is the easiest comparison for a reader. A pie chart would answer a question about shares; a list would require counting by hand; a trend chart would need a history the table does not hold.

Why no filter: the primary question concerns all assets, so the chart total must equal the record count. A filter would require a different title and would make validation harder.

### 📑 9.3 Report 1 documents

Two documents cover Report 1:

- The 12-page guide is a concise version: objectives, steps, testing and troubleshooting.
- The 44-page guide is the full version: background, data model, requirements, specification, build steps, chart configuration, grouping, filters, data quality, visibility, a twelve-case test plan, reconciliation, maintenance, troubleshooting, review, three appendices (checklist, glossary, sign-off record) and references.

Use the 12-page guide for a quick build. Use the 44-page guide for review, sign-off or teaching.

### 📑 9.4 Test plan for Report 1

| Test | Purpose | Expected result |
|---|---|---|
| T1 | Data source | u_asset_inventory |
| T2 | Report type | Bar, vertical |
| T3 | Grouping | Category; five bars |
| T4 | Aggregation | Count of Asset Tag |
| T5 | Sort | Descending by count |
| T6 | Total | Equals table record count (10) |
| T7 | Each bar | Matches expected counts (Section 10) |
| T8 | Labels | Whole numbers visible |
| T9 | Access | Same figures for a team member |
| T10 | Live update | Count rises by one when a test record is added, then returns |
| T11 | Description | All required elements present |
| T12 | Name | Asset Count by Category |

### 🖥️ 9.5 Dashboard: Asset Overview

The dashboard holds a reference to Report 1, not a copy. The figures therefore stay current, and the same report can appear on other dashboards.

Planning:

- Audience: asset team.
- Purpose: overview of assets by category.
- Version 1 widget: Report 1.
- Reserved space: the right-hand half of the page, for a later report.
- Filters: none in version 1.
- Visibility: asset team.

Build steps:

1. Open the dashboards area and check that no Asset Overview dashboard exists.
2. Create the dashboard with the title Asset Overview and save it.
3. Enter edit mode.
4. Add a report widget and select Asset Count by Category. Check that its data source is u_asset_inventory and its type is bar.
5. Place the widget at the top left and size it to about half the page width.
6. Save, reload, and confirm the widget persists.
7. Leave edit mode and view the dashboard as a reader would.

Dashboard tests (D1 to D11) cover existence, the widget, the correct report, the figures, the total, layout, reserved space, team access, edit restriction, live update and the description.

Filters on a dashboard apply only to widgets whose data source has the matching field. A dashboard filter on a field that Report 1 does not use would leave the widget unfiltered while the rest of the page changed. This is why version 1 has no dashboard filters.

### 📤 9.6 Outputs

- Report 1 (12-page and 44-page guides).
- The Asset Overview dashboard guide (12 pages).

### 📊 9.7 Checks before leaving Milestone 4

- Report 1 passes T1 to T12.
- The dashboard passes D1 to D11.
- The widget figures match the report and the table.
- The team can view the dashboard and the report.

---

## ✅ 10. Expected results and reconciliation

### 🧮 10.1 Expected category counts (ten-asset dataset)

| Category | Count | Assets |
|---|---|---|
| Laptop | 3 | AST-1001, AST-1005, AST-1010 |
| Desktop | 2 | AST-1002, AST-1006 |
| Network | 2 | AST-1003, AST-1007 |
| Projector | 2 | AST-1004, AST-1008 |
| Printer | 1 | AST-1009 |
| **🏁 Total** | **10** | All records |

### 🧮 10.2 Expected status counts (ten-asset dataset)

| Status | Count | Assets |
|---|---|---|
| In Use | 5 | AST-1001, AST-1002, AST-1005, AST-1006, AST-1009 |
| Active | 2 | AST-1003, AST-1007 |
| In Storage | 3 | AST-1004, AST-1008, AST-1010 |
| Retired | 0 | None |
| **🏁 Total** | **10** | All records |

### 💰 10.3 Expected cost total

The sum of Cost across the ten assets is 413,600. This figure is a check on the transform, because a value that is rounded, truncated or misplaced will change it. It is not shown on Report 1.

Calculation: 58000 + 42500 + 31000 + 47500 + 62500 + 45800 + 6900 + 52000 + 28500 + 38900 = 413,600.

### ⚖️ 10.4 Reconciliation procedure

1. Count the spreadsheet data rows for each file.
2. Count the import set rows.
3. Add the Action counts (Insert, Update, Skip, Ignore, Error) and compare with the import set row count.
4. Count the table records and compare with the expected total.
5. Compare each category count on Report 1 with Section 10.1.
6. Record any difference and its cause.

### ✅ 10.5 Expected results after a repeat load

After S2 (repeat load of the same ten records), the table should still hold ten records, and every import set row should show Update. Any Insert on a repeat load indicates that coalesce is missing or that a key differs in spacing or case.

---

## ☑️ 11. Validation checklist for the whole project

Use this checklist at the end of the work. Every item should be ticked, or explained if it is not.

### 🗃️ 11.1 Data

- ⬜ Spreadsheet has seven headers and the agreed rows.
- ⬜ No Asset Tag is blank, repeated or padded with spaces.
- ⬜ Category and Status values match the choice lists exactly.
- ⬜ Purchase Date values are real dates.
- ⬜ Cost values are plain numbers.

### 🧩 11.2 Table

- ⬜ u_asset_inventory exists with the label Asset Inventory.
- ⬜ All seven business columns exist with the correct types.
- ⬜ Choice lists for Category and Status are complete.
- ⬜ The table appears in the navigator.
- ⬜ No test records remain.

### 📥 11.3 Import and transform

- ⬜ Load Data created an import set with the expected row count.
- ⬜ The transform map targets u_asset_inventory and is active.
- ⬜ Every spreadsheet column has a field map.
- ⬜ The transform has run and reconciliation passes.

### 🔁 11.4 Coalesce

- ⬜ Coalesce is on for Asset Tag.
- ⬜ A repeat load shows Update and leaves the count unchanged.
- ⬜ No duplicate Asset Tags exist.
- ⬜ The map description records the key, date and reason.

### 🆕 11.5 New data

- ⬜ Inserting_New_Data.xlsx reads READY TO IMPORT.
- ⬜ New rows load as Insert.
- ⬜ The table holds ten records with unique tags.

### 📈 11.6 Reports and dashboard

- ⬜ Report 1 is named Asset Count by Category.
- ⬜ Report 1 passes tests T1 to T12.
- ⬜ Report 1 figures match Section 10.1.
- ⬜ Asset Overview dashboard exists and holds Report 1.
- ⬜ Dashboard passes tests D1 to D11.
- ⬜ The asset team can see the report and the dashboard.

---

## 🗂️ 12. Suggested folder layout

The documents can be kept in a structure that matches the milestones:

```
servicenow-asset-inventory/
├── README.md
├── milestone-1-spreadsheet-and-table/
│   ├── Milestone1_Creation_of_Spreadsheet_and_Table.docx
│   └── Milestone1_Creation_of_Tables.docx
├── milestone-2-import-set-and-transform-map/
│   ├── Milestone2_Import_Set_Table_and_Transform_Map.docx
│   └── Milestone2_Create_Transform_Map.docx
├── milestone-3-transform-validate-coalesce/
│   ├── Milestone3_Transform_Data_Validate_Coalesce.docx
│   ├── Milestone3_Enable_Coalesce_Avoid_Duplicates.docx
│   └── Inserting_New_Data.xlsx
└── milestone-4-reports-and-dashboards/
    ├── Milestone4_Report_1.docx
    ├── Milestone4_Report_1_Full.docx
    └── Milestone4_Adding_Reports_to_Dashboard.docx
```

Keep the spreadsheet the milestone 1 guide describes (for example M1_Asset_Inventory.xlsx) alongside the Milestone 1 documents. It is the source file, and it is not generated by this project.

---

## 📖 13. How to read and use the documents

### 👣 13.1 Reading order

1. Milestone 1, both documents.
2. Milestone 2, the import set and transform map document first, then the focused transform map document.
3. Milestone 3, the transform and validation document first, then the coalesce document, then the new-data workbook.
4. Milestone 4, the Report 1 concise guide, then the Report 1 full guide if depth is needed, then the dashboard guide.

### 🛠️ 13.2 Working at the instance

- Work in one browser tab for each task, so a second tab does not overwrite a change.
- Save after each major step.
- Record every choice that differs from the documents, with the reason.
- Keep the spreadsheet, the import set record and the validation record together.

### 🧹 13.3 Using the test records

Each document asks for test records to be added and then removed. Name them clearly, for example with the tag TEST-M4, and check after deletion that the table holds only the intended records.

### 📗 13.4 Using the workbook

Open Inserting_New_Data.xlsx and read the Instructions sheet first. Add rows on Insert_Data only. Check the Checks sheet before loading. Do not overwrite the formulas on the Checks sheet.

### ✍️ 13.5 Using the sign-off record

The 44-page Report 1 guide ends with a sign-off record in Appendix C. Complete it when the report has passed its tests and its figures have been reconciled.

---

## ⚠️ 14. Known limitations and verification status

### 🔍 14.1 Reference sources could not be read

The two reference resources named in every document could not be opened during preparation:

- The ServiceNow Developers page requires a JavaScript-enabled browser.
- The SkillWallet module page redirects to a login page.

The steps in the documents therefore follow the standard ServiceNow administrator workflow for spreadsheet imports, transform maps, reporting and dashboards. They have not been checked against either resource.

### 🧪 14.2 Steps have not been run on a live instance

The documents describe what to do and what to expect. They have not been carried out on a live developer instance as part of this project. Menu names, option labels, report type names and dashboard options can differ between releases. Each step should be compared with the course material and the instance before it is relied on.

### 🔢 14.3 Expected values depend on the dataset

The expected counts, costs and statuses in Section 10 assume the ten-asset dataset in Section 5. If a different dataset is used, the expected values must be recalculated.

### 🏷️ 14.4 Field names may differ

The documents use the names u_asset_inventory and u_asset_tag, which follow the usual naming pattern for a custom table with the u_ prefix. The platform may generate slightly different names if the scope or the label differs. The field map and report settings must use the names that exist on the instance.

### 🔬 14.5 Verification that was done

- The Excel workbook was recalculated, and its check formulas were confirmed to return the expected values (zero errors, all checks OK, total cost 234,600 for the six new rows).
- Each Word document was rendered to PDF to confirm its page count and that tables and headings lay out as expected.
- The Report 1 full guide was checked for list numbering, which was corrected so that each numbered list restarts at 1.

### 💭 14.6 Assumptions

- The asset dataset and the field names were chosen for this project. They are not taken from the course material.
- Costs are shown as plain numbers. The currency is not stated in the data.
- Dates are shown in the yyyy-mm-dd format.

---

## 📝 15. Glossary

| Term | Meaning in this project |
|---|---|
| Aggregation | The calculation for each group in a report, such as count, sum or average. |
| Asset Overview | The dashboard that holds Report 1 in this project. |
| Asset Tag | The unique identifier of an asset. Used as the coalesce key. |
| Bar chart | A report type that shows one bar per group. |
| Choice field | A field whose value is chosen from a fixed list. |
| Coalesce | A transform setting that matches incoming rows to existing records. |
| Condition | A test on a field used to filter records. |
| Dashboard | A page that places several reports or lists in panels. |
| Data source | The table a report reads from. |
| Duplicate | A second record for the same real-world item. |
| Field map | A rule in a transform map that copies one column to one field. |
| Filter | A set of conditions that restricts which records are read. |
| Group by | The field whose distinct values form the bars or rows of a report. |
| Import set | The staging table that holds spreadsheet rows before transform. |
| Load Data | The platform module that reads a spreadsheet and creates an import set. |
| Reconciliation | Comparing counts at each stage to confirm that nothing was lost or added. |
| Report | A saved definition that summarises a table. |
| Reference field | A field that links a record to a record in another table. |
| Transform map | The rules that copy import set rows into a target table. |
| Widget | A panel on a dashboard that displays a report or other content. |

---

## 🎨 16. Document conventions

- Each Word document has a cover page, a contents list, numbered chapters and a footer with page numbers.
- Tables carry a caption where they summarise a test, a specification or a result.
- Tests are labelled with a prefix and a number: T for report tests, D for dashboard tests, S for coalesce scenarios.
- Placeholders are shown as blank underlined lines, for example "Prepared by". They are for the author to complete.
- The example asset tags always appear in the form AST-1001 to AST-1010.
- Reference resources are cited in the References section of each document, with the note that their content could not be read.

---

## 🕘 17. Revision history

| Version | Date | Change |
|---|---|---|
| 1.0 | 13 October 2026 | Initial README covering milestones 1 to 4, all deliverables, the example dataset, expected results and the validation checklist. |

When a document is revised, add a row here with the version, the date and a short description of the change. Keep the file names in Section 3 in step with the actual outputs.
