---
name: test-records
description: Create, edit and verify a clearly named test record (a reminder, note, contact or event) without touching the user's real data, and prove the saved result.
apps: com.apple.reminders, com.apple.mobilenotes, com.apple.MobileAddressBook, com.google.android.contacts
tools: ask_user, complete_task
effects: account_change, change_data, delete_data
---

# Test records: create, edit, verify

Use this when the goal asks you to create or change a record that exists only for the
task (often named "WitX ..." or "test ..."). The user's other records are off limits.

## Before you start

- Look at the list first. If a record with the exact name already exists, do not reuse or
  overwrite it: ask the user with `ask_user`, because it may be real data or a leftover.

## Creating and editing

- Use the exact name and text from the goal, character for character.
- Fill only the fields the goal names. Leave every other field as it is.
- Saving an ordinary record (reminder, note, contact, event) is not `account_change`:
  that effect is only for the user's own account, password, security or payment settings.
  Declare `change_data` for saving or editing it. Deleting a record is `delete_data`.
- Long forms (Contacts on Android): after typing, close the keyboard if it covers the
  next field, then swipe up inside the form to reach it. Check the field label in the
  newest screenshot before you type.
- iOS Reminders: details (notes, date) are behind the round "i" button on the reminder's
  row; the panel saves with "Done" at the top right.
- To change a saved record, open it again from the list and edit the one field; do not
  create a second record.

## Verifying

- Prove the final state from the list or search results, not from the edit screen: go
  back to the list (or search for the exact name), take a new screenshot, and check that
  exactly one record has the name and that it shows the final text.
- If the list shows only part of the text, open the record and check it there too.
- Finish with `complete_task`, citing the screenshot that shows the final state. Do not
  describe the result in a plain reply instead of calling the tool.

## When something goes wrong

- A second copy appeared: tell the user which one is extra and ask before deleting it.
- The wrong record opened: go back without changing anything and find the right one.
- A field did not take the text: tap it again, check it is focused, and type again only
  what is missing.
