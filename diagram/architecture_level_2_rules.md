# Architecture Diagram Rules (Level 2: System Context)

These rules define the Level 2 Architectural Diagrams for the Bookz App. While Level 1 diagrams focus exclusively on the internal flow of a single feature (UI -> GetX -> Hive), Level 2 diagrams map the feature's relationship with the rest of the application ecosystem.

Every Level 2 diagram must incorporate a strict **6-Box Structure** to fully document a feature's Domain Boundaries, Dependencies, and Mutators.

### The 6-Box Structure

#### The Internal Core (Boxes 1, 2, & 3)
The diagram must include the internal 3-tier stack, adhering to all Level 1 rules (specific widgets, exact Dart signatures, "One Shape = One File", etc.).
1.  **Box 1: Internal UI Layer** (The screens the user interacts with inside the feature's folder).
2.  **Box 2: Internal GetX Layer** (The business logic controllers specific to this feature).
3.  **Box 3: Internal Hive Layer** (The centralized database operations managing this feature's data).

#### The External Boundaries (Boxes 4, 5, & 6)
To map the system context, the diagram must include three distinct boundary boxes. These boxes identify how other modules interact with the target feature's database (Box 3).

4.  **Box 4: External Mutators (Who changes the data?)**
    *   **Definition:** Other distinct features or modules in the app (e.g., `Sales`, `Purchases`, `Expenses`) that actively write to, update, or delete data within the target feature's database.
    *   **Visual Rule:** These features must be drawn on the **Left Side** of the diagram. Arrows must point **INTO** the target feature's Hive Layer (Box 3). Arrow labels should specify the transactional trigger (e.g., `Decreases Stock on Sale`).

5.  **Box 5: External Readers (Who reads the data?)**
    *   **Definition:** Other distinct features or modules (e.g., `Dashboard`, `Reports`, `Estimates`) that passively consume the target feature's data but never alter it.
    *   **Visual Rule:** These features must be drawn on the **Right Side** of the diagram. Arrows must point **OUT OF** the target feature's Hive Layer (Box 3) towards these readers. Arrow labels should specify the consumption purpose (e.g., `Reads Data for PDF Generation`).

6.  **Box 6: Configuration Dependencies (What changes the rules?)**
    *   **Definition:** Global settings, shared preferences, or master configuration files (e.g., `BusinessSettings`, `TaxSettings`, `Auth Roles`) that fundamentally alter the target feature's logic or behavior without changing the core data itself.
    *   **Visual Rule:** These dependencies must be drawn at the **Top or Bottom** of the external boundaries. Arrows must point **INTO** the target feature's internal GetX Layer (Box 2) or UI Layer (Box 1), indicating a behavioral dependency. Arrow labels should specify the rule being enforced (e.g., `Checks if Serial Tracking is Enabled`).

### Visual Styling for Level 2
To clearly distinguish the internal core from the external system context, apply the following high-contrast styling:

*   **Internal Core (Boxes 1, 2, 3):**
    *   UI Layer: Blue shades (`#1565c0`)
    *   GetX Layer: Green shades (`#2e7d32`)
    *   Hive Layer: Orange/Red shades (`#e65100`)
*   **External Boundaries (Boxes 4, 5, 6):**
    *   External Mutators (Box 4): Purple/Dark Magenta (`#6a1b9a`)
    *   External Readers (Box 5): Teal/Dark Cyan (`#00695c`)
    *   Configurations (Box 6): Grey/Charcoal (`#424242`)
*   **Text:** All text must remain high-contrast white (`color:#ffffff`).