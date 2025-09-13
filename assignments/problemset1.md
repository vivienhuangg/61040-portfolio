# Exercise 1 

- **Invariants**
  1. A Request’s `count` is always non-negative.  
  2. For each Request, the original requested quantity is preserved as  
     `original = remainingCount + totalPurchasedCount`.  

  The second invariant is more important because it ties together the lifecycle of a Request and ensures purchases never exceed what was originally asked. This invariant also indirectly enforces the first, since if purchases cannot exceed the original request, `remainingCount` cannot go negative.  

  The **purchase** action is most affected, because its effect reduces `remainingCount` while recording a purchase. Its design ensures that the sum remains constant.

- **Fixing an action**  
  The `removeItem` action potentially breaks the invariant if an item already has purchases. Removing the request discards the “original count,” so the relation between requests and purchases is lost.  
  **Fix**: add a precondition that no purchases exist for that request before it can be removed.

- **Inferring behavior**  
  The operational principle allows registries to be opened and closed repeatedly. The `open` action only requires the registry be inactive, and `close` only requires it be active.  
  **Reason**: this permits a user to temporarily close a registry (e.g., while editing or reorganizing requests) and later reopen it, without violating any invariants.

- **Registry deletion**  
  There is no deletion action. In practice, deletion would only be needed if storage limits were reached. Otherwise, registries can be archived, which is safer because purchase histories remain intact.

- **Queries**  
  - For the **registry owner**: list of completed purchases (to track fulfillment).  
  - For the **gift giver**: list of items not yet purchased (to avoid duplicates).

- **Hiding purchases**  
  A common feature is hiding purchases from the recipient. To support this, add a boolean field `hidePurchases` to registries. If true, owner queries return only aggregate counts rather than full purchase details (giver names, timestamps, etc.).

- **Generic types**  
  User and Item are generic parameters. For example, `Item` may be represented by SKU codes. This is preferable to names or descriptions, since SKUs are unique, stable identifiers, whereas names and prices change and are not guaranteed to be unique.

---

# Exercise 2 (done)

**concept** PasswordAuthentication  

**purpose** Provide a means to reliably recognize and restrict access to users of a system, ensuring that only registered and confirmed users can log in.  

**principle**  
A new user registers with a username and password. To complete registration, they must confirm with a secret token (sent via email). Only after confirmation can they authenticate and reuse the same credentials to be recognized on each login.  

**state**  
- a set of Users with:  
  - `Username: String` (unique identifier, case-insensitive)  
  - `Password: String`  
  - `Token: SecretToken?` (optional, for confirmation stage)  
  - `Confirmed: Boolean`  

**actions**  
- **register**(username, password): (User, SecretToken)  
  - **requires** No existing user has the same username.  
  - **effects** Creates a new User with password, sets `Confirmed = false`, generates a token, and sends it via email. Returns the new User and token.  

- **confirm**(username, token): User  
  - **requires** User exists, not yet confirmed, and token matches.  
  - **effects** Sets `Confirmed = true`, clears the token, returns User.  

- **authenticate**(username, password): User  
  - **requires** User exists with matching password and `Confirmed = true`.  
  - **effects** Returns User, granting access.  

**invariant**  
- For all distinct users `u1, u2`, `u1.username ≠ u2.username`.  
- Preserved by `register` since it requires uniqueness.  

**completeness**  
Covers full lifecycle: registration, confirmation, authentication. Undo is implicit: without confirmation, authentication is impossible, so no partial accounts are usable.  

---

# Exercise 3

**concept** PersonalAccessToken\[User, Scope]  

**purpose** Enable users to access systems programmatically through secrets that are revocable, scope-limited, and independent of their main password. This provides safer, least-privilege authentication for automated tools.  

**principle**  
A user may create multiple tokens. Each token has selected scopes (permissions) and may expire automatically. The secret is displayed only once at creation. Tokens are valid until expired or revoked.  

**state**  
- a set of Tokens with:  
  - `owner: User` (who the token belongs to)  
  - `secret: SecretString` (shown once at creation)  
  - `expiresAt: Time?` (optional expiration)  
  - `revoked: Boolean`  
  - `scopes: Set[Scope]`  

