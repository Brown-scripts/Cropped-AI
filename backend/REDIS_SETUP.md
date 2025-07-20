# Redis Cloud Setup Guide

## 🔧 Setting Up Redis Cloud for Enhanced Caching

Your Crop Disease Treatment API can use Redis Cloud for improved caching performance. Here's how to set it up:

### 📋 What You Need

You mentioned you have:
- **Password**: `NG7atZj29VPhaNu8PcJfUrOAgk3932ho`
- **Database**: `database-MD6M1ESF`

### 🔍 Getting Your Complete Redis Cloud Connection Details

1. **Log into Redis Cloud Dashboard**:
   - Go to [Redis Cloud Console](https://app.redislabs.com/)
   - Sign in to your account

2. **Find Your Database**:
   - Look for database named `database-MD6M1ESF`
   - Click on it to view details

3. **Get Connection Information**:
   - Go to the **"Configuration"** tab
   - Look for **"Public endpoint"** - this will be in format: `host:port`
   - Example: `redis-12345.c1.us-central1-1.gce.redislabs.com:12345`

### ⚡ Quick Setup Options

#### Option 1: Automatic Setup (Recommended)
```bash
python setup_redis.py
```
This script will guide you through the setup process.

#### Option 2: Manual Setup
Edit your `.env` file with the correct details:

```env
# Replace these with your actual Redis Cloud details
REDIS_HOST=redis-12345.c1.us-central1-1.gce.redislabs.com
REDIS_PORT=12345
REDIS_DB=0
REDIS_PASSWORD=NG7atZj29VPhaNu8PcJfUrOAgk3932ho
REDIS_ENABLED=true
```

### 🧪 Testing Your Connection

After updating the configuration, test it:

```bash
python -c "
import asyncio
from services.cache import cache_manager

async def test():
    await cache_manager.init_redis()
    health = await cache_manager.health_check()
    print('Redis status:', health['redis_cache'])
    await cache_manager.close_redis()

asyncio.run(test())
"
```

### 🎯 Expected Results

**✅ Success**: You should see:
```
Redis status: healthy
```

**❌ Connection Issues**: You might see:
```
Redis status: unhealthy: [error message]
```

### 🔧 Common Issues and Solutions

1. **DNS Resolution Error**:
   - Make sure you have the correct hostname from Redis Cloud dashboard
   - Check your internet connection

2. **Authentication Error**:
   - Verify your password is correct
   - Make sure there are no extra spaces in the password

3. **Connection Timeout**:
   - Check if your firewall allows outbound connections on the Redis port
   - Verify the port number is correct

### 🚀 Benefits of Using Redis Cloud

With Redis Cloud enabled, your API will have:
- **Faster response times** for repeated requests
- **Persistent caching** across API restarts
- **Better performance** under high load
- **Shared cache** if you scale to multiple instances

### 🔄 Fallback Behavior

**Don't worry if Redis Cloud doesn't work immediately!** 

The API is designed to automatically fall back to in-memory caching if Redis is unavailable. Your API will work perfectly fine either way.

### 📊 Monitoring Cache Performance

Once Redis is working, you can check cache statistics:

```bash
# Start the API
python start_api.py

# In another terminal, check cache stats
curl http://localhost:8000/health
```

### 🆘 Need Help?

If you're having trouble:

1. **Check the Redis Cloud dashboard** for the exact connection details
2. **Run the setup script**: `python setup_redis.py`
3. **Test without Redis**: The API works fine with memory cache
4. **Check logs**: Look for Redis connection messages when starting the API

### 📝 Current Configuration

Your current `.env` file is set up with placeholder values. Update it with your actual Redis Cloud endpoint details to enable Redis caching.

---

**Remember**: The API works great with or without Redis Cloud. Redis just makes it faster! 🚀
