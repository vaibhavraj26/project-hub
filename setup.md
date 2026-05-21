# 📖 ProjectHub - Local Setup Guide

## 🚀 Quick Start

### Prerequisites
- Docker installed
- Git installed
- 5 minutes of your time

### Setup (Run Services Manually)

**1. Clone repository**
```bash
git clone <your-repo-url>
cd project-hub
```

**2. Build & Run Containers**

```bash
# Run MongoDB
docker run -d --name projecthub-mongodb \
  -e MONGO_INITDB_ROOT_USERNAME=admin \
  -e MONGO_INITDB_ROOT_PASSWORD=password \
  -p 27017:27017 \
  mongo:7.0

# Build & run Backend
docker build -t projecthub-backend ./server
docker run -d --name projecthub-backend \
  -e MONGO_URI=mongodb://admin:password@host.docker.internal:27017/projecthub \
  -e JWT_SECRET=your-secret \
  -e NODE_ENV=production \
  -p 5000:5000 \
  projecthub-backend

# Build & run Frontend
docker build -t projecthub-frontend ./client
docker run -d --name projecthub-frontend \
  -e VITE_API_URL=http://localhost:5000/api \
  -p 3000:3000 \
  projecthub-frontend
```

**3. Check Containers**
```bash
docker ps
```

**Visit**: http://localhost:3000

---

## � Project Structure

```
project-hub/
├── client/
│   ├── Dockerfile           # Frontend container
│   ├── package.json
│   ├── src/
│   ├── vite.config.js
│   └── public/
├── server/
│   ├── Dockerfile           # Backend container
│   ├── package.json
│   ├── server.js
│   ├── routes/
│   ├── models/
│   └── controllers/
├── .env                     # Environment configuration
└── README.md               # Project documentation
```

---

## 📝 Environment Configuration

Create `.env` file in root with:

```env
# MongoDB
MONGO_URI=mongodb://admin:password@host.docker.internal:27017/projecthub

# Backend
PORT=5000
JWT_SECRET=pHub_s3cr3t_jwt_k3y_2026!xZ9
JWT_EXPIRE=7d
NODE_ENV=production

# Frontend
VITE_API_URL=http://localhost:5000/api

# CORS
CLIENT_URL=https://project-hub-web.vercel.app
SOCKET_IO_CORS=https://project-hub-web.vercel.app
```

---

## 🐳 Individual Container Commands

### Build Images

```bash
# Build backend image
docker build -t projecthub-backend ./server

# Build frontend image
docker build -t projecthub-frontend ./client
```

### Run MongoDB

```bash
docker run -d --name projecthub-mongodb \
  -e MONGO_INITDB_ROOT_USERNAME=admin \
  -e MONGO_INITDB_ROOT_PASSWORD=password \
  -e MONGO_INITDB_DATABASE=projecthub \
  -p 27017:27017 \
  -v mongodb_data:/data/db \
  mongo:7.0
```

### Run Backend

```bash
docker run -d --name projecthub-backend \
  -e MONGO_URI=mongodb://admin:password@host.docker.internal:27017/projecthub \
  -e JWT_SECRET=pHub_s3cr3t_jwt_k3y_2026!xZ9 \
  -e PORT=5000 \
  -e NODE_ENV=production \
  -p 5000:5000 \
  projecthub-backend
```

### Run Frontend

```bash
docker run -d --name projecthub-frontend \
  -e VITE_API_URL=http://localhost:5000/api \
  -p 3000:3000 \
  projecthub-frontend
```

### View Running Containers

```bash
docker ps
```

### View Logs

```bash
# Backend logs
docker logs -f projecthub-backend

# Frontend logs
docker logs -f projecthub-frontend

# MongoDB logs
docker logs -f projecthub-mongodb
```

### Stop All Containers

```bash
docker stop projecthub-backend projecthub-frontend projecthub-mongodb
```

### Remove All Containers

```bash
docker rm projecthub-backend projecthub-frontend projecthub-mongodb
```

### Remove All Data

```bash
docker volume rm mongodb_data
```

---

## 🧪 Testing

### Frontend Test
```bash
curl http://localhost:3000
```

### Backend Test
```bash
curl http://localhost:5000/api/health
```

### Check Container Status
```bash
docker ps | grep projecthub
```

