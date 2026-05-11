# Hotel Landing Page — Project Conversation Log

A full record of the build process for the **Buracaidelaiya / Casobe Beach Resort** landing page and admin dashboard.

---

## May 8 — Initial Build

**Request:** Build a hotel landing page with a full-screen video background and room cards.

**Rooms included:**
- Cocoon Pod
- Luxury Couples Room
- Apollo Aeropod
- Cupola
- Beachfront Cabin
- Beach Suite
- Private Villa
- Luxury Pool Villas

**Delivered:** Full-screen video hero, room cards with pricing, booking CTA, fade-in animations.

---

## May 8 — Gallery & Design Overhaul

**Request:** Remove Google Drive links from cards. Instead, clicking "View Photos" should open a lightbox gallery. Also, the design looked too "AI-built."

**Changes made:**
- Removed all Google Drive links
- Built a proper lightbox gallery (keyboard navigation: ← →, Esc to close)
- Redesigned to feel agency-built, not template-like
- Image structure: `rooms/cocoon-pod/1.jpg`, `rooms/luxury-couples/1.jpg`, etc.
- First image in each room array auto-used as the card thumbnail

---

## May 8 — Portrait Video Background Fix

**Request:** The background video is portrait orientation. The site must be responsive for both landscape and portrait views.

**Fix applied:**
```css
.hero video {
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: center center;
}

@media (orientation: landscape) {
  .hero video { object-position: center 30%; }
}
```

---

## May 8 — Video Not Showing (Troubleshooting)

**Problem:** Video wasn't rendering in the browser.

**Root causes diagnosed and resolved:**
1. Browser blocks `file://` video autoplay → Fixed by using VS Code **Go Live** (Live Server)
2. File path mismatch (`hotel/bgvideo.mp4` vs `bgvideo.mp4`) → Fixed to match Live Server root
3. Codec issue: phone recorded in **H.265/HEVC** which Chrome doesn't support → Needed H.264 conversion

**FFmpeg conversion command:**
```bash
ffmpeg -i bgvideo.mp4 -vcodec libx264 -crf 23 -acodec aac -movflags faststart bgvideo_h264.mp4
```

**FFmpeg install (Windows):**
```bash
winget install ffmpeg
```

**If PATH not updated yet, use full path:**
```bash
"C:\Users\USER\AppData\Local\Microsoft\WinGet\Packages\Gyan.FFmpeg_Microsoft.Winget.Source_8wekyb3d8bbwe\ffmpeg-8.1.1-full_build\bin\ffmpeg.exe" -i bgvideo.mp4 -vcodec libx264 -crf 23 -acodec aac -movflags faststart bgvideo_h264.mp4
```

---

## May 8 — Lightbox Fix (Mobile)

**Problem:** Lightbox header overlapping the image on mobile; thumbnails too small.

**Fix:** Rebuilt lightbox layout:

```
┌─────────────────────────────┐  ← 60px fixed header (room name + close)
│                             │
│        IMAGE FILLS          │  ← flex: 1, takes all remaining height
│        THIS SPACE           │
│                             │
├─────────────────────────────┤  ← 80px fixed thumbnail strip
│ 🖼 🖼 🖼 🖼 🖼 🖼 → scroll  │
└─────────────────────────────┘
```

- Mobile: thumbnails horizontal scroll at bottom (72×54px)
- Desktop (900px+): thumbnails in 140px right sidebar, scrolling vertically

---

## May 9 — WhatsApp CTAs & Promotions Popup

**Request:** "Book a Stay" → WhatsApp link. Add a promotions popup like the screenshot, with room cards and WhatsApp CTAs.

**Changes made:**
- "Book a Stay" button → WhatsApp deep link (`wa.me/63XXXXXXXXXX`)
- Floating button (bottom-right): "View Rooms & Rates" opens promotions popup
- Green WhatsApp floating button
- Promotions popup: all rooms with thumbnail, price, and pre-filled WhatsApp message per room

**To update WhatsApp number later:** Find `63XXXXXXXXXX` (appears 3 times) and replace with actual number e.g. `639171234567`.

