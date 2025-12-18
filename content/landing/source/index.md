* goal
  * how to
    * store data | flexible documents
    * create an Atlas deployment
    * use our tools and integrations

* [sections](includes/docs-toc.md)

* MongoDB
  * == database /
    * document-oriented
    * operational
    * built from the ground up 
    * strong consistency -- with -- ACID transactions
    * built-in query capabilities 
      * _Examples:_ geospatial search, lexical search, and vector search
  * ⚠️vs relational database⚠️
    * allows
      * store rich JSON-like documents

        ```
        {
        firstname: "Bob",
        lastname: "Smith",
        email: "bob@smith.com",
          address: {
              street: "100 Main St",
              city: "Anytown",
              state: "MO",
              zip: "11111"
          }
        } 
        ```

  * uses
    * enterprise environments
      * Reason:🧠provide security primitives🧠 
    * ALL major clouds
  * provides
    * serverless horizontal scaling

How This Documentation is Organized
-----------------------------------

* [Get Started](get-started)
      
* [Development](development.md)

* [Management](management.md)

* [Client Libraries](../../drivers/README.md)

* [Tools](tools-and-connectors.md)

* [Atlas Architecture Center](../../atlas-architecture/current/README.md)
