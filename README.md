# Beest Crabby
A slack app made for Beest YSWS. Like Flavouropheus but for beest.

Originally a fork of Master Alchemist by @aoishik, now a separate project.

## Integration with beest the platform

- Use API endpoints and Fetch API from Backend only to avoid CORS

### Endpoints

All authenticated endpoints require the header `Authorization: Bearer <AUTH_BEARER_TOKEN>`.
- DM @TheUtkarsh8939 on Slack for Token 

#### `GET /healthz`
Health check.

Auth: No

Response:
```json
{"status":"ok"}
```

#### `POST /slack/events`
Slack Events API webhook handler.

Auth: No (validated by Slack signature verification in Bolt)

Request: Slack Events API payload

Response: Slack Bolt handler response (challenge/ack)

#### `POST /ship`
Handle a project submission (ship) from a user. Sends a public message to the configured ship channel and DM's the submitting user.

Auth: Yes

Request body:
```json
{
	"user_id": "U12345678",
	"project_name": "My Project",
	"project_link": "https://example.com"
}
```

Response:
```json
{
	"public": {"ok": true, "channel": "C12345678", "ts": "1716000000.000100"},
	"dm": {"ok": true, "channel": "D12345678", "ts": "1716000000.000200"}
}
```

#### `POST /review-accept`
Handle a positive review for a submitted project. Posts a review message to the ship channel with custom reviewer profile (name/avatar) and sends DM notification to the project submitter.

Auth: Yes

Request body:
```json
{
	"user_id": "U12345678",
	"project_name": "My Project",
	"project_link": "https://example.com",
	"reviewer_id": "U87654321",
	"feedback": "Great work!",
	"currencies": "Unused Leave this empty"
}
```

Response:
```json
{"ok": true, "channel": "C12345678", "ts": "1716000000.000300"}
```

#### `POST /review-reject`
Handle a negative review for a submitted project. Posts a review message to the ship channel with custom reviewer profile (name/avatar) and sends DM notification to the project submitter.

Auth: Yes

Request body:
```json
{
	"user_id": "U12345678",
	"project_name": "My Project",
	"project_link": "https://example.com",
	"reviewer_id": "U87654321",
	"feedback": "Needs more documentation."
}
```

Response:
```json
{"ok": true, "channel": "C12345678", "ts": "1716000000.000400"}
```

#### `POST /fulfill_pending`
Send a pending order DM.

Auth: Yes

Request body:
```json
{
	"user_id": "U12345678",
	"order_id": "ORDER-001",
	"item_name": "Sticker Pack",
	"qty": "1",
	"cost": "$5"
}
```

Response:
```json
{"ok": true, "channel": "D12345678", "ts": "1716000000.000500"}
```

#### `POST /fulfill_approved`
- UNUSED so not documented in API spec, but implemented in code. Can be used to send an "approved" order DM if you want to have a 3-step order process (pending -> approved -> fulfilled)

#### `POST /fulfill_fullfilled`
Send a fulfilled order DM.

Auth: Yes

Request body:
```json
{
	"user_id": "U12345678",
	"order_id": "ORDER-001",
	"item_name": "Sticker Pack",
	"qty": "1",
	"cost": "$5"
}
```

Response:
```json
{"ok": true, "channel": "D12345678", "ts": "1716000000.000700"}
```

#### `POST /custom`
Send a custom message to a user or channel.

Auth: Yes

Request body:
```json
{
	"target_id": "U12345678",
	"message": "Hello from Beest Crabby!"
}
```

Response:
```json
{"ok": true, "channel": "D12345678", "ts": "1716000000.000800"}
```

#### `POST /fraud_review_accept`
Send a fraud-review acceptance DM.

Auth: Yes

Request body:
```json
{
	"user_id": "U12345678",
	"project_name": "My Project",
	"project_link": "https://example.com"
}
```

Response:
```json
{"ok": true, "channel": "D12345678", "ts": "1716000000.000900"}
```

#### `POST /fraud_review_reject`
Send a fraud-review rejection DM.

Auth: Yes

Request body:
```json
{
	"user_id": "U12345678",
	"project_name": "My Project",
	"project_link": "https://example.com"
}
```

Response:
```json
{"ok": true, "channel": "D12345678", "ts": "1716000000.001000"}
```