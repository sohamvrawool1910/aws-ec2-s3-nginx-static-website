# Static Website Hosting Using AWS EC2, S3, and Nginx

## Project Overview

This minor project demonstrates how to host a static website on an
Amazon EC2 instance using the Nginx web server and use an Amazon S3
bucket to store backup copies of the website files.

The website is accessed through the public IPv4 address of the EC2
instance.

## Project Objective

-   Launch an EC2 instance using Amazon Linux 2023.
-   Connect to the instance using SSH and a key pair.
-   Install and configure the Nginx web server.
-   Create a simple static website.
-   Access the website using the EC2 public IP address.
-   Create an S3 bucket and upload website backup files.

## AWS Services and Tools Used

-   **Amazon EC2:** Provides the virtual server that hosts the website.
-   **Amazon S3:** Stores backup copies of the website files.
-   **Nginx:** Web server used to serve the static website.
-   **Amazon Linux 2023:** Operating system used by the EC2 instance.
-   **SSH:** Used to connect to the EC2 instance remotely.

## Architecture

``` text
User
  |
  | HTTP request
  v
EC2 Instance (Amazon Linux 2023)
  |
  v
Nginx Web Server
  |
  v
Static Website (index.html)

S3 Bucket
  |
  +---- Backup copies of website files
```

The EC2 instance and Nginx serve the live website. Amazon S3 is used
separately to store backup files; it is not configured as the live
website origin in this project.

## Prerequisites

Before starting, make sure you have:

-   An AWS account with permission to use EC2 and S3.
-   An EC2 key pair for SSH access.
-   An SSH client (for example, Windows Terminal, PowerShell, or another
    SSH client).
-   A web browser.
-   A unique S3 bucket name.

## Implementation Steps

### 1. Launch an EC2 Instance

1.  Open the **AWS Management Console** and navigate to **EC2**.
2.  Choose **Launch instance**.
3.  Enter a suitable instance name.
4.  Select **Amazon Linux 2023** as the Amazon Machine Image (AMI).
5.  Select the **t2.micro** instance type, as specified in the project
    brief.
6.  Select or create an EC2 key pair and download the private key
    securely.
7.  Configure the security group with these inbound rules:

  Type   Protocol     Port Purpose
  ------ ---------- ------ -----------------------
  SSH    TCP            22 Remote administration
  HTTP   TCP            80 Access the website

For better security, restrict SSH (port 22) to your own IP address
instead of allowing access from anywhere.

8.  Review the settings and launch the instance.
9.  Wait until the instance is running and its status checks have
    passed.

### 2. Connect to the EC2 Instance

1.  Select the running instance in the EC2 console.
2.  Choose **Connect** and follow the SSH connection instructions.
3.  If using a local SSH client, ensure the private key file is
    accessible and has appropriate permissions.
4.  Connect using the instance's public IP address and the SSH username
    for the selected AMI.

Example command:

``` bash
ssh -i "your-key.pem" ec2-user@YOUR_EC2_PUBLIC_IP
```

Replace `your-key.pem` and `YOUR_EC2_PUBLIC_IP` with your own values. Do
not upload or share your private key.

### 3. Install and Start Nginx

Run the following commands after connecting to the EC2 instance:

``` bash
sudo dnf update -y
sudo dnf install nginx -y
sudo systemctl start nginx
sudo systemctl enable nginx
```

Check the Nginx service status:

``` bash
sudo systemctl status nginx
```

The service should show as active (running).

### 4. Create the Static Website

Create or edit the default Nginx website file:

``` bash
sudo nano /usr/share/nginx/html/index.html
```

Add sample HTML content:

``` html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My AWS Hosted Website</title>
</head>
<body>
    <h1>Welcome to My AWS Hosted Website</h1>
    <p>This static website is hosted on Amazon EC2 using Nginx.</p>
</body>
</html>
```

Save the file. If you use `nano`, press `Ctrl + O`, `Enter`, and then
`Ctrl + X`.

### 5. Test the Website

1.  Copy the **Public IPv4 address** from the EC2 instance details.
2.  Open a browser.
3.  Visit:

``` text
http://YOUR_EC2_PUBLIC_IP
```

The sample website should appear. Make sure the instance is running and
the security group allows inbound HTTP traffic on port 80.

### 6. Create an S3 Bucket

1.  Open **Amazon S3** in the AWS Management Console.
2.  Choose **Create bucket**.
3.  Enter a globally unique bucket name.
4.  Select the appropriate AWS Region.
5.  Keep **Block all public access** enabled unless your project
    specifically requires public access. The backup files do not need to
    be public for this project.
6.  Create the bucket.

### 7. Upload Website Backup Files to S3

1.  Open the bucket you created.
2.  Choose **Upload**.
3.  Add the website file, such as `index.html`, or a folder containing
    the website files.
4.  Choose **Upload**.
5.  Confirm that the uploaded files appear in the bucket.

This step stores a backup copy of the website files in S3. The live
website continues to be served by Nginx on EC2.

## Verification Checklist

-   [ ] EC2 instance launched with Amazon Linux 2023.
-   [ ] EC2 instance is running and status checks have passed.
-   [ ] Security group allows SSH (port 22) and HTTP (port 80).
-   [ ] SSH connection to the instance is successful.
-   [ ] Nginx is installed, started, and enabled.
-   [ ] Website file exists at `/usr/share/nginx/html/index.html`.
-   [ ] Website loads using the EC2 public IP address.
-   [ ] S3 bucket has been created.
-   [ ] Website backup file(s) have been uploaded to S3.

## Project Deliverables

Capture and include screenshots of:

1.  EC2 instance details showing the instance state and instance type.
2.  EC2 security group inbound rules showing ports 22 and 80.
3.  Successful connection to the EC2 instance.
4.  Nginx service status.
5.  Website displayed in a browser using the EC2 public IP address.
6.  S3 bucket details.
7.  Uploaded website backup file(s) inside the S3 bucket.

## Learning Outcomes

Through this project, I gained practical exposure to:

-   Launching and managing an Amazon EC2 instance.
-   Connecting to a Linux server using SSH and a key pair.
-   Installing and managing the Nginx web server.
-   Hosting and testing a static website on an EC2 public IP address.
-   Creating an Amazon S3 bucket and uploading backup files.
-   Understanding the basic roles of compute, web hosting, and cloud
    storage in AWS.

## Security Notes

-   Never commit private key files (`.pem`) or AWS access keys to a
    public repository.
-   Restrict SSH access to trusted IP addresses.
-   Keep S3 Block Public Access enabled for backup storage unless public
    access is explicitly required.
-   Do not include passwords, access keys, account IDs, or other
    sensitive AWS information in screenshots or documentation.

## Cleanup

To avoid ongoing AWS charges after completing the project:

1.  Back up any files you want to keep.
2.  Terminate the EC2 instance if it is no longer required.
3.  Delete the S3 bucket and its objects if they are no longer required.
4.  Review other resources created for the project, such as unused
    volumes or elastic IP addresses.

## Author

**Soham Rawool**

*Minor Project: Static Website Hosting Using EC2 and S3 (Nginx)*
