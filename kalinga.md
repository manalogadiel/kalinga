# Kalinga — App Plan Documentation

## 1. Overview
**App Name:** Kalinga
**Platform:** Flutter (mobile)
**One-liner:** One shared timeline for the whole family's day — tasks, schedules, and reminders for every family member, in one place.

## 2. Problem Statement
Families coordinate through scattered channels — a school app for the kid's schedule, sticky notes for a parent's meds, group texts for pickups. There's no single view of "what's happening today, for everyone," and no easy way to assign tasks and confirm they got done.

## 3. Solution
Kalinga merges every family member's day into one color-coded timeline. Everyone can add their own schedule items, parents can assign tasks to specific family members, and a daily overview shows who's done what today — all stored locally, viewable offline.

## 4. Target User
Families with multiple members to coordinate — kids, parents, grandparents — who currently rely on disconnected calendars, apps, or verbal reminders.

## 5. Full Vision Feature Set
*(the complete product concept — not all built in the hackathon; see Section 6 for what's actually built)*

| Feature | Description |
|---------|-------------|
| Merged Timeline | Everyone's day, color-coded by person, sorted by time |
| Self-Scheduling | Each family member can add their own calendar items, visible to the whole family |
| **Task Assignment** | Parents (or anyone) can assign a task to a specific family member (e.g. "Take meds at 5pm") |
| Mark as Done | Recipient checks off completed tasks |
| Reminders | Time-based local notifications for assigned tasks |
| Photo Proof | Attach a photo when completing a task, for fun/accountability |
| Daily Overview / Offline Analyzer | At-a-glance summary of completed vs. pending tasks per person, computed locally — works with no internet connection |
| Family Notes | Simple shared message board per person (lightweight, non-real-time "chat") |

## 6. Hackathon Build Scope (1 hour)
Given a 1-hour build window, the demo build focuses on the features that best prove the concept live:

| # | Feature | Built? | Notes |
|---|---------|--------|-------|
| 1 | Merged Timeline | ✅ Built | Hardcoded/seed data, 3–4 family members, color-coded, sorted by time |
| 2 | Role Switcher | ✅ Built | Simple dropdown filtering the same dataset |
| 3 | **Task Assignment** | ✅ Built (as seed data) | Tasks in the seed data are pre-assigned (e.g. "Assigned by Mom") and labeled as such in the UI, so the concept is visibly demoed even without a live "Add Task" form |
| 4 | Mark as Done | ✅ Built | Checkbox, local state |
| 5 | Daily Overview | ✅ Built | Per-person completed/pending count, computed from the same dataset |
| 6 | Self-Scheduling (Add Entry form) | 🗒️ Talked about, not built | Explained as part of the vision; seed data already implies it |
| 7 | Reminders (notifications) | 🗒️ Talked about, not built | Needs platform setup — too risky for 1 hour |
| 8 | Photo Proof | 🗒️ Talked about, not built | Needs image picker + permissions setup |
| 9 | Family Notes | 🗒️ Talked about, not built | Lowest priority, cut first |

**Key decision:** Task assignment stays visible in the demo by baking it into the seed data (e.g., an entry clearly labeled "Take meds — assigned by Mom" with an "Assigned" tag/icon), rather than building a live assignment flow. This proves the concept on-screen without spending build time on form UI.

## 7. Data Model

```dart
class Person {
  String id;
  String name;
  String colorHex;
  String role; // "Parent" | "Kid" | "Grandparent"
}

class TimelineEntry {
  String id;
  String personId;        // who it's for
  String? assignedById;   // who assigned it — null if self-scheduled
  String title;
  String category;        // "appointment" | "medication" | "pickup" | "task" | "other"
  DateTime time;
  bool isCompleted;
  String? proofImagePath; // vision-only, not built in 1-hour version
}
```

## 8. Screens (1-hour build)
1. **Timeline (Home)** — merged list, color-coded, sorted by time, "Assigned by X" tag on task entries
2. **Role Switcher** — dropdown/selector filtering the timeline by person
3. **Daily Overview** — summary card/header showing completed vs. pending counts per person

## 9. Suggested Packages
- State management: `provider` or plain `setState` (fastest for 1 hour)
- Date/time formatting: `intl`
- *(Vision-only, not needed for 1-hour build): `hive`, `flutter_local_notifications`, `image_picker`*

## 10. 1-Hour Build Timeline
| Time | Task |
|------|------|
| 0–10 min | Data model + 8–10 seed entries across 3 people, including 2–3 pre-assigned tasks |
| 10–40 min | Timeline screen: list view, color-coded, sorted by time, "Assigned by X" tag |
| 40–50 min | Role switcher + mark-as-done checkbox |
| 50–60 min | Daily overview counter at top |

## 11. Cut List (if even more time-constrained)
Cut in this order: Daily overview → Role switcher → keep only the merged timeline with visible task-assignment tags.
**Never cut:** the merged timeline and the visible task-assignment concept — together they *are* the pitch.
