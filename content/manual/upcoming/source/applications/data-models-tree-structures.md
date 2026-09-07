# Model Tree Structures

MongoDB allows various ways to use tree data structures to model large
hierarchical or nested data relationships.

/tutorial/model-tree-structures-with-parent-references  
Presents a data model that organizes documents in a tree-like
structure by storing references
to "parent" nodes in "child" nodes.

/tutorial/model-tree-structures-with-child-references  
Presents a data model that organizes documents in a tree-like
structure by storing references
to "child" nodes in "parent" nodes.

/tutorial/model-tree-structures-with-ancestors-array  
Presents a data model that organizes documents in a tree-like
structure by storing references
to "parent" nodes and an array that stores all ancestors.

/tutorial/model-tree-structures-with-materialized-paths  
Presents a data model that organizes documents in a tree-like
structure by storing full relationship paths between documents. In
addition to the tree node, each document stores the `_id` of the
nodes ancestors or path as a string.

/tutorial/model-tree-structures-with-nested-sets  
Presents a data model that organizes documents in a tree-like
structure using the *Nested Sets* pattern. This optimizes
discovering subtrees at the expense of tree mutability.
