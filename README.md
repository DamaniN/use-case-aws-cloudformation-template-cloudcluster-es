

Minimum options needed to deply template:

```none
aws cloudformation deploy --template-file rubrik_cloudcluster_es.template \
  --capabilities CAPABILITY_NAMED_IAM \
  --region <region> \
  --stack-name <stack_name> \
  --parameter-overrides \
    ClusterName=<cluster_name> \
    ImageId=<image-id> \
    AvailabilityZone=<availability_zone> \
    KeyPairName=<key_pair_name> \
    SubnetId=<subnet_id> \
    Vpc=<vpc_id>
```

Sample template deployment command:

```none
aws cloudformation deploy --template-file rubrik_cloudcluster_es.template \
  --capabilities CAPABILITY_NAMED_IAM \
  --region us-west-2 \
  --stack-name Rubrik-Cloud-Cluster-ES \
  --parameter-overrides \
    ClusterName=rubrik-cloud-cluster-es \
    ImageId=ami-0fb243e96a51ad197	 \
    AvailabilityZone=us-west-2a \
    KeyPairName=rubrik-cloud-cluster-es-key-pair \
    SubnetId=subnet-0123456789abcdef0	 \
    Vpc=vpc-0123456789abcdef0	
```


## Selecting a specific image

To select a specific image to deploy replace the `aws_image_id` variable with the AMI ID of the Rubrik Marketplace Image to deploy. To find a list of the Rubrik Cloud Cluster images that are available in a specific region run the following `aws` cli command (requires that the AWS CLI be installed):

```none
  aws ec2 describe-images \
    --filters 'Name=owner-id,Values=679593333241' 'Name=name,Values=rubrik-mp-cc-<X>*' \
     --query 'sort_by(Images, &CreationDate)[*].{"Create Date":CreationDate, "Image ID":ImageId, Version:Description}' \
    --region '<region>' \
    --output table
```

Where <X> is the major version of Rubrik CDM (ex. `rubrik-mp-cc-7*`)

Example: 

```none
aws ec2 describe-images \
     --filters 'Name=owner-id,Values=679593333241' 'Name=name,Values=rubrik-mp-cc-7*' \
     --query 'sort_by(Images, &CreationDate)[*].{"Create Date":CreationDate, "Image ID":ImageId, Version:Description}' \
      --region 'us-west-2' \
     --output table

------------------------------------------------------------------------------------------
|                                     DescribeImages                                     |
+--------------------------+-------------------------+-----------------------------------+
|        Create Date       |        Image ID         |              Version              |
+--------------------------+-------------------------+-----------------------------------+
|  2022-02-04T21:49:48.000Z|  ami-0056ddcc69df6fb5c  |  Rubrik OS rubrik-7-0-0-14764     |
|  2022-04-01T00:13:58.000Z|  ami-026233b876a279622  |  Rubrik OS rubrik-7-0-1-15183     |
|  2022-04-12T04:50:31.000Z|  ami-03d68b150241012ec  |  Rubrik OS rubrik-7-0-1-p1-15197  |
|  2022-04-27T05:56:27.000Z|  ami-09a3baba1545aa5f7  |  Rubrik OS rubrik-7-0-1-p2-15336  |
|  2022-05-13T21:51:54.000Z|  ami-0af1ff3ee7517fefa  |  Rubrik OS rubrik-7-0-1-p3-15425  |
|  2022-05-20T00:01:55.000Z|  ami-0cc1db55e45f3109b  |  Rubrik OS rubrik-7-0-1-p4-15453  |
|  2022-05-26T19:08:31.000Z|  ami-04d6af7c6f6629ce1  |  Rubrik OS rubrik-7-0-2-15510     |
+--------------------------+-------------------------+-----------------------------------+
```
For AWS Gov cloud change the `owner-id` to `345084742485`. 

Example:

```none
aws ec2 describe-images \
    --filters 'Name=owner-id,Values=345084742485' 'Name=name,Values=rubrik-mp-cc-7*' \
    --query 'sort_by(Images, &CreationDate)[*].{"Create Date":CreationDate, "Image ID":ImageId, Version:Description}' \
    --region 'us-gov-west-1' \
    --output table

------------------------------------------------------------------------------------------
|                                     DescribeImages                                     |
+--------------------------+-------------------------+-----------------------------------+
|        Create Date       |        Image ID         |              Version              |
+--------------------------+-------------------------+-----------------------------------+
|  2022-01-27T09:17:44.000Z|  ami-038cb33e356dfdb84  |  Rubrik OS rubrik-7-0-0-14706     |
|  2022-02-05T20:14:25.000Z|  ami-09c62e5a399fc5526  |  Rubrik OS rubrik-7-0-0-14764     |
|  2022-04-01T22:44:52.000Z|  ami-0852636d1bb4376a9  |  Rubrik OS rubrik-7-0-1-15183     |
|  2022-04-13T03:06:33.000Z|  ami-0e77ba2b8cdeb645c  |  Rubrik OS rubrik-7-0-1-p1-15197  |
|  2022-04-28T04:54:07.000Z|  ami-0486bfdcbf4ee6d5e  |  Rubrik OS rubrik-7-0-1-p2-15336  |
|  2022-05-14T19:53:12.000Z|  ami-0b519a90ae467950d  |  Rubrik OS rubrik-7-0-1-p3-15425  |
|  2022-05-20T23:18:12.000Z|  ami-060706f9a9462b5e7  |  Rubrik OS rubrik-7-0-1-p4-15453  |
+--------------------------+-------------------------+-----------------------------------+
```