# Documentation: Creating a Dynamic Website for the Café

## Lab Summary

This documentation covers the complete implementation of a dynamic website for the café using AWS services. The lab demonstrates deploying a LAMP stack application on EC2, configuring AWS Secrets Manager, creating AMIs, and deploying across multiple AWS Regions.

---

## Task 1: Analyzing the Existing EC2 Instance

### Objectives:
- Access the Amazon EC2 console to analyze the pre-created **Lab IDE** instance
- Verify instance networking configuration (public subnet, public IP)
- Review security group rules and inbound ports
- Confirm IAM role association

### Steps:

1. Access the Amazon EC2 console and locate the **Lab IDE** instance

2. Review the instance networking configuration:
   ![Instance Networking](images/Task_1%20Analyzing%20the%20existing%20EC2%20instance/instance_networking.png)

3. Review the security group configuration and inbound ports:
   ![Instance Security](images/Task_1%20Analyzing%20the%20existing%20EC2%20instance/instance_security.png)

4. Answer questions about the instance configuration:
   ![Answering Questions](images/Task_1%20Analyzing%20the%20existing%20EC2%20instance/Answering%20questions%20about%20the%20instance.png)

---

## Task 2: Connecting to the IDE on the EC2 Instance

### Objectives:
- Retrieve **LabIDEURL** and **LabIDEPassword** from AWS Details
- Connect to VS Code IDE (code-server) running on the EC2 instance
- Familiarize with the IDE interface (terminal, file browser, editor)

### Steps:

1. Retrieve the **LabIDEURL** and **LabIDEPassword** from AWS Details

2. Connect to the VS Code IDE running on the EC2 instance:
   ![VS Code IDE](images/Task_2%20Connecting%20to%20the%20IDE%20on%20the%20EC2%20instance/VsCode.png)

3. Familiarize yourself with the IDE interface (terminal, file browser, editor)

---

## Task 3: Configuring the LAMP Stack Environment

### Objectives:
- Verify the operating system
- Configure the web server to listen on port 8000
- Install and configure the database
- Set up web server file structure
- Create test webpage
- Update security group

### Steps:

#### 1. Operating System Verification
Confirm Amazon Linux instance (Red Hat 7 based):
```bash
cat /proc/version
```
![OS Version](images/Task_3%20Configuring%20the%20LAMP%20stack%20environment%20and%20confirming%20that%20the%20web%20server%20is%20accessible/operating%20system%20version.png)

#### 2. Web Server Configuration
Modify Apache to listen on port 8000 (port 80 used by IDE), start and enable httpd service, and verify PHP installation:
```bash
sudo sed -i 's/Listen 80/Listen 8000/g' /etc/httpd/conf/httpd.conf
sudo systemctl start httpd
sudo systemctl enable httpd
sudo service httpd status
php --version
```
![Web Server](images/Task_3%20Configuring%20the%20LAMP%20stack%20environment%20and%20confirming%20that%20the%20web%20server%20is%20accessible/web%20server.png)

#### 3. Database Installation
Install MariaDB 10.5 server, start and enable mariadb service:
```bash
sudo dnf install -y mariadb105-server
sudo systemctl start mariadb
sudo systemctl enable mariadb
```
![Installing Database](images/Task_3%20Configuring%20the%20LAMP%20stack%20environment%20and%20confirming%20that%20the%20web%20server%20is%20accessible/installing.png)
![Database Connected](images/Task_3%20Configuring%20the%20LAMP%20stack%20environment%20and%20confirming%20that%20the%20web%20server%20is%20accessible/db%20connected.png)

#### 4. Web Server File Configuration
Create symlink for VS Code IDE access to web files and change ownership of html directory:
```bash
ln -s /var/www/ /home/ec2-user/environment
sudo chown ec2-user:ec2-user /var/www/html
```

#### 5. Test Webpage Creation
Create `index.html` with test content and verify web server accessibility

#### 6. Security Group Update
Add inbound rule for TCP port 8000:
![Add Rule](images/Task_3%20Configuring%20the%20LAMP%20stack%20environment%20and%20confirming%20that%20the%20web%20server%20is%20accessible/add%20rule.png)

Verify the index page is accessible:
![Index Accessible](images/Task_3%20Configuring%20the%20LAMP%20stack%20environment%20and%20confirming%20that%20the%20web%20server%20is%20accessible/index%20accessible.png)

---

## Task 4: Installing the Café Application

