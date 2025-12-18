# Data Product Catalog Wiki

## Overview
This wiki documents the structure and governance model for our data product catalog, based on proven spreadsheet practices and industry best practices.

---

## What is a Data Product?

A **Data Product** is a well-defined, governed, and discoverable data asset that:
- Serves specific business purposes
- Has clear ownership and accountability  
- Meets defined quality standards
- Provides reliable access methods
- Includes comprehensive documentation

---

## Catalog Structure

### Core Components

Our data catalog is organized around these main concepts:

```
📊 Data Product
├── 🏢 Domain (Business Area)
├── 👥 Ownership & Stewardship
├── 🔧 Technical Implementation
├── 📈 Quality & SLA Metrics
├── 🔒 Security & Compliance
└── 📋 Documentation & Schema
```

---

## Data Product Attributes

### 1. Basic Information
Every data product must have:

| Attribute | Description | Example |
|-----------|-------------|---------|
| **Product Name** | Clear, descriptive name | "Daily Trade Blotter" |
| **Description** | What this product provides | "All executed trades with settlement details" |
| **Domain** | Business area | Trading, Risk, Portfolio, Market Data |
| **Business Purpose** | Why this data exists | Trade reporting, risk management, compliance |

### 2. Ownership & Governance

| Role | Responsibility | Example |
|------|----------------|---------|
| **Data Owner** | Accountable for data quality and business rules | Head of Trading |
| **Business Owner** | Defines requirements and usage policies | Trading Desk Manager |
| **Technical Contact** | Handles implementation and technical issues | trading-tech@fund.com |
| **Data Steward** | Day-to-day quality monitoring and access | Senior Trader |

### 3. Technical Implementation

| Attribute | Description | Example |
|-----------|-------------|---------|
| **Data Source** | Where data originates | Prime Broker Feeds, OMS |
| **Storage Location** | Physical/logical location | s3://fund-data/trading/trades/ |
| **Data Format** | File format or structure | Parquet, JSON, CSV |
| **Update Frequency** | How often data refreshes | Real-time, Daily, Weekly |
| **Access Method** | How to consume data | API, File share, Database |

### 4. Quality & Performance

| Metric | Description | Target |
|--------|-------------|--------|
| **Data Quality Score** | Overall quality rating (0-100) | >95% |
| **Freshness SLA** | How current data must be | T+0 by 6:00 PM |
| **Availability SLA** | Uptime commitment | 99.9% |
| **Completeness** | Percentage of expected records | >99% |

### 5. Security & Compliance

| Attribute | Values | Description |
|-----------|--------|-------------|
| **Sensitivity Level** | Public, Internal, Confidential, Restricted | Data classification |
| **PII Flag** | Yes/No | Contains personal information |
| **Retention Period** | Duration | How long to keep data |
| **Access Controls** | Read/Write permissions | Who can access |

### 6. Lifecycle Management

| Status | Description | Actions Allowed |
|--------|-------------|-----------------|
| **Draft** | Under development | Edit, Test |
| **Active** | Production ready | Consume, Monitor |
| **Deprecated** | Being phased out | Read-only |
| **Retired** | No longer available | Archive only |

---

## Data Domains

### Trading Domain
**Purpose**: Manages all trade execution and settlement activities

**Key Data Products**:
- **Trade Blotter**: Real-time executed trades
- **Order Book**: Active and historical orders  
- **Settlement Instructions**: Trade settlement details
- **Broker Allocations**: Trade allocation across brokers

**Typical Consumers**: Trading desks, Operations, Compliance, Risk

### Risk Domain  
**Purpose**: Risk measurement, monitoring, and reporting

**Key Data Products**:
- **Daily VaR**: Value at Risk calculations
- **Position Risk**: Portfolio risk exposures
- **Stress Test Results**: Scenario analysis outputs
- **Risk Limits**: Current vs. limit monitoring

**Typical Consumers**: Risk managers, Portfolio managers, Senior management

### Portfolio Domain
**Purpose**: Portfolio construction, performance, and attribution

**Key Data Products**:
- **Position Snapshots**: End-of-day holdings
- **Performance Attribution**: Return breakdown analysis  
- **Benchmark Data**: Index and benchmark information
- **Asset Allocation**: Strategic and tactical allocation

**Typical Consumers**: Portfolio managers, Clients, Performance team

### Market Data Domain
**Purpose**: Market information for pricing and analysis

**Key Data Products**:
- **Real-time Prices**: Live market data
- **End-of-Day Prices**: Official closing prices
- **Historical Data**: Time series data for analysis
- **Reference Data**: Instrument master data

**Typical Consumers**: All domains, Trading systems, Analytics

---

## Example Data Products

### 1. Daily Trade Blotter

**Basic Information**:
- **Name**: Daily Trade Blotter
- **Description**: Complete record of all executed trades with counterparty and settlement details
- **Domain**: Trading
- **Purpose**: Trade reporting, P&L calculation, settlement processing

**Ownership**:
- **Data Owner**: Head of Trading
- **Business Owner**: Trading Desk Manager  
- **Technical Contact**: trading-tech@fund.com
- **Data Steward**: Senior Trader

