# Chronos — Premium Watch Store Quick Start

Your multi-brand watch ecommerce store is ready. Everything runs in the browser with zero backend needed.

## 🚀 Getting Started

Open these files in your browser:

- **Public Store**: `store.html` — what your customers see
- **Admin Panel**: `admin-master.html` — where you manage everything
- **Home**: `index.html` — links to both

All data syncs instantly between admin and store via `localStorage` (browser storage).

---

## 🎯 What You Can Do

### Admin Panel (`admin-master.html`)

1. **Branding** 
   - Upload a logo (or leave the default ⌚)
   - Set store name, tagline, accent color
   - Changes appear live in the store

2. **Brands**
   - Add/delete watch brands (Rolex, Omega, etc.)
   - Set country of origin
   - Brands appear as tabs in the store

3. **Products**
   - Add watches with name, brand, price, gender (Mens/Womens/Unisex), stock
   - Products filter by brand and gender in the store

4. **Media**
   - Upload showcase photos and videos
   - Drag & drop or click to upload
   - Gallery appears on store home

5. **Company**
   - Edit email, phone, address
   - Write "About Us" text
   - Appears in store footer and info section

6. **Policies**
   - Shipping, Returns, Warranty, Privacy policies
   - Displayed in store policies section

7. **Support**
   - Support email/phone/response time
   - FAQs (add/manage)
   - Shows in footer

### Public Store (`store.html`)

- **Brand tabs**: Filter by brand (all brands appear as tabs)
- **Gender filter**: All / Mens / Womens
- **Search**: Find watches by name
- **Media gallery**: Showcase images/videos
- **Cart counter**: Tracks items (basic demo)
- **Dark/light theme**: Adapts to system preference
- **Responsive**: Works on mobile, tablet, desktop

---

## 🌐 Deploy to GitHub Pages (Free)

### Step 1: Create a GitHub repo
1. Go to [github.com/new](https://github.com/new)
2. Name: `ecommerce-store`
3. Keep it **Public**
4. Create

### Step 2: Push your code
```bash
git remote add origin https://github.com/YOUR_USERNAME/ecommerce-store.git
git branch -M main
git push -u origin main
```

### Step 3: Enable GitHub Pages
1. Go to your repo → **Settings** → **Pages**
2. Branch: `main`, Folder: `/ (root)`
3. Click **Save**
4. Wait 1-2 minutes

Your store is now live at: `https://YOUR_USERNAME.github.io/ecommerce-store/`

---

## 📸 Media Upload Tips

### Option 1: Browser Storage (Default, No Setup Needed)
- Photos/videos stored in your browser
- Works offline
- **Limit**: ~5-10MB per site
- **Caveat**: Only visible on that browser/device

### Option 2: External Hosting (Recommended for Real Store)

**Free Imgur Integration**:
1. Go to [imgur.com/register](https://imgur.com/register)
2. Create account
3. Go to [imgur.com/oauth2/addclient](https://imgur.com/oauth2/addclient)
   - Choose "Anonymous use" (no callback URL)
   - Click "Create Application"
4. Copy your **Client-ID**
5. Open `admin-master.html`, find line ~665:
   ```javascript
   const IMGUR_CLIENT_ID = ''; // ← paste here
   ```
6. Save and reload
7. Now media uploads go to Imgur (permanent links!)

**Other Free Options**:
- **Cloudinary**: cloudinary.com (generous free tier, easy drag-and-drop)
- **GitHub**: Commit images to your repo, link directly (best for small stores)

---

## 💡 Usage Patterns

### Daily Workflow
1. Open admin panel
2. Add/update products, brands, policies as needed
3. Click "View Store" to see changes live
4. Share the store link with customers

### Backup Your Data
All data is in browser `localStorage`. To backup:
```javascript
// In admin panel console:
copy(JSON.stringify({
  brands: localStorage.getItem('brands'),
  products: localStorage.getItem('products'),
  media: JSON.stringify(localStorage.getItem('mediaPhotos')),
  branding: localStorage.getItem('storeName')
}))
```

### Clear Demo Data
```javascript
// In browser console:
localStorage.clear();
location.reload();
```

---

## 🔧 Customization

### Change Theme Color
- Admin panel: Branding → Brand Color
- Or edit CSS variables in the HTML files

### Add More Sections
- Edit `store.html` to add new sections (testimonials, awards, etc.)
- Use same `localStorage` pattern for data

### Connect to Real Payment
Currently cart is demo-only. To add checkout:
- Use **Stripe** (recommended): stripe.com
- Use **PayPal**: paypal.com/developers
- Use **Square**: square.com
- See `STRIPE_SETUP.md` for basic integration

---

## ⚠️ Important Notes

- **Browser Storage Limit**: ~5-10MB per site (enough for ~50 products + media)
- **Per-Browser Data**: Each browser/device gets its own copy (good for demos, not for multi-user inventory)
- **Media Size**: Large videos will hit storage limits; use external hosting
- **Backup Often**: Don't rely on browser storage for production data

---

## 🎓 Next Steps

### For a Real Store
1. ✅ Use external image hosting (Imgur/Cloudinary)
2. ✅ Deploy to GitHub Pages (free)
3. Add real payment processing (Stripe/PayPal)
4. Move data to cloud database (Firebase/Supabase for shared inventory)
5. Add user accounts and order history

### For Learning/Demo
Everything is ready now! Just:
1. Add your brands and products in admin
2. Upload media
3. Share the store.html link

---

## 📞 Support

All code is in `admin-master.html` and `store.html` — they're pure HTML/CSS/JS with no dependencies.

**Troubleshooting**:
- **Changes not showing?** Make sure to click "Save" in admin, then refresh the store
- **Media not uploading?** Check browser console for errors; might be too large
- **Cart not working?** Currently demo-only; see STRIPE_SETUP.md for real checkout

---

**You're all set!** Open `store.html` to see your store, `admin-master.html` to manage it. 🎉