---
title : "AWS CLI"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 2.2 </b> "
---

AWS CLI allow you interact with AWS cloud from local terminal. Please access [https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html) to download and install the cli compatible with your OS.

To install the AWS CLI, run the following commands
```
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
```

Check aws command
```
$ aws --version
aws-cli/2.15.19 Python/3.11.6 Linux/5.10.205-195.807.amzn2.x86_64 botocore/2.4.5
```

After you successfully install the AWS CLI, you can safely delete your downloaded installer files. 
```
$ rm awscliv2.zip 
```

Configure IAM access keys to the default profile. This command provide you to configure credential to identify your IAM user and interact with specific Region.
```
$ aws configure
AWS Access Key ID [****************BBGU]:
AWS Secret Access Key [****************AX2v]:
Default region name [us-east-1]:
Default output format [None]:
```

Check successful configuration by request the account ID:
```
$ aws sts get-caller-identity
{
    "UserId": "AIDA5WUNFGD4AFX7FFU2W",
    "Account": "YOUR_ACCOUNT_ID",
    "Arn": "arn:aws:iam::YOUR_ACCOUNT_ID:user/YOU_IAM_USER"
}
```

{{%notice tip%}}
You can manage multiple IAM credentials with `--profile` parameter to interact with multiple users or AWS accounts.\
For more information, refer to this [document](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.html)
{{%/notice%}}