CLOUDBOX - BEGINNER SETUP

1. Open the CloudBox folder in VS Code.
2. In VS Code, install the "Live Server" extension (by Ritwick Dey).
3. Create a Firebase project:
   - Go to https://console.firebase.google.com/
   - Click Add project and follow the steps.
4. Register a Web App:
   - In Firebase project overview, click the Web icon (</>).
   - Give it a name such as CloudBox.
   - Register the app and copy the firebaseConfig values.
   - Open js/firebase.js and replace ALL PASTE_YOUR... placeholder values.
   - For storageBucket, copy the exact storageBucket value shown in your Firebase config.
5. Enable Email/Password sign-in:
   - Firebase Console -> Build -> Authentication -> Get started.
   - Sign-in method -> Email/Password -> Enable -> Save.
6. Create a Storage bucket:
   - Firebase Console -> Build -> Storage -> Get started.
   - Follow the setup steps and choose a nearby region.
   - If Firebase asks you to enable billing for your project/region, review the current Firebase pricing and billing requirements before proceeding.
7. Set Storage rules:
   Firebase Console -> Storage -> Rules.
   Replace the rules with the following, then click Publish:

   rules_version = '2';
   service firebase.storage {
     match /b/{bucket}/o {
       match /users/{userId}/{allPaths=**} {
         allow read, write: if request.auth != null
                            && request.auth.uid == userId;
       }
     }
   }

   These rules allow each signed-in user to access only their own users/UID folder.
   Do not use public read/write rules for a real project.

8. Run the website:
   - In VS Code, right-click index.html.
   - Select "Open with Live Server".
   - The browser should open a localhost address.
   - Create an account, log in, and test upload/view/delete.

IMPORTANT:
- Firebase web config is intended to be included in client-side web apps. Security must be enforced with Firebase Security Rules, not by hiding the config.
- This demo does not enforce a storage quota. The dashboard shows the total size of the files it lists; it is not a Firebase billing meter.
- Test with non-sensitive sample files first.
- If you change Storage rules, publish them before testing uploads.
