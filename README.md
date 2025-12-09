# 🌍 Terraform Multiple Providers Lab

In this lab, I learned how Terraform supports **multiple providers** inside the same configuration, how provider plugins are downloaded, how to create resources from multiple providers, how to fix dependency lock file issues, and how to initialize newly added providers correctly.

---

## 📋 Lab Overview

**Goal:**

* Understand how multiple providers can coexist in one Terraform project
* Inspect provider plugin directories
* Add new resource blocks that reference different providers
* Resolve lock-file issues by re-initializing Terraform
* Create local and random provider resources
* Modify and apply infrastructure across directories

**Learning Outcomes:**

* Use `terraform init`, `plan`, `apply` across multiple provider setups
* Inspect `.terraform/providers` to verify downloaded plugins
* Write HCL configurations using multiple provider blocks
* Understand how Terraform installs plugins only when needed
* Fix errors caused by adding new providers after initialization
* Create resources using both `local_file` and `random_pet` providers

---

## 🛠 Step-by-Step Journey

### **Step 1: Can We Use Multiple Providers in One Configuration?**

✔️ **Yes**, Terraform fully supports using multiple providers in the same directory.

---

### **Step 2: Inspect the multi-provider Directory**

Path:

```
/root/terraform-projects/multi-provider
```

Number of initialized providers:
✔️ **0 providers** (before running init)

---

### **Step 3: Initialize Terraform and Inspect Provider Directory**

Ran:

```bash
terraform init
```

Then checked:

```
.terraform/providers
```

Plugins downloaded:
✔️ **2 provider plugins**

---

### **Step 4: Navigate to Directory MPL and Create Config**

Path:

```
/root/terraform-projects/MPL
```

Created new file:

```
pet-name.tf
```

Added **two resources**:

### **Resource 1 — local_file**

```hcl
resource "local_file" "my-pet" {
  content  = "My pet is called Finnegan!"
  filename = "/root/pet-name"
}
```

### **Resource 2 — random_pet**

```hcl
resource "random_pet" "other-pet" {
  length    = 1
  prefix    = "mister"
  separator = "."
}
```

Then ran:

```bash
terraform init
terraform plan
terraform apply
```

✔️ Resources created successfully

---

### **Step 5: Inspect Cloud Provider Config**

Path:

```
/root/terraform-projects/provider/cloud-provider.tf
```

The **instance_type** for `aws_instance` is:

```
t.large
```

✔️ Confirmed

---

### **Step 6: Inspect Kubernetes Namespace Resource**

File:

```
kube.tf
```

Resource name for `kubernetes_namespace`:
✔️ **dev**

---

### **Step 7: Create New File in provider-A Directory**

Path:

```
/root/terraform-projects/provider-A
```

Created:

```
code.tf
```

Content:

```hcl
resource "local_file" "iac_code" {
  filename = "/opt/practice"
  content  = "Setting up infrastructure as code."
}
```

Then:

```bash
terraform init
terraform plan
terraform apply
```

✔️ Resource created successfully

---

### **Step 8: Handle Apply Error (Inconsistent Lock File)**

Running `terraform apply` again produced:

```
Inconsistent dependency lock file
```

This happens when **a new provider resource is added before running terraform init**.

✔️ Fix:

```bash
terraform init
terraform apply
```

✔️ Apply completes successfully

---

## ✅ Key Commands Summary

| Task                  | Command                     |
| --------------------- | --------------------------- |
| Initialize directory  | `terraform init`            |
| Preview changes       | `terraform plan`            |
| Apply changes         | `terraform apply`           |
| Create local_file     | `resource "local_file" ...` |
| Create random_pet     | `resource "random_pet" ...` |
| Fix lock file issues  | `terraform init`            |
| View provider plugins | `ls .terraform/providers`   |

---

## 💡 Notes / Tips

* Terraform only installs providers when they are **referenced** in configuration.
* Adding new provider resources requires **re-running `terraform init`**.
* The `.terraform.lock.hcl` file tracks exact plugin versions used.
* You can mix local, random, AWS, Kubernetes, and many other providers in one directory.
* Provider plugins are stored inside `.terraform/providers`.
* `random_pet` is helpful for generating unique names during labs.

---

## 📌 Lab Summary

| Step                           | Status | Key Takeaways              |
| ------------------------------ | ------ | -------------------------- |
| Check multi-provider directory | ✅      | Starts with 0 providers    |
| Run init → downloads plugins   | ✅      | 2 plugins installed        |
| Create `pet-name.tf` resources | ✅      | local_file + random_pet    |
| AWS instance inspection        | ✅      | instance type = t.large    |
| Kubernetes namespace           | ✅      | name = dev                 |
| Create code.tf in provider-A   | ✅      | local_file resource added  |
| Fix lock-file issue            | ✅      | Must re-run terraform init |
| Final apply                    | ✅      | Everything works           |

---

## ✅ References

* [Terraform Providers Overview](https://developer.hashicorp.com/terraform/language/providers)
* [Random Provider Docs](https://registry.terraform.io/providers/hashicorp/random/latest/docs/resources/pet)
* [Local Provider Docs](https://registry.terraform.io/providers/hashicorp/local/latest/docs/resources/file)
* [Understanding Provider Installation](https://developer.hashicorp.com/terraform/language/providers/configuration)
