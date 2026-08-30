SEOUL 2027 SHARED FAMILY EDITION — SETUP

This version uses Firebase Authentication + Cloud Firestore for secure real-time collaboration.
Each collaborator signs in with Google. The Owner invites their Google email as Editor or Viewer.

A. CREATE FIREBASE PROJECT
1. Go to Firebase Console and create a project (for example: seoul-2027-family).
2. Project settings > Your apps > Add app > Web.
3. Copy the firebaseConfig values into firebase-config.js.

B. ENABLE GOOGLE SIGN-IN
1. Firebase Console > Authentication > Sign-in method.
2. Enable Google.
3. Authentication > Settings > Authorized domains.
4. Add: chung74hk.github.io

C. CREATE FIRESTORE
1. Firebase Console > Firestore Database > Create database.
2. Choose a suitable region.
3. Open Firestore > Rules.
4. Replace the rules with the contents of firestore.rules and Publish.

D. DEPLOY TO GITHUB PAGES
Upload these files to the root of your seoul-2027 GitHub repository:
- index.html
- firebase-config.js
- manifest.webmanifest
- service-worker.js
- icon-192.png
- icon-512.png

E. USE THE APP
1. Open your GitHub Pages site and sign in with Google.
2. Press “Create Seoul 2027 Trip”.
3. Open “Manage Sharing”.
4. Add family/friends by their Google email as Editor or Viewer.
5. Press “Copy Share Link” and send that same link to them.
6. They sign in with the exact invited Google email.

ROLE RULES
- Owner: edit trip, invite/remove collaborators, change roles.
- Editor: edit, add, delete and reorder trip stops and shared checklists.
- Viewer: read only.

NOTE
The Firebase web config is not a password. Access control is enforced by Firebase Authentication and Firestore Security Rules. Do not weaken the included Firestore rules to “allow read, write: if true”.

IMPORTANT - Firebase setup before signing in
1. Firebase Console -> Authentication -> Sign-in method -> Google -> Enable -> Save.
2. Firebase Console -> Authentication -> Settings -> Authorized domains -> add: chung74hk.github.io
3. Firebase Console -> Firestore Database -> Rules -> paste firestore.rules -> Publish.
4. Upload these files to the root of the GitHub Pages repository, replacing the old version.
5. Wait for GitHub Pages to deploy, then open https://chung74hk.github.io/seoul-2027/ in a normal browser tab.
6. If an old PWA version is still shown, close/delete the old home-screen shortcut and open the site in Safari/Chrome again once so the v2 cache is installed.
7. Press Sign in with Google. After signing in, Create Seoul 2027 Trip.
