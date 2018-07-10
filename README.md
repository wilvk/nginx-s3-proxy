# nginx-s3-proxy

Test harness for proxying file requests to s3 via nginx

# Overview

TODO

# Prerequisites

You will need the following:

- A recent install of Docker
- A recent install of Docker Compose
- An AWS account with valid credentials you can use (see below).
- An s3 bucket that can accept GET requests on bucket objects using your credentials.

# For local testing:

1. You will need to get your AWS security credentials and set them as environment variables. 

```bash
export AWS_ACCESS_KEY_ID=...
export AWS_SECRET_ACCESS_KEY=...
export AWS_SECURITY_TOKEN=...
```

2. Setting `AWS_DEBUG` to true indicates that the local proxy will be used to capture header information that you can inspect.

```bash
export AWS_DEBUG=True
```

3. Running the following spins up a Docker environment for requesting files through the local proxy. (Note: The proxy does not forward requests to s3).

```bash
bin/dev-environment-up
```

4. To test, in another terminal:

```bash
curl localhost:8000/s3/<s3_bucket_name>/<s3_object_id>
```

# For production use:

This will ideally be done in an aws instance so that creds can be sourced easily using the AWS metadata endpoint. 

Alternatively, you can use the aws cli sts (Security Token Service) to obtain credentials.

1. To specify as a prod environment, unset the debug variable:

```bash
unset AWS_DEBUG
```

2. If in an aws environment, you can source your credentials using the following script. The two arguments `<aws_region>` and `<s3_bucket_name>` are required to run the script.

```bash
source bin/s3-env <aws_region> <s3_bucket_name>
```

3. Spin up the environment.

```bash
bin/dev-environment-up
```

# Notes

- All s3 objects are referenced under the `/s3/` path of the web server. This allows forwarding to other downstream websites on other paths.
- The current setup requires docker and docker-compose to be installed. The files under the `openresty` can be implemented in a non-containerised environment with a little wrangling.

# Current Limitations

Currently all proxying for both local testing and forwarding creds to s3 use port 8000. I plan to make this an env var eventually. In the interim, the values can be changed in the relevant `nginx.conf` and `docker-compose-openresty.yml` files.

# Contributions

Are more than welcome. Submit a PR to get the ball rolling.

