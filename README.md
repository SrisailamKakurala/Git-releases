# **Comprehensive Guide to Git Releases and GitHub Workflows**  

This guide covers **Git releases, tagging, GitHub releases, and how they work with GitHub Actions workflows**. It explains the entire process of creating, managing, and automating releases.  

---

## **🔹 What is a Release in Git?**  
A **release** in Git is a way to mark a specific version of your project at a stable point. It typically involves **tagging a commit** with a version number (e.g., `v1.0.0`).  

### **Why Use Releases?**  
- Keeps track of **stable versions**.  
- Helps in **deployments and rollbacks**.  
- Allows **GitHub Actions workflows** to trigger on releases.  
- Provides an easy way to **share software versions** with users.  

---

## **🔹 Git Tags (How Releases Are Marked)**  
Tags are used to mark a specific commit in Git. There are two types:  
1. **Lightweight Tags** – Just a name pointing to a commit.  
2. **Annotated Tags** – Contains metadata like author, date, and message (Recommended for releases).  

### **Creating a Tag (Releasing a Version)**
To create a new version (`v1.0.0`) in Git:  

```sh
git tag -a v1.0.0 -m "Release version 1.0.0"
```
- `-a` → Creates an **annotated** tag.  
- `v1.0.0` → The version name (you can change it).  
- `-m` → Adds a message to describe the release.  

### **Pushing the Tag to GitHub**
Once tagged, push it to GitHub:  

```sh
git push origin v1.0.0
```

To push all local tags at once:  

```sh
git push --tags
```

---

## **🔹 Managing Tags**
### **Viewing Tags**
To list all tags in a repository:  

```sh
git tag
```

### **Deleting a Tag**
If you created a tag by mistake and need to remove it:  

#### **Delete Locally**
```sh
git tag -d v1.0.0
```

#### **Delete from GitHub**
```sh
git push origin --delete v1.0.0
```

---

## **🔹 Creating a GitHub Release**
After pushing a tag, you can create a **GitHub Release**, which is more than just a tag—it allows you to attach assets, changelogs, and notes.  

### **Steps to Create a Release in GitHub**
1. Go to **Your Repo → Releases → New Release**.  
2. Select an **existing tag** (e.g., `v1.0.0`) or create a new one.  
3. Add a **title and description** (optional).  
4. Upload **binaries, ZIP files, or artifacts** (if needed).  
5. Click **"Publish release"**.  

---

## **🔹 Automating Releases with GitHub CLI**
If you want to create a release without using the GitHub UI, use the **GitHub CLI**:

```sh
gh release create v1.0.0 --title "Version 1.0.0" --notes "This is the first release."
```

---

## **🔹 Automating Releases with GitHub Actions**
GitHub Actions can automatically trigger workflows when a **release is created, updated, or deleted**.

### **Example GitHub Actions Workflow for Releases**
Create a `.github/workflows/release.yml` file in your repository:

```yaml
name: Release Workflow

on:
  release:
    types: [created] # Runs only when a new release is created

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Repository
        uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: 18

      - name: Install Dependencies
        run: npm install

      - name: Run Tests
        run: npm test

      - name: Build Project
        run: npm run build
```

### **Trigger Types in GitHub Releases**
- `created` → Runs when a new release is **published**.  
- `edited` → Runs when a release is **updated**.  
- `deleted` → Runs when a release is **deleted**.  

---

## **🔹 Checking If a Release Exists**
To check if someone has released a version on GitHub:  
1. Go to **GitHub → Your Repo → Releases**.  
2. Look for existing releases and tags.  
3. Use `git tag` to check locally.  

To fetch all remote tags:  

```sh
git fetch --tags
```

---

## **🔹 Manual vs. Automatic Triggers**
GitHub Actions can trigger workflows manually or automatically.  

- **Manual Trigger** (`workflow_dispatch`):  
  - Requires **manual execution** from GitHub UI or API.  
  - Not triggered by webhooks or GitHub events.  

- **Automatic Trigger** (`release` event):  
  - Runs automatically when a release is created/updated/deleted.  
  - Useful for **continuous deployment (CD)** workflows.  

---

## **🔹 Summary**
1. **Tag a version** (`git tag -a v1.0.0 -m "Release v1.0.0"`)  
2. **Push the tag** (`git push origin v1.0.0`)  
3. **Create a GitHub Release** (via UI or CLI)  
4. **Trigger workflows** automatically (`release` event)  
5. **Check releases** in GitHub or with `git tag`  
6. **Automate** release workflows with GitHub Actions  

---

### **🔹 Final Thoughts**
Releases are crucial for **tracking versions, maintaining stability, and deploying software efficiently**. By using **GitHub Actions**, you can automate builds, tests, and deployments whenever a new version is released.  

This guide should serve as your **go-to reference** whenever you need to work with **Git releases** and **GitHub workflows**! 🚀

**Further References:** [Releasing Project On Github](https://docs.github.com/en/repositories/releasing-projects-on-github/about-releases)
