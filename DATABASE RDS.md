# Can promote a RDS instance read replica to become a standalone primary instance using the PromoteReadReplica
  - Breaks the replica relationship with the primary instance
  - Converts fully replica into a fully writable, standalone a primary instance 
  - Allows the premoted instance can accept both read and write operation

Use for case DR scenario, when the primary instance failed, for quicky restore in another region