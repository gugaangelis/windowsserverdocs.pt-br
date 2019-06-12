---
ms.assetid: 7671e0c9-faf0-40de-808a-62f54645f891
title: Atualização para AD FS no Windows Server 2016
description: ''
author: billmath
manager: femila
ms.date: 04/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 6f3cdc34ee03fab1a8fb1d42ebed2d2f76e2618d
ms.sourcegitcommit: ccc802338b163abdad2e53b55f39addcfea04603
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/05/2019
ms.locfileid: "66687408"
---
# <a name="upgrading-to-ad-fs-in-windows-server-2016-using-a-wid-database"></a>Atualizando para o AD FS no Windows Server 2016 por meio de um banco de dados WID


> [!NOTE]  
> Iniciar apenas uma atualização com um período definitivo planejado para conclusão. Não é recomendável manter o AD FS em um estado de modo misto por um longo período de tempo, como sair do AD FS em um estado de modo misto pode causar problemas com o farm.

## <a name="upgrading-a-windows-server-2012-r2-or-2016-ad-fs-farm-to-windows-server-2019"></a>Atualizando um Windows Server 2012 R2 ou o farm 2016 AD FS para Windows Server 2019
O documento a seguir descrevem como atualizar o farm do AD FS para AD FS no Windows Server 2019, quando você estiver usando um banco de dados WID.  

### <a name="ad-fs-farm-behavior-levels-fbl"></a>Níveis de comportamento do AD FS Farm (FBL)  
No AD FS do Windows Server 2016, o nível de comportamento de farm (FBL) foi introduzido. Isso é a configuração de todo o farm que determina que o farm de recursos do AD FS pode ser usado.

A tabela a seguir lista os valores FBL pela versão do Windows Server:

| Versão do Windows Server  | FBL | Nome de banco de dados de configuração do AD FS |
| ------------- | ------------- | ------------- |
| 2012 R2  | 1  | AdfsConfiguration |
| 2016  | 3  | AdfsConfigurationV3 |
| 2019  | 4  | AdfsConfigurationV4 |

> [!NOTE]  
> Atualizar o FBL cria um novo banco de dados de configuração do AD FS.  Consulte a tabela acima para os nomes do banco de dados de configuração para cada versão do AD FS do Windows Server e o valor FBL

### <a name="new-vs-upgraded-farms"></a>Nova vs farms de servidores atualizado
Por padrão, o FBL em um novo farm do AD FS corresponde ao valor para a versão do Windows Server do primeiro nó do farm instalado.  

Um servidor do AD FS de uma versão mais recente pode ser unido a um farm do AD FS 2012 R2 ou 2016 e o farm operam no FBL mesmo como o nó existente (s). Quando você tiver várias versões do Windows Server, operando no mesmo farm, o valor de FBL da versão mais antiga, seu farm é chamado de "mixed". No entanto, você não poderá tirar proveito dos recursos das versões posteriores até que o FBL seja gerado. Com um farm misto:  

-   Os administradores podem adicionar novos servidores de federação de 2019 do Windows Server para um banco de dados existente do Windows Server 2012 R2 ou o farm 2016. Como resultado, o farm está em "modo misto" e opera no nível de comportamento do farm como o farm original. Para garantir um comportamento consistente entre o farm, recursos das versões mais recentes de FS do AD do Windows Server não podem ser configurados ou usados.  

- Antes do FBL pode ser gerado, os administradores devem remover os nós do AD FS nas versões anteriores do Windows Server do farm.  No caso de um farm de WID, observe que isso exige uma da novo PA de servidores de federação de 2019 do Windows Server ser promovido à função de nó primário no farm.

-   Depois que todos os servidores de federação no farm são a mesma versão do Windows Server, o FBL pode ser gerado.  Como resultado, os novos recursos de 2019 do servidor do Windows do AD FS, em seguida, podem ser configurados e usados.

Lembre-se de que no modo de farm misto, o farm do AD FS não é capaz de quaisquer novos recursos ou funcionalidade introduzida no AD FS no Windows Server 2019. Isso significa que as organizações que desejam experimentar os novos recursos não podem fazer isso até que o FBL seja gerado. Portanto, se sua organização está procurando para testar os novos recursos antes de rasing a FBL, você precisará implantar um farm separado para fazer isso.  

O restante de é o documento fornece as etapas para adicionar um servidor de Federação do Windows Server 2019 ao ambiente de 2012 R2 ou um Windows Server 2016 e, em seguida, acionar o FBL para Windows Server 2019. Essas etapas foram executadas em um ambiente de teste descrito no diagrama de arquitetura abaixo.  

