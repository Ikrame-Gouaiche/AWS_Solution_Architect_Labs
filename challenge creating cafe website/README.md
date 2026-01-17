# Challenge Lab: Creating a Dynamic Website for the Café

## Scenario
After the café launched the first version of its website, customers told the café staff how nice the website looks. However, in addition to providing praise, customers often asked whether they could place online orders.

Sofía, Nikhil, Frank, and Martha discussed the situation. They agreed that their business strategy and decisions should focus on delighting their customers and providing them with the best possible café experience.

## Lab Overview and Objectives
In this lab, you deploy an application on an Amazon Elastic Compute Cloud (Amazon EC2) instance. The application gives the café the ability to accept online orders. After testing that the application works as intended in the first AWS Region (the development environment), you then create an Amazon Machine Image (AMI) from the EC2 instance. You also deploy a second instance of the same application as the production environment in another AWS Region.

After completing this lab, you should be able to do the following:

- Connect to the Visual Studio Code Integrated Development Environment (VS Code IDE) on an existing EC2 instance.
- Configure the EC2 instance environment and confirm web server accessibility.
- Install a web application on an EC2 instance that also uses AWS Secrets Manager.
- Test the web application.
- Create an AMI.
- Deploy a second copy of the web application to another AWS Region.

When you start the lab, some resources are already created for you in the AWS account:

**Starting architecture** showing the AWS resources available to learners when the lab starts:

![Starting Architecture](images/Task_1%20Analyzing%20the%20existing%20EC2%20instance/instance_networking.png)

At the end of this lab, your architecture should look like the following example:

**Final architecture** showing AWS resources built after the lab challenge is complete:

![Final Architecture](images/Task_3%20Configuring%20the%20LAMP%20stack%20environment%20and%20confirming%20that%20the%20web%20server%20is%20accessible/verifying%20the%20new%20instance.png)

### Duration
This lab requires approximately 60 minutes to complete.

### AWS Service Restrictions
In this lab environment, access to AWS services and service actions might be restricted to the ones that are needed to complete the lab instructions. You might encounter errors if you attempt to access other services or perform actions beyond the ones that are described in this lab.

---

## Accessing the AWS Management Console

1. At the top of these instructions, choose **Start Lab**.  
2. The lab session starts.  
3. A timer displays at the top of the page and shows the time remaining in the session.  

**Tip:** To refresh the session length at any time, choose **Start Lab** again before the timer reaches 0:00.

Before you continue, wait until the circle icon to the right of the **AWS** link in the upper-left corner turns green. When the lab environment is ready, the **AWS Details** panel displays.

To connect to the AWS Management Console, choose the **AWS** link in the upper-left corner above the terminal window. A new browser tab opens and connects you to the console.

**Tip:** If a new browser tab does not open, a banner or icon is usually at the top of your browser with a message that your browser is preventing the site from opening pop-up windows. Choose the banner or icon, and then choose **Allow pop-ups**.

Arrange the AWS Management Console tab so that it displays alongside these instructions. Ideally, you have both browser tabs open at the same time so that you can follow the lab steps.

---

## A Business Request for the Café: Preparing an EC2 Instance to Host a Website (Challenge #1)

The café wants to introduce online ordering for customers and give café staff the ability to view submitted orders. Their current website architecture, where the website is hosted on Amazon S3, does not support the new business requirements.

In the first part of this lab, you take on the role of Sofía. You configure an EC2 instance so that it is ready to host a website for the café.

### Task 1: Analyzing the Existing EC2 Instance

1. On the AWS Management Console, in the search box, enter and choose **EC2** to open the Amazon EC2 console.  
2. Choose **Instances**.  
3. Notice the running instance named **Lab IDE**. This EC2 instance was created when you started the lab.

#### Instance Networking Configuration
![Instance Networking](images/Task_1%20Analyzing%20the%20existing%20EC2%20instance/instance_networking.png)

#### Security Group Configuration
![Instance Security](images/Task_1%20Analyzing%20the%20existing%20EC2%20instance/instance_security.png)

### Task 1.1: Answering Questions About the Instance
Your answers are evaluated when you choose the blue **Submit** button at the end of the lab.

1. At the top of these instructions, choose **i AWS Details**.  
2. Choose the **Access the multiple choice questions** link.  
3. On the page that you loaded, answer the first four questions about the Lab IDE EC2 instance:

