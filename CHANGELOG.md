## Version 0.0.6

- Depend on [perpetuity](https://github.com/jgaskins/perpetuity) 1.0 final

## Version 0.0.5

- Name indexes explicitly based on table and attributes. Previously we were counting on the PostgreSQL naming convention, but that could break in future versions of Postgres.
- Fixed a bug that was causing duplicate indexes to be built
- Deserialize booleans. They should be declared in the mapper as `TrueClass` or `FalseClass`.
- Fixed a bug that choked when you tried to save an attribute for an object  if that column was not yet added.
- Allow deleting an array of objects
- Removed VARCHAR conversion. VARCHAR and TEXT are stored exactly the same in PostgreSQL. There's no reason to limit yourself to a VARCHAR unless you're using table constraints, which are far more flexible.

## Version 0.0.4

- Set minimum logging from the database to warnings. This silences info messages like those from `DROP TABLE IF NOT EXISTS ...` when the table doesn't exist.
- Add the ability to query on ids for objects. For example: `mapper.select { |article| article.author.id == params[:author_id] }`

## Version 0.0.3

- Require the `bigdecimal` library to avoid undefined constant error: BigDecimals are converted to the Postgres `numeric` data type. In 1.9, it needs to be required explicitly. — [Avdi Grimm](https://github.com/avdi)
- Properly return the connection to the connection pool if an exception is raised in the query.

## Version 0.0.2

- Make DB creation call out to remote DB to create it instead of calling on localhost.
- Add index support
- Only load UUID extension on UUID failure. This keeps the DB log from getting flooded every time the app connects to it.
- Add support for any?/none? on query attributes for queries like `mapper.select { |user| user.friends.any? }`
- Serialize nil values in JSONHash
- Add support for in-place increment/decrement. This allows you to use a mapper's `#increment`/`#decrement` methods to modify the object without pulling it down from the DB first.
- Keep first value when inserting multiple records. This fixes a bug where a `SerializedData` value (the data structure holding what's being inserted) would modify itself in place on `each` calls.
- Cast numeric ids to numeric types on insertion. Previously, if you had an `Integer` id column, it was being returned as a string.
- Don't allow double quotes in table names. Postgres doesn't allow it, so this catches it before we have to make the DB call.
- Add support for more numeric types
- Escape double quotes in JSON strings
- Add table columns automatically if they don't exist. This way, you can simply add more attributes in your mapper and Perpetuity::Postgres will take care of it on its own.

## Version 0.0.1

- Initial release.
- Most Perpetuity CRUD features enabled.
