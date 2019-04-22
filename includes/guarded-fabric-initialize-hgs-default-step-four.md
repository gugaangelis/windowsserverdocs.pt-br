Se você tiver fornecido todos os certificados para HGS usando as impressões digitais, você será instruído a conceder acesso de leitura HGS para a chave privada desses certificados. Em um servidor com experiência Desktop instalado, conclua as seguintes etapas:

1.  Abra o Gerenciador de certificados do computador local (**certlm**)
2.  Localize o certificado (s) > clique com botão direito > todas as tarefas > Gerenciar chaves privadas
3.  Clique em **Adicionar**.
4.  Na janela do seletor de objeto, clique em **tipos de objeto** e habilite **contas de serviço**
5.  Insira o nome da conta de serviço mencionado no texto de aviso `Initialize-HgsServer`
6.  Verifique se que a gMSA tem acesso de "Leitura" à chave privada.

No server core, você precisará fazer o download de um módulo do PowerShell para ajudar a definir as permissões de chave privadas.

1.  Execute `Install-Module GuardedFabricTools` no servidor HGS se ele tiver conectividade com a Internet, ou execute `Save-Module GuardedFabricTools` em outro computador e copiar o módulo de failover para o servidor HGS.
2.  Execute `Import-Module GuardedFabricTools`. Isso adicionará as propriedades adicionais para objetos de certificado encontrados no PowerShell.
3.  Localizar sua impressão digital do certificado no PowerShell com o `Get-ChildItem Cert:\LocalMachine\My`
4.  Atualizar a ACL, substituindo a impressão digital com seu próprio e a conta gMSA no código abaixo com a conta listada no texto de aviso de `Initialize-HgsServer`.

    ```powershell
    $certificate = Get-Item "Cert:\LocalMachine\1A2B3C..."
    $certificate.Acl = $certificate.Acl | Add-AccessRule "HgsSvc_1A2B3C" Read Allow
    ```

Se você estiver usando certificados de backup de HSM ou certificados armazenados em um provedor de armazenamento de chaves de terceiros, essas etapas podem não se aplicar a você. Consulte a documentação do seu provedor de armazenamento de chaves para aprender a gerenciar permissões em sua chave privada. Em alguns casos, não há nenhuma autorização ou autorização é fornecida para todo o computador quando o certificado está instalado.

<!-- Appears in guarded-fabric-initialize-hgs-ad-mode-default.md and guarded-fabric-initialize-hgs-tpm-mode-default.md
-->