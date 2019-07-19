---
title: Fazer upgrade de controladores de domínio para o Windows Server 2016
description: Este documento descreve como atualizar do Windows Server 2012 R2 para o Windows Server 2016
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 3e144a09c99d9b72d623956e868b3f573962ed94
ms.sourcegitcommit: 67833e36b8b2c6194a1426a974c5ad9c859fa4c9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/19/2019
ms.locfileid: "68329642"
---
# <a name="upgrade-domain-controllers-to-windows-server-2016"></a>Fazer upgrade de controladores de domínio para o Windows Server 2016

Aplica-se a: Windows Server

Este tópico fornece informações básicas sobre Active Directory Domain Services no Windows Server 2016 e explica o processo de atualização de controladores de domínio do Windows Server 2012 ou do Windows Server 2012 R2.

## <a name="pre-requisites"></a>Pré-requisitos

A maneira recomendada para atualizar um domínio é promover controladores de domínio que executam versões mais recentes do Windows Server e rebaixar os controladores de domínio mais antigos, conforme necessário. Esse método é preferível para atualizar o sistema operacional de um controlador de domínio existente. Esta lista aborda as etapas gerais a serem seguidas antes de promover um controlador de domínio que executa uma versão mais recente do Windows Server:

1. Verifique se o servidor de destino atende aos requisitos de sistema.
1. Verifique a compatibilidade do aplicativo.
1. Examine as recomendações para mudar para o Windows Server 2016
1. Verifique as configurações de segurança. Para obter mais informações, consulte [recursos preteridos e alterações de comportamento relacionadas a AD DS no Windows Server 2016](../../../get-started/deprecated-features.md).
1. Verifique a conectividade com o servidor de destino a partir do computador em que você planeja executar a instalação.
1. Verifique a disponibilidade das funções mestras de operações necessárias:
   - Para instalar o primeiro DC que executa o Windows Server 2016 em um domínio e floresta existentes, o computador em que você executa a instalação precisa de conectividade com o **mestre de esquema** para executar adprep/forestprep e o mestre de infraestrutura para executar adprep/ domainprep.
   - Para instalar o primeiro DC em um domínio em que o esquema de floresta já está estendido, você só precisa de conectividade com o **mestre de infraestrutura**.
   - Para instalar ou remover um domínio em uma floresta existente, você precisa de conectividade com o **mestre de nomeação de domínio**.
   - Qualquer instalação do controlador de domínio também requer conectividade com o **mestre de RID.**
   - Se você estiver instalando o primeiro controlador de domínio somente leitura em uma floresta existente, precisará de conectividade com o **mestre de infraestrutura** para cada partição de diretório de aplicativo, também conhecido como contexto de nomenclatura não domínio ou NDNC.

### <a name="installation-steps-and-required-administrative-levels"></a>Etapas de instalação e níveis administrativos necessários

A tabela a seguir fornece um resumo das etapas de atualização e dos requisitos de permissão para realizar essas etapas

|Ação de instalação|Requisitos de credenciais|
| ----- | ----- |
|Instalar uma nova floresta|Administrador local no servidor de destino|
|Instalar um novo domínio em uma floresta existente|Administrador corporativo|
|Instalar um controlador de domínio adicional em um domínio existente|Administradores do domínio|
|Executar adprep/forestprep|Administradores de esquema, administradores corporativos e administradores do domínio|
|Executar adprep/domainprep|Administradores do domínio|
|Executar adprep/domainprep/gpprep|Administradores do domínio|
|Executar adprep/rodcprep|Administrador corporativo|

Para obter informações adicionais sobre os novos recursos do Windows Server 2016, consulte [o que há de novo no Windows server 2016](../../../get-started/what-s-new-in-windows-server-2016.md).

## <a name="supported-in-place-upgrade-paths"></a>Caminhos de atualização in-loco com suporte

Controladores de domínio que executam versões de 64 bits do Windows Server 2012 ou Windows Server 2012 R2 podem ser atualizados para o Windows Server 2016. Somente atualizações de versão de 64 bits têm suporte porque o Windows Server 2016 só é fornecido em uma versão de 64 bits.

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

