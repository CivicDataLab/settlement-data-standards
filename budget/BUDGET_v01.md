# Central Tibetan Administration Budget Data Standard
Version:0.1

Last Modified: 15th January 2025

Contributors: Prajna Prayas

## Abstract

This document specifies a data standard for representing budget documents of the Central Tibetan Administration (CTA). The standard provides a structured format for recording both revenue and expenditure, with special consideration for donor-funded activities and welfare spending across Tibetan settlements. The specification follows international government finance standards while accommodating the unique needs of the CTA.

## Status of This Document

This is a draft document and may be updated, replaced, or obsoleted by other documents at any time.

## Table of Contents

1. Introduction
2. Terminology
3. Schema Structure
4. Core Components
5. Data Types and Validation
6. Implementation Guidelines
7. Security Considerations
8. Examples
9. References
10. Acknowledgments

# 1. Introduction

## 1.1. Purpose

This specification defines a standardized format for representing budget data within the Central Tibetan Administration. It aims to facilitate:

- Transparent tracking of donor funds
- Efficient management of welfare spending
- Settlement-specific financial management
- Compliance with international financial standards
- Integration with existing financial systems

## 1.2. Scope

This standard covers:
- Revenue documentation including donor funds
- Expenditure categorization
- Settlement-specific financial tracking
- Audit trail requirements
- Validation rules

## 1.3. Notational Conventions

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in RFC 2119.

# 2. Terminology

- Donor Fund: Financial resources provided by external entities for specific purposes
- Settlement: A Tibetan community establishment in India
- Fiscal Year: The financial year running from April 1 to March 31
- COFOG: Classification of Functions of Government
- Beneficiary: Individual or group receiving welfare benefits

# 3. Schema Structure
```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "CTA Budget Schema",
  "description": "JSON Schema for Central Tibetan Administration Budget Documents",
  "type": "object",
  "required": ["metadata", "fiscal_year", "currency", "revenue_items", "spending_items"],
  "properties": {
    "metadata": {
      "type": "object",
      "required": ["version", "created_at"],
      "properties": {
        "version": {
          "type": "string",
          "pattern": "^\\d+\\.\\d+\\.\\d+$"
        },
        "created_at": {
          "type": "string",
          "format": "date-time"
        },
        "updated_at": {
          "type": "string",
          "format": "date-time"
        }
      }
    },
    "fiscal_year": {
      "type": "string",
      "pattern": "^\\d{4}-\\d{4}$",
      "description": "Fiscal year in format YYYY-YYYY"
    },
    "currency": {
      "type": "string",
      "pattern": "^[A-Z]{3}$",
      "description": "ISO 4217 currency code"
    },
    "revenue_items": {
      "type": "array",
      "items": {
        "type": "object",
        "required": ["id", "category", "amount", "fiscal_quarter"],
        "properties": {
          "id": {
            "type": "string",
            "pattern": "^REV-\\d{4}-\\d{6}$"
          },
          "category": {
            "type": "object",
            "required": ["primary"],
            "properties": {
              "primary": {
                "type": "string",
                "enum": [
                  "donor_funds",
                  "government_grants",
                  "self_generated",
                  "investment_returns",
                  "other_income"
                ]
              },
              "secondary": {
                "type": "string"
              }
            }
          },
          "amount": {
            "type": "number",
            "minimum": 0,
            "exclusiveMinimum": true
          },
          "fiscal_quarter": {
            "type": "integer",
            "minimum": 1,
            "maximum": 4
          },
          "donor_info": {
            "type": "object",
            "required": ["donor_id", "donor_type"],
            "properties": {
              "donor_id": {
                "type": "string",
                "pattern": "^DON-\\d{6}$"
              },
              "donor_type": {
                "type": "string",
                "enum": [
                  "individual",
                  "organization",
                  "government",
                  "ngo",
                  "foundation"
                ]
              },
              "donor_name": {
                "type": "string",
                "minLength": 1
              },
              "grant_reference": {
                "type": "string"
              },
              "restrictions": {
                "type": "array",
                "items": {
                  "type": "string"
                }
              }
            }
          },
          "settlement_info": {
            "$ref": "#/definitions/settlementInfo"
          },
          "audit_info": {
            "$ref": "#/definitions/auditInfo"
          }
        }
      }
    },
    "spending_items": {
      "type": "array",
      "items": {
        "type": "object",
        "required": ["id", "category", "amount", "fiscal_quarter"],
        "properties": {
          "id": {
            "type": "string",
            "pattern": "^EXP-\\d{4}-\\d{6}$"
          },
          "category": {
            "type": "object",
            "required": ["primary", "secondary"],
            "properties": {
              "primary": {
                "type": "string",
                "enum": [
                  "general_administration",
                  "community_services",
                  "economic_affairs",
                  "environmental_protection",
                  "housing_community",
                  "health",
                  "recreation_culture_religion",
                  "education",
                  "social_protection",
                  "settlement_development",
                  "cultural_preservation",
                  "advocacy_international_relations"
                ]
              },
              "secondary": {
                "type": "string"
              },
              "tertiary": {
                "type": "string"
              }
            }
          },
          "amount": {
            "type": "number",
            "minimum": 0,
            "exclusiveMinimum": true
          },
          "fiscal_quarter": {
            "type": "integer",
            "minimum": 1,
            "maximum": 4
          },
          "donor_reference": {
            "type": "string"
          },
          "settlement_info": {
            "$ref": "#/definitions/settlementInfo"
          },
          "beneficiaries": {
            "type": "object",
            "properties": {
              "total_count": {
                "type": "integer",
                "minimum": 1
              },
              "categories": {
                "type": "array",
                "items": {
                  "type": "string",
                  "enum": [
                    "students",
                    "elderly",
                    "monks_nuns",
                    "entrepreneurs",
                    "healthcare_recipients",
                    "youth",
                    "artists",
                    "households",
                    "other"
                  ]
                }
              },
              "details": {
                "type": "object",
                "additionalProperties": true
              }
            }
          },
          "program_details": {
            "type": "object",
            "additionalProperties": true
          },
          "audit_info": {
            "$ref": "#/definitions/auditInfo"
          },
          "tags": {
            "type": "array",
            "items": {
              "type": "string"
            }
          }
        }
      }
    }
  },
  "definitions": {
    "settlementInfo": {
      "type": "object",
      "required": ["settlement_id", "settlement_name", "region"],
      "properties": {
        "settlement_id": {
          "type": "string",
          "pattern": "^SET-\\d{3}$"
        },
        "settlement_name": {
          "type": "string"
        },
        "region": {
          "type": "string",
          "enum": ["uttarakhand", "himachal", "karnataka", "other"]
        },
        "population": {
          "type": "integer",
          "minimum": 0
        }
      }
    },
    "auditInfo": {
      "type": "object",
      "required": ["created_at", "created_by"],
      "properties": {
        "created_at": {
          "type": "string",
          "format": "date-time"
        },
        "created_by": {
          "type": "string"
        },
        "modified_at": {
          "type": "string",
          "format": "date-time"
        },
        "modified_by": {
          "type": "string"
        },
        "approved_at": {
          "type": "string",
          "format": "date-time"
        },
        "approved_by": {
          "type": "string"
        },
        "notes": {
          "type": "string"
        }
      }
    }
  }
}
```

