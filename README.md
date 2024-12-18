# RSA Encryption Program in Assembly

This is an educational implementation of RSA (Rivest-Shamir-Adleman) encryption in x86_64 assembly language for macOS. The program demonstrates the basic principles of RSA encryption using small prime numbers.

Sumbitted by Francis James Lagang, Margaret Grace Docdoc, and Simone Montañez. Final Project for CS 3103, DCISM, University of San Carlos. December 2024.

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

This implementation has versions for:
- macOS operating system (x86_64 architecture)
- Linux (x86_64 architecture)
- ARM64 architecture (via Docker)

### Architecture Support

The repository includes three versions of the code:
- `rsa-encrypt.asm`: Original version for macOS (x86_64)
- `rsa-encrypt-linux.asm`: Linux version for Docker container (x86_64)
- `rsa-encrypt-arm64.asm`: ARM64 version for Docker container (ARM64)

All versions implement the same RSA encryption algorithm but use different assembly instructions and system calls appropriate for their respective architectures.

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

## Running with Docker

You can run this program on any system that has Docker installed, including ARM64-based systems like Apple Silicon Macs:

### For x86_64 systems:
1. Build the Docker image:
   ```bash
   docker build -f Dockerfile.x86_64 -t rsa-encrypt-x86 .
   ```

2. Run the container:
   ```bash
   docker run -it rsa-encrypt-x86
   ```

### For ARM64 systems (e.g., Apple Silicon Macs):
1. Build the Docker image:
   ```bash
   docker build -t rsa-encrypt-arm .
   ```

2. Run the container:
   ```bash
   docker run -it rsa-encrypt-arm
   ```

The Docker containers will work on any system that has Docker installed, regardless of the operating system. The ARM64 version is specifically optimized for ARM-based systems like Apple Silicon Macs, ensuring native performance on these platforms.

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

### Interesting Case: Fixed Points

In this implementation, you might notice that some numbers encrypt to themselves. For example:
```
Original number: 21
Encrypted number: 21
Decrypted number: 21
```

These numbers are called "fixed points" of the RSA function. They occur when m^e mod n = m. This is one of the reasons why real-world RSA implementations:
1. Use much larger prime numbers to reduce the frequency of fixed points
2. Apply padding schemes that make the input more random
3. Never encrypt raw numbers directly

## Example Outputs

Here are different examples showing how the encryption works with various inputs:

### Example 1 - Regular Case
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
Encrypted number: 79
Decrypted number: 53
```

### Example 2 - Fixed Point Case
```
RSA Parameters:
  p (first prime) = 7
  q (second prime) = 13
  n (modulus) = 91 (7 × 13)
  e (public exponent) = 5
  d (private exponent) = 29
  φ(n) = 72 = (7-1) × (13-1)
------------------------
Enter first digit (0-9): 2
Enter second digit (0-9): 1
Original number: 21
Encrypted number: 21
Decrypted number: 21
```

### Example 3 - Another Case
```
RSA Parameters:
  p (first prime) = 7
  q (second prime) = 13
  n (modulus) = 91 (7 × 13)
  e (public exponent) = 5
  d (private exponent) = 29
  φ(n) = 72 = (7-1) × (13-1)
------------------------
Enter first digit (0-9): 1
Enter second digit (0-9): 1
Original number: 11
Encrypted number: 72
Decrypted number: 11
```

These examples demonstrate:
1. Regular encryption where the encrypted number is different (53 → 79 → 53)
2. A fixed point where the number encrypts to itself (21 → 21 → 21)
3. Another regular case with different values (11 → 72 → 11)
