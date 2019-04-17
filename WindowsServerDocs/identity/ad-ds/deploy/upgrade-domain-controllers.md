---
title: "Atualizar controladores de domínio para o Windows Server 2016"
description: Este documento descreve como atualizar do Windows Server 2012 R2 para Windows Server 2016
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 187972c2f44a4d7f91b1b3ac1c905529564cfa6d
ms.sourcegitcommit: f748c6c4ce700b0787ffdd1fca620c21c4331fd2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/21/2017
---
# <a name="upgrade-domain-controllers-to-windows-server-2016"></a>Atualizar controladores de domínio para o Windows Server 2016

Aplica-se a: Windows Server 2016

Este tópico fornece informações básicas sobre serviços de domínio do Active Directory no Windows Server 2016 e explica o processo para atualizar os controladores de domínio do Windows Server 2012 ou Windows Server 2012 R2. 

## <a name="pre-requisites"></a>Pré-requisitos
A maneira recomendada de um domínio de atualização é promover controladores de domínio que executam versões mais recentes do Windows Server e rebaixar os controladores de domínio mais antigos conforme necessário. Esse método é preferível atualizando o sistema operacional de um controlador de domínio existente. Essa lista abrange as etapas gerais a serem seguidas antes de promover um controlador de domínio que executa uma versão mais recente do Windows Server: 

1.  Verifique se que o servidor de destino atende aos requisitos do sistema. 
2.  Verificar a compatibilidade de aplicativos. 
3.  Examinar recomendações para mover para o Windows Server 2016 
4.  Verifique se as configurações de segurança. Para obter mais informações, consulte [obsoleto recursos e as mudanças de comportamento relacionam ao AD DS no Windows Server 2016](../../../get-started\deprecated-features.md). 
5.  Verifique a conectividade para o servidor de destino do computador onde você pretende executar a instalação. 
6.  Verifique a disponibilidade das funções de mestre de operações necessárias: 
    - Para instalar o primeiro controlador de domínio que executa o Windows Server 2016 em uma floresta e domínio existente, a máquina em que você executar a instalação precisa ter conectividade com a **mestre de esquema** para executar adprep /forestprep e o mestre de infraestrutura para executar adprep /domainprep. 
    - Para instalar o primeiro controlador de domínio em um domínio em que o esquema de floresta já foi estendido, você só precisa de conectividade com o mestre de infraestrutura. 
    - Para instalar ou remover um domínio em uma floresta existente, você precisa de conectividade com a **mestre de nomeação de domínio**. 
    - Qualquer instalação de controlador de domínio também exige conectividade com a **RID mestre.** 
    - Se você estiver instalando o primeiro controlador de domínio somente leitura em uma floresta existente, você precisa conectividade com o mestre de infraestrutura para cada partição de diretório do aplicativo, também conhecido como um contexto de nomenclatura não do domínio ou NDNC. 

### <a name="installation-steps-and-required-administrative-levels"></a>Etapas de instalação e níveis administrativos necessários
A tabela a seguir fornece um resumo das etapas atualização e os requisitos de permissão para realizar essas etapas

|Ação de instalação|Requisitos de credenciais|
| ----- | ----- |
|Instalar uma nova floresta|Administrador local no servidor de destino|
|Instalar um novo domínio em uma floresta existente|Administradores corporativos|
|Instalar um controlador de domínio adicional em um domínio existente|Administradores de domínio|
|Executar adprep /forestprep|Administradores de esquema, administradores de empresa e administradores de domínio|
|Executar adprep /domainprep|Administradores de domínio|
|Executar adprep /domainprep /gpprep|Administradores de domínio|
|Executar adprep /rodcprep|Administradores corporativos|

Para obter informações adicionais sobre os novos recursos no Windows Server 2016, consulte [o que há de novo no Windows Server 2016](../../../get-started/what-s-new-in-windows-server-2016.md).



## <a name="supported-in-place-upgrade-paths"></a>Suporte a caminhos de atualização in-loco
Controladores de domínio que executam versões de 64 bits do Windows Server 2012 ou Windows Server 2012 R2 podem ser atualizados para Windows Server 2016. Somente atualizações de versão de 64 bits são compatíveis, como o Windows Server 2016 apenas vem em uma versão de 64 bits.

