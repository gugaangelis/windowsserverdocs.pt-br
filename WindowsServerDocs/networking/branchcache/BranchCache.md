---
title: BranchCache
description: Este tópico fornece uma visão geral do BranchCache no Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-bc
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a4587cff-c086-49f1-a0bf-cd74b8a44440
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5540d89827b80a21bf23f6a2aa8f54f09dfde67a
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="branchcache"></a>BranchCache

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Neste tópico, que se destina a profissionais de tecnologia da informação (TI), fornece informações gerais sobre BranchCache, incluindo modos BranchCache, recursos, recursos e a funcionalidade de BranchCache que está disponível em sistemas operacionais diferentes.

> [!NOTE]
> Além de neste tópico, a seguinte documentação BranchCache está disponível.
> 
> - [Shell de rede BranchCache e comandos do Windows PowerShell](../branchcache/BranchCache-Network-Shell-and-Windows-PowerShell-Commands.md)
> -   [Guia de implantação do BranchCache](../branchcache/deploy/BranchCache-Deployment-Guide.md)

**Quem será interessado BranchCache?**

Se você for um administrador do sistema, rede ou arquiteto de solução de armazenamento ou outro profissional de TI, BranchCache possam lhe interessar nas seguintes circunstâncias:

- Crie ou dar suporte a infraestrutura de TI para uma organização que tem dois ou mais locais físicos e uma conexão de rede (WAN) de longa distância de filiais ao escritório.

- Você projeta ou suporte à infraestrutura de TI para uma organização que implantou tecnologias de nuvem e uma conexão WAN é usado pelo trabalhos para acessar os dados e aplicativos em locais remotos.

- Você deseja otimizar o uso de largura de banda WAN, reduzindo a quantidade de tráfego de rede entre filiais e a matriz.

- Você implantou ou planeja implantar servidores de conteúdo no seu escritório principal que combinam com as configurações que são descritas neste tópico.

- Os computadores cliente em seu filiais estiverem executando Windows 10, Windows 8.1, Windows 8 ou Windows 7.

Este tópico inclui as seções a seguir:

