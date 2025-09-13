# Exercise 1 (done)

- **Invariants**. What are two invariants of the state? (Hint: one is about aggregation/counts of items, and one relates requests and purchases). Say which one is more important and why; identify the action whose design is most affected by it, and say how it preserves it.
  - A Request's Number is always non-negative. 
  - The original requested quantity = current request count + current associated purchase count. 
    - This is the more important invariant because it guarantees consistency between request and purchase costs. Also this ensures the first invariant indirectly. 
    - **purchase** is most affected because it decrements the request's count exactly by purchase count, making sure that the sum is always the original requested quantity. 
- **Fixing an action**. Can you identify an action that potentially breaks this important invariant, and say how this might happen? How might this problem be fixed?
  - *removeItem*. if an item already has purchases, removing the request removes the original count, breaking the sum invariant. To fix, only allow removing an item if purchases don't exist already for that item. 
- **Inferring behavior**. The operational principle describes the typical scenario in which the registry is opened and eventually closed. But a concept specification often allows other scenarios. By reading the specs of the concept actions, say whether a registry can be opened and closed repeatedly. What is a reason to allow this?
  - Yes it can. Open only requires an inactive registry and close only requires an active registery, so opening and closing the registry does not violate the concept specs. This might occur when a user wants to temporarily close the registry to make edits and then reopen when edits are complete. 
- **Registry deletion**. There is no action to delete a registry. Would this matter in practice?
  - In practice, this would only matter if the database storing this information begins reaching capacity -- otherwise, archiving is "safer."
- **Queries**. What are two common queries likely to be executed against the concept state? (Hint: one is executed by a registry owner, and one by a giver of a gift.)
  - *Registry owner*: all completed purchases. 
  - *Giver of gift*: all items that have not been purchased yet. 
- **Hiding purchases**. A common feature of gift registries is to allow the recipient to choose not to see purchases so that an element of surprise is retained. How would you augment the concept specification to support this?
  - adding a boolean hidePurchases for Registrys. If true, the *registry owner* query doesn't return complete information on the purchases (they would only see count, giver names, etc. )
- **Generic types**. The User and Item types are specified as generic parameters. The Item type might be populated by SKU codes, for example. Explain why this is preferable to representing items with their names, descriptions, prices, etc.
  - SKU codes are often "eternal" and unique. Meanwhile, there are no constraints on items having unique names/descriptions/prices and items may change names/descriptions/prices at any time. 

# Exercise 2 (done)
**concept** PasswordAuthentication
  **purpose** limit access to known users
  **principle** after a user registers with a username and a password,
they must confirm the registration using a secret token (sent via email)
before they can authenticate and be treated as the same user each time
  **state**
    a set of Users with
        a string Username
        a string Password
        an optional SecretToken Token
        a boolean Confirmed
  **actions**
    **register** (username: String, password: String): (user: User, token: SecretToken)
      **requires** No existing user has the same Username (case insensitive)
      **effects** creates a new User with Username = username, Password = password, Confirmed = false, Token = new token. Returns (User, token). Sends the token to the associated email. 
    **confirm** (username: String, token: SecretToken): (user: User)
      **requires** A User u with Username = u.username exists. u.Confirmed = false. u.Token = token. 
      **effects** sets u.Confirmed = true. removes u.Token. return User u. 
    **authenticate** (username: String, password: String): (user: User)
      **requires** A User u with Username = u.username exists. That User's Password = u.password. u.Confirmed = true. 
      **effects** returns the User u.

- What essential invariant must hold on the state? How is it preserved?
  - For all distinct pairs of Users u1 and u2, u1.username != u2.username. It's preserved in the register action because it requires that the entered Username does not exist already for another User. 

# Exercise 3
- a concept specification for PersonalAccessToken and a succinct note about how it differs from PasswordAuthentication and how you might change the GitHub documentation to explain this.

# Exercise 4
- three concept specifications, with any subtleties explained in brief additional notes.