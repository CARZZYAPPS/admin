# Carzzyapps Admin

A static Firebase Authentication and Cloud Firestore workspace for browsing Carzzy Apps submissions and managing launch countdowns.

## Configure Firebase

1. The dashboard is configured for the same Firebase project as `fdback/firebase-config.js` (`ca-feedback-89122`).
2. In the Firebase console, enable **Authentication → Email/Password** and create accounts. Any signed-in account in this Firebase project can access all feedback submissions and manage countdowns; this app does not include public sign-up.
3. The dashboard includes **Feedback** (`feedbackSubmissions`) and **Contact Form** (`contactSubmissions`) tabs. Both collections need read access for signed-in users in Firestore rules. The site's and admin workspace's rules files preserve submission validation and deny updates/deletes; deploy the appropriate rules file to Firebase separately after reviewing it.
4. The **Launch countdowns** workspace stores events in `launchCountdowns`. Deploy the updated local `firestore.rules` to the same Firebase project: signed-in users can create, edit, publish, and delete countdowns; visitors can read published countdowns only. The public listing and shareable event pages are at `https://www.carzzyapps.com/launch-countdowns/`.
   Add an optional HTTP(S) destination link to a countdown; its public page shows a **Now Live** button linking to that destination after the countdown ends.
5. Use **Add more tabs** to connect another top-level Firestore collection. Custom tabs are saved only in the current browser and the collection must permit reads for signed-in users.
6. Serve or deploy this page over HTTP(S), such as with Firebase Hosting. Opening it directly as a `file:` URL does not support the Firebase module imports.

**Access note:** every Firebase Auth account in this project can read feedback and contact form documents, including submitters' names, email addresses, ratings, and messages, and can manage countdowns. Keep account creation restricted to people you trust. The Feedback tab queries the newest 100 documents by `createdAt`. The Contact Form tab reads up to 100 documents and sorts them by any recognized submission date; this also includes records that do not have a `createdAt` field. Opening a row shows every document field.
