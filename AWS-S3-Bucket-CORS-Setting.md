# Cross-Origin Resource Sharing for AWS S3 Bucket

## Introduction
For modules that request for resources from AWS S3 Bucket to be displayed on the browser, modern browsers checks the response header for origin. If the origin is not listed in the header, the browser will reject the request and this can be seen as CORS error in the developer tool of your favourite browser.

### TLDR
To allow your browser to be able to display resources in your S3 Bucket, you need to enable the CORS settings in your AWS S3 Bucket.

In this example, we are using hub.example.com, hubd.example.com and central.example.com

{

        "AllowedHeaders": [
            "*"
        ],
        "AllowedMethods": [
            "GET"
        ],
        "AllowedOrigins": [
            "https://hub.example.com", # your staging env
            "https://hubd.example.com", # your development env
            "https://central.example.com" # your production env
        ],
        "ExposeHeaders": []
}

### I need step by step instruction
***
> 1. Login to your AWS Account at aws.amazon.com.


> 2. Go to your S3 Bucket.

![step 1](https://firebasestorage.googleapis.com/v0/b/edmondtm-1d8ed.appspot.com/o/s3-cors-step1.png?alt=media&token=bcd4ee2f-551e-4f55-9a25-8f83351424c1)


> 3. Select bucket you store

![step 2](https://firebasestorage.googleapis.com/v0/b/edmondtm-1d8ed.appspot.com/o/s3-cors-step2.png?alt=media&token=dee7d10f-f1af-4f9e-94b9-3af9e3a5ab2f)


> 4. Go To Permission

![step 3](https://firebasestorage.googleapis.com/v0/b/edmondtm-1d8ed.appspot.com/o/s3cors-step3.png?alt=media&token=307f9afe-1516-4a69-b4b1-07be2d99b845)


> 5. Edit CORS

![step 4](https://firebasestorage.googleapis.com/v0/b/edmondtm-1d8ed.appspot.com/o/s3cors-step4.png?alt=media&token=d8d52c5e-9c91-4129-ae9a-b457747c4f8f)


> 6. Insert CORS Configuration and Save

![step 5](https://firebasestorage.googleapis.com/v0/b/edmondtm-1d8ed.appspot.com/o/s3-cors-step5.png?alt=media&token=03eebecc-9479-43b3-8561-fc8a91812552)

[AWS Documentation](https://docs.aws.amazon.com/AmazonS3/latest/dev/cors.html)

