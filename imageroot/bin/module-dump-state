#!/bin/bash

#
# Copyright (C) 2022 Nethesis S.r.l.
# SPDX-License-Identifier: GPL-3.0-or-later
#

set -e

echo "Dumping joplin postgres database"
podman exec joplin-pgsql pg_dump -U joplin --format=c  joplin > joplin.pg_dump