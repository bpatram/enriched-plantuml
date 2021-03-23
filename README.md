# Enriched PlantUML

Enriched PlantUML is a library for PlantUML to enhance the default appearance of your diagrams as well introducing new diagram types and paradigms for writing software documentation and technical specifications.

Features:

-   Improved default styling for standard diagram types
-   Additional macros for common formatting adjustments
-   Insert formatted title blocks
-   Insert company copyrights and confidentiality notices
-   Macros for entity-relationship diagrams (ERD)

## Getting Started

At the top of your PlantUML file, you need to include `enriched.puml`. By default, nothing will immediately happen to your diagram, you must opt-in to applying individual features. To easily bootstrap a new diagram to apply the correct styles and insert document information, copyright, and confidentiality notices you can call `$setup_std_diagram_for("<diagram type>")`. The beginning of your diagram should look something like this:

```puml
!include_once https://raw.githubusercontent.com/bpatram/enriched-plantuml/master/enriched.puml

!$title = "My Diagram"
!$company_name = "ACME Corp"
!$author_name = "John Smith"
!$revision_name = "1"

$setup_std_diagram_for("<diagram-type>")
```

### Styling

When calling `$setup_std_diagram_for` you must pass in a diagram type name as the first argument in order to apply the correct styles for that diagram type. The current possible diagram types you can style are:

-   `$setup_std_diagram_for("sequence")`
-   `$setup_std_diagram_for("activity")`
-   `$setup_std_diagram_for("sequence")`
-   `$setup_std_diagram_for("state")`
-   `$setup_std_diagram_for("class")`
-   `$setup_std_diagram_for("er")`
-   `$setup_std_diagram_for("generic")`

### Formatting helpers

-   `$use_word_wrap()`: Apply word wrapping to text within boxes and titles
-   `$use_horizontal_layout()`: Rotate the layout of all directions to be horizontal

### Inserting title blocks

When calling `$setup_std_diagram_for` it will attempt to insert a title block for you. You must define these variables _before_ making that call for it to work correctly.

You'll want to define the following:

-   `!$author_name = "Your Name"`
-   `!$revision_name = "Rev. 1"` (optional)

### Inserting copyright and confidentiality notices

When calling `$setup_std_diagram_for` it will attempt to insert a footer with company information and to identify if your diagram is confidential or not. When you define these variables it must be done _before_ making the call to `$setup_std_diagram_for`.

You can insert the name of your company or employer by defining the $company_name variable:

```puml
!$company_name = "Your Company"
```

By default, all diagrams are marked as confidential. To mark your diagram as not confidential:

```puml
!$confidential = %false()
```

### Entity-relationship diagrams

When building ER diagrams you'll want to make sure your diagram type is `"er"` when calling `$setup_std_diagram_for` to ensure styles are applied correctly.

A new object type is added called `table`. You can use it like this:

```puml
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
