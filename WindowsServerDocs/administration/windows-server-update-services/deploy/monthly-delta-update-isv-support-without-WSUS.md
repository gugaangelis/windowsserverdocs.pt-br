---
title: Suporte do ISV sem WSUS a atualização mensal Delta
description: Tópico do Windows Server Update Service (WSUS) - como o fornecedores de Software independente (ISV) pode usar temporariamente atualização mensal Delta em vez de entrega de atualização do WSUS Express para reduzir o tamanho do pacote
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: get-started article
author: sakitong
ms.author: coreyp
manager: dougkim
ms.date: 10/16/2017
ms.openlocfilehash: 272d3865bbe1a9853f5349c5e878155351525ef0
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439945"
---
# <a name="monthly-delta-update-isv-support-without-wsus"></a>Suporte do ISV sem WSUS a atualização mensal Delta

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows 10

Downloads de atualização do Windows 10 podem ser grandes, porque cada pacote contém todas as correções lançadas anteriormente para garantir consistência e simplicidade.  

Desde a versão 7, Windows tem sido capaz de reduzir o tamanho dos downloads de atualização do Windows com um recurso chamado [Express](https://technet.microsoft.com/library/cc708456(v=ws.10).aspx#Anchor_2), e embora dispositivos de consumo dão suporte a ele por padrão, os dispositivos do Windows 10 enterprise exigem Windows Server Update Services (WSUS) para tirar proveito do Express. Se você tiver o WSUS disponível, consulte [suporte de entrega ISV Express atualização](express-update-delivery-ISV-support.md). É recomendável usá-lo para habilitar a entrega de atualização do Express. 

Se você não tenha instalado o WSUS, mas precisa atualizar menores tamanhos dos pacote nesse ínterim, você pode usar a atualização mensal Delta. Atualização delta reduz os tamanhos do pacote substancialmente, mas não tanto quanto com o WSUS Express entrega de update. É recomendável que você implante a atualização do WSUS Express sempre que possível para a maior redução em tamanhos de pacote. A seguir está um gráfico que compara os tamanhos de download de Delta, cumulativo e Express para Windows 10 versão 1607:

![Comparação de tamanho de download](../../media/express-update-delivery-isv-support/delta-1.png)

## <a name="what-is-monthly-delta-update"></a>O que é a atualização de Delta mensal?

Há duas variantes da atualização de segurança mensal: Delta e cumulativas.

Atualização mensal do Delta é nova e uma solução temporária para ISVs que não têm o WSUS disponível para ajudar a reduzir o tamanho dos pacotes de atualização.

>[!IMPORTANT]
>**Atualização de delta está disponível para a manutenção do Windows 10, versão 1607 (atualização de aniversário), versão 1703 (atualização para criadores) e versão 1709 (Fall Creators Update).** Para versões após a versão 1709, você precisará implementar uma infraestrutura de implantação que dá suporte a [Express atualização entrega](express-update-delivery-ISV-support.md) para continuar aproveitando a atualizações incrementais.

Usando a atualização mensal Delta, pacotes conterá apenas as atualizações deste mês de um. Mensal cumulativo contém todas as atualizações até essa versão de atualização, resultando em um arquivo grande que aumenta a cada mês. Delta e atualizações mensais lançadas na segunda terça-feira de cada mês, também conhecido como "Tuesday da atualização". A tabela a seguir compara o Delta e atualizações cumulativas:

|                    | Mensal **Delta** atualizar                                                                                                                                                                                                       | Mensal **cumulativo** atualizar                                                                                                                                                                                             |
|--------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Escopo**          | Única atualização com **apenas novas correções para esse mês**                                                                                                                                                                           | Atualização única com todas as novas correções para esse mês e todos os últimos meses                                                                                                                                                   |
| **Aplicativo**    | Só pode ser aplicada se a atualização do mês anterior foi aplicado (cumulativas ou Delta)                                                                                                                                           | Pode ser aplicado a qualquer momento                                                                                                                                                                                                |
| **Entrega**       | **Publicado somente em Catálogo do Windows Update** onde ele pode ser baixado para uso com outras ferramentas ou processos. Não é oferecido aos computadores que estão conectados ao Windows Update                                                         | Publicado no catálogo do Windows Update, WSUS e atualização do Windows (onde todos os consumidores PCs irá instalá-lo)                                                                                                                |

Delta e cumulativas têm o mesmo número KB, com a mesma classificação e de versão ao mesmo tempo. As atualizações podem ser diferenciadas pelo título de atualização no catálogo ou pelo nome no msu:

- 2017-02 *\***atualização delta**\**  para Windows 10 versão 1607 para sistemas baseados em x64 (KB1234567)
- 2017-02 *\***atualização cumulativa**\**  para Windows 10 versão 1607 para sistemas baseados em x86 (KB1234567)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      

### <a name="when-to-use-monthly-delta-update"></a>Quando usar a atualização mensal do Delta

Se o tamanho da atualização para o dispositivo do cliente for uma preocupação, é recomendável usar o update Delta nos dispositivos que têm a atualização e atualização nos dispositivos que estão ficando atrasadas cumulativo do mês anterior. Dessa forma, todos os dispositivos exigem somente uma única atualização para colocá-los atualizados. Isso requer um pequeno ajuste em que o processo geral de gerenciamento de atualização, pois você terá que implantar diferentes atualizações com base no grau de atualização os dispositivos estão na organização:

![Comparação de tamanho de download](../../media/express-update-delivery-isv-support/delta-2.png)

### <a name="prevent-deployment-of-delta-and-cumulative-updates-in-the-same-month"></a>Impedir a implantação de atualizações cumulativas e de Delta no mesmo mês

Como a atualização de Delta e de atualização cumulativa estiverem disponíveis ao mesmo tempo, é importante entender o que acontece se você implantar ambas as atualizações no mesmo mês.

Se você aprova e implantar a mesma versão do Delta e a atualização cumulativa, você não apenas gerará tráfego de rede adicional, pois ambos serão baixadas para o PC, mas você não poderá reinicializar o computador para o Windows após a reinicialização.

Se o Delta e atualizações cumulativas inadvertidamente estão instaladas e o computador não está sendo inicializado, você pode recuperar com as seguintes etapas:

1. Inicializar no WinRE prompt de comando
2. Liste os pacotes em um estado pendente:

    `x:\windows\system32\dism.exe /image:<drive letter for windows directory> /Get-Packages >> <path to text file>`
 
    > **Exemplo**: ` x:\windows\system32\dism.exe /image:c:\ /Get-Packages >> c:\temp\packages.txt`
 
3. Abra o arquivo de texto onde você canalizada **get-packages**. Se você vir qualquer instalação pendente patches, execute **remover pacote** para cada nome do pacote:
 
   `dism.exe /image:<drive letter for windows directory> /remove-package /packagename:<package name>`
 
    > **Exemplo**: `x:\windows\system32\dism.exe /image:c:\ /remove-package /packagename:Package_for_KB4014329~31bf3856ad364e35~amd64~~10.0.1.0`
 
    >[!NOTE]
    >Não remova desinstalar pendentes patches.

>[!IMPORTANT]
>**Atualização de delta está disponível para a manutenção do Windows 10, versão 1607 (atualização de aniversário), versão 1703 (atualização para criadores) e versão 1709 (Fall Creators Update).** Para versões após a versão 1709, você precisará implementar uma infraestrutura de implantação que dá suporte a [Express atualização entrega](express-update-delivery-ISV-support.md) para continuar aproveitando a atualizações incrementais.
