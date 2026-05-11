# Hotel Landing Page — Project Conversation Log

A full record of the build process for the **Buracaidelaiya / Casobe Beach Resort** landing page and admin dashboard.

---

## May 9 — Storage Options Discussed

**Firebase (original plan):**
- Firestore for booking data
- Firebase Storage for screenshots
- Free tier: 1GB storage, 50k reads/day

---

## May 9 — Admin Password Security

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
