

From a Consul server, list the members and find leader

```

consul members \
    -http-addr=https://127.0.0.1:8443 \
    -ca-path=/var/run/secrets/consul/certs/ca.pem \
    -client-cert=/var/run/secrets/consul/certs/consul.pem \
    -client-key=/var/run/secrets/consul/certs/consul-key.pem
    
curl -k https://consul.vault.svc:8443/v1/status/leader \
    --key /var/run/secrets/consul/certs/consul-key.pem \
    --cert /var/run/secrets/consul/certs/consul.pem
    
```

From a the Vault Consul client, list the members

```

consul members \
    -http-addr=https://127.0.0.1:8443 \
    -ca-path=/var/run/secrets/consul/certs/ca.pem \
    -client-cert=/var/run/secrets/consul/certs/consul-client.pem \
    -client-key=/var/run/secrets/consul/certs/consul-client-key.pem

```
