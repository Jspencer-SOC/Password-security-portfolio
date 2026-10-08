## Password Security Portfolio Project
A hands-on demonstration of password-hashing security. I benchmarked four hashing algorithms by speed to show why slow, purpose-built password hashes resist brute-force attacks far better than general-purpose fast hashes. All work uses made-up test data in an isolated VM

## Part 1: Hashing Algorithm Benchmark
Compared MD5, bcrypt, scrypt, and argon2id on speed (hashes/second), showing why slow hashing algorithms resist brute-force attacks far better than general-purpose fast hashes.

## Part 2: Cracking Demo (Future Project)
Creating test accounts two ways: unsalted MD5 vs. salted bcrypt, and attempting to crack both using hashcat/John the Ripper, showing the real-world impact of hashing algorithm choice. All accounts and passwords are invented test data, not real credentials.

## Rules followed in this project
I invented all passwords, usernames, and hashes used here for testing purposes. No real accounts, real breach data, or anyone else's credentials were used at any point. All work was done in an isolated Kali Linux VM with no access to real accounts or systems. Nothing in this repository contains a real password, a real API key, or a hash traceable to any real account.

## Tools and environment
- Kali Linux (VM)
- Python 3 (virtual environment)
- Libraries: bcrypt, argon2-cffi

## Creating a Directory
Set up the project folder structure using mkdir and navigated into it with cd, preparing a clean workspace for the Part 1 benchmark script
![Screenshot/Creating a Dirctory.png](https://github.com/Jspencer-SOC/Password-security-portfolio/blob/49eb906df62e00769260ed1b41827d0bfa6045df/Screenshot/Creating%20a%20Dirctory.png)

## Logging into a separate VM
Created a Python virtual environment (python3 -m venv venv) and activated it (source venv/bin/activate) to keep project dependencies isolated from the system.
![Logging into diff machine](https://github.com/Jspencer-SOC/Password-security-portfolio/blob/49eb906df62e00769260ed1b41827d0bfa6045df/Screenshot/Logging%20into%20a%20sepratee%20VM.png)

## Installing dependencies
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

### Results

## First Test
After fixing the typo, ran the script successfully for the first time using the test password "Ilovemycats." Produced a full results table comparing MD5, bcrypt, scrypt, and argon2id
![first test](https://github.com/Jspencer-SOC/Password-security-portfolio/blob/fd496dbd7d7947a769c9099c84dd14db9440b2a0/Screenshot/First%20Test.png)

| Algorithm | Avg time/hash | Hashes/sec |
|-----------|---------------|------------|
| MD5       | ~0.010 ms   | ~104,000/sec |
| bcrypt    | ~0.413 s   | ~2.4/sec   |
| scrypt    | ~0.054 s   | ~18.4/sec  |
| argon2id  | ~0.111 s   | ~9.0/sec   |


"Avg time/hash" → bigger number = slower (takes longer per hash)
"Hashes/sec" → bigger number = faster (does more per second)



## Why MD5 being fastest is bad
Imagine an attacker has stolen a list of password hashes and wants to guess the original passwords. If they were hashed with MD5, my benchmark shows about 104,000 guesses per second for my first test password. That figure comes from my single-threaded Python script on a VM. Real attackers use GPUs that can test billions of MD5 hashes per second, so the real-world gap is even larger. At my measured speed, they could try millions of common passwords in minutes, and even a decent password isn't good.

## Why bcrypt being slow is good

If the same passwords were hashed with bcrypt, the attacker can test only about 2-3 guesses per second (on my VM). The difference:
- Guessing 1 million passwords with MD5: takes the hacker under 10 seconds
- Guessing that same 1 million passwords with bcrypt: takes the hacker almost 5 days

## Why scrypt and argon2id were "in the middle
scrypt and argon2id are not weaker algorithms. My first run used cost settings below recommended levels, so bcrypt  was faster than it should be. I corrected this in the "Tuned Algorithms" section.

## Sample Pass Testing
Ran the benchmark again with a different test password ("Samsmith") to confirm the results were consistent and not a one-off fluke.
![Sample pass](https://github.com/Jspencer-SOC/Password-security-portfolio/blob/fd496dbd7d7947a769c9099c84dd14db9440b2a0/Screenshot/Sample%20pass%20testing.png)

## Another Faliure
While trying to retune scrypt's cost parameter to match OWASP recommendations, I set scrypt's N too high and got a ValueError: [digital envelope routines] memory limit exceeded. This showed firsthand that cost parameters can't just be increased arbitrarily; they're constrained by available system memory.
![Error](https://github.com/Jspencer-SOC/Password-security-portfolio/blob/fd496dbd7d7947a769c9099c84dd14db9440b2a0/Screenshot/Another%20Failure.png)

## Tuned Algorithms
Corrected the scrypt and argon2id parameters to realistic (scrypt N=2^14; argon2id time_cost=2, memory_cost=19000, parallelism=1).The argon2id settings match OWASP's minimum recommended configuration. The scrypt setting is at the low end of OWASP's suggested range, chosen to fit my VM's memory
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

## Conculsion
Fast hashes like MD5 are a poor choice for storing passwords because they let an attacker test guesses extremely quickly. Slow, tunable algorithms (bcrypt, scrypt, argon2id) make each guess expensive, but their cost settings must be tuned to the hardware. Good password security combines a strong password with a slow hashing algorithm.

## Limitations
- Results come from one Kali VM, so absolute speeds will differ on other hardware.
- The benchmark is single-threaded Python with no GPU. Real attackers are much faster.
- Each algorithm was timed with a limited number of trials, so small differences are not statistically precise.



## References

Defuse Security. (2018). Secure Salted Password Hashing - How to do it Properly. Crackstation.Net. https://crackstation.net/hashing-security.htm

OWASP. (2021). Password Storage - OWASP Cheat Sheet Series. Cheatsheetseries.Owasp.Org. https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html

Python. (2024). hashlib — Secure hashes and message digests — Python 3.8.4rc1 documentation. Docs.Python.Org. https://docs.python.org/3/library/hashlib.html

developers, T. P. C. A. (2025, February 28). bcrypt: Modern password hashing for your software and your servers. PyPI. https://pypi.org/project/bcrypt/

Guilliano Molaire. (2024, December 2). Bcrypt: Why It’s a Preferred Password Hashing Algorithm. Skycloak - Managed Keycloak | IAM as a Service. https://skycloak.io/blog/bcrypt-basics-why-its-a-preferred-password-hashing-algorithm/


argon2-cffi 21.3.0 documentation. (n.d.). Argon2-Cffi.Readthedocs.Io. Retrieved October 7, 2026, from https://argon2-cffi.readthedocs.io/en/stable/


