### ❓ Interview Q&A

1. How can browser extensions pose security risks?

    * They can access page data, inject scripts, intercept TLS traffic via proxies, or use native messaging to run code.


2. What permissions should raise suspicion?

    * Global site access, native messaging, file system access, and permission to modify network requests.


3. How to safely install extensions?

    * Use official web stores, check publisher, reviews, minimal permissions, and prefer open-source extensions.


4. What is extension sandboxing?
 
    * Browser-enforced isolation that limits extension access and privileges; varies across browsers.


5. Can extensions steal passwords?

     * Yes, if they can read pages or intercept form submissions, they can steal credentials.


6. How to update extensions securely?
     
     * Use browser auto-update from official store; avoid sideloaded updates unless from trusted signed sources.


7. Difference between extensions and plugins?
    
     * Plugins were NPAPI-based native code (deprecated). Extensions are browser-managed JS-based add-ons.


8. How to report malicious extensions?

     * Use the browser store’s reporting mechanism (e.g., Chrome Web Store “Report abuse”, Firefox Add-ons report).