## 3.1. Base Structure

The schema follows a YAML/JSON format with two primary sections:
- Revenue Items
- Spending Items

```yaml
budget_document:
  type: object
  required: [fiscal_year, currency, revenue_items, spending_items]
```

## 3.2. Revenue Structure

Revenue items MUST include:
- Category classification
- Amount
- Source information
- Donor information (when applicable)

## 3.3. Spending Structure

Spending items MUST include:
- Three-level category classification
- Amount
- Settlement information
- Beneficiary information
- Audit trail

# 4. Core Components and CTA-Specific Implementations

## 4.1. Core Data Structures

## 4.1.1 Donor Information

```yaml
donor_info:
  type: object
  required: [donor_id, donor_type]
  properties:
    donor_id:
      type: string
    donor_type:
      type: string
      enum: [individual, organization, government, ngo, foundation]
```

## 4.2. Settlement Information

```yaml
settlement_info:
  type: object
  properties:
    settlement_id:
      type: string
    settlement_name:
      type: string
    region:
      type: string
      enum: [uttarakhand, himachal, karnataka, other]
```

## 4.3. Audit Information

```yaml
audit_info:
  type: object
  required: [created_at, created_by]
```

# 5. Data Types and Validation Rules

## 5.1. General Validation Rules

### 5.1.1 Identifiers
- Fiscal Year: YYYY-YYYY format
- Currency: ISO 4217 three-letter codes
- Donor ID: Alphanumeric string
- Settlement ID: Alphanumeric string

### 5.1.2 Monetary Values
- All amounts MUST be positive numbers
- Decimal places MUST NOT exceed 2
- Currency MUST be specified at document level

## 5.2. CTA-Specific Validation Rules

### 5.2.1 Settlement Validation
- Settlement names MUST match official CTA registry
- Region codes MUST be one of: uttarakhand, himachal, karnataka, other
- Population data SHOULD be updated annually

### 5.2.2 Program Validation
- Education programs MUST specify education level
- Healthcare programs MUST include facility information
- Cultural programs MUST specify duration and participant count
- Environmental projects MUST include impact metrics

