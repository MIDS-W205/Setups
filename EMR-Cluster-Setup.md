# How to create a cluster using <a href="http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-welcome.html">AWS CLI</a>

First you need to create a bucket for your inputs,outputs,codes, and errorlogs folders:

----------

    aws s3 mb s3://MyClusterTest/
    
Then we need to copy the input files as well as the source codes to their folders: (Assuming you have all the files in the current folder)

    aws s3 cp input.txt s3://MyClusterTest/input/
    
    aws s3 cp maper.py s3://MyClusterTest/codes/
    
    aws s3 cp reducer.py s3://MyClusterTest/codes/
    
You can also use sync command to sync the content of a local folder with that of a S3 folder

      aws s3 sync input s3://MyClusterTest/input
    

Once all the input data and maper/reducer codes are uploaded, the next step is to create the cluster:

----------

    aws emr create-cluster --ami-version 3.5.0 --instance-groups InstanceGroupType=MASTER,InstanceCount=1,InstanceType=m1.medium InstanceGroupType=CORE,InstanceCount=2,InstanceType=m1.medium --name "MyCluster" --log-uri s3://MyClusterTest/errorlogs/ --enable-debugging --bootstrap-actions Path=s3://MyClusterTest/install-lib.sh,,Args=[numpy,pandas]

Once you run the `create-cluster` command, it returns a cluster id that you can use to terminate the cluster:


`aws emr terminate-clusters --cluster-id <cluster-id>`

If you are not sure about the id you can get the list of active clusters by:

`aws emr list-clusters --active`

You can find various available options in [EMR Command Line Interface Options](http://docs.aws.amazon.com/ElasticMapReduce/latest/DeveloperGuide/emr-cli-commands.html)




