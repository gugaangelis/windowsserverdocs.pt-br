---
title: Fazer upgrade de controladores de domínio para o Windows Server 2016
description: Este documento descreve como atualizar do Windows Server 2012 R2 para Windows Server 2016
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: fe6fb196c996d4d95c6b58d1ab77591602e143d9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868417"
---
# <a name="upgrade-domain-controllers-to-windows-server-2016"></a>Fazer upgrade de controladores de domínio para o Windows Server 2016

Aplica-se a: Windows Server 2016

Este tópico fornece informações básicas sobre serviços de domínio do Active Directory no Windows Server 2016 e explica o processo de atualização de controladores de domínio do Windows Server 2012 ou Windows Server 2012 R2. 

## <a name="pre-requisites"></a>Pré-requisitos
A maneira recomendada para um domínio de atualização é promover controladores de domínio que executam versões mais recentes do Windows Server e rebaixar os controladores de domínio anteriores conforme necessário. Esse método é preferível para atualizar o sistema operacional de um controlador de domínio existente. Essa lista aborda etapas gerais a seguir antes de promover um controlador de domínio que executa uma versão mais recente do Windows Server: 

1.  Verifique se o servidor de destino atende aos requisitos de sistema. 
2.  Verificar a compatibilidade de aplicativo. 
3.  Analise as recomendações para mudar para o Windows Server 2016 
4.  Verifique as configurações de segurança. Para obter mais informações, consulte [recursos preteridos e alterações de comportamento relacionam ao AD DS no Windows Server 2016](../../../get-started\deprecated-features.md). 
5.  Verifique a conectividade com o servidor de destino a partir do computador em que você planeja executar a instalação. 
6.  Verifique a disponibilidade das funções mestras de operações necessárias: 
    - Para instalar o primeiro controlador de domínio que executa o Windows Server 2016 em um domínio e floresta existentes, a máquina em que você executa a instalação precisa de conectividade com o **mestre de esquema** para executar adprep/forestprep e com o mestre de infraestrutura para executar adprep/domainprep. 
    - Para instalar o primeiro controlador de domínio em um domínio onde o esquema de árvore já está estendido, é necessário apenas ter conectividade com o mestre de infraestrutura. 
    - Para instalar ou remover um domínio em uma floresta existente, você precisa de conectividade com o **mestre de nomeação de domínio**. 
    - Qualquer instalação do controlador de domínio também requer conectividade com o **mestre de RID.** 
    - Caso esteja instalando o primeiro controlador de domínio somente leitura em uma floresta existente, será necessário ter conectividade com o mestre de infraestrutura para cada partição do diretório de aplicativos, também conhecida como um contexto de nomeação não relacionado a domínio ou NDNC. 

### <a name="installation-steps-and-required-administrative-levels"></a>Etapas de instalação e níveis administrativos necessários
A tabela a seguir fornece um resumo das etapas de atualização e os requisitos de permissão para realizar essas etapas

|Ação de instalação|Requisitos de credenciais|
| ----- | ----- |
|Instalar uma nova floresta|Administrador local no servidor de destino|
|Instalar um novo domínio em uma floresta existente|Administrador corporativo|
|Instalar um controlador de domínio adicional em um domínio existente|Administradores do domínio|
|Executar adprep/forestprep|Administradores de esquema, administradores corporativos e administradores do domínio|
|Executar adprep/domainprep|Administradores do domínio|
|Executar adprep/domainprep/gpprep|Administradores do domínio|
|Executar adprep/rodcprep|Administrador corporativo|

Para obter informações adicionais sobre os novos recursos no Windows Server 2016, consulte [o que há de novo no Windows Server 2016](../../../get-started/what-s-new-in-windows-server-2016.md).



## <a name="supported-in-place-upgrade-paths"></a>Caminhos de atualização in-loco com suporte
Controladores de domínio que executam versões de 64 bits do Windows Server 2012 ou Windows Server 2012 R2 podem ser atualizados para o Windows Server 2016. Somente as atualizações de versão de 64 bits têm suporte porque o Windows Server 2016 é fornecido apenas em uma versão de 64 bits.

