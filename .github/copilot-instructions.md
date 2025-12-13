# Getting Started with AI Agents - Copilot Instructions

## Project Context

**Project Name**: Getting Started with Agents Using Azure AI Foundry  
**Purpose**: Production-ready Azure AI agent solution with file search, RAG, monitoring, and evaluation  
**Primary Technologies**: Python, Azure AI Foundry, Azure Container Apps, Azure AI Search, Application Insights  
**Repository**: https://github.com/Azure-Samples/get-started-with-ai-agents

### Solution Overview

Web-based chat application with an AI agent running in Azure Container App, leveraging:
- **Azure AI Agent Service** for agent orchestration
- **Azure AI Search** for knowledge retrieval from uploaded files
- **RAG (Retrieval-Augmented Generation)** with citations
- **Built-in monitoring** with Azure Monitor and Application Insights
- **Agent evaluation** for quality assessment
- **AI Red Teaming** for security scanning

### Repository Structure

```
get-started-with-ai-agents/
├── src/                # Python application code
├── infra/              # Bicep/Terraform infrastructure definitions
├── tests/              # Unit and integration tests
├── evals/              # Agent evaluation scripts
├── airedteaming/       # AI Red Teaming security scans
├── docs/               # Documentation and architecture diagrams
├── scripts/            # Deployment and automation scripts
├── pyproject.toml      # Python project configuration
├── requirements.txt    # Python dependencies
└── azure.yaml          # Azure Developer CLI configuration
```

**Key Directories**:
- `src/` - Azure Container App code (agent logic, web UI, API endpoints)
- `infra/` - Infrastructure as Code (Bicep templates for Azure resources)
- `evals/` - Agent evaluation framework for quality assessment
- `airedteaming/` - Security and safety scanning automation
- `docs/` - Architecture diagrams, troubleshooting, deployment guides

## Coding Agent Orchestration: Environment-Based Multi-Agent Strategy

GitHub Copilot Coding Agent enables environment-specific orchestration. Use this matrix to route tasks appropriately:

### 4-Tier Environment Strategy

**Local Development (1-2 agents)**:
- `@code-reviewer` - Code quality, Python style enforcement
- Task context: Feature development, bug fixes, local testing
- Blocking agents: None (fast iteration encouraged)
- Example: "Add new citation formatting to agent responses / @code-reviewer"

**Dev Environment (4-5 agents)**:
- `@code-reviewer` + `@test-specialist` + `@documentation-expert`
- Additional: `@azure-specialist` (for infrastructure changes)
- Task context: Azure resource configuration, CI/CD pipeline updates
- Blocking agents: None (dev feedback loop)
- Example: "Configure Azure AI Search index for product documentation / @azure-specialist @code-reviewer"

**Staging Environment (7-8 agents)**:
- Core agents + `@security-specialist` + `@performance-optimizer` + `@refactoring-expert`
- Additional: `@cicd-workflow-reviewer` (for deployment pipeline changes)
- Task context: Pre-production validation, performance gates, security audit
- Blocking agents: `@security-specialist` (mandatory for sensitive changes)
- Example: "Implement managed identity authentication for Container Apps / @security-specialist @azure-specialist"

**Production (9-10 BLOCKING agents)**:
- All agents required: `@code-reviewer`, `@security-specialist`, `@test-specialist`, `@documentation-expert`, `@refactoring-expert`, `@security-auditor`, `@performance-optimizer`, `@cicd-workflow-reviewer`, `@release-manager`, `@incident-responder`
- Task context: Production deployments, major releases, incident response
- Blocking agents: ALL (zero-tolerance for production issues)
- Example: "Deploy agent updates to production Container App / @release-manager @security-auditor"

---

## Agent Definitions (10 Core Agents)

### 1. **@code-reviewer**
- **Expertise**: Python code quality, best practices, PEP 8 compliance, architecture alignment
- **When to use**: All pull requests, code refactoring, design pattern questions
- **Outputs**: Code review feedback, refactoring suggestions, Python style corrections

### 2. **@security-specialist**
- **Expertise**: Azure security, managed identities, API security, compliance (GDPR, SOC2)
- **When to use**: Authentication/authorization changes, managed identity configuration, sensitive data handling
- **Outputs**: Security review, threat model updates, compliance checklist, secure patterns
- **CRITICAL**: Must approve any change touching authentication, secrets, or managed identities

### 3. **@test-specialist**
- **Expertise**: Python testing (pytest), agent evaluation, integration testing, CI/CD testing
- **When to use**: Feature implementation, test coverage gaps, evaluation framework updates
- **Outputs**: Test strategy, test code, coverage reports, evaluation metrics

### 4. **@documentation-expert**
- **Expertise**: Technical writing, API documentation, README maintenance, architecture diagrams
- **When to use**: Feature documentation, API changes, architectural decisions, README updates
- **Outputs**: Documentation, diagrams, deployment guides, troubleshooting docs

### 5. **@refactoring-expert**
- **Expertise**: Code refactoring, design patterns, Python optimization, architecture improvement
- **When to use**: Technical debt, large feature implementation, architecture review
- **Outputs**: Refactoring plan, improved architecture, performance metrics

