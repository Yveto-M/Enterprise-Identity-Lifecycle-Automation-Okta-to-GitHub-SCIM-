**Enterprise Identity Architecture: Okta to GitHub (SAML + SCIM)**

**Role:** Identity Architect / Engineer
**Technologies:** Okta Workforce Identity, GitHub Enterprise Cloud, SAML 2.0, SCIM 2.0
**Status:** Completed & Verified

&nbsp;

**🔹 Executive Summary**

This project architects a complete Zero Trust Identity Lifecycle for a software development organization. By integrating Okta (IdP) with GitHub Enterprise (SP), I replaced manual, insecure credential management with a fully federated and automated identity stack.


**Key Engineering Wins:**

**Federated Security:** Enforced centralized MFA and eliminated local passwords via SAML 2.0.

**Automated Lifecycle:** Achieved "Zero Touch" provisioning for Joiners via SCIM 2.0.

**Kill Switch Enforcement:** Automated immediate access revocation for terminated users, closing a critical insider threat gap.



**🔐 Phase 1: Federated Authentication (SAML 2.0)**

**Objective: Establish a cryptographically trusted bridge between the Identity Provider (Okta) and Service Provider (GitHub) to enforce SSO.**

**1.1 Application Catalog Integration**

Initiated the integration using the verified Okta Integration Network (OIN) connector for GitHub Enterprise to ensure vendor-supported compatibility.

<img width="1620" height="583" alt="okta-to-github-01" src="https://github.com/user-attachments/assets/84502d21-f7e8-4bb1-a0f4-40e4a52c02f0" />
Figure 1: Selecting the verified GitHub Enterprise Cloud integration from the OIN catalog.




**1.2 IdP Configuration (The Handshake)**

Configured the SAML parameters within Okta. This step generates the Metadata URL and X.509 Certificate required to sign assertions.

<img width="1021" height="755" alt="SAML-config-okta-2" src="https://github.com/user-attachments/assets/0cae7ed5-4e2d-43da-9186-f2b33e5d4bb9" />
Figure 2: Okta Sign-On settings showing the generated Metadata URL and setup instructions.




**1.3 Service Provider Setup (GitHub)**

Applied the Identity Provider's trust parameters into the GitHub Organization's security settings to define Okta as the source of authentication.

<img width="1030" height="350" alt="properties-to-intigrate-github-3" src="https://github.com/user-attachments/assets/72d9d684-c1c8-42cd-b41b-43885347c805" />
Figure 3: Inputting the Sign-on URL, Issuer URI, and Public Certificate into GitHub.




**1.4 Preparing Authentication Security**

Navigated to the GitHub Authentication Security panel to prepare for the SSO enforcement switch-over.

<img width="1217" height="244" alt="github-auth-config-4" src="https://github.com/user-attachments/assets/5ed9b33f-cce8-41c1-beed-f343e6f12e5a" />
Figure 4: Accessing the Authentication Security controls in the GitHub Org settings.




**1.5 Enabling the Trust**

Activated the SAML configuration on the Service Provider side, requiring valid SAML assertions for access.

<img width="1194" height="521" alt="guthub-anable-saml-5" src="https://github.com/user-attachments/assets/cc66325e-04f0-46ee-ad89-74c304913d22" />
Figure 5: Enabling SAML authentication and validating the Sign-on URL.




**1.6 Validation & Testing**

Before enforcing the policy, a test authentication request was sent to verify the handshake and prevent lockout.

<img width="963" height="654" alt="github-test-saml-config-6" src="https://github.com/user-attachments/assets/a0c3c447-9635-4780-9c09-9008963027be" />
Figure 6: Initiating the "Test SAML Configuration" check.




**1.7 Success Confirmation**

The Identity Provider successfully authenticated the user and passed the correct attributes (NameID) back to the Service Provider.

<img width="986" height="335" alt="SAML-config-successful-7" src="https://github.com/user-attachments/assets/bc95eab0-dcb9-4e46-a64c-e8e6b80d083f" />
Figure 7: Successful SAML handshake verification ("Passed").





**⚙️ Phase 2: SCIM Lifecycle Automation**

**Objective: Automate the Joiner/Mover/Leaver (JML) process to eliminate manual administration and reduce onboarding time.**

