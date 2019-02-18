# Enriched PlantUML

Enriched PlantUML is a library for PlantUML to enhance the default appearance of your diagrams as well introducing new diagram types and paradigms for writing software documentation and technical specifications.

Features:

- Improved default styling for standard diagram types
- Additional macros for common formatting adjustments
- Insert formatted title blocks
- Insert company copyrights and confidentiality notices
- Macros for entity-relationship diagrams (ERD)

## Getting Started

At the top of your PlantUML file, you need to include `enriched.puml`.

```puml
!includeurl https://raw.githubusercontent.com/bpatram/enriched-plantuml/master/enriched.puml
```

By default, nothing will immediately happen to your diagram, you must opt-in to applying individual features. Please read the sections below which go into more details for each of the features that you can opt-in to using.

### Applying styles

Add `USE_DEFAULT_STYLES()` into your diagram to style Component, Sequence, Activity, and Class Diagrams. See ERD section below for adding styles for ER diagrams.

#### Additional formatting helpers

If you would like to opt-in to word wrapping of notes, descriptions and arrow lines you can add `USE_WORD_WRAP()`. This will default to 125 characters, you can change this by passing a parameter, for example: `USE_WORD_WRAP(100)`

### Inserting title blocks

To stamp your diagram with your name, company, and a revision number you can add `header STD_HEADER` into your diagram. To set your name simply add `!define AUTHOR_NAME First Last` to your diagram and replace "First Last" with your own name. To set a company name add `!define COMPANY_NAME Company Name` to your diagram and replace "Company Name" with your own company name.

### Inserting copyright and confidentiality notices

To stamp your diagram with a confidential notice you can add `footer STD_FOOTER` into your diagram. To set a company name add `!define COMPANY_NAME Company Name` to your diagram and replace "Company Name" with your own company name.

### Entity-relationship diagrams

Due to some styling conflicts and limitations from Plantuml we must also append `USE_ERD_STYLES()` after adding `USE_DEFAULT_STYLES()` to correctly style ER diagrams without breaking existing Class Diagrams.

A new object type is added called `table`. You can use it like this:

```puml
USE_DEFAULT_STYLES()
USE_ERD_STYLES()

enum_mapping(REPORT_STATUS_ENUM, INT(11)) {
    incomplete: 0
    complete: 1
    not_applicable: 2
}

table(items) {
    column_pk()
    omitted_columns()
}

table(items_progress_report_caches) {
    column_pk()
    timestamps()
    column_fk(item_id)

    column_non_nullable(photos_uploaded, REPORT_STATUS_ENUM)
    column_non_nullable(photo_editing_work, REPORT_STATUS_ENUM)
    column_non_nullable(attribution_work, REPORT_STATUS_ENUM)
    column_non_nullable(remote_cataloging_work, REPORT_STATUS_ENUM)
    column_non_nullable(required_attributes_defined, REPORT_STATUS_ENUM)
    column_non_nullable(description_defined, REPORT_STATUS_ENUM)
    column_non_nullable(contract_defined, REPORT_STATUS_ENUM)
    column_non_nullable(condition_defined, REPORT_STATUS_ENUM)
    column_non_nullable(measurements_defined, REPORT_STATUS_ENUM)
    column_non_nullable(active, REPORT_STATUS_ENUM)
    column_non_nullable(origin_type_defined, REPORT_STATUS_ENUM)
    column_non_nullable(parcels_created, REPORT_STATUS_ENUM)
    column_non_nullable(editing_completed, REPORT_STATUS_ENUM)
}

has_one(items_progress_report_caches, items)
```