## <a name="adprep-and-domainprep"></a>Adprep e DomainPrep

Se você estiver fazendo uma atualização in-loco de um controlador de domínio existente para o sistema operacional Windows Server 2016, será necessário executar adprep/forestprep e adprep/domainprep manualmente.  Adprep/forestprep precisa ser executado apenas uma vez na floresta.  O adprep/domainprep precisa ser executado uma vez em cada domínio no qual você tenha controladores de domínio que esteja atualizando para o Windows Server 2016.

Se você estiver promovendo um novo servidor do Windows Server 2016, não precisará executá-lo manualmente.  Eles são integrados às experiências do PowerShell e do Gerenciador do Servidor.

Para obter mais informações sobre como executar a Adprep, consulte [executando adprep](https://technet.microsoft.com/library/dd464018.aspx)

## <a name="functional-level-features-and-requirements"></a>Recursos e requisitos em nível funcional

O Windows Server 2016 requer um nível funcional de floresta do Windows Server 2003. Ou seja, para poder adicionar um controlador de domínio que executa o Windows Server 2016 a uma floresta Active Directory existente, o nível funcional da floresta deve ser Windows Server 2003 ou superior. Caso a floresta contenha controladores de domínio que executam o Windows Server 2003 ou posterior, mas o nível funcional da floresta ainda seja Windows 2000, a instalação também será bloqueada.

Os controladores de domínio do Windows 2000 devem ser removidos antes da adição de controladores de domínio do Windows Server 2016 à sua floresta. Neste caso, considere o seguinte fluxo de trabalho:

1. Instale controladores de domínio que executam o Windows Server 2003 ou posterior. Esses controladores de domínio podem ser implantados em uma versão de avaliação do Windows Server. Essa etapa também requer a execução de adprep. exe para essa versão do sistema operacional como um pré-requisito.
1. Remova os controladores de domínio do Windows 2000. Especificamente, realize o rebaixamento normal ou a remoção forçada dos controladores de domínio do Windows Server 2000 do domínio e dos usuários e computadores do Active Directory utilizados para remover as contas de todos os controladores de domínio removidos.
1. Eleve o nível funcional da floresta para Windows Server 2003 ou superior.
1. Instale os controladores de domínio que executam o Windows Server 2016.
1. Remova controladores de domínio que executam versões anteriores do Windows Server.

### <a name="rolling-back-functional-levels"></a>Revertendo níveis funcionais

Depois de definir o nível funcional da floresta (FFL) com um determinado valor, não é possível reverter ou reduzir o nível funcional da floresta, com as seguintes exceções:

- Se você estiver atualizando do Windows Server 2012 R2 FFL, poderá diminuí-lo de volta para o Windows Server 2012 R2.
- Se você estiver atualizando do Windows Server 2008 R2 FFL, poderá diminuí-lo de volta para o Windows Server 2008 R2.

Depois de definir o nível funcional do domínio para um determinado valor, não será possível reverter ou reduzir o nível funcional do domínio, com as seguintes exceções:

- Ao aumentar o nível funcional do domínio para o Windows Server 2016 e se o nível funcional da floresta for o Windows Server 2012 ou inferior, você terá a opção de reverter o nível funcional do domínio para o Windows Server 2012 ou o Windows Server 2012 R2.

Para saber mais sobre os recursos disponíveis em níveis funcionais inferiores, consulte [Compreendendo os níveis funcionais AD DS (Serviços de Domínio do Active Directory)](../active-directory-functional-levels.md).

## <a name="ad-ds-interoperability-with-other-server-roles-and-windows-operating-systems"></a>Interoperabilidade do AD DS com outras funções de servidor e sistemas operacionais Windows

O AD DS não é compatível com os seguintes sistemas operacionais Windows:

- Windows MultiPoint Server
- Windows Server 2016 Essentials

O AD DS não pode ser instalado em um servidor que também executa as seguintes funções de servidor ou serviços de função:

- Microsoft Hyper-V Server 2016
- Agente de Conexão de Área de Trabalho Remota

## <a name="administration-of-windows-server-2016-servers"></a>Administração de servidores do Windows Server 2016

Use o Ferramentas de Administração de Servidor Remoto para Windows 10 para gerenciar controladores de domínio e outros servidores que executam o Windows Server 2016. Você pode executar o Ferramentas de Administração de Servidor Remoto do Windows Server 2016 em um computador que executa o Windows 10.

## <a name="step-by-step-for-upgrading-to-windows-server-2016"></a>Passo a passo para atualizar para o Windows Server 2016

Veja a seguir um exemplo simples de atualização da floresta contoso do Windows Server 2012 R2 para o Windows Server 2016.

![Atualizar, Atualização (Upgrade)](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade1.png)

1. Junte-se ao novo Windows Server 2016 à sua floresta. Reinicie quando solicitado.

   ![Atualizar, Atualização (Upgrade)](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade2.png)

1. Entre no novo Windows Server 2016 com uma conta de administrador de domínio.
1. No **Gerenciador do servidor**, em **adicionar funções e recursos**, instale **Active Directory Domain Services** no novo Windows Server 2016. Isso executará automaticamente a Adprep na floresta e no domínio do 2012 R2.

   ![Atualizar, Atualização (Upgrade)](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade3.png)

1. Em **Gerenciador do servidor**, clique no triângulo amarelo e, na lista suspensa, clique em **promover o servidor para um controlador de domínio**.

   ![Atualizar, Atualização (Upgrade)](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade4.png)

1. Na tela **configuração de implantação** , selecione **Adicionar um controlador de domínio a uma floresta existente** e clique em Avançar.

   ![Atualizar, Atualização (Upgrade)](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade5.png)

1. Na tela **Opções do controlador de domínio** , insira a senha do **modo de restauração dos serviços de diretório (DSRM)** e clique em Avançar.
1. Para o restante das telas, clique em **Avançar**.
1. Na tela de **verificação de pré-requisitos** , clique em **instalar**. Depois que a reinicialização for concluída, você poderá entrar novamente.
1. No servidor Windows Server 2012 R2, em **Gerenciador do servidor**, em ferramentas, selecione **Active Directory módulo para o Windows PowerShell**.

   ![Atualizar, Atualização (Upgrade)](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade6.png)

1. Nas janelas do PowerShell, use move-ADDirectoryServerOperationMasterRole para mover as funções FSMO. Você pode digitar o nome de cada-OperationMasterRole ou usar números para especificar as funções. Para obter mais informações [, consulte Move-ADDirectoryServerOperationMasterRole](https://technet.microsoft.com/library/hh852302.aspx)

    ``` powershell
    Move-ADDirectoryServerOperationMasterRole -Identity "DC-W2K16" -OperationMasterRole 0,1,2,3,4
    ```

    ![Atualizar, Atualização (Upgrade)](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade7.png)

1. Verifique se as funções foram movidas indo para o servidor do Windows Server 2016, em **Gerenciador do servidor**, em **ferramentas**, selecione **Active Directory módulo para o Windows PowerShell**. Use os `Get-ADDomain` cmdlets e `Get-ADForest` para exibir os detentores de função FSMO.

    ![Atualizar, Atualização (Upgrade)](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade8.png)

    ![Atualizar, Atualização (Upgrade)](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade9.png)

1. Rebaixe e remova o controlador de domínio do Windows Server 2012 R2. Para obter informações sobre como rebaixar um DC, consulte [rebaixar controladores de domínio e domínios](../../ad-ds/deploy/Demoting-Domain-Controllers-and-Domains--Level-200-.md)
1. Depois que o servidor for rebaixado e removido, você poderá aumentar os níveis funcionais da floresta e do domínio para o Windows Server 2016.

## <a name="next-steps"></a>Próximas etapas

- [Novidades na instalação e na remoção do Active Directory Domain Services](../../ad-ds/deploy/What-s-New-in-Active-Directory-Domain-Services-Installation-and-Removal.md)
- [Instalar o &#40;nível de Active Directory Domain Services 100&#41;](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md)
- [Níveis funcionais do Windows Server 2016](../../ad-ds/Windows-Server-2016-Functional-Levels.md)  