---

## 🆘 Troubleshooting

### Port Already in Use
```bash
# Find process using port 3000
netstat -ano | findstr :3000
# Kill process
taskkill /PID <PID> /F
```

### Container won't start
```bash
# View error logs
docker logs projecthub-backend

# Rebuild image
docker build --no-cache -t projecthub-backend ./server
```

### MongoDB Connection Error
```bash
# Check MongoDB is running
docker ps | grep mongodb

# Verify MONGO_URI uses host.docker.internal
MONGO_URI=mongodb://admin:password@host.docker.internal:27017/projecthub
```

### Frontend can't reach Backend
```bash
# Test backend from host machine
curl http://localhost:5000/api/health

# Use localhost:5000 in VITE_API_URL (not backend:5000)
VITE_API_URL=http://localhost:5000/api
```

---

## 🔄 Development Workflow

### Make Code Changes
```bash
# Backend changes:
# Rebuild and restart
docker build -t projecthub-backend ./server
docker stop projecthub-backend
docker run -d --name projecthub-backend ... # (see command above)

# Frontend changes:
# Rebuild and restart
docker build -t projecthub-frontend ./client
docker stop projecthub-frontend
docker run -d --name projecthub-frontend ... # (see command above)
```

### View Changes
```bash
# Frontend: http://localhost:3000 (auto-refresh)
# Backend: Check docker logs projecthub-backend
```

### Debug
```bash
# Access container shell
docker exec -it projecthub-backend sh
docker exec -it projecthub-frontend sh

# Run commands in container
docker exec projecthub-backend npm list
```

---

## 📱 Features

✅ **Real-time Updates** - Socket.io integration  
✅ **Authentication** - JWT-based  
✅ **Project Management** - CRUD operations  
✅ **Task Tracking** - Task management system  
✅ **Messaging** - Real-time messaging  
✅ **Team Collaboration** - Team features  
✅ **User Profiles** - Profile management  
✅ **Dashboard** - Activity feeds & stats  

---

## 🚀 Production Deployment

For production deployment:

1. **Backend**: Deploy Docker image to Render
2. **Frontend**: Deploy to Vercel or Netlify
3. **Database**: Use MongoDB Atlas

Environment variables must be set in your hosting platform's settings.

---

## 📚 Project Documentation

- **Server**: See `server/README.md`
- **Client**: See `client/README.md`
- **Main Project**: See `README.md`

---

## 💡 Tips

1. **Use `host.docker.internal`** to access localhost from containers (Windows/Mac)
2. **Data persists** in MongoDB volumes between runs
3. **Use `docker logs`** to debug issues
4. **Environment variables** are passed at runtime with `-e` flag
5. **Rebuild images** when package.json changes

---

## 🎯 Common Workflows

### Fresh Start
```bash
docker stop $(docker ps -q --filter "name=projecthub")
docker rm $(docker ps -aq --filter "name=projecthub")
docker volume rm mongodb_data
docker build -t projecthub-backend ./server
docker build -t projecthub-frontend ./client
# Run containers again (see commands above)
```

### Restart All Services
```bash
docker restart projecthub-backend projecthub-frontend projecthub-mongodb
```

### View All Logs
```bash
docker logs -f --tail=50 projecthub-backend
```

### Debug Backend
```bash
docker exec -it projecthub-backend sh
npm list
npm test
```

---

## ✅ Verification Checklist

- [ ] Docker installed (`docker --version`)
- [ ] Repository cloned
- [ ] `.env` file created with correct MONGO_URI
- [ ] MongoDB running (`docker ps | grep mongo`)
- [ ] Backend running (`docker ps | grep backend`)
- [ ] Frontend running (`docker ps | grep frontend`)
- [ ] Frontend loads: http://localhost:3000
- [ ] Backend responds: http://localhost:5000/api/health

---

## 🆘 Getting Help

1. **Check container status**: `docker ps`
2. **Check logs**: `docker logs projecthub-backend`
3. **Check .env file**: Verify MONGO_URI and API_URL
4. **Verify ports free**: `netstat -ano | findstr :3000`

---

**Setup Time**: 5 minutes  
**Difficulty**: Intermediate  
**Requirements**: Docker only  
**Status**: Ready to use  

🚀 **Start building!**
