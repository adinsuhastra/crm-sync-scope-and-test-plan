# Field Mapping Example

This is a fictional field mapping sample for planning a CRM sync between GoHighLevel and Close.io.

No real CRM data is used.

## Contact Identity Fields

| Purpose | GoHighLevel Field | Close.io Field | Notes |
|---|---|---|---|
| Contact ID | `ghl_contact_id` | custom field: `ghl_contact_id` | Store cross-system ID after first sync |
| Close Lead ID | custom field: `close_lead_id` | `lead.id` | Store cross-system ID for future updates |
| Email | `email` | `contacts[].emails[].email` | Primary duplicate key if present |
| Phone | `phone` | `contacts[].phones[].phone` | Secondary duplicate key |
| Full name | `firstName` + `lastName` | `contacts[].name` | Normalize spacing and capitalization |
| Company | `companyName` | `lead.name` | Confirm whether company or person is the lead entity |

## Lead / Pipeline Fields

| Purpose | GoHighLevel Field | Close.io Field | Notes |
|---|---|---|---|
| Lead source | `source` | custom field: `lead_source` | Preserve original source if already set |
| Pipeline stage | `pipelineStage` | `status_label` | Needs explicit mapping table |
| Appointment date | custom field: `appointment_date` | activity/meeting date | Confirm date format and timezone |
| Appointment status | custom field: `appointment_status` | activity status | Use a controlled list |
| Notes | `notes` | `note` activity | Avoid overwriting existing notes |

## Example Stage Mapping

| GoHighLevel Stage | Close.io Status | Notes |
|---|---|---|
| New Lead | Potential | Default for newly captured leads |
| Appointment Booked | Meeting Scheduled | Trigger only after appointment exists |
| No Show | Follow Up | Add note rather than overwrite status if uncertain |
| Won | Customer | Confirm whether Close or GHL owns final status |
| Lost | Closed Lost | Require reason if available |

## Validation Rules

- Email or phone must be present before sync.
- Contact name should not be blank.
- Appointment date must be a valid date if appointment sync is enabled.
- Timezone must be confirmed before copying appointment times.
- Required custom fields must exist in the destination CRM before live sync.
- Existing cross-system IDs should be preferred over fuzzy matching.

## Duplicate Prevention Strategy

Recommended matching order:

1. Existing stored cross-system ID.
2. Exact email match.
3. Exact normalized phone match.
4. Company + name match, only for manual review.

Do not automatically merge uncertain duplicates.

Uncertain matches should be routed to manual review before updating either CRM.

## Fields That Should Not Be Overwritten Automatically

- lifecycle/pipeline stage
- owner/assigned user
- manually written notes
- final won/lost status
- source attribution
- unsubscribed / do-not-contact status

These fields need explicit client rules before automation.
