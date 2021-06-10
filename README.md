# ROSA Installation Steps

Red Hat OpenShift Service on AWS (ROSA) is a fully-managed and jointly supported Red Hat OpenShift offering that combines the power of Red Hat OpenShift, the industry’s most comprehensive enterprise Kubernetes platform, and the AWS public cloud.

Below are all the steps required to create ROSA Cluster.

1. Login to AWS with your IAM user

2. Search for ROSA and then click Enable ROSA on your account

3. Download AWS and ROSA cli

4. Move rosa to your directory in your path so that it can be referenced from anywhere

 	<code>sudo mv rosa /usr/local/bin/</code>

5. Configure your AWS CLI with your credentials

	<code>aws configure</code>
	
6. Enter Information that you saved when you created your IAM user

	```
	AWS Access Key ID [None]: *********
	AWS Secret Access Key [None]: *********
	Default region name [None]: us-east-2
	Default output format [None]: json
	```
	
7. Check if the account got set up properly		
	<code>
	aws sts get-caller-identity
	</code>	
	
8. Set up ROSA Login		
<code>rosa login</code>		
	- To login to your Red Hat account, get an offline access token at  <https://cloud.redhat.com/openshift/token/rosa>		
	- Go to that link and copy paste the token onto the cli
		
9. Check Permissions  
<code>rosa verify permissions</code>

10. Verify that your AWS account can support the installation		
<code>rosa verify quota</code>
