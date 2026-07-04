# NorthFace_Automation – BDD Test Automation Framework (Selenium · Cucumber · TestNG · RestAssured · JUnit)

[![CI](https://github.com/REZAULKARIM2024/NorthFace_Automation/actions/workflows/ci.yml/badge.svg)](https://github.com/REZAULKARIM2024/NorthFace_Automation/actions/workflows/ci.yml)
![Java](https://img.shields.io/badge/Java-17-red)
![Selenium](https://img.shields.io/badge/Selenium-4.21-43B02A)
![Cucumber](https://img.shields.io/badge/Cucumber-7.18-23D96C)
![TestNG](https://img.shields.io/badge/TestNG-7.10-orange)
![Maven](https://img.shields.io/badge/Build-Maven-blue)
![Tests](https://img.shields.io/badge/tests-69%20passing-brightgreen)
![Pass rate](https://img.shields.io/badge/pass%20rate-100%25-brightgreen)
![Report](https://img.shields.io/badge/report-Allure-orange)

A robust, multi-layer test automation framework demonstrating modern QA/SDET practice — **UI, End-to-End,
API, Unit, Accessibility, Performance, and Security testing** in one project, with **69 automated tests**
and a real, documented engineering decision at its core.

---

## 🌐 Applications Under Test

| Application | URL | Used for |
|-------------|-----|----------|
| The North Face | https://www.thenorthface.com/ | Bot-safe smoke / navigation (production, PerimeterX-protected) |
| Automation Exercise | https://automationexercise.com/ | Search, cart, checkout, auth & API E2E |

---

## 📊 Latest Test Run

**Allure report — 69 test cases, 100% passed** (4 m 56 s, 18 suites).

| Metric | Result |
|--------|--------|
| ✅ Total test cases (Allure, unified) | **69** |
| ✅ Pass rate | **100%** |
| ❌ Failed | **0** |
| ⏱️ Duration | **4 m 56 s** |
| 🧩 Breakdown | **21** Cucumber scenarios + **48** JUnit 5 unit tests |
| 🖥️ Runtime | Java **17.0.12** (HotSpot 64-bit) · cucumber-jvm **7.18.0** · Windows 11 |

> Numbers come from the generated **Allure** report (which unifies the Cucumber and JUnit
> results into a single dashboard) and the Cucumber HTML report
> (`target/cucumber-reports/report.html`).

---

## 🚀 Overview

**NorthFace_Automation** was built against a **real, bot-protected production retail site**
(https://www.thenorthface.com/) rather than a friendly demo app — and the standout part is how it
handled the site fighting back.

- The site (protected by **PerimeterX**) served a *"Press & Hold to confirm you are a human"* bot
  challenge to the automated browser. I diagnosed it from a **live DOM dump** (`px-captcha-modal`
  overlay intercepting clicks) — a real anti-bot system, not a locator bug.
- I made the correct engineering call: keep North Face limited to **bot-safe smoke checks**, and run
  the transactional **search → cart → checkout** flow against an automation-friendly target
  (automationexercise.com) — exactly how a QA team points automation at a staging environment.
- I removed real flakiness at the source: the demo site's **Google Vignette interstitial ads** were
  hijacking navigation, so I blackholed ad/analytics domains at the Chrome layer and navigate by
  direct URL.

*Diagnose → decide → stabilise* — the difference between following a tutorial and automating a messy,
real application.

---

## 🛠️ Tech Stack

* **Language:** Java 17
* **Automation Tool:** Selenium WebDriver 4.21
* **BDD Framework:** Cucumber 7.18 (Gherkin)
* **Test Runner:** TestNG
* **API Testing:** RestAssured
* **Unit Testing:** JUnit 5
* **Load/Performance:** Apache JMeter
* **Security (DAST):** OWASP ZAP
* **Build Tool:** Maven
* **Driver Management:** WebDriverManager
* **CI:** GitHub Actions
* **IDE:** Eclipse / IntelliJ

---

## 📁 Project Structure

```bash
NorthFace_Automation/
│
├── src/test/java/com/northface/automation/
│   ├── pages/            # Page Object Model (North Face)
│   │   └── demo/         # Page Objects for the demo shop (E2E)
│   ├── stepdefinitions/  # Cucumber glue: Common, DemoShop, Auth, Api, Accessibility, Performance, Hooks
│   ├── runners/          # TestRunner (TestNG + Cucumber)
│   ├── utils/            # ConfigReader, DriverManager (ad-domain blocking, DOM dumps)
│   ├── lib/              # Pure logic: PriceUtils, CartCalculator, EmailValidator, PasswordValidator
│   └── unit/             # JUnit 5 unit tests
│
├── src/test/resources/
│   ├── features/         # Gherkin: smoke, navigation, search, cart, negative, auth, api, accessibility, performance
│   └── config.properties # URLs, browser, headless, timeouts, perf budget
│
├── qa-extras/
│   ├── jmeter/           # API load-test plan (.jmx)
│   └── zap/              # OWASP ZAP baseline scan
│
├── .github/workflows/ci.yml   # CI: runs unit + API tests on every push
├── run-all.bat / run-unit.bat / run-demo.bat
├── testng.xml            # TestNG suite
├── pom.xml               # Maven dependencies + `unit` profile
└── README.md
```

---

## 🎯 Test Coverage (69 tests · 100% passing)

| Layer | Type | Tool | Positive / Negative | Count |
|-------|------|------|:--:|:--:|
| 🔥 UI | Smoke & navigation (North Face, bot-safe) | Selenium + Cucumber | + | 2 |
| 🛒 E2E | Search → add-to-cart → checkout (demo shop) | Selenium + Cucumber | + | 8 |
| 🔐 Functional | Auth: signup / invalid login | Selenium | +/− | 2 |
| 🔌 API | productsList, brandsList, searchProduct, verifyLogin | RestAssured | +/− | 6 |
| ♿ Accessibility | lang / title / accessible-name | Selenium + JS | +/− | 2 |
| ⚡ Performance | page-load budget smoke | Selenium | +/− | 2 |
| 🧪 Unit | price / cart-math / email / password validators | JUnit 5 | +/− | 48 |
| 📈 Load | API load-test plan | Apache JMeter | — | scaffold |
| 🛡️ Security | passive baseline scan | OWASP ZAP | — | scaffold |

Every functional area has **positive and negative** cases; search is **data-driven** (`Scenario Outline` + `Examples`: dress, top, jeans, tshirt).

---

## ⚙️ Framework Highlights

* ✔️ **BDD (Cucumber + Gherkin)** — human-readable scenarios
* ✔️ **Page Object Model (POM)** — every locator lives in one place
* ✔️ **Layered design** — pages · steps · runners · utils · lib · unit
* ✔️ **Dependency Injection (PicoContainer)** — one shared `TestContext` per scenario, no static state
* ✔️ **Config-driven** — URLs / browser / headless / timeouts / perf budget, overridable with `-Dkey=value`
* ✔️ **Self-documenting failures** — screenshot **and** live-DOM dump to `target/dom-dumps/`
* ✔️ **Flakiness controls** — ad-domain blocking + direct-URL navigation
* ✔️ **CI on every push** — GitHub Actions runs the deterministic unit + API suite (green badge above)

---

## ▶️ Running the Tests

```bash
# Full Cucumber suite (UI + E2E + API + a11y + perf) → 21 scenarios
mvn clean test                                    # or double-click run-all.bat

# JUnit unit tests (fast, deterministic, no browser) → 48 tests
mvn clean test -Punit                             # or run-unit.bat

# Slice by tag
mvn clean test -Dcucumber.filter.tags="@smoke"
mvn clean test -Dcucumber.filter.tags="@api"
mvn clean test -Dcucumber.filter.tags="@negative"

# Headless (CI)
mvn clean test -Dheadless=true
```

Report: `target/cucumber-reports/report.html` (screenshots auto-attached on failure).

---

## 📈 Reporting

Three report layers are available:

| Report | What it shows | How to generate |
|--------|---------------|-----------------|
| **Allure** (recommended) | Unified interactive dashboard for **Cucumber + JUnit** — overview, pass/fail charts, suites, timeline | `run-report.bat`  •  or `mvn clean test` → `mvn test -Punit` → `mvn allure:serve` |
| **Cucumber HTML** | BDD scenario report with steps + failure screenshots | auto-generated at `target/cucumber-reports/report.html` |
| **Surefire HTML** | Plain JUnit/TestNG results table | `mvn surefire-report:report-only` |

> The Allure run above recorded **69 test cases at 100%**. `run-report.bat` runs the full
> Cucumber suite, then the JUnit tests (without cleaning, so both sets land in
> `target/allure-results`), then opens the interactive report in your browser.

## 📌 Prerequisites

* Java JDK **17+**
* Maven 3.8+
* Google Chrome (ChromeDriver auto-managed by WebDriverManager)
* Internet connection (tests hit live sites)
* *(Optional)* Docker for OWASP ZAP, Apache JMeter for load tests

---

## 📈 Future Enhancements

* Add **Extent / Allure** reporting
* Enable **parallel execution** (TestNG data-provider)
* Add **cross-browser** runs (Firefox, Edge) + **BrowserStack / Sauce Labs**
* Data-driven from **Excel / JSON**
* Expand JMeter (stress / endurance) and ZAP (active scan) coverage

---

## 👨‍💻 Author

**Rezaul Karim** — QA Automation Engineer / SDET
📧 rknyc2021@gmail.com

---

## 📄 Summary

NorthFace_Automation is a real-world, industry-aligned framework that goes beyond happy-path UI testing:
it spans **UI, E2E, API, unit, accessibility, performance, and security**, and backs every claim with a
**verified 100% pass rate — 21/21 Cucumber scenarios (4m53s) plus 48 JUnit tests, 69 total** — and CI on
every push. It also documents a genuine engineering decision (working around a live PerimeterX bot wall).
Built to be readable, maintainable, and interview-ready.
