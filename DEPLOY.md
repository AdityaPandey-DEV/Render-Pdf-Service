# Deployment Guide

## Quick Start

### 1. Create GitHub Repository

1. Go to [GitHub](https://github.com/new)
2. Create a new repository named `render-pdf-service`
3. **DO NOT** initialize with README, .gitignore, or license (we already have these)
4. Copy the repository URL

### 2. Add Remote and Push

```bash
# Add remote (replace YOUR_USERNAME with your GitHub username)
git remote add origin https://github.com/YOUR_USERNAME/render-pdf-service.git

# Push to GitHub
git push -u origin main
```

### 3. Deploy on Render

1. Go to [Render Dashboard](https://dashboard.render.com)
2. Click "New +" → "Web Service"
3. Connect your GitHub repository `render-pdf-service`
4. Render will auto-detect the Dockerfile
5. Configure:
   - **Name**: `render-pdf-service` (or any name you prefer)
   - **Environment**: `Docker`
   - **Region**: Choose closest to your Vercel deployment
   - **Branch**: `main`
   - **Root Directory**: (leave empty)
6. Add Environment Variables:
   - `RENDER_WEBHOOK_SECRET` - Generate a random secret string
   - `PORT` - `10000` (or leave default)
7. Click "Create Web Service"
8. Wait for deployment (first build takes 5-10 minutes)

### 4. Configure Vercel

After Render service is deployed, add these environment variables to your Vercel project:

- `RENDER_SERVICE_URL` - Your Render service URL (e.g., `https://render-pdf-service.onrender.com`)
- `RENDER_API_KEY` - (Optional) If you add API key authentication
- `RENDER_WEBHOOK_SECRET` - Same secret you set in Render

### 5. Test the Service

```bash
# Test health endpoint
curl https://your-service.onrender.com/health

# Test conversion (replace URLs)
curl -X POST https://your-service.onrender.com/api/convert \
  -H "Content-Type: application/json" \
  -d '{
    "docxUrl": "https://example.com/test.docx",
    "orderId": "test123",
    "callbackUrl": "https://your-vercel-app.vercel.app/api/webhooks/render"
  }'
```

## Alternative: Using GitHub CLI

If you have GitHub CLI installed:

```bash
# Create repo and push in one command
gh repo create render-pdf-service --public --source=. --remote=origin --push
```

## Troubleshooting

### Build fails on Render
- Check Render logs for errors
- Ensure Dockerfile is correct
- Verify LibreOffice installation in logs

### Service not responding
- Check Render service logs
- Verify PORT environment variable
- Test health endpoint

### Webhook not working
- Verify `RENDER_WEBHOOK_SECRET` matches in both services
- Check Vercel logs for webhook requests
- Ensure callback URL is accessible

