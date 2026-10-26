# QA Test Plan and Strategy — Sprint 5

**Project:** Toolshop QA Hands-on Assessment  
**Application:** Practice Software Testing — Buggy Release Candidate  
**Role:** QA Lead  
**Test Approach:** Risk-based, requirements-driven testing  


---

## 1. Test Objective

The objective is to validate the quality, functionality, security, accessibility, and release readiness of the Toolshop application.

Testing will cover both the web application and REST APIs.

Sprint 5 user stories and acceptance criteria are the source of truth. Where application behavior conflicts with requirements, the requirements take precedence unless an exception is justified and documented.

The final outcome will be an evidence-based GO or NO-GO recommendation.

## 2. Application Under Test

| Component | URL |
|---|---|
| Web application | https://with-bugs.practicesoftwaretesting.com |
| REST API | https://api-with-bugs.practicesoftwaretesting.com |
| Swagger documentation | https://api-with-bugs.practicesoftwaretesting.com/api/documentation |
| Source repository | https://github.com/testsmith-io/practice-software-testing |
| Sprint 5 requirements | https://github.com/testsmith-io/practice-software-testing/tree/main/_user-stories |

## 3. Scope of Testing

### 3.1 In Scope

- User registration, login, logout, and authentication
- User profile and account management
- Product listing, searching, sorting, and filtering
- Product details and inventory-related behavior
- Shopping cart and quantity management
- Checkout, payments, and order processing
- API functionality, validation, and error handling
- Authentication and authorization security
- Accessibility against WCAG 2.2 AA
- Responsive UI and cross-browser compatibility
- Negative, boundary, and regression testing

### 3.2 Out of Scope for Execution

- Load, stress, and endurance testing against the shared public site
- Destructive testing against other users' accounts or records
- Exhaustive testing of every browser/device combination
- Internal unit testing without access to the release build and development environment
- Real financial transactions unless an approved sandbox is provided

These exclusions are due to the 90-minute time limit, shared environment restrictions, and the need to protect other users.

## 4. Risk Analysis and Prioritization

Risk priority considers business impact, likelihood, user exposure, and potential security consequences.

| Rank | Feature / Risk | Impact | Priority |
|---|---|---|---|
| 1 | Unauthorized access to user data | Critical | P0 |
| 2 | Checkout or order processing failure | Critical | P0 |
| 3 | Authentication and session security | High | P0 |
| 4 | Incorrect cart quantities or prices | High | P0 |
| 5 | API authorization and input validation | High | P1 |
| 6 | Accessibility barriers in essential journeys | High | P1 |
| 7 | Product search, sorting, and filtering | Medium | P1 |
| 8 | Cross-browser and responsive defects | Medium | P2 |
| 9 | Performance degradation | Medium | P2 |

**Execution priority:** Authentication and authorization first, followed by checkout and cart, API validation, product discovery, and accessibility.

## 5. Test Levels and Types

### 5.1 Functional Testing

Validate key business workflows against Sprint 5 acceptance criteria.

Techniques:
- Positive and negative testing
- Boundary value analysis
- Equivalence partitioning
- Decision tables
- State transition testing

### 5.2 API Testing

Validate:
- HTTP response status codes
- Response body and JSON structure
- Required fields and data types
- Authentication tokens
- Unauthorized and forbidden requests
- Invalid payload handling
- Resource ownership and access restrictions

### 5.3 Security Testing

Focus on:
- Authentication bypass attempts
- Broken object-level authorization
- Broken function-level authorization
- Session and token handling
- Input validation
- Exposure of sensitive information

Security tests will use self-created accounts and resources. If an authorization weakness appears to allow access to another user's data, testing will stop before any modification or deletion.

### 5.4 Accessibility Testing

Evaluate WCAG 2.2 AA, particularly:
- Keyboard accessibility
- Visible keyboard focus
- Accessible names and labels
- Error identification and descriptions
- Color contrast
- Semantic headings
- Screen reader compatibility
- Focus order and navigation

Use keyboard-only testing, browser accessibility inspection, and axe-core where practical.

Automated accessibility tools supplement, but do not replace, manual assessment.

### 5.5 Cross-Browser Testing

Primary browser: Chromium.

Secondary browsers: Firefox , depending on time.

Validate critical journeys and layout behavior on desktop and mobile viewport sizes.

### 5.6 Performance Testing

No load or stress tests will be executed against the shared public site.

For a dedicated staging environment, the proposed approach is:
- Measure API response-time percentiles
- Measure Core Web Vitals such as LCP, INP, and CLS
- Conduct controlled load and stress tests with k6 or JMeter
- Evaluate throughput, error rates, and resource utilization
- Agree on performance thresholds with stakeholders

## 6. Automation Strategy

**Framework:** Playwright with TypeScript.

### Automate

- API authentication and authorization checks
- API negative and boundary scenarios
- Product API validation
- User login workflows
- Product search
- Shopping cart functionality
- Critical regression scenarios

### Keep Manual

- Exploratory testing
- Complex accessibility evaluation
- Visual and usability testing
- Unusual multi-step workflows
- Investigation and reproduction of defects

### Framework Design

- Page Object Model for UI
- Reusable API client
- Environment-based configuration
- Self-generated test data
- Independent tests
- HTML reporting
- GitHub Actions integration

Known-bug regression tests will fail or be explicitly marked as expected failures with links to the corresponding GitHub Issues.

## 7. Test Design Techniques by Feature

