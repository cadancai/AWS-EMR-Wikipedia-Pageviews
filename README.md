Running A AWS EMR Job
========================================================

Setting Up
---------------
* Create a t1.micro ubuntu instance on AWS. If it's your first time creating an instance, you need to create a .pem key after launch the instance, save it somewhere in your local drive. Change its mode using this: sudo chmod 400 yourpemfile.pem

![IMAGE](figures/AWS Console.png)
![IMAGE](figures/Choose AMI.png)
![IMAGE](figures/Choose T1.micro Free tier.png)
![IMAGE](figures/Launch the instance.png)

* Create an Access Key ID and Secret via AWS Console

![IMAGE](figures/AWS Console 2.png)
![IMAGE](figures/S3 Console and Security credentials.png)
![IMAGE](figures/Continue to Security Credentials.png)
![IMAGE](figures/Create Security Credentials.png)
![IMAGE](figures/Access Key.png)

* ssh into the created instance from you local machine, then follow the below steps on command line
```
~ Yes Master? $ ssh -i ~/Downloads/yourpemfile.pem ubuntu@54.86.110.206
```

* Download the AWS command line tool
```
ubuntu@ip-172-31-19-209:~$ sudo apt-get install awscli
```

* Configure AWS with the Access Key ID and Secret. Once you hit enter with the command below, follow the instruction with the prompts. 
```
ubuntu@ip-172-31-19-209:~$ aws configure
```

* Create a volume with the wikipedia pagecount data.
```
ubuntu@ip-172-31-19-209:~$ aws ec2 create-volume --region us-east-1 --availability-zone us-east-1d --snapshot-id snap-f57dec9a
```

* Attach the volume to the AWS instance
```
ubuntu@ip-172-31-19-209:~$ aws ec2 attach-volume --region us-east-1 --volume-id vol-cd008684 --instance-id i-b3587ce3 --device /dev/sdf
```

* Create a folder and mount the volume on the AWS instance. This may take quite some time (up to 20 or 30 mins).
```
ubuntu@ip-172-31-19-209:~$ mkdir /mnt/wikidata
ubuntu@ip-172-31-19-209:~$ sudo mount /dev/xvdf1 /mnt/wikidata
```

* Create a S3 bucket via the AWS console. For example, the bucket I created here is called my-bucket-lee. 

![IMAGE](figures/S3.png)
![IMAGE](figures/Create Bucket.png)
![IMAGE](figures/Create my-bucket-lee.png)
![IMAGE](figures/Create folders inside bucket.png)


* Copy all the wikipedia data to the s3 bucket. This will take up to a few hours.
```
ubuntu@ip-172-31-19-209:~$ sudo aws s3 cp /mnt/wikidata/wikistats/ s3://my-bucket-lee/input/ --recursive --region us-east-1
```

* Create mapper and reducer using python. Upload them into the S3 bucket.

![IMAGE](figures/Upload Mapper and Reducer.png)
![IMAGE](figures/Upload Mapper and Reducer 1.png)
![IMAGE](figures/Upload Mapper and Reducer 2.png)

* Create the cluster. Make sure the S3 bucket that you created is consistent. The examples below do not use my-bucket-lee that I created earlier. Replace all my-bucket with my-bucket-lee on all entries in all images below.

![IMAGE](figures/AWS Console 3.png)
![IMAGE](figures/Elastic MapReduce.png)
![IMAGE](figures/Create Cluster 1.png)
![IMAGE](figures/Create Cluster 2.png)
![IMAGE](figures/Create Cluster 3.png)
![IMAGE](figures/Create Cluster 4.png)
![IMAGE](figures/Running Cluster.png)





