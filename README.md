# Enriched PlantUML

Enriched PlantUML is a library for PlantUML to enhance the default appearance of your diagrams as well introducing new diagram types and paradigms for writing software documentation and technical specifications.

Features:

-   Improved default styling for standard diagram types
-   Additional macros for common formatting adjustments
-   Insert formatted title blocks
-   Insert company copyrights and confidentiality notices
-   Macros for entity-relationship diagrams (ERD)

## Getting Started

At the top of your PlantUML file, you need to include `enriched.puml`. By default, nothing will immediately happen to your diagram, you must opt-in to applying individual features. To easily bootstrap a new diagram to apply the correct styles and insert document information, copyright, and confidentiality notices you can call `$setup_std_diagram("<diagram type>")`. The beginning of your diagram should look something like this:

```puml
!include_once https://raw.githubusercontent.com/bpatram/enriched-plantuml/master/enriched.puml

!$title = "My Diagram"
!$company_name = "ACME Corp"
!$author_name = "John Smith"
!$revision_name = "1"

$setup_std_diagram("<diagram-type>")
```

### Styling

When calling `$setup_std_diagram` you must pass in a diagram type name as the first argument in order to apply the correct styles for that diagram type. The current possible diagram types you can style are:

-   `$setup_std_diagram("sequence")`
-   `$setup_std_diagram("activity")`
-   `$setup_std_diagram("state")`
-   `$setup_std_diagram("class")`
-   `$setup_std_diagram("usecase")`
-   `$setup_std_diagram("object")`
-   `$setup_std_diagram("deployment")`
-   `$setup_std_diagram("er")`
-   `$setup_std_diagram("generic")`
-   `$setup_std_diagram()` (same as `$setup_std_diagram("generic")`)

### Formatting helpers

-   `$use_word_wrap()`: Apply word wrapping to text within boxes and titles
-   `$use_horizontal_layout()`: Rotate the layout from top down to right to left

### Inserting title blocks

When calling `$setup_std_diagram` it will attempt to insert a title block for you. You must define these variables _before_ making that call for it to work correctly.

You'll want to define the following:

-   `!$author_name = "Your Name"`
-   `!$revision_name = "Rev. 1"` (optional)

### Inserting copyright and confidentiality notices

When calling `$setup_std_diagram` it will attempt to insert a footer with company information and to identify if your diagram is confidential or not. When you define these variables it must be done _before_ making the call to `$setup_std_diagram`.

You can insert the name of your company or employer by defining the $company_name variable:

```puml
!$company_name = "Your Company"
```

By default, all diagrams are marked as confidential. To mark your diagram as not confidential:

```puml
!$confidential = %false()
```

### Entity-relationship diagrams

When building ER diagrams you'll want to make sure your diagram type is `"er"` when calling `$setup_std_diagram` to ensure styles are applied correctly.

#### Tables

To define a new table, use the `table` object. To set the table name pass it in as the first argument. Within your tables you can use additional macros to define the columns or fields on your table.

```puml
table(your_table_name) {
    ---
    ' define your fields...
}
```

#### Fields

In most databases the identifier or reference columns use the same datatype across all tables. By default primary and foreign keys are assumed to be an INTEGER(11) datatype, however you can change this behavior by overwriting `PK_TYPE` value to something else. If you use UUIDs for example you will want to do something like this:

```puml
!define PK_TYPE UUID
```

Define a primary key field within your table using `column_pk()`. By default the field will be called 'id' but you can override this behavior by passing in a new name as the first argument. If its datatype differs from `PK_TYPE` (`INTEGER(11)` by default) you can define a new datatype by passing a second argument.

```puml
' to create a PK named 'id'
column_pk()

' or with a custom field name
column_pk('identifier')

' or with a custom datatype
column_pk('id', 'UUID')
```

Define a foreign key field within your table using `column_fk()`. You'll need to define the column name by passing in a name as the first argument. If its datatype differs from `PK_TYPE` (`INTEGER(11)` by default) you can define a new datatype by passing a second argument.

```puml
' to create a FK named 'other_table_id'
column_fk(other_table_id)

' or with a custom datatype
column_fk(other_table_id, 'UUID')
```

Define an ordinary field with either `column_non_nullable()` or `column_nullable()`. The first argument sets the column name and the second argument set its datatype.

```puml
' to create a non-nullable field
column_non_nullable(column_name, 'DATETIME')

' to create a nullable field
column_nullable(column_name, 'DATETIME')
```

When making diagrams which only highlights a specific area of concern of your schema you may want to only need to partially define your tables. There is a handy macro, `omitted_columns()` to indicate that you are omitting some fields on your table.

In most applications it is common to include some additional fields on your table to store "created_at" and "updated_at" timestamps. Instead of having to declare these fields on your table separately, you can instead simply use the `timestamps()` macro.

#### Relationships

To define a many to one relationship use `has_one()`.
To define a many to many relationship use `has_many()`.

```puml
table(books) {
    ---
    column_pk()
    column_fk(author_id)
}

table(authors) {
    ---
    column_pk()
}

has_one(books, authors)
```

#### Polymorphic Relationships

Polymorphic relationships are a non-standard concept when talking about modeling a data schema. However, most ORMs support this type of relationship so modeling them in your ER diagrams is a common challenge. Since a polymorphic relationship represents a relationship to differing tables we can represent the polymorphic relationship as an intermediate object between the table containing the foreign key and the tables that it could possibly be referencing. Below is a complete example to show how this works in practice.

Define a polymorphic foreign key and foreign key type fields on your table via `column_fk_poly()`. Define your polymorphic relationship intermediate object via `poly_assoc()`.
Relate your table with the polymorphic fields to the intermediate object via `has_one_poly()`.
Define the tables that your polymorphic relationship could reference via `poly_can_be()`.

```puml
table(items) {
    ---
    column_pk()

    column_fk_poly(ownable)
}

poly_assoc(ownable)

table(users) {
    ---
    column_pk()
    omitted_columns()
}

table(companies) {
    ---
    column_pk()
    omitted_columns()
}

has_one_poly(items, ownable)
poly_can_be(ownable, users)
poly_can_be(ownable, companies)

```

#### Enums

To represent enums in your schema, you can define an `enum_mapping` object. Pass in the enum name and its datatype as the first and second arguments. It appears similar to a table in your diagram but will be styled a bit differently and will contain the values of your enum.

To define your enum values use `enum_value()` and pass the name and value as the first and second arguments.

```puml
enum_mapping(MY_ENUM_NAME, INTEGER) {
    enum_value(my_value_one, 1)
    enum_value(my_value_two, 2)
}
```