|Se você está executando esta edição:|É possível atualizar para estas edições:|
| ----- | ----- |   
|Windows Server 2012 Standard|Windows Server 2016 Standard ou Datacenter|   
|Windows Server 2012 Datacenter|Windows Server 2016 Datacenter| 
|Windows Server 2012 R2 Standard|Windows Server 2016 Standard ou Datacenter|    
|Windows Server 2012 R2 Datacenter|Windows Server 2016 Datacenter|  
|Windows Server 2012 R2 Essentials|Windows Server 2016 Essentials|  
|Windows Storage Server 2012 Standard|Windows Storage Server 2016 Standard| 
|Grupo de trabalho do Windows Storage Server 2012|Grupo de trabalho do Windows Storage Server 2016|   
|Windows Storage Server 2012 R2 Standard|Windows Storage Server 2016 Standard|  
|Windows Storage Server 2012 R2 Workgroup|Grupo de trabalho do Windows Storage Server 2016|    

Para obter mais informações sobre caminhos de atualização com suporte, consulte [caminhos de atualização com suporte](../../../get-started/supported-upgrade-paths.md)

## <a name="adprep-and-domainprep"></a>Adprep e Domainprep
Se você estiver fazendo uma atualização in-loco de um controlador de domínio existente para o sistema operacional Windows Server 2016, será preciso executar adprep/forestprep e adprep /domainprep manualmente.  O adprep /forestprep deve ser executado apenas uma vez na floresta.  Adprep /domainprep deve ser executado uma vez em cada domínio em que você tenha os controladores de domínio que você está atualizando para o Windows Server 2016.

Se você está promovendo um novo servidor do Windows Server 2016 que não é necessário executá-los manualmente.  Eles são integrados do PowerShell e experiências do Gerenciador do servidor.

