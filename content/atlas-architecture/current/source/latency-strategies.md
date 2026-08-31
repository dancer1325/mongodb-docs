# Guidance for Atlas Latency Reduction

The following sections list configuration choices you can make to reduce latency in your Atlas deployment.

## Physical Distance

Physical distance is the primary cause of latency. The distance between users and your application, between your application and your data, and between cluster nodes all impact system latency and application performance. To reduce latency for read and write operations, it's crucial to place both your application and data geographically closer to users. Data-bearing Atlas nodes are server nodes within an Atlas cluster that store your database's data and handle read and write operations. To support faster data access for your application users, deploy data-bearing Atlas nodes to cloud provider regions that are geographically close to the majority of your application users. If your application's users are distributed across multiple geographies, such as between the US and Europe, we recommend that you deploy to one or more regions in each geography to reduce latency for users in each location. To learn more about multi-region deployments, see arch-center-paradigms-multi-region. If your data is divided by geography, such that users in each geography access different sets of data, you can also [shard](/sharding) your data by region or geography in order to optimize read and write performance for users in each geography. This approach allows you to handle large data sets and high throughput while ensuring data locality.

## Replication Configuration

[Replication](/replication) is the copying of data from the primary node to secondary nodes. The following replication configuration options can be adjusted to minimize latency for read and write operations in a replica set:

- **Write Concern Levels:** There is a trade-off between write latency and write durability when you configure [write concern](/reference/write-concern). MongoDB's default global write concern is set to `majority`, which requires each write operation to be replicated across the majority of voting, data-bearing nodes in your replica set before Atlas acknowledges the operation as complete and successful to the client. Setting a higher write concern increases write latency, but also improves write durability and prevents rollbacks during a replica set failover.
- **Read Concern and Read Preference**: There is a trade-off between query latency, data availability, and the consistency of your query responses when you configure [read concern](/reference/read-concern) and [read preference](/core/read-preference). MongoDB's default global read concern is `local`, which requires read operations to read from only one node in the local replica set without waiting to confirm that data is replicated across other nodes. Whether this node is the primary or a secondary node is determined by your read preference, which Atlas sets to `primary` by default. This default read concern and preference combination optimizes for the lowest latency reads from the most up-to-date node in the replica set, but it also runs the risk that the primary node's data may not be durable and can potentially be rolled back during a failover, and that users will not be able to query data if there is no available primary node. You can change your read preference to `primaryPreferred` to allow read operations to read from a secondary node when there is no available primary node, or if there is a geographically closer secondary node, but this runs the risk of returning stale data if a secondary node is not up-to-date. This risk can be mitigated by increasing your [write concern](/reference/write-concern) to ensure that more secondary nodes are up-to-date, with the trade-off that this increases your write latency.

   > **Important:**
   > Keep in mind that there is the possibility of a secondary node returning stale data due to replication lag.

