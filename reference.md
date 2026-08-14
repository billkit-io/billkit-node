# Reference
## Addons
<details><summary><code>client.addons.<a href="/src/api/resources/addons/client/Client.ts">attachAddon</a>({ ...params }) -> BillkitApi.AttachAddonResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Validates that the add-on key exists in the tenant's schema, attaches it to
the subject's existing assignment in the store, and invalidates the subject's
cache entries.

- If the request body is missing or malformed: returns 400 (via Axum rejection).
- If subject_id or addon_key is empty: returns 400.
- If the tenant has no schema uploaded: returns 422.
- If the add-on key does not exist in the schema: returns 422.
- If the subject has no existing assignment: returns 404.
- If a store or cache error occurs: returns 500.
- On success: returns 200 with the updated add-on list.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.addons.attachAddon({
    addon_key: "addon_key",
    subject_id: "subject_id"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `BillkitApi.AttachAddonRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `AddonsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## Assignments
<details><summary><code>client.assignments.<a href="/src/api/resources/assignments/client/Client.ts">createAssignment</a>({ ...params }) -> BillkitApi.CreateAssignmentResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Assigns a plan to a subject. Validates that the plan key exists in the tenant's
schema, writes the assignment to the store, and invalidates the subject's cache entries.

- If the request body is missing or malformed: returns 400.
- If the subject_id exceeds 256 characters: returns 400.
- If the tenant has no schema uploaded: returns 422.
- If the plan key does not exist in the schema: returns 422.
- If a store or cache error occurs: returns 500.
- On success: returns 201 with the assignment details.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.assignments.createAssignment({
    plan_key: "plan_key",
    subject_id: "subject_id"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `BillkitApi.CreateAssignmentRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `AssignmentsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.assignments.<a href="/src/api/resources/assignments/client/Client.ts">deleteAssignment</a>({ ...params }) -> void</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Removes the subject assignment from the store and invalidates the subject's
cache entries. Tenant is resolved from `X-Api-Key` via the auth middleware.

- If subject_id is empty: returns 400.
- If a store error occurs: returns 500.
- On success: returns 204 No Content (regardless of whether the assignment existed).
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.assignments.deleteAssignment({
    subject_id: "subject_id"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `BillkitApi.DeleteAssignmentRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `AssignmentsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## Billing
<details><summary><code>client.billing.<a href="/src/api/resources/billing/client/Client.ts">createPortalToken</a>({ ...params }) -> BillkitApi.CreatePortalTokenResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Generates a signed JWT that grants a specific subject access to the hosted
billing portal. The token is scoped to both the tenant and the subject,
preventing cross-tenant or cross-subject access.

## Authentication
Requires API key authentication (X-Api-Key header). Only the tenant's
backend should call this endpoint.

## Request Body
```json
{ "subject_id": "user_123" }
```

## Response (200)
```json
{
  "token": "eyJ...",
  "portal_url": "https://billing.billkit.io/acme-corp/manage?token=eyJ...",
  "expires_at": "2024-01-15T12:30:00Z"
}
```

## Errors
- 400: Missing or empty `subject_id`
- 404: Subject not found or not assigned to this tenant
- 500: Internal error (JWT signing failure, store error)
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.billing.createPortalToken({
    subject_id: "subject_id"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `BillkitApi.CreatePortalTokenRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `BillingClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## Entitlements
<details><summary><code>client.entitlements.<a href="/src/api/resources/entitlements/client/Client.ts">check</a>({ ...params }) -> BillkitApi.GateResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Authenticates via the auth middleware (X-Api-Key), resolves the entitlement
configuration from cache/store, invokes the gate decision evaluator, increments
the usage counter in cache for metered features, and returns a `GateResponse`.

- If subject_id or feature_code is empty: returns 400.
- If feature_code not found in schema: returns 404.
- If subject has no plan assigned: returns 200 with `decision: "no_plan"`.
- If a store error occurs: returns 500.
- On success: returns 200 with the gate decision.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.entitlements.check({
    feature_code: "feature_code",
    subject_id: "subject_id"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `BillkitApi.CheckRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `EntitlementsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## Contracts
<details><summary><code>client.contracts.<a href="/src/api/resources/contracts/client/Client.ts">applyContract</a>({ ...params }) -> BillkitApi.ApplyContractResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Validates that the contract key exists in the tenant's schema, applies it to
the subject's existing assignment in the store, and invalidates the subject's
cache entries.

- If the request body is missing or malformed: returns 400 (via Axum rejection).
- If subject_id or contract_key is empty: returns 400.
- If the tenant has no schema uploaded: returns 422.
- If the contract key does not exist in the schema: returns 422.
- If the subject has no existing assignment: returns 404.
- If a store or cache error occurs: returns 500.
- On success: returns 200 with the applied contract details.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.contracts.applyContract({
    contract_key: "contract_key",
    subject_id: "subject_id"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `BillkitApi.ApplyContractRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `ContractsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## Invoices
<details><summary><code>client.invoices.<a href="/src/api/resources/invoices/client/Client.ts">listInvoices</a>({ ...params }) -> BillkitApi.ListInvoicesResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Lists end-user invoices for the authenticated tenant with optional filtering
by subject_id, status, and date range.

Query parameters:
- `subject_id` — filter by subject
- `status` — filter by invoice status (open, paid, past_due, uncollectible)
- `start_date` — ISO 8601 datetime; only invoices overlapping this date or later
- `end_date` — ISO 8601 datetime; only invoices overlapping this date or earlier
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.invoices.listInvoices();

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `BillkitApi.ListInvoicesRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `InvoicesClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.invoices.<a href="/src/api/resources/invoices/client/Client.ts">previewInvoice</a>({ ...params }) -> BillkitApi.InvoicePreviewResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Calculates what the next invoice would look like for a subject based on
their current usage, plan assignment, and schema billing configuration.

Returns:
- 200 with the preview if billable usage exists
- 404 if the subject is not found or has no plan assignment
- 422 if no billing config or Connect account is configured
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.invoices.previewInvoice({
    subject_id: "subject_id"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `BillkitApi.PreviewInvoiceRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `InvoicesClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## API Keys
<details><summary><code>client.apiKeys.<a href="/src/api/resources/apiKeys/client/Client.ts">listKeys</a>() -> BillkitApi.ListKeysResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Lists all named API keys for the tenant. Keys are masked (only prefix shown).
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.apiKeys.listKeys();

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**requestOptions:** `ApiKeysClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.apiKeys.<a href="/src/api/resources/apiKeys/client/Client.ts">createKey</a>({ ...params }) -> BillkitApi.CreateKeyResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Creates a new named API key for the tenant. The raw key value is returned
once in the response and never stored — only its SHA-256 hash is persisted.

Limits tenants to MAX_KEYS_PER_TENANT keys.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.apiKeys.createKey({
    name: "name"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `BillkitApi.CreateKeyRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `ApiKeysClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.apiKeys.<a href="/src/api/resources/apiKeys/client/Client.ts">rotateKey</a>() -> BillkitApi.RotateKeyResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Generates a new API key, updates the tenant record in the store, replaces
the API key reference, updates cache mappings, and invalidates the old key.
After rotation, the old key will be rejected with 401 on subsequent requests.

- If the tenant record cannot be found: returns 500 (should not happen since
  auth middleware already resolved the tenant).
- If a store or cache error occurs: returns 500.
- On success: returns 200 with the new API key (shown only once).
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.apiKeys.rotateKey();

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**requestOptions:** `ApiKeysClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.apiKeys.<a href="/src/api/resources/apiKeys/client/Client.ts">deleteKey</a>({ ...params }) -> void</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Revokes (deletes) a specific API key by its key_id. Removes both the
tenant-scoped record and the global API key reference, and invalidates
the key in cache.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.apiKeys.deleteKey({
    key_id: "key_id"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `BillkitApi.DeleteKeyRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `ApiKeysClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## Pricing
<details><summary><code>client.pricing.<a href="/src/api/resources/pricing/client/Client.ts">getPricing</a>() -> BillkitApi.PricingResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns the authenticated tenant's pricing information formatted for a
pricing page. Plans are sorted by price ascending, features are ordered
by type (boolean → metered → stateful → attribute), and all values are
formatted for direct display.

Supports conditional requests via `ETag` / `If-None-Match` headers.
Returns 304 Not Modified when the schema has not changed.

Responses are cached in Valkey with a 15-minute sliding TTL. Cache is
checked before DynamoDB to minimize latency for hot pricing pages.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.pricing.getPricing();

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**requestOptions:** `PricingClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## Schema
<details><summary><code>client.schema.<a href="/src/api/resources/schema/client/Client.ts">getSchema</a>() -> string</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Retrieves the tenant's own schema document from the store.
Tenant is resolved from `X-Api-Key` via the auth middleware.

- If the tenant has a schema: returns 200 with the schema document body (as stored).
- If the tenant has no schema: returns 404 with `{ "error": "schema not found" }`.
- If a store error occurs: returns 500 with `{ "error": "internal error" }`.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.schema.getSchema();

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**requestOptions:** `SchemaClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.schema.<a href="/src/api/resources/schema/client/Client.ts">putSchema</a>() -> BillkitApi.ValidationErrorResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Validates, persists to store, and invalidates all tenant cache entries.
Tenant is resolved from `X-Api-Key` via the auth middleware.

- If the body is empty: returns 400.
- If the document has validation errors: returns 200 with `{ "valid": false, "errors": [...] }` (not persisted).
- If the document is valid: persists to store, invalidates cache, returns 200 with `{ "valid": true, "errors": [] }`.
- If a store or cache error occurs: returns 500.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.schema.putSchema();

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**requestOptions:** `SchemaClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.schema.<a href="/src/api/resources/schema/client/Client.ts">validateSchema</a>() -> BillkitApi.ValidationErrorResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Accepts a raw schema document body (YAML or JSON), runs the full validator
(structural + semantic), and returns a `ValidationErrorResponse` without
persisting anything.

- If the document is valid: returns 200 with `{ "valid": true, "errors": [] }`.
- If the document has validation errors: returns 200 with `{ "valid": false, "errors": [...] }`.
- If the body is empty: returns 400.

This endpoint is protected by the auth middleware (X-Api-Key), but does not
use the tenant context since validation is stateless.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.schema.validateSchema();

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**requestOptions:** `SchemaClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## Subjects
<details><summary><code>client.subjects.<a href="/src/api/resources/subjects/client/Client.ts">listSubjects</a>() -> BillkitApi.SubjectsResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns all subject assignments for the authenticated tenant.
The tenant is resolved from `X-Api-Key` via the auth middleware.

- If no subjects exist: returns HTTP 200 with an empty `subjects` array.
- If authenticated with the system API key: returns all subjects under the system tenant.
- If no valid API key is provided: returns HTTP 401 (handled by auth middleware).
- If a DynamoDB failure occurs: returns HTTP 500.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.subjects.listSubjects();

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**requestOptions:** `SubjectsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.subjects.<a href="/src/api/resources/subjects/client/Client.ts">registerSubject</a>({ ...params }) -> BillkitApi.RegisterSubjectResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Registers a subject with optional email and name. If an email is provided
and the tenant has an active Stripe Connect account, creates a Stripe Customer
on the connected account and marks the subject as "billable".

If the subject already exists (re-registration), updates the Stripe Customer's
email/name rather than creating a duplicate.

- If subject_id is missing or too long: returns 400.
- If the subject already exists: updates email/name/Stripe Customer (idempotent).
- If no Stripe Connect account is active: subject is stored without Stripe Customer.
- If Stripe API fails: returns 502 (subject is NOT partially created).
- On success: returns 201 (new) or 200 (re-registration).
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.subjects.registerSubject({
    subject_id: "subject_id"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `BillkitApi.RegisterSubjectRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `SubjectsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.subjects.<a href="/src/api/resources/subjects/client/Client.ts">registerSubjectsBatch</a>({ ...params }) -> BillkitApi.BatchRegisterResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Registers multiple subjects in a single request. Creates Stripe Customers
for each subject that has an email and where the tenant has active Connect.

- If the batch exceeds 100 subjects: returns 400.
- Individual failures are reported per-subject (no rollback).
- On success: returns 200 with results array.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.subjects.registerSubjectsBatch({
    subjects: [{
            subject_id: "subject_id"
        }]
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `BillkitApi.BatchRegisterRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `SubjectsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

<details><summary><code>client.subjects.<a href="/src/api/resources/subjects/client/Client.ts">getSubject</a>({ ...params }) -> BillkitApi.SubjectDetailResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Returns a single subject's details including plan, billing status,
payment method status, and Stripe Customer ID.

- If the subject does not exist: returns 404.
- On success: returns 200 with full subject details.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.subjects.getSubject({
    subject_id: "subject_id"
});

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**request:** `BillkitApi.GetSubjectRequest` 
    
</dd>
</dl>

<dl>
<dd>

**requestOptions:** `SubjectsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

## Tenants
<details><summary><code>client.tenants.<a href="/src/api/resources/tenants/client/Client.ts">getTenantMe</a>() -> BillkitApi.TenantMeResponse</code></summary>
<dl>
<dd>

#### 📝 Description

<dl>
<dd>

<dl>
<dd>

Protected by the auth middleware. Returns metadata about the authenticated
user's tenant. The tenant_id is resolved by the auth middleware, either from:
1. The JWT `custom:tenant_id` claim (if present), or
2. The `USER#{sub}` → `tenant_id` mapping in DynamoDB (the common path).

This endpoint then verifies the tenant record itself exists in DynamoDB and
returns its metadata.

- 200: tenant record found — returns tenant metadata (tenant_id, org_id, plan_key, created_at).
- 404: tenant_id was resolved from the user link but the TenantRecord is missing.
  This can happen after a partial database reset or data corruption.
- 401: (from auth middleware) user has no tenant association — either no
  `custom:tenant_id` claim and no USER#{sub} link in DynamoDB. This is the
  normal state for users who haven't provisioned yet, or after a full DB reset.
- 500: unexpected store error.
</dd>
</dl>
</dd>
</dl>

#### 🔌 Usage

<dl>
<dd>

<dl>
<dd>

```typescript
await client.tenants.getTenantMe();

```
</dd>
</dl>
</dd>
</dl>

#### ⚙️ Parameters

<dl>
<dd>

<dl>
<dd>

**requestOptions:** `TenantsClient.RequestOptions` 
    
</dd>
</dl>
</dd>
</dl>


</dd>
</dl>
</details>

