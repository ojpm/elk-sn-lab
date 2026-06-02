# ELK - Servicenow integration lab

Oliver Pope-Mostowicz, Q2-2026

## Overview

This document captures the implementation documentation for building a constrained ServiceNow ↔ ELK integration, to explore and validate several questions related to the integration implementation.

The primary objective is to validate an operational enrichment integration pattern while maintaining strict separation of concerns between:
ServiceNow as the authoritative ITSM/CMDB platform,
ELK as the observability and analytics platform.

The lab is intentionally:

- small
- reproducible
- controlled
- evidence-driven

## Repo structure

elk-sn-lab/
    /docs
        /journal/ - write up from implementation activities
        /reference/ - SN KB article on use of ITIL vs more granular roles: seems less relevant now, through use of the Table API, rather than CMDB
        /architecture-principles.md - notes on what architectural thoughts/constraints I had going into this activity
    /docker/cleanbuild/ - refined set of build files based on Elastic blog, should be everything required for local installation
    /screenshots/
        /elastic-agent-basic-auth/
        /postman/
        /servicenow-elastic-agent-int/
        /docker-resources.png
        /PopOS_details
        /SN-elastic-int-user-config-with-ITIL

## Pre-reqs

- Docker Desktop (recommend 4+GB RAM, 5+ CPU)
- Git
- VS Code
- Postman
- curl
- Servicenow PDI (can be requested from <developer.servicenow.com>, or request me to create you a user)

## Deploy Local ELK Stack

from the docker/cleanbuild path, first create your local secrets file (it is git-ignored):
cp .env.example .env   # then set ELASTIC_PASSWORD, KIBANA_PASSWORD, ELASTIC_APM_SECRET_TOKEN, ENCRYPTION_KEY

then execute (in order)
docker compose up -d es01 (brings up elasticsearch)
docker compose up -d kibana
docker compose up -d fleet-server

Each command will also make use of the setup container, which was defined in the original Elastic demo repo <https://github.com/elkninja/elastic-stack-docker-part-two>.

log into kibana - you'll need to switch to https (clicking from docker desktop will default to http), use the credentials from .env

Follow the instructions from halfway down <https://www.elastic.co/blog/getting-started-with-the-elastic-stack-and-docker-compose-part-2>: "Reconfigure output, add certificate" to ensure that the elastic agent on the fleet-server container is able to communicate with elasticsearch.

Restart all containers (key step)
docker compose down
docker compose up ...

## Set up Servicenow

- Create a dedicated integration account (see screenshot/SN-elastic-user)
- add cmdb_read to role assigned to user (see screenshot/int-user-privilege)
- create OAuth client credentials (see screenshot/SN-Oauth)

## Install ELK integration

add integration is available from the flyout menu
search for Servicenow, and choose the Servicenow log option (see screenshot/servicenow-elastic-agent-int/)
configure the integration with the OAuth details created in Servicenow, targeting the cmdb_ci table
repeat for the cmdb_rel_ci table

Restart all containers (key step)
docker compose down
docker compose up ...

## validate functionality

review the fleet-server logs (search on "servicenow" and make sure there are no errors in log entries immediately after API calls)
check in the Servicenow transaction logs (All > Transaction), filtering on the created by for your integration user
view the Servicenow dashboards automatically available in Kibana
