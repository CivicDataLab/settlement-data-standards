# Administrative Units Hierarchy Standard
Version 0.1 | 13th January 2024 | DRAFT
Contributors: Prajna Prayas

## Table of Contents
1. Introduction
2. Scope
3. Administrative Hierarchy
4. Implementation Notes
5. Implementation Requirements
6. Annexes

## 1. Introduction

### 1.1 Purpose
This standard defines the hierarchical structure of settlement administrative units based on ISO 3166 and national administrative frameworks.

### 1.2 Normative References
- ISO 3166-1, -2 Geographic Codes
- National Administrative Classification System
- OGC Standards for Geographic Information

## 2. Scope

### 2.1 Coverage
- National to local level administrative units to specific locale mapping
- Special administrative zones
- Historical administrative boundaries

### 2.2 Applicability
- Government agencies
- Spatial data infrastructure
- Urban planning systems
- Statistical reporting

## 3. Administrative Hierarchy

### 3.1 Hierarchy Levels

| Level | Name | Description | Example | Required Properties |
|-------|------|-------------|---------|-------------------|
| 0 | Country | Sovereign state | India | ISO 3166-1 code |
| 1 | State/Province | First-level division | Himachal Pradesh | ISO 3166-2 code |
| 2 | District | Administrative district | Kangra | District code |
| 3 | Sub-district | Subdivision of district | Dharamshala | Sub-district code |
| 4 | Municipality/City | Urban local body | McLeodganj | Local body code |
| 5 | Ward/Zone | Municipal division | Ward 54 | Ward number |
| 6 | Block/Locality | Neighborhood unit | Yogiwara | Local identifier |

### 3.2 Administrative Unit Schema

```json
{
  "type": "Feature",
  "id": "string(uuid)",
  "properties": {
    "adminUnit": {
      "level": "integer(0-6)",
      "code": "string",
      "name": {
        "default": "string",
        "local": "string",
        "alternative": ["string"]
      },
      "validityPeriod": {
        "start": "date",
        "end": "date"
      },
      "parent": {
        "code": "string",
        "level": "integer"
      },
      "status": "enum(official, unofficial, disputed)",
      "population": "integer",
      "lastCensus": "date",
      "area": {
        "value": "number",
        "unit": "string"
      },
      "administrativePath":{
         "country": {
          "code": "string",
          "name": "string",
          "level": 0
        },
        "stateOrProvince": {
          "code": "string",
          "name": "string",
          "level": 1
        },
        "district": {
          "code": "string",
          "name": "string",
          "level": 2
        },
        "subdistrictOrSubdivision": {
          "code": "string",
          "name": "string",
          "level": 3
        },
        "parentSettlement": {
          "code": "string",
          "name": "string",
          "level": 4
        },
        "childSettlement": {
          "code": "string",
          "name": "string",
          "level": 5
        },
        "locale": {
          "code": "string",
          "name": "string",
          "level": 6
        }
      }
    }
  },
    "geometry": {
    "type": "Polygon",
     "coordinates": [[[number]]]
  }
}
```

## 4. Implementation Notes

1. Code Structure:
   - Hierarchical code format: Country-State-District-Subdistrict-Municipality-Settlement-Locale
   - Each level uses standardized codes where available (ISO 3166-1, ISO 3166-2)
   - Local codes follow administrative standards

2. Naming Conventions:
   - Default name: Internationally recognized name
   - Local name: Name in local script (Tibetan for monastery, Hindi for administrative units)
   - Alternative names: Historical or commonly used variants

3. Geographical Hierarchy:
   - Complete path from country to locale level
   - Each level references its immediate parent
   - Consistent status tracking across levels

4. Data Quality:
   - Population figures from latest census (2021)
   - Area measurements in square kilometers
   - Dates in ISO 8601 format
   - Status reflects official recognition

5. Validation Rules:
   - Each level must have a valid parent (except country)
   - Codes must follow standardized format
   - Temporal validity must be continuous
   - Geographic boundaries must nest properly


### 4.1 Relationship Rules

1. Hierarchical Containment
   - Each unit must have exactly one parent (except level 0)
   - No gaps in hierarchy allowed
   - No overlaps at same level allowed

