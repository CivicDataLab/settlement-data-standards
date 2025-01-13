# Land Use and Land Cover Data Standard
Version:0.1

Last Modified: 13th January 2024

Contributors: Prajna Prayas

## Table of Contents
1. Introduction
2. Scope
3. Normative References
4. Terms and Definitions
5. Classification System
6. Data Model
7. Implementation Requirements


## 1. Introduction

### 1.1 Purpose
This standard defines the requirements for classifying and documenting land use and land cover (LULC) data based on the CORINE Land Cover (CLC) classification system with extensions for local requirements.

### 1.2 Background
This standard builds upon established international frameworks while accommodating specific local land use categories and requirements.

### 1.3 Benefits
- Standardized classification of land use/land cover
- Interoperability with international systems
- Consistent data quality and validation
- Support for temporal monitoring

## 2. Scope

### 2.1 Spatial Scope
- Minimum mapping unit: 0.25 hectares for artificial surfaces
- Minimum mapping unit: 1 hectare for other categories
- Minimum width: 10 meters for linear elements

### 2.2 Thematic Scope
- Land use classification
- Land cover characteristics
- Temporal changes
- Quality metrics

## 3. Normative References

- ISO 19115-1:2014 Geographic Metadata
- ISO 3166 for Administrative Divisions
- INSPIRE Data Specification on Land Use
- CORINE Land Cover technical guidelines
- OGC Standards for Geographic Information

## 4. Terms and Definitions

### 4.1 General Terms
- Land Use: Human utilization of land for specific purposes
- Land Cover: Physical material at the surface of the earth
- CORINE: Coordination of Information on the Environment
- Minimum Mapping Unit (MMU): Smallest area size that can be mapped

### 4.2 Classification Terms
## 1. CLC Nomenclature Structure

| Code | Category | Description | Settlement Application |
|------|-----------|-------------|----------------------|
| 1 | Artificial Surfaces | Built environment | Primary settlement areas |
| 2 | Agricultural Areas | Farming & cultivation | Agricultural projects |
| 3 | Forest/Semi-Natural | Natural vegetation | Conservation areas |
| 4 | Wetlands | Water-saturated lands | Natural water bodies |
| 5 | Water Bodies | Water surfaces | Water resources |


## 5. Classification System

### 5.1 CORINE Classification Hierarchy

#### 5.1.1 Level 1: Artificial Surfaces (Code 1)
| Level 2 Code | Level 3 Code | Name | Description | Examples |
|-------------|--------------|------|-------------|-----------|
| 1.1 | 1.1.1 | Residential | Housing areas | Camp settlements |
| | 1.1.2 | Mixed residential | Combined use | Shop-houses |
| 1.2 | 1.2.1 | Institutional | Administrative/Educational | CTSA offices |
| | 1.2.2 | Religious | Monasteries/Temples | Namgyal Monastery |
| | 1.2.3 | Cultural | Performance/Exhibition | TIPA |
| | 1.2.4 | Healthcare | Medical facilities | Delek Hospital |
| 1.3 | 1.3.1 | Commercial | Markets/Shops | Handicraft centers |
| | 1.3.2 | Tourism | Guest facilities | Hotels |
| 1.4 | 1.4.1 | Recreational | Sports/Parks | Community grounds |
| | 1.4.2 | Public spaces | Gathering areas | Community halls |

#### 5.1.2 Level 1: Agricultural Areas (Code 2)
| Level 2 Code | Level 3 Code | Name | Description | Examples |
|-------------|--------------|------|-------------|-----------|
| 2.1 | 2.1.1 | Community farming | Shared agricultural land | Organic farms |
| | 2.1.2 | Kitchen gardens | Small-scale cultivation | Home gardens |
| 2.2 | 2.2.1 | Cooperative farms | Group farming projects | Settlement farms |

#### 5.1.3 Level 1: Forest and Semi-Natural Areas (Code 3)
| Level 2 Code | Level 3 Code | Name | Description | Examples |
|-------------|--------------|------|-------------|-----------|
| 3.1 | 3.1.1 | Protected areas | Conservation zones | Sacred groves |
| | 3.1.2 | Natural forests | Wooded areas | Forest cover |
| 3.2 | 3.2.1 | Open spaces | Undeveloped land | Buffer zones |

#### 5.1.4 Level 1: Wetlands (Code 4)

| Level 2 Code | Level 3 Code | Name | Description | Examples |
|-------------|--------------|------|-------------|-----------|
| 4.1 | 4.1.1 | Natural wetlands | Natural water-saturated areas | Marsh areas |
| | 4.1.2 | Modified wetlands | Human-modified wet areas | Drainage zones |
| 4.2 | 4.2.1 | Highland wetlands | High altitude wet areas | Alpine meadows |
| | 4.2.2 | Seasonal wetlands | Periodic water accumulation | Monsoon pools |

#### 5.1.5 Level 1: Water Bodies (Code 5)
| Level 2 Code | Level 3 Code | Name | Description | Examples |
|-------------|--------------|------|-------------|-----------|
| 5.1 | 5.1.1 | Natural water bodies | Natural water features | Mountain streams |
| | 5.1.2 | Artificial water bodies | Man-made water features | Retention ponds |
| 5.2 | 5.2.1 | Water storage | Water collection | Community tanks |
| | 5.2.2 | Religious water bodies | Sacred water features | Temple pools |

### 5.2 Local Extensions
[Local Extensions(if any) to the above standards goes here]

## 6. Data Model

