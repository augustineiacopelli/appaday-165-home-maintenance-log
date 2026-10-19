# Home Maintenance Log

**AppADay 165.** Track recurring home upkeep and warranty dates in one list. Furnace filters, smoke and CO alarm batteries, gutters, water heater flushes, and appliance warranties roll forward when you tap Done, with overdue and due soon colors, undo, and JSON export. Stored on your device, no account needed.

Live: https://augustineiacopelli.github.io/appaday-165-home-maintenance-log/

Portfolio: https://augustineiacopelli.github.io/appaday/

## What it does

You keep one list of things around the house that come due. Each recurring task has a repeat interval in days, weeks, months, or years. Tap Done and the next due date rolls forward. Warranties are single dates that count down to expiry and have no Done button.

Items are sorted by due date. Each card shows a colored edge and a pill: red when overdue, amber when due soon (within 30 days for tasks, 60 days for warranties), and green otherwise. A warranty that expires today still counts as active until the day is over. The summary strip at the top counts overdue, due soon, and tracked items, and the chips filter the list to All, Overdue, Soon, or Warranties.

Tap a card to expand it for the schedule, the last done date, notes, and Edit and Delete. The Add item button opens a library of about 70 common tasks and warranties grouped by heating and cooling, safety, plumbing and water, exterior and yard, appliances, inside the house, and warranties, with a search box. Tap one and the form opens with its usual schedule already filled in (for example furnace filter every 3 months, smoke and CO alarm batteries every 6 months, a dishwasher warranty of 1 year), and the first due date set one interval from today. Every default can be changed before saving. Anything not on the list goes in through Custom item. The overflow menu holds Export JSON, Import JSON, and Add starter set, which seeds six sample items at staggered due dates and skips any you already have by name.

## How the dates roll

There are two schedule modes, chosen per task. A fixed schedule keeps the original cadence: the next due date is the earliest point on the original anchor plus a multiple of the interval that falls after both the current due date and today. A task that is months overdue therefore lands in the future on its usual rhythm instead of drifting. The other mode counts from the day you finish.

Month math clamps to the last day of short months and remembers the original day. A monthly task anchored on January 31 lands on February 28, then returns to March 31. A yearly task anchored on February 29 lands on February 28 in common years and returns to February 29 in the next leap year. Day counts use UTC midnight so daylight saving changes never shift a result.

Every change that rolls a date, deletes an item, adds the starter set, or imports a file shows a toast with Undo for a few seconds.

## Data and privacy

Everything is stored in this browser under one localStorage key. There is no account, no server, and no tracking. If the browser blocks storage, the app keeps working in memory and shows a notice to export a copy. Export JSON downloads the whole list. Import JSON merges a file into the current list, replacing items that share an id, ignoring entries it cannot read, and offering Undo.

## Tech

One self contained `index.html` in plain HTML, CSS, and vanilla JavaScript with no dependencies and no Claude API calls. It carries the AppADay home screen icon tags (icon at `icons/165.png`, home screen title Home Upkeep, full screen launch), the web app manifest link, and the shared `save.js` Save to Phone script from the portal.

Built for a 375px phone viewport with 44px tap targets, no horizontal scroll, and a dark theme with a blue accent.

## Possible future update

A Send to Task Manager option could push any item, or a batch of items, with its due date into AppADay 060, Task Manager. Yearly, monthly, and weekly items would map onto Task Manager's own repeat rules. Items on other intervals, such as every 3 or 6 months, would arrive snoozed until their due date with the real interval in the notes. The safest design hands the items to Task Manager and lets its existing duplicate matching merge them in, which needs a small update on the Task Manager side. A no change alternative would be an export file in Task Manager's backup format that imports through its existing Merge.

## Testing

The pure date functions were checked with Node assertions during the build, covering month clamping, the leap day round trip, a fixed schedule more than two intervals overdue, undo restoring both dates, and a warranty expiring today reading as not yet expired. The page was then exercised in Chromium at 375px and at desktop width for add, edit, done, undo, delete, filters, export, and import. The test scripts are not shipped.
