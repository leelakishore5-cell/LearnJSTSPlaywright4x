# Salesforce Login Test Plan

## 1. Test Plan ID and Title

| Field | Value |
| --- | --- |
| Test Plan ID | TP-SF-LOGIN-001 |
| Title | Salesforce Login UI and Authentication Test Plan |
| Version | 1.0 |
| Status | Draft — pending confirmation of open questions and approval |
| Prepared by | QA |
| Application | Salesforce CRM login |
| Requested URL | https://login.salesforce.com/?locale=in |

## 2. Objective and References

### Objective

Define a traceable, risk-based plan to verify the Salesforce login experience for valid and invalid credentials, required-field handling, and the reported Remember Me control. Establish the prerequisites and observable outcomes needed before tests are implemented or executed.

This document is a plan, not evidence that the application or any test has been run. The supplied example locators and behaviors have not been verified against the current page.

### Source references

- `00_RICE_POT_FullForm.md` — RICE-POT sections and requested Salesforce login automation constraints.
- `01_RICE_POT_Prompt.md` — requested role, login context, Page Object expectations, XPath-only constraint, and deliverable expectations.
- `02_Problem_Statement.md` — Selenium, Java, Maven, and TestNG framework objective.
- `03_Anti_Hallucinations.md` — version grounding, current API usage, valid imports, and self-verification rules.
- `04_RICE_POT_Generic_QA_Template.md` — Profile B test-plan sections, example limitations, and final review checklist.

### RICE-POT application

| Element | Applied to this plan |
| --- | --- |
| Role | Senior QA lead experienced in CRM authentication and test planning. |
| Instructions | Plan traceable testing for login risks; distinguish confirmed input from proposals; do not claim execution. |
| Context | Salesforce login URL and controls reported in the source prompt; environment details and confirmed outcomes are not supplied. |
| Examples | Use the source prompt's login controls and the generic template's traceability-row format as examples, not as verified behavior. |
| Parameters | Focus on functional login and related regression; use measurable proposed exit criteria subject to approval; automation stack is Java, Selenium, Maven, and TestNG. |
| Output | This single Markdown test plan, using Profile B's sections. |
| Tone | Technical, precise, and readable by QA, engineering, and product stakeholders. |

## 3. In Scope and Out of Scope

### In scope

- Verify the login page loads at the approved test URL and exposes the expected username/email, password, login, and Remember Me controls.
- Verify authentication with a provisioned, active test account and valid credentials.
- Verify authentication is denied for an invalid username and for an incorrect password.
- Verify required-field behavior for missing username, missing password, and both fields missing.
- Verify Remember Me behavior after its exact purpose and acceptance criteria are confirmed.
- Verify the page remains usable and the user remains unauthenticated after a failed login.
- Re-run the approved login cases as a focused regression suite after changes to the login UI or authentication integration.
- Assess whether the approved automation stack and browser can exercise the agreed scenarios reliably.

### Out of scope

- Account creation, password reset, account recovery, and user administration.
- MFA, SSO, identity-provider federation, CAPTCHA, lockout thresholds, and session-expiry behavior, except where they block or alter the agreed login scenarios. These require separate requirements and test coverage.
- Penetration testing, security certification, load/performance testing, accessibility certification, and broad compatibility testing.
- Testing against real customer accounts or production data.
- Implementing automation code as part of this test-plan deliverable.

### Scope decision requiring approval

The source prompt asks for thorough valid/invalid UI coverage and mentions Remember Me. Its filled automation example narrows automation to two scripts (one valid and one incorrect-password case) and excludes Remember Me. This plan includes Remember Me and additional functional scenarios in planned coverage. Before automation is commissioned, approve whether the deliverable is the broader test suite or the example's two-script limit.

## 4. Requirements and Planned Coverage

The source documents give a feature description and example locators, not approved product acceptance criteria. The requirement IDs below are planning IDs created for traceability; they must be reviewed by the product owner or application owner.

