---
title: "My automated backups to AWS S3 with client-side AES256"
description: "I'm very paranoid when it comes to my data. For me, the only backup solution that I could possibly use, must be simple and must have client-side AES256 encryption with the passphrase that only I would know."
coverCredit: "The bank vault in NoMad Downtown LA. Image by Benoit Linero."
tocEnabled: true
date: 2019-12-08
keywords: ["security", "encryption", "backup", "s3", "aws", "bash", "infosec"]
draft: false
---

# What I was looking for. Requirements

The reason I didn't just start using some backup software with built-in encryption is a pure lack of trust. So, if you're fine with someone managing your backups, you might think the approach described here is a total overkill. And it's okay. But there are some people out there who have the same trust issues that I do and they might find this article useful, so I write it for them. And also for myself, as a write-up what I did.

What are my personal requirements to a backup automation:

* The storage back-end must be highly available
* The storage back-end must have versioning
* The client must **not** have permission to destroy or corrupt data
* The client must be transparent and obvious in how it works
* The client must be smart enough to synchronize backups — uploads changes only
* The encryption of my backups must happen on my machine and should prompt for a passphrase during the process. It should be technically impossible for someone except me to access the backups.

I know, one can argue about the last one because this passphrase is a shared secret created by a human but I can assure you, nobody else knows my passphrase (unless somebody installs a key-logger software on my computer) and the passphrase is really strong (long, special characters, different casing, numbers, etc).

# 3-2-1 Backup Rule

There is a common rule of backup that says you must be:

* Making at least 3 copies of data, located on physically different storage media;
* Keeping these copies in no less than 2 different formats;
* Always storing at least 1 of these backups off-site (e.g. on the commercial cloud account).

I don't know where exactly this rule came from but you can definitely find some mentions on the internet (e.g [here](https://www.backblaze.com/blog/the-3-2-1-backup-strategy/) and [here](https://www.nakivo.com/blog/3-2-1-backup-rule-efficient-data-protection-strategy/)).

This rule makes sense to me, so I would like to follow this rule as much as I can.

So, for me it would be:

* 3 copies of data: 1 on AWS, 1 on a mSD card, 1 on a USB thumb drive
* 2 formats: I guess this is an exception for me, I store only AES256 encrypted TAR files
* 1 off-site: 1 backup is on AWS, so technically it's off-site of my laptop. Or I can say that my thumb drive or mSD card is off-site, whatever.

Before we talk about how I actually store backups, let's talk about the encryption I use.

# First Step: Encryption

Some time ago I wrote a bash script for encrypting a given directory. It's very simple and is based on the GPG symmetric mode and AES256 algorithm. This script creates a TAR archive and then directs it to GPG that prompts for a passphrase and performs the encryption.

You can find the source code [here on GitHub](https://github.com/rdner/dotfiles/blob/master/files/scripts/vault).

This script produces `*.enc` files that I can upload or store anywhere without fear that somebody can get access to the content.

For me, it's the simplest and most reliable solution: GPG is a very reliable tool and I understand 100% what my script is doing because it's so simple.

# Second Step: Storage in S3

We're not going to talk about my physical devices, it's pretty much manual work there.
What I want to talk about is how I use AWS CLI to sync the local directory where I store backups with a bucket folder in S3.

Obviously, you need an AWS account for everything that follows.

To prepare everything we need:

* Create a user in IAM for running AWS CLI
* Configure your AWS CLI
* Create an S3 bucket and a folder for backups
* Create a policy for the user, so it has write-only access to the bucket folder

And after the preparation we can just use another bash script.

## Create a user

* Go to the [IAM console](https://console.aws.amazon.com/iam/home#/users) and click `Add user`.
* Type in a desirable user name and check **only** `Programmatic access`. Click `Next: Permissions`
* **Don't** add the user to any groups, just skip the step clicking on `Next: Tags`
* No tags, required, just skip clicking `Next: Review`
* Review the user and submit with `Create user`. It's okay to have a warning `This user has no permissions`, we will address it later.

## Configure your AWS CLI

* Install AWS CLI, follow [the official instructions](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv1.html). e.g. in Debian it's just `sudo apt install awscli`.
* Go to [IAM console](https://console.aws.amazon.com/iam/home#/users)
* Click on your user you created
* Go to `Security credentials` tab
* Click `Create access key` and you'll a dialog with `Access key ID` and `Secret access key`. **Don't close this dialog***.
* Go to your terminal and type `awscli configure`
* Follow the [official instructions](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.html), you'll need the `Access key ID` and `Secret access key` from the previous step

Now you should have a ready-to-work AWS CLI setup on your computer.

## Create S3 bucket

* Go to the [S3 console](https://s3.console.aws.amazon.com/s3/home) and click `Create bucket`
* Type in a desirable name, select a region and click `Next`
* On the next `Configure options` step check `Keep all versions of an object in the same bucket` and click `Next`.
* On the next `Set permissions` step make sure it says `Block all public access` and click `Next`.
* After reviewing and creating the bucket browse the bucket and create a folder for backups in it

## Create a policy

Now it's time to allow our CLI user to upload new backups, for this we need to create a policy:

* Go to the [IAM console](https://console.aws.amazon.com/iam/home#/users) and click on the user you created earlier
* Click `Add inline policy`
* AWS has this visual editor by default but it's easier to put this JSON example here instead of describing where to click:

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "ListBackups",
            "Effect": "Allow",
            "Action": "s3:ListBucket",
            "Resource": "arn:aws:s3:::<bucket-name>"
        },
        {
            "Sid": "UploadBackups",
            "Effect": "Allow",
            "Action": "s3:PutObject",
            "Resource": "arn:aws:s3:::<bucket-name>/<backup-folder>/*"
        }
    ]
}
```

Replace `<bucket-name>` and `<backup-folder>` with your values. `s3:ListBucket` permission is required for the `aws s3 sync` command that we're going to use, so it's able to compare file sizes and modification dates.

## Use the backup bash script

I wrote a simple bash script that finds all files with `.enc` extension in the current directory and its sub-directories and uploads them to the given location on S3.

You can view and copy the script from my [repository on GitHub](https://github.com/rdner/dotfiles/blob/master/files/scripts/backup).

Now, when our S3 bucket exists and we have an IAM user for our CLI that has a policy to write to a backup folder on the bucket, we can finally run the script:

```bash
backup s3://<bucket-name>/<backup-folder>
```

This script is using `aws s3 sync` command that checks if the local file matches a remote file by size and modification date and if they match, it does not upload your local file anywhere.

# Fulfilled Requirements

As you might remember, I had my very strict requirements in the beginning, how does this approach fulfill them? Let's have a look:

* The storage back-end must be highly available — it's S3
* The storage back-end must have versioning — we turned it on when created the bucket
* The client must **not** have permission to destroy or corrupt data — our policy makes sure the script is allowed to only put objects, can't read or delete them. The user is allowed to upload a file with the same name but it won't corrupt data just will create a new version of it
* The client must be transparent and obvious in how it works — just small bash scripts that you can read and understand
* The client must be smart enough to synchronize backups — `aws s3 sync` uploads only changed or new files to the bucket
* The encryption of my backups must happen on my machine — the first short script also fulfills this requirement for me.

I admit, this setup is not for everyone and it's far from being simple but if you're working with AWS already it might suit you.

I hope you learned something from this article. Take care.

P.S. Please plan your expenses using S3 on AWS in advance. I'm not responsible for any unexpected costs.