### 5.2.3 Beneficiary Validation
- Age ranges MUST be specified for age-specific programs
- Beneficiary counts MUST NOT exceed settlement population
- Categories MUST match program type

### 5.2.4 Donor Fund Validation
- Restrictions MUST be explicitly listed
- Grant references MUST be unique
- Spending MUST NOT exceed allocated donor funds
- Fund utilization MUST match donor restrictions

## 5.1. Identifiers

- Fiscal Year: YYYY-YYYY format
- Currency: ISO 4217 three-letter codes
- Donor ID: Alphanumeric string
- Settlement ID: Alphanumeric string

## 5.2. Monetary Values

- All amounts MUST be positive numbers
- Decimal places MUST NOT exceed 2
- Currency MUST be specified at document level

## 5.3. Categories

Primary categories MUST be one of:
- Standard COFOG categories
- CTA-specific categories

# 6. Implementation Guidelines

## 6.1. File Format

The standard supports two formats:
- YAML (recommended for human readability)
- JSON (recommended for API interactions)

## 6.2. Version Control

Implementations MUST:
- Include schema version
- Maintain backward compatibility
- Document breaking changes

## 6.3. Error Handling

Systems implementing this standard SHOULD:
- Validate all required fields
- Check referential integrity
- Verify numerical constraints
- Validate category enumerations

# 7. Security Considerations

## 7.1. Data Privacy

- Personal donor information MUST be protected
- Beneficiary data MUST be anonymized
- Access controls MUST be implemented

## 7.2. Audit Requirements

- All modifications MUST be logged
- Approval chains MUST be documented
- Changes MUST be traceable

# 8. Implementation Examples and Use Cases

## 8.1. Common Budget Operations

### 8.1.1 Basic Revenue Entry

```yaml
revenue_items:
  - category:
      primary: donor_funds
      secondary: education_support
    amount: 5000000.00
    donor_info:
      donor_id: "DON123"
      donor_type: foundation
      donor_name: "Tibet Education Foundation"
```

### 8.1.2 Basic Spending Entry

```yaml
spending_items:
  - category:
      primary: education
      secondary: scholarship_programs
      tertiary: higher_education
    amount: 1000000.00
    donor_reference: "TEF-2024-001"
```

## 8.2. CTA-Specific Use Cases

### 8.2.1 Monastic Education Support

This use case demonstrates the handling of traditional monastic education funding:

```yaml
revenue_items:
  - category:
      primary: donor_funds
      secondary: monastic_education
    amount: 2500000.00
    donor_info:
      donor_id: "DON456"
      donor_type: foundation
      donor_name: "Buddhist Studies Foundation"
      grant_reference: "BSF-2024-002"
      restrictions: ["monastic_education_only", "traditional_studies"]

spending_items:
  - category:
      primary: education
      secondary: monastic_education
      tertiary: traditional_studies
    amount: 500000.00
    beneficiaries:
      total_count: 200
      categories: ["monks_nuns"]
      details:
        age_range: "18-25"
        study_level: "advanced"
```

### 8.2.2 Healthcare for Elderly in Settlements

Example of settlement-based healthcare management:

```yaml
spending_items:
  - category:
      primary: health
      secondary: elderly_care
      tertiary: medical_supplies
    amount: 300000.00
    settlement_info:
      settlement_id: "SET002"
      settlement_name: "Mundgod"
      region: "karnataka"
      healthcare_facilities:
        clinic_count: 2
        mobile_units: 1
    beneficiaries:
      total_count: 150
      categories: ["elderly"]
      details:
        age_range: "above_60"
        care_type: "regular_checkup"
```

### 8.2.3 Cultural Preservation Projects

Demonstrating cultural program tracking:

```yaml
spending_items:
  - category:
      primary: cultural_preservation
      secondary: performing_arts
      tertiary: training_program
    program_details:
      duration_months: 6
      participants: 30
      activities: ["traditional_dance", "music_training", "costume_making"]
```

### 8.2.4 Entrepreneurship Development

Example of business development programs:

```yaml
spending_items:
  - category:
      primary: economic_affairs
      secondary: entrepreneurship
      tertiary: skill_development
    program_details:
      program_type: "business_training"
      duration_weeks: 12
      mentorship_hours: 40
    beneficiaries:
      categories: ["entrepreneurs"]
      details:
        business_sector: ["handicrafts", "tourism", "food_production"]
```

### 8.2.5 Green Settlement Initiative

Environmental sustainability project example:

```yaml
spending_items:
  - category:
      primary: settlement_development
      secondary: environmental_sustainability
      tertiary: solar_power
    settlement_info:
      project_details:
        solar_capacity_kw: 100
        beneficiary_households: 50
        estimated_co2_reduction: "75 tons/year"
```

