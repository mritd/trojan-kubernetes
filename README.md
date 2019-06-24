# trojan-kubernetes

Kubernetes configuration templates for deploying trojan to a Kubernetes cluster.

## Deployment Guide

1. Configure your `kubectl` to use a valid cluster (See [this](https://cloud.google.com/kubernetes-engine/docs/quickstart) for GKE and [this](https://docs.aws.amazon.com/eks/latest/userguide/getting-started.html) for AWS).
1. Clone this repo and enter it.
    ```bash
    git clone https://github.com/trojan-gfw/trojan-kubernetes.git
    cd trojan-kubernetes/
    ```
1. Change the password setting in the server config file using your favorite editor. **Don't touch anything else!**
    ```bash
    vim trojan/trojan-server-config.yaml
    ```
1. Paste your certificate and private key to the secret file.
    ```bash
    vim trojan/trojan-tls-cert.yaml
    ```
    Example (**Don't copy it! It's unsafe!**):
    ```yaml
    apiVersion: v1
    kind: Secret
    metadata:
    name: trojan-tls-cert
    namespace: trojan
    labels:
        environment: production
        app: trojan
    type: kubernetes.io/tls
    stringData:
    tls.crt: |
        -----BEGIN CERTIFICATE-----
        MIIB4TCCAYugAwIBAgIUMiKFserc3CwkDePOUdrqMMFYJPMwDQYJKoZIhvcNAQEL
        BQAwRTELMAkGA1UEBhMCQVUxEzARBgNVBAgMClNvbWUtU3RhdGUxITAfBgNVBAoM
        GEludGVybmV0IFdpZGdpdHMgUHR5IEx0ZDAeFw0xOTA2MjQwNjAzMjhaFw0xOTA3
        MjQwNjAzMjhaMEUxCzAJBgNVBAYTAkFVMRMwEQYDVQQIDApTb21lLVN0YXRlMSEw
        HwYDVQQKDBhJbnRlcm5ldCBXaWRnaXRzIFB0eSBMdGQwXDANBgkqhkiG9w0BAQEF
        AANLADBIAkEAzrr0huefOGHyfhwv4n0RZBxmcWZVB8ZoslBqQwU7zvZknrzQgnwl
        gzW7Jd+sGjhU2FqzPJF2GfwCfhMfoYVDoQIDAQABo1MwUTAdBgNVHQ4EFgQUNbGS
        H1y/JOIooJVCOLG9O6ChWzwwHwYDVR0jBBgwFoAUNbGSH1y/JOIooJVCOLG9O6Ch
        WzwwDwYDVR0TAQH/BAUwAwEB/zANBgkqhkiG9w0BAQsFAANBABlks2duOyNq6mCN
        vxCxE75fr7CtQJMgBG3rsi9oZd4C/OcW8xTdd8N0WNcPM3ptcUDWz4ulUpjLwwOK
        4hCksy4=
        -----END CERTIFICATE-----
    tls.key: |
        -----BEGIN PRIVATE KEY-----
        MIIBVgIBADANBgkqhkiG9w0BAQEFAASCAUAwggE8AgEAAkEAzrr0huefOGHyfhwv
        4n0RZBxmcWZVB8ZoslBqQwU7zvZknrzQgnwlgzW7Jd+sGjhU2FqzPJF2GfwCfhMf
        oYVDoQIDAQABAkEAhTRJozNTkIzsJv4ajKFxt0PlbmQ1ndDmXR8bmRuiMfPA0B75
        xigk+ZOIv8mwSz18OEy8wlbMM9f5+ZrZBk3TEQIhAOs7DliYouCHG454nPKC+3vu
        xAcESL9Yslz3VLh+UNIjAiEA4PuzpXJijNM2/iu0vqc7QAix01bwF2vIlKnaDi8e
        RWsCIQC+lz7scd+mZFHjgb5Ij/ALXk3eEY6P2uHJiWxPf6kkWQIhAJVTOlczZmml
        vrhQdfSctly36J8m8s/4v/a8DXigmWzlAiA7ShO5gUORKIZt13wJHcCDcK2iFCHZ
        OjaefV6JHU8zbw==
        -----END PRIVATE KEY-----
    ```
1. Create the `trojan` namespace in your cluster.
    ```bash
    kubectl create namespace trojan
    ```
1. Apply all the config files to the cluster (**notice the dot**).
    ```bash
    kubectl apply --recursive -f .
    ```
1. Wait for everything to be ready, and check the status of the pods and the services.
    ```bash
    kubectl -n trojan get pods
    kubectl -n trojan get services
    ```
