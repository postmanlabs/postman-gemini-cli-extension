---
inclusion: always
---

# Postman MCP Server Guide

This file provides guidance for using the Postman MCP tools available in this session to manage API collections, workspaces, environments, and automated testing workflows.

## When to Use the Postman MCP Server

Use Postman MCP tools when you need to:
- **Create and manage API collections**: Build, update, and organize API request collections
- **Test API endpoints**: Run automated tests with assertions and validations
- **Manage environments**: Create and configure environment variables for different deployment stages
- **Generate collections from source code**: Define your API in Postman
- **Run collection tests**: Execute test suites and analyze results
- **Sync API with Postman**: Keep collections in sync with source code
- **Manage workspaces**: Organize collections and environments by project or team
- **Generate client code**: Produce production-ready code from API definitions (`code` toolset)

## Core Principles

### 0. Always Validate Against Postman Collection Schema

**CRITICAL**: All collections MUST conform to the official Postman Collection Format schema.

- **Schema URL**: `https://schema.getpostman.com/json/collection/v2.1.0/collection.json`
- **Required fields**: `info.name`, `info.schema`, and `item` array
- **Request required fields**: `name`, `request.method`, `request.url`
- **Valid HTTP methods**: GET, POST, PUT, DELETE, PATCH, HEAD, OPTIONS (uppercase)
- **Event listeners**: must be `"test"` or `"prerequest"`; `exec` must be an array of strings

### 1. Check for Existing Resources First

**ALWAYS** check before creating new workspaces or collections:

1. Read `.postman.json` if it exists — extract `workspaceId`, `collectionId`, `environmentId` and verify they still exist
2. Use `getWorkspaces` to search for workspaces matching the project name
3. Use `getCollections` to search for existing collections in the workspace
4. Present findings and ask the user to confirm before creating anything new

### 2. Always Save Configuration

Store all resource IDs in `.postman.json` in the project root:

```json
{
  "version": "0.1",
  "project": { "name": "My API", "baseUrl": "https://api.example.com" },
  "default": { "workspace": "main", "collection": "api-tests", "environment": "local" },
  "workspaces": {
    "main": { "id": "workspace-uuid", "name": "My API Project", "type": "team" }
  },
  "collections": {
    "api-tests": { "id": "collection-uuid", "uid": "userId-collection-uuid", "name": "API Tests", "workspace": "main" }
  },
  "environments": {
    "local": { "id": "env-uuid", "uid": "userId-env-uuid", "name": "Local Development", "workspace": "main" }
  }
}
```

### 3. Use Variables — Never Hardcode Values

**Environment variables** (differ per environment): `{{API_URL}}`, `{{AUTH_TOKEN}}`, `{{API_KEY}}`

**Collection variables** (same across environments): `{{API_VERSION}}`, `{{MAX_RETRIES}}`

**Dynamic variables** (set in test scripts): `{{test_item_id}}`, `{{created_resource_id}}`

```javascript
// Set a variable in a test script
pm.collectionVariables.set('item_id', pm.response.json().id);
pm.environment.set('auth_token', pm.response.json().token);
```

Use `type: "secret"` for sensitive values (API keys, passwords, tokens) — this masks them in the Postman UI.

### 4. Add Comprehensive Tests

Include test scripts in every request:

```javascript
pm.test('Status code is 200', () => pm.response.to.have.status(200));
pm.test('Response has required fields', () => {
    const data = pm.response.json();
    pm.expect(data).to.have.property('id');
    pm.expect(data).to.have.property('name');
});
pm.test('Response time is acceptable', () => pm.expect(pm.response.responseTime).to.be.below(2000));
```

---

## Workflow Patterns

### Pattern 1: Create Workspace and Collection from an API

**STEP 0**: Check for existing resources (required — see Core Principle 1)

**STEP 1**: Analyze the API code or OpenAPI spec — identify endpoints, methods, auth requirements

**STEP 2**: Create or select workspace using `createWorkspace` or an existing `workspaceId`

**STEP 3**: Create or update collection — validate against Collection schema v2.1.0 before calling `createCollection` or `putCollection`

**STEP 4**: Add test scripts as `event` objects with `listen: "test"` to every request

**STEP 5**: Create or update environment with `createEnvironment` or `putEnvironment` — include `API_URL` and auth variables

**STEP 6**: Write all IDs to `.postman.json`

### Pattern 2: Run Collection Tests

**STEP 0**: Verify resources exist — check `.postman.json`, call `getCollection` and `getEnvironment` to confirm they haven't been deleted

**STEP 1**: Confirm the API server is running before executing

**STEP 2**: Run with `runCollection`, passing both `collectionId` and `environmentId`

**STEP 3**: Display results:
```
Overall: 12 tests — 11 passed ✅, 1 failed ❌ (91.7%)

By endpoint:
  GET /items      ✅ 5/5
  POST /items     ✅ 3/3
  GET /items/{id} ⚠️  1/2 — failed: CORS headers check
```

**STEP 4**: If tests fail, investigate and offer to fix API code, test scripts, or environment config

### Pattern 3: Generate Collection from OpenAPI Spec

1. Create a spec with `createSpec` (type `OPENAPI:3.0`, files array with path + content)
2. Generate collection with `generateCollection` (specId, elementType: "collection")
3. Enhance with custom test scripts for business logic
4. Create an environment from the spec's base URL

### Pattern 4: Sync Collection with API Changes

1. Use `putCollection` to replace the collection, or `syncCollectionWithSpec` if using an OpenAPI spec
2. Update test assertions for changed response schemas
3. Re-run the collection to confirm all tests pass

---

## Rules for Test Execution

- **Never use curl** or any other HTTP client — always use `runCollection` via Postman MCP
- The API server must be running before calling `runCollection`
- Always pass an `environmentId` so variables are substituted correctly
- Display a full summary after every run and offer to fix failures

---

## Troubleshooting

**Collection not found**: Call `getCollections` with the workspace ID to list available collections and verify the ID

**Environment variables not substituted**: Ensure `environmentId` is passed to `runCollection`

**Tests failing unexpectedly**: Verify the API server is running, check environment variables (`API_URL`, auth tokens), and review test assertion logic

**Authentication errors in tests**: Verify the auth token/API key variable in the environment, check for token expiration, confirm auth headers are set on the request