- Question 1: Is the instance in a public subnet?  
- Question 2: Does the EC2 instance have an IPv4 Public IP address assigned to it?  
- Question 3: What inbound TCP port numbers are open for this instance?  
- Question 4: Does the EC2 instance have an AWS Identity and Access Management (IAM) role associated with it?  

![Answering Questions](images/Task_1%20Analyzing%20the%20existing%20EC2%20instance/Answering%20questions%20about%20the%20instance.png)

**Note:** Leave the questions webpage open in your browser tab. You return to it later in this lab.

---

### Task 2: Connecting to the IDE on the EC2 Instance

1. At the top of these instructions, choose **i AWS Details**.  
2. Copy values from the table for the following and paste it into an editor of your choice for use later:  
   - **LabIDEURL**  
   - **LabIDEPassword**  
3. In a new browser tab, paste the value for **LabIDEURL** to open the VS Code IDE.  
4. On the prompt window **Welcome to code-server**:  
   - Enter the value for **LabIDEPassword** you copied to the editor earlier.  
   - Choose **Submit** to open the VS Code IDE.

**Note:** The IDE includes:

- A bash terminal in the bottom-right panel.  
- A file browser in the left panel that shows files in the `/home/ec2-user/environment` directory on the instance.  
- A file editor in the upper-right panel. Selecting a file in the file browser, such as `README.md`, displays it in the editor.

#### VS Code IDE Interface
![VS Code IDE](images/Task_2%20Connecting%20to%20the%20IDE%20on%20the%20EC2%20instance/VsCode.png)

---

### Task 3: Configuring the LAMP Stack Environment and Confirming Web Server Accessibility

1. Observe the operating system version in the VS Code IDE bash terminal:

```bash
cat /proc/version
```

![Operating System Version](images/Task_3%20Configuring%20the%20LAMP%20stack%20environment%20and%20confirming%20that%20the%20web%20server%20is%20accessible/operating%20system%20version.png)

2. Observe the web server, PHP details, and server state:

```bash
sudo sed -i 's/Listen 80/Listen 8000/g' /etc/httpd/conf/httpd.conf
sudo systemctl start httpd
sudo systemctl enable httpd
sudo service httpd status
php --version
```

![Web Server Status](images/Task_3%20Configuring%20the%20LAMP%20stack%20environment%20and%20confirming%20that%20the%20web%20server%20is%20accessible/web%20server.png)

3. Install the database and set it to start automatically:

```bash
sudo dnf install -y mariadb105-server
sudo systemctl start mariadb
sudo systemctl enable mariadb
sudo mariadb --version
sudo service mariadb status
```

![Installing Database](images/Task_3%20Configuring%20the%20LAMP%20stack%20environment%20and%20confirming%20that%20the%20web%20server%20is%20accessible/installing.png)

![Database Connected](images/Task_3%20Configuring%20the%20LAMP%20stack%20environment%20and%20confirming%20that%20the%20web%20server%20is%20accessible/database.png)

4. Configure the EC2 instance so you can use the VS Code IDE to edit web server files:

```bash
ln -s /var/www/ /home/ec2-user/environment
sudo chown ec2-user:ec2-user /var/www/html
```

5. Create a test webpage:

* In the file browser, expand `CafeWebServer > www > html`.
* Choose **Menu > File > New File**.
* Paste:

```html
<html>Hello from the café web server!</html>
```

* Save as `index.html`.

6. Make the website accessible from the internet:

* In the Amazon EC2 console, locate the public IPv4 address.
* Open `http://<public-ip>:8000` in a browser.

**Tip:** To allow inbound HTTP traffic on TCP port 8000, update the security group as needed.

![Add Security Rule](images/Task_3%20Configuring%20the%20LAMP%20stack%20environment%20and%20confirming%20that%20the%20web%20server%20is%20accessible/add%20rule.png)

![Index Page Accessible](images/Task_3%20Configuring%20the%20LAMP%20stack%20environment%20and%20confirming%20that%20the%20web%20server%20is%20accessible/index%20accessible.png)

---

## Installing a Dynamic Website Application on the EC2 Instance (Challenge #2)

### Task 4: Installing the Café Application

1. Download and extract the application files:

