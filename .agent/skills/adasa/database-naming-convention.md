# AI-Compatible Database Naming Convention Guide for DI Team

## 1. Table Naming Rules (Pattern: TXXYName)

### 1.1 Structure Definition
```
T + XX + Y + Name
├── T: Fixed prefix "T" (Table identifier)
├── XX: Module code (2 characters, uppercase)
├── Y: Table type (1 character, uppercase)  
└── Name: Table name (max 12 characters, PascalCase)
```

### 1.2 Module Codes (XX) - Validation Dictionary
```json
{
  "DI": "Data Integration",
  "DW": "Data Warehouse", 
  "BI": "Business Intelligence",
  "ET": "ETL Process",
  "MD": "Master Data",
  "MT": "Metadata",
  "DQ": "Data Quality",
  "DG": "Data Governance",
  "DP": "Data Pipeline",
  "DS": "Data Science"
}
```

### 1.3 Table Types (Y) - Validation Dictionary
```json
{
  "M": "Master Data",
  "T": "Transaction Data", 
  "S": "System Configuration",
  "L": "Log Data",
  "D": "Dimension Table",
  "F": "Fact Table",
  "A": "Aggregate Table",
  "H": "History Table"
}
```

### 1.4 Table Name Validation Rules
```yaml
table_name_rules:
  max_length: 12
  allowed_chars: "[A-Za-z0-9]"
  case_style: "PascalCase"
  forbidden_words: ["temp", "tmp", "test", "backup"]
  required_pattern: "^T[A-Z]{2}[MTSLDFAH][A-Za-z0-9]{1,12}$"
```

## 2. Field Naming Rules (Pattern: FXAbcName)

### 2.1 Structure Definition
```
F + X + Abc + Name
├── F: Fixed prefix "F" (Field identifier)
├── X: Data type indicator (1 character, uppercase)
├── Abc: Table abbreviation (3 characters, PascalCase)
└── Name: Field name (max 10 characters, PascalCase)
```

### 2.2 Data Type Indicators (X) - Validation Dictionary
```json
{
  "T": "Text/String (VARCHAR, NVARCHAR, CHAR)",
  "D": "Date/DateTime (DATE, DATETIME, TIMESTAMP)",
  "N": "Numeric Integer (INT, BIGINT, SMALLINT)",
  "C": "Numeric Decimal (DECIMAL, FLOAT, NUMERIC)",
  "B": "Boolean (BIT, BOOLEAN)",
  "J": "JSON (JSON, JSONB)",
  "X": "XML (XML)",
  "I": "Image/Binary (BLOB, VARBINARY, IMAGE)",
  "G": "GUID/UUID (UNIQUEIDENTIFIER, UUID)"
}
```

### 2.3 Field Name Validation Rules
```yaml
field_name_rules:
  max_length: 10
  allowed_chars: "[A-Za-z0-9]"
  case_style: "PascalCase"
  required_pattern: "^F[TDNCBJXIG][A-Z][a-z]{2}[A-Za-z0-9]{1,10}$"
  reserved_names: ["ID", "Code", "Name", "Desc", "Status", "Type", "Date", "Time", "User", "Create", "Update", "Delete"]
```

## 3. Validation Schemas

### 3.1 Table Validation Schema
```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "type": "object",
  "properties": {
    "tableName": {
      "type": "string",
      "pattern": "^T[A-Z]{2}[MTSLDFAH][A-Za-z0-9]{1,12}$",
      "maxLength": 16
    },
    "moduleCode": {
      "type": "string", 
      "enum": ["DI", "DW", "BI", "ET", "MD", "MT", "DQ", "DG", "DP", "DS"]
    },
    "tableType": {
      "type": "string",
      "enum": ["M", "T", "S", "L", "D", "F", "A", "H"]
    },
    "description": {
      "type": "string",
      "minLength": 10,
      "maxLength": 200
    }
  },
  "required": ["tableName", "moduleCode", "tableType", "description"]
}
```

### 3.2 Field Validation Schema
```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "type": "object", 
  "properties": {
    "fieldName": {
      "type": "string",
      "pattern": "^F[TDNCBJXIG][A-Z][a-z]{2}[A-Za-z0-9]{1,10}$",
      "maxLength": 15
    },
    "dataType": {
      "type": "string",
      "enum": ["T", "D", "N", "C", "B", "J", "X", "I", "G"]
    },
    "tableAbbreviation": {
      "type": "string",
      "pattern": "^[A-Z][a-z]{2}$"
    },
    "isPrimaryKey": {
      "type": "boolean"
    },
    "isForeignKey": {
      "type": "boolean"
    },
    "isNullable": {
      "type": "boolean"
    }
  },
  "required": ["fieldName", "dataType", "tableAbbreviation"]
}
```

## 4. AI Validation Prompts

