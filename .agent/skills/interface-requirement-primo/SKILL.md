---
name: interface-requirement-primo
description: Expert in AdaPos+ to Primo CRM integration for corporate member management, points, coupons, and unified data reporting.
---
# CRM Primo Integration Skill

This skill provides expert guidance for integrating AdaPos+ with Primo CRM, specifically tailored for the corporate unified reporting system.

## 📚 Authorities & References
- **Project Structure**: `adaproject/Project Journal`
  - **SRS**: `PRJ24004-AdaPos+Journal-SRS-Interface CRM-02.00.00.pdf` (Functional Requirements)
  - **API Spec**: `PRIMO_s Open API Spec_03.00.02_Dev.xlsx` (Technical Implementation)
  - **Architecture**: `JourneyCRM-Architech-02.00.00.png`
- **Data Standards**: `adadocs/AdaPos+ PPG/PPG Master issue690212.md` (Corporate Data Mapping)
- **Interface Template**: `adadocs/AdaPos+ Interface Template.md`

## 🎯 Implementation Goals

### 1. Membership & Customer Data
- **Member Sync**: Implement real-time or batch synchronization of member profiles.
- **Reference Codes**: Ensure all customer data is mapped to Corporate Reference Codes (as per PPG Master).
- **Multi-Brand Support**: Handle brand-specific identifiers while maintaining a unified corporate view.

### 2. Loyalty Engine (Points & Rewards)
- **Earn Points**: Calculate points based on transaction value and rules.
- **Burn Points**: Process point redemption for discounts or products.
- **Get Balance**: Real-time balance checking via API.
- **Adjust Points**: Backend point adjustment capabilities.

### 3. Coupon Management
- **Validation**: Verify coupon validity before applying to transaction.
- **Redemption**: Confirm usage and update status in Primo.
- **Mapping**: Map coupon codes to internal promotion IDs.

### 4. Technical Standards
- **Mock Mode**: Always implement a mock service layout first for testing.
- **Error Handling**: Follow standard AdaPos+ error logging patterns.
- **Authentication**: Implement robust token management (refresh, expiry).
- **Data Integrity**: Validate all payloads against the Open API Spec before sending.

## 5. Sequence & Data Flow Patterns
- **Reference Pattern:** `REST API` (Sync) for interactive flows (Burn/Balance).
- **Diagram:** Link to standard Redemption sequence diagram (`templates/seq-primo-redemption.puml`).

## 6. Instructions for Agent
1. **Context First**: Before modifying code, verify the current API spec version in `adaproject`.
2. **Use API Integration Mode**: Ensure code supports switching between Mock and Real API via environment variables.
3. **Data Mapping**: Apply the "Corporate Reference Code" logic (Many-to-One) for any product/customer data sent to Primo.
4. **Validation**: Check inputs against `7 Properties` defined in PPG Master when dealing with product-related CRM data.
