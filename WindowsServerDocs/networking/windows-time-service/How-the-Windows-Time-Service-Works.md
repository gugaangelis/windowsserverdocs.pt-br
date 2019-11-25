---
ms.assetid: d1953097-63ea-4a0e-b860-2f3b7c175c41
title: Como funciona o serviço Horário do Windows
description: ''
author: shortpatti
ms.author: pashort
manager: dougkim
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.openlocfilehash: 2bf4a887218cd51e9c10954a75bbc1ba2112647f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405148"
---
# <a name="how-the-windows-time-service-works"></a>Como funciona o serviço Horário do Windows

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10 ou posterior

**Nesta seção**  
  
-   [Arquitetura de serviço de tempo do Windows](#windows-time-service-architecture)  
  
-   [Tempo do Windows Tempo de Serviço protocolos](#windows-time-service-time-protocols)  
  
-   [Processos e interações de serviço de tempo do Windows](#windows-time-service-processes-and-interactions)  
  
-   [Portas de rede usadas pelo serviço de tempo do Windows](#network-ports-used-by-windows-time-service)  
  
> [!NOTE]  
> Este tópico explica como funciona o serviço de tempo do Windows (W32Time). Para obter informações sobre como configurar o serviço de tempo do Windows, consulte [Configurando sistemas para alta precisão](configuring-systems-for-high-accuracy.md).
  
> [!NOTE]  
> No Windows Server 2003 e no Microsoft Windows 2000 Server, o serviço de diretório é nomeado Active Directory Directory Service. No Windows Server 2008 e versões posteriores, o serviço de diretório é denominado Active Directory Domain Services (AD DS). O restante deste tópico refere-se ao AD DS, mas as informações também são aplicáveis para o Active Directory.  
  
Embora o serviço de tempo do Windows não seja uma implementação exata do NTP (protocolo NTP), ele usa o complexo conjunto de algoritmos que é definido nas especificações de NTP para garantir que os relógios em computadores em toda a rede sejam os mais precisos possível. O ideal é que todos os relógios dos computadores em um domínio AD DS sejam sincronizados com a hora de um computador autoritativo. Muitos fatores podem afetar a sincronização de horário em uma rede. Os seguintes fatores geralmente afetam a precisão da sincronização no AD DS:  
  
-   Condições de rede  
  
-   A precisão do relógio do hardware do computador  
  
-   A quantidade de recursos de CPU e de rede disponíveis para o serviço de tempo do Windows  
  
> [!IMPORTANT]  
> Antes do Windows Server 2016, o serviço W32Time não foi projetado para atender às necessidades de aplicativos sensíveis ao tempo.  No entanto, as atualizações para o Windows Server 2016 agora permitem que você implemente uma solução para precisão 1 ms em seu domínio.  Consulte limite de tempo e suporte [precisos do windows 2016](accurate-time.md) [para configurar o serviço de tempo do Windows para ambientes de alta precisão](support-boundary.md) para obter mais informações.  
  
Os computadores que sincronizam seu tempo com menos frequência ou que não são associados a um domínio são configurados, por padrão, para sincronizar com o time.windows.com.  Portanto, é impossível garantir a precisão do tempo em computadores que têm conexões de rede intermitentes ou não.  
  
Uma floresta AD DS tem uma hierarquia de sincronização de tempo predeterminada. O serviço de tempo do Windows sincroniza o tempo entre computadores na hierarquia, com os relógios de referência mais precisos na parte superior. Se mais de uma fonte de tempo estiver configurada em um computador, o tempo do Windows usará algoritmos NTP para selecionar a melhor fonte de tempo das fontes configuradas com base na capacidade do computador de sincronizar com essa fonte de tempo. O serviço de tempo do Windows não dá suporte à sincronização de rede de pares de difusão ou multicast. Para obter mais informações sobre esses recursos de NTP, consulte RFC 1305 no banco de dados RFC da IETF.  
  
Todos os computadores que executam o serviço de tempo do Windows usam o serviço para manter o tempo mais preciso. Os computadores que são membros de um domínio atuam como um cliente de tempo por padrão, portanto, na maioria dos casos, não é necessário configurar o serviço de tempo do Windows. No entanto, o serviço de tempo do Windows pode ser configurado para solicitar a hora de uma fonte de tempo de referência designada e também pode fornecer tempo aos clientes.
  
O grau para o qual a hora do computador é preciso é chamado de estrato. A fonte de tempo mais precisa em uma rede (como um relógio de hardware) ocupa o nível mais baixo de estrato ou a camada um. Essa fonte de tempo precisa é chamada de relógio de referência. Um servidor NTP que adquire seu tempo diretamente de um relógio de referência ocupa um estrato que é um nível superior ao do relógio de referência. Os recursos que adquirem o tempo do servidor NTP são duas etapas distantes do relógio de referência e, portanto, ocupam um estrato maior do que a fonte de tempo mais precisa e assim por diante. À medida que o número de estrato de um computador aumenta, o tempo no relógio do sistema pode se tornar menos preciso. Portanto, o nível de estrato de qualquer computador é um indicador de quão próximo esse computador é sincronizado com a fonte de tempo mais precisa.  
  
Quando o Gerenciador do W32Time recebe amostras de tempo, ele usa algoritmos especiais no NTP para determinar qual das amostras de tempo é a mais apropriada para uso. O serviço de tempo também usa outro conjunto de algoritmos para determinar qual das fontes de tempo configuradas é a mais precisa. Quando o serviço de tempo tiver determinado qual exemplo de tempo é melhor, com base nos critérios acima, ele ajustará a taxa de relógio local para permitir que ela seja conconvergida em direção ao horário correto. Se a diferença de tempo entre o relógio local e a amostra de hora precisa selecionada (também chamada de distorção de tempo) for muito grande para ser corrigida ajustando a taxa de relógio local, o serviço de tempo definirá o relógio local para a hora correta. Esse ajuste de taxa de relógio ou alteração de horário de relógio direto é conhecido como disciplina de relógio.  
  
## <a name="windows-time-service-architecture"></a>Arquitetura de serviço de tempo do Windows  
O serviço de tempo do Windows consiste nos seguintes componentes:  
  
-   Gerenciador de Controle de Serviços  
  
-   Service Manager de tempo do Windows  
  
-   Disciplina do relógio  
  
-   Provedores de tempo  
  
A figura a seguir mostra a arquitetura do serviço de tempo do Windows.  
  
**Arquitetura de serviço de tempo do Windows**  
  
![Tempo do Windows](../media/Windows-Time-Service/How-the-Windows-Time-Service-Works/trnt_sec_arcc.gif)
  
O Gerenciador de controle de serviço é responsável por iniciar e parar o serviço de tempo do Windows. A Service Manager de tempo do Windows é responsável por iniciar a ação dos provedores de tempo NTP incluídos no sistema operacional. O tempo do Windows Service Manager controla todas as funções do serviço de tempo do Windows e a União de todos os exemplos de tempo. Além de fornecer informações sobre o estado atual do sistema, como a fonte de tempo atual ou a última vez em que o relógio do sistema foi atualizado, o tempo do Windows Service Manager também é responsável pela criação de eventos no log de eventos.  
  
O processo de sincronização de tempo envolve as seguintes etapas:  
  
-   Os provedores de entrada solicitam e recebem amostras de tempo de fontes de tempo NTP configuradas.  
  
-   Essas amostras de tempo são passadas para o tempo do Windows Service Manager, que coleta todos os exemplos e os passa para o subcomponente de disciplina do relógio.  
  
-   O subcomponente de disciplina de relógio aplica os algoritmos NTP que resultam na seleção da melhor amostra de tempo.  
  
-   O subcomponente de disciplina de relógio ajusta a hora do relógio do sistema para o tempo mais preciso ajustando a taxa de clock ou alterando diretamente a hora.  
  
Se um computador tiver sido designado como um servidor de horário, ele poderá enviar a hora para qualquer computador solicitando a sincronização de horário a qualquer momento nesse processo.  
  
## <a name="windows-time-service-time-protocols"></a>Tempo do Windows Tempo de Serviço protocolos  

Os protocolos de tempo determinam como os relógios de dois computadores são sincronizados. Um protocolo de tempo é responsável por determinar as melhores informações de tempo disponíveis e convergir os relógios para garantir que um tempo consistente seja mantido em sistemas separados.  
  
O serviço de tempo do Windows usa o protocolo NTP (NTP) para ajudar a sincronizar o tempo em uma rede. O NTP é um protocolo de tempo da Internet que inclui os algoritmos de disciplina necessários para a sincronização de relógios. O NTP é um protocolo de tempo mais preciso do que o protocolo NTP simples (SNTP) usado em algumas versões do Windows; no entanto, o W32Time continua a dar suporte a SNTP para habilitar a compatibilidade com versões anteriores em computadores que executam serviços de tempo baseados em SNTP, como o Windows 2000.  
  
### <a name="network-time-protocol"></a>protocolo NTP  
Protocolo NTP (NTP) é o protocolo de sincronização de tempo padrão usado pelo serviço de tempo do Windows no sistema operacional. O NTP é um protocolo de tempo altamente escalonável e tolerante a falhas e é o protocolo usado com mais frequência para sincronizar os relógios dos computadores usando uma referência de hora designada.  
  
A sincronização de tempo de NTP ocorre por um período de tempo e envolve a transferência de pacotes NTP por uma rede. Os pacotes NTP contêm carimbos de data/hora que incluem um exemplo de tempo do cliente e do servidor que participa da sincronização de tempo.  
  
O NTP depende de um relógio de referência para definir o tempo mais preciso a ser usado e sincroniza todos os relógios em uma rede para esse relógio de referência. O NTP usa UTC (tempo Universal Coordenado) como o padrão universal para a hora atual. O UTC é independente dos fusos horários e permite que o NTP seja usado em qualquer lugar do mundo, independentemente das configurações de fuso horário.  
  
#### <a name="ntp-algorithms"></a>Algoritmos NTP  
O NTP inclui dois algoritmos, um algoritmo de filtragem de relógio e um algoritmo de seleção de relógio, para auxiliar o serviço de tempo do Windows na determinação da melhor amostra de tempo. O algoritmo de filtragem de relógio foi projetado para percorrer amostras de tempo recebidas de fontes de tempo consultadas e determinar as melhores amostras de tempo de cada fonte. O algoritmo de seleção de relógio determina então o servidor de tempo mais preciso na rede. Essas informações são passadas para o algoritmo de disciplina do relógio, que usa as informações coletadas para corrigir o relógio local do computador, ao mesmo tempo em que compensa erros devido à latência da rede e à precisão do relógio do computador.  
  
Os algoritmos NTP são mais precisos em condições de cargas de servidor e rede leves a moderadas. Assim como acontece com qualquer algoritmo que leva tempo de trânsito de rede, os algoritmos NTP podem funcionar inadequadamente em condições de congestionamento extremo da rede. Para obter mais informações sobre os algoritmos NTP, consulte RFC 1305 no banco de dados RFC da IETF.  
  
#### <a name="ntp-time-provider"></a>Provedor de tempo NTP  
O serviço de tempo do Windows é um pacote de sincronização de tempo completo que pode dar suporte a uma variedade de dispositivos de hardware e protocolos de tempo. Para habilitar esse suporte, o serviço usa provedores de tempo conectáveis. Um provedor de tempo é responsável por obter carimbos de data/hora precisos (da rede ou de hardware) ou para fornecer esses carimbos de data/hora a outros computadores na rede.  
  
O provedor NTP é o provedor de horário padrão incluído no sistema operacional. O provedor NTP segue os padrões especificados pelo NTP versão 3 para um cliente e servidor e pode interagir com clientes e servidores SNTP para compatibilidade com versões anteriores com o Windows 2000 e outros clientes SNTP. O provedor de NTP no serviço de tempo do Windows consiste nas duas partes a seguir:  
  
-   **Provedor de saída do NtpServer.** Esse é um servidor de horário que responde às solicitações de tempo do cliente na rede.  
  
-   **Provedor de entrada do NtpClient.** Esse é um cliente de tempo que obtém informações de tempo de outra fonte, um dispositivo de hardware ou um servidor NTP, e pode retornar exemplos de tempo que são úteis para sincronizar o relógio local.  
  
Embora as operações reais desses dois provedores estejam fortemente relacionadas, elas aparecem independentemente do serviço de tempo. A partir do Windows 2000 Server, quando um computador com Windows está conectado a uma rede, ele é configurado como um cliente NTP. Além disso, os computadores que executam o serviço de tempo do Windows só tentam sincronizar a hora com um controlador de domínio ou uma fonte de tempo especificada manualmente por padrão. Esses são os provedores de tempo preferenciais porque estão disponíveis automaticamente e protegem fontes de tempo.  
  
#### <a name="ntp-security"></a>Segurança de NTP  

Dentro de uma floresta AD DS, o serviço de tempo do Windows depende dos recursos de segurança de domínio padrão para impor a autenticação dos dados de tempo. A segurança dos pacotes NTP que são enviados entre um computador membro do domínio e um controlador de domínio local que está agindo como um servidor de horário é baseado na autenticação de chave compartilhada. O serviço de tempo do Windows usa a chave de sessão Kerberos do computador para criar assinaturas autenticadas em pacotes NTP que são enviados pela rede. Os pacotes NTP não são transmitidos dentro do canal seguro de logon de rede. Em vez disso, quando um computador solicita a hora de um controlador de domínio na hierarquia de domínio, o serviço de tempo do Windows requer que o tempo seja autenticado. O controlador de domínio retorna as informações necessárias na forma de um valor de 64 bits que foi autenticado com a chave de sessão do serviço de logon de rede. Se o pacote NTP retornado não for assinado com a chave de sessão do computador ou estiver assinado incorretamente, a hora será rejeitada. Todas essas falhas de autenticação são registradas no log de eventos. Dessa forma, o serviço de tempo do Windows fornece segurança para dados NTP em uma floresta AD DS.  
  
Geralmente, os clientes de tempo do Windows obtêm automaticamente o tempo preciso para a sincronização de controladores de domínio no mesmo domínio. Em uma floresta, os controladores de domínio de um domínio filho sincronizam a hora com controladores de domínio em seus domínios pai. Quando um servidor de horário retorna um pacote NTP autenticado para um cliente que solicita a hora, o pacote é assinado por meio de uma chave de sessão Kerberos definida por uma conta de confiança entre domínios. A conta de confiança entre domínios é criada quando um novo domínio de AD DS ingressa em uma floresta e o serviço de logon de rede gerencia a chave de sessão. Dessa forma, o controlador de domínio configurado como confiável no domínio raiz da floresta torna-se a fonte de tempo autenticada para todos os controladores de domínio nos domínios pai e filho, e indiretamente para todos os computadores localizados na árvore de domínio.  
  
O serviço de tempo do Windows pode ser configurado para funcionar entre florestas, mas é importante observar que essa configuração não é segura. Por exemplo, um servidor NTP pode estar disponível em uma floresta diferente. No entanto, como esse computador está em uma floresta diferente, não há nenhuma chave de sessão Kerberos com a qual assinar e autenticar pacotes NTP. Para obter uma sincronização de tempo precisa de um computador em uma floresta diferente, o cliente precisa de acesso à rede para esse computador e o serviço de tempo deve ser configurado para usar uma fonte de tempo específica localizada na outra floresta. Se um cliente for configurado manualmente para acessar o tempo de um servidor NTP fora de sua própria hierarquia de domínio, os pacotes NTP enviados entre o cliente e o servidor de horário não serão autenticados e, portanto, não serão protegidos. Mesmo com a implementação de relações de confiança de floresta, o serviço de tempo do Windows não é seguro entre florestas. Embora o canal de segurança de logon NET seja o mecanismo de autenticação do serviço de tempo do Windows, não há suporte para autenticação entre florestas.  
  
#### <a name="hardware-devices-that-are-supported-by-the-windows-time-service"></a>Dispositivos de hardware com suporte no serviço de tempo do Windows  
Os relógios baseados em hardware, como o GPS ou os relógios de rádio, geralmente são usados como dispositivos de relógio de referência altamente precisos. Por padrão, o provedor de tempo NTP do serviço de tempo do Windows não oferece suporte à conexão direta de um dispositivo de hardware para um computador, embora seja possível criar um provedor de tempo independente baseado em software que dê suporte a esse tipo de conexão. Esse tipo de provedor, em conjunto com o serviço de tempo do Windows, pode fornecer uma referência de tempo estável e confiável.  
  
Dispositivos de hardware, como um Cesium clock ou um receptor de GPS (GPS), fornecem tempo atual preciso seguindo um padrão para obter uma definição precisa do tempo. Os relógios de Cesium são extremamente estáveis e não são afetados por fatores como temperatura, pressão ou umidade, mas também são muito caros. Um receptor GPS é muito menos dispendioso para operar e também é um relógio de referência preciso. Os receptores GPS obtêm seu tempo a partir de satélites que obtêm seu tempo de um relógio Cesium. Sem o uso de um provedor de tempo independente, os servidores de tempo do Windows podem adquirir seu tempo conectando-se a um servidor NTP externo, que é conectado a um dispositivo de hardware por meio de um telefone ou da Internet. Organizações como o Estados Unidos naval Observatório fornecem servidores NTP conectados a relógios de referência extremamente confiáveis.  
  
Muitos receptores de GPS e outros dispositivos de tempo podem funcionar como servidores NTP em uma rede. Você pode configurar sua floresta AD DS para sincronizar a hora desses dispositivos de hardware externos somente se eles também estiverem agindo como servidores NTP em sua rede. Para fazer isso, configure o controlador de domínio funcionando como o emulador do controlador de domínio primário (PDC) na raiz da floresta para sincronizar com o servidor NTP fornecido pelo dispositivo GPS. Para fazer isso, consulte [Configurar o serviço de tempo do Windows no emulador de PDC no domínio raiz da floresta](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731191%28v=ws.10%29).  
  
### <a name="simple-network-time-protocol"></a>protocolo NTP simples  
O protocolo NTP simples (SNTP) é um protocolo de tempo simplificado destinado a servidores e clientes que não exigem o grau de precisão que o NTP fornece. O SNTP, uma versão mais rudimentar do NTP, é o protocolo de tempo primário usado no Windows 2000. Como os formatos de pacote de rede do SNTP e do NTP são idênticos, os dois protocolos são interoperáveis. A principal diferença entre os dois é que o SNTP não tem o gerenciamento de erros e os sistemas de filtragem complexos que o NTP fornece. Para obter mais informações sobre o protocolo NTP simples, consulte RFC 1769 no banco de dados RFC da IETF.  
  
### <a name="time-protocol-interoperability"></a>Interoperabilidade de protocolo de tempo  
O serviço de tempo do Windows pode operar em um ambiente misto de computadores que executam o Windows 2000, o Windows XP e o Windows Server 2003, pois o protocolo SNTP usado no Windows 2000 é interoperável com o protocolo NTP no Windows XP e no Windows Server 2003.  
  
O serviço de tempo no Windows NT Server 4,0, chamado Timeserv, sincroniza o tempo em uma rede do Windows NT 4,0. Timeserv é um recurso complementar disponível como parte do *Microsoft Windows NT 4,0 Resource Kit* e não fornece o grau de confiabilidade de sincronização de tempo exigido pelo Windows Server 2003.  
  
O serviço de tempo do Windows pode interoperar com computadores que executam o Windows NT 4,0 porque eles podem sincronizar a hora com computadores que executam o Windows 2000 ou o Windows Server 2003; no entanto, um computador que executa o Windows 2000 ou o Windows Server 2003 não descobre automaticamente os servidores de horário do Windows NT 4,0. Por exemplo, se seu domínio estiver configurado para sincronizar a hora usando o método de sincronização baseado em hierarquia de domínio e você quiser que os computadores na hierarquia de domínio sincronizem a hora com um controlador de domínio do Windows NT 4,0, você precisará configurá-los computadores manualmente para sincronizar com os controladores de domínio do Windows NT 4,0.  
  
O Windows NT 4,0 usa um mecanismo mais simples para a sincronização de tempo do que o serviço de tempo do Windows usa. Portanto, para garantir uma sincronização de tempo precisa em sua rede, é recomendável que você atualize todos os controladores de domínio do Windows NT 4,0 para o Windows 2000 ou o Windows Server 2003.  
  
## <a name="windows-time-service-processes-and-interactions"></a>Processos e interações de serviço de tempo do Windows  

O serviço de tempo do Windows foi projetado para sincronizar os relógios dos computadores em uma rede. O processo de sincronização de horário de rede, também chamado de convergência de tempo, ocorre em uma rede à medida que cada computador acessa o tempo de um servidor de tempo mais preciso. A convergência de tempo envolve um processo pelo qual um servidor autoritativo fornece a hora atual para os computadores cliente na forma de pacotes NTP. As informações fornecidas em um pacote indicam se um ajuste precisa ser feito na hora atual do relógio do computador para que ele seja sincronizado com o servidor mais preciso.  
  
Como parte do processo de convergência de tempo, os membros do domínio tentam sincronizar a hora com qualquer controlador de domínio localizado no mesmo domínio. Se o computador for um controlador de domínio, ele tentará sincronizar com um controlador de domínio mais autoritativo.  
  
Computadores que executam o Windows XP Home Edition ou computadores que não ingressaram em um domínio não tentam sincronizar com a hierarquia de domínio, mas são configurados por padrão para obter o tempo de time.windows.com.  
  
Para estabelecer um computador executando o Windows Server 2003 como autoritativo, o computador deve ser configurado para ser uma fonte de tempo confiável. Por padrão, o primeiro controlador de domínio instalado em um domínio do Windows Server 2003 é configurado automaticamente para ser uma fonte de tempo confiável. Como é o computador autoritativo para o domínio, ele deve ser configurado para sincronizar com uma fonte de tempo externa em vez de com a hierarquia de domínio. Além disso, por padrão, todos os outros membros do domínio do Windows Server 2003 são configurados para sincronizar com a hierarquia de domínio.  
  
Depois de estabelecer uma rede do Windows Server 2003, você pode configurar o serviço de tempo do Windows para usar uma das seguintes opções de sincronização:  
  
-   Sincronização baseada em hierarquia de domínio  
  
-   Uma origem de sincronização especificada manualmente  
  
-   Todos os mecanismos de sincronização disponíveis  
  
-   Sem sincronização.  
  
Cada um desses tipos de sincronização é discutido na seção a seguir.  
  
### <a name="domain-hierarchy-based-synchronization"></a>Sincronização baseada em hierarquia de domínio  

A sincronização baseada em uma hierarquia de domínio usa a hierarquia de domínio AD DS para encontrar uma fonte confiável com a qual sincronizar a hora. Com base na hierarquia de domínio, o serviço de tempo do Windows determina a precisão de cada servidor de horário. Em uma floresta do Windows Server 2003, o computador que contém a função de mestre de operações do emulador de controlador de domínio primário (PDC), localizada no domínio raiz da floresta, mantém a posição da melhor fonte de tempo, a menos que outra fonte de tempo confiável tenha sido configurada. A figura a seguir ilustra um caminho de sincronização de tempo entre computadores em uma hierarquia de domínio.  
  
**Sincronização de tempo em uma hierarquia de AD DS**  
![de tempo do Windows](../media/Windows-Time-Service/How-the-Windows-Time-Service-Works/trnt_ntw_adhc.gif)
  
#### <a name="reliable-time-source-configuration"></a>Configuração de fonte de tempo confiável  
Um computador configurado para ser uma fonte de tempo confiável é identificado como a raiz do serviço de tempo. A raiz do serviço de tempo é o servidor autoritativo para o domínio e normalmente é configurada para recuperar o tempo de um servidor NTP externo ou dispositivo de hardware. Um servidor de horário pode ser configurado como uma fonte de tempo confiável para otimizar como o tempo é transferido em toda a hierarquia de domínio. Se um controlador de domínio estiver configurado para ser uma fonte de tempo confiável, o serviço de logon de rede anunciará esse controlador de domínio como uma fonte de tempo confiável quando ele fizer logon na rede. Quando outros controladores de domínio procuram uma fonte de tempo para sincronizar com o, eles escolhem uma fonte confiável primeiro, se houver uma disponível.  
  
#### <a name="time-source-selection"></a>Seleção de origem de tempo  
O processo de seleção de origem de tempo pode criar dois problemas em uma rede:  
  
-   Ciclos de sincronização adicionais.  
  
-   Aumento do volume no tráfego de rede.  
  
Um ciclo na rede de sincronização ocorre quando o tempo permanece consistente entre um grupo de controladores de domínio e o mesmo tempo é compartilhado entre eles continuamente sem uma ressincronização com outra fonte de tempo confiável. O algoritmo de seleção de fonte de tempo do serviço de tempo do Windows foi projetado para proteger contra esses tipos de problemas.  
  
Um computador usa um dos seguintes métodos para identificar uma fonte de tempo para sincronizar com:  
  
-   Se o computador não for membro de um domínio, ele deverá ser configurado para sincronizar com uma fonte de tempo especificada.  
  
-   Se o computador for um servidor membro ou uma estação de trabalho em um domínio, por padrão, ele seguirá a hierarquia de AD DS e sincronizará seu tempo com um controlador de domínio em seu domínio local que está atualmente executando o serviço de tempo do Windows.  
  
Se o computador for um controlador de domínio, ele fará até seis consultas para localizar outro controlador de domínio com o qual sincronizar. Cada consulta é projetada para identificar uma fonte de tempo com determinados atributos, como um tipo de controlador de domínio, um local específico e se é uma fonte de tempo confiável. A fonte de tempo também deve aderir às seguintes restrições:  
  
-   Uma fonte de tempo confiável só pode sincronizar com um controlador de domínio no domínio pai.  
  
-   Um emulador PDC pode sincronizar com uma fonte de tempo confiável em seu próprio domínio ou em qualquer controlador de domínio no domínio pai.  
  
Se o controlador de domínio não for capaz de sincronizar com o tipo de controlador de domínio que está consultando, a consulta não será feita. O controlador de domínio sabe de qual tipo de computador ele pode obter tempo antes de fazer a consulta. Por exemplo, um emulador de PDC local não tenta consultar números três ou seis porque um controlador de domínio não tenta sincronizar-se com ele mesmo.  
  
A tabela a seguir lista as consultas que um controlador de domínio faz para localizar uma fonte de tempo e a ordem na qual as consultas são feitas.  
  
**Consultas de origem de tempo do controlador de domínio**  
  
|Número da consulta|Controlador de domínio|Location|Confiabilidade da fonte de tempo|  
|----------------|---------------------|------------|------------------------------|  
|1|Controlador de domínio pai|No local|Prefere uma fonte de tempo confiável, mas pode sincronizar com uma fonte de tempo não confiável se isso for tudo disponível.|  
|2|Controlador de domínio local|No local|Só sincroniza com uma fonte de tempo confiável.|  
|3|Emulador de PDC local|No local|Não se aplica.<br /><br />Um controlador de domínio não tenta sincronizar com ele mesmo.|  
|추가를 클릭합니다.|Controlador de domínio pai|Fora do site|Prefere uma fonte de tempo confiável, mas pode sincronizar com uma fonte de tempo não confiável se isso for tudo disponível.|  
|5|Controlador de domínio local|Fora do site|Só sincroniza com uma fonte de tempo confiável.|  
|6|Emulador de PDC local|Fora do site|Não se aplica.<br /><br />Um controlador de domínio não tenta sincronizar com ele mesmo.| 
  
**Observação**  
  
-   Um computador nunca sincroniza com ele mesmo. Se o computador que está tentando sincronizar for o emulador de PDC local, ele não tentará as consultas 3 ou 6.  
  
Cada consulta retorna uma lista de controladores de domínio que podem ser usados como uma fonte de tempo. O tempo do Windows atribui a cada controlador de domínio uma pontuação que é consultada com base na confiabilidade e no local do controlador de domínio. A tabela a seguir lista as pontuações atribuídas pelo tempo do Windows a cada tipo de controlador de domínio.  
  
**Determinação da Pontuação**  
  
|Status do controlador de domínio|Pontuação|  
|----------------------------|---------|  
|Controlador de domínio localizado no mesmo site|8|  
|Controlador de domínio marcado como uma fonte de tempo confiável|추가를 클릭합니다.|  
|Controlador de domínio localizado no domínio pai|2|  
|Controlador de domínio que é um emulador de PDC|1|  
  
Quando o serviço de tempo do Windows determina que ele identificou o controlador de domínio com a melhor pontuação possível, nenhuma consulta é feita. As pontuações atribuídas pelo serviço de tempo são cumulativas, o que significa que um emulador de PDC localizado no mesmo site recebe uma pontuação de nove.  
  
Se a raiz do serviço de tempo não estiver configurada para sincronizar com uma fonte externa, o relógio de hardware interno do computador governará o tempo.  
  
### <a name="manually-specified-synchronization"></a>Sincronização especificada manualmente  
A sincronização especificada manualmente permite designar um único par ou lista de pares dos quais um computador obtém tempo. Se o computador não for membro de um domínio, ele deverá ser configurado manualmente para sincronizar com uma fonte de tempo especificada. Um computador que é membro de um domínio é configurado por padrão para sincronizar da hierarquia de domínio, a sincronização especificada manualmente é mais útil para a raiz de floresta do domínio ou para computadores que não fazem parte de um domínio. A especificação manual de um servidor NTP externo para sincronizar com o computador autoritativo para seu domínio fornece tempo confiável. No entanto, configurar o computador autoritativo para seu domínio para sincronizar com um relógio de hardware é, na verdade, uma solução melhor para fornecer o tempo de segurança mais preciso para o seu domínio.  
  
As fontes de tempo especificadas manualmente não são autenticadas, a menos que um provedor de tempo específico seja gravado para elas e, portanto, seja vulnerável a invasores. Além disso, se um computador sincronizar com uma origem especificada manualmente em vez de seu controlador de domínio de autenticação, os dois computadores poderão estar fora de sincronização, causando a falha da autenticação Kerberos. Isso pode fazer com que outras ações exigindo falha de autenticação de rede, como impressão ou compartilhamento de arquivos. Se apenas a raiz da floresta estiver configurada para sincronizar com uma fonte externa, todos os outros computadores da floresta permanecerão sincronizados entre si, dificultando os ataques de repetição.  
  
### <a name="all-available-synchronization-mechanisms"></a>Todos os mecanismos de sincronização disponíveis  

A opção "todos os mecanismos de sincronização disponíveis" é o método de sincronização mais valioso para usuários em uma rede. Esse método permite a sincronização com a hierarquia de domínio e também pode fornecer uma fonte de tempo alternativa se a hierarquia de domínio ficar indisponível, dependendo da configuração. Se o cliente não puder sincronizar a hora com a hierarquia de domínio, a fonte de tempo automaticamente retornará para a fonte de tempo especificada pela configuração do **NtpServer** . Esse método de sincronização tem maior probabilidade de fornecer um tempo preciso para os clientes.  

### <a name="stopping-time-synchronization"></a>Parando a sincronização de horário  
Há algumas situações em que você desejará impedir que um computador Sincronize seu tempo. Por exemplo, se um computador tentar sincronizar de uma fonte de tempo na Internet ou de outro site por meio de uma WAN por meio de uma conexão dial-up, isso poderá incorrer em encargos de telefone dispendiosos. Ao desabilitar a sincronização nesse computador, você impede que o computador tente acessar uma fonte de tempo em uma conexão discada.  
  
Você também pode desabilitar a sincronização para evitar a geração de erros no log de eventos. Cada vez que um computador tenta sincronizar com uma fonte de tempo que não está disponível, ele gera um erro no log de eventos. Se uma fonte de tempo for retirada da rede para manutenção agendada e você não pretende reconfigurar o cliente para sincronizar de outra fonte, você pode desabilitar a sincronização no cliente para impedi-lo de tentar a sincronização enquanto o servidor de horário Não está disponível.  
  
É útil desabilitar a sincronização no computador designado como a raiz da rede de sincronização. Isso indica que o computador raiz confia em seu relógio local. Se a raiz da hierarquia de sincronização não estiver definida como **nosync** e se não for possível sincronizar com outra fonte de tempo, os clientes não aceitarão o pacote enviado pelo computador porque seu tempo não pode ser confiável.
  
Os únicos servidores de tempo que são confiáveis para os clientes, mesmo que não tenham sido sincronizados com outra fonte de tempo, são aqueles identificados pelo cliente como servidores de horário confiáveis.  
  
### <a name="disabling-the-windows-time-service"></a>Desabilitando o serviço de tempo do Windows  
O serviço de tempo do Windows (W32Time) pode ser completamente desabilitado. Se você optar por implementar um produto de sincronização de tempo de terceiros que usa o NTP, será necessário desabilitar o serviço de tempo do Windows. Isso ocorre porque todos os servidores NTP precisam de acesso à porta 123 do protocolo UDP e, desde que o serviço de tempo do Windows esteja em execução no sistema operacional Windows Server 2003, a porta 123 permanece reservada pelo tempo do Windows.  
  
## <a name="network-ports-used-by-windows-time-service"></a>Portas de rede usadas pelo serviço de tempo do Windows  
O serviço de tempo do Windows se comunica em uma rede para identificar fontes de tempo confiáveis, obter informações de tempo e fornecer informações de tempo a outros computadores. Ele executa essa comunicação conforme definido pelas RFCs de NTP e SNTP.  
  
**Atribuições de porta para o serviço de tempo do Windows**  
  
|Nome do serviço|UDP|TCP|  
|----------------|-------|-------|  
|NTP|123|N/D|  
|SNTP|123|N/D|  
  
## <a name="see-also"></a>Consulte também  
[Referência técnica do serviço de tempo do windows](windows-time-service-tech-ref.md)
[ferramentas e configurações do serviço de tempo do windows](Windows-Time-Service-Tools-and-Settings.md)
[artigo 902229 da base de dados de conhecimento Microsoft](https://go.microsoft.com/fwlink/?LinkId=186066)