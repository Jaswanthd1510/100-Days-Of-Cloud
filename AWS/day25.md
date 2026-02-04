# Day 25: Monitoring with CloudWatch Alarms

## 📝 Task Details
The Nautilus DevOps team needs automated monitoring to detect high load on their application servers.
* **Task:** Create a CloudWatch Alarm for high CPU usage.
* **Instance:** `nautilus-ec2` (Ubuntu).
* **Alarm Name:** `nautilus-alarm`.
* **Threshold:** CPU >= 90% for 1 consecutive 5-minute period.
* **Action:** Notify existing SNS topic `nautilus-sns-topic`.

---

## 📚 Theory: Monitoring & Observability
* **CloudWatch Alarm:** A mechanism that watches a single metric over a specified time period and performs one or more actions based on the value of the metric relative to a threshold.
* **Metric:** The raw data being monitored (e.g., CPUUtilization, DiskReadBytes).
* **SNS Integration:** Alarms are decoupled from notifications. The Alarm triggers the SNS Topic, and the SNS Topic handles the actual delivery (Email, SMS, etc.).

---

## ⚙️ Solution (AWS Console)

### Step 1: Launch the EC2 Instance
1.  **Navigate:** Go to the **EC2 Dashboard** and click **Launch instances**.
2.  **Name and tags:** Enter `nautilus-ec2`.
3.  **Application and OS Images (AMI):** Select **Ubuntu**.
4.  **Instance type:** Select `t2.micro`.
5.  **Key pair:** Select an existing key pair (or proceed without one).
6.  **Network settings:** Default VPC and Security Group settings are acceptable for this task.
7.  **Finalize:** Click **Launch instance**.
    * *Wait 1-2 minutes for the instance to enter the `Running` state. Copy its **Instance ID** (e.g., `i-0abcd...`) as you will need it shortly.*

### Step 2: Create the CloudWatch Alarm
1.  **Navigate:** Go to the **CloudWatch** service dashboard.
2.  **Menu:** In the left sidebar, click **Alarms** > **All alarms**.
3.  **Start:** Click the orange **Create alarm** button.
4.  **Select Metric:**
    * Click **Select metric**.
    * Choose **EC2** > **Per-Instance Metrics**.
    * Paste your Instance ID in the search bar to find your specific instance.
    * Check the box next to the metric name **CPUUtilization**.
    * Click **Select metric**.
5.  **Configure Metric Conditions:**
    * **Statistic:** Select **Average**.
    * **Period:** Select **5 minutes**.
    * **Threshold type:** Static.
    * **Condition:** Greater than or equal to (`>=`) **90**.
    * Click **Next**.
6.  **Configure Actions:**
    * **Alarm state trigger:** Select **In alarm**.
    * **Send a notification to the following SNS topic:**
        * Select **Select an existing SNS topic**.
        * In the "Send to" dropdown, find and select **`nautilus-sns-topic`**.
    * Click **Next**.
7.  **Add Name:**
    * **Alarm name:** Enter `nautilus-alarm`.
    * Click **Next**.
8.  **Review and Create:**
    * Review all settings.
    * Scroll to the bottom and click **Create alarm**.

---

## ✅ Verification
I verified the alarm configuration using the AWS CLI to ensure all parameters were set correctly.

**1. Verification Command:**
```bash
aws cloudwatch describe-alarms --alarm-names nautilus-alarm
```
**2. Success Criteria:**

    AlarmName: "nautilus-alarm"

    MetricName: "CPUUtilization"

    Threshold: 90.0

    AlarmActions: Contains the ARN of nautilus-sns-topic.