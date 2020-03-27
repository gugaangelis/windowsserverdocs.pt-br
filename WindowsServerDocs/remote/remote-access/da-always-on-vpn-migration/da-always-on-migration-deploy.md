---
title: Migração do DirectAccess para Always On VPN
description: Migrar do DirectAccess para Always On VPN requer um processo específico para migrar clientes, o que ajuda a minimizar as condições de corrida que surgem da execução de etapas de migração fora de ordem.
manager: dougkim
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: eeca4cf7-90f0-485d-843c-76c5885c54b0
ms.author: lizross
author: eross-msft
ms.date: 06/07/2018
ms.openlocfilehash: 9f78edf0e48dc914b09a5e6f2d054e0fafba62e3
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80309300"
---
# <a name="migrate-to-always-on-vpn-and-decommission-directaccess"></a>Migre para VPN Always On e encerrar o DirectAccess

>Aplicável a: Windows Server (Canal Semestral), Windows Server 2016, Windows 10

&#171;[ **Anterior:** planejar o directaccess para Always on migração de VPN](da-always-on-migration-planning.md)<br>

Migrar do DirectAccess para Always On VPN requer um processo específico para migrar clientes, o que ajuda a minimizar as condições de corrida que surgem da execução de etapas de migração fora de ordem. Em um alto nível, o processo de migração consiste em quatro etapas principais:

1.  **Implante uma infraestrutura de VPN lado a lado.** Depois de determinar suas fases de migração e os recursos que você deseja incluir em sua implantação, você implantará a infraestrutura de VPN lado a lado com a infraestrutura existente do DirectAccess.  

2.  **Implantar certificados e configuração para os clientes.**  Quando a infraestrutura de VPN estiver pronta, você criará e publicará os certificados necessários no cliente. Quando os clientes tiverem recebido os certificados, você implantará o script de configuração VPN_Profile. ps1. Como alternativa, você pode usar o Intune para configurar o cliente VPN. Use o Microsoft Endpoint Configuration Manager ou Microsoft Intune para monitorar implantações de configuração de VPN bem-sucedidas.

3.  [!INCLUDE [remove-da-security-group-shortdesc-include](../includes/remove-da-security-group-shortdesc-include.md)]

4.  [!INCLUDE [decommission-da-shortdesc-include](../includes/decommission-da-shortdesc-include.md)]

Antes de iniciar o processo de migração do DirectAccess para Always On VPN, verifique se você planejou a migração.  Se você não planejou a migração, consulte [planejar o DirectAccess para Always on a migração de VPN](da-always-on-migration-planning.md).

>[!TIP] 
>Esta seção não é um guia de implantação passo a passo para Always On VPN, mas, em vez disso, destina-se a complementar [Always on implantação de VPN para Windows Server e Windows 10](../vpn/always-on-vpn/deploy/always-on-vpn-deploy.md) e fornecer diretrizes de implantação específicas de migração.

## <a name="deploy-a-side-by-side-vpn-infrastructure"></a>Implantar uma infraestrutura de VPN lado a lado

Você implanta a infraestrutura de VPN lado a lado com a infraestrutura existente do DirectAccess.  Para obter detalhes passo a passo, consulte [Always on implantação de VPN para Windows Server e Windows 10](../vpn/always-on-vpn/deploy/always-on-vpn-deploy.md) para instalar e configurar a infraestrutura de VPN Always on. 

A implantação lado a lado consiste nas seguintes tarefas de alto nível:

1.  Crie os grupos usuários VPN, servidores VPN e servidores NPS.

2.  Crie e publique os modelos de certificado necessários.

3.  Registre os certificados do servidor.

4.  Instale e configure o serviço de acesso remoto para Always On VPN.

5.  Instalar e configurar o NPS.

6.  Configure as regras de firewall e de DNS para Always On VPN.

A imagem a seguir fornece uma referência visual para as alterações de infraestrutura durante a migração de VPN do DirectAccess para o Always On.

![](../../media/DA-to-AlwaysOnVPN/6b64f322f945f837f22a32bf87a228f8.png)

## <a name="deploy-certificates-and-vpn-configuration-script-to-the-clients"></a>Implantar certificados e script de configuração de VPN para os clientes

Embora a maior parte da configuração do cliente VPN na seção [implantar Always on VPN](../vpn/always-on-vpn/deploy/always-on-vpn-deploy-deployment.md) , são necessárias duas etapas adicionais para concluir a migração do DirectAccess para Always on VPN com êxito. 

Você deve garantir que o **VPN_Profile. ps1** venha _depois_ que o certificado foi emitido para que o cliente VPN não tente se conectar sem ele. Para fazer isso, você executa um script que adiciona somente os usuários que registraram no certificado ao seu grupo de implantação de VPN pronto, que você usa para implantar a configuração de VPN Always On.

>[!NOTE] 
>A Microsoft recomenda que você teste esse processo antes de executá-lo em qualquer um dos seus anéis de migração de usuário.

1.  **Crie e publique o certificado VPN e habilite o objeto de Política de Grupo de registro automático (GPO).** Para implantações de VPN do Windows 10 baseadas em certificado tradicionais, um certificado é emitido para o dispositivo ou para o usuário para que ele possa autenticar a conexão. Quando o novo certificado de autenticação é criado e publicado para registro automático, você deve criar e implantar um GPO com a configuração de registro automático configurada para o grupo de usuários VPN. Para obter as etapas para configurar certificados e registro automático, consulte [Configurar a infraestrutura do servidor](../vpn/always-on-vpn/deploy/vpn-deploy-server-infrastructure.md).

