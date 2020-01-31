---
title: BranchCache
description: Este tópico fornece uma visão geral do BranchCache no Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-bc
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a4587cff-c086-49f1-a0bf-cd74b8a44440
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 15d57d12679d7441da080ad671264ca1e5e1f42c
ms.sourcegitcommit: 07c9d4ea72528401314e2789e3bc2e688fc96001
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/29/2020
ms.locfileid: "76822799"
---
# <a name="branchcache"></a>BranchCache

>Aplicável a: Windows Server (canal semestral), Windows Server 2016

Este tópico, que é direcionado a profissionais de TI (tecnologia da informação), fornece informações gerais sobre o BranchCache, incluindo os modos, os recursos e as capacidades do BranchCache, bem como a funcionalidade do BranchCache disponível em sistemas operacionais diferentes.

> [!NOTE]
> Além deste tópico, a seguinte documentação do BranchCache está disponível.
> 
> - [Comandos do Shell de rede do BranchCache e do Windows PowerShell](../branchcache/BranchCache-Network-Shell-and-Windows-PowerShell-Commands.md)
> -   [Guia de implantação do BranchCache](../branchcache/deploy/BranchCache-Deployment-Guide.md)

**Quem estará interessado no BranchCache?**

Se você for administrador do sistema, arquiteto de soluções de rede ou armazenamento ou outro profissional de TI, o BranchCache poderá ser útil nas seguintes ocasiões:

- Durante o design ou suporte de uma infraestrutura de TI para uma organização com dois ou mais locais físicos e uma conexão por WAN (rede de longa distância) das filiais para a matriz.

- Durante o design ou suporte de uma infraestrutura de TI para uma organização que implantou tecnologias na nuvem e tem uma conexão por WAN usada por funcionários para acessar dados e aplicativos em locais remotos.

- Você deseja otimizar o uso de largura de banda de WAN por meio da redução da quantidade de tráfego de rede entre as filiais e a matriz.

- Você implantou ou planeja implantar, na matriz, servidores de conteúdo que correspondam ás configurações descritas neste tópico.

- Os computadores cliente em suas filiais estão executando o Windows 10, Windows 8.1, Windows 8 ou Windows 7.

Este tópico inclui as seguintes seções:

-   [O que é BranchCache?](#bkmk_what)

-   [Modos de BranchCache](#BKMK_2)
  
-   [Servidores de conteúdo habilitados para BranchCache](#BKMK_3)
  
-   [BranchCache e nuvem](#BKMK_3a)
  
-   [Versões de informações de conteúdo](#bkmk_version)  
  
-   [Como o BranchCache lida com atualizações de conteúdo em arquivos](#bkmk_handles)  
  
-   [Guia de instalação do BranchCache](#BKMK_4)  
  
-   [Versões do sistema operacional para BranchCache](#bkmk_os)  
  
-   [Segurança do BranchCache](#bkmk_security)  
  
-   [Fluxo de conteúdo e processos](#bkmk_flow)  
  
-   [Segurança de cache](#bkmk_cache)  
  
## <a name="bkmk_what"></a>O que é BranchCache?

O BranchCache é uma tecnologia de otimização de largura de banda de WAN (rede de longa distância) incluída em algumas edições dos sistemas operacionais Windows Server 2016 e Windows 10, bem como em algumas edições do Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8 , Windows Server 2008 R2 e Windows 7. Para otimizar a largura de banda de WAN quando os usuários acessam conteúdo em servidores remotos, o BranchCache busca o conteúdo dos servidores de conteúdo da matriz ou da nuvem hospedada e o armazena em cache nas filiais, permitindo que os computadores cliente das filiais acessem o conteúdo localmente e não pela WAN.
  
Em filiais, o conteúdo é armazenado em servidores que estão configurados para hospedar o cache ou, quando nenhum servidor está disponível na filial, em computadores cliente que executam o Windows 10, Windows 8.1, Windows 8 ou Windows 7. Depois que um computador cliente solicitar e receber o conteúdo da matriz e o conteúdo for armazenado em cache na filial, outros computadores da mesma filial poderão obter o conteúdo localmente em vez baixar o conteúdo do servidor de conteúdo pelo link WAN.

Quando solicitações subsequentes do mesmo conteúdo são realizadas pelos computadores clientes, são baixadas do servidor *informações sobre o conteúdo* em vez do conteúdo propriamente dito. As informações de conteúdo consistem em hashes calculados usando partes do conteúdo original. Elas são extremamente pequenas quando comparadas ao conteúdo nos dados originais. Os computadores clientes usam as informações de conteúdo para localizar o conteúdo de um cache na filial, esteja ela localizada em um computador cliente ou servidor. Os computadores clientes e servidores também usam informações de conteúdo para proteger conteúdo em cache, de modo que ele não possa ser acessado por usuários não autorizados.

O BranchCache aumenta a produtividade dos usuários finais, melhorando os tempos de resposta a consultas de conteúdo para clientes e servidores em filiais. Além disso, ele também pode ajudar a aprimorar o desempenho de rede por meio da redução do tráfego por links WAN.

## <a name="BKMK_2"></a>Modos de BranchCache
O BranchCache tem dois modos de operação: o modo de cache distribuído e o modo de cache hospedado.

Quando você implanta o BranchCache no modo de cache distribuído, o cache de conteúdo na filial é distribuída entre os computadores clientes.

Quando você implanta o BranchCache no modo de cache hospedado, o cache de conteúdo em uma filial é hospedado em um ou mais servidores, que são chamados de servidores de cache hospedado.

> [!NOTE]
> Você pode implantar o BranchCache usando os dois modos, mas apenas um deles pode ser usado por filial. Por exemplo, se você tiver duas filiais, uma com um servidor e outra sem, será possível implantar o BranchCache no modo de cache hospedado no escritório que possui o servidor e implantar a solução no modo de cache distribuído no escritório que tem apenas computadores clientes.

Na ilustração a seguir, o BranchCache é implantado nos dois modos.  

![Modos de BranchCache](../media/BranchCache/bc_modes.jpg)

O modo de cache distribuído é mais adequado para filiais pequenas que não possuem servidor local para uso como servidor da cache hospedado. O modo de cache distribuído permite a implantação do BranchCache nas filiais sem hardware adicional.

Se a filial na qual você deseja implantar o BranchCache contiver infraestrutura adicional, como um ou mais servidores executando outras cargas de trabalho, a implantação do BranchCache em modo de cache hospedado será vantajosa pelos seguintes motivos:

### <a name="increased-cache-availability"></a>Maior disponibilidade de cache

O modo de cache hospedado aumenta a eficiência de cache porque o conteúdo estará disponível mesmo se o cliente que solicitou e armazenou os dados em cache originalmente estiver offline. Como o servidor de cache hospedado está sempre disponível, mais conteúdo é armazenado em cache, proporcionando maiores economias de largura de banda de WAN e aumentando a eficiência do BranchCache.

### <a name="centralized-caching-for-multiple-subnet-branch-offices"></a>Cache centralizado para filiais com várias sub-redes


O modo de cache distribuído funciona em uma única sub-rede. Em uma filial com várias sub-redes configurada para o modo de cache distribuído, um arquivo baixado em uma sub-rede não poder ser compartilhado com computadores clientes em outras sub-redes. 

Em função disso, os clientes em outras sub-redes, incapazes de descobrirem que o arquivo já foi baixado, obtêm o arquivo do servidor de conteúdo da matriz, usando a largura de banda de WAN no processo.

Porém, não é o caso quando você implanta o modo de cache hospedado, pois todos os clientes em uma filial com várias sub-redes podem acessar um único cache, armazenado no servidor de cache hospedado, mesmo que os clientes estejam em sub-redes diferentes. Além disso, o BranchCache no Windows Server 2016, no Windows Server 2012 R2 e no Windows Server 2012 fornece a capacidade de implantar mais de um servidor de cache hospedado por filial.

> [!CAUTION]
> Se você usa o BranchCache para cache SMB de arquivos e pastas, não desabilite Arquivos Offline. Se você desabilitar Arquivos Offline, o cache SMB do BranchCache não funcionará corretamente.

## <a name="BKMK_3"></a>Servidores de conteúdo habilitados para BranchCache

Quando você implanta o BranchCache, o conteúdo de origem é armazenado em servidores de conteúdo habilitados para BranchCache em seu escritório principal ou em uma nuvem data center. Os seguintes tipos de servidores de conteúdo são suportados pelo BranchCache:

> [!NOTE]
> Somente o conteúdo de origem, ou seja, o conteúdo que os computadores cliente obtêm inicialmente de um servidor de conteúdo habilitado para BranchCache, é acelerado pelo BranchCache. O conteúdo que os computadores clientes obtêm diretamente de outras origens, como servidores Web na Internet ou Windows Update, não é armazenado em cache por computadores clientes ou servidores de cache hospedado, nem compartilhado com outros computadores na filial. No entanto, se você quiser acelerar o conteúdo do Windows Update, poderá instalar um servidor de aplicativos do Windows Server Update Services (WSUS) em seu escritório principal ou na nuvem data center e configurá-lo como um servidor de conteúdo do BranchCache.

### <a name="web-servers"></a>Servidores da Web

Os servidores Web com suporte incluem computadores que executam o Windows Server 2016, o Windows Server 2012 R2, o Windows Server 2012 ou o Windows Server 2008 R2 que têm a função de servidor servidor Web (IIS) instalada e que usam HTTP (Hypertext Transfer Protocol) ou HTTP Secure ( HTTPS).

Além disso, o servidor Web deve ter o recurso BranchCache instalado.

### <a name="file-servers"></a>Servidores de arquivos

Os servidores de arquivos com suporte incluem computadores que executam o Windows Server 2016, o Windows Server 2012 R2, o Windows Server 2012 ou o Windows Server 2008 R2 que têm a função de servidor de serviços de arquivo e o BranchCache para o serviço de função de arquivos de rede instalados. 

Esses servidores de arquivos usam o protocolo SMB (Server Message Block) para trocar informações entre computadores. Depois de concluir a instalação do servidor de arquivos, você também deve compartilhar pastas e habilitar a geração de hash para pastas compartilhadas usando a Política de Grupo ou Política de Grupo Local para habilitar o BranchCache.

### <a name="application-servers"></a>Servidores de aplicativos

Os servidores de aplicativos com suporte incluem computadores que executam o Windows Server 2016, o Windows Server 2012 R2, o Windows Server 2012 ou o Windows Server 2008 R2 com o Serviço de Transferência Inteligente em Segundo Plano (BITS) instalado e habilitado. 

Além disso, o servidor de aplicativos deve ter o recurso BranchCache instalado. Como exemplos de servidores de aplicativos, você pode implantar servidores de ponto de distribuição de ramificação do Microsoft Windows Server Update Services (WSUS) e Microsoft Endpoint Configuration Manager como servidores de conteúdo do BranchCache.

## <a name="BKMK_3a"></a>BranchCache e nuvem

A nuvem tem um grande potencial de redução das despesas operacionais e obtenção de novos níveis de dimensionamento. Porém, afastar as cargas de trabalho das pessoas que dependem delas podem aumentar os custos de rede e prejudicar a produtividade. Os usuários esperam alto desempenho e não se preocupam onde seus aplicativos e dados estão hospedados. 

O BranchCache pode aprimorar o desempenho de aplicativos em rede e reduzir o consumo de largura de banda com um cache compartilhado de dados.  Ele aumenta a produtividade em filiais e matrizes, nas quais os funcionários usam servidores implantados na nuvem.

Como o BranchCache não exige novo hardware nem mudanças na topologia de rede, ele é uma solução excelente para melhorar as comunicações entre matrizes em nuvens privadas ou públicas.

> [!NOTE]
> Como alguns proxies da Web não podem processar cabeçalhos de codificação de conteúdo não padrão, é recomendável que você use o BranchCache com o protocolo HTTPS e não HTTP.
  
= = = = = = = Para obter mais informações sobre tecnologias de nuvem no Windows Server 2016, consulte [rede &#40;definida&#41;pelo software Sdn](../sdn/Software-Defined-Networking--SDN-.md).
  
## <a name="bkmk_version"></a>Versões de informações de conteúdo

Há duas versões das informações de conteúdo:

- As informações de conteúdo que são compatíveis com computadores que executam o Windows Server 2008 R2 e o Windows 7 são chamadas de versão 1 ou v1. Com a segmentação de arquivos do BranchCache V1, os segmentos de arquivos são maiores do que na V2 e possuem tamanho fixo. Por causa dos grandes segmentos fixos, quando um usuário faz uma alteração que modifica ao tamanho do arquivo, o segmento com a alteração é invalidado, bem como todos os segmentos no fim do arquivo. A próxima chamada ao arquivo alterado feita por outro usuário na filial leva à redução da economia de largura de banda de WAN, pois o conteúdo alterado e todo o conteúdo após a alteração são enviados pelo link WAN.

- As informações de conteúdo que são compatíveis com computadores que executam o Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012 e Windows 8 são chamadas de versão 2, ou v2. As informações de conteúdo V2 utilizam segmentos menores e de tamanho variável, mais tolerantes a alterações em um arquivo. Isso aumenta a probabilidade de reutilização de segmentos de uma versão anterior do arquivo quando os usuários acessam uma versão atualizada, fazendo com que eles recuperem somente a parte alterada do arquivo do servidor de conteúdo e usem menos largura de banda da WAN.

A tabela a seguir fornece informações sobre a versão de informações de conteúdo usada, dependendo dos sistemas operacionais de cliente, servidor de conteúdo e servidor de cache utilizados na implantação do BranchCache.

> [!NOTE]
> Na tabela a seguir, o acrônimo "so" significa sistema operacional.

|SO cliente|SO de servidor de conteúdo|SO de servidor de cache hospedado|Versões das informações de conteúdo|
|-------------|---------------------|--------------------------|-------------------------------|
|Windows Server 2008 R2 e Windows 7 |Windows Server 2012 ou posterior|Windows Server 2012 ou posterior; nenhum para o modo de cache distribuído|V1|
|Windows Server 2012 ou posterior; Windows 8 ou posterior|Windows Server 2008 R2 |Windows Server 2012 ou posterior; nenhum para o modo de cache distribuído|V1|
|Windows Server 2012 ou posterior; Windows 8 ou posterior| Windows Server 2012 ou posterior| Windows Server 2008 R2 |V1|
|Windows Server 2012 ou posterior; Windows 8 ou posterior|Windows Server 2012 ou posterior|Windows Server 2012 ou posterior; nenhum para o modo de cache distribuído|V2|

Quando você tem servidores de conteúdo e servidores de cache hospedados que executam o Windows Server 2016, o Windows Server 2012 R2 e o Windows Server 2012, eles usam a versão de informações de conteúdo que é apropriada com base no sistema operacional do cliente do BranchCache que solicita informações. 

Quando os computadores que executam o Windows Server 2012 e o Windows 8 ou sistemas operacionais posteriores solicitam conteúdo, o conteúdo e os servidores de cache hospedados usam informações de conteúdo v2; Quando os computadores que executam o Windows Server 2008 R2 e o Windows 7 solicitam conteúdo, o conteúdo e os servidores de cache hospedados usam informações de conteúdo v1.

>[!IMPORTANT]
>Quando você implanta o BranchCache no modo de cache distribuído, os clientes que utilizam versões de informações de conteúdo diferentes não compartilham conteúdo uns com os outros. Por exemplo, um computador cliente que executa o Windows 7 e um computador cliente executando o Windows 10 que estão instalados na mesma filial não compartilham conteúdo entre si.
  
## <a name="bkmk_handles"></a>Como o BranchCache lida com atualizações de conteúdo em arquivos

Quando os usuários da filial modificam ou atualizam o conteúdo dos documentos, suas alterações são gravadas diretamente no servidor de conteúdo no escritório principal sem envolvimento do BranchCache. Isso ocorre independentemente se o usuário baixou o documento do servidor de conteúdo ou o obteve de um cache distribuído ou hospedado na filial.

Quando o arquivo modificado é solicitado por um cliente diferente em uma filial, os novos segmentos do arquivo são baixados do servidor da sede e adicionados ao cache distribuído ou hospedado na filial. Por isso, os usuários das filiais sempre recebem as versões mais recentes do conteúdo armazenado em cache.

## <a name="BKMK_4"></a>Guia de instalação do BranchCache

Você pode usar Gerenciador do Servidor no Windows Server 2016 para instalar o recurso BranchCache ou o serviço de função BranchCache para arquivos de rede da função de servidor serviços de arquivo. Você pode usar a tabela a seguir para determinar se deve instalar o serviço de função ou o recurso.

|Funcionalidade|Local no computador|Instalar este elemento do BranchCache|
|-----------------|---------------------|------------------------------------|
|O servidor de conteúdo \(servidor de aplicativos baseado em BITS\)|Matriz ou datacenter na nuvem|Recurso BranchCache|
|Servidor Web \(servidor de conteúdo\)|Matriz ou datacenter na nuvem|Recurso BranchCache|
|Servidor de conteúdo do \(servidor de arquivos usando o protocolo SMB\)|Matriz ou datacenter na nuvem|Serviço de função BranchCache para Arquivos de Rede da função de servidor Serviços de Arquivo|
|Servidor de cache hospedado|Filial|Recurso BranchCache com modo de servidor de cache hospedado habilitado|
|Computador cliente habilitado por BranchCache|Filial|Nenhuma instalação necessária; Basta habilitar o BranchCache e um modo de BranchCache \(\) distribuída ou hospedado no cliente|

Para instalar o serviço de função ou o recurso, abra o Gerenciador do Servidor e selecione os computadores nos quais deseja habilitar a funcionalidade BranchCache. No Gerenciador do Servidor, clique em **Gerenciar**e depois em **Adicionar Funções e Recursos**. O assistente **Adicionar Funções e Recursos** será aberto. Conforme executa o assistente, faça as seguintes seleções:

- Na página do assistente **Selecione o tipo de instalação**, selecione **Instalação Baseada em Função ou Recursos**.

- Na página do assistente, **selecione funções de servidor**, se você estiver instalando um servidor de arquivos habilitado para BranchCache, expanda **serviços de arquivo e armazenamento** e **serviços de arquivo e iSCSI**e, em seguida, selecione **BranchCache para arquivos de rede**.  Para economizar espaço em disco, você também pode selecionar o serviço de função **eliminação de duplicação de dados** e, em seguida, continuar o assistente para instalação e conclusão. Se você não quiser instalar um servidor de arquivos habilitado para BranchCache, não instale a função serviços de arquivo e armazenamento com o BranchCache para serviço de função de arquivos de rede.

- Na página do assistente, **selecione recursos**, se você estiver instalando um servidor de conteúdo que não seja um servidor de arquivos ou se estiver instalando um servidor de cache hospedado, selecione **BranchCache**e, em seguida, prossiga com o assistente para instalação e conclusão. Se não quiser instalar um servidor de conteúdo além do servidor de arquivos ou um servidor de cache hospedado, não instale o recurso BranchCache.
  
## <a name="bkmk_os"></a>Versões do sistema operacional para BranchCache

A seguir está uma lista de sistemas operacionais que dão tipos diferentes de suporte à funcionalidade BranchCache.

### <a name="operating-systems-for-branchcache-client-computer-functionality"></a>Sistemas operacionais para a funcionalidade de computador cliente BranchCache

Os sistemas operacionais a seguir fornecem o BranchCache com suporte para Serviço de Transferência Inteligente em Segundo Plano (BITS), protocolo de transferência de texto (HTTP) e SMB (bloco de mensagens de servidor).

- Windows 10 Enterprise

- Windows 10 Education

- Windows 8.1 Enterprise

- O Windows 8 Enterprise

- Windows 7 Enterprise

- Windows 7 Ultimate

Nos sistemas operacionais a seguir, o BranchCache não dá suporte à funcionalidade HTTP e SMB, mas dá suporte à funcionalidade de BITS do BranchCache.

-   Windows 10 pro, somente suporte a BITS

-   Windows 8.1 Pro, somente suporte a BITS

-   Windows 8 Pro, somente suporte a BITS

-   Windows 7 Pro, somente suporte a BITS

> [!NOTE]
> O BranchCache não está disponível por padrão nos sistemas operacionais Windows Server 2008 ou Windows Vista. Nesses sistemas operacionais, no entanto, se você baixar e instalar a atualização do Windows Management Framework, a funcionalidade do BranchCache estará disponível somente para o protocolo Serviço de Transferência Inteligente em Segundo Plano (BITS). Para obter mais informações e baixar o Windows Management Framework, consulte [Windows Management Framework (Windows PowerShell 2,0, WinRM 2,0 e BITS 4,0)](https://go.microsoft.com/fwlink/?LinkId=188677) em https://go.microsoft.com/fwlink/?LinkId=188677.
  
### <a name="operating-systems-for-branchcache-content-server-functionality"></a>Sistemas operacionais para a funcionalidade de servidor de conteúdo BranchCache

Você pode usar as famílias de sistemas operacionais Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012 como servidores de conteúdo do BranchCache.

Além disso, a família de sistemas operacionais Windows Server 2008 R2 pode ser usada como servidores de conteúdo do BranchCache, com as seguintes exceções:

- O BranchCache não tem suporte em instalações do Server Core do Windows Server 2008 R2 Enterprise com Hyper-V.

- O BranchCache não tem suporte em instalações do Server Core do Windows Server 2008 R2 Datacenter com o Hyper-V.

### <a name="operating-systems-for-branchcache-hosted-cache-server-functionality"></a>Sistemas operacionais para a funcionalidade de servidor de cache hospedado do BranchCache

Você pode usar as famílias de sistemas operacionais Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012 como servidores de cache hospedados do BranchCache.

Além disso, os seguintes sistemas operacionais Windows Server 2008 R2 podem ser usados como servidores de cache hospedados do BranchCache:

- Windows Server 2008 R2 Enterprise

- Windows Server 2008 R2 Enterprise com Hyper-V

- Instalação do Windows Server 2008 R2 Enterprise Server Core

- Instalação do Windows Server 2008 R2 Enterprise Server Core com Hyper-V

- Windows Server 2008 R2 for Itanium-Based Systems

- Windows Server 2008 R2 Datacenter

- Windows Server 2008 R2 Datacenter com Hyper-V

- Instalação principal do Windows Server 2008 R2 Datacenter Server com Hyper-V

## <a name="bkmk_security"></a>Segurança do BranchCache

O BranchCache implementa uma abordagem segura por design, que trabalha de forma ininterrupta juntamente com as arquiteturas de segurança de rede existentes, dispensando outros equipamentos ou configurações de segurança adicionais complexas.
  
O BranchCache não é invasivo e não altera processo de autenticação ou autorização do Windows. Após a implantação do BranchCache, a autenticação ainda é realizada usando credenciais de domínio, e o modo no qual a organização com ACLs (listas de controle de acesso) funciona é inalterado. Além disso, outras configurações continuam a funcionar do mesmo modo antes da implantação do BranchCache.

O modelo de segurança do BranchCache é baseado na criação de metadados, que assumem a forma de uma série de hashes. Esses hashes também são chamados de informações de conteúdo.

Depois que as informações de conteúdo são criadas, elas são usadas em trocas de mensagens do BranchCache no lugar dos dados reais e são trocadas usando os protocolos com suporte (HTTP, HTTPS e SMB).

Os dados em cache permanecem criptografados e não podem ser acessados por clientes sem permissão de acesso a conteúdo da origem original.  Os clientes devem ser autenticados e autorizados pela origem de conteúdo original antes que possam recuperar metadados de conteúdo. Além disso, eles devem possuir metadados de conteúdo para acessar o cache no escritório local.

### <a name="how-branchcache-generates-content-information"></a>Como o BranchCache gera as informações de conteúdo

Como as informações de conteúdo são criadas a partir de vários elementos, seu valor é sempre único. Esses elementos são:

- O conteúdo real (como páginas da Web ou arquivos compartilhados) do qual os hashes são derivados.  

- Parâmetros de configuração, como algoritmo de hash e tamanho do bloco. Para gerar informações de conteúdo, o servidor divide o conteúdo em segmentos e os subdivide em blocos. O BranchCache usa hashes criptográficos seguros para identificar e verificar cada bloco e segmento, com suporte ao algoritmo de hash SHA256.

- Um segredo do servidor. Todos os servidores de conteúdo devem ser configurados com um segredo do servidor, que é um valor binário de comprimento arbitrário.

> [!NOTE]
> O uso de um segredo do servidor garante que os computadores clientes não são capazes de gerar informações de conteúdo por conta própria. Isso evita que usuários mal-intencionados usem ataques de força bruta com computadores clientes habilitados pelo BranchCache para detectar alterações pequenas no conteúdo entre versões diferentes, em situações em que o cliente tinha acesso a uma verão anterior, mas não tem acesso à versão atual.

### <a name="content-information-details"></a>Detalhes das informações de conteúdo

O BranchCache usa o segredo do servidor como uma chave para derivar um hash específico de conteúdo, que é enviado a clientes autorizados. A aplicação de um algoritmo de hash ao segredo do servidor combinado e ao hash de dados gera esse hash.

Ele é chamado de segredo de segmento. O BranchCache usa segredos de segmento para proteger comunicações. Além disso, o BranchCache cria uma lista de hashes de blocos, com blocos de dados em hash, e o hash de dados que é gerado pelo hash dessa lista.

As informações de conteúdo incluem:

- A lista de hashes de blocos:

    `BlockHashi = Hash(dataBlocki)   1<=i<=n`

- O hash de dados (HoD):

    `HoD = Hash(BlockHashList)`

- Segredo de segmento (Kp):

    `Kp = HMAC(Ks, HoD)`

O BranchCache usa o protocolo de Cache de Conteúdo de Ponta e o protocolo de Estrutura de Recuperação a fim de implementar os processos necessários para garantir a segurança do armazenamento em cache e recuperação de dados entre caches de conteúdo.

Além disso, o BranchCache manipula as informações de conteúdo com o mesmo nível de segurança usado ao manipular e transmitir o próprio conteúdo real.

## <a name="bkmk_flow"></a>Fluxo de conteúdo e processos

O fluxo das informações de conteúdo e do conteúdo real é dividido em quatro fases:

1.  [Processos do BranchCache: conteúdo da solicitação](#BKMK_8)

2.  [Processos do BranchCache: localizar conteúdo](#BKMK_9)

3.  [Processos do BranchCache: recuperar conteúdo](#BKMK_10)

4.  [Processos do BranchCache: conteúdo do cache](#BKMK_11)

As seções a seguir descrevem essas fases.

## <a name="BKMK_8"></a>Processos do BranchCache: conteúdo da solicitação

Na primeira fase, o computador cliente na filial solicita conteúdo (como um arquivo ou página da Web) de um servidor de conteúdo em um local remoto, como uma matriz. O servidor de conteúdo verifica se o computador cliente está autorizado a receber o conteúdo solicitado. Se o computador cliente estiver autorizado e o servidor de conteúdo e o cliente estiverem com o BranchCache\-habilitado, o servidor de conteúdo gerará informações de conteúdo.

Em seguida, o servidor de conteúdo envia as informações de conteúdo ao computador cliente usando o mesmo protocolo que seria usado para o conteúdo real. 

Por exemplo, se o computador cliente tiver solicitado uma página da Web por HTTP, o servidor de conteúdo enviará as informações de conteúdo usando HTTP. Por causa disso, as garantias de segurança em nível de transmissão do conteúdo e das informações de conteúdo são idênticas.

Após o recebimento da porção inicial das informações de conteúdo (hash de dados + segredo de segmento), o computador cliente realiza a seguintes ações:

- Use o segredo de segmento (Kp) como chave de criptografia (Ke).

- Gera o ID do segmento (HoHoDk) do HoD e Kp:

    `HoHoDk = HMAC(Kp, HoD + C), where C is the ASCII string "MS_P2P_CACHING" with NUL terminator.`

A principal ameaça nesta camada é o risco para o segredo de segmento, mas o BranchCache criptografa os blocos de dados de conteúdo para protegê-lo. O BranchCache faz isso usando a chave de criptografia derivada do segredo do segmento de conteúdo no qual os blocos de conteúdo estão localizados.

Essa abordagem garante que uma entidade que não possua o segredo de segmento não possa descobrir o conteúdo real em um bloco de dados. O segredo de segmento é tratado com o mesmo nível de segurança que o segmento em texto não criptografado, pois o conhecimento do segredo de um determinado segmento permite que uma entidade obtenha o segmento a partir de pares e o descriptografe. O conhecimento do segredo de servidor não produz texto não criptografados, mas pode ser usado paras derivar certos tipos de dados do texto codificado e possivelmente expor dados parcialmente conhecidos a um ataque de adivinhação pro força bruta. Por isso, o segredo de servidor deve ser confidencial.
  
## <a name="BKMK_9"></a>Processos do BranchCache: localizar conteúdo

Depois que as informações de conteúdo são recebidas pelo computador cliente, o cliente usa o ID do segmento para localizar o conteúdo solicitado no cache local da filial, esteja ele distribuído entre computadores clientes ou localizado em um servidor de cache hospedado.

Se o computador cliente estiver configurado para o modo de cache hospedado, ele será configurado com o nome do computador do servidor de cache hospedado e entrará em contato com o servidor para recuperar o conteúdo.

Porém, se o computador cliente estiver configurado para o modo de cache distribuído, o conteúdo poderá ser armazenados entre vários caches em diversos computadores da filial. O computador cliente deve descobrir onde o conteúdo está localizado antes de recuperá-lo.

Quando estão configurados para o modo de cache distribuído, os computadores clientes localizam conteúdo usando um protocolo de descoberta baseado no protocolo WS-Discovery. Os clientes enviam mensagens de sondagem multicast WS-Discovery para descobrir conteúdo em cache pela rede. Essas mensagens incluem o ID do segmento, que permite que os clientes verifique se o conteúdo solicitado corresponde ao armazenado em cache. Os clientes que recebem a mensagem de sondagem inicial respondem ao cliente da consulta com mensagens de correspondência de sondagem unicast quando o ID do segmento corresponde ao conteúdo armazenado em cache local.  

Para que o processo WS-Discovery obtenha êxito, o cliente que realiza a descoberta deve ter as informações de conteúdo corretas (fornecidas pelo servidor de conteúdo) para o conteúdo solicitado.

A ameaça principal aos dados durante a fase de solicitação de conteúdo é a divulgação das informações, pois o acesso às informações de conteúdo implica em acesso autorizado ao conteúdo. Para eliminar esse risco, o processo de descoberta não revela as informações de conteúdo, somente o ID do segmento, que não revela nada sobre o segmento em texto não criptografado com o conteúdo.

Além disso, outro computador cliente executado por um usuário mal-intencionado na mesma sub-rede pode ver o tráfego de descoberta do BranchCache para a origem de conteúdo original passando pelo roteador.

Se o conteúdo solicitado não for encontrado na filial, o cliente solicitará o conteúdo diretamente do servidor de conteúdo pelo link WAN.

Depois que o conteúdo é recebido, ele é adicionado ao cache local, seja no computador cliente ou no servidor de cache hospedado. Nesse caso, as informações de conteúdo evitam que um cliente ou servidor de cache hospedado adicione ao cache local qualquer conteúdo que não corresponda aos hashes. O processo de verificação do conteúdo por meio da correspondência de hashes garante que apenas o conteúdo válido seja adicionado ao cache e a integridade do cache local seja protegida.

## <a name="BKMK_10"></a>Processos do BranchCache: recuperar conteúdo

Depois que um computador cliente localiza o conteúdo desejado no host de conteúdo (que é um servidor de cache hospedado ou um computador cliente no modo de cache distribuído), o computador cliente começa o processo de recuperar o conteúdo.

Primeiro, o computador cliente envia uma solicitação ao host de conteúdo para o primeiro bloco necessário. A solicitação contém o ID do segmento e o intervalo de blocos que identificam o conteúdo desejado. Como apenas um bloco é enviado, o intervalo contém somente um bloco (Atualmente, não há suporte para solicitações para vários blocos.) O cliente também armazena a solicitação em sua lista de solicitações pendentes local.  

Após receber uma mensagem de solicitação válida de um cliente, o host de conteúdo verifica se o bloco especificado na solicitação existe no cache de conteúdo do host de conteúdo.

Se o host de conteúdo possuir o bloco de conteúdo, ele enviará uma resposta que contém o ID do segmento, ID do bloco, bloco de dados criptografados e vetor de inicialização usado para criptografar o bloco.

Se o host de conteúdo não possuir o bloco de conteúdo, ele enviará uma mensagem de resposta vazia. Isso informará ao computador cliente que o host de conteúdo não tem o bloco solicitado. Uma mensagem de resposta vazia contém o ID do segmento e ID do bloco solicitado, juntamente com um bloco de dados com tamanho zero.

Quando o computador cliente recebe a resposta do host de conteúdo, ele verifica se a mensagem corresponde à mensagem solicitada na lista de solicitações pendentes (o ID do segmento e o índice do bloco devem corresponder à solicitação pendente).

Se esse processo de verificação não for bem-sucedido e o computador cliente não tiver uma mensagem de solicitação correspondente na lista de solicitações pendentes, o computador cliente descartará a mensagem.

Se esse processo de verificação for bem-sucedido e o computador cliente tiver uma mensagem de solicitação correspondente na lista de solicitações pendentes, o computador cliente descriptografará o bloco. Em seguida, o cliente validará o bloco descriptografado em relação ao hash de bloco adequado das informações de conteúdo que o cliente obteve inicialmente do servidor de conteúdo original.

Se a validação do bloco for bem-sucedida, o bloco descriptografado será armazenado em cache.

Esse processo é repetido até o cliente ter todos os blocos necessários.

> [!NOTE]
> Se os segmentos de conteúdo completos não existirem em um computador, o protocolo de recuperação recuperará e constituirá o conteúdo a partir de uma combinação de origens: um conjunto de computadores clientes no modo de cache distribuído, um servidor de cache hospedado e (se os caches da filial não contiverem o conteúdo completo) o servidor de conteúdo original da matriz.

Antes de o BranchCache enviar conteúdo ou informações de conteúdo, os dados são criptografados. O BranchCache criptografa o bloco na mensagem de resposta. No Windows 7, o algoritmo de criptografia padrão usado pelo BranchCache é o AES-128, a chave de criptografia é ke e o tamanho da chave é de 128 bits, conforme determinado pelo algoritmo de criptografia. 

O BranchCache gera um vetor de inicialização adequado ao algoritmo de criptografia e usa a chave de criptografia para criptografar o bloco. Em seguida, o BranchCache registra o algoritmo de criptografia e o vetor de inicialização na mensagem. 

Os servidores e clientes nunca trocam, compartilham ou enviam a chave de criptografia uns para os outros. O cliente recebe a chave de criptografia do servidor de conteúdo e hospeda o conteúdo de origem. Em seguida, usando o algoritmo de criptografia e o vetor de inicialização recebido do servidor, ele descriptografa o bloco. Não há outra autorização ou autenticação explícita incorporada ao protocolo de download.

### <a name="security-threats"></a>Ameaças de segurança

As principais ameaças à segurança nesta camada incluem:

- Adulteração de dados:

  *Um cliente fornecendo dados para um solicitante adultera os dados*. O modelo de segurança do BranchCache usa hashes para confirmar que o cliente e o servidor não alteraram os dados.  

- Divulgação de informações:  

    *O BranchCache envia conteúdo criptografado para um cliente que especifica o ID de Segmento adequado*. Os IDs de segmento são públicos. Por isso, qualquer cliente pode receber conteúdo criptografado. Porém, se um usuário mal-intencionado obtiver conteúdo criptografado, ele devera saber a chave de criptografia para descriptografar o conteúdo. O protocolo da camada superior realiza a autenticação e fornece as informações de conteúdo ao cliente autenticado e autorizado. A segurança das informações de conteúdo é equivalente à segurança fornecida ao próprio conteúdo, e o BranchCache nunca expõe as informações de conteúdo.  

    *Um invasor detecta a transmissão para obter o conteúdo*.  O BranchCache criptografa todas as transferências entre clientes usando AES128, onde a chave secreta é Ke, evitando que os dados sejam detectados na transmissão.  As informações de conteúdo baixadas do servidor de conteúdo são protegidas exatamente do mesmo modo que os próprios dados. Por isso, a proteção delas contra divulgação é igual a que seria se o BranchCache não fosse usado.

-   Negação de serviço:

    *Um cliente é sobrecarregado com solicitações de dados*. Os protocolos do BranchCache incorporam contadores e temporizadores de gerenciamento de fila para evitar a sobrecarga dos clientes.

## <a name="BKMK_11"></a>Processos do BranchCache: conteúdo do cache

Em computadores clientes no modo de cache distribuído e servidores de cache hospedado localizados em filiais, os caches de conteúdo são criados ao longo do tempo, à medida que o conteúdo é recuperado por links WAN.

Quando os computadores clientes são configurados com o modo de cache hospedado, eles adicionam conteúdo a seu próprio cache local e também oferecem dados ao servidor de cache hospedado. O Protocolo de Cache Hospedado fornece um mecanismo para que os clientes informem o servidor de cache hospedado sobre disponibilidade de conteúdo e segmentos.

Para carregar conteúdo no servidor de cache hospedado, o cliente informa ao servidor que tem um segmento disponível. Em seguida, o servidor de cache hospedado recupera todas as informações de conteúdo associadas ao segmento oferecido e baixa os blocos no segmento de que realmente necessita. Esse processo é repetido até o cliente não ter mais segmentos a oferecer ao servidor de cache hospedado.

Para atualizar o servidor de cache hospedado usando o Protocolo de Cache Hospedado, é necessário atender ao seguintes requisitos:

- O computador cliente deve ter um conjunto de blocos em um segmento que possa oferecer ao servidor de cache hospedado. O cliente deve fornecer informações de conteúdo para o segmento oferecido; elas são compostas do ID do segmento o hash de dados do segmento, o segredo de segmento e uma lista de todos os hashes de bloco contidos no segmento.

- Para servidores de cache hospedados que executam o Windows Server 2008 R2, é necessário um certificado de servidor de cache hospedado e uma chave privada associada, e a autoridade de certificação (CA) que emitiu o certificado deve ser confiável por computadores cliente na filial. Isso permite que o cliente e o servidor participem com êxito na autenticação do servidor HTTPS.

    > [!IMPORTANT]
    > Os servidores de cache hospedados que executam o Windows Server 2016, o Windows Server 2012 R2 ou o Windows Server 2012 não exigem um certificado de servidor de cache hospedado e uma chave privada associada.  

- O computador cliente está configurado com o nome de computador do servidor de cache hospedado e o número da porta TCP por meio da qual esse servidor está escutando o tráfego do BranchCache. O certificado do servidor de cache hospedado está associado a esta porta. O nome de computador do servidor de cache hospedado poderá ser um FQDN (nome de domínio totalmente qualificado) se o servidor for um computador membro do domínio, ou um nome NetBIOS do computador se o servidor não for membro do domínio.

- O computador cliente escuta ativamente para detectar solicitações de blocos recebidas. A porta que ele usa para isso é passada como parte das mensagens de oferta do cliente para o servidor de cache hospedado. Isso permite que o servidor de cache hospedado use os protocolos do BranchCache para conectar-se ao computador cliente e recuperar blocos de dados no segmento.

- O servidor de cache hospedado começa a escutar para detectar solicitações HTTP recebidas quando é iniciado.

- Se o servidor de cache hospedado estiver configurado para exigir autenticação do computador cliente, tanto o cliente quanto o servidor deverão dar suporte à autenticação HTTPS.

### <a name="hosted-cache-mode-cache-population"></a>População de cache do modo de cache hospedado

O processo de adicionar conteúdo ao cache do servidor de cache hospedado em uma filial começa quando o cliente envia um INITIAL_OFFER_MESSAGE, que inclui a ID do segmento. A ID de segmento na solicitação de INITIAL_OFFER_MESSAGE é usada para recuperar o hash de segmento correspondente de dados, a lista de hashes de bloco e o segredo de segmento do cache de blocos do servidor de cache hospedado. Se o servidor de cache hospedado já tiver todas as informações de conteúdo de um segmento específico, a resposta a INITIAL_OFFER_MESSAGE será OK, e nenhuma solicitação de download de blocos ocorrerá.

Se o servidor de cache hospedado não tiver todos os blocos de dados oferecidos associados aos hashes de bloco no segmento, a resposta a INITIAL_OFFER_MESSAGE será INTERESTED. Em seguida, o cliente enviará SEGMENT_INFO_MESSAGE, que descreve o segmento único oferecido. O servidor de cache hospedado responde com uma mensagem de OK e inicia o download dos blocos ausentes do computador cliente que realizou a oferta.

O hash de dados do segmento, a lista de hashes de bloco e o segredo de segmento são usados para garantir que o conteúdo baixado não tenha sido violado nem alterado. Em seguida, os blocos baixados são adicionados ao cache de bloco do servidor de cache hospedado.

## <a name="bkmk_cache"></a>Segurança de cache  
Esta seção fornece informações sobre como o BranchCache protege dados em cache em computadores clientes e servidores de cache hospedado.

### <a name="client-computer-cache-security"></a>Segurança do cache do computador cliente
A maior ameaça aos dados armazenados no BranchCache é a violação. Se um invasor puder violar o conteúdo e as informações de conteúdo armazenadas em cache, ele poderá usá-los para tentar iniciar um ataque contra os computadores que usam o BranchCache. Os invasores podem iniciar um ataque inserindo softwares mal-intencionados no lugar de outros dados. O BranchCache elimina essa ameaça por meio da validação de todo o conteúdo usando os hashes de bloco encontrados nas informações de conteúdo. Se um invasor tentar realizar uma violação com esses dados, eles serão descartados e substituídos por dados válidos da origem original.

Uma ameaça secundária aos dados armazenados no BranchCache é a divulgação de informações. No modo de cache distribuído, o cliente armazena em cache somente o conteúdo que solicitou por conta própria. Porém, esses dados são armazenados como texto não criptografado e podem correr riscos. Para restringir o acesso ao cache somente ao serviço BranchCache, o cache local é protegido pelas permissões do sistema de arquivos especificadas em uma ACL. 

Embora a ACL seja eficiente para evitar que usuários não autorizados acessem o cache, é possível que um usuário com privilégios administrativos obtenha acesso ao cache por meio da alteração manual das permissões especificadas na ACL. O BranchCache não oferece proteção contra o uso mal-intencionado de uma conta administrativa.

Os dados armazenados no cache de conteúdo não são criptografados. Por isso, se o vazamento de dados for um preocupação, use tecnologias como BitLocker ou EFS (Encrypting File System). O cache local usado pelo BranchCache não aumenta o risco de divulgação de informações trazido por um computador na filial. O cache contém apenas cópias dos arquivos que permanecem descriptografados em alguma parte do disco. 

Criptografar todo o disco é particularmente importante em ambientes em que seja difícil garantir a segurança física dos clientes. Por exemplo, a criptografia de todo o disco ajuda a proteger dados confidenciais em computadores móveis que possam ser removidos do ambiente da filial.

### <a name="hosted-cache-server-cache-security"></a>Segurança do cache do servidor de cache hospedado

No modo de cache hospedado, a maior ameaça à segurança do servidor de cache hospedado é a divulgação de informações. Em um ambiente de cache hospedado, o BranchCache comporta-se de modo semelhante ao modo de cache distribuído, com a permissão do sistema de arquivos protegendo os dados em cache. A diferença é que o servidor de cache hospedado armazena todo o conteúdo solicitado por qualquer computador habilitado por BranchCache na filial, em vez de fazer isso apenas com os dados solicitados por um único cliente. As consequências do acesso não autorizado a esse cache podem ser muito mais sérias, pois há muito mais dados em risco.  
  
Em um ambiente de cache hospedado em que o servidor de cache hospedado está executando o Windows Server 2008 R2, o uso de tecnologias de criptografia, como BitLocker ou EFS, é aconselhável se qualquer um dos clientes na filial puder acessar dados confidenciais pelo link da WAN. Também é necessário evitar o acesso físico ao cache hospedado, pois a criptografia de disco funciona somente quando o computador está desligado no momento em que o invasor obtém acesso físico.  Se o computador estiver ligado ou no modo de suspensão, a criptografia de disco oferecerá pouca proteção.

> [!NOTE]
> Os servidores de cache hospedados que executam o Windows Server 2016, o Windows Server 2012 R2 ou o Windows Server 2012 criptografam todos os dados no cache por padrão, portanto, o uso de tecnologias de criptografia adicionais não é necessário.

Mesmo que um cliente esteja configurado no modo de cache hospedado, ele ainda armazenará dados em cache localmente. Por isso, convém tomar medidas para proteger o cache local (além do cache no servidor de cache hospedado).
