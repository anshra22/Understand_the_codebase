# Party Feature Session Summary

This document summarizes the complete analysis of the "Parties" feature conducted during this session.

### 1. Feature Overview & Purpose
We established that the **Parties** feature is a foundational "Root Feature" (Phase 1 Master Data). Its primary purpose is to act as the central CRM, managing all **Customers** and **Suppliers**. It is the "Who" for every transaction in the application.

### 2. Deep-Dive Analysis
We conducted a detailed analysis of the feature and found:
*   **The Data Model:** The `Party` model is comprehensive, containing over 32 fields that store everything from basic contact information and addresses to critical financial data like their running `balance`.
*   **The Business Logic:** The `PartyAddFormController` is responsible for creating and updating parties.
*   **The "Opening Balance" Dependency:** We discovered that the most complex part of this feature is its interaction with the accounting system. When a Party is created with an "Opening Balance," the controller doesn't just save the balance; it physically creates an `AccountLedger` transaction and injects it into the `accounts_ledgers` database, ensuring the books are balanced from day one.

### 3. Generated Documentation
We have created the following artifact to permanently document our findings:

*   **File Location:** `ansh/party/party_overview.md`
*   **Contents:** A detailed, formatted Markdown file containing:
    1.  A high-level overview of the feature's purpose.
    2.  A breakdown of the critical fields in the `Party` data model.
    3.  A detailed explanation of the "Opening Balance" business logic.
    4.  A complete "Dependency Audit," showing what the feature reads from (SharedPreferences, Business model) and what it writes to (`parties` box, `accounts_ledgers` box).
    5.  A clear explanation of its relevance to the Sales Invoice PDF bug.

In short, we have completed a full "Step 1" and "Step 2" analysis of the Parties feature, resulting in a single, comprehensive overview document that perfectly captures its architecture and purpose.
