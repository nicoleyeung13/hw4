# One Question For The Next Person

A quiet, pass-it-on digital journal designed for a small group of friends. Each person answers the latest waiting question and writes a new prompt for whoever comes next.

---

## 📖 How It Works

1. **Current Question Prominently Displayed:**
   - The site always shows the latest unanswered question.
   - Initial starting question:
     > *"What is something you regret doing today?"*
2. **Three-Field Turn Submission:**
   - **Nickname**: Who is writing.
   - **Your answer**: Response to the current question.
   - **A question for the next person**: The prompt that will become the next question.
3. **“Pass it on” Action:**
   - Validates that all three fields are filled in.
   - Saves the turn record to **Google Cloud Firestore**.
   - Associates the entry with the writer's Google profile (avatar and name) if signed in.
   - Clears the form only after the save succeeds.
   - Automatically synchronizes across all connected devices in real time without refreshing.
4. **Google Authentication Interface:**
   - Quiet, minimal pill located in the header.
   - Allows users to sign in with their Google account via a single click popup.
   - Pre-fills the "Nickname" field with their Google display name.
   - Displays their Google profile photo beside their name on past journal entries.
5. **Journal History:**
   - Lets visitors browse past entries one at a time using **Previous** and **Next** buttons.
   - Displays the original question answered, the response, writer's nickname (and avatar), and formatted date/time.
   - The active current question stays pinned and visible above while paging through history.

---

## 📁 File Structure

```text
hw4/
├── index.html   # Semantic HTML5 layout with Firebase SDKs loaded
├── style.css    # Warm cream paper aesthetic, serif typography, responsive design
├── app.js       # Turn logic, validation, and real-time Cloud Firestore integration
└── README.md    # Documentation and Firebase setup reference
```

---

## 🚀 How to Run the Website

### Running locally:
```bash
python3 -m http.server 8000
```
Open [http://localhost:8000](http://localhost:8000) in your browser.

---

## 🔥 Firebase Security Rules (Authentication Required)

### For Cloud Firestore (Used in this project)
1. Go to **[Firebase Console](https://console.firebase.google.com/)** > **`question-for-the-next-person`**.
2. In the left menu, click **Firestore Database** > **Rules** tab.
3. Paste the following rules:

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /entries/{entryId} {
      // Must be authenticated with Google to read and submit entries
      allow read, write: if request.auth != null;
    }
  }
}
```

> **Optional Variation (Public reading, authenticated posting):**
> If you want anyone with the link to read the journal, but require signing in with Google to post an answer:
> ```javascript
> allow read: if true;
> allow create: if request.auth != null;
> ```

4. Click **Publish**.

---

### For Realtime Database (If using Realtime DB instead)
If using Firebase Realtime Database instead of Cloud Firestore:
```json
{
  "rules": {
    "entries": {
      ".read": "auth != null",
      ".write": "auth != null"
    }
  }
}
```

---

## 💾 Data Record Format in Firestore

Each turn document is stored in the `entries` collection with the following JSON schema:

```json
{
  "name": "Maya",
  "question": "What is something you regret doing today?",
  "answer": "Not getting outside while the sun was still out.",
  "nextQuestion": "What song makes you feel like you're floating?",
  "timestamp": "2026-11-23T22:20:00.000Z"
}
```
