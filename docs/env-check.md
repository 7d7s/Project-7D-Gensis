# Environment Check Guide for Production

This guide helps you verify that your environment is production-ready before deployment.

## 1. Prerequisites
- Docker & Kubernetes (with correct versions)
- .NET Core 8.0 SDK installed
- Access to production databases (PostgreSQL, MongoDB, Redis)
- SSL certificates for secure endpoints

## 2. Environment Variables
- Ensure all required environment variables are set (see `configuration.md`).
- Use secret managers (Vault, AWS Secrets Manager, Azure Key Vault) for sensitive values.

## 3. Health Checks
- All microservices should expose `/health` endpoints.
- Use Kubernetes readiness and liveness probes.

## 4. CI/CD Pipeline
- Run all tests (unit, integration, security) before deployment.
- Validate image signatures and scan for vulnerabilities.

## 5. Final Checklist
- Logging, monitoring, and alerting enabled
- Backups configured
- Rollback procedures documented

---

Update this guide as your environment or stack evolves.