**Technical Details**:
- **Source**: Prime Broker Feeds + Order Management System
- **Location**: s3://fund-data/trading/trades/daily/
- **Format**: Parquet files, partitioned by date
- **Update**: Real-time (5 minute delay)
- **Access**: REST API, S3 direct access

**Quality & SLA**:
- **Quality Score**: 98.5%
- **Freshness**: T+0 by 6:00 PM
- **Availability**: 99.9%
- **Completeness**: >99.8%

**Security**:
- **Sensitivity**: Confidential
- **PII**: No
- **Retention**: 7 years
- **Access**: Trading team (read/write), Others (read-only with approval)

### 2. End of Day Positions

**Basic Information**:
- **Name**: End of Day Positions  
- **Description**: Daily position snapshots by account, strategy, and instrument
- **Domain**: Portfolio
- **Purpose**: Risk management, performance attribution, regulatory reporting

**Ownership**:
- **Data Owner**: Head of Portfolio Management
- **Business Owner**: Portfolio Managers
- **Technical Contact**: portfolio-tech@fund.com
- **Data Steward**: Portfolio Operations

**Technical Details**:
- **Source**: Trade Blotter + Corporate Actions + Cash Management
- **Location**: s3://fund-data/portfolio/positions/eod/
- **Format**: Parquet files with Delta Lake
- **Update**: Daily by 7:00 PM
- **Access**: BI Dashboard, API, Direct query

**Quality & SLA**:
- **Quality Score**: 99.2%
- **Freshness**: T+0 by 7:00 PM
- **Availability**: 99.9%
- **Completeness**: 100%

**Security**:
- **Sensitivity**: Confidential
- **PII**: No
- **Retention**: 10 years (regulatory requirement)
- **Access**: Portfolio team (full), Risk team (read), Compliance (read)

---

## Data Lineage Examples

### Trade Processing Flow
```
Raw Broker Feeds 
    ↓ (ETL: Trade Standardization)
Normalized Trade Blotter
    ↓ (Aggregation: Daily Position Calc)
End of Day Positions
    ↓ (Analysis: Performance Attribution)
Daily P&L Reports
```

### Risk Calculation Flow
```
End of Day Positions + Market Data
    ↓ (Risk Engine: VaR Calculation)
Daily Risk Metrics
    ↓ (Reporting: Risk Dashboard)
Risk Management Reports
```

### NAV Calculation Flow
```
Positions + Market Prices + Cash + Accruals
    ↓ (NAV Engine: Fund Accounting)
Daily NAV
    ↓ (Distribution: Client Reporting)
Investor Statements
```

---

## Quality Framework

### Quality Dimensions

**Completeness**
- All expected records are present
- No missing mandatory fields
- Target: >99%

**Accuracy** 
- Data values are correct
- Cross-system reconciliation passes
- Target: >99.5%

**Timeliness**
- Data arrives within SLA windows
- Processing completes on schedule  
- Target: 100% SLA compliance

**Consistency**
- Data aligns across systems
- No contradictory information
- Target: Zero critical exceptions

### Quality Monitoring

**Daily Checks**:
- Record count validation
- Field completeness verification
- Business rule validation
- Cross-reference verification

**Weekly Reviews**:
- Quality trend analysis
- SLA performance review
- Exception analysis
- Stakeholder feedback

**Monthly Assessments**:
- Overall quality scoring
- Process improvement recommendations
- Consumer satisfaction survey
- Governance review

---

## Access Control Framework

### Sensitivity Levels

**Public**
- No access restrictions
- Can be shared externally
- Examples: Market indices, Public company data

**Internal**  
- Internal use only
- Standard employee access
- Examples: Internal research, General market data

**Confidential**
- Business sensitive information
- Need-to-know basis
- Examples: Positions, Trading data, Performance

**Restricted**
- Highly sensitive
- Executive/specialized access only  
- Examples: Strategic plans, Regulatory investigations

### Access Types

**Read**
- View data
- Export for analysis
- Standard business use

**Write**
- Modify data values
- Update metadata
- Data steward functions

**Admin**
- Manage access controls
- Configure data pipelines
- System administration

### Access Request Process

1. **Request**: Submit access request with business justification
2. **Review**: Data owner reviews and approves/denies
3. **Provision**: Technical team grants access
4. **Monitor**: Regular access reviews and audits

---

## Implementation Guidelines

### Getting Started

1. **Identify Data Products**: Start with critical business data
2. **Assign Ownership**: Designate clear owners and stewards  
3. **Document Metadata**: Capture all required attributes
4. **Establish Quality**: Define and measure quality metrics
5. **Control Access**: Implement appropriate security controls

### Best Practices

**Naming Conventions**:
- Use clear, descriptive names
- Avoid technical jargon
- Be consistent across similar products

**Documentation**:
- Keep descriptions current
- Include business context
- Provide usage examples

**Quality Management**:
- Define measurable quality metrics
- Implement automated monitoring
- Act on quality issues promptly

**Governance**:
- Regular metadata reviews
- Clear escalation procedures
- Stakeholder communication

This model provides a comprehensive framework for managing data products in a scalable, governed manner while maintaining the practical approach of your original spreadsheet.