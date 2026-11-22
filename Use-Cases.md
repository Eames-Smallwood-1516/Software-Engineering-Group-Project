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
#### Primary Actor: Patron

#### Preconditions
Public catalog web application is online and connected to the inventory database
Patron holds a valid county library card number to place or extend a hold

### Postconditions
If a hold is placed: Tool status is updated to `On Hold` for 24 hours and linked to the Patron's library card number
If a hold is extended: `Hold` expiration timestamp is extended by an additional 24 hours and marked as extended

#### Primary Success Path
1. Patron navigates to the online tool catalog website
2. Patron browses or searches available tools across branches and views tool availability, specifications, and attached safety videos/reading materials
3. Patron selects an available tool and enters their county library card number into the hold form
4. Patron selects an available tool and enters their county library card number into the hold form
5. System places a 24-hour hold on the tool, updates its status to `On Hold`, and displays hold confirmation details

#### Extensions
3a. Selected tool is currently `Checked Out` or `On Hold`
    3a1. System disables the hold form and displays the tool's current status/expected return date
4a. Invalid Library Card Entered
    4a1. County Library Card API returns no match
    4a2. System displayed an invalid card error and prompts Patron to reenter card number
5a. (Saul Scenario) Patron requests a 24-hour extension on an existing hold
    5a1. Patron selects the option to extend their active hold
    5a2. System verifies the hold has not already been extended once and checks whether the tool is in the Top 20 most frequently borrowed items
    5a3. System extends the hold expiration by 24 hours (48 hours total) and confirms the new pickup deadline
5b. (Saul Scenario) Patron attempts to extend a hold on a Top 20 high-demand tool or an already-extended hold
    5b1. System detects the tool is in the Top 20 most borrowed list (or has already used its one-time extension).
    5b2. System rejects the extension request and displays a notice that the original 24-hour deadline remains the same

### UC-04: Process Expired Holds
#### Primary Actor: System Timer
##### Secondary Actor: Email Service

#### Preconditions
One or more active holds exist in the database with an expiration timestamp earlier than the current system time

### Postconditions
Expired hold records are marked as `Expired`
Associated tools are returned to `Available` status in inventory
Expiration notification emails are dispatched to the affected Patrons

#### Primary Success Path
1. System Timer triggers the scheduled hold-expiration check
2. System queries the database for all active holds whose 24-hour (or 48-hour extended) pickup window has elapsed
3. System updates each expired hold's status to `Expired` and updates the corresponding tool's status from `On Hold` back to `Available`
4. System sends an automated email via the Email Service to each Patron's registered email address notifying them that their hold has expired
   
#### Extensions
2a. No active holds have passed their expiration timestamp
    2a1. System logs that zero holds expired and terminates the scheduled run
4a. Email Service fails to deliver notification
    4a1. System logs the delivery failure for retry while still releasing the tool back to `Available` status so inventory is not locked
    4a2. System appends email task to dead letter queue

### UC-05: Add New or Donated Items to Inventory
#### Primary Actor: Clerk

#### Preconditions
Clerk is authenticated to the system with inventory management permissions
Physical tool has been purchased or donated and is ready for intake

### Postconditions
New tool record is created in the inventory database with branch location, replacement value, fee tier, loan duration, and condition metadata
A unique asset barcode is generated, assigned, and attached to the tool

#### Primary Success Path
1. Clerk opens the guided tool intake form in the system.
2. Clerk enters the tool's name, category, branch location, replacement cost, late-fee tier (standard $1/day vs. high-demand $3/day), loan duration (2-week standard or custom shorter duration), and optionally links to safety materials
3. System validates that all required fields are populated and generates a unique asset barcode
4. Clerk prints/affixes the barcode to the physical tool and confirms saving the record
5. System inserts the new tool into the inventory database with status `Available`
   
#### Extensions
1a. (Roy Generous Scenario) Tool is acquired via Patron donation rather than purchase
    1a1. Clerk inspects the donated tool to confirm it is in safe working order
    1a2. Clerk selects `Donated` as the acquisition source and assigns a remaining-lifespan/condition rating
    1a3. System saves the condition rating alongside the tool record and proceeds to Step 3
1b. Donated tool fails safety or working-order inspection
    1b1. Clerk rejects the donation; item is not entered into the inventory database; use case ends
3a. (Daryl Scenario) Clerk is unsure about tool categorization, fee tier, or entry details
    3a1. Clerk checks the Flag for Review option and enters explanatory notes detailing the uncertainty
    3a2. System saves the tool record with a Needs Audit flag attached for senior clerk review

### UC-06: Audit and Reconcile Inventory Records
#### Primary Actor: Senior Clerk (Dorthy)

#### Preconditions
Senior Clerk is authenticated to the system
One or more inventory records exist with a `Needs Audit` flag or require lifecycle retirement

### Postconditions
Flagged inventory records are corrected and their `Needs Audit` flags are cleared
Retired/worn-out tools are updated to `Retired` status while preserving historical loan records

#### Primary Success Path
1. Senior Clerk opens the Inventory Audit queue
2. System displays a list of all inventory records flagged with `Needs Audit` along with the notes left by the entering clerk
3. Senior Clerk selects a flagged record, reviews the notes, and updates any incorrect fields (e.g., category, fee tier, replacement cost, or loan duration)
4. Senior Clerk clears the `Needs Audi`t flag and saves the updated record
5. System updates the inventory record and removes it from the audit queue

#### Extensions
2a. Audit queue is empty
    2a1. System displays a message indicating no inventory records are currently flagged for review
3a. Senior Clerk determines a tool (such as an aging donation or damaged item) has reached the end of its usable life
    3a1. Senior Clerk marks the tool status as `Retired` and enters a retirement reason
    3a2. System removes the tool from active catalog availability while retaining its past checkout and damage history for reporting