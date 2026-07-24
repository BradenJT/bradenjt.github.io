---
layout: note
title: "DDIA Chapter 2 — Storage and Retrieval"
date: 2026-07-23
source: "Designing Data-Intensive Applications — Kleppmann"
---

## Core Idea

Explains storage and retrieval, focusing on hash indexes, LSM-trees, and B-trees

## Log-Structured Storage

- Hash Indexes: Maps keys to byte offsets in an append-only file; fast exact lookups, but keys must fit in memory.
- SSTables & LSM-trees: Keeps keys sorted; merges segment files efficiently in the background; uses Bloom filters to skip missing keys; optimized for high write throughput.

## Page-Oriented Storage

- B-Trees: Splits database into fixed-size blocks or pages (typically 4 KB); updates data in-place; uses a write-ahead log (WAL) for crash recovery; optimized for fast point reads and range queries.

## OLTP vs. OLAP

- OLTP (Online Transaction Processing): User-facing, low-latency, handles small numbers of records per query.
- OLAP (Online Analytical Processing): Data warehousing, scans millions of rows for aggregations.
- Column-Oriented Storage: Stores values of each column together instead of whole rows; improves compression and speeds up analytic queries.

## What I want to follow up on

- Why do LSM-trees offer higher write throughput compared to B-trees?
- Why does column-oriented storage achieve significantly better data compression than row-oriented storage?
- What is a bitmap index, and how does it speed up queries on low-cardinality columns?
