---
layout: note
title: "DDIA Chapter 2 — Data Models and Query"
date: 2026-07-23
source: "Designing Data-Intensive Applications — Kleppmann"
---

## Core Idea

Compares core data models like relational, document, and graph structures. It explains that no single model fits every use case, contrasting declarative query languages like SQL with imperative styles.

## Relational Model

Organizes data in tables (rows and columns); great for joins and many-to-many relationships, but suffers from impedance mismatch with object code.

## Document Model

Stores nested data as trees (JSON); offers schema flexibility and better performance via data locality for one-to-many links, but weak join support.

## Schema Handling

Relational uses schema-on-write (rigid/safe), while document uses schema-on-read (dynamic/flexible).

## What I want to follow up on

- Under what specific workloads does the impedance mismatch between object-oriented code and relational databases justify switching to a document model?
- If an application starts with a strict one-to-many data structure but evolves to require complex many-to-many relationships, how painful is the evolution path for both a document database and a relational database?
