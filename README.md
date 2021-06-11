# ROSA Installation Steps

Red Hat OpenShift Service on AWS (ROSA) is a fully-managed and jointly supported Red Hat OpenShift offering that combines the power of Red Hat OpenShift, the industry’s most comprehensive enterprise Kubernetes platform, and the AWS public cloud.

Below are all the steps required to create ROSA Cluster.

1. Login to AWS with your IAM user

2. Search for ROSA and then click Enable ROSA on your account

3. Download AWS and ROSA cli

4. Move rosa to your directory in your path so that it can be referenced from anywhere

	```
$ sudo mv rosa /usr/local/bin/
```

5. Configure your AWS CLI with your credentials

	```
$ aws configure
```
	
6. Enter Information that you saved when you created your IAM user

	```
	AWS Access Key ID [None]: *********
	AWS Secret Access Key [None]: *********
	Default region name [None]: us-east-2
	Default output format [None]: json
	```
	
7. Check if the account got set up properly

	```
$ aws sts get-caller-identity
```
8. Set up ROSA Login

	```
$ rosa login
```		
	- To login to your Red Hat account, get an offline access token at  <https://cloud.redhat.com/openshift/token/rosa>		
	- Go to that link and copy paste the token onto the cli
		
9. Check Permissions	

	```
$ rosa verify permissions
```

10. Verify that your AWS account can support the installation

	```
$ rosa verify quota</code>
```
	- You will get Insufficient ec2 quota error. ROSA installation needs **100** vCPUs and it will not allow you to go any further unless that is available to the account.
	
	- You can run this command to get more details

		```
	$ aws service-quotas get-service-quota \
	--service-code ec2 \	
	--quota-code L-1216C47A
	```

	- To increase the quote run the command below. This will open a ticket in AWS and it will take some back and forth to get it approved to 100

		```	
	$ aws service-quotas request-service-quota-increase \
	--service-code ec2 \	
	--quota-code L-1216C47A \	
	--desired-value 100
   ```
   	
11. Initialize ROSA

	```
$ rosa init
```
	- It will give you a warning that oc client is needed

		```
		$ rosa download oc
		$ sudo mv oc /usr/local/bin
		$ sudo mv openshift-client-mac/oc /usr/local/bin
		$ sudo mv openshift-client-mac/kubectl /usr/local/bin
		```
12. Initialize ROSA again

	```
$ $ rosa init
```

13. Create Cluster

	```
$ rosa create cluster --interactive
```
	- To watch the logs, run

		```
		$ rosa logs install -c democluster --watch
		```
		
14. Once the cluster is created, it will give you the link to ROSA console but you will see that it will have the Cluster_SRE User for login. To create cluster admin

	```
$ rosa create admin --cluster=democluster
```
It will auto-generate the password. Use that to login to the OpenShift cluster

15. For deleting the cluster

	```
$ rosa delete cluster --cluster=democluster
$ rosa logs uninstall -c democluster --watch
```

16. Watch logs and wait till cluster is deleted, then run the command to clean up the cloud formation stack created during rosa init

	```
$ rosa init --delete-stack
```