# AWS CloudTrail integration with Wazuh 4.13

## 1. Create an AWS account

1. Go to [https://aws.amazon.com/](https://aws.amazon.com/)
2. Click **Create an AWS Account**.
3. Fill in:

   * Email
   * Password
   * Phone number
   * Credit/debit card (required for Free Tier)
4. After registration, log in to the AWS Console:
   `console.aws.amazon.com`
5. In the top-right corner choose a region — for example:

   * `eu-north-1` (Stockholm)

---

## 2. Create an S3 bucket for CloudTrail logs

1. In the AWS search bar at the top type **S3** and open the service.
2. Click **Create bucket**.
3. Fill in:

   * **Bucket name**: `wazuh-cloudtrail-logs-<name>`
   * **Region**: the same region you selected above
4. Keep public access **blocked**.
5. Leave other settings as default.
6. Done.

---

## 3. Configure CloudTrail to send logs to S3

1. In the AWS search bar type **CloudTrail** → open **Trails**.
2. Click **Create trail**.
3. Fill in:

   * **Trail name**: `wazuh-cloudtrail-trail-<name>`
4. In **Storage location** choose:

   * **Use existing bucket** → select your bucket (from the previous section).
5. **Multi-region trail** — you can keep it enabled.
6. **Log file SSE-KMS encryption** — uncheck this option.
7. Click **Create trail**.

Now CloudTrail will store `.json.gz` files in your S3 bucket.

---

## 4. Create an IAM policy for Wazuh

Wazuh must have permissions to:

* `s3:ListBucket`
* `s3:GetObject`

1. Open **IAM → Policies → Create policy**.
2. Select the **JSON** tab.
3. Paste the following:

<details>
  <summary>Show IAM policy JSON</summary>

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": ["s3:ListBucket"],
      "Resource": "arn:aws:s3:::wazuh-cloudtrail-logs-<name>"
    },
    {
      "Effect": "Allow",
      "Action": ["s3:GetObject"],
      "Resource": "arn:aws:s3:::wazuh-cloudtrail-logs-<name>/*"
    }
  ]
}
```

</details>

4. Name the policy: `WazuhCloudTrailAccess`
5. Create the policy.

---

## 5. Create an IAM user and access keys

1. Go to **IAM → Users → Create user**.
2. User name: `wazuh-aws-reader`
3. In **Permissions** choose **Attach policies directly** and attach:

   * `WazuhCloudTrailAccess`
4. Finish user creation.
5. On the user page, click **Create access key**.

Save the **Access key ID** and **Secret access key** — you will need them later.

---

## 6. Configure AWS credentials on the Wazuh Manager

On the Wazuh server:

```bash
sudo su -
mkdir -p /root/.aws
nano /root/.aws/credentials
```

Paste:

```ini
[wazuh-aws]
aws_access_key_id = <your AccessKeyID>
aws_secret_access_key = <your SecretAccessKey>
```

Save the file.

Then create the config file:

```bash
nano /root/.aws/config
```

```ini
[profile wazuh-aws]
region = eu-north-1
output = json
```

---

## 7. Install AWS CLI (optional, for testing)

<details>
  <summary>Installation commands for AWS CLI</summary>

```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
```

Verification:

```bash
/usr/local/bin/aws --version
```

</details>

---

## 8. Test the AWS profile

```bash
/usr/local/bin/aws sts get-caller-identity --profile wazuh-aws
```

If you see `UserId` and `Arn` in the output, the profile works correctly.

---

## 9. Test access to the S3 bucket

```bash
/usr/local/bin/aws s3 ls s3://wazuh-cloudtrail-logs-<name> --profile wazuh-aws
```

If you see something like:

```text
PRE AWSLogs/
```

→ access to the bucket is working.

---

## 10. Configure Wazuh for AWS S3

Open the Wazuh configuration:

```bash
nano /var/ossec/etc/ossec.conf
```

Add the following **before** `</ossec_config>`:

<details>
  <summary>aws-s3 wodle configuration</summary>

```xml
<wodle name="aws-s3">
  <disabled>no</disabled>
  <interval>30m</interval>
  <run_on_start>yes</run_on_start>
  <skip_on_error>no</skip_on_error>
  <bucket type="cloudtrail">
    <name>wazuh-cloudtrail-logs-<name></name>
    <aws_profile>wazuh-aws</aws_profile>
  </bucket>
</wodle>
```

</details>

Save the file.

---

## 11. Restart Wazuh Manager

```bash
sudo systemctl restart wazuh-manager
```

---

## 12. Check Wazuh logs

```bash
sudo tail -f /var/ossec/logs/ossec.log | grep aws
```

Expected log lines:

```text
INFO: Starting fetching of logs
INFO: Executing Bucket Analysis
INFO: Fetching logs finished
```

There should be no error messages.

---

## 13. Check that CloudTrail events appear

CloudTrail writes files to S3 with a delay of about 1–5 minutes.
Check:

```bash
/usr/local/bin/aws s3 ls s3://wazuh-cloudtrail-logs-<name>/AWSLogs/ --recursive --profile wazuh-aws
```

If you see `.json.gz` files → Wazuh will process them.

---

## 14. Check in the Wazuh Dashboard

Open the **Amazon Web Services** section in the Wazuh dashboard.

Integration is complete — CloudTrail events are now ingested into Wazuh.

