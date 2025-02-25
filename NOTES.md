# 🚀 Fixing GitHub Authentication for `git push`

If you're seeing an error when trying to push to GitHub with `git push origin master` like:

```plaintext
remote: Support for password authentication was removed on August 13, 2021.
remote: Please see https://docs.github.com/get-started/getting-started-with-git/about-remote-repositories
fatal: Authentication failed
```

GitHub no longer allows password authentication for Git operations over HTTPS. You need to use either **Personal Access Tokens (PAT)** or **SSH Authentication**.

---

## ✅ **Solution 1: Use a Personal Access Token (PAT)**

### **Step 1: Generate a PAT**

1. Go to **GitHub → Settings → Developer Settings → Personal Access Tokens** ([Direct Link](https://github.com/settings/tokens)).
2. Click **"Generate new token (classic)"**.
3. Select the following scopes:
   - ✅ `repo` (for repository access)
   - ✅ `workflow` (if needed for CI/CD)
4. Click **Generate token** and copy it (you won’t see it again!).

### **Step 2: Use the Token Instead of a Password**

When Git prompts for a password, enter your **Personal Access Token** instead.

Alternatively, set it permanently:

```bash
git remote set-url origin https://USERNAME:TOKEN@github.com/USERNAME/REPOSITORY.git
```

---

## ✅ **Solution 2: Use SSH Authentication (Recommended)**

### **Step 1: Check for Existing SSH Keys**

```bash
ls -al ~/.ssh
```

If you see files like `id_rsa.pub` or `id_ed25519.pub`, you already have an SSH key.

### **Step 2: Generate a New SSH Key (if needed)**

```bash
ssh-keygen -t ed25519 -C "your-email@example.com"
```

Press **Enter** to accept the default location and file name.

### **Step 3: Add Your SSH Key to GitHub**

1. Copy your SSH key:
   ```bash
   cat ~/.ssh/id_ed25519.pub
   ```
2. Go to **GitHub → Settings → SSH and GPG keys** ([Direct Link](https://github.com/settings/keys)).
3. Click **"New SSH key"**, paste the key, and save.

### **Step 4: Test SSH Connection**

```bash
ssh -T git@github.com
```

Expected output:

```plaintext
Hi USERNAME! You've successfully authenticated.
```

### **Step 5: Update Your Remote URL**

```bash
git remote set-url origin git@github.com:USERNAME/REPOSITORY.git
```

Now you can push **without authentication prompts**.
asd

---

## ✅ **Solution 3: Use Git Credential Helper**

If you prefer to use HTTPS and want Git to remember your credentials:

```bash
git config --global credential.helper store
```

The first time you enter your credentials (using a **Personal Access Token**), Git will save them for future use.

---

## 🎯 **Recommended Approach**

- **For security:** Use **SSH Authentication**.
- **For quick fixes:** Use **Personal Access Tokens (PAT)**.
- **For convenience:** Use **Git Credential Helper** with PAT.

🚀 Now you're all set to push to GitHub without authentication issues! Happy coding!