| Requirement | Basis and planned scenario | Test type | Priority | Observable expected result | Data / prerequisite |
| --- | --- | --- | --- | --- | --- |
| REQ-LOGIN-01 | The reported login page opens and presents username/email, password, Login, and Remember Me controls. | UI / smoke | High | Page is reachable in the approved environment; each required control is visible and usable. | Approved URL, browser, and locator confirmation. |
| REQ-LOGIN-02 | Active account signs in with valid credentials. | Positive functional / integration | Critical | A product-approved authenticated-state signal is observed; the test does not infer success solely from a click or URL change. | Provisioned, authorized test account; approved post-login assertion; MFA/SSO expectations. |
| REQ-LOGIN-03 | Incorrect password does not authenticate an active account. | Negative functional | Critical | A product-approved authentication-failure signal is observed and no authenticated state is created. | Authorized test account and approved invalid-password assertion. |
| REQ-LOGIN-04 | Unknown or invalid username does not authenticate. | Negative functional | High | Authentication is denied and no authenticated state is created. | Synthetic or explicitly approved username; approved failure assertion. |
| REQ-LOGIN-05 | Missing username does not authenticate. | Validation | High | Submission is prevented or a product-approved validation/failure state is displayed; no authenticated state is created. | Empty username and otherwise approved test input. |
| REQ-LOGIN-06 | Missing password does not authenticate. | Validation | High | Submission is prevented or a product-approved validation/failure state is displayed; no authenticated state is created. | Approved username and empty password. |
| REQ-LOGIN-07 | Both required fields missing does not authenticate. | Validation | High | Submission is prevented or a product-approved validation/failure state is displayed; no authenticated state is created. | Empty username and password. |
| REQ-LOGIN-08 | Remember Me behaves according to its specified purpose. | Functional / regression | Medium | The confirmed behavior is retained across the agreed browser restart/session boundary; secrets are not exposed or persisted contrary to policy. | Product definition of Remember Me, consent expectations, and approved test account. |
| REQ-LOGIN-09 | Login controls and error/validation states remain usable and understandable. | UI / usability smoke | Medium | Controls can be reached and operated; failure does not leave the page in a broken or authenticated state. | Approved browser, supported viewport, and error-state requirements. |

### Planned test scenarios

| Test ID | Scenario | Key steps | Expected result |
| --- | --- | --- | --- |
| TC-LOGIN-001 | Login page smoke | Open the approved URL; inspect the four reported controls. | Page and controls are available and usable. |
| TC-LOGIN-002 | Valid login | Enter provisioned valid credentials; submit; inspect the approved authenticated-state signal. | User is authenticated as defined by the application owner. |
| TC-LOGIN-003 | Incorrect password | Enter an approved account identifier and incorrect password; submit; inspect failure and authentication state. | Login is denied; failure is observable; user remains unauthenticated. |
| TC-LOGIN-004 | Invalid or unknown username | Enter an approved synthetic/invalid username and a non-secret password; submit. | Login is denied; user remains unauthenticated. |
| TC-LOGIN-005 | Empty username | Leave username empty, enter approved test input in password, submit. | Required-field or approved failure behavior; no authenticated state. |
| TC-LOGIN-006 | Empty password | Enter approved test username, leave password empty, submit. | Required-field or approved failure behavior; no authenticated state. |
| TC-LOGIN-007 | Both fields empty | Submit with both fields empty. | Required-field or approved failure behavior; no authenticated state. |
| TC-LOGIN-008 | Remember Me selected | Select Remember Me and sign in with an approved test account; restart/reopen the browser at the agreed boundary and inspect the documented behavior. | Only the specifically approved remembered state persists; credentials or session tokens are not exposed. |
| TC-LOGIN-009 | Remember Me not selected | Sign in without selecting Remember Me; restart/reopen the browser at the agreed boundary and inspect behavior. | No remembered state persists beyond the behavior defined by the application owner. |

Do not use SQL-like input as proof of SQL-injection protection. Security testing requires a separately authorized scope and appropriate security acceptance criteria.

## 5. Test Approach, Levels, and Types

### Approach

1. Confirm requirements, environment, account provisioning, authentication flow, and assertions before execution.
2. Perform a manual smoke check of the approved page and current UI before relying on automation locators.
3. Run positive and negative functional scenarios against a non-production environment using dedicated, authorized accounts.
4. Automate only stable, approved scenarios after verifying the current DOM and the TestNG/Maven setup.
5. Capture outcome, environment, browser/version, test-data identifier (never the password), timestamp, and relevant sanitized evidence for each run.
6. Re-run the focused suite after a related change and investigate intermittent failures rather than retrying them into a pass.

### Levels and types

- **System/UI functional:** login page availability, control behavior, required fields, and user-visible failure states.
- **Integration:** accepted credentials authenticate through the approved identity flow; rejected credentials do not.
- **Regression:** repeat approved login cases after login, browser, or authentication configuration changes.
- **Compatibility smoke:** run on the single approved browser/platform baseline. Broader browser coverage is pending approval.
- **Nonfunctional/security:** excluded except for the minimal safe handling checks in REQ-LOGIN-08; no performance or penetration conclusions are implied.

### Automation constraints and decisions

