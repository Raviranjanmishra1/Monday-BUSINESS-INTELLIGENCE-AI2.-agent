# SETUP GUIDE - Monday BI Agent

## Issue Resolved
- ✓ Backend is running on port 7860 and responding correctly
- ✓ Frontend is running on port 5173
- ✓ Communication between frontend and backend is configured

## What's Still Needed
The reason you're seeing error responses is that the **Monday.com API credentials are not configured**.

### Step 1: Get Your Monday.com API Token
1. Go to https://monday.com/developers
2. Log in to your Monday.com account  
3. Navigate to **Settings** → **API & Integrations** → **API tokens**
4. Click **Create new token**
5. Give it a meaningful name (e.g., "BI Agent")
6. Copy the token

### Step 2: Get Your Board IDs  
In Monday.com, visit each board you want to use:
- Go to the Deals board → Look at the URL: `https://monday.com/boards/[BOARD_ID]`
- Copy that numeric ID
- Repeat for Work Orders board
- You now have:
  - `MONDAY_DEALS_BOARD_ID`: [numeric ID]
  - `MONDAY_WORKORDERS_BOARD_ID`: [numeric ID]

### Step 3: Get Your Groq API Key (Free)
1. Go to https://console.groq.com
2. Sign up (free tier available)
3. Navigate to **API Keys**  
4. Create a new API key
5. Copy it

### Step 4: Update the .env File
Edit `backend/.env` and replace:
- `YOUR_MONDAY_API_TOKEN` with your actual Monday.com API token
- `YOUR_DEALS_BOARD_ID` with your Deals board ID
- `YOUR_WORKORDERS_BOARD_ID` with your Work Orders board ID
- `YOUR_GROQ_API_KEY` with your Groq API key

**IMPORTANT:** Do NOT commit this file to git! It contains sensitive credentials.

### Step 5: Restart Everything
1. Kill the backend process (port 7860) if it's still running from before
2. In terminal: `cd backend` → `python main.py`
3. The frontend should auto-refresh and reconnect

## Now It Should Work
Once your credentials are configured, you should be able to:
- Ask business intelligence questions in the chat
- See real-time data from your Monday.com boards
- Get action traces and data quality reports

## Testing
To verify everything is connected:
```
# Test health check
curl http://localhost:7860/health

# Should return:
# {"status":"ok","service":"monday-bi-agent"}
```

## Troubleshooting
- **Still seeing "Network Error"?** Make sure port 7860 is running: `netstat -ano | find "7860"`
- **401 Unauthorized?** Check that your Monday API token is correct in `.env`
- **Frontend can't connect?** Verify `.env.local` has `VITE_API_BASE_URL=http://localhost:7860`
