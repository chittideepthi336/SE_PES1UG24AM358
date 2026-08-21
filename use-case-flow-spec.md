# Use-Case Flow Specification

**Use case:** Place Hold Request
**Primary actor:** Student Member
**Secondary actor:** System (Notification Service)

## Preconditions
- The student is authenticated (see *Authenticate*, included use case).
- The requested title exists in the ISBN catalog and all physical copies are currently on loan.

## Postconditions (success)
- A new entry is appended to the title's FIFO hold queue with a recorded timestamp and queue position.
- The student's dashboard reflects the new hold and its position.

## Main Success Scenario
1. Student searches the ISBN catalog and selects a title with no copies currently available.
2. System displays the title's availability status and current queue length.
3. Student selects "Place Hold."
4. System verifies the student's active hold/loan count is below the maximum allowed.
5. System appends the student to the FIFO queue and assigns a queue position.
6. System displays a confirmation with the assigned position to the student.
7. Use case ends.

## Alternate Flow — Hold Limit Reached
4a. System determines the student is already holding the maximum number of allowed reservations.
4b. System rejects the request and displays an error explaining the limit and the student's current holds.
4c. Use case ends without a queue entry being created.
