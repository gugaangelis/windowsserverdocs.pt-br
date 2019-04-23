Serviço guardião de Host deve ser instalado em uma floresta do Active Directory separada.
Certifique-se de que a máquina HGS está **não** associado a um domínio antes de iniciar e entre como administrador de computador local.

Execute os seguintes comandos para instalar o serviço guardião de Host e configurar seu domínio.
A senha especificada aqui será aplicada somente a senha do modo de reparo de serviços de diretório do Active Directory; ele irá *não* alterar a senha de logon da sua conta de administrador.
Você pode fornecer qualquer nome de domínio de sua preferência para - HgsDomainName.

```powershell
$adminPassword = ConvertTo-SecureString -AsPlainText '<password>' -Force

Install-HgsServer -HgsDomainName 'bastion.local' -SafeModeAdministratorPassword $adminPassword -Restart
```

<!-- Appears in guarded-fabric-install-hgs-default.md and set-up-hgs-for-always-encrypted-in-sql-server.md
-->