**actions**  
- **createToken**(owner, scopes, expiresAt?): (Token, SecretString)  
  - **effects** Creates a token with given scopes/expiry, returns token and secret.  

- **revokeToken**(owner, token)  
  - **requires** Token exists, belongs to owner, and is not revoked.  
  - **effects** Marks token revoked, permanently disabling it.  

- **authenticateWithToken**(user, secret): User  
  - **requires** Token exists with correct owner, secret matches, not revoked, not expired.  
  - **effects** Returns the user, with access restricted to token’s scopes.  

**invariants**  
- A secret uniquely identifies at most one valid token at a time.  
- Preserved by ensuring new tokens generate fresh, unique secrets.  

**completeness**  
Lifecycle includes creation, authentication, and revocation. Expiry naturally closes the lifecycle. Undo is supported via `revokeToken`.  

---

# Exercise 4

**concept** URLShortener\[OriginalUrl, ShortSuffix]  

**purpose** Allow users to share compact URLs that redirect to longer original URLs, improving convenience and readability.  

**principle**  
A user submits a long URL and may request a custom suffix. If none is provided, the system generates one. Each suffix uniquely maps to an original URL. Users can later look up or delete the mapping.  

**state**  
- a set of Records with:  
  - `originalUrl: String`  
  - `suffix: String (unique)`  
  - `createdAt: Time`  

**actions**  
- **shorten**(originalUrl, suffix?): ShortURL  
  - **requires** If suffix is given, it must not already exist.  
  - **effects** Creates a mapping and returns the short URL.  

- **map**(shortURL): String  
  - **requires** Mapping exists.  
  - **effects** Returns original URL.  

- **delete**(shortURL)  
  - **requires** Mapping exists.  
  - **effects** Removes mapping, freeing suffix for reuse.  

**notes**  
Without expiry, space of unique short URLs may eventually be exhausted; optional expiration could address this.  

---

**concept** ConferenceRoomBooking  

**purpose** Enable scheduling of conference rooms so multiple users can coordinate usage without conflicts.  

**principle**  
Each room can be booked by one user for a time interval. Users (or admins) can cancel bookings. Anyone can view existing bookings for a date.  

**state**  
- a set of Bookings with:  
  - `room: Room`  
  - `reserver: User`  
  - `start: Time`  
  - `end: Time`  

**actions**  
- **reserve**(room, user, start, end): Booking  
  - **requires** Room exists and interval does not overlap another booking.  
  - **effects** Creates booking for that user.  

- **cancel**(booking, user)  
  - **requires** Booking exists and user is reserver or admin.  
  - **effects** Removes booking.  

- **listBookings**(room, date): Set[Booking]  
  - **requires** Room exists.  
  - **effects** Returns all bookings intersecting with date.  

**invariants**  
- No two bookings overlap for the same room. Preserved by `reserve` precondition.  

---

**concept** BoardingPass\[User, Flight]  

**purpose** Provide passengers with an up-to-date authorization to board their flight, reflecting any changes to flight details.  

**principle**  
A boarding pass links one passenger to one flight and seat. The pass can be updated automatically if flight information changes. At boarding, validation ensures the passenger is authorized to board.  

**state**  
- a set of BoardingPasses with:  
  - `passenger: User`  
  - `flight: Flight`  
  - `seat: Seat`  
  - `status: {Scheduled, Delayed, Boarding, Closed, Canceled}`  

**actions**  
- **issue**(flight, passenger, seat): BoardingPass  
  - **requires** Passenger is ticketed for flight, seat available.  
  - **effects** Creates a boarding pass and delivers it.  

- **update**(pass, field, newValue)  
  - **requires** Pass exists.  
  - **effects** Updates field (gate, time, status, seat).  

- **validate**(pass): Boolean  
  - **requires** Pass exists.  
  - **effects** Returns true if valid for boarding, false otherwise.  

- **revoke**(pass)  
  - **requires** Pass exists and caller is airline/admin.  
  - **effects** Cancels pass, preventing boarding.  

**invariants**  
- Each passenger has at most one active boarding pass per flight.  
- Preserved by `issue` precondition.  

---
