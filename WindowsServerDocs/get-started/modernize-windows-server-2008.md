---
title: Fazer upgrade do Windows Server 2008 e do Windows Server 2008 R2
description: O Windows Server 2008 e 2008 R2 estão se aproximando do fim de serviço. Saiba como fazer upgrade local ou hospedar novamente no Azure.
ms.prod: windows-server
ms.mktglfcycl: manage
ms.sitesec: library
author: mikeblodge
ms.author: mikeblodge
ms.date: 07/12/2018
ms.topic: get-started-article
ms.localizationpriority: high
ms.openlocfilehash: 3d2c55430a78eaabfe55b764275c6e61fa80368a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80826209"
---
# <a name="upgrade-windows-server-2008-and-windows-server-2008-r2"></a>Fazer upgrade do Windows Server 2008 e do Windows Server 2008 R2

O suporte estendido para Windows Server 2008 e Windows Server 2008 R2 terminará em 14 de janeiro de 2020. Há dois caminhos de modernização disponíveis: Upgrade local ou migração por meio de nova hospedagem no Azure. **Se você hospedar novamente no Azure, poderá migrar suas imagens existentes do Servidor gratuitamente.**

![Fluxograma que descreve os caminhos de upgrade do Windows Server 2008](media/WS08_upgrade_paths.png)


## <a name="on-premises-upgrade"></a>Upgrade local
Se precisar manter seus servidores como locais e estiver executando o Windows Server 2008 ou o Windows Server 2008 R2, você precisará [fazer upgrade para o Windows Server 2012/2012 R2](installation-and-upgrade.md#upgrading-to-windows-server-2012-r2) antes de poder [fazer upgrade para o Windows Server 2016](installation-and-upgrade.md#upgrading-to-windows-server-2016). Ao fazer upgrade, você ainda terá a opção de migrar para o Azure por meio de uma nova hospedagem.

Consulte [Upgrade do Windows Server 2008 R2 ou do Windows Server 2008](installation-and-upgrade.md#upgrading-from-windows-server-2008-r2-or-windows-server-2008) para obter mais informações sobre suas opções de upgrade local.

Se estiver executando o Windows Server 2003, você precisará [fazer upgrade para o Windows Server 2008](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff972408(v%3dws.10)). Consulte os [caminhos de upgrade para o Windows Server 2008](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd979563(v=ws.10)) para obter mais informações sobre suas opções de upgrade local.


## <a name="migrate-to-azure"></a>Migrar para o Azure
Você pode migrar seus servidores locais do Windows Server 2008 e do Windows Server 2008 R2 para o Azure, onde poderá continuar a executá-los em máquinas virtuais. No Azure, você permanecerá em conformidade, ficará mais seguro e adicionará a inovação da nuvem ao seu trabalho. Os benefícios da migração para o Azure incluem:

- Atualizações de segurança no Azure.
- Obtenha mais três anos de atualizações de segurança críticas e importantes do Windows Server 2008 ou 2008 R2, incluídas sem custo adicional. 
- Upgrades sem custo no Azure.
- Adote mais serviços de nuvem conforme você estiver pronto.
- Ao migrar o SQL Server para Instâncias Gerenciadas do Azure ou VMs, você obtém mais três anos de atualizações de segurança críticas do Windows Server 2008 ou 2008 R2, incluídas sem custo adicional. 
- Aproveite as licenças existentes do SQL Server e do Windows Server para economias de nuvem exclusivas do Azure.

[![Inicie a migração para o Azure com uma imagem especializada](./media/WS08-image-banner-small.png)](uploading-specialized-WS08-image-to-azure.md)

Para iniciar a migração, consulte [Carregar uma imagem especializada do Windows Server 2008/2008 R2 no Azure](uploading-specialized-WS08-image-to-azure.md).

Para ajudá-lo a entender como analisar os recursos de TI existentes, avaliar o que você tem e identificar os benefícios de migrar serviços e aplicativos específicos para a nuvem ou manter as cargas de trabalho locais e fazer upgrade para a última versão do Windows Server, consulte o [Guia de migração para o Windows Server](https://go.microsoft.com/fwlink/?linkid=872689).

## <a name="upgrade-sql-server-20082008-r2-in-parallel-with-your-windows-servers"></a>Fazer upgrade do SQL Server 2008/2008 R2 em paralelo com seus Windows Servers

![Logotipo do SQL Server](media/sqlr2.jpg)

Se estiver executando o SQL Server 2008/2008 R2, você poderá fazer upgrade para o SQL Server [2016](https://docs.microsoft.com/sql/sql-server/sql-server-technical-documentation?view=sql-server-2016) ou [2017](https://docs.microsoft.com/sql/sql-server/sql-server-technical-documentation?view=sql-server-2017).


## <a name="additional-resources"></a>Recursos adicionais
[Microsoft Azure](https://docs.microsoft.com/azure/#pivot=products)