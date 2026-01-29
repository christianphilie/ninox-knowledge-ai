# Context Queries - When to Ask?

## Overview

This document describes when you should ask questions in Ninox requests to avoid errors and deliver better results.

**Principle**: If something is unclear or likely to be wrong, ask before generating code.

---

## CRITICAL SITUATIONS - ALWAYS ASK

### 1. Field Names Are Unclear or Likely Wrong

#### Situations:
- User mentions field names that are not unambiguous
- Field names could have various variants (e.g., `Pos` vs `Pos Nr`, `Einheit` vs `Mengeneinheit`)
- Field names contain special characters or spaces (e.g., `'Billing Date From'`)

#### Example:
```
❌ DON'T DO: Make assumption that field is called "Pos"
✅ CORRECT: "What is the field for the position number exactly? Is it 'Pos', 'Pos Nr' or something else?"
```

#### Common Field Name Variants:
- Position numbers: `Pos`, `Pos Nr`, `Position`, `PosNr`
- Units: `Einheit`, `Mengeneinheit`, `Mengeneinheit_alt`, `Unit`
- Date fields: `Datum`, `Erstellungsdatum`, `'Invoice Date'`, `'Billing Date From'`
- Relations: `Kunde`, `Customer`, `'Article / Service'`

**Rule**: If a field name is not unambiguous or various variants are possible, ALWAYS ask.

---

### 2. Table Names Are Unclear

#### Situations:
- User mentions table names that are not unambiguous
- Table names could have various variants
- Table names contain special characters or spaces

#### Example:
```
❌ DON'T DO: Make assumption that table is called "Invoices"
✅ CORRECT: "What is the table name exactly? Is it 'Invoices', 'Invoice' or 'Rechnungen'?"
```

---

### 3. Relations Are Unclear

#### Situations:
- User mentions relations, but context is unclear
- It is not clear whether it's a relation or a table
- The name of the relation is not unambiguous

#### Example:
```
❌ DON'T DO: Make assumption that 'Invoice Positions' is a relation
✅ CORRECT: "Is 'Invoice Positions' a relation in the 'Invoices' table or a separate table?"
```

---

### 4. Syntax or Functionality Is Unclear

#### Situations:
- User describes functionality that is not unambiguous
- There are multiple possible implementation approaches
- The requirement could be interpreted in various ways

#### Example:
```
❌ DON'T DO: Make assumption about how something should work
✅ CORRECT: "Should the copy take over all fields or only specific ones? Should positions be automatically sorted?"
```

---

### 5. Database Structure Is Unclear

#### Situations:
- Relationships between tables are not clear
- Field types are not known
- It is unclear which fields exist

#### Example:
```
❌ DON'T DO: Make assumption about database structure
✅ CORRECT: "What fields does the 'Invoices' table have? Is there a field 'Delivery Date'?"
```

---

## IMPORTANT QUESTIONS TO ASK

### For Field Names:
- "What is the field name exactly? [List possible variants]"
- "Does the field name contain spaces or special characters?"
- "Is the field a relation or a normal field?"

### For Table Names:
- "What is the table name exactly?"
- "Is this a table or a relation?"

### For Relations:
- "Is '[Name]' a relation or a separate table?"
- "Which table does this relation belong to?"

### For Functionality:
- "Should [function] affect all records or only specific ones?"
- "How should [behavior] work exactly?"
- "Are there special requirements I should consider?"

---

## WHEN NOT TO ASK

### Don't ask if:
- The information is already clear from the request
- It's standard Ninox syntax
- The request already contains all necessary information
- You are sure your assumption is correct

**But**: When in doubt, better ask than guess wrong!

---

## CHECKLIST BEFORE CODE GENERATION

Before generating code check:

- [ ] Are all field names unambiguous?
- [ ] Are all table names unambiguous?
- [ ] Is it clear which relations are used?
- [ ] Is the functionality completely described?
- [ ] Is the database structure clear?
- [ ] Are there special requirements that were not mentioned?

**If a question is answered with "No"**: Ask!

---

## EXAMPLE INTERACTION

### Bad Interaction:
```
User: "Create a script that copies invoices"
AI: [Generates code with assumptions about field names]
User: "That doesn't work, the field names are wrong"
```

### Good Interaction:
```
User: "Create a script that copies invoices"
AI: "Before I create the script, I need some information:
- What is the field for the position number exactly? (Pos, Pos Nr, Position?)
- What is the field for the unit? (Einheit, Mengeneinheit?)
- Which fields should be copied? All or only specific ones?"
User: [Answers with correct information]
AI: [Generates correct code]
```

---

## BEST PRACTICES

1. **Ask proactively**: Better too many questions than wrong assumptions
2. **Ask concrete questions**: Not "Is this correct?", but "What is the field name exactly?"
3. **List possible variants**: Show that you know various possibilities
4. **Ask when uncertain**: If you're not sure, ask
5. **Document assumptions**: If you make assumptions, mention them in the code

---

## SUMMARY

**Golden Rule**: If something is unclear or likely to be wrong, ask before generating code.

**Priorities**:
1. Field names - ALWAYS ask if not unambiguous
2. Table names - Ask if not unambiguous
3. Relations - Ask if context unclear
4. Functionality - Ask if ambiguous
5. Database structure - Ask if not clear

---

**Sources**:
- Common Mistakes: `references/common-mistakes.md`
- Relations and Loops: `references/relations-and-loops.md`
