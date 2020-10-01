HTTPS na MikroTiku
====

**1.** Vytvoření certifikátu vlastní certifikační autority (CA certifikát, platnost 10 let):

```/certificate add name=my-rtr-ca common-name=my-rtr-ca key-usage=key-cert-sign,crl-sign key-size=2048 days-valid=3650```

**2.** Tento certifikát si podepíšeme:

```/certificate sign my-rtr-ca```

**3.** Vytvoření certifikátu pro užití s HTTPS (platnost 10 let):

```/certificate add name=my-rtr common-name=my-rtr key-usage=tls-server key-size=2048 days-valid=3650```

**4.** I tento certifikát podepíšeme, tentokrát CA certifikátem z kroku 1 a 2:

```/certificate sign ca=my-rtr-ca my-rtr```

**Mezikrok:** Nastavení certifikátů jako důvěryhodných (Zřejmě nebude nutné, ale pro pozdější užití se to hodí.):

```/certificate set trusted=yes my-rtr-ca```

```/certificate set trusted=yes my-rtr```


**5.** Přiřazení nového HTTPS certifikátu ke službě zabezpečeného webového rozhraní (serveru):

```/ip service set www-ssl certificate=my-rtr```

**6.** Vypíšeme dostupné IP služby a jejich status:

```/ip service print```

```
Flags: X - disabled, I - invalid
 #   NAME                                                 PORT ADDRESS                                                                                   CERTIFICATE
 ...
 4 XI www-ssl                                               443                                                                                           my-rtr
...
```

**6.** Jak je vidět výše, služba ```www-ssl``` není zatím spuštěna, proto ji nyní spustíme příkazem:

```/ip service enable 4```

Výsledek:
```
 4   www-ssl                                               443                                                                                           my-rtr

```

### Zdroj
- https://blog.a2o.si/2015/08/11/mikrotik-how-to-generate-ssl-certificate-and-enable-https/
