# MongoDB Export Guide

This guide explains how to export MongoDB collections using the `mongoexport` command.

## Command Syntax

```bash
mongoexport --host <host> --port <port> --username <user> --password <password> --authenticationDatabase <auth-db> --db <database> --collection <collection> --out <output-file.json>


# Import MongoDB Collection

## 1. Copy the Exported File to the Destination Server (if needed)
Use `scp` or any other file transfer tool to transfer the file to the destination server. For example:

```bash
scp users.json username@destination-server:/path/to/destination

# Use the mongoimport Command

```bash
mongoimport --host localhost --port 27017 --username admin --password admin123 --authenticationDatabase admin --db mydb --collection users --file users.json --jsonArray

## Important Options:

```bash
--jsonArray: Use this if the input file contains multiple JSON documents (array format).
