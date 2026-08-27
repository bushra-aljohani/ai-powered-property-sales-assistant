# DreamHouse Offer Management Capstone

A Salesforce capstone project for managing real-estate offers in the DreamHouse application.

## Project Summary

This solution gives brokers a structured way to capture offers, connect each offer to a property and client, enforce basic data quality, route high-value offers for approval, and expose a clear lifecycle for reporting and automation.

The implementation lives in a Salesforce Developer Edition Playground. This repository is documentation-first: it records the architecture, verified configuration, demo path, and SkillWallet submission evidence without storing credentials or org secrets.

## Verified Salesforce Configuration

- Custom object: `Offer`
- Offer Name: Auto Number, format `OF-{0000}`
- Offer Amount: Currency(18, 0)
- Target Close Date: Date
- Offer Status: Picklist with `Submitted`, `Under Review`, `Accepted`, and `Rejected`
- Property: Master-Detail relationship to `Property`
- Contact: Lookup relationship to `Contact`
- Property Street Address: Required Text Area field `Street_Address__c`
- Offer sharing: Controlled by Parent
- Active validation rule: Offer Amount must be greater than zero
- Active validation rule: Target Close Date cannot be in the past
- Active record-triggered flow: `Offer Status Initialization`
- Active approval process: `High Value Offer Approval`
- Approval threshold: Offer Amount greater than or equal to `500000`
- Approval step: `Manager Review`
- Final approval action: Offer Status becomes `Accepted`
- Final rejection action: Offer Status becomes `Rejected`


## Demo Outcome

The demo shows a broker creating an Offer, entering a valid amount and future close date, linking the Offer to a Property and Contact, and submitting a high-value Offer for approval. 


## Status

The core data model, validation controls, initialization automation, and high-value approval process are configured in the Playground. Agentforce configuration, end-to-end testing evidence, and SkillWallet submission remain follow-up work and are tracked in the checklist.
