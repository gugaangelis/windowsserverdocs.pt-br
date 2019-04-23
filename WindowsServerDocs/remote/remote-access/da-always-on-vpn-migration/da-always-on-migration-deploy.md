---
title: Migração do DirectAccess para VPN Always On
description: Migrando do DirectAccess para sempre VPN requer um processo específico, a migração de clientes, que ajuda a minimizar as condições de corrida que podem surgir de executar as etapas de migração fora de ordem.
manager: dougkim
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: eeca4cf7-90f0-485d-843c-76c5885c54b0
ms.author: pashort
author: shortpatti
ms.date: 06/07/2018
ms.openlocfilehash: 94852b33936ece0b329614f428e845f2c7a2ac6a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854577"
---
# <a name="migrate-to-always-on-vpn-and-decommission-directaccess"></a>Migre para VPN Always On e encerrar o DirectAccess

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows 10

&#171;[ **Anterior:** Planejar o DirectAccess para a migração de VPN Always On](da-always-on-migration-planning.md)<br>

Migrando do DirectAccess para sempre VPN requer um processo específico, a migração de clientes, que ajuda a minimizar as condições de corrida que podem surgir de executar as etapas de migração fora de ordem. Em um alto nível, o processo de migração consiste nessas quatro etapas principais:

1.  **Implante uma infraestrutura VPN de lado a lado.** Depois de ter determinado suas fases de migração e os recursos que você deseja incluir em sua implantação, você implantará a infraestrutura VPN lado a lado com a infraestrutura existente do DirectAccess.  

2.  **Implante certificados e a configuração para os clientes.**  Depois que a infraestrutura VPN estiver pronta, criar e publicar os certificados necessários para o cliente. Quando os clientes receberam os certificados, você pode implantar o script de configuração VPN_Profile.ps1. Como alternativa, você pode usar o Intune para configurar o cliente VPN. Use o Microsoft System Center Configuration Manager ou o Microsoft Intune para monitorar implantações bem-sucedidas de configuração de VPN.

3.  [!INCLUDE [remove-da-security-group-shortdesc-include](../includes/remove-da-security-group-shortdesc-include.md)]

4.  [!INCLUDE [decommission-da-shortdesc-include](../includes/decommission-da-shortdesc-include.md)]

Antes de iniciar o processo de migração do DirectAccess para VPN Always On, certifique-se de que você tenha se planejado para a migração.  Se você não planejar a migração, consulte [plano do DirectAccess para a migração de VPN Always On](da-always-on-migration-planning.md).

>[!TIP] 
>Esta seção não é um guia de implantação passo a passo para VPN Always On, mas em vez disso, é destinada para complementar [Always On VPN implantação para o Windows Server e Windows 10](../vpn/always-on-vpn/deploy/always-on-vpn-deploy.md) e fornecer orientações de implantação específicas de migração.

## <a name="deploy-a-side-by-side-vpn-infrastructure"></a>Implantar uma infraestrutura VPN de lado a lado

Implantar a infraestrutura VPN lado a lado com a infraestrutura existente do DirectAccess.  Para obter detalhes passo a passo, consulte [Always On VPN implantação para o Windows Server e Windows 10](../vpn/always-on-vpn/deploy/always-on-vpn-deploy.md) para instalar e configurar a infraestrutura de VPN Always On. 

Implantação lado a lado consiste as seguintes tarefas de alto nível:

1.  Crie os grupos de usuários de VPN, servidores VPN e servidores NPS.

2.  Criar e publicar os modelos de certificado necessárias.

3.  Registre certificados de servidor.

4.  Instalar e configurar o serviço de acesso remoto para VPN Always On.

5.  Instalar e configurar o NPS.

6.  Configure o DNS e regras de firewall para VPN Always On.

A imagem a seguir fornece uma referência visual para que as alterações de infraestrutura em todo o DirectAccess-a – Always On migração de VPN.

![](../../media/DA-to-AlwaysOnVPN/6b64f322f945f837f22a32bf87a228f8.png)

## <a name="deploy-certificates-and-vpn-configuration-script-to-the-clients"></a>Implantar certificados e o script de configuração de VPN para os clientes

Embora a maior parte da configuração do cliente VPN na [implantar VPN Always On](../vpn/always-on-vpn/deploy/always-on-vpn-deploy-deployment.md) seção adicionais duas etapas são necessárias para concluir a migração do DirectAccess para VPN Always On com êxito. 

