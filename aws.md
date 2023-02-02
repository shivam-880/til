## Install and configure AWS CLI
```sh
$ curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
$ unzip awscliv2.zip
$ sudo ./aws/install
$ aws configure
```

## List files in a bucket
```sh
$ aws s3 ls s3://pometry-data/btc/
```

## Copy all files from a bucket (directory)
```sh
$ aws s3 sync s3://pometry-data/btc/partition1 .
```
