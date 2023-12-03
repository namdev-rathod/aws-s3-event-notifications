# Simple Process To Configure S3 Bucket Event Notifications For File Size Less Than 1 MB
# SNS Topic Access Policy 

```
{
  "Version": "2012-10-17",
  "Id": "__example_policy_ID",
  "Statement": [
    {
      "Sid": "example-statement-ID",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "SNS:Publish",
      "Resource": "arn:aws:sns:ap-south-1:123456789:s3-bucket-file-size-check",
      "Condition": {
        "ArnEquals": {
          "aws:SourceArn": "arn:aws:s3:::s3-bucket-for-website-hosting"
        }
      }
    },
    {
      "Sid": "AWSEvents_CloudwatchEventsForS3_Id162620581855",
      "Effect": "Allow",
      "Principal": {
        "Service": "events.amazonaws.com"
      },
      "Action": "sns:Publish",
      "Resource": "arn:aws:sns:ap-south-1:123456789:s3-bucket-file-size-check"
    },
    {
      "Sid": "AWSEvents_s3-file-size-check_Idfd19a317-289b-4120-88bc-885ed06d132a",
      "Effect": "Allow",
      "Principal": {
        "Service": "events.amazonaws.com"
      },
      "Action": "sns:Publish",
      "Resource": "arn:aws:sns:ap-south-1:123456789:s3-bucket-file-size-check"
    }
  ]
}
```

# SNS Topic Subscription Message Filter Policy

```
{
  "source": [
    "aws.s3"
  ],
  "detail-type": [
    "Object Created"
  ],
  "detail": {
    "object": {
      "size": [
        {
          "numeric": [
            "<",
            1000000
          ]
        }
      ],
      "key": [
        {
          "prefix": "data/"
        }
      ]
    }
  }
}
```

# AWS CloudWatch Event Pattern Policy

```
{
  "source": ["aws.s3"],
  "detail-type": ["Object Created"],
  "detail": {
    "object": {
      "size": [{
        "numeric": ["<", 1000000]
      }],
      "key": [{
        "prefix": "data/"
      }]
    }
  }
}
```