|Se você estiver executando essa edição:|Você pode atualizar essas edições:|
| ----- | ----- |   
|Windows Server 2012 Standard|Windows Server 2016 Standard ou Datacenter|   
|O Windows Server 2012 Datacenter|O Windows Server 2016 Datacenter| 
|Windows Server 2012 R2 Standard|Windows Server 2016 Standard ou Datacenter|    
|O Windows Server 2012 R2 Datacenter|O Windows Server 2016 Datacenter|  
|Windows Server 2012 R2 Essentials|Windows Server 2016 Essentials|  
|Windows Storage Server 2012 Standard|Windows Storage Server 2016 Standard| 
|Windows Storage Server 2012 Workgroup|Grupo de trabalho do Windows Storage Server 2016|   
|Windows Storage Server 2012 R2 Standard|Windows Storage Server 2016 Standard|  
|O grupo de trabalho do Windows Storage Server 2012 R2|Grupo de trabalho do Windows Storage Server 2016|    

Para saber mais sobre os caminhos de atualização com suporte, consulte [caminhos de atualização do suporte](../../../get-started/supported-upgrade-paths.md)

## <a name="adprep-and-domainprep"></a>Adprep e Domainprep
Se você estiver fazendo uma atualização in-loco do controlador de domínio existente para o sistema operacional Windows Server 2016, você precisará executar adprep /forestprep e /domainprep adprep manualmente.  Adprep /forestprep precisa ser executado apenas uma vez na floresta.  Adprep /domainprep precisa ser executado uma vez em cada domínio em que você tiver controladores de domínio que você estiver atualizando para o Windows Server 2016.

Se você está promovendo um novo servidor Windows Server 2016, que você não precisa executar essas manualmente.  Esses são integrados do PowerShell e experiências de Gerenciador do servidor.

