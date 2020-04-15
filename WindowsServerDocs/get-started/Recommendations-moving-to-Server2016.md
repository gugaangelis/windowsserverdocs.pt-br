---
title: Recomendações para mudar para o Windows Server 2016
description: Recomendações para mudar para o Windows Server 2016.
ms.prod: windows-server
ms.date: 10/18/2016
ms.technology: server-general
ms.topic: article
ms.assetid: 74aa1da3-7076-4a1f-ad5b-9e17bd46dba2
author: jaimeo
ms.author: jaimeo
manager: dongill
ms.localizationpriority: medium
ms.openlocfilehash: 6b02a3caa0db2a66307754ebd95865d8ba10ef4f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80826749"
---
# <a name="recommendations-for-moving-to-windows-server-2016"></a>Recomendações para mudar para o Windows Server 2016

>Aplica-se a: Windows Server 2016


|Se você está executando:|Windows Server 2012 R2 ou Windows Server 2012|Windows Server 2008 R2 ou Windows Server 2008|  
|-------------------|----------|--------------|--------------|---------------------------------------|  
|**Infraestrutura de função do Windows Server**|Escolha a atualização ou migração dependendo das [diretrizes específicas da função](https://technet.microsoft.com/windowsserver/jj554790).|-Para aproveitar os novos recursos no Windows Server 2016, implante novo hardware ou instale o Windows Server 2016 em uma máquina virtual em um host existente. Alguns novos recursos funcionam melhor em um host físico do Windows Server 2016 executando Hyper-V. <br>- Siga as [diretrizes específicas da função](https://technet.microsoft.com/windowsserver/jj554790).|
|**Gerenciamento de servidor da Microsoft e cargas de trabalho de aplicativo**|- Atualizações de aplicativo devem incluir *migração* para o Windows Server 2016. Consulte a [lista de compatibilidade](Server-Application-Compatibility.md). <br>- As atualizações ao Windows Server 2016 apenas (ou seja, sem atualizar os aplicativos) devem usar as diretrizes específicas do aplicativo.|-Para aproveitar os novos recursos no Windows Server 2016, implante novo hardware ou instale o Windows Server 2016 em uma máquina virtual em um host existente. Alguns novos recursos funcionam melhor em um host físico do Windows Server 2016 executando Hyper-V. Siga os guias de migração conforme aplicável. <br>- Ou permaneça em seu SO atual e execute em uma máquina virtual em execução em um host do Windows Server 2016 ou no Microsoft Azure. Entre em contato com seu revendedor EA, TAM ou Microsoft para conhecer opções de suporte estendido por meio do [Software Assurance](https://www.microsoft.com/Licensing/licensing-programs/software-assurance-default.aspx).|
|**Cargas de trabalho de aplicativos de ISV**|- Atualizações para o Windows Server 2016 devem usar as diretrizes específicas do aplicativo. <br>- Para mais informações sobre a compatibilidade do Windows Server com aplicativos não Microsoft, visite o [portal de Certificação de logotipo do Windows Server](https://msdn.microsoft.com/enterprisecloudcertified).|-Para aproveitar os novos recursos no Windows Server 2016, implante novo hardware ou instale o Windows Server 2016 em uma máquina virtual em um host existente. Alguns novos recursos funcionam melhor em um host físico do Windows Server 2016 executando Hyper-V. Siga os guias de migração conforme aplicável. <br>- Ou permaneça em seu SO atual e execute em uma máquina virtual em execução em um host do Windows Server 2016 ou no Microsoft Azure. Entre em contato com seu revendedor EA, TAM ou Microsoft para conhecer opções de suporte estendido por meio do [Software Assurance](https://www.microsoft.com/Licensing/licensing-programs/software-assurance-default.aspx).|
|**Cargas de trabalho de aplicativo personalizadas**|- Consulte desenvolvedores de aplicativo sobre a compatibilidade com o Windows Server 2016 e as diretrizes de atualização. <br>- Aproveite o Microsoft Azure para testar o aplicativo no Windows Server 2016 antes de mudar. <br>- Consulte as opções completas na próxima seção.|- Consulte seus desenvolvedores de aplicativo sobre a compatibilidade com o Windows Server 2016 e as diretrizes de atualização. <br>- Aproveite o Microsoft Azure para testar seu aplicativo no Windows Server 2016 antes de mudar. <br>-Para aproveitar os novos recursos no Windows Server 2016, implante novo hardware ou instale o Windows Server 2016 em uma máquina virtual em um host existente. Alguns novos recursos funcionam melhor em um host físico do Windows Server 2016 executando Hyper-V. <br>- Consulte as opções completas na próxima seção.|

## <a name="complete-options-for-moving-servers-running-custom-or-in-house-applications-on-older-versions-of-windows-server-to-windows-server-2016"></a>Opções completas para mover servidores que executam aplicativos personalizados ou internos em versões anteriores do Windows Server para o Windows Server 2016

Agora há ainda mais opções para ajudar você e seus clientes a aproveitar os recursos no Windows Server 2016, com impacto mínimo sobre os serviços e cargas de trabalho atuais.

- Experimente o sistema operacional mais recente com seu aplicativo baixando a versão de avaliação do [Windows Server](https://www.microsoft.com/evalcenter/evaluate-windows-server-2016) para teste local. Depois que o teste for concluído e a qualidade for confirmada, você poderá executar uma conversão de licença simples com uma chave de licença para o varejo (exige reinicialização).

- O [Microsoft Azure](https://azure.microsoft.com) também pode ser usado como avaliação para testes para garantir que seu aplicativo personalizado funcione no sistema operacional do servidor mais recente. Depois que o teste for concluído e qualidade for confirmada, [migre para a versão mais recente do Windows Server](https://docs.microsoft.com/windows-server/get-started/installation-and-upgrade#upgrade) em suas instalações. 

- Ou como alternativa, quando o teste for concluído e a qualidade for confirmada, o [Microsoft Azure](https://azure.microsoft.com) poderá ser usado como o local permanente para seu aplicativo ou serviço personalizado. Isso permite que o servidor antigo permaneça disponível até que você esteja pronto para mudar para o novo servidor no Azure.

    - Se já tiver o Software Assurance para Windows Server, economize dinheiro implantando com o [Benefício de Uso do Azure Híbrido](https://azure.microsoft.com/pricing/hybrid-use-benefit/). 

- Na maioria dos casos, o [Microsoft Azure](https://azure.microsoft.com) pode ser usado para hospedar o mesmo aplicativo na versão mais antiga do Windows Server em que ele é executado hoje. Migre o aplicativo e a carga de trabalho para uma máquina virtual com o sistema operacional de sua escolha usando imagens do [Azure Marketplace](https://azure.microsoft.com/marketplace/).

    - Se já tiver o Software Assurance para Windows Server, economize dinheiro implantando com o [Benefício de Uso do Azure Híbrido](https://azure.microsoft.com/pricing/hybrid-use-benefit/). 

- O programa [Software Assurance](https://www.microsoft.com/Licensing/licensing-programs/software-assurance-default.aspx) para Windows Server oferece os benefícios de direitos da nova versão. Juntamente com uma lista de outros benefícios, os servidores com Software Assurance podem ser atualizados para a versão mais recente do Windows Server na hora certa, sem necessidade de comprar uma nova licença. 

## <a name="additional-resources"></a>Recursos adicionais

- [Recursos removidos ou preteridos no Windows Server 2016](deprecated-features.md)
- Para opções gerais de atualização e migração de servidor, visite [Upgrade and conversion options for Windows Server 2016](Supported-Upgrade-Paths.md) (Opções de atualização e conversão para o Windows Server 2016).
- Para mais informações sobre o ciclo de vida e os níveis de suporte do produto, consulte [Política de Ciclo de Vida de Suporte – perguntas frequentes](https://support.microsoft.com/help/17140/support-lifecycle-policy-faq).

