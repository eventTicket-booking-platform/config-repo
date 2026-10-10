# Event Hub Configuration

Git-backed local/production properties served by Config Server. Each business
service owns its database; database names and localhost ports remain development
defaults. Credential values have no checked-in fallback.

## Required environment

Use [.env.example](.env.example) as a variable-name reference. Populate only the
variables needed by each consuming service, in its deployment environment or
private service-local `.env`. This repository's example is not automatically
loaded by Config Server.

| Profile / consumer | Required credential variables |
|---|---|
| Local Auth | `AUTH_DB_USERNAME`, `AUTH_DB_PASSWORD` |
| Local Event | `EVENT_DB_USERNAME`, `EVENT_DB_PASSWORD` |
| Local Booking | `BOOKING_DB_USERNAME`, `BOOKING_DB_PASSWORD` |
| Production Auth/Event/Booking | `MYSQL_USERNAME`, `MYSQL_PASSWORD`; also supply each service's `MYSQL_HOST`, `MYSQL_PORT`, `MYSQL_DATABASE` |
| Auth/Gateway | `KEYCLOAK_CONFIG_NAME`, `KEYCLOAK_CLIENT_SECRET`, `KEYCLOAK_CONFIG_PASSWORD` |
| Auth/Event | `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY` |
| Auth/Booking | `RABBITMQ_USERNAME`, `RABBITMQ_PASSWORD` |

Use separate database accounts and database names for each service. Match the
Keycloak realm/client, JWT issuer, broker permissions and storage settings to
your environment. A checked-in public verification key is not a private signing
key; ports, realm/client IDs and bucket names are configuration identifiers,
not credentials.

## Previously exposed credentials

**Previously committed credentials must be rotated when infrastructure is restored.**
Keycloak client secrets and bootstrap-account credentials previously used as
local defaults must be replaced. Review any database, RabbitMQ and AWS credentials
ever committed or reused from examples; revoke/rotate genuine deployed values
and update private environment/CI secret stores. Account identifiers are removed
from defaults but are not themselves rotatable passwords.

Deleting values from HEAD does not remove Git-history exposure. This cleanup
does not rewrite history or rotate external accounts. Keycloak, SonarQube EC2
and GKE restoration/credential changes require a separate operational step.
