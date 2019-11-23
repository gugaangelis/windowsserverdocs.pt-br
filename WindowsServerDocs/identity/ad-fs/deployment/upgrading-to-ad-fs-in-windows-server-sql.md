---
title: Atualizando para o AD FS no Windows Server 2016 com SQL Server
description: ''
author: billmath
manager: mtillman
ms.date: 04/11/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 70f279bf-aea1-4f4f-9ab3-e9157233e267
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: dd843724faf1c7a8101def84091484a5e7f7900f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408238"
---
# <a name="upgrading-to-ad-fs-in-windows-server-2016-with-sql-server"></a>Atualizando para o AD FS no Windows Server 2016 com SQL Server


> [!NOTE]  
> Inicie apenas uma atualização com um período de tempo definitivo planejado para conclusão. Não é recomendável manter AD FS em um estado de modo misto por um longo período de tempo, pois deixar AD FS em um estado de modo misto pode causar problemas com o farm.


## <a name="moving-from-a-windows-server-2012-r2-ad-fs-farm-to-a-windows-server-2016-ad-fs-farm"></a>Movendo de um farm de AD FS do Windows Server 2012 R2 para um farm de AD FS do Windows Server 2016  
O documento a seguir descreverá como atualizar seu AD FS farm do Windows Server 2012 R2 para AD FS no Windows Server 2016 quando você estiver usando um SQL Server para o banco de dados AD FS.  

### <a name="upgrading-ad-fs-to-windows-server-2016-fbl"></a>Atualizando AD FS para o Windows Server 2016 FBL  
O novo no AD FS para Windows Server 2016 é o recurso de nível de comportamento de farm (FBL).   Esse recurso tem todo o farm e determina os recursos que o farm de AD FS pode usar.   Por padrão, o FBL em um farm de AD FS do Windows Server 2012 R2 está no Windows Server 2012 R2 FBL.  

Um Windows Server 2016 AD FS Server pode ser adicionado a um farm do Windows Server 2012 R2 e ele funcionará no mesmo FBL que o Windows Server 2012 R2.  Quando você tem um Windows Server 2016 AD FS Server operando dessa maneira, o farm é considerado "misto".  No entanto, você não poderá aproveitar os novos recursos do Windows Server 2016 até que o FBL seja gerado para o Windows Server 2016.  Com um farm misto:  

-   Os administradores podem adicionar novos servidores de Federação do Windows Server 2016 a um farm existente do Windows Server 2012 R2.  Como resultado, o farm está no "modo misto" e opera o nível de comportamento do farm do Windows Server 2012 R2.  Para garantir um comportamento consistente em todo o farm, os novos recursos do Windows Server 2016 não podem ser configurados ou usados nesse modo.  

-   Depois que todos os servidores de Federação do Windows Server 2012 R2 tiverem sido removidos do farm de modo misto e, no caso de um farm de WID, um dos novos servidores de Federação do Windows servindo 2016 tiver sido promovido para a função de nó primário, o administrador poderá então elevar o FBL do Win o Windows Server 2012 R2 para o servidor 2016.  Como resultado, qualquer novo AD FS recursos do Windows Server 2016 podem ser configurados e usados.  

-   Como resultado do recurso de farm misto, AD FS as organizações do Windows Server 2012 R2 que procuram atualizar para o Windows Server 2016 não precisarão implantar um farm totalmente novo, exportar e importar dados de configuração.  Em vez disso, eles podem adicionar nós do Windows Server 2016 a um farm existente enquanto estiverem online e incorrerem apenas no tempo de inatividade relativamente curto envolvido no FBL raise.  

Lembre-se de que, embora no modo de farm misto, o farm de AD FS não é capaz de quaisquer novos recursos ou funcionalidades introduzidos no AD FS no Windows Server 2016.  Isso significa que as organizações que desejam experimentar novos recursos não podem fazer isso até que o FBL seja gerado.  Portanto, se sua organização estiver procurando testar os novos recursos antes de rasing o FBL, será necessário implantar um farm separado para fazer isso.  

O restante do documento is fornece as etapas para adicionar um servidor de Federação do Windows Server 2016 a um ambiente do Windows Server 2012 R2 e, em seguida, aumentar o FBL para o Windows Server 2016.  Essas etapas foram executadas em um ambiente de teste descrito pelo diagrama arquitetônico abaixo.  

> [!NOTE]  
> Antes de poder mover para AD FS no Windows Server 2016 FBL, você deve remover todos os nós do Windows 2012 R2.  Não é possível apenas atualizar um sistema operacional Windows Server 2012 R2 para o Windows Server 2016 e fazer com que ele se torne um nó 2016.  Será necessário removê-lo e substituí-lo por um novo nó 2016.  

Para o diagrama arquitetônico a seguir mostra a configuração que foi usada para validar e registrar as etapas abaixo.

![Arquitetura](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/arch.png)


#### <a name="join-the-windows-2016-ad-fs-server-to-the-ad-fs-farm"></a>Ingresse o servidor de AD FS do Windows 2016 no farm de AD FS

1.  Usando Gerenciador do Servidor instalar a função Serviços de Federação do Active Directory (AD FS) no Windows Server 2016  

