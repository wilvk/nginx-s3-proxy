# nginx-s3-proxy
Test harness for proxying file requests to s3 via nginx

for local testing:

```
export AWS_DEBUG=True
```

for s3 testing:

```
unset AWS_DEBUG
source bin/s3-env
bin/dev-environment-up
```

in another window:

```
curl localhost:8000/s3/<s3_bucket_name>/<s3_object_id>
```
