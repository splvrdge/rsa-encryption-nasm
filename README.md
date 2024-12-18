# RSA Encryption Program in Assembly

This is an educational implementation of RSA (Rivest-Shamir-Adleman) encryption in x86_64 assembly language for macOS. The program demonstrates the basic principles of RSA encryption using small prime numbers.

## What is RSA?

RSA is a public-key cryptosystem widely used for secure data transmission. It is based on the practical difficulty of factoring the product of two large prime numbers.

### Key Components

In this implementation, we use:
- p = 7 (first prime number)
- q = 13 (second prime number)
- n = p × q = 91 (modulus)
- φ(n) = (p-1) × (q-1) = 72 (Euler's totient)
- e = 5 (public exponent)
- d = 29 (private exponent)

### How RSA Works

1. **Key Generation**:
   - Choose two prime numbers (p and q)
   - Calculate n = p × q
   - Calculate φ(n) = (p-1) × (q-1)
   - Choose e (public exponent) such that 1 < e < φ(n) and e is coprime to φ(n)
   - Calculate d (private exponent) such that d × e ≡ 1 (mod φ(n))

2. **Encryption**:
   - c = m^e mod n
   - Where m is the message and c is the ciphertext

3. **Decryption**:
   - m = c^d mod n
   - Where c is the ciphertext and m is the original message

## System Requirements

This implementation is specifically designed for:
- macOS operating system
- x86_64 (64-bit) architecture
- NASM assembler
- macOS system development tools

### Compatibility Notes

This code is not directly portable to other operating systems due to:
- macOS-specific system calls
- macOS-specific binary format (Mach-O)
- macOS-specific linking conventions

To run on other systems (Linux, Windows), the code would need significant modifications to:
- System call numbers and conventions
- Binary format specifications
- Linking commands
- Assembly directives

If you need to run this on another system, consider:
1. Modifying the system calls for your target OS
2. Using appropriate assembler directives for your system
3. Adjusting the build commands for your platform

## Program Features

- Takes a two-digit number as input (0-99)
- Displays RSA parameters (p, q, n, e, d, φ(n))
- Encrypts the input using the public key (e, n)
- Decrypts the ciphertext using the private key (d, n)
- Input validation for digits

## How to Use

1. Assemble the program:
   ```bash
   nasm -f macho64 rsa-encrypt.asm
   ```

2. Link the object file:
   ```bash
   ld -o rsa-encrypt rsa-encrypt.o -macosx_version_min 10.12 -no_pie -L/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/usr/lib -lSystem
   ```

3. Run the program:
   ```bash
   ./rsa-encrypt
   ```

4. Enter two single digits when prompted

## Note

This is a simplified implementation for educational purposes. Real-world RSA:
- Uses much larger prime numbers (typically 2048 bits or larger)
- Includes proper padding schemes
- Uses secure random number generation
- Implements additional security measures

## Example Output

```
RSA Parameters:
  p (first prime) = 7
  q (second prime) = 13
  n (modulus) = 91 (7 × 13)
  e (public exponent) = 5
  d (private exponent) = 29
  φ(n) = 72 = (7-1) × (13-1)
------------------------
Enter first digit (0-9): 5
Enter second digit (0-9): 3
Original number: 53
Encrypted number: 19
Decrypted number: 53
```