```bash
cd ~/environment
wget https://aws-tc-largeobjects.s3.us-west-2.amazonaws.com/CUR-TF-200-ACACAD-3-113230/03-lab-mod5-challenge-EC2/s3/setup.zip
unzip setup.zip
wget https://aws-tc-largeobjects.s3.us-west-2.amazonaws.com/CUR-TF-200-ACACAD-3-113230/03-lab-mod5-challenge-EC2/s3/db.zip
unzip db.zip
wget https://aws-tc-largeobjects.s3.us-west-2.amazonaws.com/CUR-TF-200-ACACAD-3-113230/03-lab-mod5-challenge-EC2/s3/cafe.zip
unzip cafe.zip -d /var/www/html/
cd /var/www/html/cafe/
wget https://docs.aws.amazon.com/aws-sdk-php/v3/download/aws.zip
wget https://docs.aws.amazon.com/aws-sdk-php/v3/download/aws.phar
unzip aws -d /var/www/html/cafe/
chmod -R +r /var/www/html/cafe/
```

2. Configure application parameters:

```bash
cd
cd environment/setup/
./set-app-parameters.sh
```

![Application Parameters in Secrets Manager](images/Task_3%20Configuring%20the%20LAMP%20stack%20environment%20and%20confirming%20that%20the%20web%20server%20is%20accessible/appParametres.png)

3. Configure the MySQL database:

```bash
cd ../db/
./set-root-password.sh
./create-db.sh
```

![Create Database](images/Task_3%20Configuring%20the%20LAMP%20stack%20environment%20and%20confirming%20that%20the%20web%20server%20is%20accessible/create%20db.png)

4. Connect to the MySQL client:

```bash
mysql -u admin -p
# Paste dbPassword from Secrets Manager
show databases;
use cafe_db;
show tables;
select * from product;
exit;
```

![Database Connected](images/Task_3%20Configuring%20the%20LAMP%20stack%20environment%20and%20confirming%20that%20the%20web%20server%20is%20accessible/db%20connected.png)

![Database Tables](images/Task_3%20Configuring%20the%20LAMP%20stack%20environment%20and%20confirming%20that%20the%20web%20server%20is%20accessible/db%20tables.png)

![Product Table](images/Task_3%20Configuring%20the%20LAMP%20stack%20environment%20and%20confirming%20that%20the%20web%20server%20is%20accessible/product%20table.png)

5. Update PHP timezone:

```bash
sudo sed -i "2i date.timezone = \"America/New_York\" " /etc/php.ini
sudo service httpd restart
```

![Update Timezone](images/Task_3%20Configuring%20the%20LAMP%20stack%20environment%20and%20confirming%20that%20the%20web%20server%20is%20accessible/update%20timezone.png)

6. Test the website: Open `http://<public-ip>:8000/cafe`.

**Issue Encountered:** Website not loading menu items

![Index Not Loaded](images/Task_3%20Configuring%20the%20LAMP%20stack%20environment%20and%20confirming%20that%20the%20web%20server%20is%20accessible/index%20not%20loaded.png)

![No Menu](images/Task_3%20Configuring%20the%20LAMP%20stack%20environment%20and%20confirming%20that%20the%20web%20server%20is%20accessible/no%20menu.png)

**Solution:** Attach CafeRole IAM profile to EC2 instance

![Update IAM Role](images/Task_3%20Configuring%20the%20LAMP%20stack%20environment%20and%20confirming%20that%20the%20web%20server%20is%20accessible/update%20iam%20role.png)

![Cafe Role Attached](images/Task_3%20Configuring%20the%20LAMP%20stack%20environment%20and%20confirming%20that%20the%20web%20server%20is%20accessible/cafe%20role.png)

### Task 5: Testing the Web Application

![Menu Available](images/Task_3%20Configuring%20the%20LAMP%20stack%20environment%20and%20confirming%20that%20the%20web%20server%20is%20accessible/menu%20available.png)

![Access Café Website](images/Task_3%20Configuring%20the%20LAMP%20stack%20environment%20and%20confirming%20that%20the%20web%20server%20is%20accessible/access%20cafe%20website.png)

7. Place an order to test functionality.

![Order Submitted](images/Task_3%20Configuring%20the%20LAMP%20stack%20environment%20and%20confirming%20that%20the%20web%20server%20is%20accessible/order%20submitted.png)

