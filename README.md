Introduction Of AWS -
Image for post
AWS is a cloud that is provided by Amazon. Amazon Elastic Compute Cloud (Amazon EC2 ) is a part of Amazon.com’s cloud-computing platform, Amazon Web Services, that allows users to rent virtual computers on which to run their own computer applications. AWS EC2 service provisions resources like RAM, HardDisk. CPU etc.
What is AWS-RDS?
Amazon Relational Database Service (Amazon RDS) makes it easy to set up, operate, and scale a relational database in the cloud. It provides cost-efficient and resizable capacity while automating time-consuming administration tasks such as hardware provisioning, database setup, patching, and backups. It frees you to focus on your applications so you can give them the fast performance, high availability, security, and compatibility they need.
Amazon RDS is available on several database instance types — optimized for memory, performance, or I/O — and provides you with six familiar database engines to choose from, including Amazon Aurora, PostgreSQL, MySQL, MariaDB, Oracle Database, and SQL Server. You can use the AWS Database Migration Service to easily migrate or replicate your existing databases to Amazon RDS.
Amazon RDS | Cloud Relational Database | Amazon Web Services
Set up, operate, and scale a relational database in the cloud with just a few clicks. Amazon Relational Database…
aws.amazon.com
What is WordPress?
WordPress is open-source software, which means anyone can study its code and write their apps (plugins) and templates (themes) for it.
Image for post
WordPress is the simplest, most popular way to create your own website or blog. In fact, WordPress powers over 39.5% of all the websites on the Internet. Yes — more than one in four websites that you visit are likely powered by WordPress.
On a slightly more technical level, WordPress is an open-source content management system licensed under GPLv2, which means that anyone can use or modify the WordPress software for free. A content management system is basically a tool that makes it easy to manage important aspects of your website — like content — without needing to know anything about programming.
The end result is that WordPress makes building a website accessible to anyone — even people who aren’t developers.
🔰Task Description📄
🎯 Create an AWS EC2 instance
🎯 Configure the instance with Apache Webserver.
🎯 Download PHP application name “WordPress”.
🎯 As WordPress stores data at the backend in the MySQL Database server. Therefore, you need to set up a MySQL server using AWS RDS service using Free Tier.
🎯 Provide the endpoint/connection string to the WordPress application to make it work.
👀 Prerequisites:
For this, there are some Prerequisite
1. An account on AWS so that we can use its resources.
For Best practice Create an IAM User. Here I am going to create an IAM with Admin Power. The Steps are following as :
Launching WordPress with Mysql using private and public subnet in AWS Cloud By Terraform
A virtual private cloud (VPC) is an on-demand configurable pool of shared computing resources allocated within a public…
www.linkedin.com
Let’s Begin the task…
Step1: Launch an EC2 instance For Deploying WordPress
Amazon Elastic Compute Cloud (Amazon EC2) is a web service that provides secure, resizable compute capacity in the cloud. It is designed to make web-scale cloud computing easier for developers. Amazon EC2’s simple web service interface allows you to obtain and configure capacity with minimal friction. It provides you with complete control of your computing resources and lets you run on Amazon’s proven computing environment.
To Launch Ec2 Instance Follow the below Screenshots Steps:
Image for post
Image for post
Image for post
Image for post
Image for post
Image for post
Image for post
Image for post
Image for post
Image for post
Image for post
To launch an ec2 instance AWS CLI has commanded as :
aws ec2 run-instances --image-id ami-0e306788ff2473ccb --security-group-ids sg-0f2663203cd6eea86 --instance-type t2.micro --subnet-id subnet-4b493b07 --key-name awscsakey
Output:
Image for post
Ec2 Instance Launched
Step2: Connect EC2 instance:
To connect from the Amazon EC2 console
Open the Amazon EC2 console.
In the left navigation pane, choose Instances and select the instance to which to connect.
Choose Connect.
On the Connect To Your Instance page, choose EC2 Instance Connect (browser-based SSH connection), Connect.
For More Info follow the below link:
New: Using Amazon EC2 Instance Connect for SSH access to your EC2 Instances | Amazon Web Services
This post is courtesy of Saloni Sonpal - Senior Product Manager - Amazon EC2 Today, AWS is introducing Amazon EC2…
aws.amazon.com
Image for post
Image for post
Step3: Install Webserver Php and other dependencies for Hosting WordPress
The following command will install all prerequisites and tools required to perform the WordPress installation:
# yum install php-mysqlnd php-fpm mariadb-server httpd tar curl php-json wget -y
And
# sudo amazon-linux-extras install php7.3
Image for post
dnf and yum is almost equal but have some extra features
Image for post
Step4: Download WordPress code from the Internet and Extract it
Download and extract WordPress. Start by downloading the WordPress installation package and extracting its content:
$ curl https://wordpress.org/latest.tar.gz --output wordpress.tar.gz
$ tar xf wordpress.tar.gz
Image for post
The extracted directory shows in blue color
Step5: Copy the WordPress code to the Document root of Webserver and start the services
Copy the extracted WordPress directory into the /var/www/html directory:
# cp -r wordpress /var/www/html
Image for post
To Start the Apache webserver services:

