# Internal Ingress の TLS 対応

### 証明書の作成

- GCPのマネージド証明書は Internal Ingress には対応していない
    - セルフマネージド証明書を使う
    - kubernetes secret を使用する手もあるが、どちらにしても証明書は自前で用意する必要がある
- 以下を参考に証明書を作成
    - [https://cloud.google.com/load-balancing/docs/ssl-certificates/self-managed-certs](https://cloud.google.com/load-balancing/docs/ssl-certificates/self-managed-certs)

```bash
cat <<'EOF' >cert-config
[req]
default_bits              = 2048
req_extensions            = extension_requirements
distinguished_name        = dn_requirements

[extension_requirements]
basicConstraints          = CA:FALSE
keyUsage                  = nonRepudiation, digitalSignature, keyEncipherment
subjectAltName            = @sans_list

[dn_requirements]
countryName               = Country Name (2 letter code)
stateOrProvinceName       = State or Province Name (full name)
localityName              = Locality Name (eg, city)
0.organizationName        = Organization Name (eg, company)
organizationalUnitName    = Organizational Unit Name (eg, section)
commonName                = Common Name (e.g. server FQDN or YOUR name)
emailAddress              = Email Address

[sans_list]
DNS.1                     = *.sample

EOF
```

### req の設定

```txt
countryName               = JP
stateOrProvinceName       = Tokyo
localityName              = Shinjuku-ku
0.organizationName        = 
organizationalUnitName    = 
commonName                = 
emailAddress              = test@sample.com
```

### Ingress の設定

- annotation に必要値を設定

```
    ingress.gcp.kubernetes.io/pre-shared-cert: {CERT_NAME}
    kubernetes.io/ingress.class: "gce-internal"
    kubernetes.io/ingress.allow-http: "false"
```

### 参考

[https://cloud.google.com/kubernetes-engine/docs/how-to/internal-load-balancing](https://cloud.google.com/kubernetes-engine/docs/how-to/internal-load-balancing)

[https://cloud.google.com/kubernetes-engine/docs/how-to/internal-load-balance-ingress#https_between_client_and_load_balancer](https://cloud.google.com/kubernetes-engine/docs/how-to/internal-load-balance-ingress#https_between_client_and_load_balancer)

[https://cloud.google.com/load-balancing/docs/ssl-certificates/self-managed-certs](https://cloud.google.com/load-balancing/docs/ssl-certificates/self-managed-certs)

[https://www.slogical.co.jp/ssl/faq/csr_openssl/](https://www.slogical.co.jp/ssl/faq/csr_openssl/)
