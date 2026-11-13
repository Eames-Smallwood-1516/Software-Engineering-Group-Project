## Team Members
 - Eric Orr
 - Eames
 - Joseph Burke
 - Parker
  
# Tool Lending Library Questions

## Primary Goals

### Q: Is the primary goal to create a system to support a new Tool Library, or does the existing library system need development?

### Q: What timeline do you have for this project?

### Q: What budget exists for this project?


## Existing Systems

### Q: How are library members and their cards currently validated?

### Q: What kind of database exists for current library users?

### Q: Are cards scanned or is their data manually entered?


## Scale

### Q: How many people currently use the library on a daily, weekly, yearly basis.

### Q: How many people do you anticipate to use the tool library?


## Location

### Q: Where are the two locations?

### Q: Besides the tool library, are there any other separate locations in your system (i.e. multiple for books)?
### A: 
- We'll possibly have more tool lending locations in the future, so we should plan for it.
- There are other branches with books, but they're separate physical locations.

## Database Schema

### Q: What fields are currently in the database?

### Q: What fields are necessary for lending a tool?

### Q: How are tools being tracked, what fields are necessary?


## Lending

### Q: Are checkouts renewable?

### Q: Is there any insurance waiver or other documentation signed at checkout

### Q : Is there a limit to the amount of books/tools a person can be lent at a given time? If so, what are those limits?
### A: 
Borrowers can borrow up to five tools at once. This is unrelated to the maximum number of books they can have at once.

### Q: How does the library calculate late fees if someone checks out a book but turns it in late? How does this work for tools?
### A: The tool late fees are:
- 1 day grace period (or until closing on the next open day): No fee
- After that: $1/day for standard tools, $3/day for high-demand or large tools (tile saw, post-hole digger, pressure washer).
- Fees never exceed the tool's replacement value, or $20 per item, whichever is lower.

### Q: What if someone fails to return a lent item at all? Are they charged? Does this affect their ability to check items out?
### A:
Unreturned tools just show up as maximum late fees (value of the tool) on the account. Borrowers can't borrow any more tools until the situation is rectified. This doesn't affect their book borrowing, however.

### Q: You say checkouts are UP TO two weeks. Is there a minimum lending period? (e.g. a day)
### A:
We have some items that are shorter than two-week checkouts. There's no minimum checkout time.

### Q: Are there any age-restrictions for the tools or certain books we need to be aware of?
### A:
Tools can only be borrowed by patrons 18 and over.

### Q: How does the library handle scenarios where a book or tool is returned but damaged?
### A: 
We'll track damage and loss on a per-borrower basis. This should be viewable by the clerk doing the checkout, but generally we're not worried about deliberate damage to tools.

