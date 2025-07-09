<!--
SPDX-FileCopyrightText: 2025 Deutsche Telekom AG

SPDX-License-Identifier: CC0-1.0    
-->

# Open Telekom Integration Platform Documentation

This documentation provides an overview of the Open Telekom Integration Platform, its components, and how to get started
with deploying it.

## Documentation Overview

This documentation provides comprehensive guidance for deploying, configuring, and operating the Open Telekom
Integration Platform.

### Core Documentation

- **[Repository Overview](repository_overview.md)** - Complete listing of all platform components and their repositories
- **[Prerequisites](install/prerequisites.md)** - Required components and configurations for deployment

### Installation Guides

- **[Installation on EKS](install/installation_on_eks.md)** - AWS EKS deployment guide
- **[Installation on Minikube](install/installation_on_minikube.md)** - Local development setup

## Functional Overview

The Open Telekom Integration Platform provides comprehensive capabilities for modern enterprise integration, covering
the full spectrum of API management, event-driven architecture, and secure service communication. It delivers
production-grade functionality with enterprise-level security, scalability, and operational features.

## API Gateway

The platform's **Gateway** component (based on Kong) serves as the central entry point for all API traffic, providing:

**Traffic Management**

- Intelligent request routing and load balancing
- Rate limiting and throttling capabilities
- Gateway Mesh capability to enable location transparency

**Security Features**

- OAuth 2.0 integration
- Access Control Lists and Authorization support (API scopes)

## Event-Driven Integration

The **Horizon** component provides a comprehensive publish-subscribe messaging platform:

**Message Publishing**

- RESTful APIs for event publication
- Event validation and schema enforcement
- Guaranteed at-least-once delivery

**Subscription Management**

- Advanced, content-based filtering with JSON-based rules
- Conditional delivery based on trigger criteria
- Scoped data access for fine-grained control
- Flexible delivery options (callback and Server-Sent Events)

**Event Processing**

- Real-time event multiplexing and demultiplexing
- Circuit breaker patterns for subscriber resilience
- Automatic retry mechanisms with exponential backoff

**Monitoring and Observability**

- Event delivery tracking and status monitoring
- Performance metrics and health indicators
- Event sourcing with complete audit trails
- Circuit breaker status and health checks

## Identity and Access Management

The **Identity Provider** (Iris) component delivers enterprise-grade authentication and authorization:

**Machine-to-Machine (M2M) Authentication**

- OAuth 2.0 Client Credentials flow
- JWT token issuance and validation

## Cross-Environment Connectivity

**Gateway Mesh Architecture**

- Seamless connectivity across clouds and network environments
- Location transparency for service consumers and providers
- Cross-zone communication with consistent security

**Multi-Zone Deployment**

- Flexible zone-based deployment model
- Environment-specific routing and policies
- Zone failover support

## Observability and Monitoring

**Comprehensive Monitoring**

- Real-time metrics collection with Prometheus
- Distributed tracing with Jaeger integration

**Performance Analytics**

- API performance metrics
- Event delivery statistics and latency tracking
- Resource utilization and capacity planning

## Security-by-Design Features

### Transport Security

- Mandatory TLS 1.2+ for all communications
- Configurable cipher suites and security policies
- Man-in-the-middle attack prevention

### Access Control

- Multi-layered authentication and authorization
- Token-based security with JWT
- Access logging for compliance

## Integration Standards and Compliance

### API Standards Support

- OpenAPI 3.0 specification compliance
- TMForum Open API alignment
- RESTful API design principles
- CloudEvents for event-driven interfaces

### Ecosystem Integration

- Kubernetes-native deployment
- CI/CD pipeline integration
- Infrastructure as Code support
- Multi-cloud deployment flexibility

This comprehensive feature set makes the Open Telekom Integration Platform suitable for large-scale enterprise
integration scenarios, supporting everything from simple API exposure to complex event-driven architectures with
stringent security requirements.
