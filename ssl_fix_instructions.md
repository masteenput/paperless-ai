# SSL Certificate Fix for Self-Signed Certificates

## Problem
You're getting SSL certificate verification errors when connecting to your Paperless instance:
```
SSLError(SSLCertVerificationError(1, '[SSL: CERTIFICATE_VERIFY_FAILED] certificate verify failed: self-signed certificate in certificate chain'))
```

## Solution
The application now supports disabling SSL certificate verification for self-signed certificates.

### Quick Fix

1. **Add the following line to your `.env` file** (located in the `data` directory):
   ```
   PAPERLESS_SSL_VERIFY=false
   ```

2. **Restart the application** to apply the changes.

### Complete `.env` Example
```bash
# Paperless-NGX API configuration
PAPERLESS_URL=https://paperless.local
PAPERLESS_API_TOKEN=your-api-token-here

# SSL configuration - DISABLE for self-signed certificates
PAPERLESS_SSL_VERIFY=false
```

### Alternative Environment Variable Names
The application also supports these environment variable names for the Paperless URL:
- `PAPERLESS_API_URL`
- `PAPERLESS_URL` 
- `PAPERLESS_NGX_URL`
- `PAPERLESS_HOST`

### Security Warning
⚠️ **IMPORTANT**: Disabling SSL verification reduces security. Only use `PAPERLESS_SSL_VERIFY=false` for:
- Development environments
- Internal networks with self-signed certificates
- Testing purposes

For production environments, consider:
- Using properly signed SSL certificates
- Setting up a certificate authority
- Using Let's Encrypt for free SSL certificates

### Verification
After setting `PAPERLESS_SSL_VERIFY=false`, you should see this log message:
```
SSL certificate verification is DISABLED. This reduces security.
Only use PAPERLESS_SSL_VERIFY=false for development with self-signed certificates.
```

And the connection errors should be resolved.