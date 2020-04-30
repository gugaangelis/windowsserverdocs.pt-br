---
ms.assetid: 7671e0c9-faf0-40de-808a-62f54645f891
title: Atualização para AD FS no Windows Server 2016
author: billmath
manager: femila
ms.date: 04/09/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 9389d1565462572a5617856f0f2531580b069745
ms.sourcegitcommit: 074b59341640a8ae0586d6b37df7ba256e03a0c6
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81650071"
---
# <a name="upgrading-to-ad-fs-in-windows-server-2016-using-a-wid-database"></a>Atualizando para o AD FS no Windows Server 2016 por meio de um banco de dados WID


> [!NOTE]
> Inicie apenas uma atualização com um período de tempo definitivo planejado para conclusão. Não é recomendável manter AD FS em um estado de modo misto por um longo período de tempo, pois deixar AD FS em um estado de modo misto pode causar problemas com o farm.

## <a name="upgrading-a-windows-server-2012-r2-or-2016-ad-fs-farm-to-windows-server-2019"></a>Atualizando um farm de AD FS do Windows Server 2012 R2 ou 2016 para o Windows Server 2019
O documento a seguir descreverá como atualizar seu farm de AD FS para AD FS no Windows Server 2019 quando você estiver usando um banco de dados WID.

### <a name="ad-fs-farm-behavior-levels-fbl"></a>FBL (níveis de comportamento de farm de AD FS)
No AD FS para o Windows Server 2016, o nível de comportamento do farm (FBL) foi introduzido. Essa é uma configuração de todo o farm que determina os recursos que o farm de AD FS pode usar.

A tabela a seguir lista os valores de FBL por versão do Windows Server:

| Versão do Windows Server  | FBL | Nome do banco de dados de configuração AD FS |
| ------------- | ------------- | ------------- |
| 2012 R2  | 1  | AdfsConfiguration |
| 2016  | 3  | AdfsConfigurationV3 |
| 2019  | 4  | AdfsConfigurationV4 |

> [!NOTE]
> A atualização do FBL cria um novo banco de dados de configuração de AD FS.  Consulte a tabela acima para obter os nomes do banco de dados de configuração para cada versão AD FS do Windows Server e o valor FBL

### <a name="new-vs-upgraded-farms"></a>Novos farms do vs atualizados
Por padrão, o FBL em um novo farm de AD FS corresponde ao valor da versão do Windows Server do primeiro nó do farm instalado.

Um servidor AD FS de uma versão posterior pode ser Unido a um farm AD FS 2012 R2 ou 2016, e o farm funcionará no mesmo FBL que os nós existentes. Quando você tiver várias versões do Windows Server operando no mesmo farm no valor FBL da versão mais baixa, o farm será considerado "Mixed". No entanto, você não poderá aproveitar os recursos das versões posteriores até que o FBL seja gerado. Com um farm misto:

- Os administradores podem adicionar novos servidores de Federação do Windows Server 2019 a um farm existente do Windows Server 2012 R2 ou 2016. Como resultado, o farm está no "modo misto" e opera no mesmo nível de comportamento do farm que o farm original. Para garantir um comportamento consistente em todo o farm, os recursos das versões mais recentes do Windows Server AD FS não podem ser configurados ou usados.

- Antes que o FBL possa ser gerado, os administradores devem remover os nós de AD FS das versões anteriores do Windows Server do farm.  No caso de um farm de WID, observe que isso requer que um dos novos servidores de Federação do Windows Server 2019 seja promovido para a função de nó primário no farm.

- Depois que todos os servidores de Federação no farm estiverem na mesma versão do Windows Server, o FBL poderá ser gerado.  Como resultado, qualquer novo AD FS recursos do Windows Server 2019 podem ser configurados e usados.

Lembre-se de que, embora no modo de farm misto, o farm de AD FS não é capaz de quaisquer novos recursos ou funcionalidades introduzidos no AD FS no Windows Server 2019. Isso significa que as organizações que desejam experimentar novos recursos não podem fazer isso até que o FBL seja gerado. Portanto, se sua organização estiver procurando testar os novos recursos antes de gerar o FBL, será necessário implantar um farm separado para fazer isso.

O restante do documento is fornece as etapas para adicionar um servidor de Federação do Windows Server 2019 a um ambiente do Windows Server 2016 ou 2012 R2 e, em seguida, elevar o FBL para o Windows Server 2019. Essas etapas foram executadas em um ambiente de teste descrito pelo diagrama arquitetônico abaixo.

> [!NOTE]
> Antes de poder mover para AD FS no Windows Server 2019 FBL, você deve remover todos os nós do Windows Server 2016 ou do 2012 R2. Não é possível apenas atualizar um sistema operacional Windows Server 2016 ou 2012 R2 para o Windows Server 2019 e fazer com que ele se torne um nó 2019. Será necessário removê-lo e substituí-lo por um novo nó 2019.

> [!NOTE]
> Se grupos de AlwaysOnAvailability ou replicação de mesclagem estiverem configurados no AD FS, remova toda a replicação de quaisquer bancos de dados do ADFS antes da atualização e aponte todos os nós para o banco de dados SQL primário. Depois de fazer isso, execute a atualização do farm conforme documentado. Após a atualização, adicione grupos de AlwaysOnAvailability ou replicação de mesclagem para os novos bancos de dados.

##### <a name="to-upgrade-your-ad-fs-farm-to-windows-server-2019-farm-behavior-level"></a>Para atualizar o farm de AD FS para o nível de comportamento de farm do Windows Server 2019