- **Query Timeout Limits**: You can set global and operation-level [query timeout limits](/tutorial/query-documents/specify-query-timeout/) on your deployment to reduce the amount of time your application waits for a response before timing out. This can prevent ongoing queries from negatively impacting deployment performance for long periods of time.
- **Node Election Priority**: To increase the likelihood that the members in your main data center be elected primary before the members in an alternate data center during a [replica set election](/core/replica-set-elections), you can set the members[n].priority of the members in the alternate data center to be lower than that of the members in the primary data center. For example, if you deploy your cluster across the AWS (Amazon Web Services) regions `us-east-1` (Northern Virginia) and `us-west-1` (Northern California), and the majority of your users are in California, you can prioritize nodes in the AWS (Amazon Web Services) `us-west-1` (Northern California) region to ensure that the primary node is always geographically close to the majority of your users and can respond to read and write operations with minimal latency.
- **Mirrored Reads:** Mirrored reads reduce the impact of primary elections following an outage by pre-warming the caches on the secondary nodes. For more information, see [Mirrored Reads](/replication/#std-label-mirrored-reads).

For more guidance on implementing the best replication configuration for your needs, contact MongoDB's [Professional Services](https://www.mongodb.com/services/consulting).

## Network Configuration

You can increase security and further reduce latency using the following network connectivity options:

- **Private Endpoints:** [Private endpoints](/data-federation/admin/manage-private-endpoint/) establish direct and secure connections between your application's virtual network and your Atlas cluster, potentially reducing network hops and improving latency.
- **VPC Peering:** Configure [VPC peering](/atlas-stream-processing/manage-vpc-peering-connections/) in your replica sets to allow applications to connect to peered regions. In the event of a failover, VPC peering provides a way for an application server to detect a new primary node and route traffic accordingly.

## Data Modeling and Query Optimization

The speed at which your application accesses data contributes to latency. Good [data modeling](/data-modeling) and [query optimization](/atlas-search/performance/query-performance/) can improve data access speeds. For example, you can:

- **Reduce Document Size:** Consider shortening field names and value lengths to decrease the amount of data transferred over the network.
- **Optimize Query Patterns:** Use indexes effectively to minimize the amount of data that needs to be read across regions.

## Monitoring and Testing Latency

Atlas provides the [Real-Time Performance Panel (RTPP)](/real-time-performance-panel/) to observe latency metrics for different regions. You can also implement application-level monitoring to track end-to-end latency to and from the application. Before final production deployment, we suggest conducting performance testing under the various multi-region scenarios to identify and address latency bottlenecks. To learn more about monitoring your deployment, see arch-center-monitoring-alerts.

## Connection Configuration

We recommend that you use a connection method built on the most current driver version for your application's programming language whenever possible. While the default connection string Atlas provides is a good place to start, you can add [connection string options](/reference/connection-string-options/) to your connection string to improve performance in the context of your specific application and deployment architecture. For enterprise-level application deployments, it's especially important to [tune your connection pool settings](/tutorial/connection-pool-performance-tuning/) to meet user demand while minimizing operational latency. For example, you can use the `minPoolSize` and `maxPoolSize` options to adjust how and when the majority of your database client connections open, allowing you to prevent or plan for the latency spikes that come with the associated network overhead. The extent to which you can configure these settings depends on your deployment architecture. For example, if your application deployment is leveraging single-threaded resources, like AWS (Amazon Web Services) Lambda, your application will only ever be able to open and use one client connection. To learn more about how and where to create and use a connection pool, and where to specify connection pool settings, see [Connection Pool Overview](/administration/connection-pool-overview/).

## Example Low-Latency Application

The following sample application brings together key recommendations on this page to reduce data operation latency:

- Use the Atlas-provided connection string with retryable writes, majority write concern, and default read concern.
- Specify an operation time limit with the [maxTimeMS](/tutorial/terminate-running-operations/#maxtimems) method. For instructions on how to set `maxTimeMS`, refer to your specific Driver Documentation.
- Handle errors for duplicate keys and timeouts.

The application is an HTTP API (Application Programming Interface) that allows clients to create or list user records. It exposes an endpoint that accepts GET and POST requests http://localhost:3000:

| Method | Endpoint | Description |
| --- | --- | --- |
| `GET` | `/users` | Gets a list of user names from a `users` collection. |
| `POST` | `/users` | Requires a `name` in the request body. Adds a new user to a `users` collection. |

****

> **Note:**
> The following server application uses [Express](https://github.com/expressjs/express), which you need to add to your project as a dependency before you can run it.

```javascript
const express = require('express');
const bodyParser = require('body-parser');

// Use the latest drivers by installing & importing them
const MongoClient = require('mongodb').MongoClient;

const app = express();
app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: true }));

const uri = "mongodb+srv://<db_username>:<db_password>@cluster0-111xx.mongodb.net/test?retryWrites=true&w=majority";

const client = new MongoClient(uri, {
    useNewUrlParser: true,
    useUnifiedTopology: true
});

// ----- API routes ----- //
app.get('/', (req, res) => res.send('Welcome to my API!'));

app.get('/users', (req, res) => {
    const collection = client.db("test").collection("users");

    collection
    .find({})
    .maxTimeMS(5000)
    .toArray((err, data) => {
        if (err) {
            res.send("The request has timed out. Please check your connection and try again.");
        }
        return res.json(data);
    });
});

app.post('/users', (req, res) => {
    const collection = client.db("test").collection("users");
    collection.insertOne({ name: req.body.name })
    .then(result => {
        res.send("User successfully added!");
    }, err => {
        res.send("An application error has occurred. Please try again.");
    })
});
// ----- End of API routes ----- //

app.listen(3000, () => {
    console.log(`Listening on port 3000.`);
    client.connect(err => {
        if (err) {
            console.log("Not connected: ", err);
            process.exit(0);
        }
        console.log('Connected.');
    });
});
```

---

****

> **Note:**
> The following web application uses [FastAPI](https://github.com/tiangolo/fastapi). To create a new application, use the [FastAPI sample file](https://github.com/tiangolo/fastapi#example) structure.

```python
# File: main.py

from fastapi import FastAPI, Body, Request, Response, HTTPException, status
from fastapi.encoders import jsonable_encoder

from typing import List
from models import User

import pymongo
from pymongo import MongoClient
from pymongo import errors

# Replace the uri string with your Atlas connection string
uri = "<atlas-connection-string>"
db = "test"

app = FastAPI()

@app.on_event("startup")
def startup_db_client():
    app.mongodb_client = MongoClient(uri)
    app.database = app.mongodb_client[db]

@app.on_event("shutdown")
def shutdown_db_client():
    app.mongodb_client.close()

##### API ROUTES #####
@app.get("/users", response_description="List all users", response_model=List[User])
def list_users(request: Request):
    try: 
        users = list(request.app.database["users"].find().max_time_ms(5000))
        return users
    except pymongo.errors.ExecutionTimeout: 
        raise HTTPException(status_code=status.HTTP_503_SERVICE_UNAVAILABLE, detail="The request has timed out. Please check your connection and try again.")

@app.post("/users", response_description="Create a new user", status_code=status.HTTP_201_CREATED)
def new_user(request: Request, user: User = Body(...)):
    user = jsonable_encoder(user)
    try: 
        new_user = request.app.database["users"].insert_one(user)
        return {"message":"User successfully added!"}
    except pymongo.errors.DuplicateKeyError:
        raise HTTPException(status_code=status.HTTP_400_BAD_REQUEST, detail="Could not create user due to existing '_id' value in the collection. Try again with a different '_id' value.")
```

---

****

> **Note:**
> The following server application uses [NanoHTTPD](https://github.com/NanoHttpd/nanohttpd) and [json](https://mvnrepository.com/artifact/org.json/json) which you need to add to your project as dependencies before you can run it.

```java
// File: App.java

import java.util.Map;
import java.util.logging.Logger;

import org.bson.Document;
import org.json.JSONArray;

import com.mongodb.MongoException;
import com.mongodb.client.MongoClient;
import com.mongodb.client.MongoClients;
import com.mongodb.client.MongoCollection;
import com.mongodb.client.MongoDatabase;

import fi.iki.elonen.NanoHTTPD;

public class App extends NanoHTTPD {
    private static final Logger LOGGER = Logger.getLogger(App.class.getName());

    static int port = 3000;
    static MongoClient client = null;

    public App() throws Exception {
        super(port);

        // Replace the uri string with your MongoDB deployment's connection string
        String uri = "<atlas-connection-string>";
        client = MongoClients.create(uri);

        start(NanoHTTPD.SOCKET_READ_TIMEOUT, false);
        LOGGER.info("\nStarted the server: http://localhost:" + port + "/ \n");
    }

    public static void main(String[] args) {
        try {
            new App();
        } catch (Exception e) {
            LOGGER.severe("Couldn't start server:\n" + e);
        }
    }

    @Override
    public Response serve(IHTTPSession session) {
        StringBuilder msg = new StringBuilder();
        Map<String, String> params = session.getParms();

        Method reqMethod = session.getMethod();
        String uri = session.getUri();

        if (Method.GET == reqMethod) {
            if (uri.equals("/")) {
                msg.append("Welcome to my API!");
            } else if (uri.equals("/users")) {
                msg.append(listUsers(client));
            } else {
                msg.append("Unrecognized URI: ").append(uri);
            }
        } else if (Method.POST == reqMethod) {
            try {
                String name = params.get("name");
                if (name == null) {
                    throw new Exception("Unable to process POST request: 'name' parameter required");
                } else {
                    insertUser(client, name);
                    msg.append("User successfully added!");
                }
            } catch (Exception e) {
                msg.append(e);
            }
        }

        return newFixedLengthResponse(msg.toString());
    }

    static String listUsers(MongoClient client) {
        MongoDatabase database = client.getDatabase("test");
        MongoCollection<Document> collection = database.getCollection("users");

        final JSONArray jsonResults = new JSONArray();
        collection.find().forEach((result) -> jsonResults.put(result.toJson()));

        return jsonResults.toString();
    }

    static String insertUser(MongoClient client, String name) throws MongoException {
        MongoDatabase database = client.getDatabase("test");
        MongoCollection<Document> collection = database.getCollection("users");

        collection.insertOne(new Document().append("name", name));
        return "Successfully inserted user: " + name;
    }
}
```

---
