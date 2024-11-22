# AWS Lambda Image Analysis

A Python implementation for analyzing images using AWS Lambda and Amazon Rekognition, supporting both local files and S3-stored images.

## Prerequisites

- Python 3.11.7
- AWS Account with appropriate permissions
- AWS Lambda function configured for image analysis
- S3 bucket for image storage (if using S3 method)
- boto3 library

## Installation

```bash
pip install boto3
```

## Required Libraries
```python
import boto3
import base64
import json
import pprint
import logging
from botocore.exceptions import ClientError
```

## Configuration

```python
aws_accesskey = "Your Access Key"
aws_secretaccess = "Your Secret Access Key"
myregion = "your-region"
function_name = "mylambda_function"
```

## Features

### 1. Local Image Analysis
Analyze images stored on your local machine:

```python
def analyze_images_local(aws_access, aws_secret, aws_region, image_path):
    """
    Analyze a local image using AWS Lambda.
    
    Args:
        aws_access: AWS access key
        aws_secret: AWS secret key
        aws_region: AWS region
        image_path: Path to local image file
        
    Returns:
        dict: Analysis results including detected labels and confidence scores
    """
```

### 2. S3 Image Analysis
Analyze images stored in S3:

```python
def analyze_image_s3(aws_access, aws_secret, aws_region, bucket_name, object_key):
    """
    Analyze an image stored in S3 using AWS Lambda.
    
    Args:
        aws_access: AWS access key
        aws_secret: AWS secret key
        aws_region: AWS region
        bucket_name: S3 bucket name
        object_key: S3 object key (path to image)
        
    Returns:
        dict: Analysis results including detected labels and confidence scores
    """
```

## Usage Examples

### 1. Analyzing Local Image
```python
# Initialize clients
S3_client = boto3.client('s3', 
    aws_access_key_id=aws_accesskey,
    aws_secret_access_key=aws_secretaccess, 
    region_name=myregion
)
lambda_client = boto3.client('lambda',
    aws_access_key_id=aws_accesskey,
    aws_secret_access_key=aws_secretaccess,
    region_name=myregion
)

# Analyze local image
image_path = "path/to/your/image.jpg"
analyze_images_local(aws_accesskey, aws_secretaccess, myregion, image_path)
```

### 2. Analyzing S3 Image
```python
bucket_name = "your-bucket-name"
object_key = "path/to/image.jpg"
analyze_image_s3(aws_accesskey, aws_secretaccess, myregion, bucket_name, object_key)
```

## IAM Permissions

Required S3 bucket policy:
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "S3Access",
            "Effect": "Allow",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::bucket/folder path/*"
        }
    ]
}
```

## Response Format

The analysis returns a JSON response containing:
- Detected labels
- Confidence scores
- Parent categories
- Instance information
- Response metadata

Example response:
```python
{
    "Labels": [
        {
            "Name": "Landscape",
            "Confidence": 99.99,
            "Instances": [],
            "Parents": ["Nature", "Outdoors"],
            "Categories": ["Nature and Outdoors"]
        }
    ],
    "LabelModelVersion": "3.0",
    "ResponseMetadata": {
        "HTTPStatusCode": 200
    }
}
```

## Error Handling

The implementation handles:
- Access denied errors
- S3 bucket access issues
- Lambda invocation failures
- File reading errors
- Invalid image formats

## Best Practices

1. Security:
   - Store credentials securely
   - Use appropriate IAM roles
   - Implement least privilege access

2. Performance:
   - Consider image size limits
   - Handle timeouts appropriately
   - Implement retries for failures

3. Error Handling:
   - Implement proper logging
   - Handle all potential exceptions
   - Provide meaningful error messages
