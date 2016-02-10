# Indian Bank IFSC Codes

Based on [MySQL dump](https://github.com/bhavyanshu/indian-bank-ifsc-branch-database-sql) which, in turn, is based on the unwieldy [excel file from RBI](http://rbidocs.rbi.org.in/rdocs/content/docs/68774.xls). I doubt if I'll be updating this very often, but am happy to accept pull requests. Things that could come in are MICR codes, SWIFT codes.

Dump of PostgreSQL tables with IFSC codes of bank branches.

The SQL dump has two table and one view.
* **banks** - has two columns id and name (e.g. ICICI). The id was generated by me and is not universal.
* **branches** - has details of all branches. IFSC code is the primary key of this table. Has bank_id which is a foreign key reference into banks table.
* **bank_branches** - view on top of these tables that joins on bank_id. Convenient if you want to search by bank name

```
bank=# \d banks
            Table "public.banks"
 Column |         Type          | Modifiers 
--------+-----------------------+-----------
 name   | character varying(49) | 
 id     | bigint                | not null
Indexes:
    "banks_id_pkey" PRIMARY KEY, btree (id)
Referenced by:
    TABLE "branches" CONSTRAINT "branches_banks_fkey" FOREIGN KEY (bank_id) REFERENCES banks(id)

bank=# \d branches
            Table "public.branches"
  Column  |          Type          | Modifiers 
----------+------------------------+-----------
 ifsc     | character varying(11)  | not null
 bank_id  | bigint                 | 
 branch   | character varying(74)  | 
 address  | character varying(195) | 
 city     | character varying(50)  | 
 district | character varying(50)  | 
 state    | character varying(26)  | 
Indexes:
    "branches_ifsc_pkey" PRIMARY KEY, btree (ifsc)
Foreign-key constraints:
    "branches_banks_fkey" FOREIGN KEY (bank_id) REFERENCES banks(id)

bank=# \d bank_branches
          View "public.bank_branches"
  Column   |          Type          | Modifiers 
-----------+------------------------+-----------
 ifsc      | character varying(11)  | 
 bank_id   | bigint                 | 
 branch    | character varying(74)  | 
 address   | character varying(195) | 
 city      | character varying(50)  | 
 district  | character varying(50)  | 
 state     | character varying(26)  | 
 bank_name | character varying(49)  | 
```

# Usage

```
psql <database> < indian_banks.sql
CREATE TABLE
CREATE TABLE
CREATE VIEW
COPY 170
COPY 127857
ALTER TABLE
ALTER TABLE
ALTER TABLE
```