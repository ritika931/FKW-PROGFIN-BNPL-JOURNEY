# FKW-PROGFIN-BNPL-JOURNEY

Clickable prototype of the Flipkart Wholesale × Progfin BNPL onboarding journey, from Apply Now to an active credit line. It follows the workflow diagram and the Progfin Pre-onboarding API v4 doc, and shows which Progfin API is called at each step.

It is a single static page (`index.html`) with no build step and no dependencies. All data is sample data and nothing is sent anywhere.

## Using it

- **Tech notes** (toggle, top right): buttons that call an API are outlined in purple; hover to see the method, endpoint and API number. Loaders list the calls running in the background, and prefilled fields are tagged with the API that filled them. Turn it off for a clean customer demo.
- **Progfin API calls** (right panel): every call with its request and the response from the API doc, including error codes and T.3 events.
- **Scenario** (left panel): switch outcomes for entity type, PAN check, CKYC, DigiLocker, selfie, GSTIN, penny drop, approval and KFS.
- **Jump to a step**: start at any stage with earlier steps assumed done.
- OTPs (mobile and CKYC): `123456`. Different-CKYC-mobile scenario: first 6 digits `987650`.

## Journey and APIs

| Step | APIs |
|---|---|
| Onboarding (mobile, PAN, entity type) | 0.1 Onboard |
| Mobile OTP | 1.1 Send OTP, 1.2 Verify OTP |
| One-step KYC (CKYC) | 1.5 Search, 1.6 Send OTP, 1.7 Submit Mobile, 1.8 Verify OTP, 1.9 Resend OTP, 1.10 Status, T.3 `CKYC_IDENTITY_CONFIRMED` |
| DigiLocker fallback | 1.3 Generate Link, 1.4 Status |
| Manual Aadhaar upload (last resort) | D.1 `AADHAAR_FRONT`, `AADHAAR_BACK` |
| Live selfie | D.1 `SELFIE` |
| Personal details | M.1 States, M.2 Cities, 2.2 Get (when needed), 2.1 Submit |
| Business details | 3.1 Verify GSTIN, 3.2 Submit |
| Business proofs | D.1 (business documents) |
| Bank account | 4.1 Verify & Save (penny drop) |
| Approval | 5.1 Application Approval, 5.2 Approved Limit |
| Key Fact Statement | 6.1 Generate, 6.2 Get (poll) |
| Agreement | 7.1 eSign Status, 7.2 eNACH, 7.3 Enhance Limit |

DigiLocker, e-Sign and e-NACH are external pages; the prototype stubs them with a "Simulate" button.

Rules marked "our rule" or "partner rule" in the log (OTP cool-offs, DigiLocker retries, the 3-try CKYC mobile limit) come from the workflow diagram, not the API doc, and still need confirming with Progfin.

The prototype does not include the PAN upload (D.1 `PAN`) that the API doc lists before CKYC Search, following the current workflow diagram.

## Deploying

Vercel serves `index.html` as a static site. No framework preset or build command is needed.
