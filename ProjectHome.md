# What does it do? #
This template generates C# code that makes it possible to avoid magic strings when calling stored procedures using BLToolkit library.

## Code comparison ##
This is the usual way of calling stored procedures with BLToolkit library:
```
using(var db = new DbManager())
{
  return db
    .SetSpCommand(
      "Person_SaveWithRelations",
      db.Parameter("@Name", name),
      db.Parameter("@Email", email),
      db.Parameter("@Birth", birth),
      db.Parameter("@ExternalID", exId),
    )
    .ExecuteObject<Person>();
}
```
The bad side is there are lots of magic strings here and in case we are part of a larger team where every developer does their own bit, this can become messy. There's no compile time checking or anything here.

### After we use this T4 template ###
After we add this template to our project (and provide values to its variables) we get compile time checking, intellisense and strong typed parameters for out stored procedures like:
```
using (var db = new DataManager())
{
  return db
    .Person
    .SaveWithRelations(
      name,
      email,
      birth,
      exId
    )
    .ExecuteObject<Person>();
}
```

## Stored procedure naming conventions ##
The upper example needs a stored procedure with this name:
`Person_SaveWithRelations`