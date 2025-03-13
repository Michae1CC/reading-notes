
Storage Gateway
---

Storage Gateway - File 
- Bridges on-prem file storage and S3
- Mount Points available via NFS or SMB
- Files stored into a mount point are visible as objects in a S3 bucket

IAM
---

AWS STS Service Bearer Tokens

- Some AWS services require that you have permission to get an AWS STS service bearer token before you can access their resources programmatically. These services support a protocol that requires you to use a bearer token instead of using a traditional Sig v4
- AWS STS service bearer tokens include information from your original principal authentication that might affect your permissions.
- If you perform an action in an AWS service that generates a bearer token for you, you must have the following permissions in your IAM policy

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AllowServiceBearerToken",
            "Effect": "Allow",
            "Action": "sts:GetServiceBearerToken",
            "Resource": "*"
        }
    ]
}
```


Resource-based policies for IAM users

- Within the same account, resource-based policies that grant permissions to an IAM user ARN (that is not a federated user session) are not limited by an implicit deny in an identity-based policy or permissions boundary.

 https://docs.aws.amazon.com/images/IAM/latest/UserGuide/images/EffectivePermissions-rbp-boundary-id.png


Resource-based policies for IAM roles

- Resource-based policies that grant permissions to an IAM role ARN are limited by an implicit deny in a permissions boundary or session policy.


IAM Organizations SCPs

- SCPs are applied to an entire AWS account. They limit permissions for every request made by a principal within the account. An IAM entity (user or role) can make a request that is affected by an SCP, a permissions boundary, and an identity-based policy.

https://docs.aws.amazon.com/images/IAM/latest/UserGuide/images/EffectivePermissions-scp-boundary-id.png