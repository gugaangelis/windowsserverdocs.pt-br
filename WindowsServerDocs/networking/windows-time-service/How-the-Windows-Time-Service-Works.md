---
ms.assetid: d1953097-63ea-4a0e-b860-2f3b7c175c41
title: Como funciona o serviço Horário do Windows
description: ''
author: shortpatti
ms.author: pashort
manager: dougkim
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: networking
ms.openlocfilehash: 67c3471a726df354e0faa9e3aced491c4084e9e3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59864337"
---
# <a name="how-the-windows-time-service-works"></a>Como funciona o serviço Horário do Windows

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10 ou posterior

**Nesta seção**  
  
-   [Arquitetura de serviço de tempo do Windows](#w2k3tr_times_how_rrfo)  
  
-   [Protocolos de tempo de serviço do Windows tempo](#w2k3tr_times_how_ekoc)  
  
-   [Serviço de processos e interações de tempo de Windows](#w2k3tr_times_how_izcr)  
  
-   [Portas de rede usado pelo serviço de tempo do Windows](#w2k3tr_times_how_ydum)  
  
> [!NOTE]  
> Este tópico explica apenas como funciona o serviço de tempo do Windows (W32Time). Para obter informações sobre como configurar o serviço de tempo do Windows, consulte [Configurando os sistemas de alta precisão](configuring-systems-for-high-accuracy.md).
  
> [!NOTE]  
> No Windows Server 2003 e Microsoft Windows 2000 Server, o serviço de diretório é chamado serviço de diretório do Active Directory. No Windows Server 2008 e versões posteriores, o serviço de diretório é chamado de serviços de domínio Active Directory (AD DS). O restante deste tópico refere-se ao AD DS, mas as informações também são aplicáveis para o Active Directory.  
  
Embora o serviço horário do Windows não seja uma implementação exata do protocolo NTP (Network Time), ele usa o pacote complexo de algoritmos que é definido nas especificações de NTP para garantir que os relógios dos computadores em toda uma rede sejam tão precisos quanto possível. O ideal é que todos os relógios do computador em um domínio AD DS são sincronizados com a hora de um computador autoritativo. Muitos fatores podem afetar a sincronização de hora em uma rede. Geralmente, os seguintes fatores afetam a precisão da sincronização no AD DS:  
  
-   Condições da rede  
  
-   A precisão do relógio de hardware do computador  
  
-   A quantidade de recursos de CPU e de rede disponíveis para o serviço de tempo do Windows  
  
> [!IMPORTANT]  
> Antes do Windows Server 2016, o serviço W32Time não foi projetado para atender às necessidades do aplicativo sensível ao tempo.  No entanto, as atualizações para o Windows Server 2016 agora permitem que você implemente uma solução para 1ms precisão em seu domínio.  Ver [tempo do Windows 2016 precisos](accurate-time.md) e [limite de suporte para configurar o serviço de tempo do Windows para ambientes de alta precisão](support-boundary.md) para obter mais informações.  
  
Computadores que sincronizar menos tempo com frequência ou que não fazem parte de um domínio são configurados por padrão, para sincronizar com o time.windows.com.  Portanto, é impossível garantir a precisão de tempo em computadores que têm de intermitente ou nenhuma conexão de rede.  
  
Uma floresta do AD DS tem uma hierarquia de sincronização de tempo predeterminado. O serviço de tempo do Windows sincroniza o tempo entre computadores dentro da hierarquia, com os relógios de referência mais precisos na parte superior. Se mais de uma fonte de tempo é configurada em um computador, o tempo do Windows usa algoritmos NTP para selecionar a melhor fonte de tempo das fontes configuradas com base em capacidade de sincronizar com essa fonte de horário do computador. O serviço de tempo do Windows não oferece suporte à sincronização de rede de difusão ou multicast pares. Para obter mais informações sobre esses recursos NTP, consulte RFC 1305 no banco de dados de RFC da IETF.  
  
Cada computador que está executando o serviço de tempo do Windows usa o serviço para manter a hora mais precisa. Computadores que são membros de um domínio de agir como um cliente de tempo, por padrão, portanto, na maioria dos casos não é necessário configurar o serviço de tempo do Windows. No entanto, o serviço de tempo do Windows pode ser configurado para solicitar a hora de uma fonte de tempo designado de referência e também pode fornecer tempo aos clientes.
  
O grau ao qual a hora do computador é precisa é chamado de uma camada. A fonte de tempo mais precisa em uma rede (por exemplo, um relógio de hardware) ocupa o nível mais baixo de stratum ou stratum um. Esta fonte de horário com precisão é chamado de um relógio de referência. Um servidor NTP que adquire seu tempo diretamente de um relógio de referência ocupa uma camada que é um nível mais alto do que o relógio de referência. Recursos que adquirem de tempo do servidor NTP são duas etapas para fora o relógio de referência e, portanto, ocupam uma camada que é a maior do que a fonte de tempo mais precisa e assim por diante. Como o número de camada do computador aumenta, a hora no relógio do seu sistema pode se tornar menos precisos. Portanto, o nível de camada de qualquer computador é um indicador da forma como esse computador está sincronizado com a fonte de tempo mais precisa.  
  
Quando o Gerenciador de W32Time recebe exemplos de tempo, ele usa algoritmos especiais no NTP para determinar qual dos exemplos de tempo é o mais apropriado para uso. O serviço de tempo também usa outro conjunto de algoritmos para determinar qual das fontes de tempo configurado é o mais preciso. Quando o serviço de tempo tiver determinado que exemplo de tempo é melhor, com base em critérios acima, ele ajusta a velocidade do relógio local para permitir a convergência no sentido horário correto. Se a diferença de tempo entre o relógio local e o exemplo precisos de tempo selecionado (também chamado de tempo de distorção) é muito grande para corrigir, ajustando a velocidade do relógio local, o serviço de tempo define o relógio local com a hora correta. Esse ajuste da taxa de relógio ou alteração de hora do relógio direto é conhecido como a disciplina de relógio.  
  
## <a name="w2k3tr_times_how_rrfo"></a>Arquitetura de serviço de tempo do Windows  
O serviço de tempo do Windows consiste dos seguintes componentes:  
  
-   Gerenciador de Controle de Serviços  
  
-   Gerenciador de serviço de tempo do Windows  
  
-   Disciplina de relógio  
  
-   Provedores de tempo  
  
A figura a seguir mostra a arquitetura do serviço de tempo do Windows.  
  
**Arquitetura de serviço de tempo do Windows**  
  
![Tempo do Windows](../media/Windows-Time-Service/How-the-Windows-Time-Service-Works/trnt_sec_arcc.gif)
  
O Gerenciador de controle de serviço é responsável por iniciar e parar o serviço de tempo do Windows. O Gerenciador de serviço de tempo do Windows é responsável por iniciar a ação dos provedores de tempo de NTP incluídos com o sistema operacional. O Gerenciador de serviço de tempo do Windows controla todas as funções do serviço de tempo do Windows e a união de todas as amostras de tempo. Além fornecendo informações sobre o estado atual do sistema, como a fonte de tempo atual ou a última hora do relógio do sistema foi atualizado, o Gerenciador de serviço de tempo do Windows também é responsável pela criação de eventos no log de eventos.  
  
O processo de sincronização de hora envolve as seguintes etapas:  
  
-   Provedores de solicitação de entrada e receber amostras de tempo de fontes de horário NTP configurados.  
  
-   Esses exemplos de tempo, em seguida, são passados para o Windows serviço Gerenciador de tempo que coleta todos os exemplos e os passa para o subcomponente de disciplina de relógio.  
  
-   O subcomponente de disciplina de relógio se aplica os algoritmos NTP, que resulta na seleção do melhor exemplo de hora.  
  
-   O subcomponente de disciplina de relógio ajusta a hora do relógio do sistema para a hora mais precisa ajustar a velocidade do relógio ou alterando diretamente o tempo.  
  
Se um computador tiver sido designado como um servidor de horário, ele pode enviar o tempo de logon em qualquer computador solicitando a sincronização de hora em qualquer ponto nesse processo.  
  
## <a name="w2k3tr_times_how_ekoc"></a>Protocolos de tempo de serviço do Windows tempo  

Protocolos de tempo determinam a proximidade de dois computadores relógios estão sincronizados. Um protocolo de tempo é responsável por determinar as melhores informações de tempo disponível e convergir os relógios para garantir que um momento consistente é mantido em sistemas separados.  
  
O serviço de tempo do Windows usa o protocolo NTP (Network Time) para ajudar a sincronizar a hora em uma rede. NTP é um protocolo de tempo de Internet que inclui os algoritmos de disciplina necessários para a sincronização de relógios. NTP é um protocolo de tempo mais preciso do que o simples SNTP Network Time Protocol () que é usado em algumas versões do Windows; No entanto, W32Time continuará a dar suporte a SNTP para habilitar a compatibilidade com versões anteriores com computadores que executam serviços de tempo com base no SNTP, como o Windows 2000.  
  
### <a name="network-time-protocol"></a>Network Time Protocol  
Protocolo NTP (Network Time) é o padrão usado pelo serviço de tempo do Windows no sistema operacional de protocolo de sincronização de tempo. NTP é um protocolo de tempo e tolerante a falhas, altamente escalonável e é o protocolo mais usado para sincronizar os relógios dos computadores usando uma referência de tempo designado.  
  
Sincronização de horário NTP ocorre durante um período de tempo e envolve a transferência de pacotes do NTP em uma rede. Pacotes de NTP contenham carimbos de data / hora que incluem uma amostra de tempo com o cliente e o servidor de participação na sincronização de hora.  
  
NTP se baseia em um relógio de referência para definir a hora mais precisa para ser usado e sincroniza todos os relógios em uma rede para esse relógio de referência. NTP usa o tempo Universal Coordenado (UTC) como o padrão universal para a hora atual. UTC é independente dos fusos horários e permite que o NTP a ser usado em qualquer lugar do mundo, independentemente das configurações de fuso horário.  
  
#### <a name="ntp-algorithms"></a>Algoritmos NTP  
NTP inclui dois algoritmos, um algoritmo de filtragem de relógio e um algoritmo de seleção de relógio, para ajudar a determinar o melhor exemplo de hora o serviço horário do Windows. O algoritmo de filtragem de relógio destina-se a serem analisados por meio de exemplos de hora que são recebidos de fontes de tempo consultado e determinar as melhor amostras de tempo de cada fonte. O algoritmo de seleção de relógio, em seguida, determina o servidor de horário mais preciso na rede. Essa informação é então passada para o algoritmo de disciplina de relógio, que usa as informações coletadas para corrigir o relógio local do computador, compensando erros devido à imprecisão de relógio de latência e o computador de rede.  
  
Os algoritmos NTP são mais precisos sob condições de cargas de rede e o servidor de luz a moderadas. Assim como acontece com qualquer algoritmo que leva em conta a tempo de trânsito de rede, algoritmos NTP podem executar mal sob condições de congestionamento de rede extremo. Para obter mais informações sobre os algoritmos NTP, consulte RFC 1305 no banco de dados de RFC da IETF.  
  
#### <a name="ntp-time-provider"></a>Provedor de tempo NTP  
O serviço de tempo do Windows é um pacote de sincronização de hora completa que pode dar suporte a uma variedade de dispositivos de hardware e protocolos de tempo. Para habilitar esse suporte, o serviço usa provedores de tempo conectável. Um provedor de tempo é responsável por qualquer um dos carimbos de hora precisa obter (da rede ou de hardware) ou para fornecer esses carimbos de data / hora para outros computadores na rede.  
  
O provedor NTP é o provedor de tempo padrão incluído com o sistema operacional. O provedor NTP segue os padrões especificados pelo NTP versão 3 para um cliente e servidor e pode interagir com clientes SNTP e servidores para compatibilidade com versões anteriores com o Windows 2000 e outros clientes SNTP. O provedor NTP no serviço de tempo do Windows consiste em duas partes a seguir:  
  
-   **Provedor de NtpServer de saída.** Isso é um servidor de horário responde às solicitações de tempo de clientes na rede.  
  
-   **Provedor de entrada NtpClient.** Isso é um cliente de tempo que obtém informações de tempo de outra origem, um dispositivo de hardware ou um servidor NTP e retorna as amostras de tempo que são úteis para sincronizar o relógio local.  
  
Embora as operações reais desses dois provedores estão intimamente relacionadas, eles aparecem independentes para o serviço de tempo. Começando com o Windows 2000 Server, quando um computador Windows é conectado a uma rede, ele é configurado como um cliente NTP. Além disso, computadores que executam o tempo do Windows do serviço apenas uma tentativa de sincronizar a hora com um controlador de domínio ou uma fonte de tempo especificado manualmente por padrão. Estes são os provedores de tempo preferencial, porque eles são automaticamente disponíveis e seguros de fontes de tempo.  
  
#### <a name="ntp-security"></a>Segurança NTP  

Dentro de uma floresta do AD DS, o serviço de tempo do Windows se baseia nos recursos de segurança de domínio padrão para impor a autenticação de dados de tempo. A segurança dos pacotes NTP que são enviadas entre um computador membro do domínio e um controlador de domínio local que está atuando como um servidor de horário se baseia na autenticação de chave compartilhada. O serviço de tempo do Windows usa a chave de sessão do Kerberos do computador para criar assinaturas autenticadas nos pacotes NTP que são enviados pela rede. Pacotes de NTP não são transmitidas no canal seguro de Logon de rede. Em vez disso, quando um computador solicita o tempo de um controlador de domínio na hierarquia de domínio, o serviço de tempo do Windows requer que o tempo seja autenticado. O controlador de domínio, em seguida, retorna as informações necessárias na forma de um valor de 64 bits que foi autenticado com a chave de sessão do serviço de Logon de rede. Se o pacote NTP retornado não está assinado com a chave de sessão do computador ou está assinado incorretamente, o tempo será rejeitado. Essas falhas de autenticação são registradas no Log de eventos. Dessa forma, o serviço de tempo do Windows fornece segurança para dados do NTP em uma floresta do AD DS.  
  
Em geral, os clientes de tempo do Windows automaticamente obtêm horário com precisão para a sincronização de controladores de domínio no mesmo domínio. Em uma floresta, os controladores de domínio de um domínio filho sincronizam a hora com os controladores de domínio em seus domínios pai. Quando um servidor de horário retorna um pacote NTP autenticado a um cliente que solicita o tempo, o pacote é assinado por meio de uma chave de sessão do Kerberos definida por uma conta de confiança entre domínios. A conta de confiança entre domínios é criada quando um novo domínio do AD DS ingressa em uma floresta e o serviço de Logon de rede gerencia a chave de sessão. Dessa forma, o controlador de domínio que esteja configurado como confiável no domínio raiz da floresta torna-se a fonte de tempo autenticado para todos os controladores de domínio em domínios pai e filho e indiretamente para todos os computadores localizados na árvore de domínio.  
  
O serviço de tempo do Windows pode ser configurado para funcionar entre florestas, mas é importante observar que essa configuração não é segura. Por exemplo, um servidor NTP pode estar disponível em uma floresta diferente. No entanto, como esse computador está em uma floresta diferente, não há nenhuma chave de sessão do Kerberos com o qual assinar e autenticar NTP pacotes. Para obter a sincronização de horário com precisão de um computador em uma floresta diferente, o cliente precisa de acesso à rede para esse computador e o serviço de tempo deve ser configurado para usar uma fonte de tempo específico localizada em outra floresta. Se um cliente é configurado manualmente para o tempo de acesso de um servidor NTP fora de sua própria hierarquia de domínio, os pacotes NTP enviados entre o cliente e o servidor de horário não são autenticados e, portanto, não são seguros. Mesmo com a implementação de relações de confiança de floresta, o serviço de tempo do Windows não é seguro entre as florestas. Embora o canal de segurança de Logon de rede é o mecanismo de autenticação para o serviço de tempo do Windows, não há suporte para a autenticação entre florestas.  
  
#### <a name="hardware-devices-that-are-supported-by-the-windows-time-service"></a>Dispositivos de hardware que são compatíveis com o serviço de tempo do Windows  
Relógios com base em hardware, como GPS ou relógios de rádio geralmente são usados como dispositivos de relógio de referência altamente precisos. Por padrão, o provedor de tempo NTP de serviço de tempo do Windows não oferece suporte a conexão direta de um dispositivo de hardware para um computador, embora seja possível criar um provedor de tempo independente baseada em software que dá suporte a esse tipo de conexão. Esse tipo de provedor, em conjunto com o serviço de tempo do Windows, pode fornecer uma referência de tempo confiável e estável.  
  
Dispositivos de hardware, como um relógio cesium ou um destinatário, o sistema GPS (Global Positioning) fornecem a hora atual precisa seguindo um padrão para obter uma definição precisa de tempo. Relógios cesium são extremamente estáveis e não são afetados por fatores como temperatura, pressão ou umidade, mas também são muito caros. Um receptor GPS muito menos cara e também é um relógio de referência precisos. Os receptores GPS obtêm seu tempo de satélites que obtêm seus horários de um relógio cesium. Sem o uso de um provedor de tempo independente, os servidores de horário do Windows podem adquirir seu tempo ao se conectar a um servidor NTP externo, que está conectado a um dispositivo de hardware por meio de um telefone ou Internet. As organizações, como o Observatório Naval de Estados Unidos fornecer servidores NTP que estão conectados ao relógios referência extremamente confiável.  
  
Muitos receptores GPS e outros dispositivos de tempo podem funcionar como servidores NTP em uma rede. Você pode configurar sua floresta do AD DS para sincronizar a hora desses dispositivos de hardware externo somente se eles também estão atuando como servidores NTP na sua rede. Para fazer isso, configure o controlador de domínio está funcionando como controlador de domínio primário, emulador (PDC) na sua raiz de floresta para sincronizar com o servidor NTP fornecido pelo dispositivo GPS. Para fazer isso, consulte [configurar o serviço de tempo do Windows no emulador PDC no domínio raiz da floresta](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731191%28v=ws.10%29).  
  
### <a name="simple-network-time-protocol"></a>Protocolo de tempo de rede simples  
O simples SNTP Network Time Protocol () é um protocolo de tempo simplificado que é destinado para servidores e clientes que não exigem o grau de precisão NTP fornece. SNTP, uma versão mais rudimentar de NTP, é o protocolo de tempo primário que é usado no Windows 2000. Como os formatos do pacote de rede do SNTP e NTP são idênticos, os dois protocolos são interoperáveis. A principal diferença entre os dois é que SNTP não tem o gerenciamento de erros e os sistemas de filtragem complexos que fornece NTP. Para obter mais informações sobre o Network Time Protocol simples, consulte RFC 1769 no banco de dados de RFC da IETF.  
  
### <a name="time-protocol-interoperability"></a>Interoperabilidade de protocolo de tempo  
O serviço de tempo do Windows pode operar em um ambiente misto de computadores que executam o Windows 2000, Windows XP e Windows Server 2003, como o protocolo SNTP usado no Windows 2000 é interoperável com o protocolo NTP no Windows XP e Windows Server 2003.  
  
O serviço de tempo no Windows NT Server 4.0, chamado TimeServ, sincroniza a hora em uma rede do Windows NT 4.0. TimeServ é um recurso de complemento disponível como parte dos *Kit de recursos do Microsoft Windows NT 4.0* e não fornecer o grau de confiabilidade da sincronização de hora que é exigida pelo Windows Server 2003.  
  
O serviço de tempo do Windows pode interoperar com computadores que executam o Windows NT 4.0 porque eles podem sincronizar a hora com computadores que executam o Windows 2000 ou Windows Server 2003; No entanto, um computador que executa o Windows 2000 ou Windows Server 2003 não detecta automaticamente os servidores de horário do Windows NT 4.0. Por exemplo, se seu domínio for configurado para sincronizar a hora usando o método baseado em hierarquia do domínio de sincronização e desejar que os computadores na hierarquia de domínio para sincronizar a hora com um controlador de domínio do Windows NT 4.0, você precisará configurá-los computadores manualmente para sincronizar com os controladores de domínio do Windows NT 4.0.  
  
Windows NT 4.0 usa um mecanismo mais simples para a sincronização de tempo que o serviço de tempo do Windows usa. Portanto, para garantir a sincronização de horário com precisão em sua rede, é recomendável que você atualize os controladores de domínio do Windows NT 4.0 para o Windows 2000 ou Windows Server 2003.  
  
## <a name="w2k3tr_times_how_izcr"></a>Serviço de processos e interações de tempo de Windows  

O serviço de tempo do Windows foi projetado para sincronizar os relógios dos computadores em uma rede. O processo de sincronização de tempo de rede, também chamado de convergência de tempo, ocorre em toda uma rede como hora de acessos de cada computador de um servidor de horário mais preciso. Convergência de tempo envolve um processo pelo qual um servidor autoritativo fornece a hora atual em computadores cliente na forma de pacotes de NTP. As informações fornecidas dentro de um pacote indicam se um ajuste precisa ser feita à hora atual do computador para que ele é sincronizado com o servidor mais preciso.  
  
Como parte do processo de convergência de tempo, os membros do domínio tentam sincronizar o tempo com qualquer controlador de domínio localizado no mesmo domínio. Se o computador for um controlador de domínio, ele tenta sincronizar com um controlador de domínio mais autoritativo.  
  
Computadores que executam o Windows XP Home Edition ou em computadores que não ingressaram em um domínio não tentam sincronizar com a hierarquia de domínio, mas são configuradas por padrão para obter a hora de time.windows.com.  
  
Para estabelecer um computador que executa o Windows Server 2003 como autoritativo, o computador deve ser configurado para ser uma fonte de horário confiável. Por padrão, o primeiro controlador de domínio que é instalado em um domínio do Windows Server 2003 é automaticamente configurado para ser uma fonte de horário confiável. Como é o computador autoritativo para o domínio, ele deve ser configurado para sincronizar com uma fonte de tempo externa em vez de com a hierarquia de domínio. Também por padrão, todos os outros membros de domínio do Windows Server 2003 são configurados para sincronizar com a hierarquia de domínio.  
  
Depois de estabelecer uma rede do Windows Server 2003, você pode configurar o serviço de tempo do Windows para usar uma das seguintes opções para a sincronização:  
  
-   Sincronização da hierarquia baseada em domínio  
  
-   Uma origem de sincronização especificada manualmente  
  
-   Todos os mecanismos de sincronização disponíveis  
  
-   Nenhuma sincronização.  
  
Cada um desses tipos de sincronização é abordada na seção a seguir.  
  
### <a name="domain-hierarchy-based-synchronization"></a>Sincronização da hierarquia baseada em domínio  

Sincronização com base em uma hierarquia de domínio usa a hierarquia de domínio do AD DS para localizar uma fonte confiável com o qual sincronizar a hora. Com base na hierarquia de domínio, o serviço de tempo do Windows determina a precisão de cada servidor de horário. Em uma floresta do Windows Server 2003, o computador que detém a domínio primário (PDC) do controlador operações função de mestre emulador, localizada no domínio raiz da floresta, contém a posição da melhor fonte de tempo, a menos que outra fonte de horário confiável tiver sido configurado. A figura a seguir ilustra um caminho de tempo de sincronização entre computadores em uma hierarquia de domínio.  
  
**Sincronização de hora em uma hierarquia do AD DS**  
![Tempo do Windows](../media/Windows-Time-Service/How-the-Windows-Time-Service-Works/trnt_ntw_adhc.gif)
  
#### <a name="reliable-time-source-configuration"></a>Configuração de fonte de tempo confiável  
Um computador que está configurado para ser uma fonte de tempo confiável é identificado como a raiz do serviço de tempo. A raiz do serviço de tempo é o servidor autoritativo para o domínio e normalmente é configurada para recuperar o tempo de um servidor NTP externo ou um dispositivo de hardware. Um servidor de horário pode ser configurado como uma fonte de tempo confiável para otimizar como a hora é transferida em toda a hierarquia de domínio. Se um controlador de domínio é configurado para ser uma fonte de horário confiável, o serviço de Logon de rede anuncia esse controlador de domínio como uma fonte de tempo confiável quando ele faz logon na rede. Quando uma fonte de tempo sincronizar com procurarem outros controladores de domínio, eles escolhem uma fonte confiável primeiro se houver uma disponível.  
  
#### <a name="time-source-selection"></a>Seleção de fonte de tempo  
O processo de seleção de fonte de tempo pode criar dois problemas em uma rede:  
  
-   Ciclos de sincronização adicional.  
  
-   Aumento do volume de tráfego de rede.  
  
Um ciclo na rede de sincronização ocorre quando o tempo permanece consistente entre um grupo de controladores de domínio e a mesma hora é compartilhada entre eles continuamente sem uma ressincronização com outra fonte de horário confiável. Algoritmo de seleção de fonte de tempo do serviço de tempo do Windows foi projetado para proteger contra esses tipos de problemas.  
  
Um computador usa um dos seguintes métodos para identificar uma fonte de tempo para sincronizar com:  
  
-   Se o computador não for um membro de um domínio, ele deve ser configurado para sincronizar com uma fonte de tempo especificado.  
  
-   Se o computador for um servidor membro ou estação de trabalho dentro de um domínio, por padrão, ele segue a hierarquia de AD DS e sincroniza seu tempo com um controlador de domínio em seu domínio local que está sendo executado o serviço de tempo do Windows.  
  
Se o computador for um controlador de domínio, ele faz até seis consultas para localizar outro controlador de domínio para sincronizar com. Cada consulta é criada para identificar uma fonte de horário com determinados atributos, como um tipo de controlador de domínio, um local específico, e é uma fonte de horário confiável ou não. A fonte de tempo também deve cumprir as seguintes restrições:  
  
-   Uma fonte de horário confiável só pode sincronizar com um controlador de domínio no domínio pai.  
  
-   Um emulador de PDC pode sincronizar com uma fonte de horário confiável em seu próprio domínio ou qualquer controlador de domínio no domínio pai.  
  
Se o controlador de domínio não é capaz de sincronizar com o tipo de controlador de domínio que ele está consultando, a consulta não será feita. O controlador de domínio sabe qual tipo de computador, ele poderá obter o tempo de antes de ele torna a consulta. Por exemplo, um emulador PDC local não tenta números de consulta, três ou seis porque um controlador de domínio não tenta sincronizar com ele próprio.  
  
A tabela a seguir lista as consultas que faz com que um controlador de domínio para localizar uma fonte de tempo e a ordem na qual as consultas são feitas.  
  
**Consulta de fonte de tempo do controlador de domínio**  
  
|Número de consultas|Controlador de domínio|Location|Confiabilidade da fonte de tempo|  
|----------------|---------------------|------------|------------------------------|  
|1|Controlador de domínio pai|No site|Prefere um confiável fonte de tempo, mas ele pode sincronizar com uma fonte de tempo não confiável, se isso for tudo o que está disponível.|  
|2|Controlador de domínio local|No site|Sincroniza somente com uma fonte de horário confiável.|  
|3|Emulador PDC local|No site|Não se aplica.<br /><br />Um controlador de domínio não tenta sincronizar com ele próprio.|  
|4|Controlador de domínio pai|Fora do site|Prefere um confiável fonte de tempo, mas ele pode sincronizar com uma fonte de tempo não confiável, se isso for tudo o que está disponível.|  
|5|Controlador de domínio local|Fora do site|Sincroniza somente com uma fonte de horário confiável.|  
|6|Emulador PDC local|Fora do site|Não se aplica.<br /><br />Um controlador de domínio não tenta sincronizar com ele próprio.| 
  
**Observação**  
  
-   Um computador nunca for sincronizado com ele próprio. Se o computador em que a tentativa de sincronização é o emulador PDC local, ele não tentará 3 consultas ou 6.  
  
Cada consulta retorna uma lista de controladores de domínio que pode ser usado como uma fonte de tempo. Tempo do Windows atribui cada controlador de domínio que é consultado uma pontuação com base no local do controlador de domínio e confiabilidade. A tabela a seguir lista as pontuações atribuídas pelo tempo do Windows para cada tipo de controlador de domínio.  
  
**Determinação de pontuação**  
  
|Status do controlador de domínio|Pontuação|  
|----------------------------|---------|  
|Controlador de domínio localizado no mesmo site|8|  
|Controlador de domínio marcada como uma fonte de horário confiável|4|  
|Controlador de domínio localizado no domínio pai|2|  
|Controlador de domínio que é um emulador PDC|1|  
  
Quando o serviço de tempo do Windows determina que identificou o controlador de domínio com a melhor pontuação de possível, não há mais consultas são feitas. As pontuações atribuídas pelo serviço de tempo são cumulativas, o que significa que um emulador PDC, localizado no mesmo site recebe uma pontuação de nove.  
  
Se a raiz do serviço de tempo não está configurada para sincronizar com uma fonte externa, o relógio interno do hardware do computador controla o tempo.  
  
### <a name="manually-specified-synchronization"></a>Sincronização especificada manualmente  
Sincronização especificada manualmente permite designar um único par ou lista de colegas do qual o computador obtém o tempo. Se o computador não for um membro de um domínio, ele deve ser configurado manualmente para sincronizar com uma fonte de tempo especificado. Um computador que é que um membro de um domínio é configurado por padrão para sincronizar a partir da hierarquia de domínio, a sincronização especificada manualmente é mais útil para a raiz da floresta do domínio ou para computadores que não estão associados a um domínio. Especificar manualmente um servidor NTP externo para sincronizar com o computador autoritativo para seu domínio fornece tempo confiável. No entanto, a configuração do computador autoritativo para seu domínio sincronizar com um relógio de hardware é, na verdade, uma solução melhor para fornecer o tempo mais preciso e seguro para seu domínio.  
  
Fontes de tempo especificados manualmente não são autenticadas, a menos que seja criado um provedor de tempo específico para eles e, portanto, são vulneráveis a invasores. Além disso, se um computador for sincronizado com uma fonte especificada manualmente em vez de seu controlador de domínio de autenticação, os dois computadores ficará fora de sincronização, fazendo com que a autenticação Kerberos falhe. Isso pode causar outras ações que exigem autenticação de rede falhar, como impressão ou compartilhamento de arquivos. Se apenas a raiz da floresta é configurada para sincronizar com uma fonte externa, todos os outros computadores na floresta permanecerão sincronizados entre si, dificultando a ataques de repetição.  
  
### <a name="all-available-synchronization-mechanisms"></a>Todos os mecanismos de sincronização disponíveis  

A opção "todos os mecanismos de sincronização disponíveis" é o método de sincronização mais útil para usuários em uma rede. Esse método permite a sincronização com a hierarquia de domínio e também pode fornecer uma alternativa se a hierarquia de domínio ficar indisponível, dependendo da configuração de fonte de tempo. Se o cliente não conseguir sincronizar a hora com a hierarquia de domínio, a fonte de tempo reverterá automaticamente para a fonte de tempo especificada pelo **NtpServer** configuração. Esse método de sincronização é mais provável fornecer o horário com precisão a clientes.  

### <a name="stopping-time-synchronization"></a>Parando sincronização de hora  
Há determinadas situações em que você deseja impedir que um computador da sincronização de horário. Por exemplo, se um computador tenta sincronizar de uma fonte de horário na Internet ou de outro site em uma WAN, por meio de uma conexão dial-up, ela pode incorrer em encargos de telefone dispendiosa. Quando você desabilita a sincronização nesse computador, você impede que o computador a tentativa de acessar uma fonte de tempo ao longo de uma conexão dial-up.  
  
Você também pode desabilitar a sincronização para evitar a geração de erros no log de eventos. Sempre que um computador tenta sincronizar com uma fonte de tempo não estiver disponível, ele gera um erro no Log de eventos. Se uma fonte de horário é feita fora da rede para a manutenção agendada e você não pretende reconfigurar o cliente para sincronizar a partir de outra origem, você pode desabilitar a sincronização no cliente para impedir a tentativa de sincronização enquanto o servidor de horário não está disponível.  
  
É útil desabilitar a sincronização no computador que é designada como a raiz da rede de sincronização. Isso indica que o computador raiz confia em seu relógio local. Se a raiz da hierarquia de sincronização não está definida como **NoSync** e se não for possível sincronizar com outra fonte de tempo, os clientes não aceitam o pacote que este computador enviará porque sua hora não pode ser confiável.
  
A única vez em servidores que são confiáveis por clientes, mesmo que não tenham sido sincronizadas com outra fonte de tempo são aqueles que foram identificados como servidores de horário confiável pelo cliente.  
  
### <a name="disabling-the-windows-time-service"></a>Desabilitando o serviço de tempo do Windows  
O serviço de tempo do Windows (W32Time) pode ser completamente desabilitado. Se você optar por implementar um produto de sincronização de tempo de terceiros que usa o NTP, você deve desabilitar o serviço de tempo do Windows. Isso ocorre porque todos os servidores NTP precisam de acesso à porta de protocolo UDP (User Datagram) 123 e desde que o serviço de tempo do Windows está em execução no sistema operacional Windows Server 2003, a porta 123 permanece reservada por tempo do Windows.  
  
## <a name="w2k3tr_times_how_ydum"></a>Portas de rede usado pelo serviço de tempo do Windows  
O serviço de tempo do Windows se comunica em uma rede para identificar as fontes de horário confiável, obter informações de tempo e fornecer informações de tempo para outros computadores. Ele executa essa comunicação, conforme definido pela NTP e SNTP RFCs.  
  
**Atribuições de porta para o serviço de tempo do Windows**  
  
|Nome do serviço|UDP|TCP|  
|----------------|-------|-------|  
|NTP|123|N/D|  
|SNTP|123|N/D|  
  
## <a name="see-also"></a>Consulte também  
[Referência técnica de serviço de tempo do Windows](windows-time-service-tech-ref.md)
[ferramentas de serviço de tempo do Windows e configurações](Windows-Time-Service-Tools-and-Settings.md)
[902229 de artigo de Conhecimento Microsoft](https://go.microsoft.com/fwlink/?LinkId=186066)