User clicks “Continue with Google” on Spotify, and Spotify redirects the user’s browser to Google’s OAuth authorization endpoint with its client_id, requested scopes (email/profile), and callback URL.

Google shows its login page, where the user authenticates directly with Google by entering their Google credentials (Spotify never sees these credentials).

After successful login, Google displays a consent screen asking whether the user allows Spotify to access specific information (like email and profile).

When the user clicks Allow, Google creates a temporary authorization code representing the user’s approval and redirects the browser back to Spotify’s callback URL carrying ?code=AUTH_CODE.

The user’s browser sends this authorization code to Spotify’s backend server when loading the callback URL, informing Spotify that Google approval was granted.

Spotify backend securely sends the authorization code, along with its client_id and client_secret, to Google’s token endpoint through a server-to-server request to verify authenticity.

Google validates the authorization code, confirms Spotify’s identity, and returns an ID Token (JWT containing user identity), an Access Token (short-lived API permission), and optionally a Refresh Token (long-lived renewal credential).

Spotify verifies the ID Token by checking Google’s cryptographic signature to ensure the identity information truly came from Google and was not modified.

After verification, Spotify extracts user details (email, Google user ID) from the ID Token and either creates a new Spotify account or logs the existing user into Spotify.

Spotify generates its own session token (usually a Spotify JWT) representing the user’s authenticated session within Spotify’s system.

Spotify sends this Spotify JWT back to the user’s browser (via cookie or response), establishing the user’s logged-in state inside Spotify.

For all future requests, the browser sends the Spotify JWT to Spotify APIs, and Spotify validates the token signature to authenticate the user without contacting Google again.

If Google access tokens expire later, Spotify can use the stored refresh token to silently request new access tokens from Google without requiring the user to log in again.
