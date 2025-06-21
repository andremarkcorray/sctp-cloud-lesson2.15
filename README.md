# sctp-cloud-lesson2.15

- What is needed to authorize your EC2 to retrieve secrets from the AWS Secret Manager?
> Attach an IAM role to your EC2 instance, which must have a policy granting permission to retrieve secrets using secretsmanager:GetSecretValue. The policy should only grant access to the specific secret.

- Derive the IAM policy (i.e. JSON)?
    - Using the secret name prod/cart-service/credentials, derive a sensible ARN as the specific resource for access
The policy should allow read-only access (GetSecretValue) to a specific secret named prod/cart-service/credentials.
The secret's ARN should compose of:
Region -> ap-southeast-1
Account ID -> eg. 123456789012
Secret Name -> prod/cart-service/credentials
Unique 6-character suffix -> eg abcdef

Therefore, the general format for secret ARN would be 'arn:aws:secretsmanager:<region>:<account-id>:secret:<secret-name>-<random-aws-assigned-suffix>'
In our case, it would be: 'arn:aws:secretsmanager:ap-southeast-1:123456789012:secret:prod/cart-service/credentials-abcdef'

Thus, the IAM policy would look like:
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "secretsmanager:GetSecretValue",
      "Resource": "arn:aws:secretsmanager:ap-southeast-1:123456789012:secret:prod/cart-service/credentials-abcdef"
    }
  ]
}
