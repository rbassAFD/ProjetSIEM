# Voir les roles disponibles sur OpenSearch
curl -k -u admin:SecretPassword https://localhost:9200/_opendistro/_security/api/roles

# Créer un rôle pour Graylog
curl -k -u admin:SecretPassword -X PUT "https://localhost:9200/_opendistro/_security/api/roles/graylog" -H "Content-Type: application/json" -d '{
  "cluster_permissions": [
    "cluster_monitor",
    "cluster_composite_ops",
    "indices_monitor",
    "indices:data/write/bulk",
    "indices:data/read/search"
  ],
  "index_permissions": [
    {
      "index_patterns": ["graylog_*"],
      "allowed_actions": [
        "read",
        "write",
        "delete",
        "manage"
      ]
    }
  ],
  "tenant_permissions": []
}'

# Map Admin to Graylog
curl -k -u admin:SecretPassword -X PUT "https://localhost:9200/_opendistro/_security/api/rolesmapping/graylog" -H "Content-Type: application/json" -d '{
  "backend_roles": ["admin"],
  "hosts": [],
  "users": ["admin"],
  "and_backend_roles": []
}'

# Redémarrer le service