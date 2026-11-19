# Now Playing: Spotify login callback

A static page, served by GitHub Pages, that finishes the Spotify login for an ESP32 "now playing" display.

Spotify only redirects to HTTPS addresses, and the display is a plain HTTP device on the local network. So Spotify redirects here instead, and this page forwards the login result to the display.

## How it works

1. The display starts a PKCE login with `state` set to `<host>[:port]~<nonce>`, where host is its own local IP address.
2. After login, Spotify redirects here with `?code=...&state=...`, or with `?error=...`.
3. The page checks that the host is a local-network address or `localhost`, then redirects to `http://<host>/callback` with the same `code` (or `error`) and `state`.
4. The display checks the nonce and exchanges the code for tokens.

The page also shows the code, so it can be entered on the display by hand if the redirect fails.

## Why nothing here is secret

The login uses PKCE, so there's no client secret. The code can't be used without the code verifier, which never leaves the display. The page loads no outside scripts and sends no data anywhere except the display.
