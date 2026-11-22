## Tool Lending Library Page Wireframes

These low-fidelity layouts cover every page in the [UX Flow](UX-Flow.md). Brackets indicate controls. Error and confirmation messages appear on the page where the action occurred.

### P1 · Tool catalog

```text
 TOOL LENDING LIBRARY                              [View a hold]
 [Search tools_________________] [Search]

 [Category v]  [Location v]  [Available now □]

 TOOL                 AVAILABILITY       LOCATION
 Hedge trimmer         Available          Tool Library    [Details]
 Tile saw             Checked out        Tool Library    [Details]

 No matches: Change filters or try a different search.
```

### P2 · Tool details

```text
 [Back to catalog]
 HEDGE TRIMMER
 Available for pickup at Tool Library
 Loan period: 7 days

 [Tool photo]      Description and usage information
                  [Safety guide]  [Training video]

 [Place hold]
```

When no copy is available, show the current availability and a link back to search.

### P3 · Place hold

```text
 [Back to tool details]
 PLACE HOLD · HEDGE TRIMMER
 Pickup: Tool Library
 Pickup window: 24 hours

 Library card number [___________________]
 [Confirm hold]

 Status: Card could not be verified. Check the number and retry.
```

Use the same status area if the copy becomes unavailable or the patron has reached a hold limit. A successful request opens P4.

### P4 · View or extend hold

```text
 HOLD CONFIRMED · HEDGE TRIMMER
 Status: Ready for pickup
 Pick up by: Tuesday, 3:30 p.m.
 Pickup: Tool Library
 Bring your county library card and photo ID.

 [Extend by 24 hours]                       [Back to catalog]
```

Show the revised deadline after an extension. Show the reason when an extension is unavailable. An expired hold displays its status and a link back to the catalog. The catalog's **View a hold** entry assumes a hold lookup or hold-specific link.

### S1 · Staff dashboard

```text
 TOOL LIBRARY STAFF                         Location [Tool Library v]

 [Start checkout]       [Process return]
 [Find patron]          [Manage inventory]
```

### S2 · Patron lookup and verification

```text
 [Dashboard]  CHECKOUT · FIND PATRON
 [Scan card]  Card number [____________] [Look up]
 [Search patron by name]

 Patron: Paul Example                  County card: Valid
 [ ] Photo ID verified                 [ ] Age 18 or older confirmed
 [Record photo ID override]            [View patron record]

 Active loans: 2 of 5                  Tool borrowing: Allowed
 Ready holds: Hedge trimmer             Waiver: Current

 [Continue checkout]
```

Manual search locates a patron record for staff verification. Display the reason if the card, age, or tool borrowing status prevents checkout. If the waiver is missing or outdated, continue to S3.

### S3 · Waiver

```text
 [Back to verification]
 TOOL LENDING WAIVER                        Version [current version]
 Patron: Paul Example

 +----------------------------------------------------------------+
 | Waiver text                                                    |
 |                                                                |
 +----------------------------------------------------------------+
 [ ] I accept this waiver
 Electronic signature [________________________]

 [Save signature and continue]      [Decline and end checkout]
```

Store the signed record with the version of the waiver the patron accepted. Prompt again when a new version is required.

### S4 · Checkout

```text
 [Dashboard]  CHECKOUT · PAUL EXAMPLE
 Active loans: 2 of 5                 Remaining slots: 3
 [Scan tool]  Barcode [____________] [Add]

 COPY       TOOL              SOURCE          DUE DATE        ACTION
 T-104      Hedge trimmer      Ready hold      Nov 27          [Remove]

 [View safety resources]   [Confirm checkout]
 Status: Checkout complete. Hedge trimmer due Nov 27.
```

Show a copy-specific error if a barcode is invalid, the copy is unavailable to this patron, or the selection would exceed the five-tool limit.

### S5 · Return

```text
 [Dashboard]  RETURN
 [Scan tool]  Barcode [____________] [Look up loan]

 TOOL: Hedge trimmer · T-104           BORROWER: Paul Example
 Due: Nov 27                          Returned: Nov 29
 Condition [Working v]                Damage notes [____________]

 Late fee
 Grace period: [calculated from due date and open days]
 Billable late days: [calculated]      Total: [calculated]

 [Confirm return]
```

If the barcode is missing or fails to match an active loan, show an error and keep the loan open. Record reported damage on the patron history and update the copy status after return.

### S6 · Inventory

```text
 [Dashboard]  INVENTORY                           [Add tool]
 [Search name or barcode________] [Search]
 [Location v] [Status v]

 BARCODE    TOOL              LOCATION       STATUS       ACTION
 T-104      Hedge trimmer     Tool Library   On loan      [Edit]
 T-221      Circular saw      Tool Library   Available    [Edit]
```

### S7 · Add or edit tool

```text
 [Back to inventory]  ADD OR EDIT TOOL
 Tool name [______________]    Category [______________]
 Brand/model [____________]    Barcode [_______________]
 Location [_____________ v]    Acquisition [Purchased / Donated v]
 Condition [____________ v]    Status [________________ v]
 Loan period [___________]    Replacement value [$_____]
 Late-fee class [Standard / High-demand or large v]
 Safety resources [____________________________________]
 Intake or correction notes [__________________________]

 [Save tool]                                      [Cancel]
```

For donations, record the working-condition check during intake. Show validation errors beside the relevant fields so staff can correct them before saving.

### S8 · Patron record

```text
 [Dashboard]  PATRON RECORD
 [Search by name or card number________________] [Search]

 Paul Example                       Card: Valid
 Tool borrowing: Allowed            Active loans: 2 of 5
 [Loans and fees] [Holds] [Damage and loss] [Signed waivers]

 Hold limit [________]     Hold frequency [________]
 Tool borrowing [Allowed / Restricted v]
 Reason [__________________________________________]
 [Save restrictions]
```

Staff can open the signed waiver and its version from **Signed waivers**. The borrowing status applies to the tool library.
