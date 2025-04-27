## S3
Two way replication rules for your Multi-Region Access Point
- Replication rule enable automatic and asynchronous copying of object across buckets. A two way replication rule ensures that data is fully synchronized betwenn two or more bucket in different region.

- You can not delete a bucket if the bucket is not empty

S3 Multi-Region Access Point: provide a single global endpoint to access s3 buckets across multiple region, simplying application access and enabling failover. Configuring it in an active-passive setup allows the application to primarily use the bucket in region(active) while maintaining the ability to failover to the bucket in an another region.
- To perform the failover, the call the SubmitMultiRegionAccessPointRoutes API operation in the aws API