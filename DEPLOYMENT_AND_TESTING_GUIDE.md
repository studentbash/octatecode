# Complete Testing & Deployment Guide

**Date:** February 10, 2026
**Frontend P2P Migration v2.0.0**

---

## Pre-Flight Checklist ✅

Before testing, ensure:

- [ ] Backend running: `cd p2p-backend && npm run dev`
- [ ] Backend port 3000 accessible: `curl http://localhost:3000/api/health`
- [ ] Backend .env created with AUTH_SECRET
- [ ] Frontend dependencies installed: `npm install`
- [ ] No compile errors: `npm run build`
- [ ] WebRTC supported browser (Chrome, Firefox, Edge, Safari 10+)

---

## Quick Start (5 Minutes)

### Terminal 1: Backend
```bash
cd p2p-backend
npm install  # if needed
npm run dev
# Output: Listening on port 3000 + 3001 ✓
```

### Terminal 2: Frontend Build
```bash
npm run watch-clientd &
npm run watchreactd &
npm run watch-extensionsd &
# Watch for compilation ✓
```

### Terminal 3: Dev Server
```bash
.\scripts\code.bat
# Dev window opens ✓
```

### Browser: Test
1. Open 2 windows
2. Window 1: Create collaboration room
3. Window 2: Join with room code
4. Both: Check console for `[Collab P2P]` messages
5. Window 1: Edit text
6. Window 2: Should see edit instantly

---

## Detailed Testing (30 Minutes)

### Phase 1: Token Generation (5 min)

**Step 1: Manual Token Test**
```bash
# In a new terminal
curl -X POST http://localhost:3000/api/auth/token \
  -H "Content-Type: application/json" \
  -d '{
    "userId": "test-user",
    "roomId": "test-room"
  }'
```

**Expected Response:**
```json
{
  "status": "success",
  "token": "eyJ0eXAiOiJKV1QiLCJhbGc...",
  "userId": "test-user",
  "roomId": "test-room",
  "expiresIn": "1 hour"
}
```

**Verification:**
- [ ] Status is "success"
- [ ] Token is a string (not null)
- [ ] Token length > 20 chars
- [ ] ExpiresIn matches backend (.env AUTH_SECRET)

**If Failed:**
```
Error: "POST /api/auth/token 404"
→ Backend /api/auth/token endpoint missing
→ Check CHANGES.md for endpoint code

Error: "Invalid request"
→ Missing userId or roomId in body
→ Check curl command
```

---

### Phase 2: WebSocket Connection (5 min)

**Step 1: Open Dev Tools**
- Open Chrome/Firefox dev tools (F12)
- Go to Console tab
- Keep it visible

**Step 2: Start Collaboration**
- Click "Create Collaboration Room"
- Enter room name, your name
- Click "Create"

**Expected Console Output:**
```
[Collab] Connected to server ✓
[Collab] Generating auth token...
[Collab] User joined: (you)
(WebRTC setup...)
```

**Verification:**
- [ ] "Connected to server" appears
- [ ] "Generating auth token" appears
- [ ] No "Authentication failed" error
- [ ] Session started successfully

**If Failed:**

```
Error: "Missing auth token"
→ Token generation failed
→ Check /api/auth/token returns token
→ Check HTTP URL is correct

Error: "WebSocket connection failed"
→ Backend not running
→ Check: npm run dev in p2p-backend

Error: "Timeout waiting for auth"
→ Token validation on backend failed
→ Check AUTH_SECRET in .env matches
```

---

### Phase 3: P2P Connection (10 min)

**Step 1: Second User Joins**
- Open second browser window
- Join with room code from first window
- Console should show similar messages

**Expected Console Output (Second User):**
```
[Collab] Connected to server ✓
[Collab] Generating auth token...
[Collab] User joined: (first user)
[Collab P2P] Initiated connection to (first user)
[Collab P2P] Data channel opened with (first user)
```

**Verification:**
- [ ] "Data channel opened" appears
- [ ] No P2P errors in console
- [ ] Connection established <3 seconds
- [ ] Both users show each other as joined

**If Failed:**

```
Error: "No [Collab P2P] messages"
→ P2P not starting
→ Check both users authenticated

Error: "[Collab P2P] Failed to setup peer connection"
→ WebRTC configuration issue
→ Check browser WebRTC support
→ Try: chrome://webrtc-internals

Error: "Data channel didn't open"
→ Firewall blocking WebRTC
→ Try VPN or different network
→ Check router allows UDP
```

---

### Phase 4: Operation Sync (10 min)

