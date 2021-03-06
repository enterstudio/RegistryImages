---

copyright:
  years: 2018
lastupdated: "2018-09-14"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:tip: .tip} 
{:table: .aria-labeledby="caption"}

# Getting started with the `datashield-vault` image
{: #datashield-vault_starter}

This container image runs Vault in the Data Shield environment, providing protection for data in use.
{:shortdesc}

You can use the following Kubernetes pod spec to deploy the image:

```
    apiVersion: v1
    kind: Pod
    metadata:
      name: data-shield-vault
      labels:
        app: data-shield-vault
    spec:
      containers:
      - name: data-shield-vault
        image: <TODO INSERT APPROPRIATE IMAGE NAME HERE>
        volumeMounts:
        - mountPath: /dev/isgx
          name: isgx
        - mountPath: /dev/gsgx
          name: gsgx
        - mountPath: /var/run/aesmd/aesm.socket
          name: aesm-socket
        ports:
        - containerPort: 8200
          hostPort: 8200
          name: vault
          protocol: TCP
      volumes:
      - name: isgx
        hostPath:
          path: /dev/isgx
      - name: gsgx
        hostPath:
          path: /dev/gsgx
      - name: aesm-socket
        hostPath:
          path: /var/run/aesmd/aesm.socket
```
{: codeblock}    
    
Use `kubectl get pod -owide` or `kubectl describe pod` to look up the node IP address for your vault instance. Set the `VAULT_ADDR` environment variable to `http://<IP>:8200` to access the Data Shield-protected vault instance.

You can use vault CLI commands like `vault init`, `vault unseal`, `vault auth`, `vault write`, and `vault read` to authenticate and access secrets. For more information about how to use the Vault CLI, see [Vault Commands (CLI) ![External link icon](../../../icons/launch-glyph.svg "External link icon")](https://www.vaultproject.io/docs/commands/index.html).
