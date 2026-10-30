# Carzzyapps Admin

A static Firebase Authentication and Cloud Firestore workspace for browsing Carzzy Apps submissions, managing launch countdowns, and correcting Crema Rush best scores.

## Configure Firebase

1. The dashboard is configured for the same Firebase project as `fdback/firebase-config.js` (`ca-feedback-89122`).
2. In the Firebase console, enable **Authentication → Email/Password** and create accounts. Any signed-in account in this Firebase project can access all feedback submissions and manage countdowns; this app does not include public sign-up.
3. The dashboard includes **Feedback** (`feedbackSubmissions`) and **Contact Form** (`contactSubmissions`) tabs. Both collections need read access for signed-in users in Firestore rules. The site's and admin workspace's rules files preserve submission validation and deny updates/deletes; deploy the appropriate rules file to Firebase separately after reviewing it.
4. The **Launch countdowns** workspace stores events in `launchCountdowns`. Deploy the updated local `firestore.rules` to the same Firebase project: signed-in users can create, edit, publish, and delete countdowns; visitors can read published countdowns only. The public listing and shareable event pages are at `https://www.carzzyapps.com/launch-countdowns/`.
   Add an optional HTTP(S) destination link to a countdown; its public page shows a **Now Live** button linking to that destination after the countdown ends.
5. Use **Add more tabs** to connect another top-level Firestore collection. Custom tabs are saved only in the current browser and the collection must permit reads for signed-in users.
6. To edit Crema Rush scores, enable Email/Password sign-in in the `carzzy-apps` Firebase project and create a trusted admin account there. In that project's Firestore database, create `adminUsers/{UID}` with the signed-in account's UID as the document ID and `{ "enabled": true }` as its data. Deploy the root repository's `firestore.rules` to `carzzy-apps`. In the admin workspace, open **Crema Rush scores** and connect with that game-project account. The editor can change a player's best shift score; player accounts cannot edit other players' scores. Admin edits are revisioned so a lower corrected score is not restored from the player's old local save.
7. Add the admin site's hosting domain (for example, `admin.carzzyapps.com`) to **Authentication → Settings → Authorized domains** in the `carzzy-apps` project. The score editor signs in separately from the admin workspace because the two sites use different Firebase projects.
8. Serve or deploy this page over HTTP(S), such as with Firebase Hosting. Opening it directly as a `file:` URL does not support the Firebase module imports.

**Access note:** every Firebase Auth account in this project can read feedback and contact form documents, including submitters' names, email addresses, ratings, and messages, and can manage countdowns. Keep account creation restricted to people you trust. The Feedback tab queries the newest 100 documents by `createdAt`. The Contact Form tab reads up to 100 documents and sorts them by any recognized submission date; this also includes records that do not have a `createdAt` field. Opening a row shows every document field.

The Crema Rush score editor connects to the separate `carzzy-apps` project. Its score read/write access is restricted by the `adminUsers` allowlist in that project's Firestore rules; signed-in players can only edit their own game progress.
