---
layout: post
title: "WhatsApp Admin Panel Takeover"
date: 2025-09-27 00:00:00 +0000
categories: jekyll update
---

The provided content describes a vulnerability found by Mukul Lohar in a WhatsApp subdomain, `https://translate-dev.whatsapp.com`. This subdomain was discovered via `dnsdumpster.com` and redirected from `https://tsl102.whatsapp.net/`.

The vulnerability allowed access to an admin panel using default credentials (`USERNAME: admin`, `PASSWORD: admin`). This gave the researcher the ability to approve translations and, more critically, exposed users' email IDs, including those of WhatsApp founders Brian Acton and Jan Koum.
