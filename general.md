# GMD.DMS System Documentation

## Table of Contents
1. [System Philosophy & Design Choices](#system-philosophy--design-choices)
2. [System Overview](#system-overview)
3. [Architecture & Implementation](#architecture--implementation)
4. [User Management & Security](#user-management--security)
5. [Document Management Philosophy](#document-management-philosophy)
6. [Features & Capabilities](#features--capabilities)
7. [Deployment Timeline](#deployment-timeline)

---

## System Philosophy & Design Choices

### Core Philosophy: Metadata-Driven Document Management

GMD.DMS is built on the fundamental principle that **metadata should drive document organization**, not traditional folder structures. This represents a paradigm shift from static, hierarchical file systems to dynamic, attribute-based document management.

#### Why Metadata Over Folders?

| Aspect | Metadata Approach | Traditional Folders |
|--------|------------------|-------------------|
| **Organization** | Dynamic, multi-dimensional classification | Static, single-path hierarchy |
| **Search** | Rich filtering by attributes (date, type, owner, status) | Limited to filename and path |
| **Flexibility** | Documents can belong to multiple logical categories | Single location constraint |
| **Automation** | Enables workflow automation, lifecycle management | Manual processes only |
| **Scalability** | Handles enterprise-scale document volumes | Performance degrades with size |
| **User Experience** | Intuitive filtering and grouping | Navigation-heavy, time-consuming |

### Design Principles

#### 1. **Centralized Yet Distributed Control**
- **Central System**: Single source of truth for all corporate documents
- **Distributed Ownership**: Each department (PBDV) maintains control over their documents
- **Shared Responsibility Model**: System administration handles infrastructure, departments manage their content

#### 2. **Progressive Enhancement Architecture**
The system is designed with layers of functionality:
- **Core Layer**: Basic document storage and metadata
- **Management Layer**: Advanced filtering, search, and organization
- **Intelligence Layer**: AI-powered insights and automation
- **Integration Layer**: Connects with existing Microsoft 365 ecosystem

#### 3. **Security by Design**
- **Role-Based Access Control (RBAC)**: Granular permissions based on organizational hierarchy
- **Data Classification**: Sensitivity labels for different document types
- **Audit Trail**: Complete tracking of document access and modifications

#### 4. **User-Centric Design**
- **Multiple Interfaces**: Different views for different user needs
- **Contextual Access**: Users see only what's relevant to their role
- **Familiar Technology**: Built on Microsoft 365 to leverage existing user knowledge

---

## System Overview

### Mission Statement
"To create a comprehensive, intelligent document management system that transforms how Gemadept Corporation stores, finds, and manages its critical business documents while maintaining security and compliance."

### Key Objectives
1. **Centralization**: Eliminate document silos across the organization
2. **Discoverability**: Make any document findable within seconds
3. **Compliance**: Ensure regulatory document requirements are met
4. **Lifecycle Management**: Automate document expiration and renewal processes
5. **Knowledge Preservation**: Capture and maintain institutional knowledge

### Technology Stack
- **Platform**: Microsoft 365 & SharePoint
- **Intelligence**: Microsoft Copilot integration
- **Automation**: Power Automate workflows
- **Analytics**: Power BI reporting
- **Security**: Microsoft Information Protection

---

## Architecture & Implementation

### Document Categories (12 Types)
The system manages twelve distinct document categories, each with specific governance requirements:

1. **Internal Processes** (Quy trình nội bộ)
2. **Legal Documents** (Tài liệu pháp lý):
   - Business licenses and government approvals
   - Meeting minutes and resolutions
   - Corporate governance documents
   - Financial, accounting, and tax documents
   - Real estate and tangible assets
   - Intellectual property
   - Contracts, transactions, and agreements
   - Labor and human resources
   - Disputes, complaints, and litigation
   - Insurance
   - Safety, environment, fire prevention, and compliance

### Metadata Schema
Each document is enriched with structured metadata:

#### Required Fields
- **Document Type** (Thuộc loại tài liệu)
- **Issue Date** (Ngày ban hành)
- **Effective Date** (Ngày hiệu lực)

#### Optional Fields
- **Expiration Date** (Ngày hết hạn)
- **Signatory** (Người ký ban hành)
- **Issuing Authority** (Cơ quan ban hành)
- **Document Number** (Số hiệu văn bản)
- **Revision Count** (Số lần điều chỉnh)
- **Original Document Holder** (PBDV giữ bản gốc)

### Folder Structure Design
The system uses a hybrid approach combining automatic folder generation with metadata management:

```
├── Document Libraries
│   ├── GMD Documents [Main Library]
│   │   ├── [Unit Code] (Auto-generated)
│   │   │   ├── [Document Type] (Auto-generated)
│   │   │   └── [Custom Subfolders] (User-managed)
│   │   └── HO [Head Office]
│   │       ├── [Department Code] (Auto-generated)
│   │       └── [Document Type] (Auto-generated)
│   └── Public Documents [Published Library]
│       └── ALL (Public access)
```

---

## User Management & Security

### Access Control Matrix

| User Role | Internal Processes | Other Document Types | Public Documents |
|-----------|-------------------|---------------------|------------------|
| **Group Leadership** | View-All | View-All | View-All |
| **Department Head** | Edit (own dept) | Edit (own dept) | View-All |
| **Department PIC** | Edit (own dept) | Edit (own dept) | View-All |
| **Department Staff** | View (own dept) | Dept-managed | View-All |
| **Legal-HO** | View-All | Edit-All | View-All |
| **External Users** | No Access | No Access | View (via links) |

### Security Features

#### 1. **Sensitivity Labels**
- **Public**: Accessible to anyone
- **Internal**: Company employees only
- **Department**: Department members only
- **Personal**: Individual access only

#### 2. **Sharing Controls**
- **Named Sharing**: Specific user/group permissions
- **Link Sharing**: Password-protected, expiring links
- **QR Codes**: For external document distribution

#### 3. **Audit Capabilities**
- Complete access logging
- Change tracking
- Security incident monitoring

---

## Document Management Philosophy

### Naming Conventions
The system enforces intelligent naming conventions:

**Format**: `[Unit Code]_[Physical Name]_[Issue Date: dd.MM.yyyy]`

**Examples**:
- `GMD_BB HĐQT_Bầu CT HĐQT_12.06.2023`
- `GMD_CV_Tỷ lệ sở hữu nước ngoài_01.06.2022`

### Lifecycle Management

#### Automated Processes
1. **Document Ingestion**: Automatic metadata population
2. **Folder Organization**: Dynamic folder creation based on metadata
3. **Expiration Tracking**: Automated reminders and notifications
4. **Publication**: Seamless public document sharing

#### Notification Schedule
- **Monthly**: Documents expiring in 3 months
- **Bi-annually**: Documents expiring in 6 months (June & December)
- **Annually**: Documents expiring in 1 year (January)

### Document States
- **Draft** (Dự thảo): Work in progress
- **Issued** (Đã ban hành): Officially published
- **Revised** (Đã chỉnh sửa): Updated version
- **Archived** (Lưu trữ): Historical preservation

---

## Features & Capabilities

### Core Features

#### 1. **Advanced Search & Filtering**
- **Metadata-based search**: Filter by any document attribute
- **Full-text search**: Content-based discovery
- **Faceted navigation**: Progressive filtering refinement
- **Saved searches**: Reusable query patterns

#### 2. **AI-Powered Intelligence**
- **Copilot Integration**: Natural language document queries
- **Content Summarization**: Automatic document insights
- **Knowledge Extraction**: Key information identification

#### 3. **Automated Workflows**
- **Document Publication**: Automatic sharing to public libraries
- **Expiration Management**: Proactive renewal reminders
- **Permission Management**: Role-based access automation
- **Compliance Reporting**: Automated audit reports

#### 4. **Multiple User Interfaces**

##### a. **All Documents View**
- Traditional folder-based navigation
- Suitable for occasional users
- Familiar SharePoint experience

##### b. **GMD.DMS Interface**
- Metadata-driven document listing
- Advanced filtering capabilities
- Power user focused

##### c. **Search Interface**
- Dedicated search experience
- Rich filtering options
- Discovery-oriented design

##### d. **Management Views**
- Expiration tracking dashboard
- Original document tracking
- Administrative oversight

### Integration Capabilities

#### Microsoft 365 Ecosystem
- **OneDrive**: Personal document synchronization
- **Teams**: Collaborative document access
- **Outlook**: Email-based document sharing
- **Power Platform**: Custom automation and reporting

#### External Systems
- **Email Notifications**: Automated alerts and reminders
- **QR Code Generation**: Mobile-friendly document access
- **API Access**: Third-party system integration

---

## Deployment Timeline

### Phase 1: Foundation (November 2024 - February 2025)
- **November-December 2024**: Requirements gathering and team formation
- **January-February 2025**: Core system development and initial data migration

### Phase 2: Pilot Implementation (March 2025)
- **Early March**: Head Office, GML, and NDV pilot deployment
- **Mid-March**: User training and feedback collection
- **Late March**: System refinement and expansion planning

### Phase 3: Organization-wide Rollout (April 2025+)
- **April 1, 2025**: Official launch across entire organization
- **Ongoing**: Continuous improvement and feature enhancement

### Success Metrics
- **Document Upload Compliance**: 100% of required documents uploaded by March 23, 2025
- **User Adoption**: All departments actively using the system
- **Search Efficiency**: Average document discovery time under 30 seconds
- **Compliance**: Zero missed document renewal deadlines

---

## Support & Governance

### Support Structure
- **Technical Support**: CDS/Luân, IT/Thịnh
- **Process Support**: RM1/Bảo, RM1/Nguyên
- **Legal Support**: LEGAL/Như, LEGAL/Tuấn
- **User Community**: Microsoft Teams support channel

### Governance Model
- **System Administration**: Central IT team
- **Content Ownership**: Department-level responsibility
- **Policy Management**: Legal and compliance teams
- **User Training**: Ongoing support and documentation

This documentation represents the current state of GMD.DMS as of version 2.3, designed to transform document management practices across Gemadept Corporation through intelligent, metadata-driven approaches.