> [!NOTE]  
> Antes de poder mover para o AD FS no Windows Server 2019 FBL, você deve remover todos os Windows Server 2016 ou nós do 2012 R2. Você apenas não é possível atualizar um Windows Server 2016 ou sistema operacional do 2012 R2 para Windows Server 2019 e fazer com que ele se tornar um nó de 2019. Você precisará removê-lo e substituí-lo por um novo nó de 2019.



##### <a name="to-upgrade-your-ad-fs-farm-to-windows-server-2019-farm-behavior-level"></a>Para atualizar o farm do AD FS para o nível de comportamento de Farm do Windows Server de 2019  

1.  Usando o Gerenciador do servidor, instale a função de serviços de Federação do Active Directory sobre o Windows Server 2019

2.  Usando o Assistente de configuração do AD FS, Junte-se o novo servidor do Windows Server 2019 ao farm do AD FS existente.  

    ![atualização](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_1.png)  

3.  No servidor de federação de 2019 do Windows Server, abra o gerenciamento do AD FS. Observe que os recursos de gerenciamento não estão disponíveis porque este servidor de Federação não é o servidor primário.  

    ![atualização](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_3.png)  

4.  No servidor do Windows Server 2019, abra uma janela de comando do PowerShell com privilégios elevados e execute o seguinte cmdlet: `Set-AdfsSyncProperties -Role PrimaryComputer`

    ![atualização](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_4.png)  

5.  No servidor do AD FS que foi previamente configurado como primário, abra uma janela de comando do PowerShell com privilégios elevados e execute o seguinte cmdlet: `Set-AdfsSyncProperties -Role SecondaryComputer -PrimaryComputerName {FQDN} `

    ![atualização](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_5.png)  

6.  Agora no servidor de Federação do Windows Server 2016, abra Gerenciamento do AD FS. Observe que, agora todos os recursos do administrador aparecem porque a função primária foi transferida para esse servidor.  

    ![atualização](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_6.png)  

7.  Se você estiver atualizando um farm do AD FS 2012 R2 para 2016 ou 2019, a atualização do farm requer o esquema do AD seja pelo menos nível 85.  Para atualizar o esquema, a mídia de instalação com o Windows Server 2016, abra um prompt de comando e navegue até o diretório de support\adprep. Execute o seguinte:  `adprep /forestprep`

    ![atualização](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_7.png)  

    Depois de concluído, execute `adprep/domainprep`
    >[!NOTE]
    >Antes de executar a próxima etapa, verifique se que o Windows Server é atual executando o Windows Update de configurações. Continue esse processo até que não existam mais atualizações.
    >

    ![atualização](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_8.png)  

8. Está disponível no servidor do Windows Server 2016 abra PowerShell e execute o seguinte cmdlet:
    >[!NOTE]
    > Todos os servidores do 2012 R2 devem ser removidos do farm antes de executar a próxima etapa.

    `Invoke-AdfsFarmBehaviorLevelRaise`  

    ![atualização](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_9.png)  

9. Quando solicitado, digite Y. Isso iniciará a elevar o nível. Quando isso for concluído com êxito o FBL sejam solucionados.  

    ![atualização](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_10.png)  

10. Agora, se você for para o gerenciamento do AD FS, você verá que os novos recursos foram adicionados para a versão mais recente do AD FS

    ![atualização](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_12.png)  

11. Da mesma forma, você pode usar o cmdlet do PowerShell: `Get-AdfsFarmInformation` para mostrar a você o FBL atual.  

    ![atualização](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_13.png)  

12. Para atualizar os servidores do WAP para o nível mais recente, em cada Proxy de aplicativo Web, configure novamente o WAP, executando o seguinte comando do PowerShell em uma janela elevada:  
    ```powershell
    $trustcred = Get-Credential -Message "Enter Domain Administrator credentials"
    Install-WebApplicationProxy -CertificateThumbprint {SSLCert} -fsname fsname -FederationServiceTrustCredential $trustcred  
    ```
    Remover servidores antigos do cluster e manter somente os servidores WAP executando a versão mais recente do servidor, que foram reconfigurados acima, executando o seguinte commandlet do Powershell.
    ```powershell
    Set-WebApplicationProxyConfiguration -ConnectedServersName WAPServerName1, WAPServerName2
    ```
    Verifique a configuração de WAP ao executar o Get-WebApplicationProxyConfiguration commmandlet. O ConnectedServersName refletirá o servidor execute do comando anterior.
    ```powershell
    Get-WebApplicationProxyConfiguration
    ```
    Para atualizar o ConfigurationVersion dos servidores WAP, execute o seguinte comando do Powershell.
    ```powershell
    Set-WebApplicationProxyConfiguration -UpgradeConfigurationVersion
    ```
    Isso concluirá a atualização dos servidores WAP.