-   [O que é BranchCache?](#bkmk_what)

-   [BranchCache modos](#BKMK_2)
  
-   [Servidores de conteúdo BranchCache habilitado](#BKMK_3)
  
-   [BranchCache e a nuvem](#BKMK_3a)
  
-   [Versões de informações de conteúdo](#bkmk_version)  
  
-   [Como BranchCache lida com atualizações de conteúdo em arquivos](#bkmk_handles)  
  
-   [Guia de instalação do BranchCache](#BKMK_4)  
  
-   [Versões de sistema operacional para BranchCache](#bkmk_os)  
  
-   [BranchCache segurança](#bkmk_security)  
  
-   [Processos e fluxo de conteúdo](#bkmk_flow)  
  
-   [Segurança de cache](#bkmk_cache)  
  
## <a name="bkmk_what"></a>O que é BranchCache?

BranchCache é uma tecnologia de otimização de largura de banda (WAN) de rede de longa distância que está incluída em algumas edições dos sistemas operacionais Windows Server 2016 e o Windows 10, bem como em algumas edições do Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2 e Windows 7. Para otimizar a largura de banda WAN quando os usuários acessam conteúdo em servidores remotos, BranchCache busca o conteúdo do seu escritório principal ou hospedado servidores de conteúdo na nuvem e armazena em cache o conteúdo em filiais, permitindo que o cliente de computadores em filiais para acessar o conteúdo localmente, em vez de na WAN.
  
Em filiais, o conteúdo é armazenado em servidores que estão configurados para o cache de host ou, quando não há servidor está disponível na filial, em computadores cliente que executam o Windows 10, Windows 8.1, Windows 8 ou Windows 7. Depois que um computador cliente solicitações e recebe o conteúdo da matriz e o conteúdo é armazenado em cache na filial, outros computadores na mesma filial podem obter o conteúdo localmente em vez de baixar o conteúdo do servidor de conteúdo através da conexão WAN.

Quando o mesmo conteúdo solicitações subsequentes são feitas por computadores cliente, os clientes baixar *informações de conteúdo* do servidor em vez do conteúdo em si. Informações de conteúdo consistem de hashes que são calculados utilizando blocos do conteúdo original e são extremamente pequeno em comparação com o conteúdo nos dados originais. Computadores cliente, em seguida, usam as informações de conteúdo para localizar o conteúdo de um cache na filial, se o cache está localizado em um computador cliente ou em um servidor. Servidores e computadores cliente também usam informações de conteúdo para proteger o conteúdo em cache para que ele não pode ser acessado por usuários não autorizados.

BranchCache aumenta a produtividade do usuário final, melhorando os tempos de resposta de consulta de conteúdo para clientes e servidores em filiais e também pode ajudar a melhorar o desempenho de rede, reduzindo o tráfego por links WAN.

## <a name="BKMK_2"></a>BranchCache modos
BranchCache tem dois modos de operação: distribuído modo de cache e o modo de cache hospedado.

Quando você implanta BranchCache no modo de cache distribuído, o cache de conteúdo em uma filial é distribuído entre computadores cliente.

Quando você implanta BranchCache no modo de cache hospedado, o cache de conteúdo em uma filial é hospedado em um ou mais computadores de servidor, que são chamados de servidores de cache.

> [!NOTE]
> Você pode implantar BranchCache usando os dois modos, mas apenas um modo pode ser usado por filial. Por exemplo, se você tiver dois filiais, uma que tem um servidor e outra que não acontecer, você pode implantar BranchCache no modo de cache hospedado no escritório que contém um servidor, durante a implantação de BranchCache no modo de cache distribuído no escritório que contém apenas os computadores cliente.

A ilustração a seguir, BranchCache é implantado em ambos os modos.  

![BranchCache modos](../media/BranchCache/bc_modes.jpg)

Modo de cache distribuído é mais adequado para pequenas filiais que não contêm um servidor local para uso como um servidor de cache hospedado. Modo de cache distribuído permite que você implante BranchCache sem hardware adicional em filiais.

Se filial onde você deseja implantar BranchCache contém infraestrutura adicional, como um ou mais servidores que executam outras cargas de trabalho, implantar BranchCache no modo de cache hospedado é vantajoso pelos seguintes motivos:

### <a name="increased-cache-availability"></a>Disponibilidade de cache maior

Modo de cache hospedado aumenta a eficiência do cache porque o conteúdo está disponível, mesmo se o cliente que originalmente solicitado e os dados armazenados em cache estiver offline. Como o servidor de cache hospedado sempre está disponível, mais conteúdo é armazenado em cache, fornecendo maior economia de largura de banda WAN, e a eficiência BranchCache é aprimorada.

### <a name="centralized-caching-for-multiple-subnet-branch-offices"></a>Centralizado em cache para filiais sub-rede múltiplo


O modo de cache distribuído opera em uma única sub-rede. Em uma sub-rede múltiplo filial que está configurada para o modo de cache distribuído, um arquivo baixado para uma sub-rede não pode ser compartilhado com computadores cliente em outras sub-redes. 

Por isso, clientes em outras sub-redes, não é possível descobrir que o arquivo já foi baixado, obtém o arquivo do servidor de conteúdo principal do office, usando a largura de banda WAN no processo.

Quando você implanta o modo de cache hospedado, no entanto, isso não for o caso - todos os clientes em uma filial múltiplo sub-rede podem acessar um cache único, que é armazenado no servidor de cache hospedado, mesmo que os clientes estão em sub-redes diferentes. Além disso, BranchCache no Windows Server 2012, Windows Server 2012 R2 e Windows Server 2016 fornece a capacidade de implantar mais de um servidor de cache hospedado por filial.

> [!CAUTION]
> Se você usar BranchCache para SMB armazenamento em cache de arquivos e pastas, não desative Arquivos Offline. Se você desabilitar arquivos Offline, BranchCache SMB armazenamento em cache não funcionar corretamente.

## <a name="BKMK_3"></a>Servidores de conteúdo BranchCache habilitado

Quando você implanta BranchCache, o conteúdo de origem é armazenado nos BranchCache servidores habilitados com conteúdo em seu escritório principal ou em um centro de dados na nuvem. Os seguintes tipos de servidores de conteúdo são suportados pelo BranchCache:

> [!NOTE]
> Somente conteúdo de origem – ou seja, conteúdo que os computadores cliente inicialmente obtenham de um servidor de conteúdo habilitado BranchCache - é acelerado por BranchCache. Conteúdo que os computadores cliente obtêm diretamente de outras fontes, como servidores Web na Internet ou atualização do Windows, não é armazenado em cache por computadores cliente ou hospedado servidores de cache e, em seguida, compartilhada com outros computadores na filial. Se você quiser acelerar o conteúdo do Windows Update, no entanto, você pode instalar um servidor de aplicativo do Windows Server Update Services (WSUS) em seu escritório principal ou na nuvem data center e configurá-lo como um servidor de conteúdo BranchCache.

### <a name="web-servers"></a>Servidores Web

Servidores Web compatíveis incluem computadores que executam o Windows Server 2008 R2, Windows Server 2012 R2, Windows Server 2012 ou Windows Server 2016 que tem a função de servidor de Web Server (IIS) instalada e que usam Hypertext Transfer Protocol (HTTP) ou HTTP HTTPS (Secure).

Além disso, o servidor Web deve ter o recurso BranchCache instalado.

### <a name="file-servers"></a>Servidores de arquivos

Servidores de arquivos com suporte incluem computadores que executam o Windows Server 2008 R2, Windows Server 2012 R2, Windows Server 2012 ou Windows Server 2016 que têm a função de servidor Serviços de arquivo e o BranchCache para arquivos de rede serviço de função instalado. 

Esses servidores de arquivos usam o bloco de mensagem de servidor (SMB) para trocar informações entre computadores. Depois de concluir a instalação do seu servidor de arquivos, você também deve compartilhar pastas e ativar a geração de hash para pastas compartilhadas usando a política de computador Local ou de política de grupo para habilitar o BranchCache.

### <a name="application-servers"></a>Servidores de aplicativos

Servidores de aplicativos com suporte incluem computadores que executam o Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 ou Windows Server 2008 R2 com BITS Background Intelligent Transfer Service () instalado e habilitado. 

Além disso, o servidor de aplicativo deve ter o recurso BranchCache instalado. Como exemplos de servidores de aplicativos, você pode implantar servidores Microsoft Windows Server Update Services (WSUS) e Microsoft System Center Configuration Manager Branch Distribution Point como servidores de conteúdo BranchCache.

## <a name="BKMK_3a"></a>BranchCache e a nuvem

A nuvem tem o enorme potencial para reduzir as despesas operacionais e alcançar novos níveis de escala, mas movendo cargas de trabalho longe as pessoas que dependem delas pode aumentar os custos de redes e prejudicar a produtividade. Os usuários esperam alto desempenho e não importa onde seus aplicativos e dados são hospedados. 

BranchCache pode melhorar o desempenho de aplicativos em rede e reduzir o consumo de largura de banda com um cache de dados compartilhado.  Ele melhora a produtividade em filiais e em sede em trabalhos estiver usando servidores que são implantados na nuvem.

Como BranchCache não exige hardware novo ou alterações de topologia de rede, é uma excelente solução para melhorar a comunicação entre escritórios e nuvens públicas e privadas.

> [!NOTE]
> Como alguns proxies da Web não podem processar cabeçalhos de codificação de conteúdo não padrão, é recomendável que você use BranchCache com Hyper texto segura HTTPS (Transfer Protocol) e não em HTTP.
  
=== Para obter mais informações sobre as tecnologias de nuvem no Windows Server 2016, consulte [Software de rede definidos & #40; SDN & #41; ](../sdn/Software-Defined-Networking--SDN-.md).
  
## <a name="bkmk_version"></a>Versões de informações de conteúdo

Há duas versões de informações de conteúdo:

- Informações de conteúdo que seja compatíveis com computadores que executam o Windows Server 2008 R2 e Windows 7 são chamadas versão 1 ou V1. Com V1 BranchCache segmentação de arquivo, os segmentos de arquivo são maiores do que em V2 e são de tamanho fixo. Por causa de tamanhos de segmento fixo grandes, quando um usuário faz uma alteração que modifica o tamanho do arquivo, não só é invalidado o segmento com a mudança, mas todos os segmentos até o final do arquivo são invalidados. A próxima chamada para o arquivo alterado por outro usuário na filial, portanto, resulta em economias de largura de banda WAN reduzida porque o conteúdo alterado e todo o conteúdo após a alteração são enviados através da conexão WAN.

- Informações de conteúdo que seja compatíveis com computadores que executam o Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012 e Windows 8 são chamadas versão 2 ou V2. Informações de conteúdo v2 usam segmentos menores tamanho variável, que são mais tolerantes a alterações dentro de um arquivo. Isso aumenta a probabilidade de segmentos de uma versão mais antiga do arquivo pode ser reutilizada quando os usuários acessam uma versão atualizada, fazendo com que eles recuperar somente a parte alterada do arquivo do servidor de conteúdo e usando menos largura de banda de rede de longa distância.

A tabela a seguir fornece informações sobre a versão de informações de conteúdo que é usada dependendo de qual cliente, o servidor de conteúdo, e hospedados cache sistemas operacionais que você estiver usando em sua implantação BranchCache.

> [!NOTE]
> Na tabela a seguir, acrônimo "SO" significa o sistema operacional.

|Sistema operacional do cliente|Sistema operacional de servidor de conteúdo|Sistema operacional de servidor de Cache hospedado|Versão de informações de conteúdo|
|-------------|---------------------|--------------------------|-------------------------------|
|Windows Server 2008 R2 e Windows 7 |Windows Server 2012 ou posterior|Windows Server 2012 ou posterior; None para o modo de cache distribuído|V1|
|Windows Server 2012 ou posterior; Windows 8 ou posterior|Windows Server 2008 R2 |Windows Server 2012 ou posterior; None para o modo de cache distribuído|V1|
|Windows Server 2012 ou posterior; Windows 8 ou posterior| Windows Server 2012 ou posterior| Windows Server 2008 R2 |V1|
|Windows Server 2012 ou posterior; Windows 8 ou posterior|Windows Server 2012 ou posterior|Windows Server 2012 ou posterior; None para o modo de cache distribuído|V2|

Quando você tiver conteúdos servidores e hospedados cache servidores que executam o Windows Server 2012, Windows Server 2012 R2 e Windows Server 2016, eles usam a versão de informações de conteúdo que é apropriada com base no sistema operacional do cliente BranchCache que solicita informações. 

Quando os computadores que executam o Windows Server 2012 e Windows 8 ou sistemas operacionais posteriores solicitam conteúdo, os servidores de cache de conteúdo e hospedados usam informações de conteúdo V2; Quando os computadores que executam o Windows Server 2008 R2 e Windows 7 solicitam conteúdo, os servidores de cache de conteúdo e hospedados usam informações de conteúdo de V1.

>[!IMPORTANT]
>Quando você implanta BranchCache no modo de cache distribuído, os clientes que usam versões diferentes de informações de conteúdo não compartilhar conteúdo uns com os outros. Por exemplo, um computador cliente com Windows 7 e um computador cliente que executam o Windows 10 que estão instalados na mesma filial não compartilhar conteúdo uns com os outros.
  
## <a name="bkmk_handles"></a>Como BranchCache lida com atualizações de conteúdo em arquivos

Quando os usuários das filiais modificam ou atualizar o conteúdo de documentos, suas alterações são escritas diretamente ao servidor de conteúdo na matriz sem a participação do BranchCache. Isso é verdadeiro se o usuário baixou o documento do servidor de conteúdo ou obtido de um um cache hospedado ou distribuído na filial.

Quando o arquivo modificado é solicitado por um cliente diferente em uma filial, os segmentos novos do arquivo são baixados do servidor de matriz e adicionados para o cache hospedado ou distribuído na ramificação. Por isso, os usuários das filiais sempre recebem as versões mais recentes do conteúdo armazenado em cache.

## <a name="BKMK_4"></a>Guia de instalação do BranchCache

Você pode usar o Gerenciador do servidor no Windows Server 2016 para instalar o recurso BranchCache ou BranchCache para o serviço de função de arquivos de rede da função de servidor Serviços de arquivo. Você pode usar a tabela a seguir para determinar se deve instalar o serviço de função ou recurso.

|Funcionalidade|Local do computador|Instalar esse elemento BranchCache|
|-----------------|---------------------|------------------------------------|
|Servidor de conteúdo \ (servidor de aplicativo com base em BITS \)|Centro de dados principal escritório ou na nuvem|Recurso BranchCache|
|Servidor de conteúdo \(Web server\)|Centro de dados principal escritório ou na nuvem|Recurso BranchCache|
|Servidor de conteúdo \ (servidor de arquivos usando o protocol\ SMB)|Centro de dados principal escritório ou na nuvem|BranchCache para o serviço de função de arquivos de rede da função de servidor Serviços de arquivo|
|Servidor de cache hospedado|Filial|BranchCache recurso com o modo de servidor de cache hospedado habilitado|
|Computador cliente BranchCache habilitado|Filial|Nenhuma instalação necessária; Basta habilitar BranchCache e um modo BranchCache \(distributed or hosted\) no cliente|

Para instalar o serviço de função ou o recurso, abra o Gerenciador do servidor e selecione os computadores em que você deseja habilitar a funcionalidade de BranchCache. No Gerenciador do servidor, clique em **gerenciar**e clique em **adicionar funções e recursos**. O **adicionar funções e recursos** wizard é aberto. Como executar o assistente, faça as seleções a seguir:

- Na página do assistente **selecione tipo de instalação**, selecione **instalação baseada em função ou recurso com base em**.

- Na página do assistente **selecionar funções de servidor**, se você estiver instalando um servidor de arquivos BranchCache habilitado, expanda **File and Storage Services** e **serviços de arquivo e iSCSI**e, em seguida, selecione **BranchCache para arquivos de rede**.  Para economizar espaço em disco, você também pode selecionar o **duplicação de dados** função serviço e prossiga com o Assistente para instalação e a conclusão. Se você não quiser instalar um servidor de arquivos BranchCache habilitado, não instale a função Serviços de arquivo e armazenamento com o BranchCache para o serviço de função de arquivos de rede.

- Na página do assistente **Selecione recursos**, se você estiver instalando um servidor de conteúdo que não é um servidor de arquivos ou se você estiver instalando um servidor de cache hospedado, selecione **BranchCache**e continue o Assistente para instalação e a conclusão. Se você não quiser instalar um servidor de conteúdo diferente de um servidor de arquivos ou um servidor de cache hospedado, não instale o recurso BranchCache.
  
## <a name="bkmk_os"></a>Versões de sistema operacional para BranchCache

Esta é uma lista dos sistemas operacionais que dão suporte a diferentes tipos de funcionalidade BranchCache.

### <a name="operating-systems-for-branchcache-client-computer-functionality"></a>Sistemas operacionais para a funcionalidade de computador cliente BranchCache

Os seguintes sistemas operacionais fornecem BranchCache com suporte para o serviço de transferência inteligente em segundo plano (BITS), Hyper texto HTTP (Transfer Protocol) e bloco de mensagem de servidor (SMB).

- Windows 10 Enterprise

- Windows 10 Education

- Windows 8.1 Enterprise

- Windows 8 Enterprise

- Windows 7 Enterprise

- Windows 7 Ultimate

Os seguintes sistemas operacionais, BranchCache não oferece suporte a funcionalidade HTTP e SMB, mas oferece suporte a funcionalidade de BITS BranchCache.

-   Windows 10 Pro, BITS suportam somente

-   Windows 8.1 Pro, BITS suportam somente

-   Windows 8 Pro, BITS suportam somente

-   Windows 7 Pro, BITS suportam somente

> [!NOTE]
> BranchCache não está disponível por padrão nos sistemas operacionais Windows Server 2008 ou Windows Vista. Nesses sistemas operacionais, no entanto, se você baixa e instalar a atualização do Windows Management Framework, BranchCache funcionalidade está disponível para o protocolo de serviço de transferência inteligente em segundo plano (BITS) apenas. Para obter mais informações e para baixar o Windows Management Framework, consulte [Windows Management Framework (Windows PowerShell 2.0, WinRM 2.0 e BITS 4.0)](https://go.microsoft.com/fwlink/?LinkId=188677) em https://go.microsoft.com/fwlink/?LinkId=188677.
  
### <a name="operating-systems-for-branchcache-content-server-functionality"></a>Sistemas operacionais para BranchCache funcionalidade do servidor de conteúdo

Você pode usar os Windows Server 2012, Windows Server 2012 R2 e Windows Server 2016 famílias de sistemas operacionais como servidores de conteúdo BranchCache.

Além disso, a família Windows Server 2008 R2 dos sistemas operacionais pode ser usada como servidores de conteúdo BranchCache, com as seguintes exceções:

- BranchCache não tem suporte em instalações Server Core do Windows Server 2008 R2 Enterprise com o Hyper-V.

- BranchCache não tem suporte em instalações Server Core do Windows Server 2008 R2 Datacenter com o Hyper-V.

### <a name="operating-systems-for-branchcache-hosted-cache-server-functionality"></a>Sistemas operacionais para BranchCache hospedado funcionalidade do servidor de cache

Você pode usar os Windows Server 2012, Windows Server 2012 R2 e Windows Server 2016 famílias de sistemas operacionais como BranchCache hospedado servidores de cache.

Além disso, os seguintes sistemas operacionais Windows Server 2008 R2 pode ser usados como BranchCache hospedado servidores de cache:

- Windows Server 2008 R2 Enterprise

- Windows Server 2008 R2 Enterprise com Hyper-V

- Instalação do Windows Server 2008 R2 Enterprise Server Core

- Windows Server 2008 R2 Enterprise Server Core instalação com o Hyper-V

- Windows Server 2008 R2 para sistemas baseados em Itanium

- O Windows Server 2008 R2 Datacenter

- Windows Server 2008 R2 Datacenter com Hyper-V

- Windows Server 2008 R2 Datacenter Server Core instalação com o Hyper-V

## <a name="bkmk_security"></a>BranchCache segurança

BranchCache implementa uma abordagem segura por design que funciona em perfeita integração junto com as arquiteturas de segurança de rede existentes, sem a necessidade de equipamentos adicionais ou configuração de segurança adicional complexos.
  
BranchCache é não invasivo e não altera os processos de autenticação ou de autorização do Windows. Depois de implantar BranchCache, autenticação ainda é realizada usando as credenciais de domínio e a maneira na qual autorização com listas de controle de acesso (ACLs) funções é alterada. Além disso, outras configurações continuam a funcionar exatamente como antes da implantação BranchCache.

O modelo de segurança BranchCache baseia-se na criação de metadados, que assumem a forma de uma série de hashes. Esses hashes também são chamados de informações de conteúdo.

Depois de informações de conteúdo são criadas, ele é usado em trocas de mensagens BranchCache em vez dos dados reais, e ele é trocado usando os protocolos com suporte (HTTP, HTTPS e SMB).

Dados em cache são mantidos criptografados e não podem ser acessados por clientes que não têm permissão para acessar o conteúdo da fonte original.  Os clientes devem ser autenticados e autorizados por origem do conteúdo original antes que eles podem recuperar metadados de conteúdo e devem ter metadados de conteúdo para acessar o cache no escritório local.

### <a name="how-branchcache-generates-content-information"></a>Como BranchCache gera informações de conteúdo

Como informações de conteúdo são criadas a partir de vários elementos, o valor das informações de conteúdo sempre é exclusivo. Esses elementos são:

- O conteúdo real (como as páginas da Web ou arquivos compartilhados) da qual os hashes são derivados.  

- Parâmetros de configuração, como o tamanho de bloco e de algoritmo de hash. Para gerar informações de conteúdo, o servidor de conteúdo divide o conteúdo em segmentos e, em seguida, subdivide esses segmentos em blocos. BranchCache usa seguros hashes criptográficos para identificar e verifique se cada bloco e o segmento, dando suporte o algoritmo de hash SHA256.

- Um segredo do servidor. Todos os servidores de conteúdo devem ser configurados com um segredo do servidor, que é um valor binário de tamanho arbitrário.

> [!NOTE]
> O uso de um segredo de servidor garante que os computadores cliente não sejam capazes de gerar as informações de conteúdo propriamente ditos. Isso impede que usuários mal-intencionados usando ataques de força bruta com computadores cliente BranchCache habilitado para adivinhar mudanças secundárias no conteúdo entre as versões em situações em que o cliente acessaram para uma versão anterior, mas não tem acesso para a versão atual.

### <a name="content-information-details"></a>Detalhes de informações de conteúdo

BranchCache usa o segredo do servidor como uma chave para derivar um hash de conteúdo específicos que é enviado aos clientes autorizados. Aplicar um algoritmo de hash para o segredo do servidor combinados e o Hash de dados gera esse hash.

Esse hash é chamado o segredo do segmento. BranchCache usa segredos de segmento para proteger a comunicação. Além disso, BranchCache cria uma lista de Hash de bloco, que é a lista de blocos de dados hash, e o Hash de dados, que é gerado pelo hash a lista de Hash do bloco.

As informações de conteúdo incluem o seguinte:

- A lista de Hash do bloco:

    `BlockHashi = Hash(dataBlocki)   1<=i<=n`

- O Hash de dados (método):

    `HoD = Hash(BlockHashList)`

- Segredo do segmento (Kp):

    `Kp = HMAC(Ks, HoD)`

BranchCache usa o protocolo de armazenamento em cache de conteúdo do par e o protocolo de estrutura de recuperação para implementar os processos que são necessários para garantir o armazenamento em cache e recuperação de dados entre os caches de conteúdo seguro.

Além disso, os identificadores de BranchCache conteúdo informações com o mesmo nível de segurança que ele usa ao tratamento e transmitir o conteúdo em si próprio.

## <a name="bkmk_flow"></a>Processos e fluxo de conteúdo

O fluxo de conteúdo informações e conteúdo em si é dividido em quatro fases:

1.  [BranchCache processos: solicitar conteúdo](#BKMK_8)

2.  [BranchCache processos: localizar o conteúdo](#BKMK_9)

3.  [BranchCache processos: recuperar conteúdo](#BKMK_10)

4.  [BranchCache processos: armazenar em Cache conteúdo](#BKMK_11)

As seções a seguir descrevem essas fases.

## <a name="BKMK_8"></a>BranchCache processos: solicitar conteúdo

Na primeira fase, o computador cliente na filial solicita conteúdo, como um arquivo ou uma página da Web, de um servidor de conteúdo em um local remoto, como uma matriz. O servidor de conteúdo verifica se o computador cliente está autorizado a receber o conteúdo solicitado. Se o computador cliente está autorizado e conteúdo servidor e cliente são habilitados para BranchCache\, o servidor de conteúdo gera informações de conteúdo.

O servidor de conteúdo, em seguida, envia as informações de conteúdo para o computador cliente usando o mesmo protocolo como seria usado para o conteúdo em si. 

Por exemplo, se o computador cliente solicitou uma página da Web por HTTP, o servidor de conteúdo envia as informações de conteúdo usando HTTP. Por isso, garante a segurança de nível de transmissão do conteúdo e as informações de conteúdo são idênticos.

Depois que a parte inicial de informações de conteúdo (Hash de dados + segredo do segmento) é recebida, o computador cliente executa as seguintes ações:

- Usa o segredo do segmento (Kp) como a chave de criptografia (Ke).

- Gera a ID de segmento (HoHoDk) do método e Kp:

    `HoHoDk = HMAC(Kp, HoD + C), where C is the ASCII string "MS_P2P_CACHING" with NUL terminator.`

A principal ameaça nesta camada é o risco para o segredo do segmento, mas BranchCache criptografa os blocos de dados de conteúdo para proteger o segredo do segmento. BranchCache faz isso usando a chave de criptografia é derivada o segredo do segmento do segmento conteúdo dentro do qual os blocos de conteúdo estão localizados.

Essa abordagem garante que uma entidade que não está em posse do segredo do servidor não pode descobrir o conteúdo real em um bloco de dados. O segredo do segmento é tratado com o mesmo nível de segurança como o texto sem formatação segmento em si, como o conhecimento do segmento segredo para um determinado segmento permite que uma entidade obter o segmento de pares e, em seguida, descriptografá-los. Conhecimento do segredo do servidor não produz nenhum texto sem formatação específico imediatamente, mas pode ser usado para derivar certos tipos de dados do texto de codificação e, em seguida, possivelmente expor conhecido parcialmente alguns dados para um ataque de adivinhação de força bruta. O segredo do servidor, portanto, deve ser mantido em segredo.
  
## <a name="BKMK_9"></a>BranchCache processos: localizar o conteúdo

Depois que as informações de conteúdo são recebidas pelo computador cliente, o cliente usa a ID do segmento para localizar o conteúdo solicitado no cache local branch office, se o cache é distribuído entre computadores cliente ou está localizado em um servidor de cache hospedado.

Se o computador cliente é configurado para o modo de cache hospedado, ele está configurado com o nome do computador do servidor cache hospedado e contata o servidor para recuperar o conteúdo.

Se o computador cliente é configurado para o modo de cache distribuído, no entanto, o conteúdo pode ser armazenado em vários caches em vários computadores na filial. O computador cliente deve descobrir onde o conteúdo está localizado antes do conteúdo é recuperado.

Quando eles são configurados para o modo de cache distribuído, os computadores cliente localizam o conteúdo usando um protocolo de detecção que se baseia no protocolo detecção dinâmica de serviços de Web (WS-Discovery). Os clientes enviam WS-Discovery multicast mensagens de teste para descobrir o conteúdo armazenado em cache pela rede. Mensagens de teste incluem a identificação do segmento, que permite que os clientes verificar se o conteúdo solicitado coincide com o conteúdo armazenado em cache. Clientes que recebem a resposta de mensagem de teste inicial para o cliente consulta com mensagens unicast de correspondência de teste se a ID do segmento corresponda ao conteúdo que é armazenado em cache localmente.  

O sucesso do processo de WS-Discovery depende do fato de que o cliente que está realizando a descoberta tem as informações corretas de conteúdo, que foi fornecidas pelo servidor de conteúdo, para o conteúdo que está solicitando.

A principal ameaça aos dados durante a fase de conteúdo de solicitação é divulgação de informações, porque o acesso às informações conteúdos implica acesso autorizado ao conteúdo. Para atenuar esse risco, o processo de detecção não revelar as informações de conteúdo, que não seja a ID do segmento, que não revela nada sobre o segmento de texto sem formatação que contém o conteúdo.

Além disso, outro computador cliente executado por um usuário mal-intencionado na mesma sub-rede da rede pode ver o tráfego de descoberta BranchCache para a origem de conteúdo passando pelo roteador.

Se o conteúdo solicitado não for encontrado na filial, o cliente solicita o conteúdo diretamente do servidor de conteúdo através do link WAN.

Depois que o conteúdo é recebido, ele é adicionado ao cache local, no computador cliente ou em um servidor de cache hospedado. Nesse caso, as informações de conteúdo impede que um cliente ou servidor de cache de adição no cache local de qualquer conteúdo que não coincide com os hashes hospedado. O processo de verificação de conteúdo, correspondendo hashes garante que o conteúdo somente válido é adicionado ao cache, e a integridade do cache local está protegida.

## <a name="BKMK_10"></a>BranchCache processos: recuperar conteúdo

Depois que um computador cliente localiza o conteúdo desejado no host de conteúdo, que é um servidor de cache hospedado ou um computador do cliente de modo cache distribuído, o computador cliente começa o processo de recuperação do conteúdo.

Primeiro o computador cliente envia uma solicitação para o host de conteúdo para o primeiro bloco que ele necessita. A solicitação contém o intervalo de ID de segmento e o bloco que identificam o conteúdo desejado. Como somente um bloco é retornado, o intervalo de bloco contém apenas um único bloco. (Solicitações de vários blocos atualmente não são suportadas.) O cliente também armazena a solicitação em sua lista de solicitação pendente local.  

Ao receber uma mensagem de solicitação válido de um cliente, o host de conteúdo verifica se o bloco especificado na solicitação existe no cache de conteúdo do host de conteúdo.

Se o host de conteúdo estiver em posse do bloco de conteúdo, o host de conteúdo envia uma resposta que contém a ID de segmento, a ID do bloco, o bloco de dados criptografados e o vetor de inicialização é usado para criptografar o bloco.

Se o host de conteúdo não estiver em posse do bloco de conteúdo, o host de conteúdo envia uma mensagem de resposta vazio. Isso informa o computador cliente que o host de conteúdo não tem o bloco solicitado. Uma mensagem de resposta vazio contém a ID do segmento e a ID do bloco do bloco solicitado, juntamente com um bloco de dados de tamanho zero.

Quando o computador cliente recebe a resposta do host de conteúdo, o cliente verifica se a mensagem corresponde a uma mensagem de solicitação na sua lista de solicitações pendentes. (O índice de ID de segmento e bloco deve coincidir com que uma solicitação pendente.)

Se esse processo de verificação não for bem-sucedida, e o computador cliente não tem uma mensagem de solicitação correspondente na sua lista de solicitações pendentes, o computador cliente descarta a mensagem.

Se esse processo de verificação for bem-sucedida, e o computador cliente tem uma mensagem de solicitação correspondente na sua lista de solicitações pendentes, o computador cliente descriptografa o bloco. O cliente, em seguida, valida o bloco descriptografado contra o hash de bloco apropriado com as informações de conteúdo que o cliente obteve inicialmente do servidor de conteúdo original.

Se a validação do bloco for bem-sucedida, o bloco descriptografado é armazenado em cache.

Esse processo é repetido até que o cliente tenha todos os blocos necessários.

> [!NOTE]
> Se os segmentos completos de conteúdo que não existem em um computador, o protocolo de recuperação recupera e reúne o conteúdo de uma combinação de fontes: um conjunto de distribuída cache modo os computadores cliente, um servidor de cache hospedado e - se a filial armazena em cache não contêm o conteúdo completo - o servidor de conteúdo original na matriz.

Antes de BranchCache envia informações de conteúdo ou conteúdo, os dados são criptografados. BranchCache criptografa o bloco na mensagem de resposta. No Windows 7, o algoritmo de criptografia padrão que usa BranchCache é AES-128, a chave de criptografia é Ke e o tamanho da chave é 128 bits, conforme indicado pelo algoritmo de criptografia. 

BranchCache gera um vetor de inicialização que é adequado para o algoritmo de criptografia e usa a chave de criptografia para criptografar o bloco. BranchCache registra o algoritmo de criptografia e o vetor de inicialização na mensagem. 

Clientes e servidores nunca do exchange, compartilharem ou enviar uns aos outros a chave de criptografia. O cliente recebe a chave de criptografia do servidor de conteúdo que hospeda o conteúdo de origem. Em seguida, usando o vetor de algoritmo e inicialização de criptografia que ele recebidos do servidor, ele descriptografa o bloco. Há outros autenticação explícita ou autorização incorporado o protocolo de download.

### <a name="security-threats"></a>Ameaças de segurança

As ameaças de segurança principal nesta camada incluem:

- Interferência nos dados:

  *Um cliente de serviços de dados para um solicitante violar os dados*. O modelo de segurança BranchCache usa hashes para confirmar que o cliente nem o servidor tenha alterado os dados.  

- Divulgação de informações:  

    *BranchCache envia o conteúdo criptografado para qualquer cliente que especifica a ID de segmento apropriado*. IDs de segmento são públicos, para que qualquer cliente possa receber conteúdo criptografado. No entanto, se um usuário mal-intencionado obtiver o conteúdo criptografado, ele devem saber a chave de criptografia para descriptografar o conteúdo. O protocolo de camada superior realiza a autenticação e permite que as informações de conteúdo para o cliente autenticado e autorizado. A segurança das informações de conteúdo é equivalente à segurança fornecida para o conteúdo em si e BranchCache nunca exponha as informações de conteúdo.  

    *Um invasor fareja a transmissão para obter o conteúdo*.  BranchCache criptografa todas as transferências entre clientes usando AES128 onde a chave secreta é Ke, impedindo que dados de sendo detectados por da conexão.  Informações de conteúdo que são baixadas do servidor de conteúdo são protegidas exatamente da mesma maneira como os dados em si teria sido e, portanto, não mais ou menos protegidos contra divulgação de informações que se BranchCache não tivesse sido usado em todos.

-   Negação de serviço:

    *Um cliente é sobrecarregado por solicitações de dados*. BranchCache protocolos incorporam contadores de gerenciamento da fila e temporizadores para impedir que os clientes sendo sobrecarregado.

## <a name="BKMK_11"></a>BranchCache processos: armazenar em Cache conteúdo

Em computadores de cliente do modo de cache distribuído e servidores de cache hospedado que estão localizados em filiais, caches de conteúdo são criados ao longo do tempo conforme o conteúdo é recuperado por links WAN.

Quando os computadores cliente são configurados com o modo de cache hospedado, eles adicionar conteúdo a sua próprias cache local e também oferecem dados para o servidor de cache hospedado. O protocolo de Cache hospedado fornece um mecanismo para os clientes informar o servidor de cache hospedado sobre a disponibilidade do conteúdo e segmentá.

Para carregar conteúdo para o servidor de cache hospedado, o cliente informa ao servidor que ele tem um segmento que está disponível. O servidor de cache hospedado, em seguida, recupera todas as informações de conteúdo que está associado com o segmento oferecido, downloads e os blocos no segmento que ele é realmente necessário. Esse processo é repetido até que o cliente tenha sem mais segmentos para oferecer o servidor de cache hospedado.

Para atualizar o servidor de cache hospedado usando o protocolo de Cache hospedado, os requisitos a seguir devem ser atendidos:

- O computador cliente é necessário ter um conjunto de blocos em um segmento que ele pode oferecer para o servidor de cache hospedado. O cliente deve fornecer informações de conteúdo para o segmento oferecida; Isso é composto a ID de segmento, o segmento de Hash de dados, o segredo do segmento e uma lista de todos os hashes de bloco que estão contidos dentro do segmento.

- Para o cache hospedado servidores que executam o Windows Server 2008 R2, um servidor de cache hospedado certificado e a chave privada associada são necessários e a autoridade de certificação (CA) que emitiu o certificado deve ser confiável pelos computadores cliente na filial. Isso permite que o cliente e servidor participar com êxito na autenticação de servidor HTTPS.

    > [!IMPORTANT]
    > Servidores de cache hospedado que executam o Windows Server 2012, Windows Server 2012 R2 ou Windows Server 2016 não exigem um certificado do servidor de cache hospedado e a chave privada associada.  

- O computador cliente é configurado com o nome do computador do servidor cache hospedado e o número da porta de protocolo de controle de transmissão (TCP) no qual o servidor de cache hospedado está aguardando BranchCache tráfego. O cache hospedado certificado do servidor é associado a essa porta. O nome do computador do servidor cache hospedado pode ser um nome de domínio totalmente qualificado (FQDN), se o servidor de cache hospedado é um computador membro do domínio; ou ele pode ser o nome NetBIOS do computador se o servidor de cache hospedado não for um membro do domínio.

- O computador cliente ativamente escuta bloquear as solicitações de entrada. A porta na qual ele está ouvindo é passada como parte das mensagens oferta do cliente para o servidor de cache hospedado. Isso permite que o servidor de cache hospedado usam protocolos de BranchCache para se conectar a computador cliente para recuperar blocos de dados no segmento.

- O servidor de cache hospedado começa a ouvir solicitações HTTP de entrada quando ele é inicializado.

- Se o servidor de cache hospedado está configurado para exigir autenticação de computador cliente, o cliente e o servidor de cache hospedado são necessários para suporte a autenticação HTTPS.

### <a name="hosted-cache-mode-cache-population"></a>População de cache do modo de cache hospedado

O processo de adição de conteúdo em cache do servidor de cache hospedado em uma filial começa quando o cliente envia um INITIAL_OFFER_MESSAGE, que inclui a ID do segmento. A ID do segmento na solicitação INITIAL_OFFER_MESSAGE é usada para recuperar o Hash de dados de segmento correspondente, lista de hashes de bloco e o segredo do segmento do cache do bloco do servidor de cache hospedado. Se o servidor de cache hospedado já tem todas as informações de conteúdo de um segmento específico, a resposta para o INITIAL_OFFER_MESSAGE será Okey, e nenhuma solicitação para baixar blocos ocorre.

Se o servidor de cache hospedado não tem todos os blocos de dados oferecida que estão associados com os hashes de bloco no segmento, a resposta para a INITIAL_OFFER_MESSAGE está INTERESSADA. O cliente envia um SEGMENT_INFO_MESSAGE que descreve o segmento único que está sendo oferecido. O servidor de cache hospedado responde com uma mensagem Okey e inicia o download dos blocos de ausentes da oferta do computador cliente.

O segmento de Hash de dados, lista de hashes de bloco e o segredo do segmento são usados para garantir que o conteúdo que está sendo baixado não foi violado ou caso contrário, é alterado. Os blocos baixados, em seguida, são adicionados ao cache de bloco do servidor de cache hospedado.

## <a name="bkmk_cache"></a>Segurança de cache  
Esta seção fornece informações sobre como BranchCache protege os dados em cache em computadores cliente e em servidores de cache hospedado.

### <a name="client-computer-cache-security"></a>Segurança de cache do computador cliente
A maior ameaça dados armazenados no BranchCache é violação. Em seguida, se um invasor pode falsificar com informações de conteúdo e o conteúdo que são armazenadas em cache, seria possível usar isso para tentar e iniciar um ataque contra os computadores que estão usando BranchCache. Os invasores podem iniciar um ataque inserindo um software mal-intencionado no lugar de outros dados. BranchCache minimiza essa ameaça Validando todo o conteúdo usando hashes de bloco encontrados nas informações de conteúdo. Se um invasor tentar adulterar esses dados, ela é descartada e é substituída por dados válidos da fonte original.

Uma ameaça secundária dados armazenados no BranchCache é divulgação de informações. No modo de cache distribuído, o cliente armazena em cache apenas o conteúdo que ele solicitou propriamente dito; No entanto, esses dados são armazenados em texto não criptografado e podem estar em risco. Para ajudar a restringir o acesso de cache para o BranchCache Service só, o cache local é protegido por permissões do sistema de arquivos que são especificadas em uma ACL. 

Embora a ACL é eficaz para impedir que usuários não autorizados acessem o cache, é possível que um usuário com privilégios administrativos acessar o cache alterando as permissões que são especificadas na ACL manualmente. BranchCache não protege contra o uso mal-intencionado de uma conta administrativa.

Dados armazenados em cache conteúdo não são criptografados, portanto, se a perda de dados for uma preocupação, você pode usar tecnologias de criptografia, como o BitLocker ou o Encrypting File System (EFS). O cache local usado pelo BranchCache não aumenta a ameaça de divulgação de informações transmitidas por um computador na filial; o cache contém apenas cópias dos arquivos que residem descriptografadas em outro lugar no disco. 

Criptografar todo o disco é especialmente importante em ambientes em que a segurança física dos clientes é difícil garantir. Por exemplo, criptografar todo o disco ajuda a proteger dados confidenciais em computadores móveis que podem ser removidos do ambiente office ramificação.

### <a name="hosted-cache-server-cache-security"></a>Segurança de cache do servidor de cache hospedado

No modo de cache hospedado, a maior ameaça à segurança do servidor cache hospedado é divulgação de informações. BranchCache em um ambiente de cache hospedado se comporta de maneira semelhante ao modo de cache distribuído, com permissão de sistema de arquivo proteger os dados em cache. A diferença é que o servidor de cache hospedado armazena todos o conteúdo que qualquer computador habilitado para BranchCache na filial solicita, em vez de apenas os dados que solicita um único cliente. As consequências de invasão não autorizada nesse cache podem ser muito mais sérias, porque há muito mais dados em risco.  
  
Em um ambiente de cache hospedado em que o servidor de cache hospedado está executando o Windows Server 2008 R2, o uso de tecnologias de criptografia, como o BitLocker ou EFS é aconselhável se qualquer um dos clientes na filial poderá acessar dados confidenciais por meio do link WAN. Também é necessário para evitar o acesso físico para o cache hospedado, como funciona a criptografia de disco somente quando o computador é desligado quando o invasor obtém acesso físico.  Se o computador está ativado ou está em modo de suspensão, criptografia de disco oferece proteção pouco.

> [!NOTE]
> Servidores de cache hospedado que executam o Windows Server 2012, Windows Server 2012 R2 ou Windows Server 2016 criptografar todos os dados em cache por padrão, portanto, o uso de tecnologias de criptografia adicionais não é necessário.

Mesmo se um cliente estiver configurado no modo de cache hospedado, ainda será dados em cache localmente e você pode querer tome medidas para proteger o cache local além do cache no servidor do cache hospedado.
