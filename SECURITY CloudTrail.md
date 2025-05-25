## Storage
- Delivred log to s3 bucket for integration with other service can monitoring, analysis, archiving

## Log File Integrity Verification
- Trail provide a log file integrity verification feature that use SHA 256 hash to generate a digest file for each log file.
The digest file is signed with a private key and can be verified using cloudTrail ValidateLogs API or CLI
- This feature ensure with 100% certainty that log file have not been altered, as any modification would result a mismatch between the log file and its digest