# Array Update Operators

## Update Operators

- - Name
  - Description
- - `\$`
  - Acts as a placeholder to update the first element that matches the query condition.
- - `\$\[\]`
  - Acts as a placeholder to update all elements in an array for the documents that match the query condition.
- - `\$\[\<identifier\>\]`
  - Acts as a placeholder to update all elements that match the `arrayFilters` condition for the documents that match the query condition.
- - `\$addToSet`
  - Adds elements to an array only if they do not already exist in the set.
- - `\$pop`
  - Removes the first or last item of an array.
- - `\$pull`
  - Removes all array elements that match a specified query.
- - `\$push`
  - Adds an item to an array.
- - `\$pullAll`
  - Removes all matching values from an array.

## Update Operator Modifiers

- - Name
  - Description
- - `\$each`
  - Modifies the `\$push` and `\$addToSet` operators to append multiple items for array updates.
- - `\$position`
  - Modifies the `\$push` operator to specify the position in the array to add elements.
- - `\$slice`
  - Modifies the `\$push` operator to limit the size of updated arrays.
- - `\$sort`
  - Modifies the `\$push` operator to reorder documents stored in an array.
