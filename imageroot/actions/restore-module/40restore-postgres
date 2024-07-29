#!/bin/bash

#
# Copyright (C) 2022 Nethesis S.r.l.
# SPDX-License-Identifier: GPL-3.0-or-later
#

set -e -o pipefail
exec 1>&2 # Redirect any output to the journal (stderr)

mkdir -vp restore
cat - >restore/joplin_restore.sh <<'EOS'
# Read dump file from standard input:
pg_restore --no-owner --no-privileges -U joplin -d joplin
ec=$?
docker_temp_server_stop
exit $ec
EOS

# Override the image /docker-entrypoint-initdb.d contents, to restore the
# DB dump file. The container will be stopped at the end

podman run \
    --rm \
    --interactive \
    --network=none \
    --volume=./restore:/docker-entrypoint-initdb.d/:Z \
    --volume=postgres-data:/var/lib/postgresql/data:Z \
    --replace --name=restore_db \
    --env-file=database.env \
    --env TZ=UTC \
    "${POSTGRES_IMAGE}" <  joplin.pg_dump

# If the restore is successful, clean up:
rm -rfv restore/  joplin.pg_dump