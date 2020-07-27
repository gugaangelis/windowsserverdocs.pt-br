---
title: Instalar o Servidor com Experiência Desktop
description: Explica como obter e instalar uma instalação de Servidor com Experiência Desktop
ms.prod: windows-server
ms.date: 01/18/2017
ms.technology: server-general
ms.topic: article
ms.assetid: 5b38b8a0-4dfc-4130-be00-fc58bba99595
author: jaimeo
ms.author: jaimeo
manager: dongill
ms.localizationpriority: medium
ms.openlocfilehash: a1ac967743dcb9e38cb36e5c16f7e3dd6ec0c7d2
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86953548"
---
# <a name="install-server-with-desktop-experience"></a>Instalar o Servidor com Experiência Desktop
> Aplica-se a: Windows Server 2016
  

Ao instalar o Windows Server 2016 usando o Assistente de Instalação, é possível escolher entre o **Windows Server 2016** e o **Windows Server (Servidor com Experiência Desktop)** . A opção Servidor com Experiência Desktop é o equivalente do Windows Server 2016 à opção Instalação completa disponível no Windows Server 2012 R2 com o recurso Experiência Desktop instalado. Se você não fizer uma escolha no Assistente de instalação, o **Windows Server 2016**será instalado. Essa é a opção de instalação do **Server Core**.

A opção Servidor com Experiência Desktop instala a interface de usuário padrão e todas as ferramentas, incluindo recursos de experiência do cliente que exigiam uma instalação separada no Windows Server 2012 R2. As funções e recursos do servidor são instalados com o Gerenciador de Servidores ou por outros métodos. Comparando com a opção Server Core, requer maior espaço em disco e tem mais requisitos de manutenção, portanto recomendamos que você escolha a instalação Server Core, a menos que tenha uma necessidade específica dos elementos adicionais da interface do usuário e das ferramentas gráficas de gerenciamento que estão incluídas na opção Servidor com Experiência Desktop. Se você achar que pode trabalhar sem os elementos adicionais, consulte [Install Server Core](Getting-Started-with-Server-Core.md) (Instalar o Server Core). Para uma opção ainda mais leve, consulte [Install Nano Server](Getting-Started-with-Nano-Server.md) (Instalar o Nano Server).

> [!NOTE]
>
> Ao contrário de algumas versões anteriores do Windows Server, não é possível converter entre Server Core e Server com Experiência Desktop após a instalação. Se você instalar o Server com Experiência Desktop e posteriormente decidir usar o Server Core, deverá fazer uma nova instalação.

**Interface do usuário:** interface gráfica do usuário padrão (Shell Gráfico de Servidor). O Shell Gráfico de Servidor inclui o novo shell do Windows 10. Os recursos específicos do Windows instalados por padrão com essa opção são User-Interfaces-Infra, Server-GUI-Shell, Server-GUI-Mgmt-Infra, InkAndHandwritingServices, ServerMediaFoundation e Experiência Desktop. Embora esses recursos estejam no Gerenciador do Servidor nesta versão, não há suporte para desinstalá-los e eles não estarão disponíveis em versões futuras.

**Instale, configure, desinstale funções de servidor localmente:** com o Gerenciador do Servidor ou com o Windows PowerShell

**Instale, configure, desinstale funções de servidor remotamente:** com o Gerenciador do Servidor, com o Servidor Remoto, com as RSAT ou com o Windows PowerShell.

**Console de Gerenciamento Microsoft: instalado**

## <a name="installation-scenarios"></a>Cenários de instalação

