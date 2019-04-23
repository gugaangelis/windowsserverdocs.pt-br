Primeiro, obtenha um certificado SSL para HGS da autoridade de certificação. Cada computador host precisará confiar no certificado SSL, portanto, é recomendável que você emita o certificado SSL de sua empresa público chave infraestrutura ou terceiros da autoridade de certificação. Qualquer certificado SSL com suporte pelo IIS é compatível com o HGS, no entanto **o nome da entidade do certificado deve corresponder ao nome totalmente qualificado do serviço HGS** (nome de rede distribuída de cluster). Por exemplo, se o domínio HGS é "bastion.local" e o nome do seu serviço HGS é "hgs", seu certificado SSL deve ser emitido para "hgs.bastion.local". Você pode adicionar nomes DNS adicionais para o campo de nome alternativo do assunto do certificado se necessário.

Depois que o SSL de certificado, abra uma sessão do PowerShell elevada e fornecer o caminho do certificado quando você executa [HgsServer conjunto](https://technet.microsoft.com/itpro/powershell/windows/host-guardian-service/server/set-hgsserver):


```powershell
$sslPassword = Read-Host -AsSecureString -Prompt "SSL Certificate Password"
Set-HgsServer -Http -Https -HttpsCertificatePath 'C:\temp\HgsSSLCertificate.pfx' -HttpsCertificatePassword $sslPassword
```

Ou, se você já tiver instalado o certificado no repositório de certificados local, você pode referenciá-lo por impressão digital:

```powershell
Set-HgsServer -Http -Https -HttpsCertificateThumbprint 'A1B2C3D4E5F6...'
```

> [!IMPORTANT]
> Configuração de HGS com um certificado SSL não desabilita o ponto de extremidade HTTP.
> Se você quiser permitir apenas o uso do ponto de extremidade HTTPS, configure o Firewall do Windows para bloquear conexões de entrada para a porta 80.
> **Não modifique as associações de IIS** para sites HGS para remover o ponto de extremidade HTTP; não há suporte para isso.