2. Temporal Validity
   - All units must have validity period
   - No temporal gaps in administrative coverage
   - Historical boundaries must be preserved

3. Spatial Integrity
   - Complete coverage required at each level
   - Topology must be maintained
   - Boundaries must align with parent unit

## 5. Implementation Requirements

### 5.1 Spatial Requirements
- Minimum mapping unit: 100 m²
- Position accuracy: ±1m for urban, ±5m for rural
- Topology rules enforcement

### 5.2 Attribute Requirements
- Mandatory codes for all levels
- Bilingual names where applicable
- Population updates annually
- Area calculations in metric system

### 5.3 Temporal Requirements
- Change tracking mandatory
- Historical preservation required
- Version control implementation

## 6. Annexes

### Annex A: Administrative Code Lists
# Indian Level 1 Administrative Divisions - Code List Standard
Version 1.0 | January 2024

## 1. States Code List

| ISO 3166-2 | State Code | Default Name | Local Name | Alternative Names | Formation Date | Status | Category |
|------------|------------|--------------|------------|-------------------|----------------|---------|----------|
| IN-AN | AN | Andhra Pradesh | ఆంధ్ర ప్రదేశ్ | AP | 1956-11-01 | active | State |
| IN-AR | AR | Arunachal Pradesh | अरुणाचल प्रदेश | NEFA (pre-1972) | 1987-02-20 | active | State |
| IN-AS | AS | Assam | অসম | Ahom | 1947-08-15 | active | State |
| IN-BR | BR | Bihar | बिहार | - | 1947-08-15 | active | State |
| IN-CT | CT | Chhattisgarh | छत्तीसगढ़ | - | 2000-11-01 | active | State |
| IN-GA | GA | Goa | गोंय | - | 1987-05-30 | active | State |
| IN-GJ | GJ | Gujarat | ગુજરાત | - | 1960-05-01 | active | State |
| IN-HR | HR | Haryana | हरियाणा | - | 1966-11-01 | active | State |
| IN-HP | HP | Himachal Pradesh | हिमाचल प्रदेश | - | 1971-01-25 | active | State |
| IN-JH | JH | Jharkhand | झारखंड | - | 2000-11-15 | active | State |
| IN-KA | KA | Karnataka | ಕರ್ನಾಟಕ | Mysore (pre-1973) | 1956-11-01 | active | State |
| IN-KL | KL | Kerala | കേരളം | - | 1956-11-01 | active | State |
| IN-MP | MP | Madhya Pradesh | मध्य प्रदेश | MP | 1947-08-15 | active | State |
| IN-MH | MH | Maharashtra | महाराष्ट्र | - | 1960-05-01 | active | State |
| IN-MN | MN | Manipur | মণিপুর | - | 1972-01-21 | active | State |
| IN-ML | ML | Meghalaya | मेघालय | - | 1972-01-21 | active | State |
| IN-MZ | MZ | Mizoram | मिज़ोरम | - | 1987-02-20 | active | State |
| IN-NL | NL | Nagaland | नागालैंड | - | 1963-12-01 | active | State |
| IN-OR | OR | Odisha | ଓଡ଼ିଶା | Orissa (pre-2011) | 1947-08-15 | active | State |
| IN-PB | PB | Punjab | ਪੰਜਾਬ | - | 1947-08-15 | active | State |
| IN-RJ | RJ | Rajasthan | राजस्थान | - | 1947-08-15 | active | State |
| IN-SK | SK | Sikkim | सिक्किम | - | 1975-05-16 | active | State |
| IN-TN | TN | Tamil Nadu | தமிழ்நாடு | Madras (pre-1969) | 1947-08-15 | active | State |
| IN-TG | TG | Telangana | తెలంగాణ | - | 2014-06-02 | active | State |
| IN-TR | TR | Tripura | ত্রিপুরা | - | 1972-01-21 | active | State |
| IN-UP | UP | Uttar Pradesh | उत्तर प्रदेश | UP | 1947-08-15 | active | State |
| IN-UT | UT | Uttarakhand | उत्तराखण्ड | Uttaranchal (2000-2006) | 2000-11-09 | active | State |
| IN-WB | WB | West Bengal | পশ্চিমবঙ্গ | Bengal (pre-1947) | 1947-08-15 | active | State |