---

## May 9 — "Book a Stay" Nav Button Fix

**Request:** "Book a Stay" in the navbar should open a booking modal, not go to WhatsApp.

**Room picker modal added:**
- Clicking "Book a Stay" opens a popup listing all 9 rooms with thumbnails, pax, and pricing
- User selects a room → enters the 3-step booking flow (dates → guest details → payment screenshot)

**CSS fix for button (was anchor, changed to button element):**
```css
.nav-book {
  background: none;
  cursor: pointer;
  font-family: 'DM Sans', sans-serif;
}
```

---

## May 9 — Admin Dashboard

**Client request:** Add an admin dashboard with:

**User flow:**
1. Pick a room
2. Check available dates (against existing bookings)
3. Fill in guest details
4. Upload payment screenshot
5. Booking saved as "pending"

**Admin flow (`admin.html`):**
- Login screen
- Stats bar: total / pending / approved / declined / refunded
- Full bookings table with filters
- Click any booking → view details + payment screenshot (zoomable)
- Approve / Decline / Refund buttons

---

## May 9 — Storage Options Discussed

**Firebase (original plan):**
- Firestore for booking data
- Firebase Storage for screenshots
- Free tier: 1GB storage, 50k reads/day

**Alternatives considered:**

| Option | Database | Storage | Free Tier |
|--------|----------|---------|-----------|
| Supabase | Supabase DB | Supabase Storage | 500MB DB, 1GB storage |
| Cloudinary + Supabase | Supabase DB | Cloudinary | Both generous |
| Firebase | Firestore | Firebase Storage | 1GB storage, 50k reads/day |
| PocketBase | PocketBase | PocketBase | Unlimited (self-hosted) |

**Recommended:** Supabase — one platform, clean dashboard, plain SQL.

---

## May 9 — Admin Password Security

**Problem:** Hardcoded password visible in browser source via F12.

**Solution:** Firebase Authentication

**Why `.env` doesn't work here:**
`.env` only works server-side. This is a pure HTML/JS site — everything sent to the browser is visible regardless of `.env`.

**Fix applied:** Replaced hardcoded password with Firebase Auth (email + password). Password never appears in code.

**Setup steps:**
1. Go to Firebase Console → Authentication → Get started
2. Enable **Email/Password** provider
3. Click **Add user** → set `admin@casoberesort.com` + strong password
4. Use those credentials to log into `admin.html`

**Firestore Security Rule** (paste into Firestore → Rules):
```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /bookings/{id} {
      allow read, write: if request.auth != null;
    }
  }
}
```

---

## Firebase Setup (Quick Reference)

1. Go to [console.firebase.google.com](https://console.firebase.google.com) → Create project → name it `casobe-resort`
2. Add a **Web app** → copy the config object
3. Enable **Firestore Database** → start in test mode
4. Enable **Storage** → start in test mode
5. Paste config into both `index.html` and `admin.html` where placeholders say `YOUR_API_KEY` etc.

---

## File Structure

```
hotel/
├── index.html              ← Main landing page
├── admin.html              ← Admin dashboard
├── bgvideo_h264.mp4        ← Background video (H.264)
└── rooms/
    ├── cocoon-pod/
    │   ├── 1.jpg
    │   ├── 2.jpg
    │   └── ...
    ├── luxury-couples/
    ├── apollo-aeropod/
    ├── cupola/
    ├── beachfront-cabin/
    ├── beach-suite/
    ├── private-villa/
    └── luxury-pool-villas/
```

---

## Pending / To-Do

- [ ] Get client's WhatsApp number → replace `63XXXXXXXXXX` in `index.html` (3 occurrences)
- [ ] Get client's Firebase config → paste into `index.html` and `admin.html`
- [ ] Create admin Firebase Auth user (email + password)
- [ ] Add Firestore security rules
- [ ] Add room images into `rooms/` subfolders
- [ ] Replace `bgvideo.mp4` with converted `bgvideo_h264.mp4`
- [ ] Test booking flow end-to-end
- [ ] Test admin approve / decline / refund flow
