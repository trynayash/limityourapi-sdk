# Changelog

All notable changes to the LimitYourAPI project will be documented in this file.

---

## [2.0.1] - 2026-06-18

### 🚀 Improvements
- **Package rename and aliases:** the npm package is now `limityourapi`; added parameter aliases (`route`, `key`, `identifier`) and response aliases (`reset`, `retryAfter`, `retry_after`).
- **Improved TypeScript Support:** Added complete static type definitions and explicit interface contracts (`LimitYourAPIOptions`, `CheckOptions`, `CheckResult`) to resolve structural overlap.
- **Enhanced Documentation:** Restructured JavaScript and TypeScript integrations into separated, quickstart-focused and advanced reference guides, complete with before/after middleware examples.
- **Updated Examples:** Added Express, FastAPI, and Go Fiber configuration templates.

---

## [1.2.0] - 2026-04-13
- Node.js SDK initial feature implementation.
- Added Express middleware route throttling wrappers.
- Integrated fallback support for local client-side memory locks.
