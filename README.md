# README

`aws-get-tag` is a bash script that retrieves a tag from an EC2
instance.  By default it will retrieve the specified tag from the
currently running instance on which the script is running.

# Why do I need this?

If you are launching an EC2 from an autoscaling group and are
propagating tags to the instance, the tags *may not* be available when
your user data script runs.  I have seen occassions where a race
condition exists such that the tags have not yet been propagated
causing my user data script to fail when looking for required tags.

# How does this solve the issue?

AWS' recommendation is to use exponential backoff...so if you are
using a bash script as I have been doing to provision my EC2 on
launch, then you might want to retry the operation.  Hence this
script.

# Usage

```
usage: /usr/bin/aws-get-tag OPTIONS tag-name

Example: aws-get-tag Environment

 ENVIRONMENT=$(aws-get-tag -e Environment)

 eval $(aws-get-tag -e Environment)

Options
-------
-r    region
-i    instance id, default: currently running instance
-e    shell mode (outputs 'export TAG-NAME=value')
-n    retries, default: 1
-t    timeout (sleep in seconds between retries), default: 2
```

# Example

```
aws-get-tag -n 5 -t 2 Environment
```