### <a name="evaluation"></a>Avaliação
Você pode obter uma cópia de avaliação de 180 dias licenciada do Windows Server em [Avaliações do Windows Server](https://www.microsoft.com/evalcenter/evaluate-windows-server-2016). Escolha **Windows Server 2016 | Opção ISO de 64 bits** para download, ou visite **Windows Server 2016 | Laboratório virtual**.

> [!IMPORTANT]  
> Para versões do Windows Server 2016 anteriores a 14393.0.161119-1705.RS1_REFRESH, somente é possível executar essa conversão de avaliação para versão comercial com o Windows Server 2016 que foi instalado usando a opção de Experiência Desktop (não a opção Server Core). A partir da versão 14393.0.161119-1705. RS1_REFRESH e versões posteriores, é possível converter edições de avaliação em versão comercial, independentemente da opção de instalação usada.


### <a name="clean-installation"></a>Instalação limpa

Para instalar a opção Servidor com Experiência Desktop a partir da mídia, insira a mídia em uma unidade, reinicie o computador e execute Setup.exe. No assistente exibido, selecione **Windows Server (Servidor com Experiência Desktop)** e, em seguida, conclua o assistente.

### <a name="upgrade"></a>Atualizar, Atualização (Upgrade)
**Atualizar** significa mudar do seu sistema operacional existente para uma versão mais recente, mantendo o mesmo hardware.

Se você já tiver uma instalação completa do produto Windows Server apropriado, é possível atualizá-la para uma instalação Servidor com Experiência Desktop na edição apropriada do Windows Server 2016, conforme indicado abaixo.

> [!IMPORTANT]  
> Nesta versão, a atualização funciona melhor em máquinas virtuais, onde os drivers de hardware específicos do OEM não são necessários para uma atualização bem-sucedida. Caso contrário, a migração é a opção recomendada.  

- Não há suporte para atualizações in-loco de arquiteturas de 32 bits para 64 bits. Todas as edições do Windows Server 2016 são apenas 64 bits.
- Não há suporte para atualizações in-loco de um idioma para outro.
- Se o servidor for um controlador de domínio, confira [Atualizar controladores de domínio para o Windows Server 2012 R2 e o Windows Server 2012](../identity/ad-ds/deploy/upgrade-domain-controllers-to-windows-server-2012-r2-and-windows-server-2012.md) para saber mais.
- Não há suporte para atualizações de versões de pré-lançamento (visualizações) do Windows Server 2016. Realize uma instalação limpa do Windows Server 2016.
- Não há suporte para atualizações que alternam da instalação Server Core para uma instalação de Servidor com Desktop (e vice-versa).

Se a sua versão atual não aparecer na coluna à esquerda, a atualização para esta versão do Windows Server 2016 não tem suporte.

Se aparecer mais de uma edição na coluna à direita, a atualização para **qualquer** edição da mesma versão inicial tem suporte.

|Se você está executando esta edição:|É possível atualizar para estas edições:|  
|-------------------|----------|  
|Windows Server 2012 Standard|Windows Server 2016 Standard ou Datacenter|
|Windows Server 2012 Datacenter|Windows Server 2016 Datacenter|
|Windows Server 2012 R2 Standard|Windows Server 2016 Standard ou Datacenter|
|Windows Server 2012 R2 Datacenter|Windows Server 2016 Datacenter|
|Windows Server 2012 R2 Essentials|Windows Server 2016 Essentials|
|Windows Storage Server 2012 Standard|Windows Storage Server 2016 Standard|
|Grupo de trabalho do Windows Storage Server 2012|Grupo de trabalho do Windows Storage Server 2016|
|Windows Storage Server 2012 R2 Standard|Windows Storage Server 2016 Standard|
|Windows Storage Server 2012 R2 Workgroup|Grupo de trabalho do Windows Storage Server 2016|

Para muitas opções adicionais para mover para o Windows Server 2016, como a conversão da licença entre edições licenciadas por volume, edições de avaliação e outras, confira os detalhes em [Opções de atualização](Supported-Upgrade-Paths.md).

### <a name="migration"></a>Migração
**Migração** significa mudar do seu sistema operacional existente para o Windows Server 2016 executando uma instalação limpa em um conjunto diferente de hardware ou máquina virtual e, em seguida, transferir as cargas de trabalho do servidor mais antigo para o novo servidor. A migração, que pode variar consideravelmente dependendo das funções de servidor instaladas, é discutida em detalhes em [Instalação, atualização e migração do Windows Server](./installation-and-upgrade.md).

A capacidade de migrar varia entre as diferentes funções de servidor. A grade a seguir explica suas opções de migração e atualização de funções de servidor, especificamente ao mover para o Windows Server 2016. Para obter guias de migração sobre funções específicas, visite [Migrar funções e recursos no Windows Server](./migrate-roles-and-features.md). Para saber mais sobre instalação e atualização, confira [Instalação, atualização e migração do Windows Server](./installation-and-upgrade.md).

|Função de servidor|Pode ser atualizado do Windows Server 2012 R2?|Pode ser atualizado do Windows Server 2012?|Migração compatível?|A migração pode ser concluída sem tempo de inatividade?|  
|-------------------|----------|--------------|--------------|----------|  
|Serviços de Certificados do Active Directory|    Sim|    Sim|    Sim|    Não|
|Active Directory Domain Services|    Sim|    Sim|    Sim|    Sim|
|Serviços de Federação do Active Directory|    Não|    Não|    Sim|    Não (os novos nós precisam ser adicionados ao farm)|
|Active Directory Lightweight Directory Services|    Sim|    Sim|    Sim|    Sim|
|Active Directory Rights Management Services|    Sim|    Sim|    Sim|    Não|
|Cluster de failover|Sim com o processo [Atualização sem interrupção do sistema operacional do cluster](../failover-clustering/cluster-operating-system-rolling-upgrade.md)contém o nó Pause-Drain, Evict, atualização para o Windows Server 2016 e o reingresso no cluster original. Sim, quando o servidor é removido pelo cluster para atualização e adicionado a um cluster diferente.|Não enquanto o servidor fizer parte de um cluster. Sim, quando o servidor é removido pelo cluster para atualização e adicionado a um cluster diferente.    |Sim|Não para Clusters de Failover no Windows Server 2012. Sim para Clusters de Failover do Windows Server 2012 R2 com máquinas virtuais do Hyper-V ou Clusters de Failover do Windows Server 2012 R2 executando a função Servidor de Arquivos de Escalabilidade Horizontal. confira [Atualização sem interrupção do sistema operacional do cluster](../failover-clustering/cluster-operating-system-rolling-upgrade.md).|
|Serviços de Arquivo e Armazenamento|    Sim|    Sim|    Varia de acordo com o sub-recurso|    Não|
|Serviços de impressão e fax|    Não|    Não|    Sim (Printbrm.exe)|    Não|
|Serviços da área de trabalho Remota|    Sim, para todas as subfunções, mas o modo de farm misto não é compatível|    Sim, para todas as subfunções, mas o modo de farm misto não é compatível|    Sim|    Não|
|Servidor Web (IIS)|    Sim|    Sim|    Sim|    Não|
|Experiência do Windows Server Essentials|    Sim|    N/D, novo recurso|    Sim|    Não|
|Windows Server Update Services|    Sim|    Sim|    Sim|    Não|
|Pastas de trabalho|    Sim|    Sim|    Sim|    Sim do cluster do WS 2012 R2 ao usar a [Atualização sem interrupção do sistema operacional do cluster](../failover-clustering/cluster-operating-system-rolling-upgrade.md).|

> [!IMPORTANT]  
> Após a conclusão da instalação e imediatamente depois de ter instalado todas as funções e recursos de servidor necessários, procure por e instale as atualizações disponíveis para o Windows Server 2016 usando o Windows Update ou outros métodos de atualização.

---------------------------------------
Se você precisar de uma opção de instalação diferente, ou se concluir a instalação e estiver pronto para implantar cargas de trabalho específicas, [volte para a página principal do Windows Server 2016](Windows-Server-2016.md).
