---
title: Migrar a replicação do SYSVOL para a Replicação do DFS
ms.date: 07/02/2012
ms.prod: windows-server
ms.technology: storage
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: eb7997b4ed16812ac6b1d6c7d3ccc220f8f63ce7
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80815579"
---
# <a name="migrate-sysvol-replication-to-dfs-replication"></a>Migrar a replicação do SYSVOL para a Replicação do DFS


Atualizado: 25 de agosto de 2010

Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 e Windows Server 2008

Os controladores de domínio usam uma pasta compartilhada especial chamada SYSVOL para replicar scripts de logon e arquivos de Objeto de Política de Grupo para outros controladores de domínio. O Windows 2000 Server e o Windows Server 2003 usam o FRS (Serviço de Replicação de Arquivo) para replicar o SYSVOL, enquanto o Windows Server 2008 usa o serviço de Replicação do DFS mais recente quando está em domínios que usam o nível funcional de domínio do Windows Server 2008 e o FRS para domínios que executam níveis funcionais de domínio mais antigos.

Para usar Replicação do DFS para replicar a pasta SYSVOL, você pode criar um domínio que usa o nível funcional de domínio do Windows Server 2008 ou pode usar o procedimento que é discutido neste documento para atualizar um domínio existente e migrar a replicação para Replicação do DFS.

Este documento pressupõe que você tenha um conhecimento básico do AD DS (Active Directory Domain Services), do FRS e da Replicação do DFS (Replicação de Sistema de Arquivos Distribuído). Para obter mais informações, confira a [Visão geral do Active Directory Domain Services](https://go.microsoft.com/fwlink/?linkid=147787), a [Visão geral do FRS](https://go.microsoft.com/fwlink/?linkid=121763) ou a [Visão geral da Replicação do DFS](https://go.microsoft.com/fwlink/?linkid=121762)


> [!NOTE]
> Para baixar uma versão para impressão deste guia, acesse <a href="https://go.microsoft.com/fwlink/?linkid=150375">Guia de migração de replicação SYSVOL: FRS para Replicação do DFS</a> (https://go.microsoft.com/fwlink/?LinkId=150375)
<br>


## <a name="in-this-guide"></a>Neste guia

[Informações conceituais da migração SYSVOL](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640170(v=ws.10))

  - [Estados de migração SYSVOL](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd641052(v=ws.10))  
      
  - [Visão geral do procedimento de migração SYSVOL](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639809(v=ws.10))  
      

[Procedimento de migração SYSVOL](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639860(v=ws.10))

  - [Como migrar para o estado Preparado](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd641193(v=ws.10))  
      
  - [Como migrar para o estado Redirecionado](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd641340(v=ws.10))  
      
  - [Como migrar para o estado Eliminado](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640254(v=ws.10))  
      

[Solucionar problemas de migração SYSVOL](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640395(v=ws.10))

  - [Solucionar problemas de migração SYSVOL](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639976(v=ws.10))  
      
  - [Como reverter a migração SYSVOL para um estado estável anterior](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640509(v=ws.10))  
      

[Informações de referência da migração SYSVOL](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640293(v=ws.10))

  - [Cenários de migração SYSVOL compatíveis](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639854(v=ws.10))  
      
  - [Verificando o estado da migração SYSVOL](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639789(v=ws.10))  
      
  - [Dfsrmig](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd641227(v=ws.10))  
      
  - [Ações da ferramenta de migração SYSVOL](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639712(v=ws.10))  
      

## <a name="additional-references"></a>Referências adicionais

[Série de Migração SYSVOL: Parte 1 – Introdução ao processo de migração SYSVOL](https://go.microsoft.com/fwlink/?linkid=121756)

[Série de Migração SYSVOL: Parte 2 – Dfsrmig.exe: A ferramenta de migração SYSVOL](https://go.microsoft.com/fwlink/?linkid=121757)

[Série de Migração SYSVOL: Parte 3 – Como migrar para o estado "PREPARADO"](https://go.microsoft.com/fwlink/?linkid=121758)

[Série de Migração SYSVOL: Parte 4 – Migrar para o estado 'REDIRECIONADO'](https://go.microsoft.com/fwlink/?linkid=121759)

[Série de Migração SYSVOL: Parte 5 – Migrar para o estado 'ELIMINADO'](https://go.microsoft.com/fwlink/?linkid=121760)

[Guia passo a passo para Sistemas de Arquivos Distribuídos no Windows Server 2008](https://go.microsoft.com/fwlink/?linkid=85231)

[Referência Técnica de VPN](https://go.microsoft.com/fwlink/?linkid=121764)

