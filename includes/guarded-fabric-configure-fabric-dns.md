Há várias maneiras de configurar a resolução de nome para o domínio de malha. É uma maneira simples de configurar uma zona de encaminhador condicional no DNS para a malha. Para configurar esta zona, execute os seguintes comandos em um console do Windows PowerShell com privilégios elevados em um servidor DNS de malha. Substitua os nomes e endereços na sintaxe do Windows PowerShell abaixo conforme necessário para o seu ambiente. Adicione servidores mestres para os nós adicionais do HGS.

```
Add-DnsServerConditionalForwarderZone -Name 'bastion.local' -ReplicationScope "Forest" -MasterServers <IP addresses of HGS server>
```

<!-- Appears in guarded-fabric-configuring-fabric-dns-ad.md and guarded-fabric-configuring-fabric-dns.md and set-up-hgs-for-always-encrypted-in-sql-server.md
-->    