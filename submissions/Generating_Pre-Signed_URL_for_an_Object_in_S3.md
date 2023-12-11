# Generating Pre-Signed URLs from S3 Bucket

When working with AWS S3, it's often necessary to generate pre-signed URLs to provide temporary access to specific objects. This capability is useful for scenarios such as granting time-limited access to download or upload files securely.

## Step 1: Install `aws-sdk` in your environment

```bash
npm install aws-sdk
```

## Step 2: Create IAM Role
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:GetObject",
        "s3:PutObject",
        "s3:ListBucket"
      ],
      "Resource": [
        "arn:aws:s3:::your-specific-bucket-name",
        "arn:aws:s3:::your-specific-bucket-name/*"
      ]
    }
  ]
}
```
## Step 3: Use the Below Script to Generate a Pre-Signed URL
```javascript
const AWS = require('aws-sdk');

// Set your AWS credentials and region
AWS.config.update({
  region: 'your-region', // e.g., 'us-east-1'
});

// Create an S3 instance
const s3 = new AWS.S3();

// Set the parameters for generating a pre-signed URL
const params = {
  Bucket: 'your-bucket-name',
  Key: 'folder/file.txt', // Specify the folder and file name in the S3 bucket
  Expires: 60, // URL expiration time in seconds
};

// Generate the pre-signed URL
const preSignedUrl = s3.getSignedUrl('getObject', params);

console.log('Pre-Signed URL:', preSignedUrl);
```