# GitHub API Test Automation Suite - Case Study

## Overview
Designed and implemented a comprehensive test automation suite for the GitHub REST API v3, covering critical user workflows including authentication, repository management, issues, pull requests, and advanced search functionality. The suite validates 15+ endpoints across 6 functional areas with 40+ granular test assertions using Postman's JavaScript runtime and Chai assertion library.

## What Was Tested
- **Authentication & User Management**: Token validation, user profile retrieval, and user data consistency across public and private accounts
- **Repository Operations**: List/retrieve repositories with filtering by type and sort criteria, validate repository metadata (owner, language, visibility, star counts)
- **Issues & Pull Requests**: List and retrieve issues/PRs with state validation, confirm proper enum handling (open/closed) across paginated results
- **Search & Filtering**: Repository search by language (Python, JavaScript, etc.), issue search with label and state filters, verify pagination and total count accuracy
- **Error Handling**: Invalid token scenarios (401 Unauthorized), non-existent resource access (404), rate limit header validation
- **Performance & Data Integrity**: Response time benchmarking (< 3 seconds), JSON structure validation, null-field detection in critical user fields

## Issues Identified & Resolved
1. **Inconsistent Variable Scoping**: Initial pre-scripts failed due to Postman collection vs. environment variable hierarchy; resolved by using `pm.collectionVariables` with fallback environment variable checks
2. **Date Field Parsing**: ISO 8601 timestamps required explicit parsing before comparison; added date conversion logic in assertions
3. **Pagination Edge Cases**: Search endpoints return partial results; added validation to confirm all returned items match filter criteria rather than assuming completeness
4. **Rate Limiting Transparency**: Requests didn't surface rate limit headers in test reports; added explicit header extraction tests to monitor quota consumption

## Business Value Delivered
- **Comprehensive API Coverage**: 15+ endpoints tested with 40+ assertions eliminates blind spots in API reliability before production
- **Reusable Test Artifact**: Collection is importable into any Postman workspace and runs standalone - clients can execute tests immediately without setup friction
- **Maintainable Test Data**: Dynamic variable injection and parameterization enable tests to run against any GitHub account without hardcoding credentials or repository names
- **Quality Gate Framework**: Pre/post-script architecture allows easy extension with custom hooks (e.g., logging, Slack notifications, JIRA ticket creation)
- **Documentation by Example**: Each request includes descriptive assertions that double as API documentation for developers integrating GitHub APIs

**Tools Used**: Postman, JavaScript (pre/post-scripts), Chai assertion library, GitHub REST API v3
**Test Execution**: Can be run via Postman UI, Newman CLI, or integrated into CI/CD pipelines (Jenkins, GitHub Actions)
