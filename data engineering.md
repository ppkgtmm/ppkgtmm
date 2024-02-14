# 🗺️ Pinky's Learning

Welcome to my learning documentation ! Here, I record a summary of what I learnt in data field

## 📚 Table of Contents

- [Data Engineering](#data-engineering)

## 🎀 Data Engineering
💎 **data** is differentiated based on its arrangement and format into 3 categories namely structured, semi-structured and unstructured
| category | description | when to use |
| ---      | ---         | ---         |
| structured data | data is organized into tables with rows and columns. each row is a record with fixed set of columns. each column is an attribute or field with fixed data type | data is frequently queried and analyzed or data integrity is non-negotiable |
| semi-structured data | data is organized in specific way e.g. key-value pair, document, graph etc. records can have different set of fields and data type of fields can vary between records | record structure requires flexibility and storage overhead is a concern |
| unstructured data | data is stored in its native format and requires organization. each data item can have different set of information and information format can vary between data items | data format is unpredictable or storage and processing power is affordable |

additional notes : data integrity ensures accuracy, completeness, consistency and validity of data

💎 **database** is collection of data managed with help of software called database management system or DBMS
| category | description | when to use |
| --- | --- | --- |
| relational database | holds structured data in tabular model. uses SQL as standard query language. scales vertically by adding more resources to a single server. include rules or constraints to ensure data integrity. inherently supports relationship between records within or across tables | data integrity is non-negotiable or data consistency is not deferrable |
| non-relational database | holds semi-structured data in variety of models. query language depends on database type. scales horizontally by distributing data across multiple servers | data format is unpredictable or availability is a concern |

💎 **ACID** contains 4 key properties that ensure data integrity and reliability in relational databases especially when dealing with concurrent access and modifications
| property | description | benefit |
| --- | --- | --- |
| atomicity | transaction is treated as a single unit. either all operations within a transaction succeed or none do | prevents partial completion and data inconsistency |
| consistency | data integrity is maintained based on predefined rules or constraints. all transactions must adhere to defined constraints | prevents data corruption and unexpected changes |
| isolation | concurrent transactions appear to execute one at a time. changes made by one transaction are invisible to others until committed | prevents interference and inconsistent data views |
| durability | changes from committed transactions are permanent and survive system failures. when transaction is committed, its result is written to persistent storage e.g. disk | guarantees data consistency even after outages |
