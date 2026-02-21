# Terraform – Create and Manage S3 Bucket (Beginner Project)

In this project, I learned how to install Terraform, connect it to AWS, and use it to create an S3 bucket.

Instead of creating resources manually in the AWS Console, I used Infrastructure as Code (IaC) to define everything in a file and let Terraform build it for me.

(flow diagram)
<img width="1030" height="191" alt="Screenshot 2026-02-20 at 10 50 27 PM" src="https://github.com/user-attachments/assets/a4863ff4-90d3-4584-8511-8cd8b97329df" />

---

## What I Learned

- How to install Terraform
- What Infrastructure as Code (IaC) means
- How to connect Terraform to AWS using AWS CLI
- How to create an S3 bucket using Terraform
- How to upload a file to S3 using Terraform
- How to delete everything properly using `terraform destroy`

---

# Step 1 – Install Terraform

I downloaded Terraform from the official HashiCorp website.

After unzipping it, I moved the Terraform binary into my system PATH so I can run:

```bash
terraform version
```

If this shows a version number, it means Terraform is installed correctly.

(terraform registry documentation search)

<img width="634" height="387" alt="Screenshot 2026-02-20 at 10 51 29 PM" src="https://github.com/user-attachments/assets/63762380-0033-45be-8465-55a88281ed93" />

---

# Step 2 – Create Project Folder

I created a folder on my Desktop:

```bash
mkdir ~/Desktop/nextwork_terraform
cd ~/Desktop/nextwork_terraform
```

Then I created the main Terraform file:

```bash
touch main.tf
```

This `main.tf` file is where I write my infrastructure code.

(create terraform folder and initialized)

<img width="635" height="395" alt="Screenshot 2026-02-20 at 10 51 59 PM" src="https://github.com/user-attachments/assets/d9555adc-a241-45be-9d79-2e1ccace81b0" />

---

# Step 3 – Write Terraform Configuration

Inside `main.tf`, I added code to:

- Set AWS as the provider
- Create an S3 bucket
- Block public access to the bucket

Example:

```terraform
provider "aws" {
  region = "us-east-1"
}

resource "aws_s3_bucket" "my_bucket" {
  bucket = "nextwork-unique-bucket-123456789"
}

resource "aws_s3_bucket_public_access_block" "my_bucket_public_access_block" {
  bucket = aws_s3_bucket.my_bucket.id

  block_public_acls       = true
  ignore_public_acls      = true
  block_public_policy     = true
  restrict_public_buckets = true
}
```

Important:
The bucket name must be globally unique.

(the code and terminal shows terraform init, plan and apply)

<img width="633" height="393" alt="Screenshot 2026-02-20 at 10 53 40 PM" src="https://github.com/user-attachments/assets/af8505f2-9402-4f5f-aafa-661d40e20bfd" />

---

# Step 4 – Install and Configure AWS CLI

Terraform needs AWS credentials to create resources.

I installed AWS CLI and ran:

```bash
aws --version
```

Then I created an access key in IAM and configured it:

```bash
aws configure
```

I entered:
- Access Key ID
- Secret Access Key
- Region (example: us-east-1)
- Output format (left blank)

Now Terraform can authenticate with AWS.

(setup aws cli access key from aws iam)

<img width="634" height="392" alt="Screenshot 2026-02-20 at 10 52 34 PM" src="https://github.com/user-attachments/assets/bb1138c9-5148-4be6-8169-03fed4240aa3" />

---

# Step 5 – Initialize and Plan

Inside the project folder, I ran:

```bash
terraform init
```

This downloads the AWS provider and prepares the project.

Then I ran:

```bash
terraform plan
```

This shows what Terraform is about to create.

It showed:
- 1 S3 bucket
- 1 public access block

No changes or deletes.

---

# Step 6 – Apply Configuration

To actually create the bucket:

```bash
terraform apply
```

Typed `yes` to confirm.

After that, I checked the AWS S3 console and saw the new bucket created successfully.

(verify the bucket is created in aws s3 bucket after run terraform)

<img width="636" height="378" alt="Screenshot 2026-02-20 at 10 53 07 PM" src="https://github.com/user-attachments/assets/65f6ca60-6cbd-4246-aeea-76f17c0a1fda" />

---

# Upload File Using Terraform

I added this block to `main.tf`:

```terraform
resource "aws_s3_object" "image" {
  bucket = aws_s3_bucket.my_bucket.id
  key    = "image.png"
  source = "image.png"
}
```

Then I ran:

```bash
terraform plan
terraform apply
```

This uploaded `image.png` into the S3 bucket automatically.

No manual upload needed.

---

# Troubleshooting

Here are some common problems I faced:

### 1. No valid credential sources found

This means AWS CLI is not configured.

Fix:
Run:

```bash
aws configure
```

Make sure your access key and region are correct.

---

### 2. Security token is invalid

Usually caused by:
- Wrong access key
- Deleted access key
- Wrong region

Fix:
Delete the old key in IAM and create a new one.

---

### 3. Bucket name already exists

S3 bucket names must be globally unique.

Fix:
Add random numbers at the end of your bucket name.

---

### 4. File not found (image.png)

Terraform cannot find the file.

Fix:
Make sure:
- The file is inside the same folder as `main.tf`
- The name matches exactly (case sensitive)

---

### 5. Terraform not recognized

If `terraform version` fails:

Fix:
Make sure Terraform is inside your PATH.
Run:

```bash
echo $PATH
```

---

# Cleanup – Delete Everything

To remove the bucket and all resources:

```bash
terraform destroy
```

Typed `yes` to confirm.

I also deleted the IAM access key from the AWS Console and removed the .csv file from my computer.

---

# Key Takeaways

- Terraform allows me to define infrastructure in code.
- `terraform init` prepares the project.
- `terraform plan` previews changes.
- `terraform apply` creates resources.
- `terraform destroy` deletes resources.
- AWS CLI is required for authentication.

This project helped me understand the basics of Infrastructure as Code and how Terraform interacts with AWS.