# systemctl start httpd
Enable httpd to start after system reboot:
# systemctl enable httpd
or
# systemctl enabled --now
We also disabled SELinux because of some reasons but it is not a good practice. To disable the SELinux we use the below command :
# setenforce 0
Image for post
Now we can see our WordPress site which is running on the AWS Cloud with the IP of our instance in my case IP is 13.234.217.14
Image for post
Step6: Create RDS database For WordPress
To create a MySQL DB instance
Sign in to the AWS Management Console and open the Amazon RDS console at https://console.aws.amazon.com/rds/.
In the upper-right corner of the AWS Management Console, choose the AWS Region where you want to create the DB instance. This example uses the US West (Oregon) Region.
In the navigation pane, choose Databases.
Choose Create database.
On the Create database page, shown following, make sure that the Standard Create option is chosen, and then choose MySQL.
Image for post
Image for post
6. In the Templates section, choose the Free tier.
7. In the Settings section, set these values:
DB instance identifier — bobby_db
Master username — bobby
Auto-generate a password — Disable the option
Master password — Choose a password.
Confirm password — Retype the password
Image for post
8. In the DB instance size section, set these values:
Burstable classes (includes t classes)
db.t2.micro
9. In the Storage and Availability & durability sections, use the default values.
Image for post
10. In the Connectivity section, open Additional connectivity configuration and set these values:
Virtual private cloud (VPC) — Choose an existing VPC with both public and private subnets, such as the bobby-vpc (vpc-identifier) created in Create a VPC with private and public subnets. or Use Default
Subnet group — The DB subnet group for the VPC, such as the aws-db-subnet-group created-in Create a DB subnet group. Here I am using the default
Public access — No
Existing VPC security groups — Choose an existing VPC security group that is configured for private access, such as the aws-db-securitygroup created-in Create a VPC security group for a private DB instance. Here I am using the default
Remove other security groups, such as the default security group, by choosing the X associated with each.
Database port — 3306
Image for post
11. Open the Additional configuration section, and enter bobby_DB for Initial database name. Keep the default settings for the other options.
Image for post
12. Choose Create database to create your RDS MySQL DB instance.
Your new DB instance appears in the Databases list with the status Creating.
Image for post
13.Wait for the Status of your new DB instance to show as Available. Then choose the DB instance name to show its details.
Image for post
14. In the Connectivity & security section, view the Endpoint and Port of the DB instance.
Image for post
Step6: Provide the endpoint/connection string to the WordPress application to make it work.
Note: The endpoint and port for your DB instance. You use this information to connect your webserver to your DB instance.
To make sure that your DB instance is as secure as possible, verify that sources outside of the VPC can’t connect to your DB instance
To Connect WordPress Application with DB Follow below Steps:
Image for post
Image for post
Image for post
Special Thanks to Vimal Daga sir and Preeti Chandak Ma’am for giving me the right path of education. Thanks a lot… 😀🙌🏻
Thanks for Reading !! 🙌🏻😁📃
🔰 Keep Learning !! Keep Sharing !! 