### 6. **@security-auditor**
- **Expertise**: Security auditing, penetration testing, vulnerability scanning, Red Teaming
- **When to use**: Before production releases, security incident response, compliance audits
- **Outputs**: Audit report, vulnerability findings, remediation plan, Red Teaming results

### 7. **@performance-optimizer**
- **Expertise**: Python profiling, Azure optimization, RAG query optimization, caching strategies
- **When to use**: Performance regression, latency issues, optimization opportunities
- **Outputs**: Performance analysis, optimization recommendations, metrics improvement

### 8. **@cicd-workflow-reviewer**
- **Expertise**: GitHub Actions, Azure DevOps, azd (Azure Developer CLI), deployment automation
- **When to use**: Workflow file changes, deployment pipeline issues, azd configuration
- **Outputs**: Workflow improvements, pipeline optimization, deployment strategy

### 9. **@release-manager**
- **Expertise**: Version management, Azure deployment coordination, rollback procedures
- **When to use**: Production releases, version bumping, Container App deployments
- **Outputs**: Release plan, version updates, deployment checklist, rollback plan

### 10. **@incident-responder**
- **Expertise**: Incident response, Azure troubleshooting, emergency procedures, disaster recovery
- **When to use**: Production incidents, Container App failures, agent performance issues
- **Outputs**: Incident response plan, root cause analysis, remediation steps

---

## Cloud Infrastructure Specialists

### **@azure-specialist** - Azure AI Foundry & Infrastructure Expert
- **Use for**: Azure AI Foundry projects, Container Apps, Azure AI Search, Bicep/Terraform IaC, managed identities, observability
- **Solution Patterns**: Azure Container Apps, Azure AI Services, AI Foundry, Azure AI Search, Application Insights, Log Analytics, Key Vault
- **Example**: "Design multi-region agent deployment with Container Apps and Azure Front Door / @azure-specialist"
- **Output Includes**: Architecture diagram, Bicep modules, security checklist, observability plan, cost estimation

### **@gcp-specialist** - Google Cloud Platform Infrastructure Expert
- **Use for**: Alternative cloud deployments (not primary for this solution)
- **Solution Patterns**: GKE, Cloud Run, Vertex AI
- **Note**: This solution is Azure-native; GCP integration is not a primary use case

### **@aws-specialist** - Amazon Web Services Infrastructure Expert
- **Use for**: Alternative cloud deployments (not primary for this solution)
- **Solution Patterns**: Lambda, ECS, SageMaker
- **Note**: This solution is Azure-native; AWS integration is not a primary use case

---

## Infrastructure PR Checklist (Required for All Infrastructure Changes)

When submitting a PR involving infrastructure, security, or deployment configuration, ensure:

### Networking & Security
- [ ] Container Apps network topology documented (VNets, private endpoints)
- [ ] Private endpoints configured for AI Search and Key Vault
- [ ] All public endpoints have appropriate authentication and rate limiting
- [ ] Managed identity configured for all Azure resource access
- [ ] Encryption in transit (TLS 1.3+) enforced for all communication

### Secrets & Configuration Management
- [ ] No hardcoded secrets, API keys, or credentials in code or Bicep files
- [ ] Secrets stored in Azure Key Vault with managed identity access
- [ ] Environment variables use managed identity, not API keys
- [ ] Secret rotation policy defined and documented
- [ ] Audit logging enabled for Key Vault access

### Infrastructure as Code (IaC) & Deployment
- [ ] Bicep templates follow naming conventions and are version-controlled
- [ ] Environment separation enforced (dev, staging, prod have isolated resources)
- [ ] Deployment tested in dev/staging before production
- [ ] Rollback procedure documented and tested (azd down, redeploy previous version)
- [ ] CI/CD pipeline includes pre-deployment validation (azd provision --preview)

### Data & Compliance
- [ ] Data classification completed (PII, business data identified)
- [ ] Data retention policies implemented for AI Search indexes
- [ ] Encryption at rest enabled for all storage (Azure Storage, AI Search)
- [ ] Backup and disaster recovery procedures tested and documented
- [ ] GDPR/SOC2/compliance requirements validated for data handling

### Monitoring & Observability
- [ ] Application Insights configured for Container Apps
- [ ] Key metrics defined (agent latency, token usage, search performance, error rates)
- [ ] Alerting rules configured for critical thresholds
- [ ] Log Analytics workspace retention policies set
- [ ] Performance baselines established for agent responses

### Agent Routing & Review
- [ ] **@azure-specialist** assigned for Azure-specific infrastructure
- [ ] **@security-specialist** assigned for managed identity or security changes
- [ ] **@cicd-workflow-reviewer** assigned for workflow/deployment pipeline changes
- [ ] **@performance-optimizer** assigned for performance-critical changes

---

## Development Workflow

### Environment Setup

**Prerequisites**:
- Python 3.9+
- Azure CLI
- Azure Developer CLI (azd)
- Docker (for local Container Apps testing)
- VS Code with Python extension