2.  Usando o assistente de configuração do AD FS, ingresse o novo servidor do Windows Server 2016 no farm de AD FS existente.  Na tela de **boas-vindas** , clique em **Avançar**.
 ![farm de junção](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/configure1.png)  
3.  Na tela **conectar ao Active Directory Domain Services** , s**pecificar uma conta de administrador** com permissões para executar a configuração dos serviços de Federação e clique em **Avançar**.
4.  Na tela **especificar farm** , insira o nome do SQL Server e da instância e clique em **Avançar**.
![farm de junção](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/configure3.png)
5.  Na tela **especificar certificado SSL** , especifique o certificado e clique em **Avançar**.
![farm de junção](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/configure4.png)
6.  Na tela **especificar conta de serviço** , especifique a conta de serviço e clique em **Avançar**.
7.  Na tela **Opções de revisão** , examine as opções e clique em **Avançar**.
8.  Na tela **verificações de pré-requisitos** , verifique se todas as verificações de pré-requisito foram aprovadas e clique em **Configurar**.
9.  Na tela de **resultados** , verifique se o servidor foi configurado com êxito e clique em **fechar**.


#### <a name="remove-the-windows-server-2012-r2-ad-fs-server"></a>Remover o Windows Server 2012 R2 AD FS Server

>[!NOTE]
>Você não precisa definir o servidor de AD FS primário usando Set-AdfsSyncProperties-role ao usar o SQL como o banco de dados.  Isso ocorre porque todos os nós são considerados primários nessa configuração.

1.  No Windows Server 2012 R2 AD FS Server em Gerenciador do Servidor use **remover funções e recursos** em **gerenciar**.
![remover](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/remove1.png) do servidor
2.  Na tela **Antes de começar**, clique em **Avançar**.
3.  Na tela de **seleção de servidor** , clique em **Avançar**.
4.  Na tela **funções de servidor** , remova a marca de seleção ao lado de **serviços de Federação do Active Directory (AD FS)** e clique em **Avançar**.
![remover](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/remove2.png) do servidor
5.  Na tela **recursos** , clique em **Avançar**.
6.  Na tela de **confirmação** , clique em **remover**.
7.  Quando isso for concluído, reinicie o servidor.

#### <a name="raise-the-farm-behavior-level-fbl"></a>Aumentar o nível de comportamento do farm (FBL)
Antes desta etapa, você precisa garantir que forestprep e DomainPrep tenham sido executados em seu ambiente de Active Directory e que Active Directory tenha o esquema do Windows Server 2016.  Este documento foi iniciado com um controlador de domínio do Windows 2016 e não exigiu a execução deles porque eles foram executados quando o AD foi instalado.

>[!NOTE]
>Antes de iniciar o processo abaixo, verifique se o Windows Server 2016 está atualizado executando Windows Update de configurações.  Continue esse processo até que não existam mais atualizações.

1. Agora, no servidor do Windows Server 2016, abra o PowerShell e execute o seguinte: **$cred = Get-Credential** e pressione Enter.
2. Insira as credenciais que têm privilégios de administrador no SQL Server.
3. Agora, no PowerShell, insira o seguinte: **Invoke-AdfsFarmBehaviorLevelRaise-Credential $cred**
2. Quando solicitado, digite **Y**.  Isso começará a gerar o nível.  Quando isso for concluído, o FBL será gerado com êxito.  
![concluir a atualização](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/finish1.png)
3. Agora, se você for para AD FS Management, verá os novos nós que foram adicionados para AD FS no Windows Server 2016  
4. Da mesma forma, você pode usar o cmdlt do PowerShell: Get-AdfsFarmInformation para mostrar o FBL atual.  
![Concluir atualização](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/finish2.png)

#### <a name="upgrade-the-configuration-version-of-existing-wap-servers"></a>Atualizar a versão de configuração de servidores WAP existentes
1. Em cada proxy de aplicativo Web, reconfigure o WAP executando o seguinte comando do PowerShell em uma janela com privilégios elevados:  
    ```powershell
    $trustcred = Get-Credential -Message "Enter Domain Administrator credentials"
    Install-WebApplicationProxy -CertificateThumbprint {SSLCert} -fsname fsname -FederationServiceTrustCredential $trustcred  
    ```
2. Remova os servidores antigos do cluster e mantenha somente os servidores WAP que executam a versão mais recente do servidor, que foram reconfigurados acima, executando o cmdlet do PowerShell a seguir.
    ```powershell
    Set-WebApplicationProxyConfiguration -ConnectedServersName WAPServerName1, WAPServerName2
    ```
3. Verifique a configuração de WAP executando o commmandlet Get-WebApplicationProxyConfiguration. O ConnectedServersName refletirá a execução do servidor do comando anterior.
    ```powershell
    Get-WebApplicationProxyConfiguration
    ```
4. Para atualizar o ConfigurationVersion dos servidores WAP, execute o seguinte comando do PowerShell.
    ```powershell
    Set-WebApplicationProxyConfiguration -UpgradeConfigurationVersion
    ```
5. Verifique se o ConfigurationVersion foi atualizado com o comando Get-WebApplicationProxyConfiguration do PowerShell.
