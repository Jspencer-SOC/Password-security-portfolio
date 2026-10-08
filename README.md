## Password Security Portfolio Project
A hands-on demonstration of password-hashing security, benchmarking hash algorithm speed and simulating real cracking attempts, all using invented test data in an isolated VM. Demonstrating a useful understanding of password hashing security through two parts:

## Part 1: Hashing Algorithm Benchmark
Compared MD5, bcrypt, scrypt, and argon2id on speed (hashes/second), showing why slow hashing algorithms resist brute-force attacks far better than general-purpose fast hashes.

## Part 2: Cracking Demo (Coming Soon)
I created test accounts two ways: unsalted MD5 vs. salted bcrypt, and attempted to crack both using hashcat/John the Ripper, showing the real-world impact of hashing algorithm choice. All accounts and passwords are invented test data, not real credentials.

## Ground rules followed in this project
I invented all passwords, usernames, and hashes used here for testing purposes. No real accounts, real breach data, or anyone else's credentials were used at any point. All work was done in an isolated Kali Linux VM with no network access to real systems. Nothing in this repository contains a real password, a real API key, or a hash traceable to any real account.

## Creating a Directory
Set up the project folder structure using mkdir and navigated into it with cd, preparing a clean workspace for the Part 1 benchmark script.
![Screenshot/Creating a Dirctory.png](https://github.com/Jspencer-SOC/Password-security-portfolio/blob/49eb906df62e00769260ed1b41827d0bfa6045df/Screenshot/Creating%20a%20Dirctory.png)

## Logging into a separate VM
Created a Python virtual environment (python3 -m venv venv) and activated it (source venv/bin/activate) to keep project dependencies isolated from the system.
![Logging into diff machine](https://github.com/Jspencer-SOC/Password-security-portfolio/blob/49eb906df62e00769260ed1b41827d0bfa6045df/Screenshot/Logging%20into%20a%20sepratee%20VM.png)

## Installing bcrypt
Installed the bcrypt and argon2-cffi libraries inside the virtual environment using pip install, which are needed to run the bcrypt and argon2id hashing functions.
![Installing](https://github.com/Jspencer-SOC/Password-security-portfolio/blob/49eb906df62e00769260ed1b41827d0bfa6045df/Screenshot/Installing%20bcrypt.png)

## Writing Code
The top portion of benchmark.py imports the test password list, the timing function (time_it), and the individual hash functions for MD5, bcrypt, scrypt, and argon2id. I added comments explaining what each line does as I learned it. I was going to use a list of passwords but changed it to user input
![Code1](https://github.com/Jspencer-SOC/Password-security-portfolio/blob/49eb906df62e00769260ed1b41827d0bfa6045df/Screenshot/Pictures%20of%20code%201.png)

The remainder of benchmark.py is the ALGORITHMS dictionary mapping algorithm names to their functions, and the main() function that prompts for a test password and prints the results table.
![Code2](https://github.com/Jspencer-SOC/Password-security-portfolio/blob/49eb906df62e00769260ed1b41827d0bfa6045df/Screenshot/Picture%20of%20code%202.png)

## Debugging
Hit a NameError: name 'NUM_TRIALS' is not defined on the first run. Traced it back to a typo I had written Num_trails instead of NUM_TRIALS in the variable definition.
![Error](https://github.com/Jspencer-SOC/Password-security-portfolio/blob/49eb906df62e00769260ed1b41827d0bfa6045df/Screenshot/Error%20Ran%20into.png)

## First Test
After fixing the typo, ran the script successfully for the first time using the test password "Ilovemycats." Produced a full results table comparing MD5, bcrypt, scrypt, and argon2id
![first test](https://github.com/Jspencer-SOC/Password-security-portfolio/blob/fd496dbd7d7947a769c9099c84dd14db9440b2a0/Screenshot/First%20Test.png)

"Avg time/hash" → bigger number = slower (takes longer per hash)
"Hashes/sec" → bigger number = faster (does more per second)

 ## Why MD5 being fastest is bad

Imagine a hacker has stolen a list of passwords and wants to guess their way back to the real passwords. Those passwords were scrambled with MD5; the hacker can test about 104,000 guesses every second. They could try millions of common passwords within minutes. Even a decent password isn't very safe, because the hacker can just brute-force their way through guesses so quickly. 

## Why bcrypt being slow is good

If those same passwords were scrambled with bcrypt instead, the hacker can only test about 2-3 guesses per second. Here's the difference.
- Guessing 1 million passwords with MD5: takes the hacker under 10 seconds
- Guessing that same 1 million passwords with bcrypt: takes the hacker almost 5 days

## Why scrypt and argon2id were "in the middle
scrypt and argon2id aren't weaker algorithms. It's because I hadn't "turned up the difficulty" to the standard that they're supposed to be.

## Sample Pass Testing
Ran the benchmark again with a different test password ("Samsmith") to confirm the results were consistent and not a one-off fluke.
![Sample pass](https://github.com/Jspencer-SOC/Password-security-portfolio/blob/fd496dbd7d7947a769c9099c84dd14db9440b2a0/Screenshot/Sample%20pass%20testing.png)

## Another Faliure
While trying to retune scrypt's cost parameter to match OWASP recommendations, I set n too high and triggered a ValueError: [digital envelope routines] memory limit exceeded. This showed firsthand that cost parameters can't just be increased arbitrarily; they're constrained by available system memory.
![Error](https://github.com/Jspencer-SOC/Password-security-portfolio/blob/fd496dbd7d7947a769c9099c84dd14db9440b2a0/Screenshot/Another%20Failure.png)

## Tuned Algorithms
Corrected the scrypt and argon2id parameters to somewhat realistic (scrypt N=2^14; argon2id time_cost=2, memory_cost=19000, parallelism=1)
![Tuned](https://github.com/Jspencer-SOC/Password-security-portfolio/blob/fd496dbd7d7947a769c9099c84dd14db9440b2a0/Screenshot/Switched%20Algorthms%20to%20more%20real%20base.png)

## Does Password Complexity Affect Hashing Speed
To test this, I ran the benchmark on two very different test passwords: a long, complex one ("Ticketmaster223!!!") and a short, simple one ("Samsmith")
![Compar](https://github.com/Jspencer-SOC/Password-security-portfolio/blob/fd496dbd7d7947a769c9099c84dd14db9440b2a0/Screenshot/Comparison%20of%20easy%20password%20Vs.%20harder%20password.png)

**Result**: bcrypt, scrypt, and argon2id ran at basically the same speed for both passwords. MD5 was the only one that changed, running faster on the shorter password.

**Why**: bcrypt, scrypt, and argon2id are controlled by a fixed "cost factor" (rounds, memory, time) that dominates how long they take; the password itself barely matters. MD5 has no cost factor at all, so it's doing so little work that tiny input differences are enough to show up in the timing.

**Why this matters**: password strength and hashing algorithm choice are two separate defenses:

- A strong password means an attacker needs more guesses.
- A slow hashing algorithm means each guess takes longer.

A slow algorithm doesn't save a weak password like "123456", and a strong password doesn't save a fast algorithm like MD5. You need both.


