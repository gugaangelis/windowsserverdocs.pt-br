---
title: Atualização para o AD FS no Windows Server 2016 com o SQL Server
description: ''
author: billmath
manager: mtillman
ms.date: 04/11/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 70f279bf-aea1-4f4f-9ab3-e9157233e267
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 0a3db2a095d1a31f55bd1c8bfc5bf3c9f6bb65b8
ms.sourcegitcommit: ccc802338b163abdad2e53b55f39addcfea04603
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/05/2019
ms.locfileid: "66687400"
---
# <a name="upgrading-to-ad-fs-in-windows-server-2016-with-sql-server"></a>Atualização para o AD FS no Windows Server 2016 com o SQL Server


> [!NOTE]  
> Iniciar apenas uma atualização com um período definitivo planejado para conclusão. Não é recomendável manter o AD FS em um estado de modo misto por um longo período de tempo, como sair do AD FS em um estado de modo misto pode causar problemas com o farm.


## <a name="moving-from-a-windows-server-2012-r2-ad-fs-farm-to-a-windows-server-2016-ad-fs-farm"></a>Movimentação de um farm do AD FS do Windows Server 2012 R2 para um farm do AD FS do Windows Server 2016  
O documento a seguir descreve como atualizar seu farm do AD FS no Windows Server 2012 R2 para o AD FS no Windows Server 2016, quando você estiver usando um SQL Server para o banco de dados do AD FS.  

### <a name="upgrading-ad-fs-to-windows-server-2016-fbl"></a>Atualização do AD FS para Windows Server 2016 FBL  
Novo no AD FS do Windows Server 2016 é o recurso de nível de comportamento (FBL) do farm.   Esse recurso é todo o farm e determina os recursos que pode usar o farm do AD FS.   Por padrão, o FBL em um farm do AD FS do Windows Server 2012 R2 é o Windows Server 2012 R2 FBL.  

Um servidor do AD FS do Windows Server 2016 pode ser adicionado a um farm do Windows Server 2012 R2 e ele funcionará com o mesmo FBL como um Windows Server 2012 R2.  Quando você tiver um servidor do AD FS do Windows Server 2016 operando dessa maneira, seu farm é chamado de "mixed".  No entanto, você não poderá tirar proveito dos novos recursos do Windows Server 2016 até que o FBL é gerado para o Windows Server 2016.  Com um farm misto:  

-   Os administradores podem adicionar novos servidores de Federação do Windows Server 2016 a um farm existente do Windows Server 2012 R2.  Como resultado, o farm está em "modo misto" e opera o nível de comportamento de farm do Windows Server 2012 R2.  Para garantir um comportamento consistente entre o farm, novos recursos do Windows Server 2016 não podem ser configurados ou usados nesse modo.  

-   Depois que todos os servidores de Federação do Windows Server 2012 R2 foram removidos do farm de modo misto e, no caso de um farm de WID, um dos novos servidores de Federação do Windows Server 2016 foi promovido à função de nó primário, o administrador, em seguida, pode gerar o FBL do Win Windows Server 2012 R2 para Windows Server 2016.  Como resultado, quaisquer novos recursos do AD FS Windows Server 2016 podem ser configurados e usados.  

-   Como resultado do recurso de farm misto, o AD FS no Windows Server 2012 R2, as organizações que desejam para atualizar para o Windows Server 2016 não precisará implantar um farm totalmente novo, exportar e importar dados de configuração.  Em vez disso, eles podem adicionar nós do Windows Server 2016 a um farm existente enquanto ele está online e incorram somente em relativamente breve tempo de inatividade envolvidas na raise FBL.  

Lembre-se de que no modo de farm misto, o farm do AD FS não é capaz de quaisquer novos recursos ou funcionalidade introduzida no AD FS no Windows Server 2016.  Isso significa que as organizações que desejam experimentar os novos recursos não podem fazer isso até que o FBL seja gerado.  Portanto, se sua organização está procurando para testar os novos recursos antes de rasing a FBL, você precisará implantar um farm separado para fazer isso.  

O restante de é o documento fornece as etapas para adicionar um servidor de Federação do Windows Server 2016 em um ambiente do Windows Server 2012 R2 e, depois, aumentar o FBL para o Windows Server 2016.  Essas etapas foram executadas em um ambiente de teste descrito no diagrama de arquitetura abaixo.  

> [!NOTE]  
> Antes de poder mover para o AD FS no Windows Server 2016 FBL, você deve remover todos os nós do Windows 2012 R2.  Você apenas não é possível atualizar um sistema de operacional do Windows Server 2012 R2 para Windows Server 2016 e fazer com que ele se tornar um nó de 2016.  Você precisará removê-lo e substituí-lo por um novo nó de 2016.  

Diagrama arquitetura a seguir mostra a configuração que foi usada para validar e registrar as etapas a seguir.

![Arquitetura](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/arch.png)


#### <a name="join-the-windows-2016-ad-fs-server-to-the-ad-fs-farm"></a>Ingressar o servidor Windows 2016 AD FS no farm do AD FS

1.  Usando o Gerenciador do servidor instalar a função de serviços de Federação do Active Directory do Windows Server 2016  

