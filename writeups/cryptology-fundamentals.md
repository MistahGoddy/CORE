# Cryptology Fundamentals — Write-up

All challenges in this module followed a similar pattern: a file containing text encoded with a specific scheme, with the task being to recover the original plaintext. This module tests recognition and correct application of common encoding schemes, not cryptography — everything here is reversible without a key.

> **Note:** In line with certification integrity guidance, this write-up documents *methodology and tools only*. Actual decoded answers and screenshots showing solved output have been withheld so this doesn't function as an answer key for others taking the same certification.

## Binary

**Method:** CyberChef → `From Binary` operation (Delimiter: Space, Byte Length: 8) converts each 8-bit binary chunk to its ASCII character.

## Decimal / ASCII

**Method:** CyberChef → `From Decimal` operation (Delimiter: Space) maps each decimal number directly to its ASCII character.

## Base64

**Method:** CyberChef → `From Base64` operation reverses standard Base64 encoding back to plaintext.

## URL Encoding

**Method:** CyberChef → `URL Decode` operation converts `%XX` percent-encoded sequences back to their original characters.

## Hex

**Method:** CyberChef → `From Hex` operation (Delimiter: Auto) converts hex byte pairs back to ASCII.

## Base32

**Method:** CyberChef → `From Base32` operation reverses Base32 encoding using the standard `A-Z2-7=` alphabet.

## Quoted-Printable

**Method:** CyberChef → `From Quoted Printable` operation converts `=XX` sequences back to their original characters.

## HTML Character Entities

**Method:** Solved via Python's built-in `html` module rather than CyberChef, as a way to cross-verify results with a second independent tool.

```bash
python3 -c "import html; print(html.unescape('<encoded_string>'))"
```

## Uuencoding

**Method:** Solved via Python's `binascii.a2b_uu()`, again to demonstrate the same result achievable outside CyberChef.

```bash
python3 -c "
import binascii
with open('encoded.uu') as f:
    lines = f.readlines()
data_lines = [l for l in lines if not l.startswith('begin') and not l.startswith('end') and l.strip() != '\`']
decoded = b''.join(binascii.a2b_uu(l) for l in data_lines)
print(decoded.decode())
"
```

---

## Hashing Techniques

MD5 and SHA1 are **one-way** hash functions — there is no algorithm to reverse them directly. The only viable approach is a lookup against precomputed hash tables (rainbow tables) or brute-force/dictionary attacks.

**Method:** Both hashes were resolved via [CrackStation](https://crackstation.net/), which maintains a 190GB, 15-billion-entry lookup table for MD5/SHA1. This is a standard first step in real-world password-hash auditing, since unsalted hashes of common words/passwords are almost always already present in public rainbow tables.

Actual cracked values are withheld here for the same reason as above.
