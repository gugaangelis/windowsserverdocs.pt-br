---
title: Fazer upgrade do Windows Server 2008 e do Windows Server 2008 R2
description: O Windows Server 2008 e 2008 R2 estão se aproximando do fim de serviço. Saiba como fazer upgrade local ou hospedar novamente no Azure.
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: mikeblodge
ms.author: mikeblodge
ms.date: 07/12/2018
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.localizationpriority: high
ms.openlocfilehash: 4127eab613abb429a200f513a11b944e05da0f76
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59851337"
---
# <a name="upgrade-windows-server-2008-and-windows-server-2008-r2"></a>Fazer upgrade do Windows Server 2008 e do Windows Server 2008 R2

O suporte estendido para Windows Server 2008 e Windows Server 2008 R2 terminará em 14 de janeiro de 2020. Há dois caminhos de modernização disponíveis: Local de atualização ou migração por nova hospedagem no Azure. **Se você hospedar novamente no Azure, você pode migrar suas imagens de servidor existentes gratuitamente.**

![Fluxograma que descreve os caminhos de upgrade do Windows Server 2008](media/WS08_upgrade_paths.png)


## <a name="on-premises-upgrade"></a>Upgrade local
Se precisar manter seu servidores no local e se estiver executando o Windows Server 2008 ou o Windows Server 2008 R2, você precisará [fazer upgrade para o Windows Server 2012/2012 R2](installation-and-upgrade.md#upgrading-to-windows-server-2012-r2) antes de poder [fazer upgrade para o Windows Server 2016](installation-and-upgrade.md#upgrading-to-windows-server-2016). Conforme faz upgrade, você ainda terá a opção de migrar para o Azure por nova hospedagem.

Veja [Upgrade do Windows Server 2008 R2 ou do Windows Server 2008](installation-and-upgrade.md#upgrading-from-windows-server-2008-r2-or-windows-server-2008) para obter mais informações sobre suas opções de upgrade local.

Se estiver executando o Windows Server 2003, você precisará [fazer upgrade para o Windows Server 2008](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff972408(v%3dws.10)). Veja os [caminhos de upgrade para o Windows Server 2008](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd979563(v=ws.10)) para obter mais informações sobre suas opções de upgrade local.


## <a name="migrate-to-azure"></a>Migrar para o Azure
Você pode migrar seus servidores locais do Windows Server 2008 e do Windows Server 2008 R2 para o Azure, onde poderá prosseguir para executá-los em máquinas virtuais. No Azure, você permanecerá em conformidade, ficará mais protegido e adicionará inovação de nuvem ao seu trabalho. Os benefícios de migração para o Azure incluem:

- Atualizações de segurança no Azure.
- Obtenha mais três anos de atualizações de segurança críticas e importantes do Windows Server 2008 R2 ou do 2008, incluídas sem custo adicional. 
- Upgrades sem custo no Azure.
- Adote mais serviços de nuvem conforme você estiver pronto.
- Ao migrar o SQL Server para Instâncias Gerenciadas do Azure ou VMs, você obtém mais três anos de atualizações de segurança críticas do Windows Server 2008 R2 ou 2008, incluídas sem custo adicional. 
- Aproveite as licenças existentes do SQL Server e do Windows Server para economia de nuvem exclusiva do Azure.

<a href="uploading-specialized-WS08-image-to-azure.md"><img src="media/WS08-image-banner-small.png"></a>

Para iniciar a migração, veja [Fazer upload de uma imagem especializada do Windows Server 2008/2008 R2 no Azure](uploading-specialized-WS08-image-to-azure.md).

Para ajudá-lo a entender como analisar os recursos de TI existentes, avalie o que você tem e identifique os benefícios da mudança de serviços e aplicativos para a nuvem ou de manter as cargas de trabalho locais e do upgrade para a última versão do Windows Server, veja [Guia de migração para o Windows Server](https://go.microsoft.com/fwlink/?linkid=872689).


## <a name="upgrade-sql-server-20082008-r2-in-parallel-with-your-windows-servers"></a>Fazer upgrade do SQL Server 2008/2008 R2 em paralelo com os servidores do Windows

![Logotipo do SQL Server](media/sqlr2.jpg)

Se estiver executando o SQL Server 2008/2008 R2, poderá fazer upgrade para o SQL Server [2016](https://docs.microsoft.com/sql/sql-server/sql-server-technical-documentation?view=sql-server-2016) ou [2017](https://docs.microsoft.com/sql/sql-server/sql-server-technical-documentation?view=sql-server-2017).


## <a name="additional-resources"></a>Recursos adicionais
[Microsoft Azure](https://docs.microsoft.com/azure/#pivot=products)