**Step 1: Edit in First Window**
1. Click in editor
2. Type some text: "Hello P2P!"
3. Watch console for:
```
[Collab P2P] Sent operation to peer (user) via P2P
```

**Step 2: Check Second Window**
```
[Collab P2P] Received operation from (user)
```

**Step 3: Verify**
- [ ] Text appears instantly in both windows
- [ ] Latency < 50ms (check timestamp in console)
- [ ] No server routing (check network tab - no operation posts)
- [ ] Resolve conflicts: Both edit same line simultaneously

**If Failed:**

```
Error: "Sent operation to peer" but not received
→ Data channel closed
→ Check console for "Data channel closed"

Error: "Sent via server" (not P2P)
→ Check: console doesn't show "[Collab P2P] Sent"
→ Operations are being queued on server
→ Verify data channels are open

Error: "Operations not syncing"
→ Check network tab - should NOT see operation POST
→ Should only see WebSocket signaling
→ Verify CRDT/OT algorithm in receiving user
```

---

### Phase 5: Stress Testing (5 min)

**Step 1: 3+ Users**
1. Open 3rd browser window
2. Join same room
3. All three should connect P2P

**Expected:**
```
User 1 ↔ P2P ↔ User 2
    ↖       ↗
       User 3
```

**Verification:**
- [ ] Three users can edit simultaneously
- [ ] Edits sync to all three
- [ ] No duplicate operations
- [ ] Server memory stays low (check backend logs)

**Step 2: Rapid Editing**
1. User 1: Edit line 1
2. User 2: Edit line 2
3. User 3: Edit line 3
4. Repeat 10 times

**Verification:**
- [ ] No lost operations
- [ ] Conflict resolution works (OT handles)
- [ ] Final state is consistent

**Step 3: Large Document**
1. Create 1000+ line document
2. All users edit different parts
3. Verify sync completes

---

## Performance Benchmarking

### Metrics to Monitor

**Browser Dev Tools → Performance tab:**

1. **Operation Latency**
   - Measure time from edit to remote appearance
   - Expected: <10ms (local), <50ms (remote)
   - Bad: >500ms (indicates server routing)

2. **Memory Usage**
   - Monitor Memory tab with 3 users
   - Expected: <100MB per user
   - Bad: >500MB (memory leak)

3. **CPU Usage**
   - Monitor Performance tab during rapid edits
   - Expected: <20% CPU
   - Bad: >80% (inefficient OT)

### Backend Monitoring

```bash
# Terminal: Monitor server memory
watch -n 1 'ps aux | grep node'
# Expected: <50MB RAM with 3 users
# Bad: >200MB (operation storage issue)
```

---

## Troubleshooting Guide

### Network Issues

**Problem: "Cannot reach backend"**
```bash
# Check 1: Backend running
curl http://localhost:3000/api/health
# Should return: {"status":"ok"}

# Check 2: Port accessible
netstat -tlnp | grep 3000
# Should show node listening on 3000

# Check 3: Firewall
# Windows: Check Windows Defender Firewall
# Mac: System Preferences → Security & Privacy
# Linux: sudo ufw allow 3000
```

**Problem: "WebSocket connection fails"**
```bash
# Check: Backend listening on 3001
netstat -tlnp | grep 3001
# Should show signaling server listening

# Test: WebSocket directly
# Use: https://www.websocket.org/echo.html
# Connect to: ws://localhost:3001
```

### Browser Issues

**Problem: "WebRTC not working"**
```
Check 1: Browser support
chrome://webrtc-internals  (Chrome)
about:webrtc              (Firefox)

Check 2: Permission granted
- Allow camera/microphone if prompted
- Check: chrome://settings/content/camera

Check 3: STUN server reachable
- Dev tools → Network → look for STUN requests
- Google STUN is in code: stun:stun.l.google.com:19302
```

**Problem: "Empty operations from P2P"**
```
Check 1: Enable debug logs
- Open console
- Look for [Collab P2P] messages
- Should show operation details

Check 2: Check OT transformation
- Verify: OperationalTransform.transform()
- Check: operationalTransform.ts file
```

### Auth Issues

**Problem: "Authentication failed"**
```bash
# Check 1: Token endpoint works
curl -X POST http://localhost:3000/api/auth/token \
  -H "Content-Type: application/json" \
  -d '{"userId":"test","roomId":"test"}'

# Check 2: AUTH_SECRET set
grep AUTH_SECRET p2p-backend/.env
# Should not be empty

# Check 3: Token validation in server
# Check: src/signalingServer.ts auth handler
# Verify: authManager.validateCredentials()
```

