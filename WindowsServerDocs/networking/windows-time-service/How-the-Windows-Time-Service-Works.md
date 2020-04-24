---
ms.assetid: d1953097-63ea-4a0e-b860-2f3b7c175c41
title: Como funciona o serviço Horário do Windows
author: dcuomo
ms.author: dacuo
manager: dougkim
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.openlocfilehash: b8b30893abe4cdfe8d7e8c5a95ede651f85643a9
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/23/2020
ms.locfileid: "80861659"
---
# <a name="how-the-windows-time-service-works"></a>Como funciona o serviço Horário do Windows

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10 ou posterior

**Nesta seção**  
  
-   [Arquitetura do Serviço de Horário do Windows](#windows-time-service-architecture)  
  
-   [Protocolos de tempo do Serviço de Horário do Windows](#windows-time-service-time-protocols)  
  
-   [Processos e interações do Serviço de Horário do Windows](#windows-time-service-processes-and-interactions)  
  
-   [Portas de rede usadas pelo Serviço de Horário do Windows](#network-ports-used-by-windows-time-service)  
  
> [!NOTE]  
> Este tópico explica como funciona o Serviço de Horário do Windows (W32Time). Para obter informações de como configurar o Serviço de Horário do Windows, confira [Configuração de sistemas para alta precisão](configuring-systems-for-high-accuracy.md).
  
> [!NOTE]  
> No Windows Server 2003 e no Microsoft Windows 2000 Server, o serviço de diretório é denominado serviço de diretório Active Directory. No Windows Server 2008 e versões posteriores, o serviço de diretório é chamado de AD DS (Active Directory Domain Services). O restante deste tópico refere-se ao AD DS, mas as informações também são aplicáveis para o Active Directory.  
  
Embora o Serviço de Horário do Windows não seja uma implementação exata do protocolo NTP, ele usa o complexo conjunto de algoritmos definido nas especificações do NTP para verificar se os relógios em computadores em toda a rede são os mais precisos possíveis. O ideal é que todos os relógios dos computadores de um domínio do AD DS sejam sincronizados com a hora de um computador oficial. Muitos fatores podem afetar a sincronização de horário em uma rede. Os seguintes fatores geralmente afetam a precisão da sincronização no AD DS:  
  
-   Condições da rede  
  
-   A precisão do relógio do hardware do computador  
  
-   A quantidade de recursos de CPU e de rede disponíveis para o Serviço de Horário do Windows  
  
> [!IMPORTANT]  
> Antes do Windows Server 2016, o serviço W32Time não era criado para atender a necessidades de aplicativos sensíveis ao tempo.  Contudo, agora as atualizações do Windows Server 2016 permitem que você implemente uma solução para ter precisão de 1 ms em seu domínio.  Confira [Tempo Preciso do Windows 2016](accurate-time.md) e [Limite de suporte para configurar o Serviço de Horário do Windows para ambientes de alta precisão](support-boundary.md) para obter mais informações.  
  
Os computadores que sincronizam o horário com menos frequência ou que não são associados a um domínio, são configurados para sincronizar com o time.windows.com por padrão.  Portanto, é impossível garantir a precisão do tempo em computadores que têm conexões de rede intermitentes ou que não têm conexão.  
  
Uma floresta do AD DS tem uma hierarquia de sincronização de tempo predeterminada. O Serviço de Horário do Windows sincroniza o tempo entre computadores da hierarquia, com os relógios de referência mais precisos na parte superior. Se mais de uma fonte de horário estiver configurada em um computador, o Horário do Windows usará algoritmos NTP para selecionar a melhor fonte de horário dentre as fontes configuradas com base na capacidade do computador de sincronizar com essa fonte de horário. O Serviço de Horário do Windows não é compatível com a sincronização de rede de pares de difusão ou difusão seletiva. Para obter mais informações sobre esses recursos de NTP, confira RFC 1305 no banco de dados RFC da IETF.  
  
Todos os computadores que executam o Serviço de Horário do Windows usam o serviço para manter o tempo mais preciso. Os computadores que são membros de um domínio atuam como um cliente de tempo por padrão, portanto, na maioria dos casos, não é necessário configurar o Serviço de Horário do Windows. No entanto, o Serviço de Horário do Windows pode ser configurado para solicitar a hora de uma fonte de horário de referência designada e também pode fornecer o horário aos clientes.
  
O grau de precisão da hora de um computador é chamado de estrato. A fonte de horário mais precisa em uma rede (como um relógio de hardware) ocupa o nível mais baixo de estrato ou estrato um. Essa fonte de horário precisa é chamada de relógio de referência. Um servidor NTP que adquire o horário diretamente de um relógio de referência ocupa um estrato que é um nível acima do relógio de referência. Os recursos que adquirem o tempo do servidor NTP são dois níveis acima do relógio de referência e, portanto, ocupam um estrato maior do que a fonte de horário mais precisa e assim por diante. À medida que o número de estrato de um computador aumenta, o tempo no relógio do sistema pode se tornar menos preciso. Portanto, o nível de estrato de qualquer computador é um indicador de quão próximo ele está sincronizado com a fonte de horário mais precisa.  
  
Quando o Gerenciador do W32Time recebe amostras de tempo, ele usa algoritmos especiais do protocolo NTP para determinar qual das amostras de tempo é a mais apropriada para uso. O serviço de horário também usa outro conjunto de algoritmos para determinar qual das fontes de horário configuradas é a mais precisa. Quando o serviço de horário tiver determinado qual amostra de tempo é a melhor, com base nos critérios acima, ele ajustará a velocidade do relógio local para permitir que ela convirja em direção ao horário correto. Se a diferença de tempo entre o relógio local e a amostra de hora precisa selecionada (também chamada de distorção de tempo) for muito grande para ser corrigida ajustando a velocidade do relógio local, o serviço de horário definirá o relógio local com a hora correta. Esse ajuste da velocidade do relógio ou alteração direta do horário do relógio é conhecida como disciplina do relógio.  
  
## <a name="windows-time-service-architecture"></a>Arquitetura de serviço de tempo do Windows  
O Serviço de Horário do Windows consiste nos seguintes componentes:  
  
-   Gerenciador de Controle de Serviço  
  
-   Gerenciador do Serviço de Horário do Windows  
  
-   Disciplina do relógio  
  
-   Provedores de tempo  
  
A figura a seguir mostra a arquitetura do Serviço de Horário do Windows.  
  
**Arquitetura do Serviço de Horário do Windows**  
  
![Tempo do Windows](../media/Windows-Time-Service/How-the-Windows-Time-Service-Works/trnt_sec_arcc.gif)
  
O Gerenciador de Controle de Serviço é responsável por iniciar e parar o Serviço de Horário do Windows. O Gerenciador do Serviço de Horário do Windows é responsável por iniciar a ação dos provedores de horário de NTP incluídos no sistema operacional. O Gerenciador do Serviço de Horário do Windows controla todas as funções do Serviço de Horário do Windows e o agrupamento de todas as amostras de tempo. Além de fornecer informações sobre o estado atual do sistema, como a fonte de horário atual ou a última vez em que o relógio do sistema foi atualizado, o Gerenciador do Serviço de Horário do Windows também é responsável pela criação de eventos no log de eventos.  
  
O processo de sincronização do tempo envolve as seguintes etapas:  
  
-   Os provedores de entrada solicitam e recebem amostras de tempo de fontes de horário de NTP configuradas.  
  
-   Essas amostras de tempo são passadas para o Gerenciador do Serviço de Horário do Windows, que coleta todas as amostras e as passa para o subcomponente de disciplina do relógio.  
  
-   O subcomponente de disciplina do relógio aplica os algoritmos do NTP que resultam na seleção da melhor amostra de tempo.  
  
-   O subcomponente de disciplina do relógio ajusta a hora do relógio do sistema com o tempo mais preciso ajustando a velocidade do relógio ou alterando diretamente a hora.  
  
Se um computador tiver sido designado como um servidor de horário, ele poderá enviar a hora para qualquer computador solicitando a sincronização de horário a qualquer momento nesse processo.  
  
## <a name="windows-time-service-time-protocols"></a>Protocolos de tempo do Serviço de Horário do Windows  

Os protocolos de tempo determinam qual a precisão entre a sincronia dos relógios de dois computadores. Um protocolo de tempo é responsável por determinar as melhores informações de tempo disponíveis e convergir os relógios para garantir que um tempo consistente seja mantido em sistemas separados.  
  
O Serviço de Horário do Windows usa o NTP (protocolo NTP) para ajudar a sincronizar o tempo em uma rede. O NTP é um protocolo de tempo da Internet que inclui os algoritmos de disciplina necessários para sincronizar relógios. O NTP é um protocolo de tempo mais preciso que o SNTP (protocolo NTP simples) usado em algumas versões do Windows; no entanto, o W32Time continua dando suporte ao SNTP para permitir a compatibilidade com versões anteriores com computadores que executam serviços de tempo baseados em SNTP, como o Windows 2000.  
  
### <a name="network-time-protocol"></a>Protocolo NTP  
O NTP (protocolo NTP) é o protocolo de sincronização de tempo padrão usado pelo Serviço de Horário do Windows no sistema operacional. O NTP é um protocolo de tempo altamente escalonável e tolerante a falhas e é o protocolo usado com mais frequência para sincronizar os relógios dos computadores usando uma referência de hora designada.  
  
A sincronização de tempo do NTP ocorre durante um período de tempo e envolve a transferência de pacotes de NTP por uma rede. Os pacotes de NTP contêm carimbos de data/hora que incluem uma amostra de tempo do cliente e do servidor que participa da sincronização de tempo.  
  
O NTP depende de um relógio de referência para definir o tempo mais preciso a ser usado e sincroniza todos os relógios em uma rede de acordo com esse relógio de referência. O NTP usa o UTC (Tempo Universal Coordenado) como o padrão universal para a hora atual. O UTC não depende dos fusos horários e permite que o NTP seja usado em qualquer lugar do mundo, independentemente das configurações de fuso horário.  
  
#### <a name="ntp-algorithms"></a>Algoritmos de NTP  
O NTP inclui dois algoritmos, um algoritmo de filtragem de relógio e um algoritmo de seleção de relógio, para auxiliar o Serviço de Horário do Windows na determinação da melhor amostra de tempo. O algoritmo de filtragem de relógio foi projetado para percorrer amostras de tempo recebidas de fontes de horário consultadas e determinar as melhores amostras de tempo de cada fonte. O algoritmo de seleção de relógio determina então o servidor de tempo mais preciso na rede. Essas informações são passadas para o algoritmo de disciplina do relógio, que usa as informações coletadas para corrigir o relógio local do computador e compensar erros devido à latência da rede e à imprecisão do relógio do computador.  
  
Os algoritmos NTP são mais precisos em condições de cargas de servidor e rede leves a moderadas. Assim como acontece com qualquer algoritmo que leva em conta o tempo de trânsito na rede, os algoritmos NTP podem funcionar inadequadamente em condições de congestionamento extremo da rede. Para obter mais informações sobre os algoritmos NTP, confira RFC 1305 no banco de dados RFC da IETF.  
  
#### <a name="ntp-time-provider"></a>Provedor de tempo do NTP  
O Serviço de Horário do Windows é um pacote de sincronização de tempo completo compatível com uma variedade de dispositivos de hardware e protocolos de tempo. Para habilitar essa compatibilidade, o serviço usa provedores de horário conectáveis. Um provedor de horário é responsável por obter carimbos de data/hora precisos (da rede ou do hardware) ou por fornecer esses carimbos de data/hora a outros computadores na rede.  
  
O provedor de NTP é o provedor de horário padrão incluído no sistema operacional. O provedor de NTP segue os padrões especificados pelo NTP versão 3 para um cliente e servidor e pode interagir com clientes e servidores SNTP para compatibilidade com versões anteriores do Windows 2000 e outros clientes SNTP. O provedor de NTP no Serviço de Horário do Windows consiste nas duas partes abaixo:  
  
-   **Provedor de saída NtpServer.** Esse é um servidor de horário que responde às solicitações de tempo do cliente na rede.  
  
-   **Provedor de entrada NtpClient.** Esse é um cliente de horário que obtém informações de horário de outra fonte (seja um dispositivo de hardware ou um servidor de NTP) e pode retornar amostras de tempo que são úteis para sincronizar o relógio local.  
  
Embora as operações reais desses dois provedores estejam fortemente relacionadas, eles aparecem de maneira independente ao serviço de horário. Começando com o Windows 2000 Server, quando um computador com Windows está conectado a uma rede, ele é configurado como um cliente de NTP. Além disso, os computadores que executam o Serviço de Horário do Windows só tentam sincronizar a hora com um controlador de domínio ou uma fonte de horário especificada manualmente por padrão. Esses são os provedores de horário preferidos porque são fontes de horário seguras disponíveis automaticamente.  
  
#### <a name="ntp-security"></a>Segurança de NTP  

Dentro de uma floresta do AD DS, o Serviço de Horário do Windows depende de recursos de segurança de domínio padrão para impor a autenticação dos dados de tempo. A segurança dos pacotes de NTP que são enviados entre um computador membro do domínio e um controlador de domínio local que está agindo como um servidor de horário é baseada na autenticação de chave compartilhada. O Serviço de Horário do Windows usa a chave da sessão Kerberos do computador para criar assinaturas autenticadas em pacotes de NTP que são enviados pela rede. Os pacotes de NTP não são transmitidos dentro do canal seguro do Logon de Rede. Em vez disso, quando um computador solicita a hora de um controlador de domínio na hierarquia de domínio, o Serviço de Horário do Windows requer que o tempo seja autenticado. O controlador de domínio retorna as informações necessárias na forma de um valor de 64 bits que foi autenticado com a chave da sessão do serviço Logon de Rede. Se o pacote NTP retornado não for assinado com a chave da sessão do computador ou estiver assinado incorretamente, a hora será rejeitada. Todas essas falhas de autenticação são registradas no Log de eventos. Dessa forma, o Serviço de Horário do Windows fornece segurança para dados do NTP em uma floresta do AD DS.  
  
Geralmente, os clientes de tempo do Windows obtêm automaticamente de controladores de domínio no mesmo domínio o tempo preciso para a sincronização. Em uma floresta, os controladores de domínio de um domínio filho sincronizam a hora com controladores de domínio em seus domínios pai. Quando um servidor de horário retorna um pacote de NTP autenticado para um cliente que solicita a hora, o pacote é assinado por meio de uma chave da sessão Kerberos definida por uma conta de confiança entre domínios. A conta de confiança entre domínios é criada quando um novo domínio do AD DS ingressa em uma floresta e o serviço Logon de Rede gerencia a chave da sessão. Dessa forma, o controlador de domínio configurado como confiável no domínio raiz da floresta torna-se a fonte de horário autenticada para todos os controladores de domínio nos domínios pai e filho e, indiretamente, para todos os computadores localizados na árvore do domínio.  
  
O Serviço de Horário do Windows pode ser configurado para funcionar entre florestas, mas é importante observar que essa configuração não é segura. Por exemplo, um servidor NTP pode estar disponível em uma floresta diferente. No entanto, como esse computador está em uma floresta diferente, não há nenhuma chave da sessão Kerberos com a qual assinar e autenticar pacotes de NTP. Para obter uma sincronização de tempo precisa de um computador em uma floresta diferente, o cliente precisa de acesso à rede para esse computador e o serviço de horário deve ser configurado para usar uma fonte de horário específica localizada na outra floresta. Se um cliente for configurado manualmente para acessar o horário de um servidor NTP fora da própria hierarquia de domínio, os pacotes de NTP enviados entre o cliente e o servidor de horário não serão autenticados e, portanto, não serão protegidos. Mesmo com a implementação de relações de confiança de floresta, o Serviço de Horário do Windows não é seguro entre florestas. Embora o canal de segurança Logon de Rede seja o mecanismo de autenticação do Serviço de Horário do Windows, não há suporte para autenticação entre florestas.  
  
#### <a name="hardware-devices-that-are-supported-by-the-windows-time-service"></a>Dispositivos de hardware compatíveis com o Serviço de Horário do Windows  
Os relógios baseados em hardware, como o GPS ou os relógios de rádio, geralmente são usados como dispositivos de relógio de referência altamente precisos. Por padrão, o provedor de horário NTP do Serviço de Horário do Windows não é compatível com a conexão direta de um dispositivo de hardware com um computador, embora seja possível criar um provedor de horário independente baseado em software compatível com esse tipo de conexão. Esse tipo de provedor, em conjunto com o Serviço de Horário do Windows, pode fornecer uma referência de tempo estável e confiável.  
  
Dispositivos de hardware, como um relógio atômico ou um receptor de GPS, fornecem tempo atual preciso seguindo um padrão para obter uma definição precisa do tempo. Os relógios atômicos são extremamente estáveis e não são afetados por fatores como temperatura, pressão ou umidade, mas também são muito caros. Um receptor GPS é muito menos dispendioso para operar e também é um relógio de referência preciso. Os receptores GPS obtêm seu tempo de satélites que obtêm seu tempo de um relógio atômico. Sem o uso de um provedor de horário independente, os servidores de horário do Windows podem obter a hora conectando-se a um servidor de NTP externo, que é conectado a um dispositivo de hardware por meio de um telefone ou da Internet. Organizações como o Observatório Naval dos Estados Unidos fornecem servidores NTP conectados a relógios de referência extremamente confiáveis.  
  
Muitos receptores de GPS e outros dispositivos de tempo podem funcionar como servidores de NTP em uma rede. Você poderá configurar sua floresta do AD DS para sincronizar a hora desses dispositivos de hardware externos somente se eles também estiverem agindo como servidores NTP em sua rede. Para fazer isso, configure o controlador de domínio que funciona como o emulador do PDC (controlador de domínio primário) na raiz da floresta para sincronizar com o servidor NTP fornecido pelo dispositivo GPS. Para fazer isso, confira [Configurar o Serviço de Horário do Windows no emulador do PDC no domínio raiz da floresta](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731191%28v=ws.10%29).  
  
### <a name="simple-network-time-protocol"></a>Protocolo NTP Simples  
O SNTP (Protocolo NTP Simples) é um protocolo de tempo simplificado destinado a servidores e clientes que não exigem o grau de precisão que o NTP fornece. O SNTP, uma versão mais rudimentar do NTP, é o protocolo de tempo primário usado no Windows 2000. Como os formatos de pacote de rede do SNTP e do NTP são idênticos, os dois protocolos são interoperáveis. A principal diferença entre os dois é que o SNTP não tem o gerenciamento de erros e os sistemas de filtragem complexos que o NTP fornece. Para obter mais informações sobre o Protocolo NTP Simples, confira RFC 1769 no banco de dados RFC da IETF.  
  
### <a name="time-protocol-interoperability"></a>Interoperabilidade de protocolo de tempo  
O Serviço de Horário do Windows pode operar em um ambiente misto de computadores que executam o Windows 2000, o Windows XP e o Windows Server 2003, pois o protocolo SNTP usado no Windows 2000 é interoperável com o protocolo NTP no Windows XP e no Windows Server 2003.  
  
O serviço de horário no Windows NT Server 4.0, chamado TimeServ, sincroniza o tempo em uma rede do Windows NT 4.0. O TimeServ é um recurso complementar disponível como parte do *Kit de Recursos do Microsoft Windows NT 4.0* e não fornece o grau de confiabilidade da sincronização de tempo necessária para o Windows Server 2003.  
  
O Serviço de Horário do Windows pode interoperar com computadores que executam o Windows NT 4.0 porque eles podem sincronizar a hora com computadores que executam o Windows 2000 ou o Windows Server 2003; no entanto, um computador que executa o Windows 2000 ou o Windows Server 2003 não descobre automaticamente os servidores de tempo do Windows NT 4.0. Por exemplo, se o domínio estiver configurado para sincronizar a hora usando o método de sincronização baseado em hierarquia de domínio e você quiser que os computadores na hierarquia de domínio sincronizem a hora com um controlador de domínio do Windows NT 4.0, precisará configurar esses computadores manualmente para sincronizar com os controladores de domínio do Windows NT 4.0.  
  
O Windows NT 4.0 usa um mecanismo mais simples do que o usado pelo Serviço de Horário do Windows para a sincronização de tempo. Portanto, para garantir uma sincronização precisa de tempo em sua rede, é recomendável que você atualize todos os controladores de domínio do Windows NT 4.0 para o Windows 2000 ou o Windows Server 2003.  
  
## <a name="windows-time-service-processes-and-interactions"></a>Processos e interações do Serviço de Horário do Windows  

O Serviço de Horário do Windows foi projetado para sincronizar os relógios dos computadores em uma rede. O processo de sincronização do horário da rede, também chamado de convergência de tempo, ocorre em uma rede à medida que cada computador acessa o tempo de um servidor de tempo mais preciso. A convergência de tempo envolve um processo pelo qual um servidor oficial fornece a hora atual para os computadores cliente na forma de pacotes de NTP. As informações fornecidas em um pacote indicam se um ajuste precisa ser feito na hora atual do relógio do computador para que ele seja sincronizado com o servidor mais preciso.  
  
Como parte do processo de convergência de tempo, os membros do domínio tentam sincronizar a hora com qualquer controlador de domínio localizado no mesmo domínio. Se o computador for um controlador de domínio, ele tentará sincronizar com um controlador de domínio mais confiável.  
  
Computadores que executam o Windows XP Home Edition ou computadores que não ingressaram em um domínio não tentam sincronizar com a hierarquia de domínio, mas são configurados por padrão para obter o tempo de time.windows.com.  
  
Para estabelecer um computador executando o Windows Server 2003 como oficial, o computador deve ser configurado para ser uma fonte de horário confiável. Por padrão, o primeiro controlador de domínio instalado em um domínio do Windows Server 2003 é configurado automaticamente para ser uma fonte de horário confiável. Como é o computador oficial do domínio, ele deve ser configurado para sincronizar com uma fonte de horário externa em vez de fazê-lo com a hierarquia de domínio. Além disso, por padrão, todos os outros membros do domínio do Windows Server 2003 são configurados para sincronizar com a hierarquia de domínio.  
  
Depois de estabelecer uma rede do Windows Server 2003, você pode configurar o Serviço de Horário do Windows para usar uma das seguintes opções de sincronização:  
  
-   Sincronização baseada em hierarquia de domínio  
  
-   Uma origem de sincronização especificada manualmente  
  
-   Todos os mecanismos de sincronização disponíveis  
  
-   Nenhuma sincronização.  
  
Cada um desses tipos de sincronização é discutido na seção a seguir.  
  
### <a name="domain-hierarchy-based-synchronization"></a>Sincronização baseada em hierarquia de domínio  

A sincronização baseada em uma hierarquia de domínio usa a hierarquia de domínio do AD DS para encontrar uma fonte confiável com a qual sincronizar a hora. Com base na hierarquia de domínio, o Serviço de Horário do Windows determina a precisão de cada servidor de horário. Em uma floresta do Windows Server 2003, o computador que contém a função de mestre de operações do emulador de PDC (controlador de domínio primário), localizada no domínio raiz da floresta, mantém a posição da melhor fonte de horário, a menos que outra fonte de horário confiável tenha sido configurada. A figura a seguir ilustra um caminho de sincronização de tempo entre computadores em uma hierarquia de domínio.  
  
**Sincronização de tempo em uma hierarquia do AD DS**  
![Horário do Windows](../media/Windows-Time-Service/How-the-Windows-Time-Service-Works/trnt_ntw_adhc.gif)
  
#### <a name="reliable-time-source-configuration"></a>Configuração de fonte de horário confiável  
Um computador configurado para ser uma fonte de horário confiável é identificado como a raiz do serviço de horário. A raiz do serviço de horário é o servidor oficial para o domínio e normalmente é configurada para recuperar o tempo de um servidor NTP externo ou dispositivo de hardware. Um servidor de horário pode ser configurado como uma fonte de horário confiável para otimizar como o tempo é transferido por toda a hierarquia de domínio. Se um controlador de domínio estiver configurado para ser uma fonte de horário confiável, o serviço Logon de Rede anunciará esse controlador de domínio como uma fonte de horário confiável quando ele fizer logon na rede. Quando outros controladores de domínio procuram uma fonte de horário com a qual sincronizar, eles escolhem uma fonte confiável primeiro, se disponível.  
  
#### <a name="time-source-selection"></a>Seleção de fonte de horário  
O processo de seleção da fonte de horário pode criar dois problemas em uma rede:  
  
-   Ciclos de sincronização adicionais.  
  
-   Aumento do volume no tráfego de rede.  
  
Um ciclo na rede de sincronização ocorre quando o tempo permanece consistente entre um grupo de controladores de domínio e o mesmo tempo é compartilhado entre eles continuamente sem uma ressincronização com outra fonte de horário confiável. O algoritmo de seleção de fonte de horário do Serviço de Horário do Windows é projetado para proteger contra esses tipos de problemas.  
  
Um computador usa um dos seguintes métodos para identificar uma fonte de horário com a qual sincronizar:  
  
-   Se o computador não é membro de um domínio, ele deve ser configurado para sincronizar com uma fonte de horário especificada.  
  
-   Se o computador é um servidor membro ou uma estação de trabalho em um domínio, por padrão, ele segue a hierarquia do AD DS e sincroniza seu horário com um controlador de domínio em seu domínio local que esteja atualmente executando o Serviço de Horário do Windows.  
  
Se o computador é um controlador de domínio, ele faz até seis consultas para localizar outro controlador de domínio com o qual sincronizar. Cada consulta é projetada para identificar uma fonte de horário com determinados atributos, como um tipo de controlador de domínio, uma localização específica e se é uma fonte de horário confiável ou não. A fonte de horário também deve aderir às seguintes restrições:  
  
-   Uma fonte de horário confiável só pode sincronizar com um controlador de domínio no domínio pai.  
  
-   Um emulador de PDC pode sincronizar com uma fonte de horário confiável em um domínio próprio ou em qualquer controlador de domínio no domínio pai.  
  
Se o controlador de domínio não for capaz de sincronizar com o tipo de controlador de domínio que está consultando, a consulta não será feita. O controlador de domínio sabe de qual tipo de computador ele pode obter tempo antes de fazer a consulta. Por exemplo, um emulador de PDC local não tenta consultar os números três ou seis porque um controlador de domínio não tenta sincronizar-se com ele mesmo.  
  
A tabela a seguir lista as consultas que um controlador de domínio faz para localizar uma fonte de horário e a ordem na qual as consultas são feitas.  
  
**Consultas de fonte de horário do controlador de domínio**  
  
|Número da consulta|Controlador de domínio|Local|Confiabilidade da fonte de horário|  
|----------------|---------------------|------------|------------------------------|  
|1|Controlador de domínio pai|No local|Prefere uma fonte de horário confiável, mas poderá sincronizar com uma fonte de horário não confiável se for o que está disponível.|  
|2|Controlador de domínio local|No local|Só sincroniza com uma fonte de horário confiável.|  
|3|Emulador de PDC local|No local|Não se aplica.<p>Um controlador de domínio não tenta sincronizar com ele mesmo.|  
|4|Controlador de domínio pai|Fora do site|Prefere uma fonte de horário confiável, mas poderá sincronizar com uma fonte de horário não confiável se for o que está disponível.|  
|5|Controlador de domínio local|Fora do site|Só sincroniza com uma fonte de horário confiável.|  
|6|Emulador de PDC local|Fora do site|Não se aplica.<p>Um controlador de domínio não tenta sincronizar com ele mesmo.| 
  
**Observação**  
  
-   Um computador nunca sincroniza com ele mesmo. Se o computador que está tentando sincronizar for o emulador de PDC local, ele não tentará as consultas 3 ou 6.  
  
Cada consulta retorna uma lista de controladores de domínio que podem ser usados como uma fonte de horário. O Horário do Windows atribui a cada controlador de domínio uma pontuação que é consultada com base na confiabilidade e na localização do controlador de domínio. A tabela a seguir lista as pontuações atribuídas pelo Horário do Windows a cada tipo de controlador de domínio.  
  
**Determinação da Pontuação**  
  
|Status do controlador de domínio|Pontuação|  
|----------------------------|---------|  
|Controlador de domínio localizado no mesmo site|8|  
|Controlador de domínio marcado como uma fonte de horário confiável|4|  
|Controlador de domínio localizado no domínio pai|2|  
|Controlador de domínio que é um emulador de PDC|1|  
  
Quando o Serviço de Horário do Windows determina que identificou o controlador de domínio com a melhor pontuação possível, ele para de fazer consultas. As pontuações atribuídas pelo serviço de horário são cumulativas, o que significa que um emulador de PDC localizado no mesmo site recebe uma pontuação igual a nove.  
  
Se a raiz do serviço de horário não estiver configurada para sincronizar com uma fonte externa, o relógio de hardware interno do computador controlará o tempo.  
  
### <a name="manually-specified-synchronization"></a>Sincronização especificada manualmente  
A sincronização especificada manualmente permite designar um par ou uma lista de pares dos quais um computador obtém o tempo. Se o computador não é membro de um domínio, ele deve ser configurado manualmente para sincronizar com uma fonte de horário especificada. Um computador que é membro de um domínio é configurado por padrão para sincronizar na hierarquia de domínio; a sincronização especificada manualmente é mais útil para a raiz de floresta do domínio ou para computadores que não fazem parte de um domínio. A especificação manual de um servidor NTP externo para sincronizar com o computador oficial do seu domínio fornece um tempo confiável. No entanto, configurar o computador oficial do seu domínio para sincronizar com um relógio de hardware é, na verdade, uma solução melhor para fornecer o tempo mais preciso e seguro para o seu domínio.  
  
As fontes de horários especificadas manualmente não são autenticadas, a menos que um provedor de horário específico seja gravado para elas e, portanto, seja vulnerável a invasores. Além disso, se um computador sincronizar com uma fonte especificada manualmente em vez de seu controlador de domínio de autenticação, os dois computadores poderão ficar fora de sincronização, causando a falha da autenticação Kerberos. Isso pode causar falha em outras ações que exigem autenticação de rede, como impressão ou compartilhamento de arquivos. Se apenas a raiz da floresta estiver configurada para sincronizar com uma fonte externa, todos os outros computadores da floresta permanecerão sincronizados entre si, dificultando os ataques de repetição.  
  
### <a name="all-available-synchronization-mechanisms"></a>Todos os mecanismos de sincronização disponíveis  

A opção "todos os mecanismos de sincronização disponíveis" é o método de sincronização mais valioso para usuários em uma rede. Esse método permite a sincronização com a hierarquia do domínio e também pode fornecer uma fonte de horário alternativa, caso a hierarquia do domínio não esteja disponível, dependendo da configuração. Se o cliente não puder sincronizar a hora com a hierarquia do domínio, a fonte de horário automaticamente voltará para a fonte de horário especificada pela configuração **NtpServer**. Esse método de sincronização tem maior probabilidade de fornecer um tempo preciso para os clientes.  

### <a name="stopping-time-synchronization"></a>Parando a sincronização de tempo  
Há algumas situações em que você desejará impedir que um computador sincronize seu tempo. Por exemplo, se um computador tentar sincronizar usando uma fonte de horário da Internet ou outro site por meio de uma WAN de uma conexão discada, isso poderá gerar altos encargos de telefone. Ao desabilitar a sincronização nesse computador, você impedirá que o computador tente acessar uma fonte de horário por meio de uma conexão discada.  
  
Você também pode desabilitar a sincronização para evitar a geração de erros no log de eventos. Cada vez que um computador tenta sincronizar com uma fonte de horário que não está disponível, ele gera um erro no Log de Eventos. Se uma fonte de horário for retirada da rede para manutenção agendada e você não pretender reconfigurar o cliente para sincronizar com outra fonte, você poderá desabilitar a sincronização no cliente para impedi-lo de tentar a sincronização enquanto o servidor de horário não estiver disponível.  
  
É útil desabilitar a sincronização no computador designado como a raiz da rede de sincronização. Isso indica que o computador raiz confia em seu relógio local. Se a raiz da hierarquia de sincronização não estiver definida como **NoSync** e se não for possível sincronizar com outra fonte de horário, os clientes não aceitarão o pacote enviado por esse computador, pois seu tempo não é confiável.
  
Os únicos servidores de tempo que são confiáveis para os clientes, mesmo que não tenham sido sincronizados com outra fonte de horário, são aqueles identificados pelo cliente como servidores de horário confiáveis.  
  
### <a name="disabling-the-windows-time-service"></a>Desabilitando o Serviço de Horário do Windows  
O Serviço de Horário do Windows (W32Time) pode ser completamente desabilitado. Se você optar por implementar um produto de sincronização de tempo de terceiros que usa o NTP, será necessário desabilitar o Serviço de Horário do Windows. Isso ocorre porque todos os servidores NTP precisam de acesso à porta 123 do protocolo UDP e, enquanto o Serviço de Horário do Windows estiver em execução no sistema operacional Windows Server 2003, a porta 123 permanecerá reservada pelo Horário do Windows.  
  
## <a name="network-ports-used-by-windows-time-service"></a>Portas de rede usadas pelo Serviço de Horário do Windows  
O Serviço de Horário do Windows se comunica em uma rede para identificar fontes de horário confiáveis, obter informações de tempo e fornecer informações de tempo a outros computadores. Ele executa essa comunicação conforme definido pelas RFCs de NTP e SNTP.  
  
**Atribuições de porta para o Serviço de Horário do Windows**  
  
|Nome do serviço|UDP|TCP|  
|----------------|-------|-------|  
|NTP|123|N/D|  
|SNTP|123|N/D|  
  
## <a name="see-also"></a>Consulte Também  
[Referência técnica do Serviço de Horário do Windows](windows-time-service-tech-ref.md)
[Ferramentas e configurações do Serviço de Horário do Windows](Windows-Time-Service-Tools-and-Settings.md)
[Artigo 902229 da Base de Dados de Conhecimento Microsoft](https://go.microsoft.com/fwlink/?LinkId=186066)