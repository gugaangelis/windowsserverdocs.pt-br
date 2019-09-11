---
title: Suporte expresso a ISVs para entrega de atualizações
description: Tópico do Windows Server Update Service (WSUS) – como ISVs (fornecedores independentes de software) podem configurar a entrega de atualização expressa usando o WSUS
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
ms.openlocfilehash: 0f5893d47219e9263ed7f35bee472848a47c6164
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70868741"
---
# <a name="express-update-delivery-isv-support"></a>Suporte expresso a ISVs para entrega de atualizações

>Aplica-se a: Windows 10, Windows Server 2016

Os downloads de atualização do Windows 10 podem ser grandes porque cada pacote contém todas as correções lançadas anteriormente para garantir a consistência e a simplicidade.  

Desde a versão 7, o Windows conseguiu reduzir o tamanho dos downloads de Windows Update com um recurso chamado [Express](https://technet.microsoft.com/library/cc708456(v=ws.10).aspx#Anchor_2)e, embora os dispositivos do consumidor ofereçam suporte a ele por padrão, os dispositivos Windows 10 Enterprise exigem que o Windows Server Update Services (WSUS) execute vantagem do Express.

## <a name="how-microsoft-supports-express"></a>Como a Microsoft dá suporte ao Express

- **Express no WSUS autônomo**

    A entrega de atualização expressa já está [disponível em todas as versões com suporte do WSUS](https://technet.microsoft.com/library/cc708456(v=ws.10).aspx).

- **Express em dispositivos conectados diretamente a Windows Update** 

    Os dispositivos de consumo dão suporte ao download expresso: eles usam o cliente Windows Update (WU) para verificar, baixar e instalar atualizações. Durante a fase de download, o cliente WU solicita pacotes expressos e baixa os intervalos de bytes apropriados.

-  **Dispositivos corporativos gerenciados usando o [Windows Update para Empresas](https://technet.microsoft.com/itpro/windows/manage/waas-manage-updates-wufb)** também obtêm o benefício de suporte de entrega de atualização Express sem qualquer alteração na configuração.

## <a name="how-isvs-can-take-advantage-of-express"></a>Como os ISVs podem tirar proveito do Express

Os ISVs podem usar o WSUS e o cliente WU para dar suporte à entrega de atualização expressa. A Microsoft recomenda as três etapas a seguir, cada uma discutida mais detalhadamente nas seções abaixo:

1.  [**Configurar o WSUS**](#BKMK_1)

    O servidor do WSUS é necessário para verificações & sincronizações de atualização (informações adicionais podem ser encontradas [aqui](https://technet.microsoft.com/library/dn800972(v=ws.11).aspx))

2.  [**Especificar e popular um cache de arquivos do ISV**](#BKMK_2)

    Um cache de arquivos do ISV é recomendado para hospedar o conteúdo de atualização, que inclui os arquivos de gabinete de atualização (arquivos. cab) e os pacotes expressos (arquivos. PSF).

3.  [**Configurar um agente cliente ISV para direcionar as operações do cliente WU**](#BKMK_3)

>[!NOTE]
>Requer a atualização cumulativa para a versão 1607 do Windows 10 em (ou posterior) de janeiro de 2017 ([KB3213986 (Build do so 14393,693)](https://support.microsoft.com/en-us/help/4009938/january-10-2017-kb3213986-os-build-14393-693) a ser instalado.
    
   - O agente cliente ISV determina quais atualizações aprovar e quando baixar e instalar atualizações
   - O cliente WU determina os intervalos de bytes para baixar e inicia a solicitação de download

### <a name="BKMK_1"></a>Etapa 1: Configurar o WSUS

O WSUS serve como a interface para Windows Update e gerencia todos os metadados que descrevem os pacotes expressos que precisam ser baixados. Se você precisar implantar o, consulte [**visão geral do Windows Server Update Services 3,0 SP2**](https://technet.microsoft.com/library/dd939931(v=ws.10).aspx). Depois que o WSUS tiver sido implantado, a principal consideração será armazenar ou não o conteúdo de atualização localmente no servidor do WSUS. Ao configurar o WSUS, recomendamos não armazenar atualizações localmente. Isso pressupõe que você já tenha a implantação de direcionamento de software desses pacotes em seu ambiente. Para obter mais informações sobre como configurar o armazenamento local do WSUS, consulte [**determinar onde armazenar atualizações**](https://technet.microsoft.com/library/cc720494(v=ws.10).aspx).

### <a name="BKMK_2"></a>Etapa 2: Especificar e popular o cache de arquivos do ISV 

#### <a name="specify-the-isv-file-cache"></a>Especificar o cache de arquivos do ISV

As novas Política de Grupo do lado do cliente e as configurações de MDM (gerenciamento de dispositivo móvel) detalhadas na [**referência do provedor de serviços de configuração**](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/configuration-service-provider-reference) definem o local do cache de arquivos do ISV.

| **Name**                                              | **Descrição**                                                                                                                                                      |
|-------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Configure um local de download alternativo para atualizações. | Especifica um servidor de intranet alternativo para hospedar atualizações de Microsoft Update. Você pode usar esse serviço de atualização para atualizar automaticamente os computadores em sua rede |

Há duas opções ao configurar o local de download alternativo para o cache de arquivos do ISV:

1. **Especificar um nome de host do servidor http do ISV**, que é o cache de arquivos do ISV
    
    Essa abordagem configura o cliente WU para fazer solicitações de download para o servidor HTTP especificado na política

2. **Especificar localhost**
 
    Essa abordagem configura o cliente WU para fazer solicitações de download no localhost. Isso permite que o agente do cliente ISV manipule essas solicitações e rota conforme apropriado para atender à solicitação de download.

> [!IMPORTANT]
> O cache de arquivos do ISV requer o seguinte:                                                          
> - O servidor deve ser compatível com HTTP 1,1 de acordo com a RFC:<http://www.w3.org/Protocols/rfc2616/rfc2616.html>                                                                                                                                                                
> Especificamente, o servidor Web precisa dar suporte a                                                                                                                                                                                                                                       Solicitações [**Head**](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) e [**Get**](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.htm)<br>                                                                                                                                                                                                                                                                                                  -Solicitações de intervalo parcial<br>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   -Keep-Alive<br>                                                                                                                                                                                                                                                                                                                                                                                                                            -Não use "transferência-codificação: em partes"                                                                                                 

#### <a name="populate-the-isv-file-cache"></a>Popular o cache de arquivos do ISV

O cache de arquivos do ISV deve ser preenchido com arquivos associados às atualizações a serem instaladas em clientes gerenciados. 

**Para popular o cache de arquivos do ISV:**

1. Use as [APIs do WSUS](https://msdn.microsoft.com/library/windows/desktop/microsoft.updateservices.administration.updatefile(v=vs.85).aspx) para acessar o caminho do arquivo e o nome do arquivo da atualização para o serviço Mu.

    Os metadados de cada atualização no servidor do WSUS contêm o caminho do arquivo e o nome do arquivo da atualização em Microsoft Update da seguinte maneira (Microsoft Update nome do host em negrito, seguido **<http://download.windowsupdate.com>** por caminho e nome do arquivo):/c/msdownload/update/software/updt/2016/09/ Windows 10.0-kb3195781-x64_0c06079bccc35cba35a48bd2b1ec46f818bd2e74. msu

2. Baixe arquivos de Microsoft Update e armazene-os no cache de arquivos do ISV usando um destes dois métodos: 

   - Armazenar arquivos usando o **mesmo caminho de pasta do serviço MU**

   - Armazenar arquivos usando um **caminho de pasta definido pelo ISV**

     Ter o servidor HTTP (ou localhost) redirecionar solicitações **http Get** , que referenciam o caminho da pasta MU e o nome do arquivo para o local do arquivo ISV.

### <a name="BKMK_3"></a>Etapa 3: Configurar um agente cliente ISV para direcionar as operações do cliente WU

O agente cliente ISV orquestra o download e a instalação de atualizações aprovadas usando o seguinte fluxo de trabalho recomendado:

1.  O agente cliente do ISV chama o cliente WU para verificar no servidor do WSUS

2.  A verificação retorna o conjunto de atualizações aplicáveis para o cliente WU

3.  O cliente ISV determina quais atualizações aprovar, baixar e instalar

4.  O agente cliente ISV chama o cliente WU para baixar as atualizações aprovadas

5.  Depois que as atualizações forem baixadas, o agente cliente ISV chamará o cliente WU para instalar as atualizações aprovadas

Consulte [pesquisando, baixando e instalando atualizações](https://msdn.microsoft.com/library/windows/desktop/aa387102(v=vs.85).aspx) para obter informações adicionais sobre como usar o cliente Wu para verificar, baixar e instalar atualizações.

### <a name="download-workflow-options"></a>Baixar opções de fluxo de trabalho

A seguir, há duas ilustrações de baixar opções de fluxo de trabalho de um cache de arquivos do ISV:

![Fluxo de trabalho 1](../../media/express-update-delivery-isv-support/image1.png)

![Fluxo de trabalho 2](../../media/express-update-delivery-isv-support/image2.png)
### <a name="how-express-download-works"></a>Como o download Express funciona

- Para atualizações do sistema operacional com suporte ao Express, há duas versões da carga de arquivos armazenados no serviço:

  - **Versão de arquivo completo** --basicamente substituindo as versões locais dos binários de atualização

  - **Versão Express** --contendo os deltas necessários para corrigir os binários existentes no dispositivo. 

    A versão de arquivo completo e a versão expressa são referenciadas nos metadados da atualização, que foram baixados para o cliente como parte da fase de verificação. 

    **O download do Express funciona da seguinte maneira:**

    O cliente WU tentará baixar o Express primeiro e, em determinadas situações, retornará ao arquivo completo, se necessário (por exemplo, se passar por um proxy que não dá suporte a solicitações de intervalo de bytes).

  1. Quando o cliente WU inicia um download expresso, **o cliente Wu primeiro baixa um stub**, que faz parte do pacote expresso.

  2. **O cliente Wu passa esse stub para o Windows Installer**, que usa o stub para fazer um inventário local, comparando os deltas do arquivo no dispositivo com o que é necessário para chegar à versão mais recente do arquivo que está sendo oferecido.

  3. **Em seguida, o Windows Installer solicita que o cliente Wu Baixe os intervalos** que foram determinados para serem necessários.

  4. **O cliente Wu baixa esses intervalos e os passa para o Windows Installer**, que aplica os intervalos e, em seguida, determina se são necessários intervalos adicionais. Isso se repete até que o Windows Installer informe ao cliente WU que todos os intervalos necessários foram baixados.

  Neste ponto, o download é concluído, e a atualização está pronta para ser instalada.

### <a name="how-delivery-optimization-reduces-bandwidth-consumption"></a>Como a otimização de entrega reduz o consumo de largura de banda

A otimização de entrega (DO) é uma solução de cache distribuído de organização individual para empresas que buscam reduzir o consumo de largura de banda para atualizações de sistema operacional, atualizações de sistema operacional e aplicativos. Permite que os clientes baixem esses elementos de fontes alternativas (como outros pares na rede) em conjunto com o local de download especificado (o cache de arquivos do ISV neste cenário).

Por padrão, no Windows 10 Enterprise e Education, o permite o compartilhamento ponto a ponto somente na rede da organização, mas você pode configurá-lo de maneira diferente usando as configurações de Política de Grupo e de MDM (gerenciamento de dispositivo móvel).

Consulte [Configurar a otimização de entrega para atualizações do Windows 10](https://technet.microsoft.com/itpro/windows/manage/waas-delivery-optimization) para obter mais informações sobre o.
