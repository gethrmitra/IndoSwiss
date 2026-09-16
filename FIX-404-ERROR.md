# GitHub Pages Deployment Troubleshooting

## Issue: "There isn't a GitHub Pages site here" (404 Error)

### Step 1: Verify Your Repository is Public
1. Go to your GitHub repo: `https://github.com/YOUR_USERNAME/ecommerce-store`
2. Click **Settings** (top right)
3. Scroll down to "Danger Zone"
4. **Visibility** should show "Public"
5. If it says "Private", click "Change to public" and confirm

### Step 2: Enable GitHub Pages
1. In **Settings**, scroll down to **Pages** section (left sidebar)
2. Under "Source":
   - **Branch**: Select `main` (not master)
   - **Folder**: Select `/ (root)`
   - Click **Save**
3. **IMPORTANT**: You should see a blue banner saying "Your site is ready to be published at https://YOUR_USERNAME.github.io/ecommerce-store/"

### Step 3: Wait for Deployment
- GitHub Pages takes 1-5 minutes to deploy
- The page will show a status message while building
- Don't refresh constantly - wait 2-3 minutes, then try again

### Step 4: Check Deployment Status
1. Still in **Settings** → **Pages**
2. Look for the deployment status (should show "Active" with a green checkmark)
3. If it shows "Pending", wait a few more minutes

### Step 5: Test Your Store
Once deployed, try these URLs:

```
https://YOUR_USERNAME.github.io/ecommerce-store/
https://YOUR_USERNAME.github.io/ecommerce-store/index.html
https://YOUR_USERNAME.github.io/ecommerce-store/store.html
https://YOUR_USERNAME.github.io/ecommerce-store/admin-master.html
```

Start with the first one.

---

## Common Issues

**"Still getting 404 after 5 minutes"**
- Double-check your repo is **Public** (not Private)
- Verify the branch is `main` (run `git branch` to check)
- Make sure you pushed all files: `git push -u origin main`
- Clear browser cache (Ctrl+Shift+Delete)

**"Getting different errors"**
- Check the **Pages** section shows "Active" status
- Verify your repo name is exactly `ecommerce-store`
- Try accessing `https://YOUR_USERNAME.github.io/` (without the repo name)

**"Files not showing in repo"**
- Run in terminal:
  ```bash
  cd C:\Users\Sarath\ Narayan\ecommerce-store
  git status
  ```
- Should show "nothing to commit"
- If files are listed, run:
  ```bash
  git add .
  git push origin main
  ```

---

## If Still Not Working

Let me verify your setup. Please tell me:

1. What is your GitHub username?
2. Can you access `https://github.com/YOUR_USERNAME/ecommerce-store` and see your files?
3. In **Settings** → **Pages**, what does the "Source" section show?
4. What does the status message say (green checkmark, orange dot, etc.)?

---

## Alternative: Test Locally First

Before deploying, test your store works locally:

1. Open `C:\Users\Sarath Narayan\ecommerce-store\store.html` in your browser
2. Should see the store with all content
3. Open `admin-master.html` and add a test product
4. Refresh `store.html` - product should appear
5. If this works, your files are correct

Then deployment to GitHub should work once Pages is enabled.

---

## Quick Checklist

- [ ] Repository is **Public** (not Private)
- [ ] Pushed to GitHub: `git push -u origin main`
- [ ] Enabled Pages: Settings → Pages → Source: main branch, / (root)
- [ ] Status shows "Active" with green checkmark
- [ ] Waited 2-5 minutes
- [ ] Tried in incognito window (clear cache)
- [ ] Repo is named `ecommerce-store`