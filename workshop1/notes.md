# Workshop1: Data ingestion with dlt

## What is data loading, or data ingestion?

Data ingestion is the process of extracting data from a producer, transporting it to a convenient environment, and preparing it for usage by normalizing it, sometimes cleaning, and adding metadata.

## dataset & magic
- A wild dataset magically appears(remember taxi csv files)
    - structured & explicit schema
    - weakly typed & no schema
- Be the magician!

## python generators
- yield one value a time
- next(generator) --> to het next item
- you can use for loops
- we can use list comprehension --> ex: my_nums = (x*x for x in range(1,6)) 
- convert it to list (you lose the adv) --> list(generator)
- more efficient as it only get element by element