Para obter mais informações sobre como executar o adprep consulte [executando Adprep](https://technet.microsoft.com/library/dd464018.aspx) 


## <a name="functional-level-features-and-requirements"></a>Recursos e requisitos em nível funcional
Windows Server 2016 requer um nível funcional de floresta do Windows Server 2003. Ou seja, antes de adicionar um controlador de domínio que executa o Windows Server 2016 em uma floresta do Active Directory existente, o nível funcional da floresta deve ser Windows Server 2003 ou superior. Caso a floresta contenha controladores de domínio que executam o Windows Server 2003 ou posterior, mas o nível funcional da floresta ainda seja Windows 2000, a instalação também será bloqueada. 

Controladores de domínio do Windows 2000 devem ser removidos antes de adicionar controladores de domínio do Windows Server 2016 à sua floresta. Neste caso, considere o seguinte fluxo de trabalho: 


1. Instale controladores de domínio que executam o Windows Server 2003 ou posterior. Esses controladores de domínio podem ser implantados em uma versão de avaliação do Windows Server. Esta etapa também requer a execução adprep.exe para aquela versão do sistema operacional como um pré-requisito. 
2.  Remova os controladores de domínio do Windows 2000. Especificamente, realize o rebaixamento normal ou a remoção forçada dos controladores de domínio do Windows Server 2000 do domínio e dos usuários e computadores do Active Directory utilizados para remover as contas de todos os controladores de domínio removidos. 
3.  Eleve o nível funcional da floresta para Windows Server 2003 ou superior. 
4.  Instale controladores de domínio que executam o Windows Server 2016. 
5.  Remova controladores de domínio que executam versões anteriores do Windows Server. 

### <a name="rolling-back-functional-levels"></a>Revertendo níveis funcionais

Depois de definir o nível funcional da floresta (FFL) para um valor determinado, não é possível reverter ou diminuir o nível funcional de floresta, com as seguintes exceções: 

- Se você estiver atualizando do Windows Server 2012 R2 FFL, você poderá reduzi-lo de volta para o Windows Server 2012 R2. 
- Se você estiver atualizando do Windows Server 2008 R2 FFL, você poderá reduzi-lo de volta para o Windows Server 2008 R2.

Depois de definir o nível funcional de domínio para um valor determinado, não é possível reverter ou diminuir o nível funcional de domínio, com as seguintes exceções: 

- Quando você aumentar o nível funcional do domínio para o Windows Server 2016 e se o nível funcional de floresta for Windows Server 2012 ou inferior, você tem a opção de reverter o nível funcional do domínio de volta para o Windows Server 2012 ou Windows Server 2012 R2 

Para saber mais sobre os recursos disponíveis em níveis funcionais inferiores, consulte [Compreendendo os níveis funcionais AD DS (Serviços de Domínio do Active Directory)](../active-directory-functional-levels.md). 
 
## <a name="ad-ds-interoperability-with-other-server-roles-and-windows-operating-systems"></a>Interoperabilidade do AD DS com outras funções de servidor e sistemas operacionais Windows
O AD DS não é compatível com os seguintes sistemas operacionais Windows: 


- Windows MultiPoint Server 
- Windows Server 2016 Essentials 

O AD DS não pode ser instalado em um servidor que também executa as seguintes funções de servidor ou serviços de função: 

- Microsoft Hyper-V Server 2016
- Agente de Conexão de Área de Trabalho Remota 

## <a name="administration-of-windows-server-2016-servers"></a>Administração de servidores do Windows Server 2016
Use as ferramentas de administração do Remote Server para Windows 10 para gerenciar controladores de domínio e outros servidores que executam o Windows Server 2016. Você pode executar o Windows Server 2016 administração ferramentas de servidor remoto em um computador que executa o Windows 10. 

## <a name="step-by-step-for-upgrading-to-windows-server-2016"></a>Passo a passo para atualizar para o Windows Server 2016
O exemplo a seguir é um exemplo simples de atualizar a floresta da Contoso do Windows Server 2012 R2 para o Windows Server 2016.

![Atualizar, Atualização (Upgrade)](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade1.png)

1.  Junte-se o novo Windows Server 2016 à sua floresta. Reinicie quando solicitado. 
![Atualizar](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade2.png)
2.  Entrar para o novo Windows Server 2016 com uma conta de administrador de domínio.
3.  No **Gerenciador de servidores**, em **para adicionar funções e recursos**, instalar **serviços de domínio do Active Directory** o novo Windows Server 2016. Isso será executado automaticamente adprep no 2012 R2 floresta e domínio.
![Atualizar](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade3.png) 
4.  Na **Gerenciador de servidores**, clique no triângulo amarelo e, na lista suspensa, clique em **promover o servidor para um controlador de domínio**. 
![Atualizar](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade4.png)
5.  Sobre o **configuração de implantação** tela, selecione **adicionar um controlador de domínio a uma floresta existente** e clique em Avançar. 
![Atualizar](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade5.png)
6.  Sobre o **opções do controlador de domínio** tela, insira o **Directory Services Restore DSRM (modo)** senha e clique em próximo. 
7.  Para o restante das telas, clique em **próxima**. 
8.  Sobre o **Verificar pré-requisitos** tela, clique em **instalar**. Depois que a reinicialização for concluída, você pode entrar novamente.
9.  No servidor Windows Server 2012 R2, no **Gerenciador de servidores**, em ferramentas, selecione **módulo Active Directory para Windows PowerShell**. 
![Atualizar](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade6.png)
10. No windows PowerShell use Move-ADDirectoryServerOperationMasterRole para mover as funções FSMO. Você pode digitar o nome de cada OperationMasterRole - ou usa números para especificar as funções. Para obter mais informações, consulte [ADDirectoryServerOperationMasterRole de movimentação](https://technet.microsoft.com/library/hh852302.aspx)

   ``` powershell
   Move-ADDirectoryServerOperationMasterRole -Identity "DC-W2K16" -OperationMasterRole 0,1,2,3,4
   ```

   ![Atualizar, Atualização (Upgrade)](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade7.png)</br>
11. Verifique se as funções foram movidas indo para o servidor do Windows Server 2016, em **Gerenciador de servidores**, em **ferramentas**, selecione **módulo Active Directory para Windows PowerShell**. Use o `Get-ADDomain` e `Get-ADForest` cmdlets para exibir os detentores de função FSMO.
![Atualizar](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade8.png)
![atualizar](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade9.png)
12. Rebaixar e remover o controlador de domínio do Windows Server 2012 R2. Para obter informações sobre o rebaixamento de um controlador de domínio, consulte [rebaixar controladores de domínio e domínios](../../ad-ds/deploy/Demoting-Domain-Controllers-and-Domains--Level-200-.md)
13. Depois que o servidor é rebaixou e removeu você pode elevar os níveis funcionais de domínio para o Windows Server 2016 e funcional da floresta.


## <a name="next-steps"></a>Próximas etapas
-   [O que há de novo no domínio do Active Directory serviços de instalação e remoção](../../ad-ds/deploy/What-s-New-in-Active-Directory-Domain-Services-Installation-and-Removal.md)  
-   [Instalar serviços de domínio do Active Directory &#40;nível 100&#41;](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md)     
-   [Níveis funcionais do Windows Server 2016](../../ad-ds/Windows-Server-2016-Functional-Levels.md)  
