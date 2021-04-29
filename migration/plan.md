# Plan

Requirements

* Each tenant as own domain?
* Share DB?
* How should end-user registration work?
* CICD & Canary environment?

Accounts

* github account and repo
* AWS account
* Sentry
* Amplitude

AWS

* VPC
* Public hosted zone
  * local.navlab.com
* Private hosted zone
* RDS
* ECS for RPC API
* ECS for Auth API
* ECS for Admin panel APIs
* S3 for logos
* S3 for frontend assets
* API gateway + lambda for email

DB

* Replace dynamic sequelize models with hardcoded models in code
* Remove tsv column for some tables
* Create table to store tenant metadata including name and logo
* Create tables for roles and policies

RPC API

* Remove logic that accesses mongo

Auth API

* Rebuild to work with Postgres instead of mongo

Admin Panel

* CRUD tenants
* CRUD users
* CRUD roles and policies

Navlab Frontend

* Create a new create-react-app project
* Replace dynamic navigator with a static one with hardcoded routes
* Move code for coded views to git repo and integrate with webpack
* Data layer
* Master data screens

