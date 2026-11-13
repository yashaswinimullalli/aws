# AWS Lab 4 – Working with EBS

## Aim
To create an Amazon EBS volume, attach it to an EC2 instance, format and mount it, create a snapshot, and restore the data using a new volume.

## Prerequisites
- Start the Vocareum lab and wait until **Lab status: ready**.
- Open the AWS Management Console.
- An EC2 instance named **Lab** is already available.
- Use the AWS Region and Availability Zone assigned to the lab.

---

## Task 1: Create a New EBS Volume

1. Open **EC2 → Instances** and note the Availability Zone of the **Lab** instance.
2. Go to **EC2 → Volumes → Create volume**.
3. Configure:
   - **Volume type:** General Purpose SSD (gp2)
   - **Size:** 1 GiB
   - **Availability Zone:** Same AZ as the Lab instance
4. Add tag: `Name = My Volume`.
5. Choose **Create volume**.
6. Refresh the Volumes page until the volume state becomes **Available**.

## Task 2: Attach the Volume to an Instance

1. Select **My Volume**.
2. Choose **Actions → Attach volume**.
3. Select the **Lab** instance.
4. Set **Device name:** `/dev/sdb`.
5. Choose **Attach volume**.
6. Confirm that the volume state becomes **In-use**.

## Task 3: Connect to Your Amazon EC2 Instance

1. Go to **EC2 → Instances**.
2. Select the **Lab** instance and choose **Connect**.
3. Open the **Session Manager** tab and choose **Connect**.
4. In the terminal, switch to the `ec2-user` account:

```bash
sudo su -l ec2-user
```

## Task 4: Create and Configure Your File System

1. Check the available storage:

```bash
df -h
```

2. Create an ext3 file system on the new volume:

```bash
sudo mkfs -t ext3 /dev/sdb
```

3. Create a mount directory:

```bash
sudo mkdir /mnt/data-store
```

4. Mount the volume:

```bash
sudo mount /dev/sdb /mnt/data-store
```

5. Configure automatic mounting at startup:

```bash
echo "/dev/sdb /mnt/data-store ext3 defaults,noatime 1 2" | sudo tee -a /etc/fstab
```

6. Verify the configuration:

```bash
cat /etc/fstab
```

7. Check storage again and confirm the new volume is mounted at `/mnt/data-store`:

```bash
df -h
```

8. Create a file on the mounted volume:

```bash
sudo sh -c "echo some text has been written > /mnt/data-store/file.txt"
```

9. Verify the file contents:

```bash
cat /mnt/data-store/file.txt
```

## Task 5: Create an Amazon EBS Snapshot

1. In the AWS Console, go to **EC2 → Volumes**.
2. Select **My Volume** and choose **Actions → Create snapshot**.
3. Add tag: `Name = My Snapshot`.
4. Choose **Create snapshot**.
5. Go to **EC2 → Snapshots** and wait until the snapshot status becomes **Completed**.
6. Return to the Session Manager terminal and delete the original file:

```bash
sudo rm /mnt/data-store/file.txt
```

7. Verify that it has been deleted:

```bash
ls /mnt/data-store/
```

## Task 6: Restore the Amazon EBS Snapshot

### A. Create a volume from the snapshot

1. In **EC2 → Snapshots**, select **My Snapshot**.
2. Choose **Actions → Create volume from snapshot**.
3. Select the same Availability Zone used earlier.
4. Add tag: `Name = Restored Volume`.
5. Choose **Create volume**.

### B. Attach the restored volume

1. Go to **EC2 → Volumes** and select **Restored Volume**.
2. Choose **Actions → Attach volume**.
3. Select the **Lab** instance.
4. Set **Device name:** `/dev/sdc` (or the next available device identifier).
5. Choose **Attach volume** and confirm the state is **In-use**.

### C. Mount and verify the restored volume

1. Create a mount directory:

```bash
sudo mkdir /mnt/data-store2
```

2. Mount the restored volume:

```bash
sudo mount /dev/sdc /mnt/data-store2
```

3. Verify that the deleted file is present in the restored volume:

```bash
ls /mnt/data-store2/
```

Expected file: `file.txt`

---

## Submission and Lab Completion

1. Choose **Submit** at the top of the lab instructions.
2. Choose **Yes** to confirm.
3. Wait for the Grades panel, or choose **Grades** after a couple of minutes.
4. Open **Submission Report** to review task feedback.
5. If needed, correct the work and submit again; the last submission is recorded.
6. Choose **End Lab**, confirm with **Yes**, and close the completion panel.

## Result

Successfully created and attached an EBS volume, formatted it with an ext3 file system, mounted it, stored a file, created a snapshot, restored the snapshot to a new volume, and verified that the original file was recovered.

## Important Concepts for Revision

| Concept | Meaning |
|---|---|
| EBS volume | Persistent block storage for an EC2 instance |
| Availability Zone | An EBS volume must be attached to an EC2 instance in the same AZ |
| Attach | Connects an EBS volume to an EC2 instance |
| File system | Organizes data on a storage device; this lab uses ext3 |
| Mount point | Directory through which the file system is accessed |
| `/etc/fstab` | Configuration file used to define file systems mounted at startup |
| Snapshot | Point-in-time backup of an EBS volume |
| Restore | Creates a new EBS volume from a snapshot |

## Quick Memory Flow

**Create → Attach → Connect → Format & Mount → Snapshot → Restore**

- Create: 1 GiB volume named **My Volume**
- Attach: `/dev/sdb`
- Mount: `/mnt/data-store`
- Backup: **My Snapshot**
- Restore: **Restored Volume**, attached as `/dev/sdc`
- Verify: `file.txt` exists under `/mnt/data-store2/`
