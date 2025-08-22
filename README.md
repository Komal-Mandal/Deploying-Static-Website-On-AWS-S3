# 🌩️ What is AWS S3?

- S3 (Simple Storage Service) is like Google Drive / Dropbox but on AWS Cloud.

- You can store any type of file (photos, videos, documents, backups, website files, etc.).

- Your files are stored inside Buckets (just like folders).

- Each file you upload is called an Object.

- It is safe, scalable, and pay-as-you-go (you pay only for the storage you use).

# ✨ Key Points

- Highly durable → Your data is stored in multiple locations.

- Access control → You can keep files private or make them public.

- Can host static websites → HTML, CSS, JS can be uploaded and served as a website.

# ✅ In short:
AWS S3 = Online storage from Amazon, like a giant cloud hard drive, where you pay only for what you use.

# 🚀 Hosting a Static Website on AWS S3

## 📌 Prerequisites

- AWS account

- Basic knowledge of HTML/CSS/JS

- Your static website files (HTML, CSS, JS, images, etc.)

# 🛠 Steps to Host a Static Website

## 1️⃣ Create an S3 Bucket

- Go in to your AWS Management Console.

- Go to S3 service.

- Click Create bucket.

- Enter a unique bucket name (e.g., my-first-static-website).

- Choose your region.

- Uncheck “Block all public access”.

- Click Create bucket.

## 2️⃣ Upload Website Files

- Open your bucket.

- Click Upload → Add files.

- Upload your index.html and other files.

- Click Upload.

## 3️⃣ Enable Static Website Hosting

- Inside your bucket → Properties.

- Scroll to Static website hosting.

- Select Use this bucket to host a website.

Enter:

- Index document: index.html

- Error document: error.html (optional)

- Save changes.

## 4️⃣ Make Files Public

Go to your bucket → Permissions.

Add a Bucket policy:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::my-first-static-website/*"
    }
  ]
}
```

## 5️⃣ Access Your Website 🌍

- Go to Bucket → Properties → Static website hosting.

- Copy the Endpoint URL.

- Paste it into your browser →  Your website is live!

## 🔄 Enable Versioning in S3

Versioning helps you keep multiple versions of the same file.

Example: If you overwrite index.html, the old version is still stored and can be restored.

## Steps:

- Open your S3 bucket.

- Go to Properties → Bucket Versioning.

- Click Edit → Enable Versioning → Save changes.

- Now, whenever you upload a new file with the same name, S3 will keep the old version also.

# S3 Replication

S3 Replication means automatically copying objects from one S3 bucket to another.

This helps in backup, disaster recovery, or compliance.

## There are 2 types:

- Same-Region Replication (SRR) → Copy objects to another bucket in the same AWS region.

- Example: Backup data within Mumbai region.

- Cross-Region Replication (CRR) → Copy objects to another bucket in a different AWS region.

- Example: Copy from Mumbai bucket to Singapore bucket.

## Steps to Enable Replication

- Open your Source Bucket → Go to Management tab.

- Find Replication rules → Click Create replication rule.

- Enter a Rule name.

- Choose Destination Bucket (must be in same account or another account).

- Enable Bucket Versioning on both source and destination (required).

- Save the rule .

## 📌 Things to Remember

- Versioning must be enabled in both source & destination buckets.

- Replication only works for new objects (files uploaded after rule is set).

- You can choose to replicate:

      1. All objects

      2. Objects with specific tags or prefixes

## 🎯 Example Use Cases

- 🌐 Global users → Copy data to closer regions for faster access.

- 🔒 Compliance → Keep a copy in another region for disaster recovery.

- 🗂 Backup → Keep a second copy of files automatically.

# ⏳ S3 Bucket Lifecycle

Lifecycle rules in S3 help you automate how long files stay in storage and what happens to them later.

Example: Move old files to cheaper storage (Glacier) or delete them after a few days.

## What Can Lifecycle Do?

Transition actions → Move objects to another storage class

Example: After 30 days, move from Standard → Infrequent Access (IA).

After 90 days, move to Glacier (very cheap, for archive).

Expiration actions → Delete objects after a certain time

Example: Delete log files after 365 days.

Clean up incomplete uploads

Example: Delete unfinished multipart uploads after 7 days.

## Steps to Add a Lifecycle Rule

- Open your S3 bucket → Go to Management tab.

- Scroll to Lifecycle rules → Click Create lifecycle rule.

- Enter a Rule name.

- Choose what to apply rule to:

    - All objects

    - Objects with specific prefix (e.g., logs/)

    - Objects with specific tags

- Add Transitions / Expirations:

    - Transition after 30 days → Standard-IA

    - Transition after 90 days → Glacier

    - Expire after 365 days → Delete file

- Save the rule 

## 📌 Example Use Case

- Logs → Keep in Standard for 7 days → Move to Glacier for 1 year → Auto-delete after 2 years.

- Website backups → Store in Standard for 30 days → Move to Infrequent Access → Keep forever.

## 🎯 Why Lifecycle is Useful?

- Saves money (old data moves to cheaper storage).

- Keeps bucket clean (delete old/unnecessary files).

# 🌨️ AWS Snow Family 

<img width="360" height="180" alt="image" src="https://github.com/user-attachments/assets/d428c11c-fc9c-44cc-856f-2faa7091eca0" />

The AWS Snow Family is a group of physical devices that help you move a large amount of data between your on-premises location (like your office, data center, or local servers) and AWS Cloud (S3, EC2, etc.).

Instead of transferring TBs or PBs of data over the internet (which is slow, costly, and risky), Amazon ships you a Snow device. You copy your data into it, then send it back to AWS. They upload it directly into your S3 bucket.

## 🚀 Members of Snow Family

### Snowcone 🪶

- Smallest device (like a lunchbox).

- Holds a few TBs of data.

- Portable and can even run at remote/edge locations (ships, factories, remote sites).

- Good for edge computing + small data transfer.

### Snowball 📦

- Medium-sized device (like a suitcase).

- Good for 10s to 100s of TBs of data transfer.

### Snowmobile 🚚

- Giant truck-sized device (literally a container on a truck).

- Used when you need to move exabytes (1,000+ PBs) of data.

- AWS brings a truck to your data center, loads your data, and ships it securely.

## When to Use Snow Family with S3?

- If you have huge datasets (videos, IoT data, backups, archives) that are too slow to upload via the internet.

- If your internet is expensive or unreliable.

- For data migration, disaster recovery, or edge computing.

### 📌 Example:

Suppose you have 500 TB of video data from CCTV cameras in your company. Uploading this over the internet could take months.
Instead, AWS sends you a Snowball Edge. You copy all data, ship it back, and AWS uploads it directly into your S3 bucket.

## 🗄️ AWS Storage Gateway 

<img width="785" height="361" alt="image" src="https://github.com/user-attachments/assets/5dd4ec3c-ed89-489a-9a32-b1fc5b0381c1" />

- Think of it as a connector between your office computer storage and AWS Cloud storage.

- If you save files in your office (on-premise), they also get stored in AWS S3/Glacier/EBS.

- It helps in backup, recovery, and moving data to the cloud without losing local access.
