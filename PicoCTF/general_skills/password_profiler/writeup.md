# Challenge Name: Password Profiler

**CTF:** 
**Category: General skills**  
**Difficulty: Low** 
**Points: 100** 
**Date: 29-05-26** 

---

## Description
> We intercepted a suspicious file from a system, but instead of the password itself, it only contains its SHA-1 hash. Using OSINT techniques, you are provided with personal details about the target. Your task is to leverage this information to generate a custom password list and recover the original password by matching its hash.

Download the following files:

userinfo: Contains the personal details.
```
First Name: Alice
Surname: Johnson
Nickname: AJ
Birthdate: 15-07-1990
Partner's Name: Bob
Child's Name: Charlie
```

hash: Contains the SHA-1 hash of the password.

`hash value: 968c2349040273dd57dc4be7e238c5ac200ceac5`

check_password: Script to test passwords against the hash.

```
check_password.py

#!/usr/bin/env python3
import hashlib

HASH_FILE = "hash.txt"
WORDLIST_FILE = "passwords.txt" # wordlist that was generated using CUPP

def load_hash():
    with open(HASH_FILE, "r") as f:
        return f.read().strip()

def crack_password(target_hash):
    with open(WORDLIST_FILE, "r", encoding="utf-8", errors="ignore") as f:
        for password in f:
            password = password.strip()
            if hashlib.sha1(password.encode()).hexdigest() == target_hash:
                return password
    return None

if __name__ == "__main__":
    target_hash = load_hash()
    result = crack_password(target_hash)
    if result:
        print(f"Password found: picoCTF{{{result}}}")
    else:
        print("No match found.")

```

---

## Recon & Initial Observations
From the desciption its clear that we should use this user info to tailor a suitable wordlist using the cupp tool, and then use it to macth the hash. To find the actual password.


---

## Approach / Thought Process
Everything is straight forward. Nothing complex which needs reasoning or thinking. 

---

## Solution

### Step 1 — Run CUPP
Ran the tool, it asks for a set of details about the victim, who's password we are trying to crack. After giving the inputs it genarated a wordlist named "alice.txt".

```bash
python3 cupp.py -i
```

### Step 2 — 
Since the **password_checker.py** takes a wordlist named "password.txt" as the input, I copied the wordlist **alice.txt** generated to the same folder as the **password_checker.py** folder. And ran the **password_checker.py**
```bash
python3 check_password.py 
```

Running the .py file gave the following output:
`Password found: picoCTF{Aj_15901990}`

---

## Tools Used
- CUPP (from github)

---

## Key Takeaway
How personal information about the target can be used to curate a personlaized wordlist to crack the victims password. One shouldnt use personal details in the password, it could be useful to the attacker to crack the password.

---

## Flag
`picoCTF{Aj_15901990}`
