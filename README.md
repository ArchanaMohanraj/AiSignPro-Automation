AiSign Pro – QA Automation (Playwright + JavaScript, POM)
Automated end-to-end test for the AiSign Pro staging site: https://cpastaging.aisignpro.com/
Prerequisites
• Node.js 18 or higher (check with node -v)
• Google Chrome or Chromium (installed in step 2 below)
• Internet access to the staging site
How to run (3 commands)
Open a terminal inside the AiSignPro-Automation folder (the folder that contains package.json) and run:
npm install
npx playwright install chromium
npm test
To open the HTML report afterwards:
npm run test:report
The report is generated in the reports/ folder. Open it with the command above rather than double-clicking index.html, so screenshots and traces load correctly.
What the test covers (TC01 – TC16)
The whole flow runs as one test with 16 steps, so the HTML report shows each step in order:
• TC01: Login with invalid mobile number
• TC02: Login with invalid OTP
• TC03: Login with valid credentials
• TC04: Navigate to Customers
• TC05: Duplicate phone validation
• TC06: Duplicate email validation
• TC07: Duplicate phone and email validation
• TC08: Create a new customer
• TC09: Assign document and send agreement email
• TC10: Verify customer details in the grid
• TC11: Expand customer row and inspect the assigned document
• TC12: Verify preview document and capture signature position
• TC13: Verify download documents popup
• TC14: Download agreement PDF
• TC15: Verify downloaded PDF file
• TC16: Logout
A new unique customer is generated on every run, so the test can be run repeatedly.
Project structure
• tests/ – test spec (customer-agreement.spec.js)
• pages/ – Page Object Model classes (LoginPage, CustomerPage, AssignAgreementPage)
• test-data/ – customerData.json (login and duplicate-customer data)
• evidence/ – screenshots and the downloaded PDF used as bug proof
• reports/ – generated Playwright HTML report
• playwright.config.js – configuration (base URL, reporter, timeouts)
Bugs found
BUG-01 – Signature position mismatch in preview The signature in the View Attachment preview appears at a different position from where it was placed. Evidence: evidence/BUG-01-preview-document.png, evidence/BUG-01-signature-alignment.png
BUG-02 – Signature missing in downloaded attachment The signature is missing in the downloaded PDF. Evidence: evidence/BUG-02-downloaded-agreement.pdf, evidence/BUG-02-signature-missing-in-pdf.txt
Known limitations
• The Mailinator inbox cannot be accessed from automation, so the received mail subject and the signing link are not verified. This is documented as a limitation.
• TC15 verifies that the downloaded PDF exists, is not empty and has a valid PDF header. It does not automatically verify that the signature is visible inside the PDF; that was confirmed manually (see BUG-02).
• The browser runs in headed mode (headless: false) for local runs, so a Chrome window will open during the test.
Troubleshooting
• "Cannot find module @playwright/test": you skipped npm install, or you are in the wrong folder.
• "Executable doesn't exist": run npx playwright install chromium.
• "npm ERR! enoent package.json": open the terminal inside the AiSignPro-Automation folder, not its parent.
• Timeouts or failures at login: check that the staging site is reachable and the test data in test-data/customerData.json is still valid.
