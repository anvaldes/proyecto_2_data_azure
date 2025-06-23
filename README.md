# 🔥 Simple PySpark Pipeline on Azure HDInsight

This repository contains a lightweight **PySpark-based pipeline** that runs on **Azure HDInsight (Spark 3.1)**, designed to follow a minimal and reproducible workflow:

**➡️ Create Cluster → Upload Job to Blob Storage → Run Job → Check Output → Delete Cluster**

No Terraform is required, and all resources can be provisioned from the Azure CLI.

---

## ⚙️ Workflow Overview

```text
1. Create HDInsight Spark Cluster
2. Upload PySpark Job to Azure Blob Storage
3. Connect via SSH and run the job
4. Validate output in Blob Storage
5. Delete the Cluster
```

## 🚀 Run PySpark Job on Azure HDInsight

### 🛠️ Step 1: Create the Spark Cluster

```bash
az hdinsight create \
  --name spark-cluster-job \
  --resource-group rg-proyecto1 \
  --type spark \
  --location centralus \
  --version 5.0 \
  --component-version Spark=3.1 \
  --cluster-tier Standard \
  --headnode-size Standard_E4a_v4 \
  --workernode-size Standard_E4a_v4 \
  --workernode-count 2 \
  --storage-account proyecto1bvaldes0622 \
  --storage-account-key "<YOUR_STORAGE_KEY>" \
  --storage-container data-pipeline-1 \
  --ssh-user azureuser \
  --ssh-password "TuPasswordSegura123" \
  --http-user admin \
  --http-password "TuPasswordWeb123"
```

### 📤 Step 2: Upload the PySpark Job

```bash
az storage blob upload \
  --account-name proyecto1bvaldes0622 \
  --container-name data-pipeline-1 \
  --name script_pyspark/job.py \
  --file ./job.py
```

### 🔐 Step 3: Connect via SSH and Run the Job

```bash
ssh azureuser@spark-cluster-job-ssh.azurehdinsight.net

spark-submit wasbs://data-pipeline-1@proyecto1bvaldes0622.blob.core.windows.net/script_pyspark/job.py
```

### 🧹 Step 4: Delete the Cluster

```bash
az hdinsight delete \
  --name spark-cluster-job \
  --resource-group rg-proyecto1 \
  --yes
```

---




