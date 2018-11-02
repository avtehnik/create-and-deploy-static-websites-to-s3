Create bucket

```bash
aws s3api create-bucket --region eu-central-1 --create-bucket-configuration LocationConstraint=eu-central-1  --profile  your-profile  --bucket your-bucket-name
```

Enable static website hosting

```bash
aws s3 website s3://your-bucket-name/ --index-document index.html   --profile  your-profile
```

Upload files to s3

```bash
aws s3 sync src-folder s3://your-bucket-name --acl public-read  --profile  your-profile
```

Create cloudfront distribution

```bash
aws cloudfront create-distribution --distribution-config file://distconfig.json --profile  your-profile
```


distconfig.json

```json
{
    "Comment": "",
    "CacheBehaviors": {
      "Quantity": 0
    },
    "Logging": {
      "Bucket": "",
      "Prefix": "",
      "Enabled": false,
      "IncludeCookies": false
    },
    "Origins": {
      "Items": [
        {
          "OriginPath": "",
          "CustomOriginConfig": {
            "OriginProtocolPolicy": "http-only",
            "HTTPPort": 80,
            "HTTPSPort": 443
          },
          "Id": "s3-your-bucket-name.s3-website.eu-central-1.amazonaws.com",
          "DomainName": "your-bucket-name.s3-website.eu-central-1.amazonaws.com"
        }
      ],
      "Quantity": 1
    },
    "DefaultRootObject": "index.html",
    "PriceClass": "PriceClass_All",
    "Enabled": true,
    "DefaultCacheBehavior": {
      "TrustedSigners": {
        "Enabled": false,
        "Quantity": 0
      },
      "TargetOriginId": "s3-your-bucket-name.s3-website.eu-central-1.amazonaws.com",
      "ViewerProtocolPolicy": "allow-all",
      "ForwardedValues": {
        "Headers": {
          "Quantity": 0
        },
        "Cookies": {
          "Forward": "none"
        },
        "QueryString": false
      },
      "MaxTTL": 2629746,
      "SmoothStreaming": false,
      "DefaultTTL": 2629746,
      "MinTTL": 2629746,
      "AllowedMethods": {
        "Items": [
          "GET",
          "HEAD"
        ],
        "CachedMethods": {
          "Items": [
            "GET",
            "HEAD"
          ],
          "Quantity": 2
        },
        "Quantity": 2
      }
    },
    "CallerReference": "Mon May 22 21:39:53 CEST 2018",
    "CustomErrorResponses": {
      "Quantity": 0
    },
    "Restrictions": {
      "GeoRestriction": {
        "RestrictionType": "none",
        "Quantity": 0
      }
    },
    "Aliases": {
      "Items": [
        "alternative-domain.com"
      ],
      "Quantity": 1
    }
}
```

```bash
aws s3api put-bucket-policy --bucket your-bucket-name --policy file://policy.json
```

policy.json

```json
{
    "Version": "2008-10-17",
    "Statement": [
        {
            "Sid": "AllowPublicRead",
            "Effect": "Allow",
            "Principal": {
                "AWS": "*"
            },
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::your-bucket-name/*"
        }
    ]
}
```