**2.1 Initiating API Integration**

Unlike SAML (Certificate-based), SCIM requires an API Token to authorize the IdP to make changes to the SP's user directory.

<img width="816" height="611" alt="configure api-8" src="https://github.com/user-attachments/assets/641b307c-1e12-42df-b3ab-ef73d9f0e360" />
Figure 8: Initiating the API integration for SCIM provisioning.




**2.2 Establishing the OAuth Connection**

Selected the authentication method to bind the Okta tenant to the GitHub Organization.

<img width="962" height="665" alt="autho-with-github-9" src="https://github.com/user-attachments/assets/642f350d-db3b-4448-9e63-d2e9ba94317f" />
Figure 9: Authenticating with GitHub Enterprise Cloud.




**2.3 Authorization Grant**

Granted Okta the necessary OAuth scopes to manage users and teams within the GitHub Organization.

<img width="559" height="610" alt="autho-complete-10" src="https://github.com/user-attachments/assets/4ceae5e6-0ef6-473b-883b-0592c14adee0" />
Figure 10: Authorizing the OktaOAN integration application.




**2.4 Enabling CRUD Operations (The Automation Logic)**

Configured the provisioning rules. Explicitly enabled Create Users, Update Attributes, and Deactivate Users to ensure full lifecycle control.

<img width="957" height="710" alt="enable crud-action-11" src="https://github.com/user-attachments/assets/fc123f9e-e754-4613-bd44-671370deac63" />
Figure 11: Enabling Create, Update, and Deactivate user workflows.




**🧪 Phase 3: Execution & Verification (The "Ghost User" Test)**

**Objective: Prove that a user created in Okta appears in GitHub without human intervention.**

**3.1 The "Joiner" Event (Creation)**

Created a test user Ghost Malone in the Okta Directory to simulate a new hire.

<img width="1021" height="381" alt="create-app-user-12" src="https://github.com/user-attachments/assets/31bf12c2-5b8d-4915-aac2-769ae2a79403" />
Figure 12: User created in Source System (Okta) in a pending state.




**3.2 Entitlement Assignment**

Assigned the GitHub application to the user, triggering the SCIM "Create" event.

<img width="616" height="392" alt="assign-user-to-org-13" src="https://github.com/user-attachments/assets/f2abcf90-190f-4f6b-93a8-14b4d694802b" />
Figure 13: Assigning the GitHub application to the user profile.




**3.3 Activation Workflow**

Manually activated the user to bypass the email verification loop for lab testing purposes.




<img width="618" height="331" alt="mannually activate user-14" src="https://github.com/user-attachments/assets/5c00e74c-cb86-40fd-805a-b835a37f376c" />
Figure 14: Forcing user activation to trigger immediate provisioning.

**3.4 Automated Provisioning Result**

Verified that the user account (invite) appeared in the target system (GitHub) immediately.

<img width="1268" height="282" alt="github-invite-confirmed-15" src="https://github.com/user-attachments/assets/3eccb60c-6562-476b-bbab-336a339198b9" />
Figure 15: SCIM successfully pushes the new user invite to GitHub immediately.




**3.5 The "Leaver" Event (Kill Switch)**

Deactivated the user in Okta to simulate an employee termination.

<img width="1035" height="487" alt="deactivated-user-okta-16" src="https://github.com/user-attachments/assets/fa0431d9-dd45-4003-b470-59deac6063f7" />
Figure 16: Admin deactivates user in Source System (Okta).




**3.6 Access Revocation Verification**

Confirmed that the user was removed or suspended in the target system.

<img width="1280" height="263" alt="deactivated-user-on-github-17" src="https://github.com/user-attachments/assets/a58d3b78-5cd2-41ca-bbe2-b96868f7594e" />
Figure 17: The SCIM 'Deactivate' signal successfully removes access/invites in the target system.




**📊 Project Outcomes**

**Security:** Enforced centralized authentication policy (MFA) via SAML federation.

**Risk Reduction:** Automated de-provisioning eliminates the risk of "Zombie Accounts" retaining access after termination.

**Operational Efficiency:** Reduced onboarding time from ~15 minutes (manual) to <30 seconds (automated).