Você deve garantir que o **VPN_Profile.ps1** vem _depois_ o certificado foi emitido para que o cliente VPN não tenta se conectar sem ele. Para fazer isso, você deve executar um script que adiciona somente os usuários que se registraram no certificado ao seu grupo de implantação de VPN pronta, que você usa para implantar a configuração de VPN Always On.

>[!NOTE] 
>A Microsoft recomenda que você teste esse processo antes de executá-lo em qualquer um de seus anéis de migração do usuário.

1.  **Criar e publicar o certificado VPN e habilitar o objeto de diretiva de grupo de registro automático (GPO).** Para implantações de VPN do Windows 10 tradicionais, com base em certificado, um certificado é emitido para o dispositivo ou usuário para que ele possa autenticar a conexão. Quando o novo certificado de autenticação é criado e publicado para o registro automático, você deve criar e implantar um GPO com a configuração de registro automático configurada para o grupo de usuários da VPN. Para obter as etapas configurar certificados e o registro automático, consulte [configurar a infraestrutura de servidor](../vpn/always-on-vpn/deploy/vpn-deploy-server-infrastructure.md).

2.  **Adicione usuários ao grupo de usuários da VPN.** Adicione qualquer usuários migrados para o grupo de usuários da VPN. Os usuários permanecem no grupo de segurança após você ter migrado-los para que eles possam receber as atualizações de certificado no futuro. Continue a adicionar usuários a esse grupo até que você moveu todos os usuários do DirectAccess para VPN Always On. 

3.  **Identifique os usuários que receberam um certificado de autenticação de VPN.** Você estiver migrando do DirectAccess, portanto, você precisará adicionar um método para identificar quando um cliente recebeu o certificado necessário e está pronto para receber as informações de configuração de VPN. Execute o **GetUsersWithCert.ps1** script para adicionar os usuários que atualmente são emitidos certificados nonrevoked originado do nome do modelo especificado para um grupo de segurança do AD DS especificado. Por exemplo, depois de executar o **GetUsersWithCert.ps1** script, qualquer usuário emitiu um certificado válido do certificado de autenticação VPN modelo é adicionado ao grupo de implantação de VPN pronta.

    >[!NOTE] 
    >Se você não tiver um método para identificar quando um cliente recebeu o certificado necessário, você pode implantar a configuração de VPN antes do certificado foi emitido para o usuário, fazendo com que a conexão VPN falhe. Para evitar essa situação, execute as **GetUsersWithCert.ps1** script na autoridade de certificação ou em uma agenda para sincronizar os usuários que tiverem recebido o certificado para o grupo de implantação de VPN pronta. Em seguida, você usará esse grupo de segurança para sua implantação de configuração de VPN no System Center Configuration Manager ou o Intune, que garante que o cliente gerenciado não recebe a configuração de VPN antes de receber o certificado de destino.
    
    ### <a name="getuserswithcertps1"></a>GetUsersWithCert.ps1
    
    ```powershell
    Import-module ActiveDirectory
    Import-Module AdcsAdministration
    
    $TemplateName = 'VPNUserAuthentication'##Certificate Template Name (not the friendly name)
    $GroupName = 'VPN Deployment Ready' ##Group you will add the users to
    $CSServerName = 'localhost\corp-dc-ca' ##CA Server Information
    
    $users= @()
    $TemplateID = (get-CATemplate | Where-Object {$_.Name -like $TemplateName} | Select-Object oid).oid
    $View = New-Object -ComObject CertificateAuthority.View
    $NULL = $View.OpenConnection($CSServerName)
    $View.SetResultColumnCount(3)
    $i1 = $View.GetColumnIndex($false, "User Principal Name")
    $i2 = $View.GetColumnIndex($false, "Certificate Template")
    $i3 = $View.GetColumnIndex($false, "Revocation Date")
    $i1, $i2, $i3 | %{$View.SetResultColumn($_) }
    $Row= $View.OpenView()
    
    while ($Row.Next() -ne -1){
    $Cert = New-Object PsObject
    $Col = $Row.EnumCertViewColumn()
    [void]$Col.Next()
    do {
    $Cert | Add-Member -MemberType NoteProperty $($Col.GetDisplayName()) -Value $($Col.GetValue(1)) -Force
          }
    until ($Col.Next() -eq -1)
    $col = ''
    
    if($cert."Certificate Template" -eq $TemplateID -and $cert."Revocation Date" -eq $NULL){
       $users= $users+= $cert."User Principal Name"
    $temp = $cert."User Principal Name"
    $user = get-aduser -Filter {UserPrincipalName -eq $temp} –Property UserPrincipalName
    Add-ADGroupMember $GroupName $user
       }
      }
    ```

