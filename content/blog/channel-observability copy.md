---
title: Channel Observability, Not Guesswork
description: The metrics and alerts we rely on to catch routing failures before users do.
date: 2026-01-30
readingTime: 5
slug: channel-observability
---

## Overview

Lightning nodes fail quietly. The difference between a smooth week and a support spiral is a tiny set of metrics that
surface broken routing, stalled peers, or liquidity drift early.

## What we watch

- Route failure rate, segmented by peer and destination.
- Persistent HTLCs that hint at stuck channels.
- Sudden drops in outbound liquidity on key peers.

## The alert map

We use a simple three-tier alert system: early warning, degraded, and paging. The emphasis is on catching patterns,
not single anomalies, so the alerts stay actionable and the team keeps trust in the signal.

Treat observability as an operating layer, not a dashboard. It makes every other playbook faster.
