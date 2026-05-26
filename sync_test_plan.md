# CRM Sync Test Plan

## Purpose

Test a CRM sync safely before enabling live automation between GoHighLevel and Close.io.

This is a planning sample only. It does not connect to either CRM.

## Test Environment

Preferred:

- sandbox GoHighLevel account
- sandbox Close.io account
- fictional contacts only
- test pipeline/stages
- no real campaigns or messaging enabled

If sandbox accounts are not available:

- use a small controlled test list
- label all records clearly as test data
- disable outbound messaging
- get client approval before any live record update

## Pre-Test Checklist

- Sync direction is confirmed.
- Source of truth is confirmed.
- Required fields are mapped.
- Duplicate rules are documented.
- Do-not-overwrite fields are listed.
- Error handling path is defined.
- Manual approval checkpoint is defined.
- Test records are fictional or client-approved.
- Live messaging is disabled.
- Rollback plan is documented.

## Test Cases

### Test 1: New Contact With Email

Input:
New contact exists in source CRM with name, email, phone, company, and source.

Expected result:
Destination CRM creates one new matching contact/lead.

Checks:

- no duplicate created
- email copied correctly
- phone normalized
- source copied
- cross-system ID stored

### Test 2: Existing Contact Matched by Email

Input:
Contact exists in both CRMs with same email.

Expected result:
Destination record is updated, not duplicated.

Checks:

- existing record updated
- no new record created
- protected fields not overwritten

### Test 3: Existing Contact Matched by Phone

Input:
Email missing, phone exists in both systems.

Expected result:
Record is matched by normalized phone only if phone match is exact.

Checks:

- phone normalized
- uncertain match routed to manual review

### Test 4: Missing Required Fields

Input:
Contact has blank email and blank phone.

Expected result:
Record is not synced automatically.

Checks:

- routed to manual review
- error note created
- no destination record created

### Test 5: Appointment Created in Close.io

Input:
Appointment or meeting activity is created in Close.io.

Expected result:
GoHighLevel receives appointment date/status only if contact match is confirmed.

Checks:

- date format correct
- timezone correct
- appointment status mapped
- no message sent automatically

### Test 6: Stage Conflict

Input:
GoHighLevel says `Won`, Close.io says `Potential`.

Expected result:
Conflict is routed to manual review.

Checks:

- no automatic overwrite
- conflict reason logged
- reviewer can choose source of truth

### Test 7: API Failure or Rate Limit

Input:
Destination API fails or rate limit is reached.

Expected result:
Workflow logs failure and retries only according to safe retry rules.

Checks:

- no repeated duplicate creation
- failure notification created
- record can be retried safely

## Manual Approval Checkpoint

Before live sync is enabled, a human should approve:

- field mapping
- duplicate detection rules
- protected fields
- test results
- rollback plan
- go-live timing

## Go / No-Go Criteria

Go only if:

- all core test cases pass
- no duplicate records are created
- no protected fields are overwritten
- failed records are logged
- manual review path works
- client approves go-live

No-go if:

- duplicate behavior is uncertain
- source of truth is unclear
- appointment timezone is unclear
- outbound messaging cannot be disabled
- API credentials or permissions are not safely configured