4. Implante a configuração de VPN Always On. Como a VPN certificados de autenticação emitidos, e você executar o **GetUsersWithCert.ps1** script, os usuários são adicionados ao grupo de segurança pronto de implantação de VPN.


| Se você estiver usando...  | Então... |
| ---- | ---- |
| System Center Configuration Manager | Crie uma coleção de usuários com base na associação do grupo de segurança.<br><br>![](../../media/DA-to-AlwaysOnVPN/b38723b3ffcfacd697b83dd41a177f66.png)!|
| Intune | Simplesmente direcione o grupo de segurança diretamente após a sincronização. |
|
    
Cada vez que você executa o **GetUsersWithCert.ps1** script de configuração, você também deve executar uma regra de descoberta do AD DS para atualizar a associação de grupo de segurança no System Center Configuration Manager. Além disso, certifique-se de que associação ocorre a atualização para a coleção de implantação com frequência suficiente (alinhado com a regra de descoberta e de script).

Para obter informações adicionais sobre como usar o System Center Configuration Manager ou o Intune para implantar a VPN Always On para os clientes do Windows, consulte [Always On VPN implantação para o Windows Server e Windows 10](../vpn/always-on-vpn/deploy/always-on-vpn-deploy.md). Certifique-se, no entanto, para incorporar essas tarefas específicas de migração.

>[!NOTE] 
>Incorporar essas tarefas específicas de migração é uma diferença fundamental entre uma implantação simples de VPN Always On e migração do DirectAccess para VPN Always On. Certifique-se de definir corretamente a coleção para o grupo de segurança em vez de usar o método no guia de implantação de destino.


## <a name="remove-devices-from-the-directaccess-security-group"></a>Remover dispositivos do grupo de segurança do DirectAccess

Como os usuários receberão o certificado de autenticação e o **VPN_Profile.ps1** script de configuração, consulte correspondente implantações bem-sucedidas de script de configuração VPN no System Center Configuration Manager ou do Intune. Após cada implantação, remova o dispositivo do usuário do grupo de segurança do DirectAccess, para que posteriormente, você pode remover o DirectAccess. Intune e o System Center Configuration Manager contém informações de atribuição de dispositivo de usuário para ajudá-lo a determinar o dispositivo de cada usuário.

>[!NOTE] 
>Se você estiver aplicando GPOs do DirectAccess por meio de unidades organizacionais (OUs) em vez de grupos de computadores, mover o objeto de computador do usuário a UO.

## <a name="decommission-the-directaccess-infrastructure"></a>Encerrar a infraestrutura do DirectAccess

Quando você terminar de migrar todos os seus clientes DirectAccess para VPN Always On, você pode desativar a infraestrutura do DirectAccess e remover as configurações do DirectAccess da diretiva de grupo. A Microsoft recomenda executando as seguintes etapas para remover o DirectAccess do seu ambiente normalmente:

1.  [!INCLUDE [remove-config-settings-shortdesc-include](../includes/remove-config-settings-shortdesc-include.md)]

    ![](../../media/DA-to-AlwaysOnVPN/dbdc3d80e8dc1b8665f7b15d7d2ee1f6.png)

2.  [!INCLUDE [remove-da-security-group-shortdesc-include](../includes/remove-da-security-group-shortdesc-include.md)]

3.  **Limpeza de DNS.** Certifique-se de remover todos os registros do servidor DNS interno e o servidor DNS público relacionados ao DirectAccess, por exemplo, DA.contoso.com, DAGateway.contoso.com.

4.  [!INCLUDE [decommission-da-shortdesc-include](../includes/decommission-da-shortdesc-include.md)]

5.  **Remova todos os certificados DirectAccess dos serviços de certificados do Active Directory.** Se você usou os certificados de computador para a implementação do DirectAccess, remova os modelos publicados na pasta modelos de certificado no console de autoridade de certificação.

## <a name="next-step"></a>Próximas etapas
Concluir migração do DirectAccess para VPN Always On. 

---