### Objectives:
- Download application files
- Configure application parameters
- Set up the database
- Configure PHP timezone

### Steps:

#### 1. Application Files Download
Download setup.zip, db.zip, cafe.zip and extract files to appropriate directories. Download AWS SDK for PHP.

#### 2. Application Parameters Configuration
Run `set-app-parameters.sh` script to create 7 secrets in AWS Secrets Manager:
![App Parameters](images/Task_3%20Configuring%20the%20LAMP%20stack%20environment%20and%20confirming%20that%20the%20web%20server%20is%20accessible/appParametres.png)

#### 3. Database Configuration
Set root password, create cafe_db database, and populate product table:
![Create DB](images/Task_3%20Configuring%20the%20LAMP%20stack%20environment%20and%20confirming%20that%20the%20web%20server%20is%20accessible/create%20db.png)
![DB Tables](images/Task_3%20Configuring%20the%20LAMP%20stack%20environment%20and%20confirming%20that%20the%20web%20server%20is%20accessible/db%20tables.png)
![Product Table](images/Task_3%20Configuring%20the%20LAMP%20stack%20environment%20and%20confirming%20that%20the%20web%20server%20is%20accessible/product%20table.png)

#### 4. PHP Timezone Configuration
Update timezone to America/New_York and restart Apache:
![Update Timezone](images/Task_3%20Configuring%20the%20LAMP%20stack%20environment%20and%20confirming%20that%20the%20web%20server%20is%20accessible/update%20timezone.png)

#### 5. Troubleshooting IAM Role
If the index is not loaded, verify the IAM role is attached:
![Index Not Loaded](images/Task_3%20Configuring%20the%20LAMP%20stack%20environment%20and%20confirming%20that%20the%20web%20server%20is%20accessible/index%20not%20loaded.png)

Update the IAM role for the instance:
![Update IAM Role](images/Task_3%20Configuring%20the%20LAMP%20stack%20environment%20and%20confirming%20that%20the%20web%20server%20is%20accessible/update%20iam%20role.png)
![Cafe Role](images/Task_3%20Configuring%20the%20LAMP%20stack%20environment%20and%20confirming%20that%20the%20web%20server%20is%20accessible/cafe%20role.png)

---

## Task 5: Testing the Web Application

### Objectives:
- Access café website
- Verify menu page displays correctly
- Place test orders
- Verify order history functionality

### Steps:

1. If menu is not available initially, ensure IAM role is properly attached (as shown in Task 4):
   ![No Menu](images/Task_3%20Configuring%20the%20LAMP%20stack%20environment%20and%20confirming%20that%20the%20web%20server%20is%20accessible/no%20menu.png)

2. After fixing IAM role, verify the menu is available:
   ![Menu Available](images/Task_3%20Configuring%20the%20LAMP%20stack%20environment%20and%20confirming%20that%20the%20web%20server%20is%20accessible/menu%20available.png)

3. Access the café website at `http://<public-ip>:8000/cafe`:
   ![Access Cafe](images/Task_3%20Configuring%20the%20LAMP%20stack%20environment%20and%20confirming%20that%20the%20web%20server%20is%20accessible/access%20cafe%20website.png)

4. Place a test order:
   ![Order Submitted](images/Task_3%20Configuring%20the%20LAMP%20stack%20environment%20and%20confirming%20that%20the%20web%20server%20is%20accessible/order%20submitted.png)

5. Verify the order history functionality:
   ![Order History](images/Task_3%20Configuring%20the%20LAMP%20stack%20environment%20and%20confirming%20that%20the%20web%20server%20is%20accessible/order%20history.png)

---

## Task 6: Creating an AMI and Launching Another EC2 Instance

### Objectives:
- Create SSH key pair
- Create AMI from Lab IDE instance
- Copy AMI to Oregon region
- Launch production instance
- Configure Secrets Manager for Oregon

### Steps:

#### 1. SSH Key Pair Creation
Set static hostname to `cafeserver`, generate RSA key pair, and add public key to authorized_keys:
```bash
sudo hostname cafeserver
ssh-keygen -t rsa -f ~/.ssh/id_rsa
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
```
![Key Pair](images/Task_3%20Configuring%20the%20LAMP%20stack%20environment%20and%20confirming%20that%20the%20web%20server%20is%20accessible/key%20pair.png)

#### 2. AMI Creation
Create AMI named **CafeServer** from Lab IDE instance and wait for AMI to become available