### 4.1 Table Name Validation Prompt
```
Validate the following table name against DI team naming conventions:

Table Name: {table_name}

Check:
1. Starts with "T"
2. Module code (positions 2-3) is valid: DI|DW|BI|ET|MD|MT|DQ|DG|DP|DS
3. Table type (position 4) is valid: M|T|S|L|D|F|A|H
4. Name part (positions 5+) follows PascalCase and max 12 chars
5. Total length ≤ 16 characters
6. No forbidden words: temp, tmp, test, backup

Pattern: ^T[A-Z]{2}[MTSLDFAH][A-Za-z0-9]{1,12}$

Return: {valid: boolean, errors: string[], suggestions: string[]}
```

### 4.2 Field Name Validation Prompt
```
Validate the following field name against DI team naming conventions:

Field Name: {field_name}
Table Name: {table_name}

Check:
1. Starts with "F"
2. Data type indicator (position 2) is valid: T|D|N|C|B|J|X|I|G
3. Table abbreviation (positions 3-5) matches table pattern
4. Field name part follows PascalCase and max 10 chars
5. Total length ≤ 15 characters
6. Data type matches SQL column type

Pattern: ^F[TDNCBJXIG][A-Z][a-z]{2}[A-Za-z0-9]{1,10}$

Return: {valid: boolean, errors: string[], dataTypeMatch: boolean}
```

## 5. Code Examples for AI Integration

### 5.1 Python Validation Function
```python
import re
from typing import Dict, List, Tuple

class DINameValidator:
    MODULE_CODES = {"DI", "DW", "BI", "ET", "MD", "MT", "DQ", "DG", "DP", "DS"}
    TABLE_TYPES = {"M", "T", "S", "L", "D", "F", "A", "H"}
    DATA_TYPES = {"T", "D", "N", "C", "B", "J", "X", "I", "G"}
    
    @staticmethod
    def validate_table_name(table_name: str) -> Dict:
        pattern = r"^T([A-Z]{2})([MTSLDFAH])([A-Za-z0-9]{1,12})$"
        match = re.match(pattern, table_name)
        
        if not match:
            return {"valid": False, "error": "Invalid table name pattern"}
        
        module_code, table_type, name_part = match.groups()
        
        errors = []
        if module_code not in DINameValidator.MODULE_CODES:
            errors.append(f"Invalid module code: {module_code}")
        if table_type not in DINameValidator.TABLE_TYPES:
            errors.append(f"Invalid table type: {table_type}")
        if len(table_name) > 16:
            errors.append("Table name too long (max 16 chars)")
            
        return {
            "valid": len(errors) == 0,
            "errors": errors,
            "module_code": module_code,
            "table_type": table_type,
            "name_part": name_part
        }
    
    @staticmethod 
    def validate_field_name(field_name: str, table_name: str) -> Dict:
        pattern = r"^F([TDNCBJXIG])([A-Z][a-z]{2})([A-Za-z0-9]{1,10})$"
        match = re.match(pattern, field_name)
        
        if not match:
            return {"valid": False, "error": "Invalid field name pattern"}
        
        data_type, table_abbr, name_part = match.groups()
        
        errors = []
        if data_type not in DINameValidator.DATA_TYPES:
            errors.append(f"Invalid data type indicator: {data_type}")
        if len(field_name) > 15:
            errors.append("Field name too long (max 15 chars)")
            
        return {
            "valid": len(errors) == 0,
            "errors": errors,
            "data_type": data_type,
            "table_abbreviation": table_abbr,
            "name_part": name_part
        }
```

### 5.2 SQL Validation Query
```sql
-- Table name validation function
CREATE FUNCTION ValidateTableName(@TableName NVARCHAR(50))
RETURNS TABLE
AS
RETURN
(
    SELECT 
        @TableName AS TableName,
        CASE 
            WHEN @TableName LIKE 'T[A-Z][A-Z][MTSLDFAH]%' 
                AND LEN(@TableName) <= 16
                AND SUBSTRING(@TableName, 2, 2) IN ('DI','DW','BI','ET','MD','MT','DQ','DG','DP','DS')
                AND SUBSTRING(@TableName, 4, 1) IN ('M','T','S','L','D','F','A','H')
            THEN 1 
            ELSE 0 
        END AS IsValid,
        SUBSTRING(@TableName, 2, 2) AS ModuleCode,
        SUBSTRING(@TableName, 4, 1) AS TableType,
        SUBSTRING(@TableName, 5, LEN(@TableName)-4) AS NamePart
);

-- Field name validation function  
CREATE FUNCTION ValidateFieldName(@FieldName NVARCHAR(50))
RETURNS TABLE
AS
RETURN
(
    SELECT 
        @FieldName AS FieldName,
        CASE 
            WHEN @FieldName LIKE 'F[TDNCBJXIG][A-Z][a-z][a-z]%'
                AND LEN(@FieldName) <= 15
                AND SUBSTRING(@FieldName, 2, 1) IN ('T','D','N','C','B','J','X','I','G')
            THEN 1 
            ELSE 0 
        END AS IsValid,
        SUBSTRING(@FieldName, 2, 1) AS DataType,
        SUBSTRING(@FieldName, 3, 3) AS TableAbbr,
        SUBSTRING(@FieldName, 6, LEN(@FieldName)-5) AS NamePart
);
```

