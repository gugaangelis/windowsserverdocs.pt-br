1.  Execute [Install-HgsServer](https://technet.microsoft.com/itpro/powershell/windows/hgsserver/install-hgsserver) para ingressar no domínio e promover o nó a um controlador de domínio.

    ```powershell
    $adSafeModePassword = ConvertTo-SecureString -AsPlainText '<password>' -Force

    $cred = Get-Credential 'relecloud\Administrator'

    Install-HgsServer -HgsDomainName 'bastion.local' -HgsDomainCredential $cred -SafeModeAdministratorPassword $adSafeModePassword -Restart
    ```

2.  Quando o servidor for reiniciado, faça logon com uma conta de administrador de domínio.

<!-- Appears twice in guarded-fabric-configure-additional-hgs-nodes.md 
-->