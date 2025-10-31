# SSL Certificate Solutions for Self-Signed Certificates

## Problem
You're getting SSL certificate verification errors when connecting to your Paperless instance:
```
SSLError(SSLCertVerificationError(1, '[SSL: CERTIFICATE_VERIFY_FAILED] certificate verify failed: self-signed certificate in certificate chain'))
```

## Solutions

### Solution 1: Auto-Fetch Certificate (Recommended)

The application can automatically fetch and trust the SSL certificate from your Paperless instance. This is more secure than disabling SSL verification completely.

**Add this line to your `.env` file:**
```bash
PAPERLESS_SSL_FETCH_CERT=true
```

### Solution 2: Disable SSL Verification (Less Secure)

If certificate fetching doesn't work, you can disable SSL verification entirely.

**Add this line to your `.env` file:**
```bash
PAPERLESS_SSL_VERIFY=false
```

## Complete `.env` File Examples

### Option 1: Certificate Fetching (Recommended)
```bash
# Paperless-NGX API configuration
PAPERLESS_API_URL=https://paperless.local
PAPERLESS_API_TOKEN=your-api-token-here

# SSL configuration - Fetch and trust the certificate
PAPERLESS_SSL_FETCH_CERT=true
```

### Option 2: Disable SSL Verification
```bash
# Paperless-NGX API configuration
PAPERLESS_API_URL=https://paperless.local
PAPERLESS_API_TOKEN=your-api-token-here

# SSL configuration - Disable verification
PAPERLESS_SSL_VERIFY=false
```

## How Certificate Fetching Works

1. The application connects to your Paperless instance
2. Downloads the SSL certificate
3. Saves it to a temporary file
4. Uses this certificate for all subsequent SSL verification
5. This ensures the connection is encrypted and authenticated

## Log Messages

### Certificate Fetching Enabled
```
SSL certificate fetching is enabled for self-signed certificates.
This provides better security than disabling SSL verification completely.
Fetching SSL certificate from paperless.local:443
SSL certificate saved to /tmp/tmpXXXXXX.pem
Using fetched SSL certificate for verification: /tmp/tmpXXXXXX.pem
```

### SSL Verification Disabled
```
SSL certificate verification is DISABLED. This reduces security.
Only use PAPERLESS_SSL_VERIFY=false for development with self-signed certificates.
```

## Security Comparison

| Method | Security Level | Use Case |
|--------|---------------|----------|
| Certificate Fetching | High | Self-signed certificates in controlled environments |
| SSL Verification Disabled | Low | Development/testing only |
| Standard SSL | Highest | Production with valid certificates |

## Troubleshooting

### Certificate Fetching Fails
If certificate fetching fails, check:
- Network connectivity to your Paperless instance
- Correct hostname in PAPERLESS_API_URL
- Port accessibility (usually 443 for HTTPS)

The application will automatically fall back to standard SSL verification if certificate fetching fails.

### Still Getting SSL Errors
1. Try `PAPERLESS_SSL_FETCH_CERT=true` first
2. If that fails, use `PAPERLESS_SSL_VERIFY=false` as a last resort
3. Check your `.env` file is in the correct location (`data` directory)
4. Restart the application after making changes

## Production Recommendations

For production environments, consider:
- Using properly signed SSL certificates (Let's Encrypt, commercial CA)
- Setting up a certificate authority for internal use
- Using a reverse proxy with proper SSL termination