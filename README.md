# SHA-256 Cryptographic Hash Implementer

This repository contains a low-level, manual implementation of the SHA-256 cryptographic hashing algorithm written entirely in C. It was developed to build a deep understanding of cryptographic primitives, byte-level data manipulation, and secure memory handling without relying on external cryptography libraries.

## Overview

The program takes a string input via the command line and outputs its corresponding 256-bit (32-byte) SHA-256 hash in hexadecimal format. It strictly follows the FIPS 180-4 specification for the Secure Hash Standard.

## Implementation Details & Reference

This implementation was coded directly from the official National Institute of Standards and Technology (NIST) specification paper:
* **FIPS PUB 180-4: Secure Hash Standard (SHS)**

Working directly from the mathematical definitions and pseudocode provided in the standard, the logic was manually translated into C to ensure a strict, dependency-free implementation of the `SIG0`, `SIG1`, `EP0`, and `EP1` transformation functions and the 64-round compression loop.

## Architecture & Logic

The algorithm processes the input through four primary stages, implemented as distinct functions:

1. **`pad_block`:** Handles message padding. It appends a `1` bit (as `0x80`), pads with zeros until the message length is congruent to 448 bits modulo 512, and then appends the original message length as a 64-bit big-endian integer.
2. **`parse_block`:** Converts the padded 8-bit byte array into 16 32-bit words, handling big-endian conversion.
3. **`expand_block`:** Expands the initial 16 words into a 64-word message schedule (`W` array) using the SHA-256 `SIG0` and `SIG1` transformation functions.
4. **`compress_block`:** The core compression loop. It updates the 8 working variables (`a` through `h`) over 64 rounds using the message schedule, cryptographic constants (`K`), and nonlinear functions (`Ch`, `Maj`, `EP0`, `EP1`).

## Limitations

Currently, this implementation only supports inputs that fit within a single 512-bit (64-byte) block (meaning the input string must be strictly less than 56 bytes). It does not yet implement the chunking loop required to process multi-block inputs or continuous file streams.

## Usage

1. **Compile:** Compile the code using standard GCC:
   ```bash
   gcc sha256.c -o sha256
2. **Run:**
3. ```bash
   ./sha256 "hello world"
