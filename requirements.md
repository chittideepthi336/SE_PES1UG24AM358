# Requirements Table — Digital Campus Library Reservation Gateway

## Functional Requirements

| ID | Type | Description | Priority | Acceptance Criteria | Rationale |
|---|---|---|---|---|---|
| FR-001 | Functional | The system shall allow members to search the ISBN repository and place hold requests on currently loaned books to enter a FIFO waitlist. | High | Pass: user is assigned correct position in queue. Fail: user already holding max allowed books places additional reservation. | Core value proposition — without holds, the queue-based lending model doesn't exist. |
| FR-002 | Functional | The system shall allow a student to cancel a pending hold request at any point before the book is allocated to them. | Medium | Pass: hold removed from queue and all subsequent positions shift up by one. Fail: attempt to cancel a hold already converted to an active loan is rejected. | Students' needs change; a rigid queue with no exit path causes phantom demand and wasted holds. |
| FR-003 | Functional | The system shall automatically notify a student by email when a held book becomes available, and hold the reservation for a fixed 48-hour pickup window. | High | Pass: notification sent within 5 minutes of return check-in; reservation auto-expires and passes to next in queue after 48 hours if uncollected. Fail: no notification sent, or book released before window elapses. | Manual notification doesn't scale past a handful of holds; this is the automation the "Digital" in the system name promises. |
| FR-004 | Functional | The system shall allow a student to renew a currently borrowed book by one additional loan period, provided no other member is on the hold queue for that title. | Medium | Pass: due date extended and confirmation shown. Fail: renewal blocked with reason shown if a hold exists on the title. | Protects queue fairness — renewals can't be allowed to indefinitely starve waiting members. |
| FR-005 | Functional | The system shall allow the Head Librarian to view and manually reorder or remove entries in any book's hold queue (e.g., for lost/damaged copies or policy exceptions). | High | Pass: librarian's queue edit is saved and reflected immediately to affected students. Fail: a non-librarian account attempting this action is denied with an authorization error. | Automated FIFO logic breaks down in edge cases (lost books, disputes) — an administrative override is a business necessity, not a nice-to-have. |

## Non-Functional Requirements

| ID | Type | Description | Priority | Acceptance Criteria | Rationale |
|---|---|---|---|---|---|
| NFR-001 | Performance & Security | The catalog search query shall return paginated results for 50,000 titles in less than 150 ms. | High | Pass: benchmarking tests confirm target latency and security standards under simulated peak load. | Given directly in the lab handout as the sample NFR. |
| NFR-002 | Reliability & Usability | The reservation and queue-position status shown to a student shall reflect the true database state within 2 seconds of any change (hold placed, cancelled, or fulfilled), and the system shall maintain 99.5% uptime during library operating hours. | Medium | Pass: status displayed to user matches backend state in timed tests; uptime logs meet the 99.5% threshold over a rolling 30-day window. Fail: stale queue position shown, or downtime exceeds the SLA. | Students act on the position number they see — if it lags reality, they misjudge whether to keep waiting, which erodes trust in the whole queue mechanism. |