```json
{
  "id": "string(uuid)",
  "administrativeUnitId": "string(uuid)",
  "properties": {
    "metadata": {
      "identifier": {
        "code": "string",
        "namespace": "string", 
        "version": "string"
      },
      "dataSource": {
        "organization": "string",
        "collection": "string",
        "collectionDate": "string(ISO-8601)",
        "accuracy": "number(meters)",
        "methodologyReference": "string(URI)"
      },
      "quality": {
        "spatialResolution": "number(meters)",
        "temporalValidity": {
          "start": "string(ISO-8601)",
          "end": "string(ISO-8601)"
        },
        "completeness": "number(percentage)",
        "positionalAccuracy": "number(meters)"
      }
    },
    "landUse": {
      "classification": {
        "level1": {
          "code": "integer(1-5)",
          "name": "string"
        },
        "level2": {
          "code": "string",
          "name": "string"
        },
        "level3": {
          "code": "string", 
          "name": "string"
        }
      },
      "attributes": {
        "name": "string",
        "status": "enum(active, planned, historical)",
        "ownership": "enum(community, monastery, government, private)",
        "useType": "enum(primary, mixed, temporary)",
        "buildingDensity": {
          "value": "number",
          "unit": "percentage"
        },
        "developmentStatus": "enum(developed, underdeveloped, undeveloped)",
        "validityPeriod": {
          "start": "date",
          "end": "date"
        }
      },
      "specificUse": {
        "religious": {
          "type": "string",
          "tradition": "string",
          "capacity": "number",
          "yearEstablished": "date"
        },
        "institutional": {
          "type": "string",
          "organization": "string",
          "function": "string", 
          "operationalStatus": "enum(active, inactive)"
        },
        "waterBody": {
          "type": "string",
          "source": "enum(natural, artificial)",
          "seasonality": "enum(permanent, seasonal)",
          "usage": "string"
        }
      },
      "area": {
        "value": "number",
        "unit": "string"
      }
    }
  }
}
```

### 6.1 Core Schema
```json
{

  "properties": {
    "metadata": {
      // Metadata attributes 
    },
    "classification": {
      // Classification attributes 
    },
    "attributes": {
      // Physical and coverage attributes 
    }
  }
}
```

### 6.2 Required Attributes

#### 6.2.1 Core Attributes

```json
{
  "landUse": {
    "classification": {
      "level1": {
        "code": "integer(1-5)",
        "name": "string"
      },
      "level2": {
        "code": "string",
        "name": "string"
      },
      "level3": {
        "code": "string",
        "name": "string"
      }
    },
    "attributes": {
      "status": {
        "type": "enum",
        "values": ["active", "planned", "historical"]
      },
      "ownership": {
        "type": "enum",
        "values": ["community", "monastery", "government", "private"]
      },
      "useType": {
        "type": "enum",
        "values": ["primary", "mixed", "temporary"]
      },
      "buildingDensity": {
        "type": "number",
        "unit": "percentage"
      },
      "developmentStatus": {
        "type": "enum",
        "values": ["developed", "underdeveloped", "undeveloped"]
      }
    }
  }
}
```

### 6.3 Specific Use Attributes

#### 6.3.1 Religious/Cultural Properties
```json
{
  "religious": {
    "type": "string",
    "tradition": "string",
    "capacity": "number",
    "yearEstablished": "date"
  }
}
```

#### 6.3.2 Institutional Properties
```json
{
  "institutional": {
    "type": "string",
    "organization": "string",
    "function": "string",
    "operationalStatus": "enum(active, inactive)"
  }
}
```

## 3. Example Implementation

```json
{

  "id":"string(uuid)",
  "administrativeUnitId":"string(uuid)", //linkage with administrative units
  "properties": {
    "metadata":{
       "identifier": {
       "code": "string",
       "namespace": "string", 
       "version": "string"
     },
     "dataSource": {
       "organization": "string",
       "collection": "string",
       "collectionDate": "string(ISO-8601)",
       "accuracy": "number(meters)",
       "methodologyReference": "string(URI)"
     },
     "quality": {
       "spatialResolution": "number(meters)",
       "temporalValidity": {
         "start": "string(ISO-8601)",
         "end": "string(ISO-8601)"
       },
       "completeness": "number(percentage)",
       "positionalAccuracy": "number(meters)"
     }
    },
    "landUse": {
      "classification": {
        "level1": {
          "code": 1,
          "name": "Artificial Surfaces"
        },
        "level2": {
          "code": "1.2",
          "name": "Institutional"
        },
        "level3": {
          "code": "1.2.2",
          "name": "Religious"
        }
      },
      "attributes": {
          "name": "Namgyal Monastery",
        "status": "active",
        "ownership": "monastery",
        "useType": "primary",
        "buildingDensity": 65,
        "developmentStatus": "developed"
      },
      "specificUse":{
        "religious": {
        "type": "Monastery",
        "tradition": "Gelug",
        "capacity": 200,
        "yearEstablished": "YYYY"
      },

      },
 
    },
  
    "area": {
      "value": 0.024,
      "unit": "square_kilometers"
    }
  }
}
```

## 4. Implementation Guidelines

### 4.1 Classification Rules
1. Always use most detailed level (Level 3) when possible
2. Mixed-use areas should be classified by dominant use
3. Minimum mapping unit: 0.25 ha for artificial surfaces

### 4.2 Required Documentation
1. Primary use must be specified
2. Ownership status must be clear
3. Development status must be current
4. Historical status must be tracked

### 4.3 Quality Control
1. Regular updates (annual minimum)
2. Field verification required
3. Community validation recommended
4. Historical documentation preserved