#### 3. AMI Copy to Oregon Region
Copy AMI from us-east-1 to us-west-2:
![Copy AMI](images/Task_3%20Configuring%20the%20LAMP%20stack%20environment%20and%20confirming%20that%20the%20web%20server%20is%20accessible/copy%20ami.png)

Search for the AMI in the other region:
![Search AMI](images/Task_3%20Configuring%20the%20LAMP%20stack%20environment%20and%20confirming%20that%20the%20web%20server%20is%20accessible/search%20ami%20id%20in%20other%20region.png)

Verify AMI is available in the other region:
![AMI Available](images/Task_3%20Configuring%20the%20LAMP%20stack%20environment%20and%20confirming%20that%20the%20web%20server%20is%20accessible/ami%20available%20in%20other%20region.png)

#### 4. Launch Production Instance
Launch **ProdCafeServer** instance in Oregon, configured with:
- t2.small instance type
- Lab VPC Region 2
- cafeSG security group (ports 22, 8000)
- CafeRole IAM profile

![EC2 Region 2](images/Task_3%20Configuring%20the%20LAMP%20stack%20environment%20and%20confirming%20that%20the%20web%20server%20is%20accessible/ec2%20region%202%20creation.png)
![Launching Instance](images/Task_3%20Configuring%20the%20LAMP%20stack%20environment%20and%20confirming%20that%20the%20web%20server%20is%20accessible/lunching%20instance.png)

#### 5. Secrets Manager Configuration for Oregon
Update `set-app-parameters.sh` with:
- Region: us-west-2
- PublicDNS: ProdCafeServer DNS

Run script to create secrets in Oregon region:
![Edited Parameters](images/Task_3%20Configuring%20the%20LAMP%20stack%20environment%20and%20confirming%20that%20the%20web%20server%20is%20accessible/edited%20app%20parameters.png)

---

## Task 7: Verifying the New Café Instance

### Objectives:
- Verify ProdCafeServer instance running in Oregon
- Test website accessibility
- Confirm menu and ordering functionality

### Steps:

1. Verify the new instance is running and accessible:
   ![Verifying Instance](images/Task_3%20Configuring%20the%20LAMP%20stack%20environment%20and%20confirming%20that%20the%20web%20server%20is%20accessible/verifying%20the%20new%20instance.png)

2. Test website accessibility at `http://<prod-public-ip>:8000/cafe`

3. Confirm menu and ordering functionality works correctly

---

## Quiz/Assessment

### Steps:

1. Complete the QCM questions:
   ![QCM](images/Task_3%20Configuring%20the%20LAMP%20stack%20environment%20and%20confirming%20that%20the%20web%20server%20is%20accessible/qcm.png)

2. Continue with additional questions:
   ![QCM Suite](images/Task_3%20Configuring%20the%20LAMP%20stack%20environment%20and%20confirming%20that%20the%20web%20server%20is%20accessible/qcm%20(suite).png)

3. Review justifications for answers:
   ![QCM Justification](images/Task_3%20Configuring%20the%20LAMP%20stack%20environment%20and%20confirming%20that%20the%20web%20server%20is%20accessible/qcm%20justification.png)

---

## Final Architecture

| Component | Development (us-east-1) | Production (us-west-2) |
|-----------|------------------------|------------------------|
| Instance Name | Lab IDE | ProdCafeServer |
| Instance Type | t2.micro | t2.small |
| VPC | Lab VPC | Lab VPC Region 2 |
| IAM Role | CafeRole | CafeRole |
| Security Group | Ports 22, 80, 8000 | cafeSG (Ports 22, 8000) |
| Secrets Manager | 7 secrets configured | 7 secrets configured |

---

## Key Learnings

1. **LAMP Stack on EC2**: Configured Apache, MariaDB, and PHP on Amazon Linux
2. **AWS Secrets Manager**: Securely stored database credentials and application parameters
3. **AMI Creation & Copy**: Created machine images and deployed across regions
4. **Multi-Region Deployment**: Set up development and production environments in different AWS Regions
5. **Security Groups**: Configured inbound rules for web traffic
6. **IAM Roles**: Attached roles for EC2 to access Secrets Manager

---

## Troubleshooting Notes

### Issue: Website not loading menu
**Solution**: Attach CafeRole IAM profile to EC2 instance for Secrets Manager access

### Issue: Cannot access website from internet
**Solution**: Add inbound rule for TCP port 8000 in security group

### Issue: Production instance not working
**Solution**: Run `set-app-parameters.sh` with updated region and DNS values