## 2. Union Territories Code List

| ISO 3166-2 | UT Code | Default Name | Local Name | Alternative Names | Formation Date | Status | Category |
|------------|---------|--------------|------------|-------------------|----------------|---------|----------|
| IN-AN | AN | Andaman and Nicobar Islands | अंडमान और निकोबार द्वीपसमूह | - | 1956-11-01 | active | Union Territory |
| IN-CH | CH | Chandigarh | चंडीगढ़ | - | 1966-11-01 | active | Union Territory |
| IN-DN | DN | Dadra and Nagar Haveli and Daman and Diu | दादरा और नगर हवेली और दमन और दीव | - | 2020-01-26 | active | Union Territory |
| IN-DL | DL | Delhi | दिल्ली | National Capital Territory of Delhi | 1956-11-01 | active | Union Territory |
| IN-JK | JK | Jammu and Kashmir | जम्मू और कश्मीर | - | 2019-10-31 | active | Union Territory |
| IN-LA | LA | Ladakh | लद्दाख | - | 2019-10-31 | active | Union Territory |
| IN-LD | LD | Lakshadweep | लक्षद्वीप | Laccadive Islands (pre-1973) | 1956-11-01 | active | Union Territory |
| IN-PY | PY | Puducherry | புதுச்சேரி | Pondicherry (pre-2006) | 1962-08-16 | active | Union Territory |

## 3. Code Structure

### 3.1 Format Rules
- ISO 3166-2: IN-XX format
- State/UT Code: 2-letter code
- All codes are uppercase
- No special characters or numbers

### 3.2 Naming Conventions
- Default Name: Official English name
- Local Name: Name in primary official language
- Alternative Names: Historical or commonly used variants

## 4. Implementation Notes

### 4.1 Usage Requirements
1. Always use official codes in data exchange
2. Include formation date for temporal queries
3. Maintain alternative names for historical searches
4. Include local names for multilingual support

### 4.2 Validation Rules
1. Codes must match ISO 3166-2 format
2. Names must use official spellings
3. Dates must be in ISO 8601 format
4. Status must reflect current official status

### Annex B: Implementation Examples

# Administrative Unit Schema - Namgyal Monastery Example

## 1. Schema Mapping

Here's how you can map Namgyal Monastery following the above standardisation:

```json
{
  "type": "Feature",
  "properties": {
    "adminUnit": {
      "level": 6,
      "code": "IN-HP-KAN-DHM-DHM-MCD-NAM",
      "name": {
        "default": "Namgyal Monastery",
        "local": "རྣམ་རྒྱལ་དགོན་པ",
        "alternative": ["Dalai Lama Temple", "Namgyal Dratsang"]
      },
      "validityPeriod": {
        "start": "YYYY-MM-DD",
        "end": null
      },
      "parent": {
        "code": "IN-HP-KAN-DHM-DHM-MCD",
        "level": 5
      },
      "status": "official",
      "population": 185,
      "lastCensus": "2021-03-01",
      "area": {
        "value": 0.024,
        "unit": "square_kilometers"
      },
      "administrativePath": {
        "country": {
          "code": "IND",
          "name": "India",
          "level": 0
        },
        "stateOrProvince": {
          "code": "IN-HP",
          "name": "Himachal Pradesh",
          "level": 1
        },
        "district": {
          "code": "KAN",
          "name": "Kangra",
          "level": 2
        },
        "subdistrictOrSubdivision": {
          "code": "DHM",
          "name": "Dharamshala",
          "level": 3
        },
        "parentSettlement": {
          "code": "DHM",
          "name": "Dharamshala",
          "level": 4
        },
        "childSettlement": {
          "code": "MCD",
          "name": "McLeodganj",
          "level": 5
        },
        "locale": {
          "code": "NAM",
          "name": "Namgyal Monastery",
          "level": 6
        }
      }
    }
  },
  "geometry": {
    "type": "Polygon",
    "coordinates": [
      [
        [76.3214, 32.2381],
        [76.3224, 32.2381],
        [76.3224, 32.2385],
        [76.3214, 32.2385],
        [76.3214, 32.2381]
      ]
    ]
  }
}
```
