---
title: Migrar a replicação do SYSVOL para a replicação do DFS
ms.date: 07/02/2012
ms.prod: windows-server
ms.technology: storage
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: 56877bc5ddb3ea5f24f4057051775094654d8bbf
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386038"
---
# <a name="migrate-sysvol-replication-to-dfs-replication"></a>Migrar a replicação do SYSVOL para a replicação do DFS


Atualizado: 25 de agosto de 2010

Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 e Windows Server 2008

Os controladores de domínio usam uma pasta compartilhada especial chamada SYSVOL para replicar scripts de logon e Política de Grupo arquivos de objeto para outros controladores de domínio. O Windows 2000 Server e o Windows Server 2003 usam o FRS (serviço de replicação de arquivo) para replicar o SYSVOL, enquanto o Windows Server 2008 usa o serviço de Replicação do DFS mais recente quando estiver em domínios que usam o nível funcional de domínio do Windows Server 2008 e o FRS para domínios que Execute níveis funcionais de domínio mais antigos.

Para usar Replicação do DFS para replicar a pasta SYSVOL, você pode criar um novo domínio que usa o nível funcional de domínio do Windows Server 2008 ou pode usar o procedimento que é discutido neste documento para atualizar um domínio existente e migrar a replicação para o Replicação do DFS.

Este documento pressupõe que você tenha um conhecimento básico de Active Directory Domain Services (AD DS), FRS e replicação de Sistema de Arquivos Distribuído (Replicação do DFS). Para obter mais informações, consulte [visão geral do Active Directory Domain Services](http://go.microsoft.com/fwlink/?linkid=147787), [visão geral do FRS](http://go.microsoft.com/fwlink/?linkid=121763)ou [visão geral do replicação do DFS](http://go.microsoft.com/fwlink/?linkid=121762)


> [!NOTE]
> Para baixar uma versão imprimível deste guia, vá para <a href="http://go.microsoft.com/fwlink/?linkid=150375">o guia de migração de replicação do SYSVOL: FRS para Replicação do DFS</a> (http://go.microsoft.com/fwlink/?LinkId=150375)
<br>


## <a name="in-this-guide"></a>Neste guia

[Informações conceituais de migração do SYSVOL](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640170(v=ws.10))

  - [Estados de migração do SYSVOL](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd641052(v=ws.10))  
      
  - [Visão geral do procedimento de migração do SYSVOL](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639809(v=ws.10))  
      

[Procedimento de migração de SYSVOL](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639860(v=ws.10))

  - [Migrando para o estado preparado](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd641193(v=ws.10))  
      
  - [Migrando para o estado Redirecionado](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd641340(v=ws.10))  
      
  - [Migrando para o estado eliminado](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640254(v=ws.10))  
      

[Solução de problemas de migração do SYSVOL](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640395(v=ws.10))

  - [Solucionando problemas de migração do SYSVOL](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639976(v=ws.10))  
      
  - [Revertendo a migração do SYSVOL para um estado estável anterior](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640509(v=ws.10))  
      

[Informações de referência de migração do SYSVOL](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640293(v=ws.10))

  - [Cenários de migração de SYSVOL com suporte](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639854(v=ws.10))  
      
  - [Verificando o estado da migração do SYSVOL](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639789(v=ws.10))  
      
  - [DFSRMIG](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd641227(v=ws.10))  
      
  - [Ações da ferramenta de migração do SYSVOL](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639712(v=ws.10))  
      

## <a name="additional-references"></a>Referências adicionais

[Série de migração do SYSVOL: Parte 1 – Introdução ao processo de migração do SYSVOL](http://go.microsoft.com/fwlink/?linkid=121756)

[Série de migração do SYSVOL: Parte 2 – Dfsrmig. exe: A ferramenta de migração do SYSVOL](http://go.microsoft.com/fwlink/?linkid=121757)

[Série de migração do SYSVOL: Parte 3 – migrando para o estado ' preparado '](http://go.microsoft.com/fwlink/?linkid=121758)

[Série de migração do SYSVOL: Parte 4 – migrando para o estado "Redirecionado"](http://go.microsoft.com/fwlink/?linkid=121759)

[Série de migração do SYSVOL: Parte 5 – migrando para o estado "ELIMINAdo"](http://go.microsoft.com/fwlink/?linkid=121760)

[Guia passo a passo para sistemas de arquivos distribuídos no Windows Server 2008](http://go.microsoft.com/fwlink/?linkid=85231)

[Referência técnica do FRS](http://go.microsoft.com/fwlink/?linkid=121764)

