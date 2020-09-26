## CloudFront

### Edge Location
The location where content will be cached(separate to AWS region/AZ).

can upload file to edge location.

Files can cached for TTL

Edge location is not read-only. you can write to them(eg: S3 transfer acceleration)

### Origin

### Distribution
a collection of edge locations

* web distribution
* RTMP - Media Streaming

### Tips
you can clear cached object , but will be charged.


### Clean

create invalidation(object path /*) - not free , will be charged

### CloudFront signed URLs and Cookies

* Signed URLs
A signed URL is for individual file

* Cookies
1 cookie = multiple files

#### How to create signed URLs and Cookies
* Attack policy
    * URL expiration
    * IP Ranges
    * trusted signed(which AWS account can create signed URLs)
    
OAI (authentication)

### S3 signed URL
can access S3 bucket directly.