**Setup Steps**:
```bash
# Clone repository
git clone https://github.com/Azure-Samples/get-started-with-ai-agents.git
cd get-started-with-ai-agents

# Install Python dependencies
pip install -r requirements-dev.txt

# Login to Azure
az login
azd auth login

# Provision Azure resources
azd provision

# Deploy application
azd deploy

# Verify deployment
azd monitor --overview
```

### Local Development

**Run locally with Azure resources**:
```bash
# Start local development server
python -m src.main

# Run with hot reload
uvicorn src.main:app --reload

# Test agent locally
python -m tests.test_agent
```

**Docker local testing**:
```bash
# Build container
docker build -t agent-app .

# Run container locally
docker run -p 8000:8000 agent-app
```

### Running Tests

**Unit Tests**:
```bash
# Run all tests
pytest

# Run with coverage
pytest --cov=src --cov-report=html

# Run specific test module
pytest tests/test_agent.py
```

**Agent Evaluation**:
```bash
# Run agent evaluation suite
python -m evals.run_evaluation

# Generate evaluation report
python -m evals.generate_report
```

**AI Red Teaming Security Scans**:
```bash
# Run security scans
python -m airedteaming.run_scan

# Review Red Team findings
python -m airedteaming.review_findings
```

### Deployment

**Deploy to Azure**:
```bash
# Deploy all changes
azd deploy

# Deploy specific service
azd deploy <service-name>

# Monitor deployment
azd monitor --live
```

**Environment-specific deployment**:
```bash
# Deploy to dev
azd deploy --environment dev

# Deploy to staging
azd deploy --environment staging

# Deploy to production (requires approval)
azd deploy --environment prod
```

## Architecture Principles

This solution follows Azure Well-Architected Framework:
- **Reliability**: Multi-region deployment, health checks, graceful degradation
- **Security**: Managed identities, private endpoints, Key Vault secrets, Red Teaming
- **Cost Optimization**: Right-sized Container Apps, auto-scaling, Consumption pricing
- **Operational Excellence**: Monitoring, logging, evaluation, automated deployment
- **Performance Efficiency**: Caching, RAG optimization, async operations

## Python Code Conventions

- **PEP 8 Compliance**: Follow Python style guidelines
- **Type Hints**: Use type hints for function signatures
- **Async/Await**: Use async for I/O-bound operations (Azure API calls)
- **Error Handling**: Graceful error handling with structured logging
- **Logging**: Use Python logging module with Azure Application Insights integration
- **Testing**: Minimum 80% code coverage for new features

## Azure-Specific Patterns

### Managed Identity Authentication

```python
from azure.identity import DefaultAzureCredential

# Use managed identity (works in Container Apps)
credential = DefaultAzureCredential()
client = SearchClient(endpoint, index_name, credential)
```

### Application Insights Integration

```python
from opencensus.ext.azure.log_exporter import AzureLogHandler

# Configure Application Insights
logger.addHandler(AzureLogHandler(
    connection_string=os.environ["APPLICATIONINSIGHTS_CONNECTION_STRING"]
))
```

### Azure AI Search RAG Pattern

```python
from azure.search.documents import SearchClient

# Perform RAG query with citations
results = search_client.search(query, top=5)
context = "\n".join([doc["content"] for doc in results])
citations = [{"title": doc["title"], "url": doc["url"]} for doc in results]
```

## Security Guidelines

- **Managed Identities Only**: Never use API keys for Azure resource access
- **Key Vault Integration**: Store all secrets in Azure Key Vault
- **Private Endpoints**: Use private endpoints for AI Search, Key Vault, Storage
- **Network Isolation**: Container Apps in VNet with NSGs
- **Red Teaming**: Run AI Red Teaming scans before production deployment
- **Compliance**: Follow GDPR, SOC2, and industry-specific compliance requirements

## Common Issues

### Issue 1: Managed Identity Authentication Failures
**Solution**: Verify managed identity is enabled on Container App, check RBAC roles on target resources

### Issue 2: AI Search Index Not Found
**Solution**: Verify index name matches environment variable, confirm indexing completed, check AI Search service status

### Issue 3: Agent Responses Lack Citations
**Solution**: Verify RAG configuration, check AI Search query results, validate citation extraction logic

### Issue 4: Container App Startup Failures
**Solution**: Check Application Insights logs, verify environment variables, confirm managed identity configuration

## Resources

- [Azure AI Foundry Documentation](https://learn.microsoft.com/azure/ai-services/agents/)
- [Azure Container Apps Documentation](https://learn.microsoft.com/azure/container-apps/)
- [Azure Developer CLI (azd) Reference](https://learn.microsoft.com/azure/developer/azure-developer-cli/)
- [Troubleshooting Guide](./docs/troubleshooting.md)
- [Architecture Diagrams](./docs/images/architecture.png)

---

**Maintained By**: Azure AI Samples Team  
**Last Updated**: December 12, 2025  
**Template Version**: 1.0 (AICraftWorks Org Standard)
