# MovieBox API – Render Ready + Proxy Support (v2.5.4)

## ⚠️ Important: Geo-block on Render

MovieBox blocks most non-India servers (including Render US free instances).

If you see:
```
Failed to bootstrap MovieBox token
```
or
```
MovieBox geo-blocked this server region
```

### Fix (choose one):

**Option A – Best: Use an Indian / Singapore proxy**

1. Get any HTTP proxy located in **India** (or Singapore)
2. On Render → Environment → Add:
   ```
   MOVIEBOX_PROXY = http://username:password@proxy-host:port
   ```
3. Save + Manual Deploy

**Option B – Host on India/Singapore VPS**
- Contabo, Hetzner, DigitalOcean (Bangalore/Singapore), or any cheap Indian VPS
- No proxy needed

**Option C – Railway / Fly.io** (sometimes works better, still may need proxy)

---

## Deploy on Render

1. Upload this repo / zip
2. **Build**: `pip install -r requirements.txt`
3. **Start**: `uvicorn main:app --host 0.0.0.0 --port $PORT`
4. After first deploy, set these env vars:

| Key | Value | Required? |
|-----|-------|---------|
| `PUBLIC_BASE_URL` | `https://your-app.onrender.com` | Recommended |
| `MOVIEBOX_PROXY` | `http://user:pass@indian-proxy:port` | **Yes if geo-blocked** |

5. Manual Deploy again

---

## App Usage

**Search**
```
GET /search?q=Animal
```

**Streams (with ready proxy_url)**
```
GET /api/stream/{subject_id}
GET /api/stream/{subject_id}?se=1&ep=2
```

Response contains:
```json
{
  "quality": "1080p",
  "url": "https://cdn.../file.mp4?sign=...",
  "proxy_url": "https://your-app.onrender.com/proxy?url=...&referer=..."
}
```

App me simply:
```
player.play(stream.proxy_url)
```

---

## Other Endpoints

- `/api/stream/{id}/captions` – subtitles
- `/detail/{id}` – metadata
- `/episodes/{id}` – seasons
- `/proxy?url=...&referer=...` – video proxy
- `/health` – health check
- `/` – web dashboard

---

## Local

```bash
pip install -r requirements.txt
python main.py
```

Optional CLI:
```bash
python cli.py
```

MIT License
