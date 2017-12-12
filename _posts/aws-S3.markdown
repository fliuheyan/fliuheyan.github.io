## AWS S3
aws所有资源默认是私有的

lifecycle
website
versioning
Policy & ACL
logging(access log)

### permission
1. resource based policies
ACL
policy
2. user policies
IAM

### static web hosting
部署，配置好S3和Route53之后将所有静态资源sync到bucket就OK
S3 + Route53
1. bucket
`bucket-name.s3-website-region.amazonaws.com`
`bucket-name.s3-website.region.amazonaws.com`
2. Use this bucket to host a website

request routing

### docker access control


### Glacier
