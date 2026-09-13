# WeParty

**WeParty** is an Android party-planning app that helps hosts and guests organize events in one place — from invites and RSVPs to shared shopping lists, real-time group chat, and push notifications.

Built as a collaborative team project with **Kotlin**, **Jetpack Compose**, and **Firebase**.

---

## Overview

WeParty turns party planning into a shared workflow instead of scattered group texts:

- **Hosts** create events, invite friends, manage item lists, and track attendance
- **Guests** RSVP, claim what they’re bringing, chat with the group, and get notified when something changes
- **Everyone** stays synced through a real-time event inbox, in-app alerts, and Firebase Cloud Messaging push notifications

---

## Core Features

| Area | What it does |
|------|----------------|
| **Auth & onboarding** | Email/password + Google Sign-In, email verification, biometric login, password recovery, onboarding walkthrough |
| **Event lifecycle** | Create, edit, and delete events; invite guests; deep-link invites via FlowLinks |
| **Home & calendar** | Event cards, date filtering, calendar view |
| **Event info & RSVP** | Event details, attendance (going / maybe / declined), claimed vs unclaimed items |
| **Event dashboard** | Chronological event inbox, per-event chat room, shared item checklist, photo gallery |
| **Shopping** | Per-event checklists + consolidated shopping list with optional Walmart price lookup |
| **Social** | Friends system, friend requests, in-app event invites |
| **Notifications** | In-app notification center + FCM push for messages, claimed items, invites, and reminders |
| **Profile** | User profile, dietary preferences, host management, account deletion |

---

## Tech Stack

- **Language / UI:** Kotlin, Jetpack Compose, Material 3
- **Architecture:** Activity-based screens + shared `EventViewModel` (LiveData / StateFlow)
- **Backend:** Firebase Auth, Cloud Firestore, Firebase Storage, Firebase Cloud Messaging
- **Cloud:** Firebase Cloud Functions (notification delivery + scheduled reminders)
- **Networking:** Retrofit (Walmart price API)
- **Other:** Coil (images), Google Play Services Auth, AndroidX Biometric, deep linking (FlowLinks)

---

## Project Structure

```
WePartyApp/
├── app/src/main/java/com/example/wepartyapp/
│   ├── ui/
│   │   ├── auth/                 # Login, signup, splash, password recovery
│   │   ├── onboarding/           # First-run walkthrough
│   │   ├── home/                 # Main shell, notifications, FCM service
│   │   ├── create_event/         # Multi-step create-event flow
│   │   ├── event_dashboard/      # Event inbox, chat, checklist, event info
│   │   ├── calendar/             # Calendar screen
│   │   ├── profile/              # Profile & dietary preferences
│   │   ├── api/                  # Retrofit / Walmart price models
│   │   ├── EventViewModel.kt     # Shared Firebase-backed app state
│   │   └── theme/                # Compose theme & typography
│   └── utils/                    # Shared activity utilities (e.g. inactivity timeout)
├── functions/                    # Firebase Cloud Functions (Node.js)
└── ...
```

---

## My Contributions — Andy Tran

I owned the **event dashboard / messaging experience**: the real-time chat system, shared item checklist inside events, unread indicators, and the notification hooks that keep guests in sync.

### Event dashboard & inbox
- Built the **Event Dashboard UI** and chronological **event inbox** so users can browse upcoming parties they’re hosting or invited to
- Implemented **`ChatRoomActivity`** navigation and fixed the AndroidManifest registration crash that blocked entering chat rooms
- Polished dashboard UI (including theming the event info summary to match the app’s pink palette)

### Persistent real-time chat
- Implemented the **persistent per-event chat system** backed by Firestore (`events/{eventId}/messages`)
- Wired chat to the shared ViewModel (`listenToMessages`, `sendMessage`) with chronological message ordering
- Synced **last-message previews** onto event documents so the inbox shows recent activity at a glance

### Shared item checklist
- Built the **in-event item checklist** inside the messaging screen (`ChatFeedContent`)
- Users can **add items** and **claim / unclaim** what they’re bringing, with ownership displayed by name
- Connected checklist actions to Firestore updates via `toggleItemCheck` and `addItemToExistingEvent`

### Unread message indicators
- Added **unread badges** on chat rooms in the event dashboard
- Tracked read state with `readByUsers` on event documents
- Fixed the bug where indicators appeared correctly but **didn’t clear after messages were read** (`markEventAsRead`)

### Notifications for chat & checklist
- Extended notification logic so guests get **in-app alerts and push notifications** when:
  - someone sends a new event message
  - someone claims an item on the shared checklist
- Integrated with the app’s notification pipeline (`sendAppNotification` → Firestore `notifications` → Cloud Functions → FCM)

### Key files I worked in
- `ui/event_dashboard/EventDashboardActivity.kt` — inbox + `ChatRoomActivity`
- `ui/event_dashboard/ChatFeedContent.kt` — chat UI, checklist, view switching
- `ui/EventViewModel.kt` — messaging, checklist, unread, notification triggers
- `AndroidManifest.xml` — activity registration for chat navigation

---

## Team

Collaborative group project. High-level ownership across the team:

| Member | Focus areas |
|--------|-------------|
| **Andy Tran** | Event dashboard, real-time chat, checklist, unread indicators, message/checklist notifications |
| **Rodney** | Auth (Google Sign-In, biometrics, account deletion), friends/invites, FCM & Cloud Functions, deep links, photo gallery |
| **Lesly Morales** | Create/edit event flow, consolidated shopping list UI, polish & cleanup |
| **Maret Merced** | Event info screen, attendance/RSVP, home date filters, event deletion, auth handle fixes |
| **Hasani** | Login/signup cleanup, password recovery |

---

## Getting Started

### Prerequisites
- Android Studio (recent stable)
- JDK 11+
- A Firebase project with Auth, Firestore, Storage, and Cloud Messaging enabled
- `google-services.json` placed under `WePartyApp/app/` (not committed for security)

### Run the app
```bash
cd WePartyApp
./gradlew :app:assembleDebug
```

Or open the `WePartyApp` folder in Android Studio and run the `app` configuration on an emulator or device (min SDK 26).

### Optional: Cloud Functions
```bash
cd WePartyApp/functions
npm install
# Deploy from the Firebase CLI when credentials are configured
```

---

## Resume / Portfolio Blurb

> Built **WeParty**, a Kotlin/Jetpack Compose Android app for collaborative party planning with Firebase. Owned the event dashboard experience — real-time Firestore chat, shared item checklists with claim ownership, unread indicators, and push/in-app notifications for messages and checklist updates.

---

## License

Class / academic group project. Not published as a commercial product.