- Planned stack: Java 17 or an explicitly approved supported Java version, Selenium 4.x, Maven, and TestNG 7.x. Exact versions must be verified against the target environment and dependency repositories before implementation.
- Use current Selenium APIs, `java.time.Duration` for waits, and explicit waits for required conditions. Do not use `Thread.sleep()`, obsolete timeout signatures, fabricated APIs, or swallowed exceptions.
- The source instructions conflict on PageFactory: `01_RICE_POT_Prompt.md` mandates `PageFactory` and `@FindBy(xpath = "...")`, while `03_Anti_Hallucinations.md` explicitly says not to use `PageFactory` and prefers explicit `By` locators with wait wrappers. Obtain an owner decision before implementation; do not silently combine or disregard these requirements.
- The source prompt requires XPath-only locators. Treat its XPath examples as unverified; confirm them against the current DOM and document any approved locator change before automation.
- Use TestNG lifecycle annotations at their appropriate suite/test/method levels, targeted exception handling or explicit propagation, and guaranteed cleanup.
- Do not hardcode credentials or commit secrets. Read test credentials from an approved secret store or protected environment variables.
- Keep the plan's broader coverage distinct from the source example's two-script output restriction; reconcile the count and deliverable before code generation.

## 6. Environment, Tools, Access, and Test Data

| Item | Plan / status |
| --- | --- |
| Application URL | Requested: `https://login.salesforce.com/?locale=in`. Confirm the authorized non-production URL before testing. |
| Environment / tenant | Not provided. Use only an approved test tenant or sandbox; do not run credential tests against production without explicit authorization. |
| Browser / version | Not provided; must be agreed and recorded. |
| Operating system | Not provided for the test execution environment. |
| Java / Selenium / Maven / TestNG versions | Target stack is described in the source documents; exact versions and compatibility are not approved. Verify and pin supported versions during implementation. |
| Test account | Not provided. Application owner must provision a dedicated active account and arrange reset/cleanup. |
| Invalid credentials | Use approved synthetic inputs or a designated test account; avoid lockout and alerting side effects. |
| Authenticated-state assertion | Not provided. Application owner must identify a stable, non-sensitive success signal. |
| Invalid-login assertion | Not provided. Application owner must identify stable failure behavior without assuming exact copy or locator. |
| MFA / SSO / CAPTCHA | Behavior not provided. Establish whether these are disabled, test-bypassed through an approved mechanism, or included in separate scope. Never attempt an unauthorized bypass. |
| Remember Me semantics | Not provided. Confirm whether this remembers an identifier, authentication state, or another preference, and define the persistence boundary. |
| Tooling | Proposed: Maven, TestNG, Selenium WebDriver, approved browser driver management, version control, and the team's defect tracker. Exact tools not provided. |
| Credentials and evidence | Store secrets outside source control; redact credentials, tokens, and personal data from logs, screenshots, and reports. |

## 7. Entry and Exit Criteria

All thresholds below are proposed for approval; they are not sourced product commitments.

### Entry criteria

- Product/application owner approves the requirements and expected outcomes in Section 4.
- An authorized, stable test environment and URL are available.
- A dedicated test account, safe invalid-input policy, and account recovery/reset path are available.
- The expected authenticated and denied states are observable and documented.
- MFA, SSO, CAPTCHA, and Remember Me behavior are clarified for the agreed scope.
- Browser/platform baseline and automation design decisions (including PageFactory versus explicit `By`) are approved.
- Required test tooling and dependencies are available; credentials are delivered through an approved secret mechanism.
- For a test cycle, no known environment outage prevents meaningful execution.

### Exit criteria

- 100% of approved in-scope test cases have a recorded result (pass, fail, blocked, or not run) for the planned cycle.
- 100% of Critical-priority cases pass, or each failure has an owner-approved disposition before release.
- No open Critical or High severity login defect remains without a documented, authorized disposition.
- Every failure has a defect or documented triage decision; every blocked/not-run test has a reason.
- Test evidence contains no plaintext credentials, tokens, or unnecessary personal data.
- Results, known limitations, residual risks, and requirement coverage are reviewed by the designated approver.

## 8. Roles, Responsibilities, Estimates, and Schedule

| Role | Responsibility | Owner / estimate |
| --- | --- | --- |
| Product or application owner | Approve requirements, expected outcomes, environment, Remember Me behavior, and release disposition. | Not provided |
| QA lead | Maintain scope, risk, traceability, entry/exit criteria, and test-cycle reporting. | Not provided |
| QA engineer / SDET | Prepare data, execute approved tests, implement agreed automation, and report defects. | Not provided |
| Salesforce / identity administrator | Provision test access and clarify MFA, SSO, account policy, and reset procedures. | Not provided |
| Development / support | Triage and resolve defects; support environment and locator changes. | Not provided |

Schedule, staffing, and effort are not provided and cannot be estimated reliably until environment access, account setup, and open decisions are resolved.

## 9. Defect Management and Reporting

