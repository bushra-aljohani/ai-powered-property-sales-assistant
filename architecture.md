# Solution Architecture

## Business Problem

DreamHouse brokers need a reliable way to capture client offers against properties. Offers must be traceable to the property and client, protected by data-quality rules, and escalated when the amount is high enough to require management review.

## Data Model

| Component | Type | Purpose |
|---|---|---|
| Property | Managed custom object | The home being offered on |
| Contact | Standard object | The prospective buyer or client |
| Offer | Custom object | The offer submitted by a client |
| Offer Amount | Currency field | Monetary value of the offer |
| Target Close Date | Date field | Intended completion date |
| Offer Status | Picklist | Lifecycle state for automation and reporting |
| Property | Master-detail field on Offer | Enforces parent-controlled access and related records |
| Contact | Lookup field on Offer | Links the offer to the client |
| Street Address | Required text area on Property | Captures the visit or property location |

## Security Design

- `Offer` inherits sharing behavior from `Property` because the relationship is master-detail.
- Validation rules prevent invalid amounts and past target dates.
- Approval locks a submitted high-value Offer while it is under review.
- The approval process is configured for Property Owner submission and Manager Review.
- A future production deployment should use a dedicated permission set for broker and manager access, with least privilege for Offer fields and actions.

## Automation Design

### New Offer Initialization

`Offer Status Initialization` is an active record-triggered flow that runs after an Offer is created and sets the lifecycle status to `Submitted`.

### High-Value Approval

`High Value Offer Approval` is active for Offers where `Offer Amount >= 500000`.

- Initial submitter: Property Owner
- Approval step: Manager Review
- Final approval: set Offer Status to `Accepted`
- Final rejection: set Offer Status to `Rejected`
- During approval: record locked
- After rejection: record unlocked

## Future Agentforce Layer

The intended Agentforce extension is a broker-facing assistant that can answer questions about Offers and Properties, summarize a client’s offer history, and invoke approved actions such as launching the Offer approval flow. Agentforce should use least-privilege access and should never expose confidential data to an unauthorized user.

