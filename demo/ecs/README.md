# REST-o-rant on AWS EC2 Container Service (ECS) with Fargate

This demos show you how to deploy to ECS with XL Deploy.

**N.B.:**<br/>
As of July 25th 2018, this demo depends on a SNAPSHOT build of the AWS plugin, which is still under development. More manual steps are required for now. Those will be replaced once the plugin has been released as part of Deploy 8.2.

## Prerequisites
* AWS Command Line Tools
* Python 3.6 or up

## Step 0. Use SNAPSHOT version of AWS plugin for XL Deploy

**MANUAL STEP:**<br/>
Replace `xld-aws-plugin-8.1.0.xldp` with the latest `xld-aws-plugin-8.1.1-SNAPSHOT.xldp` from the `master` branch of the `xld-aws-plugin` repository

## Step 1. Configure AWS in XL Deploy

Make sure you have setup the AWS command line interface installed and configured correctly as per these instructions:
https://docs.aws.amazon.com/cli/latest/userguide/tutorial-ec2-ubuntu.html#configure-cli

This demo will not use the AWS command line interface itself, but will use the credentials and configuration in the `~/.aws/credentials` and `~/.aws/config` files:
https://docs.aws.amazon.com/cli/latest/userguide/cli-config-files.html

Once you've configured the AWS command line interface, use the `awsconfig2xld.py` script in the `config` directory to create XL YAML files that will create the AWS environment in XL Deploy.

```
$ ./config/awsconfig2xld.py > /tmp/AWSConfig.yaml
```

Now send this file to XL Deploy using

```
$ xl apply -f /tmp/AWSConfig.yaml
```

## Step 2. Import the REST-o-rant cluster definition:

Import the REST-o-rant ECS/Fargate cluster definition for AWS into XL Deploy:

```
$ xl apply -f demo/ecs/rest-o-rant-ecs-fargate-cluster.yaml
```

## Step 3. Provision the cluster in XL Deploy

1. Go to the XL Deploy UI running on http://localhost:4516.
2. Deploy the version `1.0` of the `rest-o-rant-ecs-fargate-cluster` application in the directory `Applications/AWS` to the `AWS` environment.

## Step 4. Step 2. Import the REST-o-rant application definition:

**MANUAL STEP:**</br>
Change the `subnets` and `securitygroups` in the `demo/ecs/rest-o-rant-ecs-service.yaml` to refer to the subnets and security groups created by the previous step.

Import the REST-o-rant application definition into XL Deploy:

```
$ xl apply -f demo/ecs/rest-o-rant-ecs-service.yaml
```

## Step 5. Deploy the app in XL Deploy:

1. Go to the XL Deploy UI running on http://localhost:4516.
2. Deploy the version `1.0` of the `rest-o-rant-ecs-service` application in the directory `Applications/AWS` to the `AWS` environment.

## Step 6. Access the rest-o-rant-app REST API

1. Go to the AWS console
2. Go to the ECS section of the AWS console
3. Go to the region to which you've deployed the ECS Fargate cluster
4. Click on the `ror-ecs-cluster`
5. Click on the `rest-o-rant-service`
6. Select the Tasks tab
7. Click on the first task
8. Copy the public IP
9. Open a new browser window and access:<br/>
http://PUBLICIP:8080/rest-o-rant-api/findrestaurants