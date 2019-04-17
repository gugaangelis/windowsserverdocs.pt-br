---
ms.assetid: d1953097-63ea-4a0e-b860-2f3b7c175c41
title: Como funciona o serviço de tempo do Windows
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: networking
ms.openlocfilehash: c9ab52229c2a16d6ae6b0b2733c52e53a25cfa44
ms.sourcegitcommit: fb4e2ace2e0a29e0f6b028f1cb945cab6aa6ee2c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/05/2018
---
# <a name="how-the-windows-time-service-works"></a>Como funciona o serviço de tempo do Windows

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

**Nesta seção**  
  
-   [Arquitetura de serviço de tempo do Windows](#w2k3tr_times_how_rrfo)  
  
-   [Protocolos de tempo de serviço de tempo do Windows](#w2k3tr_times_how_ekoc)  
  
-   [Serviço processos e interações de tempo do Windows](#w2k3tr_times_how_izcr)  
  
-   [Portas de rede usadas pelo serviço de tempo do Windows](#w2k3tr_times_how_ydum)  
  
> [!NOTE]  
> Este tópico explica apenas como funciona o serviço de tempo do Windows (W32Time). Para obter informações sobre como configurar o serviço de tempo do Windows, consulte a lista de tópicos na seção [onde encontrar informações de configuração serviço de tempo de Windows](https://technet.microsoft.com/library/cc773061.aspx).  
  
> [!NOTE]  
> No Windows Server 2003 e Microsoft Windows 2000 Server, o serviço de diretório é nomeado serviço de diretório do Active Directory. No Windows Server 2008 e versões posteriores, o serviço de diretório é nomeado os serviços de domínio do Active Directory (AD DS). O restante deste tópico refere-se no AD DS, mas as informações também se aplicam ao Active Directory.  
  
Embora o serviço de tempo do Windows não é uma implementação exata do protocolo de tempo de rede (NTP), ele usa o pacote complexo de algoritmos que é definido nas especificações NTP para garantir que os relógios em computadores em uma rede sejam mais precisos possível. Idealmente, todos os relógios do computador em um domínio do AD DS são sincronizados com o tempo de um computador autoritativo. Muitos fatores podem afetar a sincronização de tempo em uma rede. Geralmente, os seguintes fatores afetam a precisão de sincronização no AD DS:  
  
-   Condições de rede  
  
-   A precisão do relógio de hardware do computador  
  
-   A quantidade de recursos de CPU e rede disponíveis para o serviço de tempo do Windows  
  
> [!IMPORTANT]  
> Antes do Windows Server 2016, o serviço W32Time não foi projetado para atender às necessidades do aplicativo de detecção de hora.  No entanto, as atualizações para Windows Server 2016 agora permitem que você implemente uma solução para 1 MS precisão em seu domínio.  Consulte [Windows 2016 precisão tempo](accurate-time.md) e [limite de suporte para configurar o serviço de tempo do Windows para ambientes de alta precisão](https://go.microsoft.com/fwlink/?LinkID=179459) para obter mais informações.  
  
Computadores que sincronizam menos tempo com frequência ou não tenha ingressados em um domínio são configurados, por padrão, para sincronizar com time.windows.com. Portanto, é impossível garantir a precisão de tempo em computadores que têm intermitente ou nenhuma conexão de rede.  
  
Uma floresta do AD DS tem uma hierarquia de sincronização de tempo predeterminado. O serviço de tempo do Windows sincroniza o tempo entre computadores dentro da hierarquia, com os mais precisos relógios de referência na parte superior. Se mais de uma fonte de tempo é configurada em um computador, o tempo do Windows usa algoritmos NTP para selecionar a melhor fonte de tempo das fontes configuradas com base na capacidade do computador para sincronizar com essa fonte de tempo. O serviço de tempo do Windows não oferece suporte a sincronização de rede de transmissão ou pares multicast. Para obter mais informações sobre esses recursos NTP, consulte RFC 1305 no banco de dados RFC IETF.  
  
Cada computador que esteja executando o serviço de tempo do Windows usa o serviço para manter a hora mais precisa. Os computadores que são membros de um domínio atuar como um cliente de tempo por padrão, portanto, na maioria dos casos não é necessário configurar o serviço de tempo do Windows. No entanto, o serviço de tempo do Windows pode ser configurado para solicitar a hora de uma fonte de tempo de referência designado e também pode fornecer o tempo para os clientes.
  
O grau ao qual a hora do computador é precisa é chamado de uma camada. A fonte mais precisa de tempo em uma rede (como um relógio de hardware) ocupa o nível mais baixo de camada ou camada um. Essa fonte de tempo precisas é chamado um relógio de referência. Um servidor NTP que adquire seu tempo diretamente de um relógio de referência ocupa uma camada é um nível mais alto do que o relógio de referência. Recursos que adquire o tempo do servidor NTP são duas etapas longe o relógio de referência e, portanto, ocupam uma camada que tenha duas maior do que a fonte de tempo mais precisa e assim por diante. Como número de camada do computador aumenta, a hora no relógio seu sistema pode se tornar menos preciso. Portanto, o nível de camada de qualquer computador que é um indicador de proximidade nesse computador é sincronizado com a fonte de tempo mais precisa.  
  
Quando o Gerenciador de W32Time recebe amostras de tempo, ele usa algoritmos especiais em NTP para determinar quais as amostras de tempo são as mais apropriadas para uso. O serviço de tempo também usa outro conjunto de algoritmos para determinar qual das fontes de tempo configurado é o mais preciso. Quando o serviço de tempo determinou qual amostra de tempo é melhor, com base em critérios acima, ajusta a velocidade do relógio local para permitir a convergência no sentido horário correto. Se a diferença de tempo entre o relógio do local e a amostra de tempo precisas selecionado (também chamada o tempo inclinação) for muito grande para corrigir, ajustando a velocidade do relógio local, o serviço de tempo define o relógio local a hora corretas. Esse ajuste da taxa de relógio ou alteração de hora do relógio direta é conhecido como disciplina do relógio.  
  
## <a name="w2k3tr_times_how_rrfo"></a>Arquitetura de serviço de tempo do Windows  
O serviço de tempo do Windows consiste nos seguintes componentes:  
  
-   Gerenciador de controle de serviço  
  
-   Gerenciador de serviço de tempo do Windows  
  
-   Relógio disciplina  
  
-   Provedores de tempo  
  
A figura a seguir mostra a arquitetura do serviço de tempo do Windows.  
  
**Arquitetura de serviço de tempo do Windows**  
  
![Tempo do Windows](media/How-the-Windows-Time-Service-Works/trnt_sec_arcc.gif)  
  
O Gerenciador de controle de serviço é responsável por iniciando e interrompendo o serviço de tempo do Windows. O Gerenciador de serviço de tempo do Windows é responsável por iniciar a ação dos provedores de tempo de NTP incluídos com o sistema operacional. O Gerenciador de serviço de tempo do Windows controla todas as funções do serviço de tempo do Windows e a unir de todas as amostras de tempo. Além o fornecimento de informações sobre o estado atual do sistema, como a fonte de tempo atual ou a última vez que o relógio do sistema foi atualizado, o Gerenciador de serviço de tempo do Windows também é responsável pela criação de eventos no log de eventos.  
  
O processo de sincronização de tempo envolve as seguintes etapas:  
  
-   Provedores de entrada solicitam e recebem amostras de tempo de fontes de tempo NTP configurados.  
  
-   Estes exemplos de tempo são passados para o Windows serviço Gerenciador de tempo que todos os exemplos de coleta e transmite para o subcomponente de disciplina do relógio.  
  
-   O subcomponente de disciplina relógio aplica os algoritmos NTP resultando na seleção da melhor amostra de tempo.  
  
-   O subcomponente de disciplina relógio ajusta a hora do relógio do sistema para o tempo mais preciso ajustar a taxa de relógio ou alterando diretamente o tempo.  
  
Se um computador tiver sido designado como um servidor de horário, ele pode enviar o tempo em qualquer computador solicitando a sincronização de tempo em qualquer ponto neste processo.  
  
## <a name="w2k3tr_times_how_ekoc"></a>Protocolos de tempo de serviço de tempo do Windows  

Protocolos de tempo determinam o grau de correspondência entre dois computadores relógios são sincronizados. Um protocolo de tempo é responsável por determinar as melhores informações de horário e convergindo os relógios para garantir que um tempo consistente é mantido em sistemas separados.  
  
O serviço de tempo do Windows usa o protocolo de tempo de rede (NTP) para ajudar a sincronizar o tempo em uma rede. NTP é um protocolo de tempo de Internet que inclui os algoritmos de disciplina necessários para sincronizar relógios. NTP é um protocolo de tempo mais preciso do que a simples SNTP Network Time Protocol () que é usado em algumas versões do Windows; No entanto W32Time continua a dar suporte a SNTP para habilitar a compatibilidade com computadores que executam serviços de tempo com base no SNTP, como o Windows 2000.  
  
### <a name="network-time-protocol"></a>Protocolo de tempo de rede  
Protocolo NTP (Network Time) é o padrão usado pelo serviço de tempo do Windows no sistema operacional de protocolo de sincronização de tempo. NTP é um protocolo de tempo tolerantes a falhas e altamente escalonáveis e o protocolo usado com mais frequência de sincronização relógios do computador usando uma referência de tempo designado.  
  
Sincronização de tempo NTP ocorre durante um período de tempo e envolve a transferência de NTP pacotes em uma rede. Pacotes NTP contêm carimbos de data / hora que incluem uma amostra de tempo de cliente e servidor participação na sincronização de tempo.  
  
NTP depende de um relógio de referência para definir o horário mais preciso para ser usado e sincroniza todos os relógios em uma rede para esse relógio de referência. NTP usa Tempo Universal Coordenado (UTC) como o padrão universal para hora atual. UTC é independente de fusos horários e permite NTP a ser usado em qualquer lugar do mundo independentemente das configurações de fuso horário.  
  
#### <a name="ntp-algorithms"></a>Algoritmos NTP  
NTP inclui dois algoritmos, um algoritmo de filtragem de relógio e um algoritmo de seleção de relógio, para ajudar o serviço de tempo do Windows em determinar o melhor exemplo de tempo. O algoritmo de filtragem de relógio foi projetado para conferir amostras de tempo que são recebidas de fontes de tempo consultado e determinam as melhor amostras de tempo de cada fonte. O algoritmo de seleção de relógio, em seguida, determina o servidor de horário mais preciso na rede. Essas informações é então passadas para o algoritmo de disciplina relógio, que usa as informações coletadas para corrigir o relógio do computador local enquanto compensar erros devido a imprecisão de relógio de latência e o computador de rede.  
  
Os algoritmos NTP são mais precisos em condições de cargas de rede e o servidor de luz para moderada. Assim como com qualquer algoritmo que leva tempo de transporte de rede em consideração, algoritmos NTP talvez desempenho insatisfatório em condições de congestionamento extrema de rede. Para obter mais informações sobre os algoritmos NTP, consulte RFC 1305 no banco de dados RFC IETF.  
  
#### <a name="ntp-time-provider"></a>Provedor de tempo NTP  
O serviço de tempo do Windows é um pacote de sincronização de hora completa que pode dar suporte a uma variedade de dispositivos de hardware e protocolos de tempo. Para habilitar esse suporte, o serviço usa provedores de tempo conectável. Um provedor de tempo é responsável por qualquer obter precisas carimbos de data (da rede ou de hardware) ou para fornecer os carimbos de data / hora para outros computadores pela rede.  
  
O provedor NTP é o provedor de hora padrão incluído com o sistema operacional. O provedor NTP siga os padrões especificados pelo NTP versão 3 para um cliente e servidor e pode interagir com clientes SNTP e servidores para compatibilidade com o Windows 2000 e outros clientes SNTP. O provedor NTP no serviço de tempo do Windows consiste em duas partes a seguir:  
  
-   **Provedor de saída NtpServer.** Isso é um servidor de horário que responde às solicitações de tempo de cliente na rede.  
  
-   **Provedor de entrada NtpClient.** Esse é um cliente de tempo que obtém informações de tempo de outra fonte, um dispositivo de hardware ou um servidor NTP e pode retornar amostras de tempo que são úteis para sincronizar o relógio do local.  
  
Embora as operações reais desses dois provedores são estreitamente relacionadas, elas aparecem independentes para o serviço de tempo. A partir do Windows 2000 Server, quando um computador Windows está conectado a uma rede, ele é configurado como um cliente NTP. Além disso, computadores que executam o Windows tempo serviço somente tentar sincronizar tempo com um controlador de domínio ou uma fonte de tempo especificado manualmente por padrão. Estes são os provedores de tempo preferencial porque estão automaticamente disponíveis e seguras fontes de tempo.  
  
#### <a name="ntp-security"></a>Segurança NTP  

Dentro de uma floresta do AD DS, o serviço de tempo do Windows depende de recursos de segurança de domínio padrão para impor a autenticação de dados de tempo. A segurança dos pacotes NTP que são enviados entre um computador membro do domínio e um controlador de domínio local que está atuando como um servidor de horário baseia-se a autenticação de chave compartilhada. O serviço de tempo do Windows usa a chave de sessão Kerberos do computador para criar assinaturas autenticadas em pacotes NTP que são enviados pela rede. Pacotes NTP não são transmitidas dentro do canal seguro de Logon de rede. Em vez disso, quando um computador solicita o tempo de um controlador de domínio na hierarquia de domínio, o serviço de tempo do Windows requer que o tempo seja autenticado. O controlador de domínio, em seguida, retorna as informações necessárias na forma de um valor de 64 bits que foi autenticado com a chave de sessão do serviço de Logon de rede. Se o pacote NTP retornado não está assinado com uma chave de sessão do computador ou está assinado incorretamente, o tempo será rejeitado. Todas essas falhas de autenticação são registradas no Log de eventos. Dessa forma, o serviço de tempo do Windows fornece segurança de dados NTP em uma floresta do AD DS.  
  
Em geral, os clientes de tempo do Windows automaticamente obtêm tempo preciso para sincronização de controladores de domínio no mesmo domínio. Em uma floresta, os controladores de domínio de um domínio filho sincronizam tempo com controladores de domínio em seus domínios pai. Quando um servidor de horário retorna um pacote NTP autenticado para um cliente que solicita o tempo, o pacote é assinado por meio de uma chave de sessão Kerberos definida por uma conta de confiança entre domínios. A conta de confiança entre domínios é criada quando um novo domínio do AD DS ingressar em uma floresta, e o serviço de Logon de rede gerencia a chave de sessão. Dessa forma, o controlador de domínio que esteja configurado como confiável no domínio raiz da floresta torna-se a fonte de tempo autenticado para todos os controladores de domínio em domínios pai e filho e indiretamente para todos os computadores localizados na árvore de domínio.  
  
O serviço de tempo do Windows pode ser configurado para funcionar entre florestas, mas é importante observar que essa configuração não é segura. Por exemplo, um servidor NTP pode estar disponível em uma floresta diferente. No entanto, porque o computador está em uma floresta diferente, não há nenhuma chave de sessão Kerberos com o qual você deseja assinar e autenticar NTP pacotes. Para obter a sincronização de tempo precisas de um computador em uma floresta diferente, o cliente precisa de acesso de rede ao computador e o serviço de tempo deve ser configurado para usar uma fonte de tempo específico localizada em outra floresta. Se um cliente for configurado manualmente para tempo de acesso de um servidor de NTP fora da sua própria hierarquia de domínio, os pacotes NTP enviados entre o cliente e o servidor de horário não são autenticados e, portanto, não são seguros. Mesmo com a implementação de relações de confiança de floresta, o serviço de tempo do Windows não é seguro entre florestas. Embora o canal seguro de Logon de rede é o mecanismo de autenticação para o serviço de tempo do Windows, não há suporte para a autenticação entre florestas.  
  
#### <a name="hardware-devices-that-are-supported-by-the-windows-time-service"></a>Dispositivos de hardware que são compatíveis com o serviço de tempo do Windows  
Relógios baseada em hardware, como GPS ou relógios de rádio geralmente são usados como dispositivos de relógio de referência de alta precisão. Por padrão, o provedor de tempo NTP de serviço de tempo do Windows não oferece suporte a conexão direta de um dispositivo de hardware para um computador, embora seja possível criar um provedor de tempo independente com base no software que dá suporte a esse tipo de conexão. Esse tipo de provedor, em conjunto com o serviço de tempo do Windows, pode fornecer uma referência de tempo confiável e estável.  
  
Dispositivos de hardware, como um relógio de cesium ou um receptor de sistema de posicionamento Global (GPS), oferecem um tempo atual preciso seguindo um padrão para obter uma definição de tempo precisa. Relógios cesium são extremamente estáveis e não são afetados por fatores como temperatura, pressão ou umidade, mas também são muito caros. Um receptor GPS é muito mais barato operar e também é um relógio de referência precisas. Receptores GPS obtêm seus horários de satélites que obtêm seus horários de um relógio de cesium. Sem o uso de um provedor de tempo independentes, servidores de tempo do Windows podem adquirir o tempo conectando a um servidor NTP externo, que é conectado a um dispositivo de hardware por meio de um telefone ou na Internet. As organizações, como o Observatório Naval Estados Unidos fornecem servidores NTP que estão conectados a relógios de referência extremamente confiável.  
  
Muitos receptores GPS e outros dispositivos de tempo podem funcionar como servidores NTP em uma rede. Você pode configurar seu floresta do AD DS para sincronizar o tempo desde esses dispositivos de hardware externo somente se eles também são atuando como servidores NTP em sua rede. Para fazer isso, configure o controlador de domínio funcionando como controlador de domínio primário emulador (PDC) em sua raiz da floresta para sincronizar com o servidor NTP fornecido pelo dispositivo GPS. Para fazer isso, consulte [configurar o serviço de tempo do Windows no emulador do PDC no domínio raiz da floresta](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731191%28v=ws.10%29).  
  
### <a name="simple-network-time-protocol"></a>Protocolo de tempo de rede simples  
A rede SNTP Simple Time Protocol () é um protocolo de tempo simplificada que se destina a clientes que não exigem o grau de precisão NTP fornece e servidores. SNTP, uma versão mais rudimentar de NTP, é o protocolo de tempo principal que é usado no Windows 2000. Como os formatos de pacote de rede do SNTP e NTP são idênticos, os dois protocolos são interoperáveis. A principal diferença entre os dois é que SNTP não tem o gerenciamento de erro e sistemas de filtragem complexos que NTP fornece. Para obter mais informações sobre o protocolo Simple de tempo de rede, consulte RFC 1769 no banco de dados RFC IETF.  
  
### <a name="time-protocol-interoperability"></a>Interoperabilidade do protocolo de tempo  
O serviço de tempo do Windows pode operar em um ambiente misto de computadores que executam o Windows 2000, Windows XP e Windows Server 2003, como o protocolo SNTP usado no Windows 2000 é interoperável com o protocolo NTP no Windows XP e Windows Server 2003.  
  
O serviço de tempo no Windows NT Server 4.0, chamado TimeServ, sincroniza o tempo em uma rede do Windows NT 4.0. TimeServ é um recurso de complemento disponível como parte do *Microsoft Windows NT 4.0 Resource Kit* e não fornece o grau de confiabilidade de sincronização de tempo que é exigida pelo Windows Server 2003.  
  
O serviço de tempo do Windows pode interoperar com computadores que executam o Windows NT 4.0 porque eles podem ser sincronizadas tempo com computadores que executam o Windows 2000 ou Windows Server 2003; No entanto, um computador executando o Windows 2000 ou Windows Server 2003 não detecta automaticamente servidores de tempo do Windows NT 4.0. Por exemplo, se o domínio está configurado para sincronizar tempo usando o domínio baseada na hierarquia de método de sincronização e você deseja que os computadores na hierarquia de domínios para sincronizar o tempo com um controlador de domínio do Windows NT 4.0, você precisa configurar esses computadores manualmente para sincronizar com os controladores de domínio do Windows NT 4.0.  
  
Windows NT 4.0 usa um mecanismo mais simples para a sincronização de tempo que o serviço de tempo do Windows usa. Portanto, para garantir a sincronização de tempo precisas em sua rede, é recomendável que você atualize os controladores de domínio do Windows NT 4.0 para o Windows 2000 ou Windows Server 2003.  
  
## <a name="w2k3tr_times_how_izcr"></a>Serviço processos e interações de tempo do Windows  

O serviço de tempo do Windows foi projetado para sincronizar os relógios dos computadores em uma rede. O processo de sincronização de tempo de rede, também chamado de convergência de tempo, ocorre em uma rede como cada vez que acessos ao computador de um servidor de tempo mais preciso. Convergência de tempo envolve um processo pelo qual um servidor de autorização fornece a hora atual em computadores cliente na forma de pacotes NTP. As informações fornecidas em um pacote indicam se um ajuste precisa ser feita a hora do relógio atual do computador para que ele seja sincronizado com o servidor mais preciso.  
  
Como parte do processo de convergência de tempo, os membros do domínio tentam sincronizar tempo com qualquer controlador de domínio localizado no mesmo domínio. Se o computador for um controlador de domínio, ele tenta sincronizar com um controlador de domínio mais autoritativo.  
  
Computadores que executam o Windows XP Home Edition ou computadores que não tenham ingressados em um domínio não tentar sincronizar com a hierarquia de domínio, mas são configuradas por padrão para obter o tempo do time.windows.com.  
  
Para estabelecer um computador executando o Windows Server 2003 como autoritativo, o computador deve ser configurado para ser uma fonte de tempo confiável. Por padrão, o primeiro controlador de domínio que está instalado em um domínio do Windows Server 2003 automaticamente está configurado para ser uma fonte de tempo confiável. Como é o computador autoritativo para o domínio, ele deve ser configurado para sincronizar com uma fonte de horário externo, em vez com a hierarquia de domínio. Também por padrão, todos os outros membros do domínio do Windows Server 2003 são configurados para sincronizar com a hierarquia de domínio.  
  
Depois que você tiver estabelecido uma rede do Windows Server 2003, você pode configurar o serviço de tempo do Windows para usar uma das seguintes opções de sincronização:  
  
-   Sincronização de baseada na hierarquia de domínio  
  
-   Uma fonte de sincronização especificada manualmente  
  
-   Todos os mecanismos de sincronização disponíveis  
  
-   Nenhuma sincronização.  
  
Cada um desses tipos de sincronização é discutido na seção a seguir.  
  
### <a name="domain-hierarchy-based-synchronization"></a>Sincronização de baseada na hierarquia de domínio  

Sincronização com base em uma hierarquia de domínios usa a hierarquia de domínio do AD DS para encontrar uma fonte confiável com a ser sincronizado. Com base na hierarquia de domínio, o serviço de tempo do Windows determina a precisão de cada servidor de horário. Em uma floresta do Windows Server 2003, o computador que contém a função domínio primário (PDC) do controlador emulador operações mestre, localizada no domínio raiz da floresta, mantém a posição da fonte de tempo recomendada, a menos que outra fonte de tempo confiável tiver sido configurado. A figura a seguir ilustra um caminho de tempo de sincronização entre computadores em uma hierarquia de domínio.  
  
**Sincronização de tempo em uma hierarquia do AD DS**  
  
![Tempo do Windows](media/How-the-Windows-Time-Service-Works/trnt_ntw_adhc.gif)  
  
#### <a name="reliable-time-source-configuration"></a>Configuração da fonte de tempo confiável  
Um computador que esteja configurado para ser uma fonte de tempo confiável é identificado como a raiz do serviço de tempo. A raiz do serviço de tempo é o servidor autoritativo para o domínio e normalmente é configurada para recuperar o tempo de um servidor NTP externo ou um dispositivo de hardware. Um servidor de horário pode ser configurado como uma fonte de tempo confiável para otimizar a como o tempo é transferido em toda a hierarquia de domínio. Se um controlador de domínio está configurado para ser uma fonte de tempo confiável, o serviço de Logon de rede anuncia esse controlador de domínio como uma fonte de tempo confiável quando ele fizer logon na rede. Quando outros controladores de domínio procurarem uma fonte de tempo sincronizar com, eles escolher uma fonte confiável primeiro se houver um disponível.  
  
#### <a name="time-source-selection"></a>Seleção de fonte de tempo  
O processo de seleção de fonte de tempo pode criar dois problemas em uma rede:  
  
-   Ciclos de sincronização adicionais.  
  
-   Volume aumento no tráfego de rede.  
  
Um ciclo na rede sincronização ocorre quando o tempo permanece consistente entre um grupo de controladores de domínio e o mesmo tempo é compartilhado entre eles continuamente sem uma nova sincronização com outra fonte de tempo confiável. Algoritmo de seleção de origem de tempo do serviço de tempo do Windows foi projetado para se proteger contra esses tipos de problemas.  
  
Um computador usa um dos seguintes métodos para identificar uma fonte de tempo para sincronizar com:  
  
-   Se o computador não for membro de um domínio, ele deve ser configurado para sincronizar com uma fonte de tempo especificado.  
  
-   Se o computador for um servidor membro ou estação de trabalho em um domínio, por padrão, ele segue a hierarquia do AD DS e sincroniza seu tempo com um controlador de domínio no domínio local que está executando o serviço de tempo do Windows.  
  
Se o computador for um controlador de domínio, ele torna até seis consultas para localizar outro controlador de domínio para sincronizar com. Cada consulta foi projetada para identificar uma fonte de tempo com determinados atributos, como um tipo de controlador de domínio, um local específico, e é uma fonte de tempo confiável ou não. A fonte de tempo também deve cumprir as restrições a seguir:  
  
-   Uma fonte de tempo confiável pode sincronizar apenas com um controlador de domínio no domínio pai.  
  
-   Um emulador do PDC pode sincronizar com uma fonte de tempo confiável em seu próprio domínio ou qualquer controlador de domínio no domínio pai.  
  
Se o controlador de domínio não for capaz de sincronizar com o tipo de controlador de domínio que está consultando, não será feita a consulta. O controlador de domínio sabe qual tipo de computador, ele pode obter o tempo desde antes que ele faz com que a consulta. Por exemplo, um emulador do PDC local não tenta números de consulta três ou seis porque um controlador de domínio não tenta sincronizar com em si.  
  
A tabela a seguir lista as consultas que faz com um controlador de domínio para encontrar uma fonte de tempo e a ordem em que as consultas são feitas.  
  
**Consulta de fonte de tempo de controlador de domínio**  
  
|Número de consulta|Controlador de domínio|Localização|Confiabilidade da fonte de tempo|  
|----------------|---------------------|------------|------------------------------|  
|1|Controlador de domínio pai|No site|Prefere um confiável fonte de tempo, mas ele pode sincronizar com uma fonte de tempo não confiável se isso é tudo o que está disponível.|  
|2|Controlador de domínio local|No site|Sincroniza somente com uma fonte de tempo confiável.|  
|3|Emulador do PDC local|No site|Não se aplica.<br /><br />Um controlador de domínio não tentar sincronizar com em si.|  
|4|Controlador de domínio pai|Fora do site|Prefere um confiável fonte de tempo, mas ele pode sincronizar com uma fonte de tempo não confiável se isso é tudo o que está disponível.|  
|5|Controlador de domínio local|Fora do site|Sincroniza somente com uma fonte de tempo confiável.|  
|6|Emulador do PDC local|Fora do site|Não se aplica.<br /><br />Um controlador de domínio não tentar sincronizar com em si.|  
  
**Observação**  
  
-   Um computador nunca sincroniza com em si. Se o computador a tentativa de sincronização for o emulador do PDC local, não tente consultas 3 ou 6.  
  
Cada consulta retorna uma lista de controladores de domínio que pode ser usado como uma fonte de tempo. Tempo do Windows atribui cada controlador de domínio é consultada uma pontuação de acordo com a confiabilidade e o local do controlador de domínio. A tabela a seguir lista as pontuações atribuídas pelo tempo do Windows para cada tipo de controlador de domínio.  
  
**Determinação de pontuação**  
  
|Status de controlador de domínio|Pontuação|  
|----------------------------|---------|  
|Controlador de domínio localizado no mesmo site|8|  
|Controlador de domínio marcados como uma fonte de tempo confiável|4|  
|Controlador de domínio localizado no domínio pai|2|  
|Controlador de domínio que é um emulador do PDC|1|  
  
Quando o serviço de tempo do Windows determina que identificou o controlador de domínio com a melhor pontuação possível, não há mais consultas são feitas. As pontuações atribuídas pelo serviço de tempo são cumulativas, o que significa que um emulador do PDC localizado no mesmo site recebe uma pontuação de nove.  
  
Se a raiz do serviço de tempo não estiver configurada para sincronizar com uma fonte externa, o relógio de hardware interno do computador rege o tempo.  
  
### <a name="manually-specified-synchronization"></a>Sincronização especificada manualmente  
Sincronização especificada manualmente permite que você designar um único par ou na lista de pares do qual computador obtém o horário. Se o computador não for membro de um domínio, ele deve ser configurado manualmente para sincronizar com uma fonte de tempo especificado. Um computador que é que um membro de um domínio está configurado por padrão para sincronizar a partir a hierarquia de domínio, sincronização especificada manualmente é mais útil para a raiz da floresta do domínio ou para computadores que não tenham ingressados em um domínio. Especificar manualmente um servidor NTP externo para sincronizar com o computador autoritativo para seu domínio fornece um tempo confiável. No entanto, configurar o computador autoritativo para seu domínio sincronizar com um relógio de hardware é realmente uma solução melhor para fornecer o horário mais preciso e seguro para seu domínio.  
  
Fontes de tempo especificados manualmente não são autenticadas, a menos que seja criado um provedor de tempo específico para eles, e, portanto, são vulneráveis a ataques. Além disso, se um computador é sincronizado com uma fonte especificada manualmente em vez do controlador de domínio autenticado, os dois computadores podem ser fora de sincronia, causando falha na autenticação Kerberos. Isso pode causar outras ações que requerem autenticação de rede falhar, como impressão ou compartilhamento de arquivos. Se apenas a raiz da floresta está configurada para sincronizar com uma fonte externa, todos os outros computadores na floresta permanecerão sincronizados entre si, dificultando a ataques de repetição.  
  
### <a name="all-available-synchronization-mechanisms"></a>Todos os mecanismos de sincronização disponíveis  

A opção "todos os mecanismos de sincronização disponíveis" é o método de sincronização mais útil para usuários em uma rede. Esse método permite a sincronização com a hierarquia de domínio e também pode fornecer uma alternativa a fonte de tempo se a hierarquia de domínio não estiver disponível, dependendo da configuração. Se o cliente estiver não é possível sincronizar o tempo com a hierarquia de domínio, a fonte de tempo automaticamente utilizará a fonte de tempo especificada pelo **NtpServer** configuração. Esse método de sincronização é mais provável oferecer um tempo preciso aos clientes.  

### <a name="stopping-time-synchronization"></a>Sincronização de tempo parando  
Há determinadas situações em que deseja interromper a um computador da sincronização de seu tempo. Por exemplo, se um computador tenta sincronizar de uma fonte de horário na Internet ou de outro site por uma WAN por meio de uma conexão dial-up, ele pode incorrer em encargos de telefone barata. Quando você desabilitar a sincronização no computador, você evita que o computador tentar acessar uma fonte de tempo ao longo de uma conexão dial-up.  
  
Você também pode desabilitar a sincronização para impedir a geração de erros no log de eventos. Cada vez que um computador tenta sincronizar com uma fonte de tempo não estiver disponível, ele gera um erro no Log de eventos. Se uma fonte de tempo é retirada fora da rede para manutenção agendada e você não pretende reconfigurar o cliente a sincronizar de outra fonte, você pode desabilitar a sincronização no cliente para impedir que ele tente sincronização enquanto o servidor de horário não estiver disponível.  
  
É útil desabilitar a sincronização no computador em que é designada como a raiz da rede sincronização. Isso indica que o computador de raiz confia em seu relógio local. Se a raiz da hierarquia de sincronização não está definida para **NoSync** e se ele for capaz de sincronizar com outra fonte de tempo, os clientes não aceitar o pacote que este computador envia porque seu tempo não possam ser confiável.  
  
O único momento em servidores que sejam confiáveis por clientes, mesmo se eles não tem sido sincronizado com outra fonte de tempo são aqueles que foram identificados como servidores de horário confiável pelo cliente.  
  
### <a name="disabling-the-windows-time-service"></a>Desativar o serviço de tempo do Windows  
O serviço de tempo do Windows (W32Time) pode ser completamente desativado. Se você optar por implementar um produto de sincronização de tempo de terceiros que usa NTP, você deverá desabilitar o serviço de tempo do Windows. Isso ocorre porque todos os servidores NTP precisam de acesso a porta UDP User Datagram Protocol () 123 e desde que o serviço de tempo do Windows está em execução no sistema operacional Windows Server 2003, a porta 123 permanece reservada pelo tempo do Windows.  
  
## <a name="w2k3tr_times_how_ydum"></a>Portas de rede usadas pelo serviço de tempo do Windows  
O serviço de tempo do Windows se comunica com uma rede para identificar as fontes de tempo confiável, obter informações de tempo e fornecer informações de tempo para outros computadores. Ele executa essa comunicação, conforme definido pelo NTP e SNTP RFCs.  
  
**Atribuições de porta para o serviço de tempo do Windows**  
  
|Nome do serviço|UDP|TCP|  
|----------------|-------|-------|  
|NTP|123|N/D|  
|SNTP|123|N/D|  
  
## <a name="see-also"></a>Consulte também  
[Referência técnica de serviço de tempo do Windows](https://technet.microsoft.com/library/cc773061.aspx)  
[Ferramentas de serviço de tempo do Windows e configurações](Windows-Time-Service-Tools-and-Settings.md)  
[Artigo da Base de Conhecimento Microsoft 902229](https://go.microsoft.com/fwlink/?LinkId=186066)  
  


