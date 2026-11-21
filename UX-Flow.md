# Tool Lending Library UX Flow

```mermaid
flowchart TD
    subgraph Public["Patron website"]
        CAT["P1 · Tool catalog"] --> ITEM["P2 · Tool details"]
        ITEM --> AVAIL{"Available to hold?"}
        AVAIL -->|Yes| HOLD["P3 · Place hold"]
        AVAIL -->|No| CAT
        HOLD --> VALID{"Valid card, available copy, and hold privileges?"}
        VALID -->|No| HOLD
        VALID -->|Yes| MANAGE["P4 · View or extend hold"]
        MANAGE --> ACTION{"Pickup, extension, or expiry?"}
        ACTION -->|Extension requested| EXT{"Active hold, outside top 20, and unused extension?"}
        EXT -->|Yes: add 24 hours| MANAGE
        EXT -->|No: show reason| MANAGE
        ACTION -->|Expires| EMAIL(["Release copy and email patron"])
    end

    subgraph Staff["Tool library staff"]
        HOME["S1 · Staff dashboard"] --> LOOK["S2 · Patron lookup and verification"]
        ACTION -->|Pickup with card| LOOK
        LOOK --> CARD{"Valid county library card?"}
        CARD -->|No| STOP(["Explain why checkout cannot proceed"])
        CARD -->|Yes| ID{"Photo ID verified or staff override recorded?"}
        ID -->|No| STOP
        ID -->|Yes| RULES{"Age 18 or older and tool borrowing allowed?"}
        RULES -->|No| STOP
        RULES -->|Yes| WAIVER{"Current waiver signed?"}
        WAIVER -->|No| SIGN["S3 · Waiver"]
        SIGN -->|Declined| STOP
        SIGN -->|Signed: save version and signature| OUT["S4 · Checkout"]
        WAIVER -->|Yes| OUT
        OUT --> LIMIT{"Copies lendable to patron and total loans at most five?"}
        LIMIT -->|No: correct selection| OUT
        LIMIT -->|Yes| LOAN(["Record borrower, copies, and due dates"])
        HOME --> RET["S5 · Return"]
        RET --> BAR{"Barcode matches an active loan?"}
        BAR -->|No: show error and keep loan open| RET
        BAR -->|Yes| COND["Record condition and review automatic late fee"]
        COND --> DONE(["Close loan and update copy status"])
        HOME --> INV["S6 · Inventory"]
        INV --> EDIT["S7 · Add or edit tool"]
        EDIT -->|Save valid record| INV
        LOOK --> PROFILE["S8 · Patron record"]
        HOME --> PROFILE
        PROFILE --> ACCESS(["Review loans, holds, history, waivers, or restrictions"])
    end
```

## Rules and design assumptions

- Holds last 24 hours. Eligible holds can be extended once for another 24 hours. Tools in the top 20 by checkout frequency are ineligible. The design assumes patrons can open a hold-specific link to review or request an extension.
- Staff can start a walk-in checkout from the dashboard. The photo ID override follows Franklin's scenario. Staff still verify the library card, age, borrowing privileges, and waiver.
- Each copy has its own barcode. Checkout shows each copy's due date based on its lending period of up to two weeks. Staff can share safety resources when available.
- Return fees follow the initial Q&A: one-day grace period (or until closing on the next open day), then $1 per day for standard tools or $3 per day for high-demand or large tools, capped at the lower of $20 or replacement value. Cathy's scenario does not state how the grace period was applied. The wireframe uses the initial Q&A rule and leaves damage charges unspecified.
- The patron record supports individual hold limits, hold frequency limits, and tool borrowing restrictions. Unreturned tools can block further tool borrowing while book borrowing remains available. The Q&A gives both a general fee cap and a replacement-value example for unreturned tools, so the unreturned-item charge needs clarification.
- Location appears in catalog and inventory so additional tool branches can be added later.
