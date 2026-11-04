# SQL Injection: Retrieving Hidden Data

## Environment

PortSwigger Web Security Academy — intentionally vulnerable training lab.

Lab: https://portswigger.net/web-security/sql-injection/lab-retrieve-hidden-data

## Summary

The product category parameter allowed modification of the SQL query,
making unpublished products visible.

## Steps to reproduce

1. Launch the lab and select a product category.
2. Replace the category value with `Accessories' OR 1=1-- `.
3. Submit the request.
4. Observe additional products and the lab's Solved status.

## Expected behavior

The category parameter should be treated as data.
Only published products in the selected category should be returned.

## Observed behavior

The injected condition returned products across categories,
including unpublished products.

## Explanation

The apostrophe closes the category string.
`OR 1=1` introduces an always-true condition.
The SQL comment removes the remaining publication check.

As a comparison, `Accessories' AND 1=2-- ` returned no products:
a false condition is valid SQL and does not itself cause a syntax error.

## Impact

A visitor could access product information that was not intended
to be publicly available.

## Recommended fix

Use parameterized queries to keep user input separate from SQL code.
Keep publication and access restrictions enforced on the server.

## What I learned

User input must be handled as data rather than executable SQL.
An empty query result is different from a database error.
