# Deployment Guide

## ✅ What's Fixed

Your watch store now has:

1. **Admin Panel Persistence** ✅
   - All "Save" buttons now actually store data to `localStorage`
   - Company info, policies, support details all persist and sync to the store
   - Changes appear instantly when you visit `store.html`

2. **Media Upload Ready** ✅
   - Photos and videos upload with drag-and-drop or click
   - Default: stored in browser (no setup needed)
   - Ready for Imgur integration (free, no limits)

3. **Live Sync Between Admin & Store** ✅
   - Edit in admin → click "View Store" → see changes live
   - All data flows through `localStorage` (no backend needed)
   - Works offline

4. **Responsive Design** ✅
   - Mobile, tablet, desktop all supported
   - Dark/light theme (follows system preference)
   - Touch-friendly UI

---

## 🚀 Deploy to GitHub Pages (5 Minutes)

### Prerequisites
- GitHub account (free at github.com)
- Git installed on your computer

### Step-by-Step

1. **Create GitHub Repository**
   - Go to github.com/new
   - Name: `ecommerce-store`
   - **Keep it PUBLIC** (required for free pages)
   - Click "Create repository"

2. **Push Your Code**
   ```bash
   cd /path/to/ecommerce-store
   git remote add origin https://github.com/YOUR_USERNAME/ecommerce-store.git
   git branch -M main
   git push -u origin main
   ```
   Replace `YOUR_USERNAME` with your actual GitHub username.

3. **Enable GitHub Pages**
   - Go to your repo on GitHub
   - Settings → Pages
   - Source: `main` branch, `/ (root)` folder
   - Click Save
   - Wait 1-2 minutes

4. **Your Store is Live!**
   ```
   https://YOUR_USERNAME.github.io/ecommerce-store/
   ```

   - Store: `.../store.html`
   - Admin: `.../admin-master.html`
   - Home: `/` (redirects to index.html)

---

## 📸 Media Hosting Options

### Option 1: Browser Storage (Default)
- **Setup**: None needed
- **Storage**: ~5-10MB per browser
- **Limitation**: Only visible on that device/browser
- **Best for**: Demo, testing

### Option 2: Imgur (Recommended)
Free permanent image hosting with no setup complexity.

1. Create account: https://imgur.com/register
2. Create API Client: https://imgur.com/oauth2/addclient
   - Choose "Anonymous use"
   - Get your Client-ID
3. Open `admin-master.html` in a text editor
4. Find this line (around line 670):
   ```javascript
   const IMGUR_CLIENT_ID = ''; // ← paste here
   ```
5. Paste your Client-ID between the quotes
6. Save and reload the admin panel
7. Media uploads now go to Imgur (permanent links!)

### Option 3: Cloudinary
More features, still free tier available.
1. Sign up: https://cloudinary.com
2. Get API key from dashboard
3. Use their web upload widget (advanced)

### Option 4: GitHub Hosting
Commit images directly to your repo.
```bash
# In your repo, create /images folder
mkdir images
# Add your images
git add images/
git commit -m "Add product images"
git push
```
Then link them: `https://raw.githubusercontent.com/YOUR_USERNAME/ecommerce-store/main/images/photo.jpg`

---

## 🔄 Updating Your Store

### After Adding Products/Media
1. Make changes in admin panel
2. Commit to GitHub (optional but recommended):
   ```bash
   git add .
   git commit -m "Update products and media"
   git push
   ```

### If You Want to Backup Data
All data is in your browser's `localStorage`. Backup by copying it:

```javascript
// In browser console (F12):
JSON.stringify({
  brands: JSON.parse(localStorage.getItem('brands')),
  products: JSON.parse(localStorage.getItem('products')),
  branding: {
    name: localStorage.getItem('storeName'),
    tagline: localStorage.getItem('tagline')
  }
})
```

---

## 🔒 Security Notes

- **Admin Panel**: Currently unprotected (anyone with the link can edit)
- **For a Real Store**: Add password protection:
  ```javascript
  // At top of admin-master.html, before any content:
  const adminPassword = prompt('Enter admin password:');
  if (adminPassword !== 'your-secret-password') {
    document.body.innerHTML = '<h1>Access Denied</h1>';
    throw new Error('Unauthorized');
  }
  ```
- **Better**: Use GitHub's authentication (requires backend)

---

## 📊 Storage Limits

| Item | Limit | Notes |
|------|-------|-------|
| localStorage | 5-10MB | Per browser/device |
| Imgur uploads | Unlimited | Permanent links |
| GitHub repo | 1GB | Plenty for files |
| GitHub Pages | 100GB/month | More than enough |

---

## ❌ Troubleshooting

**Changes in admin don't show in store**
- Make sure to click "Save" for each section
- Refresh the store page (F5)
- Check browser console for errors (F12)

**Media won't upload**
- File too large? Try smaller images
- Check console for error messages
- Storage full? Clear browser data (Settings → Clear browsing data)

**Can't push to GitHub**
- Run `git remote -v` to check your remote URL
- Make sure you have push access to the repo
- For auth issues on Windows, use GitHub CLI: `gh auth login`

**Store doesn't load at github.io URL**
- GitHub Pages takes 1-2 minutes to deploy
- Try viewing in an incognito window (cache issue)
- Check your GitHub repo settings > Pages again

---

## 🎯 Next Steps

### Immediate
1. ✅ Deploy to GitHub Pages (follow guide above)
2. ✅ Test by adding a product in admin, see it in store
3. ✅ Share the store link with friends

### Soon
1. Add your watch brands and products
2. Upload showcase photos/videos
3. Write company info and policies
4. Set up Imgur for unlimited media

### Eventually
1. Add real payment processing (Stripe/PayPal)
2. Move to cloud database for shared inventory (Firebase/Supabase)
3. Add user accounts and order history
4. Add admin password protection

---

## 📝 File Structure

```
ecommerce-store/
├── index.html              ← Home page (links to store & admin)
├── store.html              ← Public storefront
├── admin-master.html       ← Admin control panel
├── QUICKSTART.md           ← Getting started guide
├── DEPLOYMENT.md           ← This file
└── .git/                   ← Git repository
```

---

**You're ready to go live!** Questions? Check QUICKSTART.md or look at the code in `store.html` and `admin-master.html` — it's all commented HTML/CSS/JS. 🎉
