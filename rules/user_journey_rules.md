# Rule 1: The User Journey Mapping Standard

Before analyzing business logic or databases, we must map the **User Journey**. This rule ensures we understand *what* the user is trying to achieve and *how* they interact with the app before we dive into the complex code that makes it happen.

When mapping a User Journey (Step 1), the resulting diagram or document MUST adhere to these strict guidelines:

### 1. Define the Entry Point (The Trigger)
Every journey must have a clear starting action. 
*   *Example:* "User clicks 'Business Settings' from the Sidebar Menu."
*   *Why:* We need to know how the user navigates to this feature.

### 2. Map Visual States (What the user sees)
Document the major UI components the user encounters in chronological order.
*   *Example:* "Displays 5 Category Cards," "Opens a Bottom Sheet," or "Shows a Success Snackbar."
*   *Why:* It defines the boundaries of the frontend screens we need to audit.

### 3. Map Human Actions (What the user does)
Document the physical interactions required.
*   *Example:* "User types a string," "User selects a Radio Button," or "User uploads a File."
*   *Why:* It tells us what kind of data the UI is collecting (Strings, Booleans, Files).

### 4. Map UI Decision Branches (Dynamic UI)
If the UI changes shape based on data, map those branches.
*   *Example:* "If setting is 'RADIO', show bubbles. If setting is 'TEXT', show keyboard."
*   *Why:* It exposes hidden complexity in the frontend code before we even look at the controller.

### 5. Map Secondary Actions & Edge Cases (The Alternatives)
Don't just map the "Happy Path". You must explicitly map alternative actions.
*   *Example:* "User clicks 'Search' icon," "User clicks 'Cancel' on the Bottom Sheet," or "User clicks 'Remove Image'."
*   *Why:* Missing a "Cancel" button or a "Search" bar means we are missing entire chunks of frontend code that need to be audited and maintained.

### 6. Define the Exit State (The Result)
How does the journey end from the user's perspective?
*   *Example:* "Bottom sheet closes and list updates instantly."
*   *Why:* It defines the "Definition of Done" for the frontend interaction.

### 7. STRICT BOUNDARY: No Backend Logic Allowed
The User Journey diagram MUST NOT contain complex database logic, API calls, or deep GetX controller state management.
*   *Rule:* You can say "App saves data," but you cannot say "App calls `Hive.openBox('settings').put(...)`".
*   *Why:* Mixing backend logic into the frontend journey creates confusing, bloated diagrams. We save the backend logic for Step 2 (GetX Methods) and Step 4 (System Architecture).