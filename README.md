# SignalMe — built with Dropframe

Trusted Web Activity (TWA) Android wrapper for **https://tanstack-start-app.nnadigideon20.workers.dev/**.

## One-time setup (5 min)

1. **Create a new GitHub repo** and push these files:
   ```bash
   git init && git add . && git commit -m "init"
   git branch -M main
   git remote add origin https://github.com/<you>/tanstackstartapp.git
   git push -u origin main
   ```

2. **Generate a signing keystore** (once, reuse forever):
   ```bash
   keytool -genkey -v -keystore release.jks -alias upload \
     -keyalg RSA -keysize 2048 -validity 10000
   base64 release.jks > keystore.b64
   ```

3. **Add GitHub secrets** (Settings → Secrets → Actions):
   - `KEYSTORE_BASE64` — contents of keystore.b64
   - `KEYSTORE_PASSWORD`
   - `KEY_ALIAS` (e.g. `upload`)
   - `KEY_PASSWORD`

4. **Push.** GitHub Actions builds your **.apk** + **.aab** automatically.
   Download from the Actions tab → latest run → Artifacts.

5. **Verify Digital Asset Links** on **https://tanstack-start-app.nnadigideon20.workers.dev/** so your URL bar hides:
   Place this at `/.well-known/assetlinks.json` on your site:
   ```json
   [{
     "relation": ["delegate_permission/common.handle_all_urls"],
     "target": { "namespace": "android_app",
       "package_name": "app.dropframe.dev.workers.nnadigideon20.tanstackstartapp",
       "sha256_cert_fingerprints": ["<run: keytool -list -v -keystore release.jks>"] }
   }]
   ```

6. **Upload the .aab to Google Play Console.** Done — every push rebuilds.
