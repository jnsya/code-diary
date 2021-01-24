# Security


## Encryption & Hashing

- _Encryption_ is reversible
  - Used to scramble data with the intention of de-scrambling it later. 
    - eg: transmitting financial data, writing to someone, etc.

- _Hashing_ is not reversible
  - Therefore useful for authenticating data.
    - eg: verify that a given password is correct, verify that some file has not been changed
  - Maps data of any size to a unique string of fixed length.
    - eg: SHA-256 algorithm converts data into a 256-bit, 64-character hexadecimal string
  
Terms:
- _to Hash_
- _to Encrypt_

- _Salt_
  - Random text added to some data before hashing. 
  - Purpose: prevent hacker using rainbow table to deduce password from a given hash.
  - By adding text prior to hashing, a hacker can't guess the password from the hash.
  - Salts are stored in the database alongside the hash.
  - 
- _Collision_
  - When a hashing algorithm produces the same hash from different inputs.
  - Problem 
- _Rainbow table_
  - A key/value list of passwords and their corresponding hash value.
  - Given a password hash, you can check the table for a corresponding password.
  - Countered by using a _salt_.
- _Check-sum_


## Password hashing walkthrough

### Saving a password:
- User enters a password on a form.
- The password is sent to the server
  - Password is unhashed, but the entire message is encrypted with https.
- Salt is added to the password, and the resulting string is hashed.
- The hash value of password+salt is saved to the database, alongside the unhashed salt.
- (The password itself is not stored!)

### Checking a password:
- User enters a password on a form & password is sent to the server.
- The server retrieves the hash and salt.
- The salt is added to the user-entered password, and hashed.
- The new hash is compared to the hash in the database.
  - If it's the same, the user is authenticated.
  - If not, the user is rejected.
