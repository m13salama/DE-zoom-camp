# Analytics Engineering

## what is analytics engineer?
- fill the gap between data engineering nad data analysis
- data loading
- data storing --> bigquery
- data modeling -->dbt/ dataform
- data presentation --> tableau

## ETL vs ELT

## Kimball's dimensional modeling
- we want to deliver understandable data to all business user
- we want fast queries too
- priority user understandability over non redundancy (3NF)
- fact tables --> verbs
- dimensional tables --> nouns
- kitchen analogy --> stage, processing, presentation areas

## what is dbt
db t is a transformation workflow that allows anyone that knows SQL to deploy analytics code following software engineering best practices like modularity, portability, CI/CD, and documentation.

## Modular data modeling
- models are just sql files
- models
    - ephemeral
    - view
    - table
    - incremental
- define sources in yml file