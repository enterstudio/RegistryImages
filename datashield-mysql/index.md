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

# Getting started with the `datashield-mysql` image
{: #datashield-mysql_starter}

This container image runs MariaDB in the Data Shield environment, providing protection for data in use.
{:shortdesc}

You can use the following Kubernetes pod spec to deploy the image:

```
    apiVersion: v1
    kind: Pod
    metadata:
      name: data-shield-mariadb
      labels:
        app: data-shield-mariadb
    spec:
      containers:
      - name: data-shield-mariadb
        image: <TODO INSERT APPROPRIATE IMAGE NAME HERE>
        volumeMounts:
        - mountPath: /dev/isgx
          name: isgx
        - mountPath: /dev/gsgx
          name: gsgx
        - mountPath: /var/run/aesmd/aesm.socket
          name: aesm-socket
        ports:
        - containerPort: 3306
          hostPort: 3306
          name: mysql
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
    
You can connect to the MariaDB instance by running the following command:

```
    mysql -h <node> --protocol tcp -uroot
```
{: pre}

The Data Shield MariaDB image requires that client connections use TLS. Depending on your client version, you might need to add `--ssl` or `--ssl-mode require` to the client command line.