1. Usando Gerenciador do Servidor, instale a função Serviços de Federação do Active Directory (AD FS) no Windows Server 2019

2. Usando o assistente de configuração do AD FS, ingresse o novo servidor do Windows Server 2019 no farm de AD FS existente.

![atualização](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_1.png)

3. No servidor de Federação do Windows Server 2019, abra o gerenciamento de AD FS. Observe que os recursos de gerenciamento não estão disponíveis porque esse servidor de Federação não é o servidor primário.

![atualização](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_3.png)

4. No servidor do Windows Server 2019, abra uma janela de comando do PowerShell com privilégios elevados e execute o seguinte cmdlet:

```PowerShell
Set-AdfsSyncProperties -Role PrimaryComputer
```

![atualização](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_4.png)

5. No servidor AD FS configurado anteriormente como primário, abra uma janela de comando do PowerShell com privilégios elevados e execute o seguinte cmdlet:

```PowerShell
Set-AdfsSyncProperties -Role SecondaryComputer -PrimaryComputerName {FQDN}
```

![atualização](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_5.png)

6. Agora, no servidor de Federação do Windows Server 2016, abra AD FS Management. Observe que agora todos os recursos de administração são exibidos porque a função primária foi transferida para esse servidor.

![atualização](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_6.png)

7. Se você estiver atualizando um farm AD FS 2012 R2 para 2016 ou 2019, a atualização do farm exigirá que o esquema do AD esteja pelo menos no nível 85.  Para atualizar o esquema, com a mídia de instalação do Windows Server 2016, abra um prompt de comando e navegue até o diretório support\adprep. Execute o seguinte:`adprep /forestprep`

![atualização](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_7.png)

Depois que a execução for concluída`adprep/domainprep`

> [!NOTE]
> Antes de executar a próxima etapa, verifique se o Windows Server está atualizado executando Windows Update de configurações. Continue esse processo até que não existam mais atualizações.

![atualização](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_8.png)

8. Agora, no servidor do Windows Server 2016, abra o PowerShell e execute o seguinte cmdlet:


> [!NOTE]
> Todos os servidores 2012 R2 devem ser removidos do farm antes da execução da próxima etapa.

```PowerShell
Invoke-AdfsFarmBehaviorLevelRaise
```

![atualização](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_9.png)

9. Quando solicitado, digite Y. Isso começará a gerar o nível. Quando isso for concluído, o FBL será gerado com êxito.

![atualização](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_10.png)

10. Agora, se você for para AD FS Management, verá que os novos recursos foram adicionados para a versão mais recente do AD FS

![atualização](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_12.png)

11. Da mesma forma, você pode usar o cmdlet `Get-AdfsFarmInformation` do PowerShell: para mostrar o FBl atual.

![atualização](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_13.png)

12. Para atualizar os servidores WAP para o nível mais recente, em cada proxy de aplicativo Web, reconfigure o WAP executando o seguinte cmdlet do PowerShell em uma janela com privilégios elevados:

```PowerShell
$trustcred = Get-Credential -Message "Enter Domain Administrator credentials"
Install-WebApplicationProxy -CertificateThumbprint {SSLCert} -fsname fsname -FederationServiceTrustCredential $trustcred
```

Remova os servidores antigos do cluster e mantenha somente os servidores WAP que executam a versão mais recente do servidor, que foram reconfigurados acima, executando o seguinte cmdlet do PowerShell.

```PowerShell
Set-WebApplicationProxyConfiguration -ConnectedServersName WAPServerName1, WAPServerName2
```

Verifique a configuração de WAP executando o cmdlet Get-WebApplicationProxyConfiguration. O ConnectedServersName refletirá a execução do servidor do comando anterior.

```PowerShell
Get-WebApplicationProxyConfiguration
```
Para atualizar o ConfigurationVersion dos servidores WAP, execute o seguinte comando do PowerShell.

```PowerShell
Set-WebApplicationProxyConfiguration -UpgradeConfigurationVersion
```

Isso concluirá a atualização dos servidores WAP.


> [!NOTE] 
> Existe um problema de PRT conhecido no AD FS 2019 se o Windows Hello para empresas com uma confiança de certificado híbrido for executado. Você poderá receber esse erro nos logs de eventos de administrador do ADFS: Solicitação OAuth inválida recebida. O 'NAME' do cliente é proibido de acessar o recurso com o escopo 'ugs'. Para corrigir esse erro: 
> 1. Inicie o console de gerenciamento do AD FS. Procure "Serviços > Descrições de Escopo"
> 2. Clique com o botão direito do mouse em "Descrições de Escopo" e selecione "Adicionar Descrição de Escopo"
> 3. No nome, digite "ugs" e clique em Aplicar > OK
> 4. Inicie o PowerShell como Administrador
> 5. Execute o comando "Get-AdfsApplicationPermission". Procure os ScopeNames: {openid, aza} que tenham o ClientRoleIdentifier. Anote o ObjectIdentifier.
> 6. Execute o comando "Set-AdfsApplicationPermission -TargetIdentifier <ObjectIdentifier da etapa 5> -AddScope 'ugs'
> 7. Reinicie o serviço ADFS.
> 8. No cliente: Reinicie o cliente. O usuário deverá ser solicitado a provisionar o WHFB.
> 9. Se a janela de provisionamento não for exibida, será necessário coletar logs de rastreamento do NGC e solucionar problemas adicionais.
