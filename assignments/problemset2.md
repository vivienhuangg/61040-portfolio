# Concept Questions

1. The context defines the scope of uniqueness and guarantees thare are no duplicated strings within the same context. The context will end up being the domain (eg. tinyurl.com).
2. It must store the used strings to check for collisions with newly generated nonces. In the context of using a counter, we can treat the associated "counter" as the ID of the nonce. Since we increment upon generation, we ensure that there are no collisions.
3. The advantage is that users more easily remember common dictionary words as opposed to a string of letters and numbers -- this make it easy for users to share (verbally/visually)/remember shortenings. One disadvantage is that non-intended users are more likely to guess a link. To change the current concept, instead of mapping counters to base-62 strings, have NonceGeneration.generate pick unused sequences of words from a curated dictionary, tracking those word-sequences in each context’s used set. The returned nonce is then the joined word sequence (e.g. "silver-piano"), ensuring memorability while still guaranteeing uniqueness within the context.

# Synchronization Questions

1. The first sync doesn't need the targetUrl -- it's function is to get a fresh nonce so it only needs the context to ensure uniqueness. Meanwhile, register creates the mapping from shortUrlBase to targetUrl, so we need both the shortUrl and targetUrl.

2. Sometimes, it's just not true that the variable name and argument name are the same, which may happen if variables are getting passed between multiple functions. For example, in register, NonceGeneration returns a nonce, which gets used as a shortUrlSuffix in UrlShortening.register.

3. It's not in the last sync because it only needs the newly created shortUrl. It waits for the completion of UrlShortening.register so it's not waiting for a request from the user.

4. We can then hardcode the context "bit.ly" into the syncs.

   * **sync generate**

     ```
     when Request.shortenUrl ()
     then NonceGeneration.generate (context: "bit.ly")
     ```

   * **sync register**

     ```
     when
       Request.shortenUrl (targetUrl)
       NonceGeneration.generate (): (nonce)
     then UrlShortening.register (shortUrlSuffix: nonce, shortUrlBase: "bit.ly", targetUrl)
     ```

   * **sync setExpiry**

     ```
     when UrlShortening.register (): (shortUrl)
     then ExpiringResource.setExpiry (resource: shortUrl, seconds: 3600)
     ```

5. **sync onExpire**

   ```
   when ExpiringResource.expireResource (): (resource: shortUrl)
   then UrlShortening.delete (shortUrl)
   ```

# Extending the Design

1. * **concept** AnalyticsCounting \[ShortUrl]

     * **purpose** count accesses to each short URL
     * **principle** after a short URL is looked up, its count increases by one; reading returns the current count
     * **state** a set of Shortenings with
       a shortUrl String
       a count Integer
     * **actions**

       * record (shortUrl)

         * requires a shortening exists for shortUrl
         * effect increments count\[shortUrl] by 1 every time a shortUrl is accessed
       * read (shortUrl) : (count: Integer)

         * requires a shortening exists for shortUrl
         * effect returns count\[shortUrl]

   * **concept** AnalyticsAccess \[User, ShortUrl]

     * **purpose** restrict analytics so only the owner of a short URL can view them
     * **principle** after recording an owner for a short URL, only that user may view its analytics
     * **state** a set of Shortenings with
       a shortUrl String
       an owner User
     * **actions**

       * setOwner (user: User, shortUrl: String)

         * requires a shortening exists for shortUrl
         * effect owner\[shortUrl] := user
       * authorize (user: User, shortUrl: String)

         * requires owner\[shortUrl] = user
       * respond (user: User, shortUrl: String, count: Integer)

         * requires owner\[shortUrl] = user
         * effect delivers count to user (no state change)

2. * **sync ownOnRegister**

     ```
     when
       Request.shortenUrl (by: user, targetUrl, shortUrlBase)
       UrlShortening.register (): (shortUrl)
     then AnalyticsAccess.setOwner (user, shortUrl)
     ```

   * **sync countOnLookup**

     ```
     when UrlShortening.lookup (shortUrl): (targetUrl)
     then AnalyticsCounting.record (shortUrl)
     ```

   * **sync viewAnalytics**

     ```
     when
       Request.viewAnalytics (by: user, shortUrl)
       AnalyticsAccess.authorize (user, shortUrl)
       AnalyticsCounting.read (shortUrl): (count)
     then AnalyticsAccess.respond (user, shortUrl, count)
     ```

3. * Add a userChoice concept that lets a requester propose a specific suffix if it’s available under a base. A sync routes requests with a desiredSuffix to UrlShortening.register directly; otherwise the normal nonce generator runs. This keeps generation policy separate from registration and avoids coupling target selection with alias picking. Conflicts (already-taken suffixes) are handled locally by userChoice availability check.
   * Swap nonce source for a WordNonceGeneration concept that draws from a curated dictionary and enforces minimum entropy (e.g., 2–3 words). The generate sync stays the same shape; only the generator changes, so UrlShortening and Analytics remain untouched. The trade-off is memorability vs. length/guessability, which the entropy setting can tune.
   * Add TargetUrlAnalytics concept that maintains counts per (owner, targetUrl) in addition to per-shortURL counts. A lookup sync increments both the shortURL counter and the aggregated target counter (after checking ownership via access concept). This lets creators see consolidated usage when multiple aliases point to the same destination. Privacy remains intact because reads still go through the same authorization gate.
   * Add a GuessDefense concept that sets a minimum entropy (or length) per context and a SecureNonceGeneration that honors it. The generator can lengthen tokens or retry until the policy is met, without changing UrlShortening or Analytics code. This preserves separation of concerns: policy is configured in one place; generation implements it; registration just consumes a nonce. As namespaces grow, the user can raise the entropy threshold without redesigning other modules.
   * Not desirable because it weakens the “analytics are not public” requirement and creates sharing risks.
