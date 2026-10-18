# Carzzyapps Admin

A static Firebase Authentication and Cloud Firestore admin interface for browsing Carzzy Apps feedback submissions.

## Configure Firebase

1. The dashboard is configured for the same Firebase project as `fdback/firebase-config.js` (`ca-feedback-89122`).
2. In the Firebase console, enable **Authentication → Email/Password** and create accounts. Any signed-in account in this Firebase project can access all feedback submissions; this app does not include public sign-up.
3. The dashboard reads `feedbackSubmissions`. The matching rule in `fdback/firestore.rules` permits reads by any authenticated account and preserves public form submissions without allowing updates or deletes.
4. Serve or deploy this page over HTTP(S), such as with Firebase Hosting. Opening it directly as a `file:` URL does not support the Firebase module imports.

**Access note:** every Firebase Auth account in this project can read all feedback documents, including submitters' names, email addresses, and comments. Keep account creation restricted to people you trust. The page queries the newest 100 documents by `createdAt` and displays the feedback form's `username`, `email`, `rating`, and `reason` fields. Opening a row shows every document field.
