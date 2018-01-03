HTTPS na MikroTiku
====

**1.** Vytvoření certifikátu vlastní certifikační autority (CA certifikát):

```/certificate add name=my-rtr-ca common-name=my-rtr-ca key-usage=key-cert-sign,crl-sign```

**2.** Tento certifikát si podepíšeme:

```/certificate sign my-rtr-ca```

**3.** Vytvoření certifikátu pro užití s HTTPS:

```/certificate add name=my-rtr common-name=my-rtr key-usage=tls-server```

**4.** I tento certifikát podepíšeme, tentokrát CA certifikátem z kroku 1 a 2:

```/certificate sign ca=my-rtr-ca my-rtr```

**Mezikrok:** Nastavení certifikátů jako důvěryhodných (Zřejmě nebude nutné, ale pro pozdější užití se to hodí.):

```/certificate set trusted=yes my-rtr-ca```

```/certificate set trusted=yes my-rtr```


**5.** Přiřazení nového HTTPS certifikátu ke službě zabezpečeného webového rozhraní (serveru):

```/ip service set www-ssl certificate=my-rtr```

### Zdroj
- https://blog.a2o.si/2015/08/11/mikrotik-how-to-generate-ssl-certificate-and-enable-https/
