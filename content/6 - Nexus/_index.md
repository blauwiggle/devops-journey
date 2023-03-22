---
pre: "<b>6. </b>"
chapter: true
title: "Artifact Repository Manager with Nexus"
date: 2023-03-22T06:48:57+01:00
weight: 6
---

The information on Nexus and, more generally, an Artifact Repository Manager was fantastic, as I needed to step in 
and help with a Proof of Concept (PoC) project on short notice, tasked with building the entire streamlined pipeline 
process. First, I had to set up an EC2 instance where Nexus could be installed. Subsequently, the project required 
various additional components, such as a GitLab Runner, a Caddy web server for obtaining certificates, and so on.

I also took the time to watch your GitLab CI/CD course and built the pipeline for the monorepo accordingly. In short,
the project consists of a Spring Boot backend, an Angular frontend, and a Java Maven importer responsible for 
loading data into the backend. The backend generates various artifacts, which are then uploaded to Nexus. The 
importer requires snapshots from Nexus, while the frontend, in this case, operated independently of the rest and 
didn't need Nexus. Nevertheless, it was great that these specific exercises were covered.


