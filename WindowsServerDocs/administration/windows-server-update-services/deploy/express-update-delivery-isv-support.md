---
title: Suporte a ISV para entrega de atualização Express
description: Tópico do WSUS (Windows Server Update Service) – como ISVs (fornecedores independentes de software) podem configurar a entrega de atualização expressa usando o WSUS
ms.prod: windows-server
ms.technology: manage-wsus
ms.topic: get-started article
author: sakitong
ms.author: coreyp
manager: lizapo
ms.date: 10/16/2017
ms.openlocfilehash: 60d01ef425ed96160cd76afdd7c27c081c778add
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/23/2020
ms.locfileid: "80828769"
---
# <a name="express-update-delivery-isv-support"></a>Suporte a ISV para entrega de atualização Express

>Aplica-se a: Windows 10, Windows Server 2016

Os downloads do Windows 10 Update podem ser grandes, pois cada pacote contém todas as correções lançadas anteriormente para garantir a consistência e a simplicidade.  

Desde a versão 7, o Windows conseguiu reduzir o tamanho dos downloads do Windows Update com um recurso chamado [Express](https://technet.microsoft.com/library/cc708456(v=ws.10).aspx#Anchor_2) e, embora os dispositivos do consumidor sejam compatíveis com ele por padrão, os dispositivos Windows 10 Enterprise precisam do Windows Server Update Services (WSUS) para utilizar o Express.

## <a name="how-microsoft-supports-express"></a>Como a Microsoft dá suporte ao Express

- **Express no WSUS Autônomo**

    A entrega de atualização Express já está disponível em [todas as versões com suporte do WSUS](https://technet.microsoft.com/library/cc708456(v=ws.10).aspx).

- **Express em dispositivos conectados diretamente ao Windows Update** 

    Os dispositivos de consumo dão suporte ao download Express: eles usam o cliente WU (Windows Update) para verificar, baixar e instalar atualizações. Durante a fase de download, o cliente WU solicita pacotes Express e baixa os intervalos de bytes apropriados.

-  **Dispositivos corporativos gerenciados usando o [Windows Update para Empresas](https://technet.microsoft.com/itpro/windows/manage/waas-manage-updates-wufb)** também obtêm o benefício de suporte de entrega de atualização Express sem qualquer alteração na configuração.

## <a name="how-isvs-can-take-advantage-of-express"></a>Como os ISVs podem aproveitar o Express

Os ISVs podem usar o WSUS e o cliente WU para dar suporte à entrega de atualização Express. A Microsoft recomenda as três etapas a seguir, cada uma discutida mais detalhadamente nas seções abaixo:

1.  [**Configurar o WSUS**](#BKMK_1)

    O servidor do WSUS é necessário para verificações e sincronizações de atualização (informações adicionais podem ser encontradas [aqui](https://technet.microsoft.com/library/dn800972(v=ws.11).aspx))

2.  [**Especificar e preenche um cache de arquivos do ISV**](#BKMK_2)

    Um cache de arquivos do ISV é recomendado para hospedar o conteúdo de atualização, que inclui os arquivos de gabinete de atualização (arquivos .cab) e os pacotes Express (arquivos .psf).

3.  [**Configurar um agente cliente ISV para direcionar as operações do cliente WU**](#BKMK_3)

>[!NOTE]
>Requer que a atualização cumulativa para a versão 1607 do Windows 10 em janeiro de 2017 (ou depois) [KB3213986 (Build do SO 14393.693)](https://support.microsoft.com/help/4009938/january-10-2017-kb3213986-os-build-14393-693) seja instalada.
    
   - O agente cliente ISV determina quais atualizações aprovar e quando baixar e instalar atualizações
   - O cliente WU determina os intervalos de bytes para baixar e inicia a solicitação de download

### <a name="step-1-configure-wsus"></a><a name=BKMK_1></a>Etapa 1: Configurar o WSUS

O WSUS serve como a interface para o Windows Update e gerencia todos os metadados que descrevem os pacotes Express que precisam ser baixados. Se você precisar implantar, confira [**Visão geral do Windows Server Update Services 3.0 SP2**](https://technet.microsoft.com/library/dd939931(v=ws.10).aspx). Depois que o WSUS tiver sido implantado, a principal consideração será armazenar ou não o conteúdo da atualização localmente no servidor do WSUS. Ao configurar o WSUS, recomendamos não armazenar atualizações localmente. Isso pressupõe que você já tenha a implantação de direcionamento de software desses pacotes em seu ambiente. Para saber mais sobre como configurar o armazenamento local do WSUS, confira [**Determinar em que local armazenar atualizações**](https://technet.microsoft.com/library/cc720494(v=ws.10).aspx).

### <a name="step-2-specify-and-populate-the-isv-file-cache"></a><a name=BKMK_2></a>Etapa 2: Especificar e preencher um cache de arquivos do ISV 

#### <a name="specify-the-isv-file-cache"></a>Especificar o cache de arquivos do ISV

Novas configurações de Política de Grupo do cliente e de MDM (gerenciamento de dispositivo móvel) detalhadas na [**Referência do provedor de serviços de configuração**](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/configuration-service-provider-reference) definem o local do cache de arquivos do ISV.

| **Nome**                                              | **Descrição**                                                                                                                                                      |
|-------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Configure um local de download alternativo para atualizações. | Especifica um servidor de intranet alternativo para hospedar atualizações do Microsoft Update. Você pode então usar esse serviço de atualização para atualizar automaticamente os computadores em sua rede |

Há duas opções ao configurar o local de download alternativo para o cache de arquivos do ISV:

1. **Especifique um nome de host do servidor HTTP do ISV**, que é o cache de arquivos do ISV
    
    Essa abordagem configura o cliente WU para fazer solicitações de download para o servidor HTTP especificado na política

2. **Especificar localhost**
 
    Essa abordagem configura o cliente WU para fazer solicitações de download no localhost. Isso permite que o agente cliente ISV manipule essas solicitações e roteie-as conforme apropriado para atender à solicitação de download.

> [!IMPORTANT]
> O cache de arquivos do ISV requer o seguinte:                                                          
> - O servidor deve estar em conformidade com HTTP 1.1 de acordo com a RFC: <http://www.w3.org/Protocols/rfc2616/rfc2616.html>                                                                                                                                                                
> Especificamente, o servidor Web precisa dar suporte a solicitações                                                                                                                                                                                                                                       [**HEAD**](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) e [**GET**](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.htm)<br>                                                                                                                                                                                                                                                                                                  - Solicitações de intervalo parcial<br>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   - Keep-alive<br>                                                                                                                                                                                                                                                                                                                                                                                                                            – Não use Transfer-Encoding:chunked                                                                                                 

#### <a name="populate-the-isv-file-cache"></a>Preencher o cache de arquivos do ISV

O cache de arquivos do ISV deve ser preenchido com arquivos associados às atualizações a serem instaladas em clientes gerenciados. 

**Para preencher o cache de arquivos do ISV:**

1. Use as [APIs do WSUS](https://msdn.microsoft.com/library/windows/desktop/microsoft.updateservices.administration.updatefile(v=vs.85).aspx) para acessar o caminho do arquivo e o nome do arquivo da atualização para o serviço MU.

    Os metadados de cada atualização no servidor do WSUS contêm o caminho do arquivo e o nome do arquivo da atualização no Microsoft Update da seguinte maneira (nome de host do Microsoft Update em negrito, seguido pelo caminho do arquivo e o nome do arquivo): **<http://download.windowsupdate.com>** /c/msdownload/update/software/updt/2016/09/windows10.0-kb3195781-x64_0c06079bccc35cba35a48bd2b1ec46f818bd2e74.msu

2. Baixe arquivos do Microsoft Update e armazene-os no cache de arquivos do ISV usando um destes dois métodos: 

   - Armazenar arquivos usando o **mesmo caminho de pasta do serviço MU**

   - Armazenar arquivos usando um **caminho de pasta definido pelo ISV**

     Providencie que o servidor HTTP (ou localhost) redirecione solicitações **HTTP GET**, que fazem referência ao caminho da pasta MU e ao nome do arquivo para o local do arquivo ISV.

### <a name="step-3-set-up-an-isv-client-agent-to-direct-wu-client-operations"></a><a name=BKMK_3></a>Etapa 3: Configurar um agente cliente ISV para direcionar as operações do cliente WU

O agente cliente ISV orquestra o download e a instalação de atualizações aprovadas usando o seguinte fluxo de trabalho recomendado:

1.  O agente cliente do ISV chama o cliente WU para verificar no servidor do WSUS

2.  A verificação retorna o conjunto de atualizações aplicáveis ao cliente WU

3.  O cliente ISV determina quais atualizações aprovar, baixar e instalar

4.  O agente cliente ISV chama o cliente WU para baixar as atualizações aprovadas

5.  Depois que as atualizações forem baixadas, o agente cliente ISV chamará o cliente WU para instalar as atualizações aprovadas

Confira [Como pesquisar, baixar e instalar atualizações](https://msdn.microsoft.com/library/windows/desktop/aa387102(v=vs.85).aspx) para obter informações adicionais sobre como usar o cliente WU para verificar, baixar e instalar atualizações.

### <a name="download-workflow-options"></a>Opções do fluxo de trabalho de download

A seguir, há duas ilustrações das opções de fluxo de trabalho de download de um cache de arquivos do ISV:

![Fluxo de trabalho 1](../../media/express-update-delivery-isv-support/image1.png)

![Fluxo de trabalho 2](../../media/express-update-delivery-isv-support/image2.png)
### <a name="how-express-download-works"></a>Como o download do Express funciona

- Para atualizações do sistema operacional com suporte ao Express, há duas versões do conteúdo de arquivos armazenados no serviço:

  - **Versão de arquivos completos** – essencialmente substituindo as versões locais dos binários de atualização

  - **Versão Express** – contendo os deltas necessários para corrigir os binários existentes no dispositivo. 

    A versão de arquivos completos e a versão Express são referenciadas nos metadados da atualização, que foi baixada para o cliente como parte da fase de Verificação. 

    **O download Express funciona da seguinte maneira:**

    O cliente WU tentará baixar por meio do Express primeiro e, em determinadas situações, recorrerá aos arquivos completos, se necessário (por exemplo, se usar um proxy que não dê suporte a solicitações de intervalos de bytes).

  1. Quando o cliente WU inicia um download Express, **o cliente WU primeiro baixa um stub**, que faz parte do pacote Express.

  2. **O cliente WU passa esse stub de código para o Windows installer**, que usa o stub para fazer um inventário local, comparando os deltas do arquivo no dispositivo com o que é necessário para obter a versão mais recente do arquivo que está sendo oferecido.

  3. O **Windows installer solicita que o cliente Windows Update baixe os intervalos** que foram determinados como necessários.

  4. **O cliente WU baixa esses intervalos e os passa para o Windows Installer**, que aplica os intervalos e, em seguida, determina se intervalos adicionais são necessários. Isso é repetido até que o Windows installer informe ao cliente WU que todos os intervalos necessários foram baixados.

  Neste ponto, o download é concluído e a atualização está pronta para ser instalada.

### <a name="how-delivery-optimization-reduces-bandwidth-consumption"></a>Como a otimização de entrega reduz o consumo de largura de banda

A DO (otimização de entrega) é uma solução de cache distribuído de organização individual para empresas que buscam reduzir o consumo de largura de banda para atualizações de sistema operacional, upgrades de sistema operacional e aplicativos. Permite que os clientes baixem esses elementos de fontes alternativas (como outros pares na rede) em conjunto com o local de download especificado (o cache de arquivos do ISV neste cenário).

Por padrão, no Windows 10 Enterprise e Education, a DO permite o compartilhamento ponto a ponto somente na rede da organização, mas você pode configurá-la de forma diferente usando a Política de Grupo e configurações de MDM (gerenciamento de dispositivo móvel).

Confira [Configurar a otimização de entrega para atualizações do Windows 10](https://technet.microsoft.com/itpro/windows/manage/waas-delivery-optimization) para obter mais informações sobre DO.
