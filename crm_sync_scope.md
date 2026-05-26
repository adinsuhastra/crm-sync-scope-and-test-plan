# CRM Sync Scope

## Goal

Plan a safe CRM sync between GoHighLevel and Close.io so contact or appointment data stays aligned without creating duplicates, overwriting important fields, or sending unintended messages.

This document is a planning sample only. It uses fictional assumptions and does not connect to either platform.

## Sync Direction Options

### Option 1: Close.io to GoHighLevel

Use when Close.io is the source of truth for appointments, sales activity, or outbound lead management.

Example:

- A new appointment is created in Close.io.
- The contact is created or updated in GoHighLevel.
- The appointment date/status is copied to matching fields in GoHighLevel.

### Option 2: GoHighLevel to Close.io

Use when GoHighLevel is the source of truth for marketing forms, lead capture, pipeline stage, or campaign activity.

Example:

- A lead fills a GoHighLevel form.
- The contact is created or updated in Close.io.
- The lead source, status, and notes are copied to Close.io.

### Option 3: Bidirectional Sync

Use only if explicitly required.

Bidirectional sync is riskier because both systems can update the same record. It needs clear conflict rules and duplicate handling before implementation.

Recommended first step:

Start with one direction in a test environment, then expand after the rules are proven.

## Client Questions Before Implementation

1. Which system should be the source of truth for contacts?
2. Which system should be the source of truth for appointments?
3. What exact event should trigger the sync?
4. Which fields are required?
5. Which fields must never be overwritten?
6. How should duplicates be detected?
7. What should happen if both CRMs have different values for the same contact?
8. Are there custom fields in either CRM?
9. Should deleted records be synced or ignored?
10. Should failed syncs notify someone?
11. Should the sync run instantly or on a schedule?
12. Is there a sandbox or test account?
13. Are API limits or webhook limits documented?
14. Who approves the sync before it goes live?

## Minimum Safe Scope

For a junior support trial, the safest scope is:

- document source-of-truth rules
- map required fields
- define duplicate detection logic
- create test records
- create test checklist
- prepare a handoff note for the builder/client

Live API implementation should happen only after the client confirms requirements and provides approved access.

## Suggested First Deliverable

A short scoping/test-plan package:

- field map
- sync direction decision
- duplicate rules
- error cases
- sample test records
- manual approval checkpoint
- go/no-go checklist

This makes the project safer before any live automation is enabled.
