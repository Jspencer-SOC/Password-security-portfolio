## Password Security Portfolio Project
A hands-on demonstration of password-hashing security, benchmarking hash algorithm speed and simulating real cracking attempts, all using invented test data in an isolated VM. Demonstrating a useful understanding of password hashing security through two parts:

## Part 1: Hashing Algorithm Benchmark
Compared MD5, bcrypt, scrypt, and argon2id on speed (hashes/second), showing why slow hashing algorithms resist brute-force attacks far better than general-purpose fast hashes.

## Part 2: Cracking Demo
Hashed invented test accounts two ways: unsalted MD5 vs. salted bcrypt, and attempted to crack both using hashcat/John the Ripper, showing the real-world impact of hashing algorithm choice. All accounts and passwords are invented test data, not real credentials.

## Ground rules followed in this project
I invented all passwords, usernames, and hashes used here for testing purposes. No real accounts, real breach data, or anyone else's credentials were used at any point. All work was done in an isolated Kali Linux VM with no network access to real systems. Nothing in this repository contains a real password, a real API key, or a hash traceable to any real account.



