# Git Setup Guide for Quantum AI Bharat OS

## Step-by-Step Instructions to Push to GitHub

### 1. Initialize Git Repository

Open terminal in your project folder and run:

```bash
git init
```

### 2. Add All Files

```bash
git add .
```

### 3. Create First Commit

```bash
git commit -m "Initial commit: Quantum AI Bharat OS - Design & Documentation"
```

### 4. Create GitHub Repository

1. Go to https://github.com
2. Click "New Repository" (+ icon, top right)
3. Repository name: `Quantum-AI-Bharat-OS`
4. Description: "Ethical AI platform for Bharat - Learning, Healthcare, Rural, Business AI with human control"
5. Choose: **Public** (for hackathon submission)
6. **DO NOT** initialize with README (we already have one)
7. Click "Create Repository"

### 5. Connect to GitHub

Copy the commands GitHub shows you, or use these (replace YOUR_USERNAME):

```bash
git remote add origin https://github.com/YOUR_USERNAME/Quantum-AI-Bharat-OS.git
git branch -M main
git push -u origin main
```

### 6. Verify Upload

Go to your GitHub repository URL and verify all files are there:
- README.md
- docs/ folder
- diagrams/ folder
- LICENSE
- etc.

---

## Common Git Commands

### Check Status
```bash
git status
```

### Add Specific Files
```bash
git add filename.md
```

### Commit Changes
```bash
git commit -m "Your commit message"
```

### Push to GitHub
```bash
git push
```

### Pull Latest Changes
```bash
git pull
```

### View Commit History
```bash
git log --oneline
```

---

## For Future Updates

When you make changes:

```bash
# 1. Check what changed
git status

# 2. Add changes
git add .

# 3. Commit with message
git commit -m "Update: description of changes"

# 4. Push to GitHub
git push
```

---

## Troubleshooting

### If you get authentication error:

**Option 1: Use Personal Access Token**
1. Go to GitHub Settings → Developer Settings → Personal Access Tokens
2. Generate new token (classic)
3. Select scopes: `repo` (full control)
4. Copy the token
5. Use token as password when pushing

**Option 2: Use SSH**
1. Generate SSH key: `ssh-keygen -t ed25519 -C "your_email@example.com"`
2. Add to GitHub: Settings → SSH and GPG keys
3. Change remote URL: `git remote set-url origin git@github.com:YOUR_USERNAME/Quantum-AI-Bharat-OS.git`

### If you need to change remote URL:
```bash
git remote set-url origin https://github.com/YOUR_USERNAME/Quantum-AI-Bharat-OS.git
```

### If you accidentally committed sensitive data:
```bash
# Remove file from Git but keep locally
git rm --cached filename

# Commit the removal
git commit -m "Remove sensitive file"

# Push
git push
```

---

## Best Practices

✅ **DO:**
- Write clear commit messages
- Commit frequently with small changes
- Review changes before committing (`git status`, `git diff`)
- Keep sensitive data out of Git (use .gitignore)

❌ **DON'T:**
- Commit API keys or passwords
- Commit large binary files (>100MB)
- Force push to main branch (`git push -f`)
- Commit without testing

---

## GitHub Repository Settings (After Upload)

### 1. Add Topics
Go to repository → About (gear icon) → Add topics:
- `ai`
- `bharat`
- `india`
- `ethical-ai`
- `healthcare`
- `education`
- `agriculture`
- `fairness`
- `hackathon`

### 2. Add Description
"Ethical AI platform for Bharat with Learning, Healthcare, Rural, and Business AI modules under human control"

### 3. Enable Issues
Settings → Features → Check "Issues"

### 4. Add README Badges (Optional)
Add status badges to README.md:
- License badge
- Build status (when you have CI/CD)
- Version badge

---

## For Hackathon Submission

Make sure your repository has:
- ✅ README.md (main documentation)
- ✅ requirements.md (in docs/)
- ✅ design.md (in docs/)
- ✅ Architecture diagrams (in diagrams/)
- ✅ LICENSE file
- ✅ Clear project structure
- ✅ Professional presentation

**Repository URL Format:**
```
https://github.com/YOUR_USERNAME/Quantum-AI-Bharat-OS
```

Submit this URL to the hackathon platform! 🚀

---

**Need Help?**
- Git Documentation: https://git-scm.com/doc
- GitHub Guides: https://guides.github.com
- Git Cheat Sheet: https://education.github.com/git-cheat-sheet-education.pdf
