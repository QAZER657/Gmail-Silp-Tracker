# Google OAuth Setup Guide

## Fix "Failed to initialize Google authentication" Error

To resolve the Google authentication error, you need to configure Google OAuth credentials.

### Step 1: Create Google Cloud Project

1. Go to [Google Cloud Console](https://console.cloud.google.com/)
2. Create a new project or select an existing one
3. Make sure billing is enabled (required for API usage)

### Step 2: Enable Gmail API

1. Go to "APIs & Services" > "Library"
2. Search for "Gmail API"
3. Click on "Gmail API" and press "Enable"

### Step 3: Configure OAuth Consent Screen

1. Go to "APIs & Services" > "OAuth consent screen"
2. Choose "External" user type
3. Fill in the required information:
   - App name: "Mail Archivist" (or your preferred name)
   - User support email: your email
   - Developer contact information: your email
4. Add scopes:
   - Click "Add or Remove Scopes"
   - Search for "Gmail API"
   - Select "https://www.googleapis.com/auth/gmail.readonly"
5. Add test users (your email address)
6. Save and continue

### Step 4: Create OAuth 2.0 Credentials

1. Go to "APIs & Services" > "Credentials"
2. Click "Create Credentials" > "OAuth 2.0 Client IDs"
3. Choose "Web application" as the application type
4. Name: "Mail Archivist Web Client"

### Step 5: Configure OAuth Client

1. **Authorized JavaScript origins:**
   ```
   http://localhost:8080
   http://localhost:3000
   http://127.0.0.1:8080
   http://127.0.0.1:3000
   ```

2. **Authorized redirect URIs:**
   ```
   http://localhost:8080
   http://localhost:3000
   http://127.0.0.1:8080
   http://127.0.0.1:3000
   ```

3. Click "Create"

### Step 6: Get Your Client ID

After creating the OAuth client, you'll get a Client ID that looks like:
```
123456789-abcdefghijklmnopqrstuvwxyz.apps.googleusercontent.com
```

### Step 7: Configure Environment Variables

1. Create a `.env` file in the project root (same level as `package.json`)
2. Add your client ID:

```env
VITE_GMAIL_CLIENT_ID=469338151253-lnh7o96v2serrkm89nd3tk7l8fj4k6kt.apps.googleusercontent.com
```

Replace `your_client_id_here` with the actual Client ID from Step 5.

### Step 8: Restart Development Server

1. Stop the current development server (Ctrl+C)
2. Restart it with: `npm run dev`

### Step 9: Test the Connection

1. Open your application at `http://localhost:8080`
2. Open browser console (F12) to see debug logs
3. Click "Connect to Gmail"
4. You should now see the Google OAuth consent screen
5. Sign in with your Google account
6. Grant the requested permissions

## Troubleshooting

### Common Issues:

1. **"Invalid client" error**: 
   - Double-check your Client ID in the `.env` file
   - Make sure there are no extra spaces or characters
   - Verify the Client ID matches exactly what's in Google Console

2. **"Redirect URI mismatch"**: 
   - Make sure the redirect URIs in Google Console match your local URLs exactly
   - Include both `localhost` and `127.0.0.1` variants

3. **"API not enabled"**: 
   - Ensure Gmail API is enabled in your Google Cloud project
   - Check that the OAuth consent screen is configured

4. **"Quota exceeded"**: 
   - Check your Google Cloud billing and quotas
   - Gmail API has daily quotas that might be exceeded

5. **"Failed to initialize Google authentication"**:
   - Check browser console for detailed error messages
   - Verify the environment variable is loaded (check console log)
   - Make sure the OAuth consent screen is published or you're a test user
   - Ensure the Gmail API scope is added to the consent screen

### Debug Steps:

1. **Check Environment Variable**:
   - Open browser console (F12)
   - Look for "Environment Client ID:" log message
   - It should show your actual client ID, not "YOUR_GMAIL_CLIENT_ID"

2. **Check Google Console**:
   - Verify OAuth consent screen is configured
   - Ensure Gmail API scope is added
   - Check that your email is added as a test user

3. **Check Network Tab**:
   - Look for failed requests to Google APIs
   - Check for CORS errors

### Security Notes:

- Never commit your `.env` file to version control
- The `.env` file is already in `.gitignore` to prevent accidental commits
- For production, use environment variables in your hosting platform

## Additional Resources

- [Google Gmail API Documentation](https://developers.google.com/gmail/api)
- [Google OAuth 2.0 Guide](https://developers.google.com/identity/protocols/oauth2)
- [Vite Environment Variables](https://vitejs.dev/guide/env-and-mode.html)
