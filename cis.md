from pathlib import Path

content = r'''# Cybersecurity Practicals 1–10 — Beginner-Friendly README

**Purpose:** Learn the code as a complete Python beginner. Every practical includes the question/aim, code, line-by-line explanation, key variables/functions, a memory trick, and what the output means.

> **Important:** These are classroom demonstrations, not production-ready security tools. The malware scanner only checks a tiny demo signature list; the firewall program only simulates rules; the Diffie–Hellman values and RSA key size are for learning. Do not treat them as real-world secure configurations.

## Python basics used in almost every practical

- `print(...)` displays text or values on the screen.
- `name = value` stores a value in a variable. A variable is just a labelled box.
- Text goes inside quotes: `"hello"`. A `b"hello"` value is **bytes**, useful when working with cryptography.
- `if condition:` means “only do the indented lines if the condition is true.”
- `else:` means “otherwise.”
- `for item in collection:` repeats code once for each item.
- `def function_name(...):` creates a reusable function. It runs when called.
- `return value` sends a result back to the code that called the function.
- `import module` loads a Python library.
- `# comment` is a note for humans; Python ignores it.
- Indentation (spaces at the start of a line) matters in Python. Lines inside a function, loop, `if`, `try`, or `with` must be indented.
- `==` compares two values; `=` assigns a value.
- `!=` means “not equal”; `in` checks membership; `not in` checks absence.
- `len(x)` gives the length; `range(n)` gives numbers from `0` up to (but not including) `n`.
- `True` and `False` are Boolean values.
- `a[:5]` takes the first five items; `a[-1]` is the last item; `a[::2]` takes every second item.
- `f"Hello {name}"` is an f-string: Python inserts the value of `name` into the text.
- A function call such as `caesar_encrypt(text, shift)` means “run that function using these inputs.”

---

# Practical 1 — Substitution and Transposition Ciphers

## Question / Aim

Implement substitution and transposition ciphers. Encrypt a message and then decrypt it back to the original text.

- **Caesar cipher:** shifts each letter by a fixed number. This is a substitution cipher.
- **Rail-fence demonstration in this notebook:** places characters at even indexes first and odd indexes second. This is a simple transposition.

## Code

```python
print("Name: Samad Shaikh | Roll No: 56\\n")

def caesar_encrypt(text, shift):
    result = ""
    for ch in text:
        if ch.isalpha():
            base = ord('A') if ch.isupper() else ord('a')
            result += chr((ord(ch) - base + shift) % 26 + base)
        else:
            result += ch
    return result

def caesar_decrypt(text, shift):
    return caesar_encrypt(text, -shift)

def rail_fence_encrypt(text):
    return text[::2] + text[1::2]

def rail_fence_decrypt(cipher):
    mid = (len(cipher) + 1) // 2
    even, odd = cipher[:mid], cipher[mid:]
    result = ""
    for i in range(len(even)):
        result += even[i]
        if i < len(odd):
            result += odd[i]
    return result

plaintext = "Samad Shaikh Roll No 56"
shift = 3

print("Plaintext:", plaintext)
c = caesar_encrypt(plaintext, shift)
print("Caesar Encrypted:", c)
print("Caesar Decrypted:", caesar_decrypt(c, shift))

r = rail_fence_encrypt(plaintext)
print("Rail Fence Encrypted:", r)
print("Rail Fence Decrypted:", rail_fence_decrypt(r))
```

## Every line explained

| Code | Meaning |
|---|---|
| `print("Name: ...\\n")` | Prints the student heading. `\\n` in this displayed code represents the newline escape `\n`, which leaves a blank line after the heading. |
| `def caesar_encrypt(text, shift):` | Creates a function named `caesar_encrypt`. It receives the message (`text`) and how far to shift letters (`shift`). |
| `result = ""` | Starts with an empty string. Encrypted characters will be added to it. |
| `for ch in text:` | Visits each character of the message, one at a time. `ch` means the current character. |
| `if ch.isalpha():` | Checks whether the character is a letter. Spaces and digits are not letters. |
| `base = ord('A') if ch.isupper() else ord('a')` | Finds the numeric code for uppercase `A` or lowercase `a`, preserving the letter's case. `ord()` converts a character to a number. |
| `ord(ch) - base` | Converts a letter into a zero-based alphabet position: A/a = 0, B/b = 1, and so on. |
| `(... + shift) % 26` | Adds the shift and uses `% 26` to wrap around the 26-letter alphabet. |
| `chr(... + base)` | Converts the calculated number back into a letter. `chr()` converts a number into a character. |
| `result += ...` | Adds the newly encrypted letter to the end of `result`. `+=` means “update by adding.” |
| `else: result += ch` | If the character is not a letter, such as a space or digit, keep it unchanged. |
| `return result` | Sends the finished encrypted text back to the caller. |
| `def caesar_decrypt(text, shift):` | Creates the decryption function. |
| `return caesar_encrypt(text, -shift)` | Decrypts by applying the same shift in the opposite direction. If encryption shifts by `3`, decryption shifts by `-3`. |
| `def rail_fence_encrypt(text):` | Defines the transposition demonstration. |
| `return text[::2] + text[1::2]` | Takes characters at indexes 0, 2, 4… then indexes 1, 3, 5… and joins both groups. It rearranges characters instead of changing them. |
| `def rail_fence_decrypt(cipher):` | Defines the function that reverses that rearrangement. |
| `mid = (len(cipher) + 1) // 2` | Finds the size of the first group. `len()` counts characters; `//` divides and rounds down. Adding 1 handles odd-length text. |
| `even, odd = cipher[:mid], cipher[mid:]` | Splits the encrypted string into the first group and the second group. `[:mid]` means start through just before `mid`; `[mid:]` means from `mid` to the end. |
| `result = ""` | Creates an empty string for the recovered message. |
| `for i in range(len(even)):` | Loops through each index in the first group. `range(n)` counts from 0 to `n-1`. |
| `result += even[i]` | Adds the next character from the first group. |
| `if i < len(odd):` | Checks that the second group has a character at the same index. |
| `result += odd[i]` | Adds the matching character from the second group. |
| `plaintext = "Samad Shaikh Roll No 56"` | Stores the original message to test. |
| `shift = 3` | Sets the Caesar shift to three positions. |
| `print("Plaintext:", plaintext)` | Displays the original message. |
| `c = caesar_encrypt(plaintext, shift)` | Calls the encryption function and stores its returned result in `c`. |
| `print("Caesar Encrypted:", c)` | Displays the encrypted Caesar message. |
| `print("Caesar Decrypted:", caesar_decrypt(c, shift))` | Decrypts `c` and displays the recovered message. |
| `r = rail_fence_encrypt(plaintext)` | Encrypts the same original message using the transposition demonstration. |
| `print("Rail Fence Encrypted:", r)` | Displays the rearranged characters. |
| `print("Rail Fence Decrypted:", rail_fence_decrypt(r))` | Reverses the rearrangement and displays the original message. |

### Variables and functions to remember

- `text` / `plaintext`: original message.
- `shift`: number of alphabet positions to move.
- `ch`: one character currently being processed.
- `result`: builds the answer one character at a time.
- `c` and `r`: store the two encrypted results.
- `ord()` = character to number; `chr()` = number to character.
- `% 26` = wrap around the alphabet.

### Memory trick

**Caesar = shift letters. Rail fence = rearrange letters.**  
For Caesar remember: **loop → check letter → shift → add → return**.

### Output idea

The encrypted text should look different, but both decryption functions should recover `Samad Shaikh Roll No 56`.

> Note: this rail-fence function is a simple even-index/odd-index transposition demonstration, not the full zig-zag rail-fence cipher commonly described in textbooks.

---

# Practical 2 — RSA Encryption and Decryption

## Question / Aim

Use RSA public-key cryptography to encrypt a message with a public key and decrypt it with the matching private key.

## Code

```python
# In a notebook, install the library once if it is missing:
# !pip install rsa -q

import rsa

print("Name: Samad Shaikh | Roll No: 56\\n")

public_key, private_key = rsa.newkeys(512)
message = "Hello, I am Samad Shaikh, Roll 56"

encrypted = rsa.encrypt(message.encode('utf8'), public_key)
print("Original Message:", message)
print("Encrypted Message:", encrypted)

decrypted = rsa.decrypt(encrypted, private_key).decode('utf8')
print("Decrypted Message:", decrypted)
```

## Every line explained

| Code | Meaning |
|---|---|
| `# !pip install rsa -q` | A notebook command to install the `rsa` library. `#` makes it a comment here; in Google Colab/Jupyter, remove the `#` to run it. `-q` means quieter installation output. |
| `import rsa` | Loads the RSA library so the program can create keys and encrypt/decrypt. |
| `print(...)` | Displays the heading. |
| `public_key, private_key = rsa.newkeys(512)` | Generates a matching public/private key pair. Python unpacks the two returned values into two variables. `512` is the demonstration key size; it is too small for modern real-world security. |
| `message = "Hello, ..."` | Stores the original message as normal text (a string). |
| `message.encode('utf8')` | Converts the string into bytes because the encryption function expects bytes. UTF-8 is a text encoding. |
| `rsa.encrypt(..., public_key)` | Encrypts the bytes using the public key. The result is stored in `encrypted`. |
| `print("Original Message:", message)` | Prints the readable original text. |
| `print("Encrypted Message:", encrypted)` | Prints the encrypted bytes, which usually look like unreadable symbols. |
| `rsa.decrypt(encrypted, private_key)` | Uses the matching private key to recover the original bytes. |
| `.decode('utf8')` | Converts the recovered bytes back into readable text. |
| `decrypted = ...` | Stores the recovered text in `decrypted`. |
| `print("Decrypted Message:", decrypted)` | Displays the recovered message. It should match the original. |

### Variables and functions

- `public_key`: shared key used for encryption in this example.
- `private_key`: secret key used for decryption.
- `message`: original text.
- `encrypted`: unreadable encrypted bytes.
- `decrypted`: recovered text.
- `rsa.newkeys()`: generates keys.
- `rsa.encrypt()`: encrypts.
- `rsa.decrypt()`: decrypts.
- `.encode()`: text → bytes; `.decode()`: bytes → text.

### Memory trick

**RSA = Make keys → Encode → Encrypt with public → Decrypt with private → Decode.**

### Output idea

The encrypted output changes each run and looks unreadable. The decrypted output should say `Hello, I am Samad Shaikh, Roll 56`.

---

# Practical 3 — Message Authentication Code (HMAC)

## Question / Aim

Create an HMAC to check whether a message is authentic and unchanged. Then change the message and show that verification fails.

## Code

```python
import hmac
import hashlib

print("Name: Samad Shaikh | Roll No: 56\\n")

secret_key = b"samad_shaikh_56_secret"
message = b"Transfer Rs.1000 - Samad Shaikh, Roll No 56"

mac = hmac.new(secret_key, message, hashlib.sha256).hexdigest()
print("Message:", message)
print("Generated HMAC:", mac)

received_mac = hmac.new(secret_key, message, hashlib.sha256).hexdigest()
if hmac.compare_digest(mac, received_mac):
    print("Verification: Message is authentic and unaltered.")
else:
    print("Verification: FAILED")

tampered = b"Transfer Rs.9000 - Samad Shaikh, Roll No 56"
tampered_mac = hmac.new(secret_key, tampered, hashlib.sha256).hexdigest()
if hmac.compare_digest(mac, tampered_mac):
    print("Tampered: accepted")
else:
    print("Tampered message detected: HMAC mismatch.")
```

## Every line explained

| Code | Meaning |
|---|---|
| `import hmac` | Loads functions for creating and checking HMACs. |
| `import hashlib` | Loads hashing algorithms such as SHA-256. |
| `secret_key = b"..."` | Stores the shared secret key as bytes. The `b` means bytes rather than ordinary text. In real applications, never hard-code a secret like this. |
| `message = b"Transfer Rs.1000..."` | Stores the message as bytes. |
| `hmac.new(secret_key, message, hashlib.sha256)` | Creates an HMAC using the secret key, message, and SHA-256 hash algorithm. |
| `.hexdigest()` | Converts the resulting HMAC into readable hexadecimal characters (0–9 and a–f). |
| `mac = ...` | Stores the original message's HMAC. |
| `print("Message:", message)` | Displays the original message. |
| `print("Generated HMAC:", mac)` | Displays the generated authentication code. |
| `received_mac = ...` | Simulates the receiver calculating the HMAC again with the same key and message. In this classroom code, it recalculates locally. |
| `if hmac.compare_digest(mac, received_mac):` | Safely compares the two HMAC values. If they match, the condition is true. |
| `print("Verification: ...")` | Shows that the message passed the check. |
| `else:` | Runs if the values do not match. |
| `print("Verification: FAILED")` | Displays a failure message. |
| `tampered = b"Transfer Rs.9000..."` | Creates a changed message: the amount has changed from 1000 to 9000. |
| `tampered_mac = ...` | Calculates a new HMAC for the changed message. |
| `if hmac.compare_digest(mac, tampered_mac):` | Checks whether the original HMAC matches the changed message's HMAC. They should not match. |
| `print("Tampered: accepted")` | This branch should not occur for the changed message in this example. |
| `print("Tampered message detected...")` | Displays the expected result: the changed message has a different HMAC. |

### Variables and functions

- `secret_key`: secret shared key.
- `message`: original data.
- `mac`: original HMAC.
- `received_mac`: receiver's recalculated HMAC.
- `tampered`: changed message.
- `tampered_mac`: HMAC of the changed message.
- `hmac.new()`: creates HMAC.
- `hashlib.sha256`: selects SHA-256.
- `.hexdigest()`: converts digest to hexadecimal text.
- `hmac.compare_digest()`: compares the values.

### Memory trick

**HMAC = Same key + same message → same code. Change the message → different code.**

### Output idea

First, verification succeeds. Then the changed amount is detected because the HMAC no longer matches.

---

# Practical 4 — Digital Signatures

## Question / Aim

Create a digital signature using an RSA private key, verify it with the public key, and test a changed message.

## Code

```python
# In a notebook, install the library once if it is missing:
# !pip install rsa -q

import rsa

print("Name: Samad Shaikh | Roll No: 56\\n")

public_key, private_key = rsa.newkeys(512)
message = b"Signed by Samad Shaikh, Roll No 56"

signature = rsa.sign(message, private_key, 'SHA-1')
print("Message:", message)
print("Signature (first 30 bytes):", signature[:30], "...")

try:
    rsa.verify(message, signature, public_key)
    print("Signature Verified: Message is authentic.")
except rsa.VerificationError:
    print("Verification Failed!")

try:
    rsa.verify(b"Signed by Someone Else", signature, public_key)
    print("Tampered message verified (unexpected)")
except rsa.VerificationError:
    print("Tampered message: Verification Failed (as expected).")
```

## Every line explained

| Code | Meaning |
|---|---|
| `# !pip install rsa -q` | Notebook installation command; run without the leading `#` only if the library is missing. |
| `import rsa` | Loads RSA signing and verification functions. |
| `print(...)` | Displays the heading. |
| `public_key, private_key = rsa.newkeys(512)` | Creates a matching key pair for the demonstration. A 512-bit RSA key and SHA-1 are outdated and must not be used for real security. |
| `message = b"Signed by..."` | Stores the signed message as bytes. |
| `rsa.sign(message, private_key, 'SHA-1')` | Creates a signature using the private key and selected digest algorithm. |
| `signature = ...` | Stores the signature bytes. |
| `print("Message:", message)` | Displays the message being signed. |
| `signature[:30]` | Selects only the first 30 bytes so the output is shorter. |
| `try:` | Starts a block where Python will attempt an operation that might fail. |
| `rsa.verify(message, signature, public_key)` | Checks whether the signature matches this message and public key. |
| `print("Signature Verified...")` | Runs if verification succeeds. |
| `except rsa.VerificationError:` | Catches the specific error raised when signature verification fails. |
| `print("Verification Failed!")` | Displays the failure message if the original message's signature does not verify. |
| `rsa.verify(b"Signed by Someone Else", signature, public_key)` | Tries the same signature against a different message. |
| `print("Tampered message verified (unexpected)")` | This would indicate an unexpected result; it should not run for the changed message. |
| `print("Tampered message: Verification Failed...")` | Displays the expected result for the changed message. |

### Variables and functions

- `message`: data being signed.
- `private_key`: creates the signature.
- `public_key`: checks the signature.
- `signature`: proof generated by signing.
- `rsa.sign()`: creates a signature.
- `rsa.verify()`: verifies it.
- `try/except`: handles a verification failure without crashing the whole program.

### Memory trick

**Signature = Private key signs; public key verifies.**  
**HMAC uses one shared secret; a digital signature uses a key pair.**

### Output idea

The original message verifies successfully. The altered message fails verification.

---

# Practical 5 — Diffie–Hellman Key Exchange

## Question / Aim

Demonstrate how two people can independently calculate the same shared secret without sending the secret itself over the network.

## Code

```python
import random

print("Name: Samad Shaikh | Roll No: 56\\n")

P = 2147483647        # public prime
G = 5                 # public generator

samad_private = random.randint(2, P - 2)
bob_private = random.randint(2, P - 2)

samad_public = pow(G, samad_private, P)
bob_public = pow(G, bob_private, P)

print("Public Prime (P):", P)
print("Generator (G):", G)
print("Samad Shaikh's Public Key (x):", samad_public)
print("Bob's Public Key (y):", bob_public)

samad_secret = pow(bob_public, samad_private, P)
bob_secret = pow(samad_public, bob_private, P)

print("\\nSamad Shaikh's Shared Secret:", samad_secret)
print("Bob's Shared Secret:", bob_secret)
print("Secrets match:", samad_secret == bob_secret)
```

## Every line explained

| Code | Meaning |
|---|---|
| `import random` | Loads Python's random-number tools. This is for the classroom demonstration only; use a cryptographically secure library for real key exchange. |
| `print(...)` | Displays the heading. |
| `P = 2147483647` | Stores the public prime number used in the calculation. Both parties can know it. |
| `G = 5` | Stores the public generator/base. Both parties can know it. |
| `samad_private = random.randint(2, P - 2)` | Chooses Samad's private number between 2 and `P - 2`, including both endpoints. |
| `bob_private = random.randint(2, P - 2)` | Chooses Bob's separate private number. |
| `pow(G, samad_private, P)` | Calculates \(G^{samad\_private} \bmod P\) efficiently. The third argument tells `pow` to perform modular exponentiation. |
| `samad_public = ...` | Stores Samad's public value, which can be shared. |
| `bob_public = ...` | Stores Bob's public value, calculated using Bob's private number. |
| `print("Public Prime...", P)` | Displays the public prime. |
| `print("Generator...", G)` | Displays the public generator. |
| `print("...Public Key...", samad_public)` | Displays Samad's public value. |
| `print("...Public Key...", bob_public)` | Displays Bob's public value. |
| `pow(bob_public, samad_private, P)` | Samad uses Bob's public value and Samad's private value to calculate a shared secret. |
| `pow(samad_public, bob_private, P)` | Bob performs the matching calculation with Samad's public value and Bob's private value. |
| `samad_secret = ...` | Stores Samad's calculated secret. |
| `bob_secret = ...` | Stores Bob's calculated secret. |
| `print("...Shared Secret...", samad_secret)` | Displays Samad's result. |
| `print("...Shared Secret...", bob_secret)` | Displays Bob's result. |
| `samad_secret == bob_secret` | Compares both values. It evaluates to `True` if they are equal. |
| `print("Secrets match:", ...)` | Displays whether the shared secrets match. |

### Variables and functions

- `P`: public prime.
- `G`: public generator.
- `samad_private`, `bob_private`: private numbers; do not share them.
- `samad_public`, `bob_public`: public values that can be exchanged.
- `samad_secret`, `bob_secret`: independently calculated shared secret.
- `random.randint(a, b)`: chooses an integer from `a` to `b`, including both.
- `pow(a, b, c)`: calculates \(a^b \bmod c\).

### Memory trick

**Public P and G → choose private numbers → calculate public keys → exchange public keys → calculate secret → compare.**

### Output idea

The public values differ, but both shared-secret values should match, so the final comparison should be `True`.

---

# Practical 6 — IP Security (IPsec) Demonstration

## Question / Aim

Demonstrate packet confidentiality and integrity by encrypting data, calculating a message authentication code, verifying it, and detecting tampering.

> This program demonstrates some IPsec-like ideas in Python. It does **not** implement the IPsec protocol, its packet formats, or its real AH/ESP processing.

## Code

```python
# In a notebook, install the library once if it is missing:
# !pip install cryptography -q

import os
import hmac
import hashlib
from cryptography.fernet import Fernet

print("Name: Samad Shaikh | Roll No: 56\\n")

encryption_key = Fernet.generate_key()
cipher = Fernet(encryption_key)
auth_key = os.urandom(16)

payload = b"Sensitive IP Packet Data - Samad Shaikh, Roll No 56"

encrypted_payload = cipher.encrypt(payload)
packet_mac = hmac.new(auth_key, encrypted_payload, hashlib.sha256).hexdigest()

print("--- IPsec Outbound Processing ---")
print("Original Payload:", payload)
print("Encrypted Payload (ESP):", encrypted_payload[:20], "...")
print("Packet MAC (AH):", packet_mac)

received_mac = hmac.new(auth_key, encrypted_payload, hashlib.sha256).hexdigest()
if hmac.compare_digest(packet_mac, received_mac):
    decrypted_payload = cipher.decrypt(encrypted_payload)
    print("\\n--- IPsec Inbound Processing ---")
    print("Packet Authenticated. Decrypted Payload:", decrypted_payload)
else:
    print("Packet Dropped: Integrity check failed.")

tampered = bytearray(encrypted_payload)
tampered[-1] ^= 1
tampered_mac = hmac.new(auth_key, bytes(tampered), hashlib.sha256).hexdigest()
if not hmac.compare_digest(packet_mac, tampered_mac):
    print("\\n[Tamper Test] Packet Dropped: Integrity check failed.")
```

## Every line explained

| Code | Meaning |
|---|---|
| `# !pip install cryptography -q` | Notebook installation command for the `cryptography` library, if needed. |
| `import os` | Loads operating-system utilities; here it supplies random bytes. |
| `import hmac` | Loads HMAC functions for integrity checking. |
| `import hashlib` | Loads hash algorithms, including SHA-256. |
| `from cryptography.fernet import Fernet` | Imports the Fernet class, which provides authenticated symmetric encryption. |
| `encryption_key = Fernet.generate_key()` | Generates a random encryption key. Keep it secret. |
| `cipher = Fernet(encryption_key)` | Creates a Fernet object that can encrypt and decrypt with that key. |
| `auth_key = os.urandom(16)` | Generates 16 cryptographically random bytes for the separate HMAC key. |
| `payload = b"Sensitive..."` | Stores the original packet-like data as bytes. |
| `encrypted_payload = cipher.encrypt(payload)` | Encrypts and authenticates the payload with Fernet. |
| `packet_mac = hmac.new(...)` | Calculates an HMAC-SHA256 over the encrypted bytes. |
| `print("--- IPsec Outbound Processing ---")` | Prints a heading for the sending/outbound stage. |
| `print("Original Payload:", payload)` | Shows the original data. |
| `encrypted_payload[:20]` | Displays only the first 20 bytes of the encrypted result. |
| `print("Packet MAC (AH):", packet_mac)` | Prints the demonstration HMAC. The label is only illustrative; this is not an actual IPsec AH field. |
| `received_mac = hmac.new(...)` | Simulates the receiving side recalculating the HMAC from the received encrypted data. |
| `if hmac.compare_digest(packet_mac, received_mac):` | Checks whether the original and recalculated MAC values match. |
| `decrypted_payload = cipher.decrypt(encrypted_payload)` | Decrypts the encrypted data after the check passes. Fernet also verifies its own authentication tag. |
| `print("--- IPsec Inbound Processing ---")` | Prints a heading for the receiving/inbound stage. |
| `print("Packet Authenticated...", decrypted_payload)` | Displays the recovered original data. |
| `else:` | Runs if the HMAC values do not match. |
| `print("Packet Dropped...")` | Reports that the integrity check failed. |
| `tampered = bytearray(encrypted_payload)` | Makes a mutable copy of the encrypted bytes so one byte can be changed. |
| `tampered[-1] ^= 1` | Flips one bit in the last byte. `-1` means last position; `^=` applies bitwise XOR and stores the result. |
| `bytes(tampered)` | Converts the mutable `bytearray` back into immutable bytes. |
| `tampered_mac = hmac.new(...)` | Calculates a new HMAC for the modified data. |
| `if not hmac.compare_digest(packet_mac, tampered_mac):` | Checks whether the original MAC and tampered-data MAC differ. `not` reverses the Boolean result. |
| `print("[Tamper Test]...")` | Reports that the altered packet-like data was detected. |

### Variables and functions

- `encryption_key`: key used by Fernet.
- `cipher`: Fernet encryption/decryption object.
- `auth_key`: separate HMAC key.
- `payload`: original data.
- `encrypted_payload`: encrypted data.
- `packet_mac`: HMAC of encrypted data.
- `received_mac`: recalculated HMAC.
- `tampered`: modified copy of the encrypted data.
- `Fernet.generate_key()`: creates an encryption key.
- `cipher.encrypt()`: encrypts.
- `cipher.decrypt()`: decrypts and authenticates.
- `os.urandom(16)`: produces 16 random bytes.

### Memory trick

**Outbound: Key → Encrypt → MAC. Inbound: Recalculate MAC → Compare → Decrypt. Tamper: flip a bit → MAC changes.**

### Output idea

The normal data is recovered when verification succeeds. The tampering test should report that the altered data is rejected.

---

# Practical 7 — Web Security using SSL/TLS

## Question / Aim

Create a TLS connection to a website and display the TLS version and certificate details.

## Code

```python
import ssl
import socket

print("Name: Samad Shaikh | Roll No: 56\\n")

hostname = 'www.google.com'
port = 443

context = ssl.create_default_context()
print(f"Attempting to establish SSL/TLS connection to {hostname}:{port}...")

with socket.create_connection((hostname, port)) as sock:
    with context.wrap_socket(sock, server_hostname=hostname) as ssock:
        print("Connection Established! TLS Version:", ssock.version())
        cert = ssock.getpeercert()
        print("\\n--- Certificate Details ---")
        print("Subject:", dict(x[0] for x in cert['subject'])['commonName'])
        print("Issuer:", dict(x[0] for x in cert['issuer'])['organizationName'])
        print("Valid From:", cert['notBefore'])
        print("Valid Until:", cert['notAfter'])
```

## Every line explained

| Code | Meaning |
|---|---|
| `import ssl` | Loads Python's TLS/SSL tools. |
| `import socket` | Loads network connection tools. |
| `hostname = 'www.google.com'` | Stores the website hostname to connect to. |
| `port = 443` | Stores the standard port for HTTPS. |
| `context = ssl.create_default_context()` | Creates a TLS context with secure default settings and certificate verification. |
| `print(f"...{hostname}:{port}...")` | Prints a status message. The `f` lets Python insert the hostname and port values. |
| `with socket.create_connection((hostname, port)) as sock:` | Opens a TCP connection to the website. `with` manages the connection and closes it afterward. `sock` is the socket object. |
| `context.wrap_socket(sock, server_hostname=hostname)` | Wraps the TCP connection in TLS and supplies the hostname for server-name indication and certificate hostname checking. |
| `as ssock:` | Names the TLS-wrapped connection `ssock`. |
| `ssock.version()` | Returns the negotiated TLS protocol version, such as TLSv1.3. |
| `cert = ssock.getpeercert()` | Retrieves the server's peer certificate details after the TLS connection is established. |
| `print("--- Certificate Details ---")` | Prints a heading. |
| `cert['subject']` | Gets the certificate's subject information, describing who the certificate is for. |
| `dict(x[0] for x in cert['subject'])` | Converts the subject's tuple structure into a dictionary for easier lookup. |
| `['commonName']` | Selects the common-name field from the subject. |
| `cert['issuer']` | Gets the certificate issuer information. |
| `['organizationName']` | Selects the issuer's organization name. |
| `cert['notBefore']` | Reads the certificate's validity start time. |
| `cert['notAfter']` | Reads the certificate's validity end time. |

### Variables and functions

- `hostname`: website name.
- `port`: network port, 443 for HTTPS.
- `context`: TLS settings.
- `sock`: ordinary TCP socket.
- `ssock`: TLS-protected socket.
- `cert`: certificate details.
- `socket.create_connection()`: opens a TCP connection.
- `context.wrap_socket()`: adds TLS.
- `ssock.version()`: shows negotiated TLS version.
- `ssock.getpeercert()`: gets certificate information.

### Memory trick

**SSL/TLS = Import → hostname + port → context → TCP socket → wrap with TLS → show version → inspect certificate.**

### Output idea

If the website is reachable and its certificate is accepted, the program displays the TLS version, certificate subject, issuer, and validity dates. The exact values can change over time. Internet access is required.

---

# Practical 8 — Intrusion Detection System (IDS)

## Question / Aim

Create a simple IDS simulator that reads sample server logs and alerts when an IP address has too many failed password attempts.

## Code

```python
print("Name: Samad Shaikh | Roll No: 56\\n")

def simple_ids(log_data, threshold=3):
    failed_attempts = {}
    alerts = []
    for line in log_data:
        if "Failed password" in line:
            parts = line.split()
            ip = parts[-1]
            failed_attempts[ip] = failed_attempts.get(ip, 0) + 1
            if failed_attempts[ip] >= threshold and ip not in alerts:
                alerts.append(ip)
                print(f"[ALERT] Possible Brute Force Attack detected from IP: {ip}")

simulated_logs = [
    "Jan 10 14:00:01 server sshd: Accepted password for root from 192.168.1.5",
    "Jan 10 14:05:12 server sshd: Failed password for admin from 10.0.0.22",
    "Jan 10 14:05:15 server sshd: Failed password for admin from 10.0.0.22",
    "Jan 10 14:05:18 server sshd: Failed password for admin from 10.0.0.22",
    "Jan 10 14:06:00 server sshd: Failed password for root from 172.16.0.4"
]

print("Running IDS Simulator...")
simple_ids(simulated_logs, threshold=3)
```

## Every line explained

| Code | Meaning |
|---|---|
| `def simple_ids(log_data, threshold=3):` | Defines an IDS function. `log_data` is a list of log lines; `threshold=3` is the default alert count. |
| `failed_attempts = {}` | Creates an empty dictionary. It will store each IP address and its number of failed attempts. |
| `alerts = []` | Creates an empty list of IP addresses that have already triggered an alert. |
| `for line in log_data:` | Reads each log line one by one. |
| `if "Failed password" in line:` | Processes only lines that contain the failed-password phrase. Successful logins are skipped. |
| `parts = line.split()` | Splits the log line into words using whitespace. |
| `ip = parts[-1]` | Takes the final word as the IP address. This works for the sample logs because each line ends with an IP. |
| `failed_attempts[ip] = failed_attempts.get(ip, 0) + 1` | Reads the current count for this IP, using `0` if it has not appeared before, then adds one. |
| `if failed_attempts[ip] >= threshold and ip not in alerts:` | Checks whether this IP has reached the threshold and has not already triggered an alert. Both conditions must be true. |
| `alerts.append(ip)` | Adds the IP to the list of already-alerted IPs. |
| `print(f"[ALERT] ... {ip}")` | Prints an alert with the IP address inserted into the text. |
| `simulated_logs = [` | Starts a list of fake/example log entries. |
| Each quoted log line | Represents a server event. One line is a successful login; three lines show failures from `10.0.0.22`; one shows a failure from another IP. |
| `]` | Ends the list. |
| `print("Running IDS Simulator...")` | Displays a message before the function runs. |
| `simple_ids(simulated_logs, threshold=3)` | Calls the function with the sample logs and sets the alert threshold to three failures. |

### Variables and functions

- `log_data`: all log lines being checked.
- `line`: one log line.
- `threshold`: how many failures trigger an alert.
- `failed_attempts`: dictionary of counts by IP.
- `parts`: words from one log line.
- `ip`: IP address extracted from the line.
- `alerts`: IP addresses already alerted on.
- `.split()`: divides text into words.
- `.get(ip, 0)`: gets a dictionary value or returns 0 if absent.
- `.append(ip)`: adds an item to a list.

### Memory trick

**Read line → look for failure → split words → take last word as IP → count → alert at 3.**

### Output idea

The IP `10.0.0.22` has three failed attempts, so it triggers an alert. This is a simplified teaching example; real log formats require more careful parsing.

---

# Practical 9 — Malware Analysis and Detection

## Question / Aim

Calculate a file-content hash and compare it against a small list of known hash signatures to demonstrate signature-based detection.

## Code

```python
import hashlib

print("Name: Samad Shaikh | Roll No: 56\\n")

malware_signatures = {
    "e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855": "Trojan.Win32.Generic",
    "5e884898da28047151d0e56f8dc6292773603d0d6aabbdd62a11ef721d1542d8": "Ransom.WannaCry"
}

def scan_file_content(content):
    file_hash = hashlib.sha256(content).hexdigest()
    print(f"Calculated File Hash: {file_hash}")
    if file_hash in malware_signatures:
        print(f"[WARNING] Malware Detected! Threat Name: {malware_signatures[file_hash]}")
    else:
        print("[CLEAN] No known malware signatures found.")

print("Scanning File 1...")
scan_file_content(b"")

print("\\nScanning File 2...")
scan_file_content(b"print('Hello World - Samad Shaikh, Roll No 56')")
```

## Every line explained

| Code | Meaning |
|---|---|
| `import hashlib` | Loads hashing algorithms, including SHA-256. |
| `malware_signatures = {` | Starts a dictionary of example hash strings mapped to example threat names. |
| First long hash string | A SHA-256 hash for empty bytes is used as a demo signature, labelled `Trojan.Win32.Generic` in this sample dictionary. This is a classroom mapping, not proof that empty content is malware. |
| Second long hash string | Another example hash mapped to the label `Ransom.WannaCry`. |
| `}` | Ends the dictionary. |
| `def scan_file_content(content):` | Defines a function that accepts bytes representing file content. |
| `file_hash = hashlib.sha256(content).hexdigest()` | Calculates the SHA-256 digest of the bytes and returns it as hexadecimal text. |
| `print(f"Calculated File Hash: {file_hash}")` | Displays the calculated hash. |
| `if file_hash in malware_signatures:` | Checks whether the calculated hash is a key in the dictionary. |
| `print(f"[WARNING] ... {malware_signatures[file_hash]}")` | If it matches a key, displays the corresponding threat label. |
| `else:` | Runs when the hash is not in the signature dictionary. |
| `print("[CLEAN] ...")` | Says no known signature matched. It does **not** prove the file is safe. |
| `print("Scanning File 1...")` | Displays a label for the first test. |
| `scan_file_content(b"")` | Scans empty bytes. The SHA-256 hash of empty bytes matches the first dictionary key, so the demo prints its mapped label. |
| `print("\\nScanning File 2...")` | Prints a blank line and a label for the second test. |
| `scan_file_content(b"...")` | Scans a sample byte string containing Python source text. Its hash is not in this small list, so the demo reports no known match. |

### Variables and functions

- `malware_signatures`: dictionary of known hash → threat label.
- `content`: bytes to scan.
- `file_hash`: SHA-256 hash of the content.
- `hashlib.sha256()`: calculates a SHA-256 hash.
- `.hexdigest()`: converts the digest to hexadecimal text.
- `in`: checks whether the hash exists as a dictionary key.

### Memory trick

**Read bytes → calculate SHA-256 → look up hash → match means known signature; no match means unknown, not guaranteed safe.**

### Output idea

The first sample matches a deliberately configured dictionary entry and is labelled as malware. The second sample has no matching signature. This is a simplified signature-matching demo, not a real antivirus scanner.

---

# Practical 10 — Firewall Configuration and Rule-Based Filtering

## Question / Aim

Simulate firewall rules that allow or block incoming traffic based on its source IP address.

## Code

```python
print("Name: Samad Shaikh | Roll No: 56\\n")

def firewall_filter(packet_ip, firewall_rules):
    for rule_ip, action in firewall_rules.items():
        if packet_ip == rule_ip or rule_ip == "ANY":
            if action == "ALLOW":
                print(f"[+] Traffic from {packet_ip} is ALLOWED.")
            elif action == "BLOCK":
                print(f"[-] Traffic from {packet_ip} is BLOCKED.")
            return
    print(f"[-] Traffic from {packet_ip} Dropped (Default Policy).")

rules = {
    "192.168.1.50": "BLOCK",  # Malicious IP in this demo
    "10.0.0.5": "ALLOW",      # Trusted internal IP in this demo
    "ANY": "ALLOW"            # Default allow for this demo
}

incoming_traffic = ["10.0.0.5", "192.168.1.50", "172.16.0.1"]

print("Starting Firewall Packet Processor...")
for ip in incoming_traffic:
    firewall_filter(ip, rules)
```

## Every line explained

| Code | Meaning |
|---|---|
| `def firewall_filter(packet_ip, firewall_rules):` | Defines a function that receives one source IP and a dictionary of firewall rules. |
| `for rule_ip, action in firewall_rules.items():` | Loops through each rule. `.items()` gives each key and value; here, the key is `rule_ip` and the value is `action`. |
| `if packet_ip == rule_ip or rule_ip == "ANY":` | Checks whether the source IP exactly matches the rule, or whether the rule is the special catch-all `ANY`. |
| `if action == "ALLOW":` | Checks whether the matching rule says to allow traffic. |
| `print(f"[+] Traffic from {packet_ip} is ALLOWED.")` | Displays an allowed message and inserts the IP address. |
| `elif action == "BLOCK":` | If the action was not `ALLOW`, checks whether it is `BLOCK`. `elif` means “else if.” |
| `print(f"[-] Traffic from {packet_ip} is BLOCKED.")` | Displays a blocked message. |
| `return` | Stops this function after the first matching rule has been handled. |
| `print("...Dropped (Default Policy).")` | Runs only if the loop finishes without matching any rule. It reports the default action. |
| `rules = { ... }` | Creates a dictionary mapping source IPs or `ANY` to actions. |
| `"192.168.1.50": "BLOCK"` | Blocks this exact IP in the simulation. |
| `"10.0.0.5": "ALLOW"` | Allows this exact IP in the simulation. |
| `"ANY": "ALLOW"` | Allows any IP that did not match an earlier rule. Since the program checks rules in insertion order, this rule is last. |
| `incoming_traffic = [...]` | Creates a list of three source IP addresses to test. |
| `print("Starting Firewall Packet Processor...")` | Displays a startup message. |
| `for ip in incoming_traffic:` | Processes each IP address in the list. |
| `firewall_filter(ip, rules)` | Calls the filtering function for the current IP using the rules dictionary. |

### Variables and functions

- `packet_ip`: source IP currently being checked.
- `firewall_rules`: dictionary of rules.
- `rule_ip`: IP or special rule name being checked.
- `action`: action such as `ALLOW` or `BLOCK`.
- `rules`: the configured example rules.
- `incoming_traffic`: list of test IPs.
- `.items()`: returns dictionary key/value pairs.
- `return`: exits the function.

### Memory trick

**Loop over rules → compare IP → ALLOW or BLOCK → return. If nothing matches, use default policy.**

### Output idea

- `10.0.0.5` matches `ALLOW`.
- `192.168.1.50` matches `BLOCK`.
- `172.16.0.1` reaches the `ANY: ALLOW` rule and is allowed.

> This is only a simulation. Real firewalls process network packets with state, ports, protocols, interfaces, rule priorities, and other controls.

---

# Last-minute cheat sheet

| Practical | Main idea | Must remember |
|---|---|---|
| 1. Ciphers | Hide text by shifting/rearranging letters | `ord`, `chr`, `% 26`, `return` |
| 2. RSA | Public-key encryption and private-key decryption | `newkeys`, `encrypt`, `decrypt`, `encode`, `decode` |
| 3. HMAC | Detect message changes using a shared secret | `hmac.new`, `sha256`, `compare_digest` |
| 4. Digital signature | Sign with private key; verify with public key | `rsa.sign`, `rsa.verify`, `try/except` |
| 5. Diffie–Hellman | Two people calculate the same shared secret | private → public → shared secret, `pow(a,b,P)` |
| 6. IPsec demo | Encrypt data and check integrity | `Fernet`, `encrypt`, `decrypt`, HMAC |
| 7. SSL/TLS | Make a protected website connection | `socket`, `ssl`, `wrap_socket`, certificate |
| 8. IDS | Count failed logins and alert | `for`, `if`, dictionary `.get`, list `.append` |
| 9. Malware detection | Match a content hash to known signatures | `sha256`, `hexdigest`, dictionary lookup |
| 10. Firewall | Apply allow/block rules to source IPs | `.items`, `if/elif`, `return`, default rule |

## Five Python patterns worth memorizing

### 1. Function pattern

```python
def function_name(input_value):
    result = input_value
    return result
```

**Meaning:** define a task, work on an input, return the answer.

### 2. Loop pattern

```python
for item in items:
    print(item)
```

**Meaning:** repeat the indented code for every item.

### 3. If/else pattern

```python
if condition:
    print("Yes")
else:
    print("No")
```

**Meaning:** choose between two paths.

### 4. Dictionary counting pattern

```python
counts[key] = counts.get(key, 0) + 1
```

**Meaning:** get the old count (or zero), then add one.

### 5. Try/except pattern

```python
try:
    risky_operation()
except SomeError:
    print("Operation failed")
```

**Meaning:** try an operation; handle a particular error if it occurs.

## How to memorize each practical quickly

1. Memorize the **question and aim** in one sentence.
2. Learn the **flow** (the arrows/steps) before memorizing code.
3. Learn the three to six important function names for that practical.
4. Practise typing the code once while reading the explanations.
5. Cover the code and try to write the structure from the memory trick.
6. If you forget a variable name, remember its role: original message, encrypted result, key, count, rule, or output.

**Exam strategy:** if you get stuck, write the imports, define the input variables, show the main function call, and print the result. Keep indentation correct. Never confuse `=` (assign) with `==` (compare).
'''

path = Path("/mnt/data/README.md")
path.write_text(content, encoding="utf-8")
print(f"Created {path} ({len(content.splitlines())} lines, {path.stat().st_size} bytes)")