| Feature | Technique | Purpose |
|---|---|---|
| Login | Equivalence partitioning | Valid and invalid credentials |
| Registration | Boundary value analysis | Required fields and input lengths |
| Authentication | State transition | Login, logout, token expiration |
| Product search | Equivalence partitioning | Matching and nonmatching queries |
| Product filters | Decision tables | Filter combinations |
| Cart | State transition | Add, update, remove |
| Checkout | Decision tables | Valid and invalid checkout combinations |
| API validation | Negative testing | Missing and malformed payloads |
| Authorization | Access-control matrix | User and role permissions |
| Accessibility | WCAG criterion-based checks | Accessibility compliance |

## 8. High-Level Test Cases

The following are planned test scenarios. Each must be mapped to the relevant Sprint 5 user story and exact acceptance criterion before execution.

| Test ID | Feature | Test Scenario | Expected Result |
|---|---|---|---|
| TC-001 | Login | Login with valid credentials | User authenticated |
| TC-002 | Login | Login with invalid credentials | Authentication rejected |
| TC-003 | Registration | Submit invalid registration fields | Validation errors displayed |
| TC-004 | Authorization | Access protected API without token | Request rejected |
| TC-005 | Authorization | Access another test account's protected resource | Access denied |
| TC-006 | Products | Search by product keyword | Correct matching results |
| TC-007 | Products | Apply product filters | Results follow selected criteria |
| TC-008 | Cart | Add a product | Cart updated correctly |
| TC-009 | Cart | Change product quantity | Quantity and total updated correctly |
| TC-010 | Cart | Remove a product | Product removed |
| TC-011 | Checkout | Submit missing required details | Validation prevents invalid checkout |
| TC-012 | Orders | Complete valid order workflow | Order created according to requirements |
| TC-013 | API | Submit malformed request body | Appropriate client error |
| TC-014 | Accessibility | Navigate forms using keyboard | Logical focus and operable controls |
| TC-015 | Accessibility | Submit invalid form | Accessible error identification |

**Traceability requirement:** Record each case's Sprint 5 story URL, acceptance criterion, execution result, and associated defect ID. No test should be marked passed solely because it matches current application behavior.

## 9. Test Data

- Register unique test accounts for the assessment.
- Use separate self-created accounts for authorization checks.
- Generate unique test identifiers.
- Use existing public product data for read-only catalog checks.
- Modify or delete only records created by the tests.
- Never use shared demo accounts for lockout testing.
- Do not commit passwords, tokens, or secrets.

## 10. Test Environment

| Item | Configuration |
|---|---|
| Environment | Shared buggy release candidate |
| UI | Angular web application |
| API | REST API |
| Automation | Playwright TypeScript |
| Browser | Chromium, Firefox, WebKit |
| API documentation | Swagger |
| Defect tracking | GitHub Issues |
| CI | GitHub Actions |
| Reporting | Playwright HTML report |

Record the actual browser version, operating system, test account, and execution timestamp when running tests and reporting bugs.

## 11. Entry Criteria

Testing begins when:
- Sprint 5 requirements are accessible
- The release candidate is available
- API documentation is available
- Test accounts can be created
- Testing tools are configured
- High-risk scenarios are identified

## 12. Exit Criteria

Testing can conclude when:
- Prioritized critical workflows have been evaluated
- Executed tests have recorded results
- Defects include reproducible evidence
- Critical security risks are assessed
- Automation results are reviewed
- Known limitations and untested areas are documented
- A release recommendation has been prepared

Release approval requires no unresolved Critical defects and no unacceptable Major defects affecting core workflows.

## 13. Defect Management

Each defect will be reported as a separate GitHub Issue containing:

- Environment: URL, browser/version, OS, test account
- Severity and justification
- Priority
- User story and acceptance criterion
- Steps to reproduce
- Expected result
- Actual result
- Screenshot, video, or API request/response evidence

### Severity Classification

| Severity | Definition |
|---|---|
| Critical | Security exposure or essential business workflow blocked |
| Major | Important functionality broken |
| Minor | Limited functionality affected with a workaround |
| Trivial | Cosmetic or negligible functional impact |

### Priority Classification

- **High:** Requires resolution before release or urgent triage
- **Medium:** Important, but may have a workaround
- **Low:** Can be scheduled for later

Defects will be triaged by reproducibility, impact, severity, business priority, and release risk.

Fixed defects must be retested, followed by relevant regression testing.

## 14. Test Execution Schedule

| Activity | Duration |
|---|---|
| Test planning and strategy | 20 minutes |
| Exploratory testing and defect reporting | 30 minutes |
| API/UI automation and CI | 30 minutes |
| Test summary and release decision | 10 minutes |
| **Total** | **90 minutes** |

## 15. Release Recommendation Criteria

### GO

Release may proceed when:
- No unresolved Critical defects remain
- Core workflows meet their acceptance criteria
- No unacceptable authorization vulnerabilities exist
- Important automation checks pass
- Residual risks are accepted by stakeholders

### NO-GO

Release must be blocked when:
- Critical authorization vulnerabilities remain
- Checkout or order creation is broken
- Sensitive customer information is exposed
- Critical authentication failures remain
- Essential accessibility barriers prevent completion of key journeys

**Current recommendation: Pending execution.**

The final decision will be recorded in `TEST_SUMMARY.md` based on actual test evidence.

## 16. Deliverables

1. `TEST_PLAN.md` — Test strategy and risk analysis
2. GitHub Issues — Verified defects with evidence
3. API automation — Minimum 8 tests
4. UI automation — Minimum 4 tests
5. GitHub Actions — Automated PR execution and HTML reports
6. `TEST_SUMMARY.md` — Results and release recommendation
7. `README.md` — Setup instructions and links to PRs and defects

---

**QA Lead principle:** Release readiness is determined by verified requirements, defect impact, test evidence, and residual risk—not merely by the number of passing tests.
