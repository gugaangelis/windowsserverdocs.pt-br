---
title: Diretrizes de ajuste de desempenho do Windows Server 2016
description: Diretrizes de ajuste de desempenho para Windows Server 2016
ms.topic: landing-page
ms.author: phstee
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 69b65e9a7cb5e935a8c6b1a8ab500e33307a092d
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87896694"
---
# <a name="performance-tuning-guidelines-for-windows-server-2016"></a>Diretrizes de ajuste de desempenho para Windows Server 2016

Ao executar um sistema de servidor em sua organização, talvez algumas exigências comerciais não sejam atendidas usando as configurações de servidor padrão. Por exemplo, talvez você precise do consumo de energia mais baixo possível, ou da menor latência possível ou da taxa de transferência máxima possível em seu servidor. Este guia fornece um conjunto de diretrizes que você pode usar para ajustar as configurações de servidor no Windows Server 2016 e obter ganhos de eficiência de energia ou de desempenho incrementais, especialmente quando a natureza da carga de trabalho variar pouco ao longo do tempo.

É importante que seus ajustes considerem o hardware, a carga de trabalho, os orçamentos de energia e as metas de desempenho do seu servidor. Este guia descreve cada configuração e seu possível efeito para ajudar você a tomar uma decisão bem informada sobre a relevância dela para o seu sistema, carga de trabalho, desempenho e metas de uso de energia.

> [!warning]
> As configurações de Registro e os parâmetros de ajuste mudaram consideravelmente entre as versões do Windows Server. Use as diretrizes de ajustes mais recentes para evitar resultados inesperados.

## <a name="in-this-guide"></a>Neste guia
Este guia organiza as diretrizes de desempenho e de ajuste para Windows Server 2016 em três categorias de ajustes:

|Hardware do servidor | Função de servidor | Subsistema do servidor |
|:---:|:---:|:---:|
|[Considerações sobre desempenho do hardware](hardware/index.md) |[Servidores do Active Directory](role/active-directory-server/index.md) |[Gerenciamento de memória e cache](subsystem/cache-memory-management/index.md)|
|[Considerações de energia do hardware](hardware/power.md)|[Servidores de arquivos](role/file-server/index.md)|[Subsistema de rede](../../networking/technologies/network-subsystem/net-sub-performance-top.md)|
||[Servidores Hyper-V](role/hyper-v-server/index.md)|[Espaços de Armazenamento Diretos](subsystem/storage-spaces-direct/index.md)|
||[Serviços de Área de Trabalho Remota](role/remote-desktop/session-hosts.md)|[SDN (Rede definida pelo software)](subsystem/software-defined-networking/index.md)|
||[Servidores Web](role/web-server/index.md)||
||[Contêineres do Windows Server](role/windows-server-container/index.md)||


## <a name="changes-in-this-version"></a>Alterações nesta versão

### <a name="sections-added"></a>Seções adicionadas
- [Considerações de configuração de tipo de instalação do Nano Server](../../get-started/getting-started-with-nano-server.md)


- [Rede definida pelo software](subsystem/software-defined-networking/index.md), incluindo [HNV](subsystem/software-defined-networking/hnv-gateway-performance.md) e [diretrizes de configuração de gateway SLB](subsystem/software-defined-networking/slb-gateway-performance.md)

- [Espaços de Armazenamento Diretos](subsystem/storage-spaces-direct/index.md)

- [HTTP1.1 e HTTP2](role/web-server/http-performance.md)

- [Contêineres do Windows Server](role/windows-server-container/index.md)

### <a name="sections-changed"></a>Seções alteradas

- Atualizações na seção [Diretrizes do Active Directory](role/active-directory-server/index.md)

- Atualizações na seção [Diretrizes do servidor de arquivos](role/file-server/index.md)

- Atualizações na seção [Diretrizes do servidor Web](role/web-server/index.md)

- Atualizações na seção [Diretrizes de energia do hardware](hardware/power.md)

- Atualizações na seção [Diretrizes de ajuste do PowerShell](powershell/index.md)

- Atualizações consideráveis na seção [Diretrizes do Hyper-V](role/hyper-v-server/index.md)

- *Ajuste de desempenho para cargas de trabalho removido*, ponteiros para recursos relevantes adicionados ao [artigo Recursos adicionais de ajuste](additional-resources.md)

- *Remoção das seções de armazenamento dedicado* e inclusão da nova seção [Espaços de Armazenamento Diretos](subsystem/storage-spaces-direct/index.md) e conteúdo canônico da Technet

- *Remoção da seção de rede dedicada* e inclusão do conteúdo canônico da Technet
