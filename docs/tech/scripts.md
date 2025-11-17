# Scripts

via google AI:

> MySQL's latin1 character set is effectively equivalent to Windows-1252 (CP1252), not the stricter ISO-8859-1 (also known as Latin-1). While ISO-8859-1 leaves some code points (specifically in the 0x80 to 0x9F range) undefined, Windows-1252 assigns characters to these positions, including common symbols like the Euro sign (€) and various typographic characters.
>
> Therefore, when working with MySQL's latin1 character set, especially when dealing with data originating from Windows environments, it is generally safer to assume that the encoding in use is Windows-1252. This distinction is important for correct character representation, particularly when migrating data or ensuring consistent display across different platforms.
> 
> For example, if you store a character like the Euro sign in a latin1 column in MySQL, it will be stored using its Windows-1252 byte representation. If a client application then attempts to interpret this data as strict ISO-8859-1, it might encounter an undefined character or display an incorrect symbol.


```python
def fix_mojibake(mangled_text):
    """
    Fix the mojibake by mapping Windows-1252 specific characters
    back to their byte values, then decoding as UTF-8
    """

    # Windows-1252 character to byte mapping
    # These are characters that exist in Windows-1252 but not in ISO-8859-1
    win1252_to_byte = {
        '\u0152': 0x8C,  # Œ
        '\u0153': 0x9C,  # œ
        '\u0160': 0x8A,  # Š
        '\u0161': 0x9A,  # š
        '\u0178': 0x9F,  # Ÿ
        '\u017D': 0x8E,  # Ž
        '\u017E': 0x9E,  # ž
        '\u0192': 0x83,  # ƒ
        '\u02C6': 0x88,  # ˆ
        '\u02DC': 0x98,  # ˜
        '\u2013': 0x96,  # –
        '\u2014': 0x97,  # —
        '\u2018': 0x91,  # '
        '\u2019': 0x92,  # '
        '\u201A': 0x82,  # ‚
        '\u201C': 0x93,  # "
        '\u201D': 0x94,  # "
        '\u201E': 0x84,  # „
        '\u2020': 0x86,  # †
        '\u2021': 0x87,  # ‡
        '\u2022': 0x95,  # •
        '\u2026': 0x85,  # …
        '\u2030': 0x89,  # ‰
        '\u2039': 0x8B,  # ‹
        '\u203A': 0x9B,  # ›
        '\u20AC': 0x80,  # €
        '\u2122': 0x99,  # ™
    }

    # Build byte array
    byte_array = bytearray()

    for char in mangled_text:
        code = ord(char)

        if char in win1252_to_byte:
            # This is a Windows-1252 specific character
            byte_array.append(win1252_to_byte[char])
        elif code <= 0xFF:
            # This can be represented as a single byte in Latin-1
            byte_array.append(code)
        else:
            # Unknown character - use replacement
            print(f"Warning: Unknown character '{char}' (U+{code:04X})")
            byte_array.append(0x3F)  # ?

    # Now decode as UTF-8
    try:
        result = bytes(byte_array).decode('utf-8')
        return result
    except UnicodeDecodeError as e:
        print(f"Error decoding as UTF-8: {e}")
        print(f"Bytes: {byte_array.hex()}")
        return None

```
