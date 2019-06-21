---
title: Suporte expresso a ISVs para entrega de atualizações
description: Tópico do Windows Server Update Service (WSUS) - como o fornecedores de Software independente (ISV) pode configurar a entrega de update Express usando o WSUS
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: get-started article
author: sakitong
ms.author: coreyp
manager: lizapo
ms.date: 10/16/2017
ms.openlocfilehash: 5f2a99bb69fd41c05013788187838f8fceb5f69a
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280446"
---
# <a name="express-update-delivery-isv-support"></a>Suporte expresso a ISVs para entrega de atualizações

>Aplica-se a: Windows 10, Windows Server 2016

Downloads de atualização do Windows 10 podem ser grandes, porque cada pacote contém todas as correções lançadas anteriormente para garantir consistência e simplicidade.  

Desde a versão 7, Windows tem sido capaz de reduzir o tamanho dos downloads de atualização do Windows com um recurso chamado [Express](https://technet.microsoft.com/library/cc708456(v=ws.10).aspx#Anchor_2), e embora dispositivos de consumo dão suporte a ele por padrão, os dispositivos do Windows 10 enterprise exigem Windows Server Update Services (WSUS) para tirar proveito do Express.

## <a name="how-microsoft-supports-express"></a>Como a Microsoft dá suporte ao Express

- **Express no WSUS autônomo**

    Entrega de update Express já está [disponível em todas as versões com suporte do WSUS](https://technet.microsoft.com/library/cc708456(v=ws.10).aspx).

- **Express nos dispositivos diretamente conectados ao Windows Update** 

    Suportam a dispositivos de consumo download Express: usarem o cliente do Windows Update (WU) para verificar, baixar e instalar atualizações. Durante a fase de download, o cliente WU solicitações de pacotes de Express e baixa os intervalos de bytes apropriados.

-  **Dispositivos corporativos gerenciados usando o [Windows Update para Empresas](https://technet.microsoft.com/itpro/windows/manage/waas-manage-updates-wufb)** também obtêm o benefício de suporte de entrega de atualização Express sem qualquer alteração na configuração.

## <a name="how-isvs-can-take-advantage-of-express"></a>Como os ISVs podem tirar proveito do Express

Os ISVs podem usar o WSUS e o cliente do WU para dar suporte à entrega de atualização do Express. A Microsoft recomenda as três etapas a seguir, cada discutido mais detalhadamente nas seções a seguir:

1.  [**Configurar o WSUS**](#BKMK_1)

    Servidor do WSUS é necessário para verificar, & sincronizações de atualização (informações adicionais podem ser encontradas [aqui](https://technet.microsoft.com/library/dn800972(v=ws.11).aspx))

2.  [**Especifique e preencher um cache de arquivo do ISV**](#BKMK_2)

    Um cache de arquivo do ISV é recomendado para hospedar o conteúdo da atualização, que inclui os arquivos de gabinete de atualização (arquivos. cab) e o Express pacotes (arquivos .psf).

3.  [**Configurar um agente de cliente do ISV para direcionar as operações do cliente WU**](#BKMK_3)

>[!NOTE]
>Requer a versão cumulativa Update para Windows 10 versão 1607 (ou depois) de janeiro de 2017 ([KB3213986 (SO Build 14393.693)](https://support.microsoft.com/en-us/help/4009938/january-10-2017-kb3213986-os-build-14393-693) a serem instalados.
    
   - O agente de cliente do ISV determina quais atualizações para aprovar e quando baixar e instalar atualizações
   - O cliente WU determina os intervalos de bytes para baixar e inicia a solicitação de download

### <a name="BKMK_1"></a>Etapa 1: Configurar o WSUS

WSUS serve como a interface do Windows Update e gerencia todos os metadados que descrevem os pacotes do Express que precisam ser baixado. Se você precisar implantar, consulte [ **visão geral do Windows Server Update Services 3.0 SP2**](https://technet.microsoft.com/library/dd939931(v=ws.10).aspx). Após a implantação do WSUS, a principal consideração é se deseja ou não armazenar o conteúdo de atualização localmente no servidor do WSUS. Ao configurar o WSUS, é recomendável não armazenar atualizações localmente. Isso pressupõe que você já tem software direcionando a implantação desses pacotes em seu ambiente. Para obter mais informações sobre como configurar o armazenamento local do WSUS, consulte [ **Determine Where to Store atualizações**](https://technet.microsoft.com/library/cc720494(v=ws.10).aspx).

### <a name="BKMK_2"></a>Etapa 2: Especifique e popular o Cache de arquivo de ISV 

#### <a name="specify-the-isv-file-cache"></a>Especificar o Cache de arquivo de ISV

Novo lado do cliente diretiva de grupo e gerenciamento de dispositivo móvel (MDM) as configurações detalhadas na [ **referência do provedor de serviço de configuração** ](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/configuration-service-provider-reference) definem o local do cache de arquivo do ISV.

| **Nome**                                              | **Descrição**                                                                                                                                                      |
|-------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Configure um local alternativo de download para atualizações. | Especifica um servidor de intranet alternativo para atualizações de host do Microsoft Update. Em seguida, você pode usar esse serviço de atualização para atualizar automaticamente os computadores em sua rede |

Há duas opções ao configurar o local de download alternativo para o cache de arquivo do ISV:

1. **Especifique um nome de host do servidor HTTP do ISV**, que é o cache de arquivos de ISV
    
    Essa abordagem configura o cliente do WU para fazer solicitações de download para o servidor HTTP especificado na política

2. **Especifique o localhost**
 
    Essa abordagem configura o cliente do WU para fazer solicitações de download ao localhost. Isso permite que o agente de cliente do ISV para lidar com essas solicitações e a rota conforme apropriado para atender à solicitação de download.

> [!IMPORTANT]
> O cache de arquivos do ISV requer o seguinte:                                                          
> - O servidor deve estar em conformidade pelo RFC do HTTP 1.1: <http://www.w3.org/Protocols/rfc2616/rfc2616.html>                                                                                                                                                                
> Especificamente, o servidor web precisa para dar suporte                                                                                                                                                                                                                                       [ **HEAD** ](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) e [ **obter** ](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.htm) solicitações<br>                                                                                                                                                                                                                                                                                                  -Solicitações de intervalo de parcial<br>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   - Keep-alive<br>                                                                                                                                                                                                                                                                                                                                                                                                                            -Não use "Transfer-Encoding: em bloco"                                                                                                 

#### <a name="populate-the-isv-file-cache"></a>Popular o Cache de arquivo de ISV

O cache de arquivos do ISV deve ser preenchido com os arquivos associados com as atualizações a serem instalados nos clientes gerenciados. 

**Para popular o cache de arquivo do ISV:**

1. Use [as APIs do WSUS](https://msdn.microsoft.com/library/windows/desktop/microsoft.updateservices.administration.updatefile(v=vs.85).aspx) para acessar o caminho do arquivo e o nome de arquivo para o serviço MU a atualização.

    Os metadados para cada atualização no servidor do WSUS contém a atualização de um caminho de arquivo e nome do arquivo no Microsoft Update, da seguinte maneira (nome de host do Microsoft Update em negrito, seguido pelo nome de arquivo e caminho do arquivo): **<http://download.windowsupdate.com>** /c/msdownload/atualização / software/updt/2016/09/windows10.0-kb3195781-x64_0c06079bccc35cba35a48bd2b1ec46f818bd2e74.msu

2. Baixar arquivos do Microsoft Update e armazená-los no cache de arquivo do ISV usando um destes dois métodos: 

   - Arquivos da Store usando o **mesmo caminho de pasta que o serviço MU**

   - Arquivos da Store usando um **caminho da pasta definido pelo ISV**

     Ter o redirecionamento HTTP de servidor (ou localhost) **HTTP GET** solicitações, que fazem referência a MU caminho e nome da pasta, para o local do arquivo de ISV.

### <a name="BKMK_3"></a>Etapa 3: Configurar um agente de cliente do ISV para direcionar as operações do cliente WU

O agente de cliente do ISV orquestra o download e instalação de atualizações aprovadas usando o seguinte fluxo de trabalho recomendado:

1.  O agente de cliente do ISV chama o cliente do WU para verificar em relação ao servidor do WSUS

2.  A verificação retorna o conjunto de atualizações aplicáveis ao cliente WU

3.  O cliente do ISV determina quais atualizações deverão ser aprovar, baixe e instale

4.  O cliente do WU ISV cliente agente chamadas para baixar as atualizações aprovadas

5.  Depois que as atualizações tiverem sido baixadas, o agente de cliente do ISV chama o cliente do WU para instalar as atualizações aprovadas

Ver [pesquisando, baixando e instalando atualizações](https://msdn.microsoft.com/library/windows/desktop/aa387102(v=vs.85).aspx) para obter informações adicionais sobre como usar o cliente do WU para verificar, baixar e instalar atualizações.

### <a name="download-workflow-options"></a>Opções de fluxo de trabalho de download

A seguir estão as duas ilustrações de opções de fluxo de trabalho de download de um cache de arquivo do ISV:

![Fluxo de trabalho 1](../../media/express-update-delivery-isv-support/image1.png)

![Fluxo de trabalho 2](../../media/express-update-delivery-isv-support/image2.png)
### <a name="how-express-download-works"></a>Como o download Express funciona

- Para atualizações do sistema operacional com suporte ao Express, há duas versões da carga de arquivos armazenados no serviço:

  - **Versão completa** – essencialmente, substituindo as versões locais de binários de atualização

  - **Versão expressa** – que contêm os deltas precisava do patch os binários existentes no dispositivo. 

    A versão completa e a versão Express são referenciados nos metadados da atualização, que foi baixado para o cliente como parte da fase de verificação. 

    **Download Express funciona da seguinte maneira:**

    O cliente WU tentarão baixar Express primeiro e, em determinadas situações outono volta para o arquivo completo se necessário (por exemplo, se passar por um proxy que não dá suporte a solicitações de intervalo de bytes).

  1. Quando o cliente do WU inicia um download do Express **o cliente do WU primeiro baixa um stub**, que faz parte do pacote do Express.

  2. **O cliente WU passa esse fragmento de código para o instalador do Windows**, que usa o stub para fazer um inventário local, comparando os deltas do arquivo no dispositivo com o que é necessário para obter a versão mais recente do arquivo que está sendo oferecido.

  3. O **instalador do Windows solicita, em seguida, o cliente do WU para baixar os intervalos de** que têm sido determinado como necessária.

  4. **O cliente WU downloads esses intervalos e os passa para o instalador do Windows**, que aplica os intervalos e, em seguida, determina se os intervalos de adicionais são necessárias. Isso é repetido até que o instalador do Windows informa o cliente WU que todos os intervalos necessários tiverem sido baixados.

  Neste ponto, o download é concluído, e a atualização está pronta para ser instalada.

### <a name="how-delivery-optimization-reduces-bandwidth-consumption"></a>Como a otimização de entrega reduz o consumo de largura de banda

Otimização de entrega (DO) é uma solução de cache distribuído auto-organizada para as empresas que buscam para reduzir o consumo de largura de banda para atualizações do sistema operacional, atualizações do sistema operacional e aplicativos. Permite que os clientes baixem os elementos de fontes alternativas (como outros pares na rede) em conjunto com o local de download especificada (o cache de arquivo ISV neste cenário).

Por padrão no Windows 10 Enterprise e Education, permite o compartilhamento de ponto a ponto na rede da organização só, mas você pode configurá-lo diferente usando a diretiva de grupo e as configurações de MDM (gerenciamento) do dispositivo móvel.

Consulte a [configurar a entrega de otimização para Windows 10 atualiza](https://technet.microsoft.com/itpro/windows/manage/waas-delivery-optimization) para obter mais informações sobre como fazer.
