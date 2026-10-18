# Carzzyapps Admin

A static Firebase Authentication and Cloud Firestore admin interface for browsing Carzzy Apps feedback submissions.

## Configure Firebase

1. The dashboard is configured for the same Firebase project as `fdback/firebase-config.js` (`ca-feedback-89122`).
2. In the Firebase console, enable **Authentication → Email/Password** and create accounts. Any signed-in account in this Firebase project can access all feedback submissions; this app does not include public sign-up.
3. The dashboard includes **Feedback** (`feedbackSubmissions`) and **Contact Form** (`contactSubmissions`) tabs. Both collections need read access for signed-in users in Firestore rules. The local rules files preserve submission validation and deny updates/deletes; they are excluded from Git for safety and must be deployed to Firebase separately.
4. Use **Add more tabs** to connect another top-level Firestore collection. Custom tabs are saved only in the current browser and the collection must permit reads for signed-in users.
5. Serve or deploy this page over HTTP(S), such as with Firebase Hosting. Opening it directly as a `file:` URL does not support the Firebase module imports.

**Access note:** every Firebase Auth account in this project can read feedback and contact form documents, including submitters' names, email addresses, ratings, and messages. Keep account creation restricted to people you trust. The built-in tabs query the newest 100 documents by `createdAt`; opening a row shows every document field.