## 8.1. Revenue Entry

```yaml
revenue_items:
  - category:
      primary: donor_funds
      secondary: education_support
    amount: 5000000.00
    donor_info:
      donor_id: "DON123"
      donor_type: foundation
      donor_name: "Tibet Education Foundation"
```

## 8.2. Spending Entry

```yaml
spending_items:
  - category:
      primary: education
      secondary: scholarship_programs
      tertiary: higher_education
    amount: 1000000.00
    donor_reference: "TEF-2024-001"
```

```json
{
  "metadata": {
    "version": "1.0.0",
    "created_at": "2024-01-15T10:00:00Z",
    "updated_at": "2024-01-15T10:00:00Z"
  },
  "fiscal_year": "2024-2025",
  "currency": "INR",
  "revenue_items": [
    {
      "id": "REV-2024-000001",
      "category": {
        "primary": "donor_funds",
        "secondary": "education_support"
      },
      "amount": 5000000.00,
      "fiscal_quarter": 1,
      "donor_info": {
        "donor_id": "DON-000123",
        "donor_type": "foundation",
        "donor_name": "Tibet Education Foundation",
        "grant_reference": "TEF-2024-001",
        "restrictions": [
          "education_only",
          "scholarship_priority"
        ]
      },
      "audit_info": {
        "created_at": "2024-01-15T10:00:00Z",
        "created_by": "finance_officer",
        "approved_at": "2024-01-15T11:00:00Z",
        "approved_by": "finance_director",
        "notes": "Annual education grant"
      }
    },
    {
      "id": "REV-2024-000002",
      "category": {
        "primary": "donor_funds",
        "secondary": "monastic_education"
      },
      "amount": 2500000.00,
      "fiscal_quarter": 2,
      "donor_info": {
        "donor_id": "DON-000456",
        "donor_type": "foundation",
        "donor_name": "Buddhist Studies Foundation",
        "grant_reference": "BSF-2024-002",
        "restrictions": [
          "monastic_education_only",
          "traditional_studies"
        ]
      },
      "audit_info": {
        "created_at": "2024-01-15T10:00:00Z",
        "created_by": "finance_officer"
      }
    }
  ],
  "spending_items": [
    {
      "id": "EXP-2024-000001",
      "category": {
        "primary": "education",
        "secondary": "scholarship_programs",
        "tertiary": "higher_education"
      },
      "amount": 1000000.00,
      "fiscal_quarter": 2,
      "donor_reference": "TEF-2024-001",
      "settlement_info": {
        "settlement_id": "SET-001",
        "settlement_name": "Dharamshala",
        "region": "himachal",
        "population": 8500
      },
      "beneficiaries": {
        "total_count": 50,
        "categories": ["students"],
        "details": {
          "education_level": "undergraduate",
          "age_range": "18-25",
          "fields_of_study": ["medicine", "engineering", "business"]
        }
      },
      "program_details": {
        "academic_year": "2024-2025",
        "semester": "Fall",
        "scholarship_type": "full",
        "includes_living_expense": true
      },
      "audit_info": {
        "created_at": "2024-01-15T10:00:00Z",
        "created_by": "education_officer",
        "approved_at": "2024-01-15T14:00:00Z",
        "approved_by": "education_secretary"
      },
      "tags": [
        "higher_education",
        "scholarships",
        "donor_funded"
      ]
    },
    {
      "id": "EXP-2024-000002",
      "category": {
        "primary": "cultural_preservation",
        "secondary": "performing_arts",
        "tertiary": "training_program"
      },
      "amount": 250000.00,
      "fiscal_quarter": 3,
      "settlement_info": {
        "settlement_id": "SET-002",
        "settlement_name": "Bylakuppe",
        "region": "karnataka",
        "population": 12000
      },
      "beneficiaries": {
        "total_count": 30,
        "categories": ["youth", "artists"],
        "details": {
          "age_range": "16-30",
          "program_duration": "6 months",
          "performance_types": [
            "traditional_dance",
            "music",
            "opera"
          ]
        }
      },
      "program_details": {
        "duration_months": 6,
        "weekly_hours": 20,
        "includes_materials": true,
        "culminating_event": "Annual Cultural Festival"
      },
      "audit_info": {
        "created_at": "2024-01-15T10:00:00Z",
        "created_by": "cultural_officer"
      },
      "tags": [
        "culture",
        "arts",
        "youth_program"
      ]
    }
  ]
}
```

# 9. References

1. Classification of Functions of Government (COFOG)
2. ISO 4217 Currency Codes
3. Government Finance Statistics Manual 2014 (IMF)

