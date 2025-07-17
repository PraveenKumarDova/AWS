## Amazon Machine Image (AMI) – End-to-End Practical Workflow for Students

This document explains how to create a **custom AMI** from an EC2 instance, the relationship between AMIs and snapshots, how to launch EC2 instances using the custom AMI, and how to copy it across **regions** and **accounts**. This is often used in real-world projects for scaling, disaster recovery, migration, and replication of standardized environments.

---

### Step 1: Launch a Base EC2 Instance

1. Go to EC2 Console → Launch Instance
2. Select **Amazon Linux 2 AMI** (Free tier eligible)
3. Choose instance type: `t2.micro`
4. Configure details:

   * VPC: Default or custom
   * Auto-assign Public IP: Enabled
5. Add 8 GB gp2/gp3 EBS root volume (default)
6. Configure Security Group:

   * Allow SSH (port 22)
   * Allow HTTP (port 80)
7. Select or create a key pair and launch the instance

---

### Step 2: Configure the Instance (Install Apache)

1. Connect via SSH:

```bash
ssh -i my-key.pem ec2-user@<Public-IP>
```

2. Update and install Apache (httpd):

```bash
sudo yum update -y
sudo yum install httpd -y
```

3. Start and enable Apache:

```bash
sudo systemctl start httpd
sudo systemctl enable httpd
```

4. Add a custom index page:

```bash
  echo "<h1>This is a custom AMI-based Apache Web Server</h1>" | sudo tee /var/www/html/index.html
```

5. Test in browser:

```
http://<Public-IP>
```

---

### Step 3: Create AMI from the Instance

1. Go to **EC2 Console → Instances**
2. Right-click the running instance → **Image and templates** → **Create image**
3. Fill in:

   * Name: `custom-apache-ami`
   * Description: `AMI with pre-installed Apache and custom homepage`
   * No reboot: Optional (checking it avoids reboot)
4. Click **Create image**

#### What Happens Internally:

* AWS **creates a snapshot** of the root EBS volume (and any additional volumes)
* AMI metadata is stored, linking to the snapshot
* It takes a few minutes to appear under **AMIs** (left menu)

---

### Step 4: Launch EC2 Instances from the AMI

1. Go to **EC2 Console → AMIs**

2. Select your AMI (`custom-apache-ami`)

3. Click **Launch instance from image**

4. Choose instance type, network, key pair, and security group (same as normal EC2 launch)

5. Launch

6. Access the new instance:

```bash
ssh -i my-key.pem ec2-user@<Public-IP>
```

7. Open browser:

```
http://<Public-IP>
```

You will see the same Apache custom page as in the original instance.

---

### Step 5: Copy AMI to Another Region

1. Go to **EC2 Console → AMIs**

2. Select the AMI → **Actions → Copy AMI**

3. Choose:

   * Destination Region: Example – Singapore (`ap-southeast-1`)
   * Name: `custom-apache-ami-copy`
   * Encryption: optional (can enable or keep same)

4. Click **Copy AMI**

5. Switch to the destination region in the top-right menu

6. Go to **AMIs** and wait for the copied image to appear

From here, launch new EC2 instances as usual.

---

### Step 6: Share AMI to Another AWS Account

1. Go to **EC2 Console → AMIs**
2. Select the AMI → **Modify Image Permissions**
3. Change from **Private** to **Add account ID**

   * Enter the 12-digit AWS Account ID of the target account
   * Click **Save**

#### On the Target Account:

1. Log in to target AWS account
2. Go to **EC2 → AMIs → Owned by other accounts**
3. Select the shared AMI and **copy it** to your account
4. Use the copied AMI to launch EC2 instances

---

### Additional Teaching Points

* Creating an AMI is a **best practice** before making major changes to EC2.
* AMIs can include **multiple volumes** (e.g., root + data).
* Snapshots used by AMIs are **stored in the same region** as the instance.
* AMIs are **region-specific** by default. They must be copied to be used in other regions.
* You can create AMIs with **encrypted snapshots** for secure environments.
* AMIs help in creating **identical environments** for horizontal scaling or multi-AZ setups.

---

This full cycle covers EC2 → Customization → AMI → Reuse → Cross-region/cross-account deployment. It mirrors what real companies do for templating production-ready environments.
