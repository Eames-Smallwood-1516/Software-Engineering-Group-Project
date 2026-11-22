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

### UC-02: Return / Check in Tools and Assesses Fees
#### Primary Actor: Clerk

#### Preconditions
Clerk is authenticated on system
Tool being returned is registered in the system with a valid barcode and currently has `Checked Out` as its status.

### Postconditions
Active loan record is closed with a return timestamp
Item status is updated to `Available` or `Maintenance` or `Retired`
Any applicable late or damage fees are recorded on Patron's record

#### Primary Success Path
1. Patron brings one or more borrowed tools back to the Clerk for return
2. Clerk scans the barcode on the returned tool
3. System validates the barcode against active loan records and retrieves associated Patron profile and loan details, and confirms that tool is returned by the due date or one day grace period
4. Clerk physically inspects the tool and confirms it is in working condition
5. System closes the loan record, updates the tool status to `Available`

#### Extensions
2a. (Roy Scenario) Tool has no barcode or active loan record
   2a1. System rejects scan and alerts clerk that the item is unrecognized or not checked out
   2a2. Clerk declines to check in the personal/unmatched item and informs the Patron to return the library's barcoded tool; use case ends
3a. (Cathy Scenario) Tool is returned past due date + grace period
    3a1. System calculates the overdue days past the 1-day grace period and multiplies by the tool's daily rate ($1/day for standard tools, $3/day for high-demand tools)
    3a2. System caps the calculated late fee at the lesser of $20 or the tool's replacement value
    3a3. System displays the assessed late fee to the Clerk and records the balance on the Patron's account
4a. (Cathy Scenario) Tool is returned damaged or broken
    4a1. Clerk flags tool as damaged/broken and enters damage notes
    4a2. System assesses the damage fee, logs the damage incident on the Patron's profile for future clerks to see, and updates the tool status to Maintenance/Retired
4b. Tool is never returned
    4b1. System assesses the damage fee, logs the damage incident on the Patron's profile for future clerks to see, and updates the tool status to Maintenance/Retired
    4b2. System places a borrowing block on the Patron's tool library account until resolved, without affecting book borrowing privileges

### UC-03: Browse Catalog and Place/Extend Holds
#### Primary Actor:

#### Preconditions

### Postconditions

#### Primary Success Path

#### Extensions

### UC-04: Process Expired Holds
#### Primary Actor:

#### Preconditions

### Postconditions

#### Primary Success Path

#### Extensions

### UC-05: Add New or Donated Items to Inventory
#### Primary Actor:

#### Preconditions

### Postconditions

#### Primary Success Path

#### Extensions

### UC-06: Audit and Reconcile Inventory Records
#### Primary Actor:

#### Preconditions

### Postconditions

#### Primary Success Path

#### Extensions