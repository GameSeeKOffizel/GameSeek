# 🔌 API Reference

GameSeek uses a combination of **WebSocket** messages and **HTTP** endpoints.

-----

## WebSocket Events

All WebSocket messages are JSON objects with a `type` field.

### Client → Server

|Type            |Description                     |
|----------------|--------------------------------|
|`send_message`  |Send a chat message             |
|`edit_message`  |Edit an existing message        |
|`delete_message`|Delete a message                |
|`join_channel`  |Join a voice/text channel       |
|`leave_channel` |Leave a channel                 |
|`webrtc_offer`  |Send WebRTC offer (voice/screen)|
|`webrtc_answer` |Send WebRTC answer              |
|`webrtc_ice`    |Send ICE candidate              |

### Server → Client

|Type             |Description               |
|-----------------|--------------------------|
|`message`        |New chat message broadcast|
|`message_edited` |Edited message broadcast  |
|`message_deleted`|Deleted message broadcast |
|`user_joined`    |User joined channel       |
|`user_left`      |User left channel         |
|`webrtc_offer`   |Forwarded WebRTC offer    |
|`webrtc_answer`  |Forwarded WebRTC answer   |
|`webrtc_ice`     |Forwarded ICE candidate   |

-----

## HTTP Endpoints

Base URL: `https://gameseekapp.xyz` (production) or `http://localhost:8080` (local)

### Auth

|Method|Endpoint       |Description        |
|------|---------------|-------------------|
|`POST`|`/register`    |Register new user  |
|`POST`|`/verify-email`|Verify 6-digit code|
|`POST`|`/login`       |Login              |
|`POST`|`/login-verify`|Verify login code  |

### Support

|Method|Endpoint  |Description          |
|------|----------|---------------------|
|`POST`|`/support`|Submit support ticket|

-----

## Support Ticket Format

```json
{
  "ticket_id": "GS-A1B2C3D4",
  "name": "Max Mustermann",
  "email": "user@example.com",
  "subject": "Login issue",
  "message": "I cannot log in to my account."
}
```

Ticket IDs are generated client-side in `GS-XXXXXXXX` format.
