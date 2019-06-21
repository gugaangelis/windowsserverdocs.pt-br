---
Title: Migrar a replicação do SYSVOL para a replicação do DFS
ms.date: 07/02/2012
ms.prod: windows-server-threshold
ms.technology: storage
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: 30b3c5925971a2709ba422a017a73f47e72813a7
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67284433"
---
# <a name="migrate-sysvol-replication-to-dfs-replication"></a>Migrar a replicação do SYSVOL para a replicação do DFS


Atualizado: 25 de agosto de 2010

Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 e Windows Server 2008

Controladores de domínio usam uma pasta compartilhada especial chamada SYSVOL para replicar scripts de logon e os arquivos de objeto de diretiva de grupo para outros controladores de domínio. Windows 2000 Server e Windows Server 2003 para usar a replicação FRS (serviço) para replicar o SYSVOL, enquanto o Windows Server 2008 usa o serviço de replicação DFS mais recente quando estiver em domínios que usam o nível funcional de domínio do Windows Server 2008 e o FRS para domínios que Execute os níveis funcionais de domínio mais antigos.

Para usar a replicação DFS para replicar a pasta SYSVOL, você pode criar um novo domínio que usa o nível funcional de domínio do Windows Server 2008 ou você pode usar o procedimento que é abordado neste documento para atualizar um domínio existente e migrar para a replicação Replicação do DFS.

Este documento presume que você tenha um conhecimento básico de serviços de domínio Active Directory (AD DS), FRS e sistema de replicação de arquivos distribuído (replicação do DFS). Para obter mais informações, consulte [visão geral dos serviços do domínio do Active Directory](http://go.microsoft.com/fwlink/?linkid=147787), [visão geral do FRS](http://go.microsoft.com/fwlink/?linkid=121763), ou [visão geral da replicação do DFS](http://go.microsoft.com/fwlink/?linkid=121762)


> [!NOTE]
> Para baixar uma versão imprimível deste guia, vá para <a href="http://go.microsoft.com/fwlink/?linkid=150375">guia de migração de replicação SYSVOL: Replicação FRS para DFS</a> (http://go.microsoft.com/fwlink/?LinkId=150375)
<br>


## <a name="in-this-guide"></a>Neste guia

[Informações conceituais da migração SYSVOL](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640170(v=ws.10))

  - [Estados de migração SYSVOL](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd641052(v=ws.10))  
      
  - [Visão geral do procedimento de migração SYSVOL](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639809(v=ws.10))  
      

[Procedimento de migração SYSVOL](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639860(v=ws.10))

  - [Migrando para o estado preparado](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd641193(v=ws.10))  
      
  - [Migrando para o estado redirecionado](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd641340(v=ws.10))  
      
  - [Migrando para o estado eliminado](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640254(v=ws.10))  
      

[Solucionando problemas de migração SYSVOL](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640395(v=ws.10))

  - [Solucionando problemas de migração SYSVOL](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639976(v=ws.10))  
      
  - [Revertendo a migração de SYSVOL para um estado estável anterior](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640509(v=ws.10))  
      

[Informações de referência de migração SYSVOL](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640293(v=ws.10))

  - [Cenários de migração SYSVOL com suporte](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639854(v=ws.10))  
      
  - [Verificando o estado de migração SYSVOL](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639789(v=ws.10))  
      
  - [Dfsrmig](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd641227(v=ws.10))  
      
  - [Ações da ferramenta de migração SYSVOL](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd639712(v=ws.10))  
      

## <a name="additional-references"></a>Referências adicionais

[Série de migração SYSVOL: Parte 1 — Introdução ao processo de migração SYSVOL](http://go.microsoft.com/fwlink/?linkid=121756)

[Série de migração SYSVOL: Parte 2—Dfsrmig.exe: A ferramenta de migração SYSVOL](http://go.microsoft.com/fwlink/?linkid=121757)

[Série de migração SYSVOL: Parte 3 – migrando para o estado 'PREPARADO'](http://go.microsoft.com/fwlink/?linkid=121758)

[Série de migração SYSVOL: Parte 4 – Migrando para o estado 'REDIRECIONADO'](http://go.microsoft.com/fwlink/?linkid=121759)

[Série de migração SYSVOL: Parte 5 – migrando para o estado 'ELIMINADAS'](http://go.microsoft.com/fwlink/?linkid=121760)

[Guia passo a passo para sistemas de arquivos distribuído no Windows Server 2008](http://go.microsoft.com/fwlink/?linkid=85231)

[Referência técnica do FRS](http://go.microsoft.com/fwlink/?linkid=121764)

