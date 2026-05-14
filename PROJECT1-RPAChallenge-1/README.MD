## Project: Dynamic Web Data Migration (RPA Challenge)

### Executive Summary

This project automates the migration of structured employee data from a local Excel workbook into a dynamic web application. The primary challenge of this process is the **unstable UI environment**, where form fields change their screen positions and internal ID attributes with every transaction.
### 1. The Strategic Problem

Manual data entry into systems with shifting layouts is prone to high error rates and cognitive fatigue. For a standard automation, static selectors would fail immediately.
- **Process:** Data extraction from `.xlsx` and entry into a 7-field web form.
- **Pain Points:** Traditional selectors are ineffective due to dynamic `id` attributes and changing field coordinates.
- **Objective:** Develop a resilient bot capable of 100% accuracy regardless of UI layout shifts.
### 2. The Architectural Solution

The solution utilizes a **Linear Transactional Model** built within the UiPath Modern Design Experience. It emphasizes **selector stability** through a multi-layered identification strategy.
- **Data Handling:** Implemented a `Read Range` activity to ingest data into a `DataTable` variable, followed by a `For Each Row` loop for iterative processing.
- **Resilient Targeting:** Leveraged **UiPath Modern UIA (UI Automation)** using `NTypeInto` activities. Instead of relying on volatile IDs, I implemented **Anchor-based targeting**.
    - **Primary Target:** Fuzzy Selectors for the input fields.
    - **Anchor:** Static text labels (e.g., "First Name", "Phone Number") to ensure the bot "finds" the correct field relative to its label, regardless of where it moves on the screen.
---
### 3. Technical Stack

|**Category**|**Tools / Technologies**|
|---|---|
|**Platform**|UiPath Studio (Modern Design Experience)|
|**Activities**|`uix:NApplicationCard`, `uix:NTypeInto`, `uix:NClick`, `Workbook Read Range`|
|**Logic**|Data Table Manipulation, Fuzzy Selectors, Anchor-Based Logic|
|**Environment**|Chrome Browser, Microsoft Excel|
|**Versioning**|.NET Desktop Runtime compatibility|

---
### 4. Technical Deep-Dive: Logic Flow

1. **Initialization:** The bot opens the Chrome browser and navigates to the target URL while simultaneously reading the local `challenge.xlsx` file.
2. **Data Processing:**
    - The bot iterates through the rows of the Excel file.
    - For each record, it identifies the dynamic input fields using a combination of **Fuzzy Selectors** and **Visual Anchors**.
3. **Submission:** After the 7th field is filled, the bot triggers a `Click` on the "Submit" button to load the next dynamic state.
### 5. Impact & Results

- **Reliability:** Achieved a **0% failure rate** on UI element identification across multiple form refreshes.
- **Scalability:** The anchor-based logic developed here can be applied to any legacy or web system where the UI is inconsistent or updated frequently without notice.
- **Performance:** Reduced data entry time by approximately **85%** compared to manual human entry.

> This project demonstrates the transition from "vulnerable" automation to "resilient" automation. By ignoring volatile attributes and focusing on relative UI anchors, the bot remains functional even if the website's backend code or frontend layout is completely overhauled.

### Workflow Execution Schema

This ASCII representation maps the execution flow from the initialization phase through the iterative data entry logic

```
      [ START: Main Sequence ]
                  |
                  v
    +------------------------------+
    | Use Application/Browser:     |
    | Chrome (rpachallenge.com)    |
    +--------------+---------------+
                   |
                   v
    +------------------------------+
    | Read Range Workbook          |
    | (challenge.xlsx -> dt_Input) |
    +--------------+---------------+
                   |
                   v
    +------------------------------+
    | Write Line                   |
    | (Log Data Table to Output)   |
    +--------------+---------------+
                   |
                   v
    +------------------------------+
    | Click 'Start' Button         |  
    +--------------+---------------+     
                   |
                   v
    +------------------------------+
    | FOR EACH ROW (CurrentRow)    | <-----------+
    | in [dt_Input]                |             |
    +--------------+---------------+             |
                   |                             |
                   v                             |
        +-----------------------+                |
        | Type Into: First Name |                |
        +----------+------------+                |
                   |                             |
                   v                             |
        +-----------------------+                |
        | Type Into: Last Name  |                |
        +----------+------------+                |
                   |                             |
                   v                             |
        +-----------------------+                |
        | Type Into: Company    |                |
        +----------+------------+                |
                   |                             |
                   v                             |
        +-----------------------+                |
        | Type Into: Role       |                |
        +----------+------------+                |
                   |                             |
                   v                             |
        +-----------------------+                |
        | Type Into: Address    |                |
        +----------+------------+                |
                   |                             |
                   v                             |
        +-----------------------+                |
        | Type Into: Email      |                |
        +----------+------------+                |
                   |                             |
                   v                             |
        +-----------------------+                |
        | Type Into: Phone      |                |
        +----------+------------+                |
                   |                             |
                   v                             |
        +-----------------------+                |
        | Click 'Submit'        |                |
        +----------+------------+                |
                   |                             |
                   +-----------------------------+
                   |
                   v
        [ END: Main Sequence ]
```
