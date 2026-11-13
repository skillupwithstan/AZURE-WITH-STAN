Here is the markdown file content for **"Azure Storage - Recover Deleted Items"**, including the embedded clickable YouTube video thumbnail and link:

---

# Azure Storage - Recover Deleted Items

Accidental deletions can happen in any cloud environment. Fortunately, Microsoft Azure provides built-in mechanisms to recover deleted storage accounts, containers, and blobs within specific retention windows.

---

## 1. How to Recover a Deleted Storage Account

* **Retention Period:** Deleted storage accounts can typically be recovered within **14 days** of deletion.
* **Steps via Azure Portal:**
1. Navigate to **Storage accounts** in the Azure Portal.
2. Click on the **Restore** button located at the top menu bar [[01:48](https://www.youtube.com/watch?v=hFVx30VEf9c&t=108)].
3. Select your subscription and choose the deleted storage account from the list of recently deleted accounts [[01:57](https://www.youtube.com/watch?v=hFVx30VEf9c&t=117)].
4. Click **Restore** to recreate the storage account with its original configuration [[02:15](https://www.youtube.com/watch?v=hFVx30VEf9c&t=135)].



---

## 2. How to Recover a Deleted Container

* **Prerequisites:** Soft delete must be enabled beforehand under **Data protection** [[06:03](https://www.youtube.com/watch?v=hFVx30VEf9c&t=363)].
* **Steps via Azure Portal:**
1. Open your specific **Storage Account** and go to **Containers** under Data storage [[03:28](https://www.youtube.com/watch?v=hFVx30VEf9c&t=208)].
2. Toggle on the **Show deleted containers** option [[04:05](https://www.youtube.com/watch?v=hFVx30VEf9c&t=245)].
3. Locate the container with the status set to *Deleted* [[04:09](https://www.youtube.com/watch?v=hFVx30VEf9c&t=249)].
4. Click the options menu (`...`) next to the container and select **Undelete** (or use the top **Restore containers** option) [[04:28](https://www.youtube.com/watch?v=hFVx30VEf9c&t=268)].



---

## 3. How to Recover a Deleted Blob (Object)

* **Prerequisites:** **Enable soft delete for blobs** must be turned on in the **Data protection** settings of your storage account with a retention period configured [[06:05](https://www.youtube.com/watch?v=hFVx30VEf9c&t=365)].
* **Steps via Azure Portal:**
1. Navigate to the target **Container** where the blob resided [[04:53](https://www.youtube.com/watch?v=hFVx30VEf9c&t=293)].
2. Toggle on the **Show deleted blobs** switch [[05:21](https://www.youtube.com/watch?v=hFVx30VEf9c&t=321)].
3. Select the deleted blob (marked with a *Deleted* status) [[05:24](https://www.youtube.com/watch?v=hFVx30VEf9c&t=324)].
4. Click on the options menu (`...`) and choose **Undelete** to restore it back to active status [[05:35](https://www.youtube.com/watch?v=hFVx30VEf9c&t=335)].



---

## 4. Best Practices: Overcoming Accidental Deletions

To prevent accidental deletions across your Azure Storage resources, implement resource locks:

* Go to your **Storage Account** -> **Locks** under Settings [[07:00](https://www.youtube.com/watch?v=hFVx30VEf9c&t=420)].
* Click **+ Add**, provide a lock name (e.g., `AvoidDeletion`), and set the lock type to **Delete** [[07:08](https://www.youtube.com/watch?v=hFVx30VEf9c&t=428)].
* This prevents any accidental or unauthorized deletions of the critical storage resource.