2.  **Adicione usuários ao grupo de usuários VPN.** Adicione qualquer usuário que você migrar para o grupo de usuários VPN. Esses usuários permanecem nesse grupo de segurança depois de terem sido migrados para que possam receber quaisquer atualizações de certificado no futuro. Continue a adicionar usuários a este grupo até que você tenha movido todos os usuários do DirectAccess para Always On VPN. 

3.  **Identifique os usuários que receberam um certificado de autenticação de VPN.** Você está migrando do DirectAccess, portanto, será necessário adicionar um método para identificar quando um cliente recebeu o certificado necessário e está pronto para receber as informações de configuração de VPN. Execute o script **GetUsersWithCert. ps1** para adicionar usuários que atualmente recebem certificados não revogados provenientes do nome do modelo especificado para um grupo de segurança de AD DS especificado. Por exemplo, depois de executar o script **GetUsersWithCert. ps1** , qualquer usuário que emitiu um certificado válido do modelo de certificado de autenticação de VPN é adicionado ao grupo pronto de implantação de VPN.

    >[!NOTE] 
    >Se você não tiver um método para identificar quando um cliente recebeu o certificado necessário, você poderá implantar a configuração de VPN antes que o certificado tenha sido emitido para o usuário, fazendo com que a conexão VPN falhe. Para evitar essa situação, execute o script **GetUsersWithCert. ps1** na autoridade de certificação ou em uma agenda para sincronizar os usuários que receberam o certificado para o grupo pronto de implantação de VPN. Em seguida, você usará esse grupo de segurança para direcionar sua implantação de configuração de VPN no Microsoft Endpoint Configuration Manager ou no Intune, o que garante que o cliente gerenciado não receba a configuração de VPN antes de receber o certificado.
    
    ### <a name="getuserswithcertps1"></a>GetUsersWithCert. ps1
    
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

4. Implante a configuração de VPN Always On. Como os certificados de autenticação de VPN são emitidos e você executa o script **GetUsersWithCert. ps1** , os usuários são adicionados ao grupo de segurança de implantação de VPN pronto.


| Se você estiver usando...  | Então... |
| ---- | ---- |
| Configuration Manager | Crie uma coleção de usuários com base na associação do grupo de segurança.<br><br>![](../../media/DA-to-AlwaysOnVPN/b38723b3ffcfacd697b83dd41a177f66.png)!|
| Intune | Basta direcionar o grupo de segurança diretamente depois que ele for sincronizado. |
|
    
Sempre que executar o script de configuração **GetUsersWithCert. ps1** , você também deverá executar uma regra de descoberta de AD DS para atualizar a associação de grupo de segurança no Configuration Manager. Além disso, verifique se a atualização de associação para a coleção de implantação ocorre com frequência (alinhada com a regra de descoberta e script).

Para obter informações adicionais sobre como usar o Configuration Manager ou o Intune para implantar Always On VPN em clientes Windows, consulte [implantação de vpn Always on para Windows Server e Windows 10](../vpn/always-on-vpn/deploy/always-on-vpn-deploy.md). No entanto, não se esqueça de incorporar essas tarefas específicas à migração.

>[!NOTE] 
>Incorporar essas tarefas específicas de migração é uma diferença crítica entre uma implantação de VPN simples Always On e a migração do DirectAccess para Always On VPN. Certifique-se de definir corretamente a coleção para o grupo de segurança de destino em vez de usar o método no guia de implantação.


## <a name="remove-devices-from-the-directaccess-security-group"></a>Remover dispositivos do grupo de segurança do DirectAccess

Conforme os usuários recebem o certificado de autenticação e o script de configuração **VPN_Profile. ps1** , você vê as implantações de script de configuração de VPN bem-sucedidas correspondentes no Configuration Manager ou no Intune. Após cada implantação, remova o dispositivo desse usuário do grupo de segurança do DirectAccess para que você possa remover o DirectAccess posteriormente. O Intune e o Configuration Manager contêm informações de atribuição de dispositivo de usuário para ajudá-lo a determinar o dispositivo de cada usuário.

>[!NOTE] 
>Se você estiver aplicando GPOs do DirectAccess por meio de UOs (unidades organizacionais) em vez de grupos de computadores, mova o objeto do computador do usuário para fora da UO.

## <a name="decommission-the-directaccess-infrastructure"></a>Encerrar a infraestrutura do DirectAccess

Quando terminar de migrar todos os clientes do DirectAccess para Always On VPN, você poderá encerrar a infraestrutura do DirectAccess e remover as configurações do DirectAccess de Política de Grupo. A Microsoft recomenda executar as seguintes etapas para remover o DirectAccess do seu ambiente normalmente:

1.  [!INCLUDE [remove-config-settings-shortdesc-include](../includes/remove-config-settings-shortdesc-include.md)]

    ![](../../media/DA-to-AlwaysOnVPN/dbdc3d80e8dc1b8665f7b15d7d2ee1f6.png)

2.  [!INCLUDE [remove-da-security-group-shortdesc-include](../includes/remove-da-security-group-shortdesc-include.md)]

3.  **Limpe o DNS.** Certifique-se de remover todos os registros do servidor DNS interno e do servidor DNS público relacionados ao DirectAccess, por exemplo, DA.contoso.com, DAGateway.contoso.com.

4.  [!INCLUDE [decommission-da-shortdesc-include](../includes/decommission-da-shortdesc-include.md)]

5.  **Remova os certificados do DirectAccess de Active Directory serviços de certificados.** Se você usou certificados de computador para a implementação do DirectAccess, remova os modelos publicados da pasta modelos de certificado no console da autoridade de certificação.

## <a name="next-step"></a>Próximas etapas
Você concluiu a migração do DirectAccess para Always On VPN. 

---