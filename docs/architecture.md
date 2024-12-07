# IE Bank Architecture Design

## Infrastructure Architecture

### 1. Azure Services Description

#### GitHub
- Used for: Source code management and CI/CD
- Features:
  - Branch protection rules for main branch
  - Environments for Dev/UAT configurations
  - Actions for automated workflows
  - Secrets management

#### App Service for Containers
- Configuration:
  - Name: jwendt-be-{env}
  - SKU: B1 (1 Core, 1.75GB RAM)
  - Linux container hosting
  - Python 3.11 base image
  - Environment variables for configuration
  - Integrated with PostgreSQL

#### App Service Plan
- Configuration:
  - Linux based
  - B1 SKU
  - Auto-scale rules
  - Production workload capable

#### PostgreSQL Database
- Configuration:
  - Flexible Server deployment
  - Standard_B1ms SKU
  - 32GB storage
  - Version 15
  - 7-day backup retention
  - Geo-redundancy: Disabled

#### Static Website (Frontend)
- Configuration:
  - Vue.js application hosting
  - Built-in CI/CD
  - Global content delivery
  - SSL/TLS enabled

#### Azure Container Registry
- Configuration:
  - Basic SKU
  - Admin access enabled
  - Integrated with App Service
  - Private container storage

#### Key Vault
- Configuration:
  - Standard SKU
  - RBAC enabled
  - Soft delete enabled
  - Stores:
    - Database credentials
    - Container registry credentials

#### Log Analytics Workspace
- Configuration:
  - 30-day retention
  - Custom queries enabled
  - Container logs
  - Performance metrics

#### Application Insights
- Configuration:
  - Workspace-based
  - Live metrics
  - Request tracking
  - Performance monitoring

### 2. Environment Design

#### Development Environment
```yaml
Resource Group: BCSAI2024-DEVOPS-STUDENTS-A-DEV
Access: Contributor
Services:
  AppService:
    name: jwendt-be-dev
    sku: B1
    debugEnabled: true
  Database:
    name: jwendt-dbsrv-dev
    sku: Standard_B1ms
  StaticWebApp:
    name: jwendt-fe-dev
    sku: Free
  KeyVault:
    name: jwendt-kv-dev
    sku: standard
```

#### UAT Environment
```yaml
Resource Group: BCSAI2024-DEVOPS-STUDENTS-A-UAT
Access: Reader
Services:
  AppService:
    name: jwendt-be-uat
    sku: B1
    debugEnabled: false
  Database:
    name: jwendt-dbsrv-uat
    sku: Standard_B1ms
  StaticWebApp:
    name: jwendt-fe-uat
    sku: Free
  KeyVault:
    name: jwendt-kv-uat
    sku: standard
```

### 3. Well-Architected Framework Design

#### Reliability Pillar (with SRE)
- Multi-zone deployment
- Automatic failover
- Health monitoring
- Backup strategy:
  - Database: 7-day retention
  - Configuration backups
- Disaster recovery plan

#### Security Pillar (with Security Engineer)
- Network isolation
- Key Vault integration
- Managed identities
- SSL/TLS enforcement
- Access control:
  - RBAC
  - Least privilege
- Regular security scanning

#### Cost Optimization (with Infrastructure Developer)
- Right-sized resources
- Autoscaling rules
- Dev/Prod differentiation
- Resource tags for billing
- Budget alerts
- Cost monitoring

#### Operational Excellence (with Full Stack Developer)
- Infrastructure as Code (Bicep)
- CI/CD pipelines
- Monitoring strategy
- Documentation
- Alerting
- Automated testing

#### Performance Efficiency (with Infrastructure & SRE)
- Performance testing
- Scaling rules
- Resource optimization
- Monitoring metrics
- Performance baselines
- Caching strategy

### 4. User Stories

<antArtifact identifier="user-stories" type="text/markdown" title="Implementation User Stories">
1. **Infrastructure Base Setup**
   ```yaml
   ID: IEBANK-1
   Title: Base Infrastructure Setup
   Description: Setup core Azure infrastructure
   Acceptance Criteria:
     - Resource groups created
     - Network configured
     - Base services deployed
   ```

2. **Container Configuration**
   ```yaml
   ID: IEBANK-2
   Title: Container Infrastructure
   Description: Setup container hosting environment
   Acceptance Criteria:
     - Container registry deployed
     - App Service configured
     - Container deployment working
   ```

3. **Database Setup**
   ```yaml
   ID: IEBANK-3
   Title: Database Infrastructure
   Description: Setup PostgreSQL database
   Acceptance Criteria:
     - Database server deployed
     - Connections working
     - Backups configured
   ```

4. **Security Implementation**
   ```yaml
   ID: IEBANK-4
   Title: Security Setup
   Description: Implement security measures
   Acceptance Criteria:
     - Key Vault deployed
     - Secrets stored
     - Access controls configured
   ```

5. **Monitoring Setup**
   ```yaml
   ID: IEBANK-5
   Title: Monitoring Infrastructure
   Description: Setup monitoring services
   Acceptance Criteria:
     - App Insights configured
     - Log Analytics working
     - Alerts setup
   ```
