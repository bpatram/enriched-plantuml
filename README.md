# Enriched PlantUML

Enriched PlantUML is a library for PlantUML to enhance the default appearance of your diagrams as well introducing new diagram types and paradigms for writing software documentation and technical specifications.

Features:

-   Improved default styling for standard diagram types
-   Additional macros for common formatting adjustments
-   Insert formatted title blocks
-   Insert company copyrights and confidentiality notices
-   Macros for entity-relationship diagrams (ERD)

## Getting Started

At the top of your PlantUML file, you need to include `enriched.puml`. By default, nothing will immediately happen to your diagram, you must opt-in to applying individual features. To do so, you can easily bootstrap a new diagram by calling `Enrich("<diagram type>")`.

The beginning of your diagram should look something like this:

```puml
!include_once https://raw.githubusercontent.com/bpatram/enriched-plantuml/master/enriched.puml

Enrich("<diagram-type>", $title = "My Diagram")
```

### Styling

When calling `Enrich` you should pass in a diagram type as the first argument in order to apply the correct styles for that diagram type. The current possible diagram types you can style are:

-   `"sequence"`
-   `"activity"`
-   `"state"`
-   `"class"`
-   `"usecase"`
-   `"object"`
-   `"deployment"`
-   `"er"`
-   `"generic"`
-   Leaving it blank will only apply basic styling, not specific to any diagram type

### Formatting helpers

-   `$use_word_wrap()`: Apply word wrapping to text within boxes and titles
-   `$use_horizontal_layout()`: Rotate the layout from top down to right to left

### Inserting title blocks

Currently, title blocks are formatted as a header that appears in the top right of your diagram. This information will only be shown if you define certain arguments when calling `Enrich`.

Here is a list of named arguments you can define when calling Enrich that relate to the title block:

-   `$author = "Your Name"` (optional)
-   `$revision = "Rev. 1"` (optional)

```puml
Enrich("<diagram type>", $author = "Your Name", $revision = "1")
```

### Inserting copyright and confidentiality notices

Currently, copyright and confidentiality notices are formatted as a footer that appears in the bottom center of your diagram. By default, all diagrams are treated as confidential. You can define a company name and/or override the default behavior with some additional arguments passed when calling `Enrich`.

Here is a list of named arguments you can define when calling Enrich that relate to copyright/confidentiality:

-   `$company = "Your Employer"` (optional)
-   `$confidential = 0` (optional)

```puml
Enrich("<diagram type>", $company = "My Company", $confidential = 0)
```

---

## Entity-relationship diagrams

Enriched PlantUML extends the default class diagram syntax that PlantUML offers to more easily aid in diagraming database schemas. When building ER diagrams you'll want to make sure your diagram type is `"er"` when calling `Enrich` to ensure styles are applied correctly. Below goes into more depth on what all Enriched adds and how to use it for ER diagrams.

### Assumptions/defaults

There are some assumptions made by this library when generating ER diagrams. These defaults can be overridden but have been optimized for diagraming databases created using ActiveRecord (a popular ORM for Ruby on Rails applications). You can call `SET_SCHEMA_DEFAULTS` with the following named arguments to override them.

Below is a list of the named arguments you can pass along with the current defaults used:

-   `$id_name = "id"`: Default PK column name when calling `table_pk()`.
-   `$id_type = "INTEGER(11)"`: Default PK/FK datatype when creating PK/FK columns.
-   `$fk_suffix = "_id"`: Default text to append to the polymorphic name when creating FK column.
-   `$poly_type_type = "VARCHAR"`: Default datatype for polymorphic "type" columns.
-   `$poly_type_suffix = "_type"`: Default text to append to the polymorphic name when creating type column.

Below is a simple example of changing the default to use a "UUID" datatype instead of sequential IDs for PK/FKs:

```puml
SET_SCHEMA_DEFAULTS($id_type="UUID")
```

### Tables

To define a new table, use the `Table` element. To set the table name pass it in as the first argument. Within your table you can use additional macros to define the columns your table and to associate tables together (read on).

```puml
Table(your_table_name) {
    ---
    ' define your fields here...
}
```

### Primary keys

Define a primary key field within your table using `table_pk()`. By default the field will be called 'id' with a datatype of 'INTEGER(11)' but you can override this behavior by passing in arguments or by changing the schema defaults via `SET_SCHEMA_DEFAULTS`

```puml
' to create a PK named 'id' (based on schema defaults)
table_pk()

' or with a custom field name
table_pk('identifier')

' or with a custom datatype
table_pk('id', 'UUID')
```

### Foreign keys

