# Sending an Email using AWS SES

When constructing backend systems, the common requirement arises to programmatically send transactional emails to customers or users within internal applications. This solution utilizes AWS Simple Email Service (SES) to dispatch transactional emails to designated email addresses.

### Step 1: Install aws-sdk in your environment

```bash
npm install aws-sdk
```

### Step 2: Create IAM role with below mentioned policy and attach it to the Lambda function or EC2 machine.

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

### Step 3: Use the below script to 

```javascript
const AWS = require('aws-sdk');
const fs = require('fs');

// Set your AWS credentials and region
AWS.config.update({
  region: 'your-region', // e.g., 'us-east-1'
});

// Create an S3 instance
const s3 = new AWS.S3();

// Set the parameters for S3 upload
const uploadParams = {
  Bucket: 'your-bucket-name',
  Key: 'folder/file.txt', // Specify the folder and file name in the S3 bucket
  Body: fs.createReadStream('local-file.txt'), // Local file to upload
};

// Perform the upload
s3.upload(uploadParams, (err, data) => {
  if (err) {
    console.error('Error uploading file:', err);
  } else {
    console.log('File uploaded successfully. S3 Location:', data.Location);
  }
});

```