---

## Common Errors & Fixes

| Error | Cause | Fix |
|-------|-------|-----|
| `Missing auth token` | Token gen failed | Check /api/auth/token works |
| `Cannot read property 'send' of null` | WebSocket not connected | Verify backend on 3001 |
| `No [Collab P2P] messages` | P2P not starting | Check both users authenticated |
| `Data channel didn't open` | Firewall/NAT | Try VPN, check UFW |
| `Operations not syncing` | Data channels closed | Monitor console for disconnects |
| `High latency (>500ms)` | Still using server | Check for server operation routing |
| `Memory keeps growing` | Leak in WebRTC | Monitor RTCPeerConnection cleanup |

---

## Deployment Checklist

### Pre-Deployment
- [ ] All tests pass (above checklist)
- [ ] No TypeScript errors: `npm run build`
- [ ] No console warnings
- [ ] Performance acceptable
- [ ] 3+ user test successful

### Backend Deployment
- [ ] Push backend to production
- [ ] Set AUTH_SECRET environment variable
- [ ] Update PORT if using custom
- [ ] Test `/api/auth/token` endpoint
- [ ] Enable HTTPS (WSS) if required

### Frontend Deployment
- [ ] Build frontend: `npm run compile`
- [ ] Update backend URL in config
- [ ] Test token generation in production
- [ ] Test P2P with production backend
- [ ] Monitor error logs

### Post-Deployment
- [ ] Monitor server memory (should stay <50MB)
- [ ] Monitor CPU usage
- [ ] Check error logs daily
- [ ] Test new features work
- [ ] Gather user feedback

---

## Performance Optimization Tips

### If Latency is High:
1. Reduce cursor broadcast interval (in code it's 50ms)
2. Check network latency: `ping your-server`
3. Use TURN servers for NAT traversal
4. Move server closer (regional deployment)

### If Memory is High:
1. Check DataChannels cleanup in handleMessage
2. Verify Peer connections close on user-left
3. Monitor: `process.memoryUsage()` in backend
4. Check for WebRTC connection leaks

### If CPU is High:
1. Reduce OT transformation frequency
2. Batch operations instead of sending individually
3. Optimize document model
4. Profile with Chrome DevTools

---

## Success Criteria

✅ **Phase 1: Token Generation**
- Token endpoint accessible
- Token valid for 1 hour
- Different tokens for different rooms

✅ **Phase 2: WebSocket Auth**
- Connection established
- Token validation passes
- Invalid tokens rejected

✅ **Phase 3: P2P Setup**
- Offer/Answer exchange completes
- ICE candidates exchanged
- Data channels open

✅ **Phase 4: Operation Sync**
- Operations sent via P2P
- Operations received via P2P
- Latency <50ms
- Server not broadcasting operations

✅ **Phase 5: Multi-User**
- 3+ users can collaborate
- Conflict resolution works
- Consistent final state
- Server memory low

✅ **Phase 6: Stability**
- No disconnections
- Reconnection works
- Long sessions stable
- No memory leaks

---

## Support & Debugging

### Enable Verbose Logging
Edit `collaborationSyncService.ts`:
```typescript
private handleMessage(rawData: string): void {
  console.log('[DEBUG] Message:', rawData);  // Add this
  try {
    const message = JSON.parse(rawData);
    // ... rest of code
  }
}
```

### Test P2P Directly
```javascript
// In browser console
manager.getStats()
// Returns connection status, session info, operation count
```

### Monitor Network Traffic
```
Chrome Dev Tools → Network → WS (WebSocket tab)
Should see:
- /ws (WebSocket connection)
- webrtc-offer (signaling)
- webrtc-answer (signaling)
- webrtc-ice-candidate (ICE)

Should NOT see:
- /api/operations (means P2P not working)
```

---

## Documentation Files

Reference these for detailed info:
- `FRONTEND_P2P_MIGRATION.md` - Complete migration guide
- `FRONTEND_CHANGES_QUICK_REF.md` - Quick reference
- `FRONTEND_CODE_CHANGES_DETAILED.md` - Line-by-line changes
- `p2p-backend/CHANGES.md` - Backend changes
- `p2p-backend/P2P_ARCHITECTURE.md` - Architecture overview
- `p2p-backend/USER_AUTHENTICATION.md` - Auth flow details

---

**Status:** ✅ Ready to Deploy
**Last Updated:** February 10, 2026
**Next Step:** Follow "Quick Start" above