2.  Usando o Assistente de configuração do AD FS, Junte-se o novo servidor do Windows Server 2016 para o farm do AD FS existente.  Sobre o **bem-vindo** tela clique **próxima**.
 ![Unir o farm](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/configure1.png)  
3.  Sobre o **conectar-se ao Active Directory Domain Services** tela, s**especificar uma conta de administrador** com permissões para executar a configuração de serviços de Federação e clique em **Avançar**.
4.  Sobre o **especificar Farm** tela, insira o nome do SQL server e a instância e, em seguida, clique em **próxima**.
![Unir o farm](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/configure3.png)
5.  Sobre o **especificar o certificado SSL** tela, especifique o certificado e clique em **próxima**.
![Unir o farm](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/configure4.png)
6.  Sobre o **especificar conta de serviço** tela, especifique a conta de serviço e clique em **próxima**.
7.  Sobre o **examinar opções** tela, examine as opções e clique em **próxima**.
8.  Sobre o **verificações de pré-requisitos** tela, certifique-se de que todas as verificações de pré-requisito foram aprovadas e clique em **configurar**.
9.  Sobre o **resultados** tela, verifique se o servidor foi configurado com êxito e clique em **fechar**.


#### <a name="remove-the-windows-server-2012-r2-ad-fs-server"></a>Remover o servidor do AD FS do Windows Server 2012 R2

>[!NOTE]
>Não é necessário definir o servidor primário do AD FS usando Set-AdfsSyncProperties-função ao usar o SQL como o banco de dados.  Isso ocorre porque todos os nós são considerados principais nessa configuração.

1.  No servidor do AD FS do Windows Server 2012 R2 no Server Manager, use **remover funções e recursos** sob **gerenciar**.
![Remover servidor](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/remove1.png)
2.  Na tela **Antes de começar**, clique em **Avançar**.
3.  Sobre o **seleção de servidor** tela, clique em **próxima**.
4.  Sobre o **funções de servidor** tela, remova a seleção ao lado **serviços de Federação do Active Directory** e clique em **próxima**.
![Remover servidor](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/remove2.png)
5.  Sobre o **recursos** tela, clique em **próxima**.
6.  Sobre o **confirmação** tela, clique em **remover**.
7.  Quando isso for concluído, reinicie o servidor.

#### <a name="raise-the-farm-behavior-level-fbl"></a>Aumentar o nível de comportamento de Farm (FBL)
Antes dessa etapa, você precisa garantir que forestprep e domainprep foi executados no ambiente do Active Directory e que o Active Directory com o esquema do Windows Server 2016.  Este documento começou com um controlador de domínio do Windows 2016 e não exigia a execução desses porque eles foram executados quando o AD foi instalado.

>[!NOTE]
>Antes de iniciar o processo abaixo, verifique se o que Windows Server 2016 é atual executando o Windows Update de configurações.  Continue esse processo até que não existam mais atualizações.

1. Está disponível no servidor do Windows Server 2016, abra o PowerShell e execute o seguinte: **$cred = Get-Credential** e pressione enter.
2. Digite as credenciais com privilégios de administrador no SQL Server.
3. Agora no PowerShell, digite o seguinte: **Invoke-AdfsFarmBehaviorLevelRaise-$cred de credencial**
2. Quando solicitado, digite **Y**.  Isso iniciará a elevar o nível.  Quando isso for concluído com êxito o FBL sejam solucionados.  
![Concluir a atualização](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/finish1.png)
3. Agora, se você for para o gerenciamento do AD FS, você verá os novos nós que foram adicionados para o AD FS no Windows Server 2016  
4. Da mesma forma, você pode usar o cmdlet do PowerShell:  Get-AdfsFarmInformation para mostrar a você o FBL atual.  
![Concluir a atualização](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/finish2.png)

#### <a name="upgrade-the-configuration-version-of-existing-wap-servers"></a>Atualizar a versão da configuração dos servidores existentes do WAP
1. Em cada Proxy de aplicativo Web, configure novamente o WAP, executando o seguinte comando do PowerShell em uma janela elevada:  
    ```powershell
    $trustcred = Get-Credential -Message "Enter Domain Administrator credentials"
    Install-WebApplicationProxy -CertificateThumbprint {SSLCert} -fsname fsname -FederationServiceTrustCredential $trustcred  
    ```
2. Remover servidores antigos do cluster e manter somente os servidores WAP executando a versão mais recente do servidor, que foram reconfigurados acima, executando o seguinte commandlet do Powershell.
    ```powershell
    Set-WebApplicationProxyConfiguration -ConnectedServersName WAPServerName1, WAPServerName2
    ```
3. Verifique a configuração de WAP ao executar o Get-WebApplicationProxyConfiguration commmandlet. O ConnectedServersName refletirá o servidor execute do comando anterior.
    ```powershell
    Get-WebApplicationProxyConfiguration
    ```
4. Para atualizar o ConfigurationVersion dos servidores WAP, execute o seguinte comando do Powershell.
    ```powershell
    Set-WebApplicationProxyConfiguration -UpgradeConfigurationVersion
    ```
5. Verifique se que o ConfigurationVersion foi atualizado com o comando do Powershell Get-WebApplicationProxyConfiguration.
