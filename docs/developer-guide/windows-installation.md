# NextGen MedTech Platform - Windows Installation Guide

## Prerequisites

### Required Software
- Windows 10/11 Professional or Enterprise (64-bit)
- Docker Desktop for Windows (latest version)
- Git for Windows
- Visual Studio 2022 (any edition) or VS Code
- .NET 8 SDK
- Node.js LTS (18.x or later)
- PostgreSQL 15 or later
- MongoDB 6.0 or later

### System Requirements
- 16GB RAM minimum (32GB recommended)
- 4 CPU cores minimum (8 cores recommended)
- 100GB free disk space
- Virtualization enabled in BIOS

## Installation Steps

### 1. Clone Repository
```powershell
git clone https://github.com/your-org/nextgen-medtech.git
cd nextgen-medtech
```

### 2. Environment Setup

#### Configure Docker Desktop
1. Open Docker Desktop
2. Go to Settings > Resources
3. Allocate at least 8GB RAM and 4 CPUs
4. Enable Kubernetes

#### Install Dependencies
```powershell
# Install backend dependencies
cd backend
dotnet restore

# Install frontend dependencies
cd ../frontend
npm install
```

### 3. Database Setup

#### PostgreSQL
1. Create databases:
```sql
CREATE DATABASE nextgen_medtech;
CREATE DATABASE nextgen_medtech_test;
```

2. Run migrations:
```powershell
cd backend/services/database
dotnet ef database update
```

#### MongoDB
1. Create collections and indexes:
```powershell
cd backend/scripts
./setup-mongodb.ps1
```

### 4. Configuration

1. Copy environment templates:
```powershell
cp .env.example .env
cp backend/services/.env.example backend/services/.env
cp frontend/.env.example frontend/.env
```

2. Update configuration files with your local settings:
- `backend/services/appsettings.Development.json`
- `frontend/.env`

### 5. Start Development Environment

#### Option 1: Docker Compose (Recommended)
```powershell
docker-compose up -d
```

#### Option 2: Local Development
1. Start backend services:
```powershell
cd backend/services
dotnet run --project NextGenMedTech.Gateway
```

2. Start frontend:
```powershell
cd frontend
npm run dev
```

### 6. Verify Installation

1. Access services:
- API Gateway: http://localhost:5000
- Swagger UI: http://localhost:5000/swagger
- Frontend: http://localhost:3000

2. Run health checks:
```powershell
cd backend/scripts
./health-check.ps1
```

## Troubleshooting

### Common Issues

1. **Docker fails to start**
   - Ensure Hyper-V is enabled
   - Verify virtualization is enabled in BIOS
   - Check Windows features for containers support

2. **Database connection errors**
   - Verify PostgreSQL service is running
   - Check connection strings in appsettings.json
   - Ensure firewall allows database connections

3. **Port conflicts**
   - Check for services using ports 5000, 3000
   - Modify port settings in docker-compose.yml

### Getting Help

1. Check logs:
```powershell
docker-compose logs -f
```

2. Debug services:
- Use Visual Studio debugger
- Check application logs in `backend/logs`
- Monitor Docker container status

## Security Notes

1. Development certificates:
```powershell
dotnet dev-certs https --trust
```

2. Default credentials are for development only

3. Enable Windows Defender exceptions for development folders

## Next Steps

1. Review [Developer Guide](../developer-guide/README.md)
2. Setup [CI/CD Pipeline](../developer-guide/cicd-setup.md)
3. Configure [Monitoring](../developer-guide/monitoring.md)

## Version Compatibility

| Component | Version |
|-----------|----------|
| Windows   | 10/11    |
| Docker    | 24.x     |
| .NET      | 8.0      |
| Node.js   | 18.x     |
| PostgreSQL| 15.x     |
| MongoDB   | 6.0      |

## Support

For additional support:
- Create GitHub issues
- Contact development team
- Check documentation updates