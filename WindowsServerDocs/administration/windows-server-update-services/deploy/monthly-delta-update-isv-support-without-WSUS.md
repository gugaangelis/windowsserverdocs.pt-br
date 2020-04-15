---
title: Suporte mensal a ISVs para atualização delta sem WSUS
description: Tópico sobre o WSUS (Windows Server Update Service) – como ISVs (fornecedores independentes de software) podem usar temporariamente a atualização Delta mensal, em vez da entrega de atualizações do WSUS Express para reduzir o tamanho do pacote
ms.prod: windows-server
ms.technology: manage-wsus
ms.topic: get-started article
author: sakitong
ms.author: coreyp
manager: dougkim
ms.date: 10/16/2017
ms.openlocfilehash: 3ccddd3bfd55ae340dc5273905bb475e7d2cb98a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80828739"
---
# <a name="monthly-delta-update-isv-support-without-wsus"></a>Suporte mensal a ISVs para atualização delta sem WSUS

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016, Windows 10

Os downloads do Windows 10 Update podem ser grandes, pois cada pacote contém todas as correções lançadas anteriormente para garantir a consistência e a simplicidade.  

Desde a versão 7, o Windows conseguiu reduzir o tamanho dos downloads do Windows Update com um recurso chamado [Express](https://technet.microsoft.com/library/cc708456(v=ws.10).aspx#Anchor_2) e, embora os dispositivos do consumidor sejam compatíveis com ele por padrão, os dispositivos Windows 10 Enterprise precisam do Windows Server Update Services (WSUS) para utilizar o Express. Se você tiver o WSUS disponível, confira [Suporte ao ISV de entrega de atualização Express](express-update-delivery-ISV-support.md). É recomendável usá-lo para habilitar a entrega de atualização Express. 

Se você não tiver o WSUS instalado, mas precisar de tamanhos de pacote de atualização menores enquanto isso, poderá usar a atualização Delta mensal. A atualização Delta reduz substancialmente os tamanhos de pacote, mas não tanto quanto a entrega de atualizações Express do WSUS. Recomendamos implantar a atualização Express do WSUS sempre que possível para a máxima redução nos tamanhos dos pacotes. Veja a seguir um gráfico que compara os tamanhos de download das atualizações Delta, Cumulativa e Express para o Windows 10 versão 1607:

![Comparação de tamanho do download](../../media/express-update-delivery-isv-support/delta-1.png)

## <a name="what-is-monthly-delta-update"></a>Qual é a atualização Delta Mensal?

Há duas variantes da atualização de segurança mensal: Delta e cumulativo.

A atualização Delta Mensal é nova e uma solução provisória para ISVs que não têm o WSUS disponível para ajudar a reduzir tamanhos de pacote de atualização.

>[!IMPORTANT]
>**A atualização Delta está disponível para manutenção do Windows 10, versão 1607 (Atualização de Aniversário), versão 1703 (Atualização para Criadores) e versão 1709 (Fall Creators Update).** Para versões posteriores à versão 1709, você precisará implementar uma infraestrutura de implantação compatível com a [entrega de atualização Express](express-update-delivery-ISV-support.md) para continuar utilizando atualizações incrementais.

Ao usar a atualização Delta mensal, os pacotes conterão apenas as atualizações de um mês. A Cumulativa Mensal contém todas as atualizações até esta versão de atualização, resultando em um arquivo grande que cresce a cada mês. As atualizações Delta e Mensal são lançadas na segunda terça-feira de cada mês, também conhecida como Atualização de terças-feira. A tabela a seguir compara as atualizações Delta e Cumulativa:

|                    | Atualização **Delta** mensal                                                                                                                                                                                                       | Atualização mensal **Cumulativa**                                                                                                                                                                                             |
|--------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Escopo**          | Atualização única com **apenas novas correções para esse mês**                                                                                                                                                                           | Atualização única com todas as novas correções para esse mês e todos os meses anteriores                                                                                                                                                   |
| **Aplicativo**    | Só poderá ser aplicado se a atualização do mês anterior tiver sido aplicada (Cumulativa ou Delta)                                                                                                                                           | Pode ser aplicada a qualquer momento                                                                                                                                                                                                |
| **Entrega**       | **Publicada somente para o Catálogo do Windows Update** em que ele pode ser baixado para uso com outras ferramentas ou processos. Não oferecido para computadores conectados ao Windows Update                                                         | Publicado no Windows Update (em que todos os computadores do consumidor o instalarão), WSUS e o Catálogo do Windows Update                                                                                                                |

Delta e Cumulativo têm o mesmo número de KB, com a mesma classificação, são liberados ao mesmo tempo. As atualizações podem ser diferenciadas pelo título da atualização no catálogo ou pelo nome da msu:

- 2017-02 *\***Atualização Delta**\**  para Windows 10 versão 1607 para sistemas baseados em x64 (KB1234567)
- 2017-02 *\***Atualização Cumulativa**\**  para Windows 10 versão 1607 para sistemas baseados em x86 (KB1234567)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      

### <a name="when-to-use-monthly-delta-update"></a>Quando usar a Atualização Delta Mensal

Se o tamanho da atualização para o dispositivo cliente for uma preocupação, é recomendável usar a atualização Delta nos dispositivos que têm a atualização do mês anterior e a atualização cumulativa nos dispositivos que estão atrasados. Dessa forma, todos os dispositivos exigem apenas uma única atualização para ficarem atualizados. Isso requer um pequeno ajuste no processo geral de gerenciamento de atualizações, pois você precisará implantar atualizações diferentes com base em quão atualizados os dispositivos estão na organização:

![Comparação de tamanho do download](../../media/express-update-delivery-isv-support/delta-2.png)

### <a name="prevent-deployment-of-delta-and-cumulative-updates-in-the-same-month"></a>Impedir a implantação de atualizações Delta e Cumulativas no mesmo mês

Como a atualização Delta e a atualização Cumulativa estão disponíveis ao mesmo tempo, é importante entender o que acontecerá se você implantar ambas as atualizações no mesmo mês.

Se você aprovar e implantar a mesma versão da atualização Delta e da atualização Cumulativa, você não apenas gerará tráfego de rede adicional, já que ambas serão baixadas para o PC, como talvez não consiga reinicializar o computador para o Windows após a reinicialização.

Se as atualizações Delta e Cumulativa forem instaladas inadvertidamente e o computador não estiver mais sendo inicializado, você poderá se recuperar com as seguintes etapas:

1. Inicializar no prompt de comando do WinRE
2. Listar os pacotes em um estado pendente:

    `x:\windows\system32\dism.exe /image:<drive letter for windows directory> /Get-Packages >> <path to text file>`
 
    > **Exemplo**: ` x:\windows\system32\dism.exe /image:c:\ /Get-Packages >> c:\temp\packages.txt`
 
3. Abra o arquivo de texto em que você canalizou **get-packages**. Se você vir qualquer patch de instalação pendente, execute **remove-package** para cada nome de pacote:
 
   `dism.exe /image:<drive letter for windows directory> /remove-package /packagename:<package name>`
 
    > **Exemplo**: `x:\windows\system32\dism.exe /image:c:\ /remove-package /packagename:Package_for_KB4014329~31bf3856ad364e35~amd64~~10.0.1.0`
 
    >[!NOTE]
    >Não remova os patches que estão aguardando desinstalação.

>[!IMPORTANT]
>**A atualização Delta está disponível para manutenção do Windows 10, versão 1607 (Atualização de Aniversário), versão 1703 (Atualização para Criadores) e versão 1709 (Fall Creators Update).** Para versões posteriores à versão 1709, você precisará implementar uma infraestrutura de implantação compatível com a [entrega de atualização Express](express-update-delivery-ISV-support.md) para continuar utilizando atualizações incrementais.