![Order History](images/Task_3%20Configuring%20the%20LAMP%20stack%20environment%20and%20confirming%20that%20the%20web%20server%20is%20accessible/order%20history.png)

---

## Creating Development and Production Websites in Different AWS Regions (Challenge #3)

### Task 6: Creating an AMI and Launching Another EC2 Instance

1. Set hostname and create a key pair:

```bash
sudo hostname cafeserver
ssh-keygen -t rsa -f ~/.ssh/id_rsa
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
```

![Key Pair Generation](images/Task_3%20Configuring%20the%20LAMP%20stack%20environment%20and%20confirming%20that%20the%20web%20server%20is%20accessible/key%20pair.png)

2. Create an AMI:

* In EC2 console: **Actions > Images and templates > Create image**
* Name: `CafeServer`
* Wait until **Available**

#### Answering Questions About AMIs

![QCM Questions](images/Task_3%20Configuring%20the%20LAMP%20stack%20environment%20and%20confirming%20that%20the%20web%20server%20is%20accessible/qcm.png)

![QCM Continuation](images/Task_3%20Configuring%20the%20LAMP%20stack%20environment%20and%20confirming%20that%20the%20web%20server%20is%20accessible/qcm%20(suite).png)

![QCM Justification](images/Task_3%20Configuring%20the%20LAMP%20stack%20environment%20and%20confirming%20that%20the%20web%20server%20is%20accessible/qcm%20justification.png)

3. Copy AMI to Oregon (us-west-2) Region and launch instance:

![Copy AMI](images/Task_3%20Configuring%20the%20LAMP%20stack%20environment%20and%20confirming%20that%20the%20web%20server%20is%20accessible/copy%20ami.png)

![Search AMI in Other Region](images/Task_3%20Configuring%20the%20LAMP%20stack%20environment%20and%20confirming%20that%20the%20web%20server%20is%20accessible/search%20ami%20id%20in%20other%20region.png)

![AMI Available in Other Region](images/Task_3%20Configuring%20the%20LAMP%20stack%20environment%20and%20confirming%20that%20the%20web%20server%20is%20accessible/ami%20available%20in%20other%20region.png)

* **Name:** `ProdCafeServer`
* **Instance type:** `t2.small`
* **Key pair:** Proceed without key pair
* **VPC:** `Lab VPC Region 2`
* **Subnet:** Public Subnet
* **Security Group:** `cafeSG` (ports 22 & 8000)
* **IAM Instance profile:** `CafeRole`

![EC2 Instance Creation in Region 2](images/Task_3%20Configuring%20the%20LAMP%20stack%20environment%20and%20confirming%20that%20the%20web%20server%20is%20accessible/ec2%20region%202%20creation.png)

![Launching Instance](images/Task_3%20Configuring%20the%20LAMP%20stack%20environment%20and%20confirming%20that%20the%20web%20server%20is%20accessible/lunching%20instance.png)

4. Update Secrets Manager for the new Region:

* Edit `~/environment/setup/set-app-parameters.sh`:

```bash
region="us-west-2"
publicDNS="<public-dns-of-ProdCafeServer-instance>"
```

![Edited App Parameters](images/Task_3%20Configuring%20the%20LAMP%20stack%20environment%20and%20confirming%20that%20the%20web%20server%20is%20accessible/edited%20app%20parameters.png)

* Run script:

```bash
cd ~/environment/setup/
./set-app-parameters.sh
```

### Task 7: Verifying the New Café Instance

* Confirm `ProdCafeServer` is running in Oregon Region.
* Open `http://<public-ip>:8000/cafe` and test placing orders.

![Verifying the New Instance](images/Task_3%20Configuring%20the%20LAMP%20stack%20environment%20and%20confirming%20that%20the%20web%20server%20is%20accessible/verifying%20the%20new%20instance.png)

**Optional SSH connection:**

```bash
ssh -i ~/.ssh/id_rsa ec2-user@<public-ip-of-ProdCafeServer>
```

---

## Update from the Café

* Development instance: us-east-1
* Production instance: us-west-2
* Developers can test enhancements on development before migrating to production.

---

## Conclusion

You have successfully:

* Connected to VS Code IDE on an EC2 instance
* Analyzed EC2 instance environment and confirmed web server accessibility
* Installed a web application using Secrets Manager
* Tested the web application
* Created an AMI
* Deployed a second copy of the web application in another AWS Region

