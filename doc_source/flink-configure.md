# Configuring Flink<a name="flink-configure"></a>

You may want to configure Flink using a configuration file\. For example, the main configuration file for Flink is called `flink-conf.yaml`\. This is configurable using the Amazon EMR configuration API so when you start your cluster, you supply a configuration for the file to modify\.

**To configure the number of task slots used for Flink using the AWS CLI**

1. Create a file, `configuration.json`, with the following content:

   ```
   [
       {
         "Classification": "flink-conf",
         "Properties": {
           "taskmanager.numberOfTaskSlots":"2"
         }
       }
   ]
   ```

1. Next, create a cluster with the following configuration:

   ```
   aws emr create-cluster --release-label emr-5.17.0 \
   --applications Name=Flink \
   --configurations file://./configurations.json \
   --region us-east-1 \
   --log-uri s3://myLogUri \
   --instance-type m4.large \
   --instance-count 2 \
   --service-role EMR_DefaultRole \ 
   --ec2-attributes KeyName=YourKeyName,InstanceProfile=EMR_EC2_DefaultRole
   ```

**Note**  
It is also possible to change some configurations using the Flink API\. For more information, see [https://ci.apache.org/projects/flink/flink-docs-master/dev/api_concepts.html](https://ci.apache.org/projects/flink/flink-docs-master/dev/api_concepts.html) in the Flink documentation\.

## Parallelism Options<a name="flink-parallelism"></a>

As the owner of your application, you know best what resources should be assigned to tasks within Flink\. For the purposes of the examples in this documentation, use the same number of tasks as the slave instances that you use for the application\. We generally recommend this for the initial level of parallelism but you can also increase the granularity of parallelism using task slots, which should generally not exceed the number of [virtual cores](https://aws.amazon.com/ec2/virtualcores/) per instance\. For more information about Flink’s architecture, see [https://ci.apache.org/projects/flink/flink-docs-master/concepts/index.html](https://ci.apache.org/projects/flink/flink-docs-master/concepts/index.html) in the Flink documentation\.

## Configurable Files<a name="flink-configurable-files"></a>

Currently, the files that are configurable within the Amazon EMR configuration API are:
+ `flink-conf.yaml`
+ `log4j.properties`
+ `log4j-yarn-session.properties`
+ `log4j-cli.properties`