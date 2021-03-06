go-aws-auth
===========

Go-AWS-Auth is a comprehensive, lightweight library for signing requests to Amazon Web Services.

It's easy to use: simply build your HTTP request, then call `awsauth.Sign(req)` before sending your request over the wire.

**[Full GoDoc Documentation](http://godoc.org/github.com/smartystreets/go-aws-auth)**


### Supported signing mechanisms

- [Signed Signature Version 2](http://docs.aws.amazon.com/general/latest/gr/signature-version-2.html)
- [Signed Signature Version 3](http://docs.aws.amazon.com/general/latest/gr/signing_aws_api_requests.html)
- [Signed Signature Version 4](http://docs.aws.amazon.com/general/latest/gr/signature-version-4.html)
- [Custom S3 Authentication Scheme](http://docs.aws.amazon.com/AmazonS3/latest/dev/RESTAuthentication.html)
- [Security Token Service](http://docs.aws.amazon.com/STS/latest/APIReference/Welcome.html)
- [S3 Query String Authentication](http://docs.aws.amazon.com/AmazonS3/latest/dev/S3_QSAuth.html)
- Coming soon: IAM Role Authentication

For more info about AWS authentication, see the [comprehensive docs](http://docs.aws.amazon.com/general/latest/gr/signing_aws_api_requests.html) at AWS.


### Install

Go get it:

	$ go get github.com/smartystreets/go-aws-auth
	
Then import it:

	import "github.com/smartystreets/awsauth"


### Using your AWS Credentials

You can do it two ways.

1. **Recommended:** Set the `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY` environment variables with your credentials. The library will automatically detect and use them. Optionally, you may also set the `AWS_SECURITY_TOKEN` environment variable if you are using temporary credentials from [STS](http://docs.aws.amazon.com/STS/latest/APIReference/Welcome.html).

2. You can set `awsauth.Keys` with hard-coded credentials (for testing or spike code):
```go
awsauth.Keys = &awsauth.Credentials{
	AccessKeyID: "Access Key ID", 
	SecretAccessKey: "Secret Access Key",
	SecurityToken: "Security Token",	// STS (optional)
}
```

Setting the credentials manually will override environment variables.



### Signing requests

Once your credentials are set up, just make the request, have it signed, and perform the request as usual.

```go
url := "https://iam.amazonaws.com/?Action=ListRoles&Version=2010-05-08"
client := &http.Client{}

req, err := http.NewRequest("GET", url, nil)

awsauth.Sign(req)	// Automatically chooses the best signing mechanism for the service

resp, err := client.Do(req)
```


### Contributing

This library isn't quite complete (yet) but should work well for most common AWS interactions. Please feel free to contribute by forking, opening issues, submitting pull requests, etc. Thanks to all contributors!
