To retrieve a secret from secretManager encrypted by KMS Customer managed key (CMK), the caller (IAM role, ..) must needs:
- IAM Permission to get value from secret manager (secretmanager:GetSecretValue)
- KMS Permission to decrypt the secret using CMK