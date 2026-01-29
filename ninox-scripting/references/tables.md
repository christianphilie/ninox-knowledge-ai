# Ninox Databases - Tables

## Overview

This documentation describes concepts around tables in Ninox.

**Source**: https://forum.ninox.de/category/docs

---

## Table Basics

### Creating Tables
Tables are created in the Ninox interface and contain records.

### Records
- Each table contains records
- Each record has fields
- Fields can have various types

**Source**: Ninox Documentation - Tables

---

## Select Statements

### All Records
```ninox
select Table
```

### Filtered Records
```ninox
select Table where condition
```

### Sorted Records
```ninox
select Table order by field
select Table order by field desc
```

### Limited Count
```ninox
select Table limit 10
```

**Source**: https://ninox.com/de/tutorials/advanced-tutorial-2

---

## Record Operations

### Create Record
```ninox
let newRecord := create Table;
newRecord.Field := value;
```

### Read Record
```ninox
let record := first(select Table where ID = 1);
let value := record.Field;
```

### Update Record
```ninox
record.Field := newValue;
```

### Delete Record
```ninox
delete record;
```

**Source**: Ninox Documentation

---

## Field Access

### Simple Fields
```ninox
record.TextField
record.NumberField
record.DateField
```

### Relation Fields
```ninox
record.RelationField
record.RelationField.Field
```

**Source**: Ninox Documentation

---

## Table Relationships

### 1:N Relationship
A table can reference multiple records in another table.

### N:1 Relationship
Multiple records can reference one record in another table.

### M:N Relationship
Many-to-many relationship via intermediate table.

**Source**: https://forum.ninox.de/category/docs

---

## Important Notes

1. **Performance**: Use `select ... where` for better performance
2. **Relationships**: Use relation fields for links between tables
3. **Fields**: Direct access via dot notation

---

**Sources**:
- Official Documentation: https://forum.ninox.de/category/docs
- Ninox Tutorials: https://ninox.com/de/tutorials