- Log each reproducible failure in the approved defect tracker with a concise title, environment, browser/version, build or configuration, test ID, reproduction steps, actual result, expected result, and sanitized evidence.
- Never include passwords, session tokens, recovery codes, or unredacted personal information in a defect.
- Prioritize based on authentication impact: inability of legitimate users to sign in or unauthorized authentication is Critical; broad degradation or missing required negative behavior is High; limited UI/validation issues are Medium or Low subject to team policy.
- QA, development, and the application owner triage severity, reproducibility, scope, and release impact. Severity definitions and response SLAs are not provided; use the team's approved policy.
- Report execution totals by pass/fail/blocked/not run, requirement coverage, open defects by severity, environment health, and known risks at the end of each agreed cycle. Reporting cadence is not provided.

## 10. Risks, Dependencies, Assumptions, and Open Questions

### Risks and dependencies

- Testing an unapproved production login can affect real accounts, trigger lockouts, or generate security alerts. An authorized test tenant and accounts are prerequisites.
- MFA, SSO, CAPTCHA, conditional access, or account policy can block automated sign-in. Confirm supported test flows with the application owner; do not bypass controls without authorization.
- Unverified sample XPath expressions may not match the current DOM and must not be treated as established locators.
- Login error wording and page behavior may vary by locale, tenant, or identity policy; assert approved behavior rather than invented exact text.
- Remember Me can persist sensitive state. Its semantics, retention boundary, and security expectations must be defined before verification.
- The required PageFactory pattern conflicts with the anti-hallucination document's prohibition. Resolve before designing page objects.
- The example's two-script limit conflicts with broad functional coverage and the Remember Me requirement. Agree on the final scope and script count.
- A public endpoint, network dependency, or external identity service may introduce failures unrelated to application changes; record environment status when triaging.

### Assumptions

- Login is a username/email and password workflow, as described by the source prompt.
- Only dedicated, authorized test accounts will be used.
- No acceptance criteria, selectors, or error messages are confirmed merely because the source prompt includes examples.
- Test-case priorities, thresholds, and the proposed exit criteria in this plan require stakeholder approval.

### Open questions

1. What non-production Salesforce URL and tenant are approved for these tests?
2. Which browser and operating-system baseline must be supported?
3. Who provisions the valid test account, and how are account lockout and reset handled?
4. What exact observable signal confirms a successful login and a denied login?
5. Does the login flow include MFA, SSO, CAPTCHA, or other required steps?
6. What does Remember Me remember, and what persistence behavior is expected?
7. Should automation follow the prompt's PageFactory requirement or the anti-hallucination document's explicit-`By` recommendation?
8. Should deliverables cover all scenarios in this plan, or be limited to the two scripts in the filled automation example?
9. Which exact dependency versions, reporting tools, and defect-priority policy are approved?
10. Who approves the plan, the test results, and any residual defects?

## 11. Suspension and Resumption Criteria

### Suspend testing when

- The environment is unavailable or is not confirmed as authorized for testing.
- The valid test account is unavailable, locked, or cannot complete its approved authentication flow.
- MFA, SSO, CAPTCHA, or an identity-policy change makes the agreed test path unavailable.
- A test risks affecting real users, triggering uncontrolled lockouts, or exposing credentials or sensitive data.
- A blocking defect prevents meaningful execution of multiple in-scope cases.

### Resume testing when

- The application owner confirms the environment and access are restored and authorized.
- Test-account access and any required authentication steps are working through an approved process.
- The blocking defect or environment issue is resolved or explicitly accepted for continued testing.
- Credentials and evidence handling meet the agreed security requirements.

Record the suspension reason, affected cases, owner, and resume decision in the test-cycle report.

## 12. Test Deliverables and Approval

### Deliverables

- This test plan and approved revisions.
- Approved requirements-to-test coverage and test data setup instructions, with no secrets.
- Test execution results and sanitized evidence for each in-scope case.
- Defect records and test-cycle summary.
- If separately approved, maintainable Maven/TestNG automation and its execution instructions.

### Approval

| Approval role | Name | Decision / date |
| --- | --- | --- |
| Product / application owner | Not provided | Pending |
| QA lead | Not provided | Pending |
| Development / identity owner | Not provided | Pending |

### Final review checklist

- [x] Test-plan profile and required sections follow the generic QA template.
- [x] RICE-POT elements and the supplied Salesforce/login context are represented.
- [x] Source examples are distinguished from verified application behavior.
- [x] Requirements have traceable planned coverage and measurable proposed exit criteria.
- [x] Missing inputs, conflicting constraints, assumptions, and risks are explicit.
- [x] No credentials or fabricated execution results are included.
- [ ] Stakeholders approve scope, open decisions, criteria, environment, and plan.
