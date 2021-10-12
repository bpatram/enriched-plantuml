# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Changed

-   Diagrams no longer default to confidential
-   `table_column` will default to `'VARCHAR'` data type if unspecified

## [3.1.0] - 2021-05-08

### Added

-   `JunctionTable` element for quickly making a junction table between two tables

### Changed

-   Renamed `$poly_id_suffix` to `$fk_suffix` argument in `SET_SCHEMA_DEFAULTS`
-   Setting the diagram type when calling `Enrich` is no longer case sensitive

## [3.0.1] - 2021-05-04

### Fixed

-   Missing revision name in title block

## [3.0.0] - 2021-05-04

### Added

-   `PartialTable` element type for quickly defining a Table with omitted columns and a PK
-   `TableLayer` element for grouping tables together
-   Control which timestamp table columns are added when calling `table_timestamps()`
-   Support for "destroyed_at" timestamp table column
-   Setting and presenting table column nullability as a data type

### Changed

-   `$setup_std_diagram` renamed to `Enrich`
-   Define diagram info via names arguments to `Enrich` instead of global variables
-   `PK_TYPE` replaced by calling `SET_SCHEMA_DEFAULTS` with named argument `$id_type`
-   `table` element renamed to `Table` (case-sensitive)
-   `enum_mapping` element renamed to `EnumType` (case-sensitive)
-   `poly_assoc` merged into `has_one_poly`
-   `timestamps` renamed to `table_timestamps`
-   `omitted_columns` renamed to `table_omission`
-   `column_fk_poly` renamed to `table_poly_fk`
-   Omitted columns on tables is more clear
-   Table foreign keys and polymorphic columns are non-nullable by default

## [2.5.0] - 2021-04-29

### Added

-   Use `enum_value('enum name', 'enum value')` when defining `enum` objects
-   Ability to model polymorphic relationships in ER diagrams

### Changed

-   ER diagrams to use orth/straight lines by default
-   Improvements to container shape styles
-   Improvements to ER table field styling
-   Hide the merge diamond on conditional flows in activity diagrams by default

## [2.4.0] - 2021-04-28

### Added

-   Support for Deployment diagrams via new diagram type "deployment"

## [2.3.0] - 2021-04-26

### Added

-   Support for Object diagrams via new diagram type "object"

## [2.2.0] - 2021-04-26

### Added

-   Support for Use Case diagrams via new diagram type "usecase"

## [2.1.0] - 2021-04-26

### Added

-   Ability to retheme diagrams by overwriting `$theme_data` json data

### Changed

-   Renamed `$setup_std_diagram_for` to `$setup_std_diagram`
-   No longer need to set a diagram type when bootstrapping generic diagrams
-   Improvements to footer styling when company information is missing
-   Setting `$title` is no longer required

## [2.0.0] - 2021-03-23

### Added

-   Easier way to bootstrap your diagram via `$setup_std_diagram_for("<diagram-type>")`
-   Must define diagram type when applying styles
-   `$use_horizontal_layout()` layout helper
-   Styling for all builtin PlantUML shapes and objects
-   Ability to mark diagrams as non-confidential (confidential by default)
-   Support for setting a diagram revision number (via `$revision_name`)
-   Toggle to hide diagram title, headers, and footer when using `$setup_std_diagram_for`

### Changed

-   Requires PlantUML v2021 and higher
-   `USE_WORD_WRAP()` now called `$use_word_wrap()`
-   `USE_DEFAULT_STYLES()` replaced by `$apply_std_style()`
-   `USE_ERD_STYLES()` replaced by calling `$setup_std_diagram_for("er")`
-   `header STD_HEADER` replaced by `$setup_std_diagram_for`
-   `footer STD_FOOTER` replaced by `$setup_std_diagram_for`
-   No need to call `title` when using `$setup_std_diagram_for` with `$title` set
-   `TODAY` replaced by `$today`
-   `COMPANY_NAME` replaced by `$company_name`
-   `AUTHOR_NAME` replaced by `$author_name`
-   Refined styling for consistency across all diagram types
-   Sequence diagrams will default to `autoactivate on` and `autonumber`
-   Actors will use non-stick variant (except in sequence diagrams)
-   State shapes to use compact shape if they have no description text

## [1.0.0] - 2019-02-18

### Added

-   First release of Enriched!
-   This CHANGELOG file to hopefully serve as an evolving example of a
    standardized open source project CHANGELOG.
-   README now contains how to setup and use Enriched
