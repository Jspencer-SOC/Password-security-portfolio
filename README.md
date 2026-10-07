## Password Security Portfolio Project
A hands-on demonstration of password-hashing security, benchmarking hash algorithm speed and simulating real cracking attempts, all using invented test data in an isolated VM. Demonstrating a useful understanding of password hashing security through two parts:

## Part 1: Hashing Algorithm Benchmark
Compared MD5, bcrypt, scrypt, and argon2id on speed (hashes/second), showing why slow hashing algorithms resist brute-force attacks far better than general-purpose fast hashes.

## Part 2: Cracking Demo
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
The top portion of benchmark.py imports, the test password list, the timing function (time_it), and the individual hash functions for MD5, bcrypt, scrypt, and argon2id. I added comments explaining what each line does as I learned it. I was going to use a list of passwords but changed it to user input
![Code1](https://github.com/Jspencer-SOC/Password-security-portfolio/blob/49eb906df62e00769260ed1b41827d0bfa6045df/Screenshot/Pictures%20of%20code%201.png)

The remainder of benchmark.py is the ALGORITHMS dictionary mapping algorithm names to their functions, and the main() function that prompts for a test password and prints the results table.
![Code2](https://github.com/Jspencer-SOC/Password-security-portfolio/blob/49eb906df62e00769260ed1b41827d0bfa6045df/Screenshot/Picture%20of%20code%202.png)

## Debugging
Hit a NameError: name 'NUM_TRIALS' is not defined on the first run. Traced it back to a typo I had written Num_trails instead of NUM_TRIALS in the variable definition.
![Error](https://github.com/Jspencer-SOC/Password-security-portfolio/blob/49eb906df62e00769260ed1b41827d0bfa6045df/Screenshot/Error%20Ran%20into.png)




