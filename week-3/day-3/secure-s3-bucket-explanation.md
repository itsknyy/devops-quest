# 1. Parameters

    BucketName:
    Type: String
    Description: Name of the S3 bucket
    AllowedPattern: "^[a-z0-9.-]{3,63}$"

- The user must provide a bucket name.
- The name must follow S3 naming rules.

---

# 2. Resources → SecureBucket (The S3 bucket)

    DeletionPolicy: Retain
    UpdateReplacePolicy: Retain

- `DeletionPolicy: Retain`: Keeps the bucket when the stack is deleted.
- `UpdateReplacePolicy: Retain`: Keeps the old bucket if CloudFormation needs to replace it during an update.

---

# 3. Versioning Enabled

    VersioningConfiguration:
    Status: Enabled

- Keeps previous versions of files
- prevents accidental deletion.

---

# 4. Ownership Controls

    OwnershipControls:
    Rules:
        - ObjectOwnership: BucketOwnerEnforced

- Bucket owner automatically owns all uploaded objects.
- Removes issues caused by other AWS accounts uploading data.

---

# 5. Block All Public Access

    PublicAccessBlockConfiguration:
    BlockPublicAcls: true
    IgnorePublicAcls: true
    BlockPublicPolicy: true
    RestrictPublicBuckets: true

- Blocks any attempt to set public ACLs on the bucket or its objects.
- Ignores all public ACLs, even if someone tries to apply them — they have no effect.
- Prevents adding bucket policies that would allow public access.
- Rejects all requests from outside the AWS account, unless they are authenticated — even if someone tries to make the bucket public.

---

# 6. Default Encryption

    BucketEncryption:
        ServerSideEncryptionConfiguration:
            - ServerSideEncryptionByDefault:
                SSEAlgorithm: AES256

- Every file is automatically encrypted with AES-256.

---

# 7. SecureBucketPolicy (Bucket Policy)

## Deny HTTP (non-secure) requests

    -   Sid: DenyInsecureTransport
    Effect: Deny
    Principal: "*"
    Action: "s3:*"
    Resource: 
        - bucket ARN
        - bucket ARN/*
    Condition:
        Bool:
        aws:SecureTransport: false

- Prevents anyone from sending unencrypted traffic.

---

## Deny uploads that are NOT encrypted

    - Sid: DenyUnEncryptedObjectUploads
    Effect: Deny
    Action: "s3:PutObject"
    Resource: bucket ARN/*
    Condition:
        StringNotEquals:
        s3:x-amz-server-side-encryption: AES256

- All uploaded objects must use encryption AES256.

---

**NOTE:**
`!Sub` is used instead of `!Ref` because it can build full strings.
For example:
- !Ref BucketName --> returns only the bucket name
- !Sub arn:aws:s3:::${BucketName} --> builds the full ARN using that name

---