Define a foreign key field within your table using `table_fk()`. You'll need to define the column name by passing in a name as the first argument. By default the datatype will be 'INTEGER(11)' but you can override this behavior by passing in a second argument or by changing the schema defaults via `SET_SCHEMA_DEFAULTS`. By default, FK columns are non-nullable, you can override this behavior by passing in a `$nullable` named argument and setting it to true.

```puml
' to create a FK named 'other_table_id'
table_fk(other_table_id)

' or with a custom datatype
table_fk(other_table_id, 'UUID')

' or if it is nullable
table_fk(other_table_id, $nullable=1)
```

### Table columns

Define an ordinary field with `table_column()`. The first argument sets the column name and the second argument set its datatype. You can pass additional data, like if it is nullable or not using named arguments. By default columns are nullable, you can override this by passing in a `$nullable` named argument and setting it to true.

```puml
' to create a nullable field (default behavior)
table_column(column_name, 'DATETIME')

' to create a non-nullable field
table_column(column_name, 'DATETIME', $nullable=0)
```

### Timestamp columns

In most applications it is common to include some additional fields on your table to store "created_at" and "updated_at" timestamps (and sometimes "destroyed_at"). Instead of having to declare these fields on your table separately via `table_column()`, you can instead use `table_timestamps()`. You can configure which timestamp columns to add via named arguments.

```puml
' to add 'created_at' and 'updated_at' timestamp columns (default behavior)
table_timestamps()

' to add 'destroyed_at' timestamp column (in addition to created_at and updated_at)
table_timestamps($destroyed_at=1)

' to add only a 'created_at' timestamp column
table_timestamps($updated_at=0)
```

### Partial Tables

Often times you will find yourself only needing to diagram a single area of concern in your schema and not the entire schema. In this case there are some handy helpers provided. In the scenario where you just need to reference a table and handwave over its entire shape, you can use the `PartialTable` element. If you are diagraming out a table but want to handwave over irrelevant columns you can use `table_omission()`.

```puml
' creates a table with just a PK and 'omitted columns'
PartialTable(users)

' or to create a table with a mix of omitted and defined columns
Table(users) {
    table_pk()
    table_timestamps()
    table_omission()
    table_fk(foo_id)
}
```

### Relationships

To define a many to one relationship use `has_one()`.
To define a many to many relationship use `has_many()`.

```puml
Table(books) {
    ---
    table_pk()
    table_fk(author_id)
}

Table(authors) {
    ---
    table_pk()
}

has_one(books, authors)
```

### Polymorphic Relationships

Polymorphic relationships are a non-standard concept when talking about modeling a data schema. However, most ORMs support this type of relationship so modeling them in your ER diagrams is a common challenge. Since a polymorphic relationship represents a relationship to differing tables we can represent the polymorphic relationship as an intermediate object between the table containing the foreign key and the tables that it could possibly be referencing. Below is a complete example to show how this works in practice.

Define a polymorphic foreign key and foreign key type fields on your table via `table_poly_fk()`.
Relate your table with the polymorphic fields to the intermediate object via `has_one_poly()`.
Define the tables that your polymorphic relationship could reference via `poly_can_be()`.

```puml
Table(items) {
    ---
    table_pk()

    table_poly_fk(ownable)
}

PartialTable(users)
PartialTable(companies)

has_one_poly(items, ownable)
poly_can_be(ownable, users)
poly_can_be(ownable, companies)

```

### Junction Tables

Joining two tables is pretty common so there is an element to make this less tedious, `JunctionTable`. By default this will create a table with both tables names concatenated together, a primary key (optional) and two foreign keys to each table.

```puml
PartialTable(users)
PartialTable(companies)

JunctionTable(users, companies)

' or as a non-identifying junction table
JunctionTable(users, companies, $table_pk=0)
```

### Enums

To represent enums in your schema, you can define an `EnumType` element. Pass in the enum name and its datatype as the first and second arguments. It appears similar to a table in your diagram but will be styled a bit differently and will contain the values of your enum.

To define your enum values use `enum_value()` and pass the name and value as the first and second arguments.

```puml
EnumType(MY_ENUM_NAME, "INT(11")) {
    enum_value(my_value_one, 1)
    enum_value(my_value_two, 2)
}
```

### Table grouping

When diagraming larger schemas you may want to group tables by domain to more clearly organize things. You can use the `TableLayer` element to do so. When you have multiple groups in a single diagram you may want to more easily make a distinction between them, to do so you can set a background color via the `$color` named argument.

```puml
' create a group with just a name
TableLayer("Identity") {
    PartialTable(users)
    PartialTable(access_logs)
}

' create a group and set a color
TableLayer("Inventory", $color="lightblue") {
    PartialTable(items)
}

has_one(items, user)
```
