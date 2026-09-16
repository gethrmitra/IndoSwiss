# Fix GitHub Authentication

## The Problem
You're logged in as `kaushal-kjha` but your repo is `gethrmitra/IndoSwiss`

## Quick Fix - Use Personal Access Token

### Step 1: Create a Personal Access Token
1. Go to: https://github.com/settings/tokens/new
2. Name it: `IndoSwiss-Deploy`
3. Expiration: 90 days (or longer)
4. Select scopes:
   - ✅ `repo` (full control of private repositories)
5. Click "Generate token"
6. **COPY THE TOKEN** (you won't see it again!)

### Step 2: Use Token to Push
Replace `YOUR_TOKEN` with the token you just copied, and run:

```bash
cd C:\Users\Sarath\ Narayan\ecommerce-store

git push https://gethrmitra:YOUR_TOKEN@github.com/gethrmitra/IndoSwiss.git main
```

Example:
```bash
git push https://gethrmitra:ghp_1a2b3c4d5e6f7g8h9i0j@github.com/gethrmitra/IndoSwiss.git main
```

### Step 3: (Optional) Save Credentials
To avoid entering token every time, store it:

```bash
git config --global credential.helper manager
git push -u origin main
```

Then enter username: `gethrmitra` and password: `YOUR_TOKEN` when prompted

---

## Alternative: Switch Git User

If you want to use SSH or have multiple accounts:

```bash
# Check current user
git config user.email
git config user.name

# Set for this repo only
git config user.email "your-gethrmitra-email@gmail.com"
git config user.name "gethrmitra"

# Then try pushing again
git push -u origin main
```

---

## After Pushing

Once files are on GitHub:

1. Go to: https://github.com/gethrmitra/IndoSwiss
2. Settings → Pages
3. Source: `main` branch, `/ (root)` folder
4. Click Save
5. Wait 2-5 minutes

Your store will be at: `https://gethrmitra.github.io/IndoSwiss/`

Let me know once you've created the token and I can help you push!