## S3
Two way replication rules for your Multi-Region Access Point
- Replication rule enable automatic and asynchronous copying of object across buckets. A two way replication rule ensures that data is fully synchronized betwenn two or more bucket in different region.

- You can not delete a bucket if the bucket is not empty

S3 Multi-Region Access Point: provide a single global endpoint to access s3 buckets across multiple region, simplying application access and enabling failover. Configuring it in an active-passive setup allows the application to primarily use the bucket in region(active) while maintaining the ability to failover to the bucket in an another region.
- To perform the failover, the call the SubmitMultiRegionAccessPointRoutes API operation in the aws API


S3 Server access log: can capture detailed request information, including user, file key, and timestamps, bucket, requester, operation
For import S3 server log to Aurora, we need requirement a custom process (lambda, GLUE) transfert them into a relational database for parser
Can use Athena for create a temporary table in a new bucket and query this log

- MD5 Checksum: A 128 bit hash to verify data integrity by comparing the source's file MD5 checksum with the object's upload checksum
- S3 PutObject: Support intergrity checking via Content-MD5 header and return an ETAG in the response, which can be use for verification
- ETag: a response header that often represents the MD5 checksum of the uploaded object


Encrypt 
- SSE-S3: can read high rate > 10 000 per seconde
- KMS: throttle rate < 10 000 per seconde