## 6. AI Training Data Examples

### 6.1 Valid Table Names
```json
[
  {"name": "TDIMSource", "module": "DI", "type": "M", "description": "Data source master"},
  {"name": "TDITProcess", "module": "DI", "type": "T", "description": "Process transactions"},
  {"name": "TDWSales", "module": "DW", "type": "F", "description": "Sales fact table"},
  {"name": "TBIDCustomer", "module": "BI", "type": "D", "description": "Customer dimension"},
  {"name": "TETLConfig", "module": "ET", "type": "S", "description": "ETL configuration"}
]
```

### 6.2 Invalid Table Names with Reasons
```json
[
  {"name": "Source", "reason": "Missing T prefix and module code"},
  {"name": "TDIMSourceData", "reason": "Name part too long (>12 chars)"},
  {"name": "TXYTABLE", "reason": "Invalid module code XY"},
  {"name": "TDIXSource", "reason": "Invalid table type X"},
  {"name": "tdimsource", "reason": "Should be PascalCase"}
]
```

### 6.3 Valid Field Names
```json
[
  {"name": "FTSrcCode", "dataType": "T", "abbr": "Src", "meaning": "Source code text"},
  {"name": "FDPrcStart", "dataType": "D", "abbr": "Prc", "meaning": "Process start date"},
  {"name": "FNConCount", "dataType": "N", "abbr": "Con", "meaning": "Connection count"},
  {"name": "FCSchDuration", "dataType": "C", "abbr": "Sch", "meaning": "Schedule duration"},
  {"name": "FBLogActive", "dataType": "B", "abbr": "Log", "meaning": "Log active flag"}
]
```

## 7. Automated Validation Rules

### 7.1 Table Creation Validation
```yaml
table_validation:
  pre_creation_checks:
    - name_pattern_validation
    - module_code_verification  
    - table_type_verification
    - length_validation
    - duplicate_check
  
  required_fields:
    - primary_key_field
    - created_date_field
    - updated_date_field
    - status_field
    
  forbidden_patterns:
    - ".*temp.*"
    - ".*test.*" 
    - ".*backup.*"
    - ".*copy.*"
```

### 7.2 Field Creation Validation
```yaml
field_validation:
  pre_creation_checks:
    - name_pattern_validation
    - data_type_consistency
    - table_abbreviation_match
    - length_validation
    - reserved_name_check
    
  data_type_mappings:
    T: ["VARCHAR", "NVARCHAR", "CHAR", "TEXT"]
    D: ["DATE", "DATETIME", "TIMESTAMP"]
    N: ["INT", "BIGINT", "SMALLINT", "TINYINT"]
    C: ["DECIMAL", "NUMERIC", "FLOAT", "REAL"]
    B: ["BIT", "BOOLEAN"]
    J: ["JSON", "JSONB"]
    X: ["XML"]
    I: ["BLOB", "VARBINARY", "IMAGE"]
    G: ["UNIQUEIDENTIFIER", "UUID"]
```

## 8. Error Messages and Suggestions

### 8.1 Standardized Error Messages
```json
{
  "INVALID_TABLE_PREFIX": "Table name must start with 'T'",
  "INVALID_MODULE_CODE": "Module code must be one of: DI, DW, BI, ET, MD, MT, DQ, DG, DP, DS",
  "INVALID_TABLE_TYPE": "Table type must be one of: M, T, S, L, D, F, A, H", 
  "TABLE_NAME_TOO_LONG": "Table name exceeds 16 characters maximum",
  "INVALID_FIELD_PREFIX": "Field name must start with 'F'",
  "INVALID_DATA_TYPE": "Data type indicator must be one of: T, D, N, C, B, J, X, I, G",
  "FIELD_NAME_TOO_LONG": "Field name exceeds 15 characters maximum",
  "ABBR_MISMATCH": "Table abbreviation doesn't match table name pattern"
}
```

### 8.2 Auto-suggestion Algorithm
```python
def suggest_corrections(invalid_name: str, name_type: str) -> List[str]:
    suggestions = []
    
    if name_type == "table":
        if not invalid_name.startswith('T'):
            suggestions.append(f"T{invalid_name}")
        
        # Add module code if missing
        if len(invalid_name) < 4:
            for module in MODULE_CODES:
                suggestions.append(f"T{module}M{invalid_name[1:]}")
    
    elif name_type == "field":
        if not invalid_name.startswith('F'):
            suggestions.append(f"F{invalid_name}")
        
        # Suggest data type based on name patterns
        if any(word in invalid_name.lower() for word in ['name', 'desc', 'code']):
            suggestions.append(f"FT{invalid_name[1:]}")
        elif any(word in invalid_name.lower() for word in ['date', 'time']):
            suggestions.append(f"FD{invalid_name[1:]}")
    
    return suggestions[:3]  # Return top 3 suggestions
```

This structured format enables AI systems to:
1. Parse naming rules programmatically
2. Validate names against defined patterns
3. Generate specific error messages
4. Suggest corrections automatically
5. Learn from examples and patterns
6. Integrate with development tools and databases