Para obter mais informações sobre como executar adprep veja [Adprep em execução](https://technet.microsoft.com/library/dd464018.aspx) 


## <a name="functional-level-features-and-requirements"></a>Requisitos e recursos de nível funcionais
Windows Server 2016 requer um nível funcional da floresta do Windows Server 2003. Ou seja, antes de adicionar um controlador de domínio que executa o Windows Server 2016 para uma floresta do Active Directory existente, o nível funcional da floresta deve ser Windows Server 2003 ou posterior. Se a floresta contém controladores de domínio que executam o Windows Server 2003 ou posterior mas floresta funcional nível ainda é Windows 2000, a instalação também está bloqueada. 

Controladores de domínio do Windows 2000 devem ser removidos antes da adição de controladores de domínio do Windows Server 2016 à floresta. Nesse caso, considere seguir o fluxo de trabalho: 


1. Instale os controladores de domínio que executam o Windows Server 2003 ou posterior. Esses controladores de domínio podem ser implantadas em uma versão de avaliação do Windows Server. Esta etapa também exige executando adprep.exe para essa versão do sistema operacional como pré-requisito. 
2.  Remova os controladores de domínio do Windows 2000. Especificamente, normalmente rebaixar ou remover forçadamente os controladores de domínio do Windows Server 2000 do domínio e usados Active Directory usuários e computadores para remover as contas de controlador de domínio para todos os controladores de domínio removido. 
3.  Aumente o nível funcional da floresta para o Windows Server 2003 ou posterior. 
4.  Instale os controladores de domínio que executam o Windows Server 2016. 
5.  Remova os controladores de domínio que executam versões anteriores do Windows Server. 

### <a name="rolling-back-functional-levels"></a>Reversão níveis funcionais

Depois de definir o nível funcional da floresta (FFL) para um determinado valor, você não pode reverter ou diminuir o nível funcional da floresta, com as seguintes exceções: 

- Se você estiver atualizando do Windows Server 2012 R2 FFL, você pode reduzi-lo voltar ao Windows Server 2012 R2. 
- Se você estiver atualizando do Windows Server 2008 R2 FFL, você pode reduzi-la novamente ao Windows Server 2008 R2.

Depois de definir o nível funcional do domínio para um determinado valor, você não pode reverter ou diminuir o nível funcional de domínio, com as seguintes exceções: 

- Quando você elevar o nível funcional de domínio para Windows Server 2016 e se o nível funcional da floresta for Windows Server 2012 ou inferior, você terá a opção de reverter o nível funcional do domínio de volta para o Windows Server 2012 ou Windows Server 2012 R2 

Para obter mais informações sobre os recursos que estão disponíveis em níveis inferiores funcionais, consulte [níveis funcionais de Noções básicas sobre serviços Active Directory domínio (AD DS)](../active-directory-functional-levels.md). 
 
## <a name="ad-ds-interoperability-with-other-server-roles-and-windows-operating-systems"></a>Interoperabilidade do AD DS com outras funções de servidor e sistemas operacionais Windows
AD DS não é compatível com os seguintes sistemas operacionais Windows: 


- Windows MultiPoint Server 
- Windows Server 2016 Essentials 

AD DS não pode ser instalado em um servidor que também executa as seguintes funções de servidor ou serviços de função: 

- Microsoft Hyper-V Server 2016
- Agente de Conexão de área de trabalho remota 

## <a name="administration-of-windows-server-2016-servers"></a>Administração de servidores do Windows Server 2016
Use as ferramentas de administração do remoto servidor para Windows 10 para gerenciar controladores de domínio e outros servidores que executam o Windows Server 2016. Você pode executar o Windows Server 2016 administração ferramentas de servidor remoto em um computador que executa o Windows 10. 

## <a name="step-by-step-for-upgrading-to-windows-server-2016"></a>Passo a passo para atualizar para o Windows Server 2016
A seguir está um exemplo simple de atualização da floresta Contoso do Windows Server 2012 R2 para o Windows Server 2016.

![Atualização](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade1.png)

1.  Ingresse o novo Windows Server 2016 floresta. Reinicie quando solicitado. 
![Atualização](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade2.png)
2.  Entrar para o novo Windows Server 2016 com uma conta de administrador do domínio.
3.  Em **Gerenciador do servidor**, em **adicionar funções e recursos**, instalar **Active Directory Domain Services** sobre o novo Windows Server 2016. Isso será executado automaticamente adprep em 2012 R2 floresta e domínio.
![Atualização](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade3.png) 
4.  Em **Gerenciador do servidor**, clique no triângulo amarelo e na lista suspensa, clique em **promover o servidor para um controlador de domínio**. 
![Atualização](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade4.png)
5.  Sobre o **implantação configuração** e selecione **adicionar um controlador de domínio a uma floresta existente** e clique em Avançar. 
![Atualização](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade5.png)
6.  Sobre o **opções de controlador de domínio** de tela, insira o **modo de restauração de serviços de diretório (DSRM)** senha e clique em próximo. 
7.  Para o restante das telas de clique **próxima**. 
8.  Sobre o **Verificar pré-requisito** de tela, clique em **instalar**. Assim que tiver concluído a reinicialização você pode entrar novamente.
9.  No servidor Windows Server 2012 R2, no **Gerenciador do servidor**, em ferramentas, selecione **módulo Active Directory para Windows PowerShell**. 
![Atualização](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade6.png)
10. No windows PowerShell use Move-ADDirectoryServerOperationMasterRole para mover as funções FSMO. Você pode digitar o nome de cada OperationMasterRole - ou usar números para especificar as funções. Os números. Para obter mais informações, consulte [ADDirectoryServerOperationMasterRole de movimento](https://technet.microsoft.com/library/hh852302.aspx)

``` powershell
Move-ADDirectoryServerOperationMasterRole -Identity "DC-W2K16" -OperationMasterRole 0,1,2,3,4
```

![Atualização](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade7.png)</br>
11. Verifique se as funções foram movidas indo para o servidor Windows Server 2016, em **Gerenciador do servidor**, em **ferramentas**, selecione **módulo Active Directory para Windows PowerShell**. Use o `Get-ADDomain` e `Get-ADForest` cmdlets para exibir os MESTRES de operações.
![Atualizar] (media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade8.png)
! [Atualizar](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade9.png)
12. Rebaixe e remover o controlador de domínio do Windows Server 2012 R2. Para obter informações sobre Rebaixando um controlador de domínio, consulte [rebaixando controladores de domínio e domínios](../../ad-ds/deploy/Demoting-Domain-Controllers-and-Domains--Level-200-.md)
13. Depois que o servidor é rebaixado e removido, você pode emitir funcional da floresta e níveis funcionais de domínio para o Windows Server 2016.


## <a name="next-steps"></a>Próximas etapas
-   [O que há de novo no domínio do Active Directory serviços de instalação e remoção](../../ad-ds/deploy/What-s-New-in-Active-Directory-Domain-Services-Installation-and-Removal.md)  
-   [Instalar os serviços de domínio do Active Directory & #40; Nível de 100 & #41;](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md)     
-   [Níveis funcionais do Windows Server 2016](../../ad-ds/Windows-Server-2016-Functional-Levels.md)  
