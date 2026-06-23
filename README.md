# GitHub API Test Automation Suite

A comprehensive test automation suite for the GitHub REST API v3, built with Postman and Chai assertions. Covers authentication, repository management, issues, pull requests, search functionality, error handling, and performance validation.

## Features

- **15+ API endpoints** tested across 6 functional areas
- **40+ granular assertions** validating status codes, response structure, and data integrity
- **Dynamic variable injection** for credential-free test execution
- **Pre/post-script automation** for setup, teardown, and cross-request data flow
- **Error scenario coverage** including 401 Unauthorized, 404 Not Found, and rate limit handling
- **Performance benchmarking** with response time validation
- **CI/CD ready** for integration with Jenkins, GitHub Actions, or Newman CLI

## Prerequisites

- Postman (v9.0+)
- GitHub Personal Access Token (PAT)
- Internet connectivity

## Setup

### 1. Generate a GitHub Personal Access Token

1. Go to https://github.com/settings/tokens
2. Click "Generate new token (classic)"
3. Select scopes:
   - `repo` (full control of private repositories)
   - `public_repo` (access to public repositories)
   - `user` (user profile data)
4. Copy the generated token

### 2. Import the Collection

1. Open Postman
2. Click "Import" in the top-left
3. Select `GitHub_API_Test_Suite.postman_collection.json`
4. Collection will appear in your workspace

### 3. Configure Variables

1. Click the collection name in the left sidebar
2. Go to "Variables" tab
3. Set the following:
   - `github_token`: Paste your GitHub PAT (from Step 1)
   - `github_user`: Your GitHub username (e.g., octocat)
   - `base_url`: `https://api.github.com` (pre-filled, no change needed)

## Running Tests

### Option 1: Postman UI

1. Select the collection folder or individual request
2. Click the blue "Send" button
3. View test results in the "Test Results" tab below the response
4. Green checkmarks indicate passing tests; red X indicates failures

### Option 2: Newman CLI (Automated)

```bash
# Install Newman globally
npm install -g newman

# Run the collection with your token
newman run GitHub_API_Test_Suite.postman_collection.json \
  --environment environment.json \
  --reporters cli,json \
  --reporter-json-export test-results.json
```

### Option 3: CI/CD Integration

```yaml
# Example GitHub Actions workflow
name: API Tests
on: [push, pull_request]
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: actions/setup-node@v2
      - run: npm install -g newman
      - run: newman run GitHub_API_Test_Suite.postman_collection.json \
          --environment environment.json
```

## Test Suites

### 1. Setup & Auth
- Validates GitHub token configuration
- Initializes collection variables for downstream requests

### 2. User & Profile Tests
- Get authenticated user
- Get user by username
- Validates user profile structure and data consistency

### 3. Repository Tests
- List user repositories with filtering
- Get specific repository metadata
- Validates owner, language, visibility, and star count fields

### 4. Issues & Pull Requests
- List repository issues with state validation
- List pull requests with required field checks
- Confirms open/closed state enumeration

### 5. Search & Filtering
- Search repositories by language
- Search issues with label and state filters
- Validates pagination and total count accuracy

### 6. Error Handling & Edge Cases
- Invalid token (401 Unauthorized)
- Non-existent resource (404 Not Found)
- Rate limit header validation

### 7. Performance & Response Validation
- Response time benchmarking (< 3 seconds)
- JSON structure and null-field detection

## Understanding Test Assertions

Each request includes Chai assertions in the "Tests" tab. Examples:

```javascript
// Status code validation
pm.test('Status code is 200', function() {
  pm.response.to.have.status(200);
});

// Response structure validation
pm.test('Response has required user fields', function() {
  const response = pm.response.json();
  pm.expect(response).to.have.property('login');
  pm.expect(response).to.have.property('id');
});

// Data type validation
pm.test('User data types are correct', function() {
  const response = pm.response.json();
  pm.expect(response.login).to.be.a('string');
  pm.expect(response.id).to.be.a('number');
});
```

## Customization

### Add Custom Tests

1. Click on a request
2. Go to the "Tests" tab
3. Add Chai assertions:
```javascript
pm.test('My custom test', function() {
  pm.expect(pm.response.json().someField).to.equal('expected_value');
});
```

### Extend with New Endpoints

1. Right-click the collection in the sidebar
2. Click "Add request"
3. Configure method, URL, headers, and body
4. Add assertions in the "Tests" tab
5. Use pre/post-scripts to pass data between requests

### Integrate with External Tools

Pre/post-scripts support external integrations:

```javascript
// Send to Slack on failure
if (pm.test.results.some(test => test.error)) {
  pm.sendRequest("https://hooks.slack.com/services/YOUR/WEBHOOK", {
    method: 'POST',
    body: {
      text: `API tests failed: ${pm.test.results}`
    }
  });
}
```

## Troubleshooting

### Invalid Token Error
- Verify your GitHub PAT has not expired
- Check "github_token" variable is set correctly in collection variables
- Regenerate PAT if needed at https://github.com/settings/tokens

### Tests Failing on Repositories
- Ensure your GitHub account has at least one repository
- If not, create a test repo first
- Update `test_repo` variable manually if needed

### Rate Limit Exceeded
- GitHub API has rate limits (60 requests/hour for unauthenticated, 5000 for authenticated)
- Wait 1 hour for limit reset
- Check `x-ratelimit-reset` header for exact reset time

### Variable Not Found Errors
- Verify all variables are set in the collection "Variables" tab
- Check variable names match exactly (case-sensitive)
- Use `pm.collectionVariables.get('variable_name')` in scripts

## API Documentation

- GitHub REST API v3 Docs: https://docs.github.com/en/rest
- Postman Learning: https://learning.postman.com/
- Chai Assertion Library: https://www.chaijs.com/

## License

This test suite is provided as-is for testing and educational purposes.

## Support

For issues with:
- **GitHub API**: Check https://github.com/orgs/github/discussions
- **Postman**: Visit https://postman.com/support
- **Test Suite**: Review assertions in each request's "Tests" tab
