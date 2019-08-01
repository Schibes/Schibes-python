# Schibes-challenge

This GitHub repo contains an AWS CloudFormation template that builds a secure, scalable, load balanced demo website in the AWS us-west-2 (Oregon) region. It also contains a very small "Hello World" static HTML page that can be replaced with the website code of your choice and an unrelated Python script (in a separate subfolder). The web servers run NGINX on Ubuntu Server 16.04.

The demo website serves HTTPS only (no HTTP) via a certificate generated inside Amazon Certificate Manager (ACM). Validations are done constantly throughout stack creation via automated tests run by CloudFormation as well as health checks implemented on the application load balancer.

# Requirements:

1) A valid AWS account with sufficient IAM permissions to create all required template resources
2) An active, valid DNS domain in Route 53 owned by your AWS account
3) *optional* An active, valid AWS EC2 key pair if you would like to be able to SSH into your EC2 instances after they are created.

# Instructions (AWS Web GUI):

1) Download the cloudformation-template.yml YAML file
2) Log into your AWS account and go to the us-west-2 (Oregon) region
3) Go to AWS CloudFormation and deploy the template using the following options: "Create Stack", "Template is Ready", and "Upload a Template File" 
4) Enter a name for the stack to be built with the template and populate the parameters as follows:
   * FQDN will be your full website name, like __foo.example.com__
   * WebServerKeyName will be your AWS EC2 key pair used to SSH into the web servers (this is optional, if not needed then comment it out in YAML) 
   * myDomain is your AWS Route 53 domain name, like __example.com__
   * myHostedDomain is the same as myDomain BUT WITH A DOT AT THE END, like __example.com.__     (<---- notice trailing dot)
5) Because SSL certificate validation in AWS is still a manual process (absent third-party tools) you will need to go into AWS ACM and click to approve the certificate generated by this template. While doing this be sure to add the appropriate record into Route 53 at the same time or the template will not complete successfully. More detailed instructions can be found in the AWS documentation at https://docs.aws.amazon.com/acm/latest/userguide/gs-acm-validate-dns.html
   
# Instructions (AWS CLI)

Replace the values for "--stack-name" and "--parameter-overrides" in the snippet below with choices that will work in your own AWS environment. Also, if not using third-party automation tools you will still need to log into the AWS GUI and approve the SSL certificate and Route 53 entry as shown in step 5 of the instructions above.

`aws cloudformation deploy --template-file cloudformation-template.yml --stack-name cloudformation-demo --parameter-overrides WebServerKeyName=myEC2keypair myDomain=example.com myHostedDomain=example.com. FQDN=foo.example.com`

