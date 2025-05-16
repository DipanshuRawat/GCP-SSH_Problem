Here's a clean and ready-to-use **README section** for setting up a passwordless SSH connection to a Google Cloud VM using a custom SSH key with no passphrase:

---

## 🔐 Passwordless SSH Access to GCP VM

This guide explains how to create and use an SSH key **without a passphrase** for seamless access to your Google Cloud VM via `gcloud compute ssh`.

---

### ✅ 1. Generate a New SSH Key Without a Passphrase

Run the following command:

```bash
ssh-keygen -t rsa -f ~/.ssh/gcloud_no_pass -C $USER -N ""
```

* `-f ~/.ssh/gcloud_no_pass` → Save the key with a custom name
* `-N ""` → No passphrase (passwordless login)

---

### ✅ 2. Upload the Public Key to GCP

If **OS Login is enabled** on your project or VM:

```bash
gcloud compute os-login ssh-keys add --key-file ~/.ssh/gcloud_no_pass.pub
```

If **OS Login is disabled**, manually add the key to the VM or project metadata:

```bash
cat ~/.ssh/gcloud_no_pass.pub
```

Then:

* Go to **Google Cloud Console → Compute Engine → Metadata**
* Add the contents under **SSH Keys**

---

### ✅ 3. Connect to the VM Using the Custom Key

Use the following command to SSH into your VM:

```bash
gcloud compute ssh <your-vm-name> \
  --zone <your-zone> \
  --project <your-project-id> \
  --ssh-key-file=~/.ssh/gcloud_no_pass
```

Example:

```bash
gcloud compute ssh merchants-dev-mongodb-tenant-vm-us-ct1-01 \
  --zone us-central1-a \
  --project np-merchants \
  --ssh-key-file=~/.ssh/gcloud_no_pass \
  --tunnel-through-iap
```

---

### ✅ 4. (Optional) Set Up SSH Config for Convenience

Add this to your `~/.ssh/config` to simplify future logins:

```ssh
Host my-gcp-vm
    HostName <EXTERNAL_IP or VM_HOSTNAME>
    User <your-username>
    IdentityFile ~/.ssh/gcloud_no_pass
```

Then just run:

```bash
ssh my-gcp-vm
```

---

Let me know if you want this in `.md` format or added to a GitHub repo structure!
