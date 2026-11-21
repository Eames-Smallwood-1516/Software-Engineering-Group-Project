# Use Cases
## Refer to `User-Scenarios.md` for narrative understanding of Use Cases

### UC-01: Check out tools to Patron
#### Primary Actor: Clerk
Secondary Actors: Library Card API, Waiver System

#### Preconditions
Clerk is authenticated to system
Tools being checked out are registered in system via valid barcodes.

#### Postconditions
Active loan record is inserted into database
Tool Inventory item status' updated to `Checked Out`

#### Primary Success Path
1. Patron presents their county library card and photo ID to Clerk
2. Clerk enters or scans library card into system
3. System queries the county library card system to retrieve Patron's name, contact info, standing, and other details
4. Clerk inspects Patron's Photo ID to confirm identity and age
5. System verifies Patron has fewer than 5 tools checked out currently
6. Clerk scans barcodes of each item to be borrowed, up to five items
7. System looks up each tool and their loan rules, such as the standard 2 week loan vs a shorter loan duration.
8. Clerk confirms checkout, loan record is created and inserted into database, and a receipt is handed to Patron

#### Extensions
1a. (Franklin Scenario) Patron forgot or lost photo ID.
    1a1. Clerk searches up the Patron manually by entering name or other details
    1a2. Clerk selects `Manual ID Override` for the purposes of logging a reason
    1a3.  System records override and proceeds to step 5
4a. (Billy Scenario) Patorn is under 18 years of age.
    4a1. Clerk flags/identifies the Patron is a minor.
    4a2. System blocks the checkout and displays an age-restriction notice; use case ends
5a. (Cathy Scenario) Patron has an unreturned and overdue tool
    5a1. System detects an unreturned tool that is overdue and checked out the Patron
    5a2. The system displays the Patron's damage/loss history and blocks new checkouts until resolved; use case ends.
5b. (Saul Goodman Scenario) Patron has not signed current waiver version
    5b1. System prompts Patron to review and sign the current electronic waiver
    5b2. System saves a timestamped, versioned signature to cloud storage and proceeds to step 6.
6a. (Cathy Scenario) Patron attempts to borrow more than 5 tools
    6a1. System rejects the 6th scan and alerts clerk that the 5 tool limit has been reached.
