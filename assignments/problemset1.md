# Exercise 1

* **Invariants**

  * A Request's Number is always non-negative.
  * The original requested quantity = current request count + current associated purchase count.

    * This is the more important invariant because it guarantees consistency between request and purchase counts. Also, this ensures the first invariant indirectly.
    * **purchase** is most affected because it decrements the request's count exactly by purchase count, making sure that the sum is always the original requested quantity.

* **Fixing an action**

  * *removeItem*: if an item already has purchases, removing the request removes the original count, breaking the sum invariant. To fix, only allow removing an item if purchases don't exist already for that item.

* **Inferring behavior**

  * Yes it can. *Open* only requires an inactive registry and *close* only requires an active registry, so opening and closing the registry does not violate the concept specs. This might occur when a user wants to temporarily close the registry to make edits and then reopen when edits are complete.

* **Registry deletion**

  * In practice, this would only matter if the database storing this information begins reaching capacity — otherwise, archiving is "safer."

* **Queries**

  * *Registry owner*: all completed purchases.
  * *Giver of gift*: all items that have not been purchased yet.

* **Hiding purchases**

  * Add a boolean `hidePurchases` for Registrys. If true, the *registry owner* query doesn't return complete information on the purchases (they would only see count, giver names, etc.).

* **Generic types**

  * SKU codes are often "eternal" and unique. Meanwhile, there are no constraints on items having unique names/descriptions/prices and items may change names/descriptions/prices at any time.

---

# Exercise 2

**concept** PasswordAuthentication

**purpose** limit access to known users

**principle** after a user registers with a username and a password, they must confirm the registration using a secret token (sent via email) before they can authenticate and be treated as the same user each time

**state**

* a set of Users with

  * a string Username
  * a string Password
  * an optional SecretToken Token
  * a boolean Confirmed

**actions**

* **register** (username: String, password: String): (user: User, token: SecretToken)

  * **requires** No existing user has the same Username (case insensitive)
  * **effects** creates a new User with Username = username, Password = password, Confirmed = false, Token = new token. Returns (User, token). Sends the token to the associated email.

* **confirm** (username: String, token: SecretToken): (user: User)

  * **requires** A User u with Username = u.username exists. u.Confirmed = false. u.Token = token.
  * **effects** sets u.Confirmed = true. removes u.Token. return User u.

* **authenticate** (username: String, password: String): (user: User)

  * **requires** A User u with Username = u.username exists. That User's Password = u.password. u.Confirmed = true.
  * **effects** returns the User u.

**Essential invariant:**

* For all distinct pairs of Users u1 and u2, u1.username != u2.username.
* Preserved in the *register* action because it requires that the entered Username does not exist already for another User.

---

# Exercise 3

**concept** PersonalAccessToken \[User, Scope]

**purpose** Allow programmatic authentication with revocable, scope-limited secrets instead of a password.

**principle** A user can create multiple tokens; each has selected scopes and optional expiry. The secret is shown once at creation. A token works until it expires or is revoked.

**state**

* a set of Tokens with

  * an owner User
  * a SecretString secret
  * a Time expiresAt (optional)
  * a flag revoked
  * a set of Scopes Scopes

**actions**

* **createToken** (owner: User, scopes: set of Scope, expiresAt?: Time): (token: Token, secret: SecretString)

  * **effects** create a new token with given owner, scopes, expiry; return the token and its secret

* **revokeToken** (owner: User, token: Token)

  * **requires** token exists, belongs to owner, and not revoked
  * **effects** mark the token revoked

* **authenticateWithToken** (user: User, secret: SecretString): (user: User)

  * **requires** a token exists with token.owner = user, token.secret = secret, not revoked, and (if expiresAt present) not expired
  * **effects** return the user, with access limited by token.scopes

**Notes**:
Personal access tokens (PATs) differ from passwords by being multiple, scope-limited, and individually revocable keys for API/CLI use, shown only once at creation. They reduce risk if leaked by expiring or being revoked without affecting other tokens. GitHub’s docs should start with a clear password-vs-PAT comparison and emphasize that PATs are separate, least-privilege credentials, not replacements for the web password.

---

# Exercise 4

**concept** URLShortener \[OriginalUrl, ShortSuffix]

**purpose** Provide shorter, easily shareable URLs that redirect to longer, original URLs.

**principle** a user submits a long URL and optionally a custom suffix;
if no suffix is provided, the system generates one;
each suffix uniquely identifies a mapping to an original URL;
users can later resolve or delete shortened URLs.

**state**

* a set of Records with

  * a string originalUrl
  * a string suffix (unique)
  * a Time CreationTime

**actions**

* **shorten** (originalUrl: String, suffix: Optional\[String]): (shortURL: String)

  * **requires** If suffix is given, it must not already exist.
  * **effects** Creates a new mapping from suffix (user-provided or autogenerated) to the given original URL. Returns the shortened URL.

* **map** (shortURL: String): (url: String)

  * **requires** shortURL exists in the mapping.
  * **effects** Returns the associated original URL.

* **delete** (shortURL: String)

  * **requires** shortURL exists.
  * **effects** Removes mapping so shortURL no longer resolves.

**notes**: may need to implement some expiry for links otherwise there would not be enough unique "tiny" URLs over time.

---

**concept** ConferenceRoomBooking

**purpose** Allow users to reserve conference rooms at specific times.

**principle** each room can be reserved for a specific time interval by at most one booking;
a booking belongs to a user and can later be canceled by that user or an admin;
the system allows inspection of existing reservations.

**state**

* a set of Bookings with

  * a room Room
  * a user Reserver
  * a time StartTime
  * a time EndTime

**actions**

* **reserve** (room: Room, user: User, start: Timestamp, end: Timestamp): (booking: Booking)

  * **requires** room exists and no booking overlap from start (inclusive) to end (exclusive)
  * **effects** create a new booking for the user in the room for this interval

* **cancel** (booking: Booking, user: User)

  * **requires** booking exists and user is the booker or an admin
  * **effects** remove the booking

* **listBookings** (room: Room, date: Date): (bookings: Set\[Booking])

  * **requires** room exists
  * **effects** return all bookings for the room that intersect with this date

**notes**: may implement admin-level authority (who may cancel and view all the details of bookings).

---

**concept** BoardingPass \[User, Flight, Timestamp]

**purpose** provide a digital pass for boarding a flight that updates automatically with flight changes

**principle** each boarding pass corresponds to one passenger and one flight;
the pass contains information such as departure time, gate, seat, and status;
when the flight’s information changes, the boarding pass is updated in real time;
the pass is validated at boarding to confirm authorization.

**state**

* a set of BoardingPasses with

  * a user Passenger
  * a string FlightNumber
  * a time DepartureTime
  * a string Gate
  * a seat Seat
  * a enum Status in {Scheduled, Delayed, Boarding, Closed, Canceled}

**actions**

* **issue** (flight: Flight, passenger: User, seat: String): (pass: BoardingPass)

  * **requires** passenger is ticketed for the flight and seat is available
  * **effects** create and deliver a boarding pass for this passenger and seat

* **update** (pass: BoardingPass, field: (Gate | DepartureTime | Status | Seat), newValue: (Gate | DepartureTime | Status | Seat))

  * **requires** pass exists
  * **effects** modify the boarding pass with the new value for the given field

* **validate** (pass: BoardingPass): (result: Boolean)

  * **requires** pass exists
  * **effects** return true if the pass authorizes boarding, false otherwise

**notes**: may implement real-time notifications to updates to BoardingPass.
