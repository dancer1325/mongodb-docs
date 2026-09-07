# Self-Managed Replica Set Maintenance Tutorials

The following tutorials provide information in maintaining existing
replica sets.

/tutorial/change-oplog-size  
Increase the size of the oplog which logs operations. In
most cases, the default oplog size is sufficient.

/tutorial/perform-maintence-on-replica-set-members  
Perform maintenance on a member of a replica set while minimizing
downtime.

/tutorial/force-member-to-be-primary  
Force a replica set member to become primary.

/tutorial/resync-replica-set-member  
Sync the data on a member. Either perform initial sync on a new
member or resync the data on an existing member that has fallen
too far behind to catch up by way of normal replication.

/tutorial/configure-replica-set-tag-sets  
Assign tags to replica set members for use in targeting read and
write operations to specific members.

/tutorial/reconfigure-replica-set-with-unavailable-members  
Reconfigure a replica set when a majority of replica set members
are down or unreachable.

/tutorial/manage-chained-replication  
Disable or enable chained replication. Chained replication occurs
when a secondary replicates from another secondary instead of the
primary.

/tutorial/change-hostnames-in-a-replica-set  
Update the replica set configuration to reflect changes in
members' hostnames.

/tutorial/configure-replica-set-secondary-sync-target  
Specify the member that a secondary member synchronizes from.

/tutorial/rename-unsharded-replica-set  
Rename an unsharded replica set.

/tutorial/modify-psa-replica-set-safely  
Safely perform some reconfiguration changes on a
primary-secondary-arbiter (PSA) replica set or on a replica set that
is changing to a PSA architecture.

/tutorial/mitigate-psa-performance-issues  
Reduce cache pressure and increased write traffic for a deployment
that has a three-member primary-secondary-arbiter (PSA) architecture.
