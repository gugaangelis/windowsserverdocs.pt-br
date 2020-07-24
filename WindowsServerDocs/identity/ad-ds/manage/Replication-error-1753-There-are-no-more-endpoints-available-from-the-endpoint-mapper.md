---
ms.assetid: 0f21951c-b1bf-43bb-a329-bbb40c58c876
title: Erro de replicação 1753 Não há mais pontos de extremidade disponíveis do mapeador de ponto de extremidade
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: ca7ab368c9e15de15f733070a5bcb06584956500
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86961128"
---
# <a name="replication-error-1753-there-are-no-more-endpoints-available-from-the-endpoint-mapper"></a>Erro de replicação 1753 Não há mais pontos de extremidade disponíveis do mapeador de ponto de extremidade

>Aplica-se a: Windows Server

Este artigo descreve os sintomas, as etapas de causa e resolução para as operações de Active Directory que falham com o erro 1753 do Win32: "não há mais pontos de extremidade disponíveis no mapeador de Endpoint".

O DCDIAG relata que o teste de conectividade, Active Directory teste de replicações ou teste KnowsOfRoleHolders falhou com o erro 1753: "não há mais pontos de extremidade disponíveis no mapeador de ponto".

```
Testing server: <site><DC Name>
Starting test: Connectivity
* Active Directory LDAP Services Check
* Active Directory RPC Services Check
[<DC Name>] DsBindWithSpnEx() failed with error 1753,
There are no more endpoints available from the endpoint mapper..
Printing RPC Extended Error Info:
Error Record 1, ProcessID is <process ID> (DcDiag)
System Time is: <date> <time>
Generating component is 2 (RPC runtime)
Status is 1753: There are no more endpoints available from the endpoint mapper. Detection location is 500
NumberOfParameters is 4
Unicode string: ncacn_ip_tcp
Unicode string: <source DC object GUID>._msdcs.contoso.com
Long val: -481213899
Long val: 65537
Error Record 2, ProcessID is 700 (DcDiag)
System Time is: <date> <time>
Generating component is 2 (RPC runtime)
Status is 1753: There are no more endpoints available from the endpoint mapper.
NumberOfParameters is 1
Unicode string: 1025
[Replications Check,<DC Name>] A recent replication attempt failed:
From <source DC> to <destination DC>
Naming Context: <DN path of directory partition>
The replication generated an error (1753):
There are no more endpoints available from the endpoint mapper.
The failure occurred at <date> <time>.
The last success occurred at <date> <time>.
3 failures have occurred since the last success.
The directory on <DC name> is in the process.
of starting up or shutting down, and is not available.
Verify machine is not hung during boot.
```

REPADMIN.EXE relata que a tentativa de replicação falhou com o status 1753.
Os comandos REPADMIN que normalmente citam o status 1753 incluem, mas não estão limitados a:

* REPADMIN/REPLSUM
* REPADMIN/SHOWREPL
* REPADMIN/SHOWREPS
* REPADMIN/SYNCALL

A saída de exemplo de "REPADMIN/SHOWREPS" que descreve a replicação de entrada de CONTOSO-DC2 para CONTOSO-DC1 falha com o erro "o acesso à replicação foi negado" é mostrado abaixo:

```
Default-First-Site-NameCONTOSO-DC1
DSA Options: IS_GC 
Site Options: (none)
DSA object GUID: b6dc8589-7e00-4a5d-b688-045aef63ec01
DSA invocationID: b6dc8589-7e00-4a5d-b688-045aef63ec01
==== INBOUND NEIGHBORS ======================================
DC=contoso,DC=com
Default-First-Site-NameCONTOSO-DC2 via RPC
DSA object GUID: 74fbe06c-932c-46b5-831b-af9e31f496b2
Last attempt @ <date> <time> failed, result 1753 (0x6d9):
There are no more endpoints available from the endpoint mapper.
<#> consecutive failure(s).
Last success @ <date> <time>.
```

O comando **check Replication Topology** em Active Directory sites e serviços retorna "não há mais pontos de extremidade disponíveis no mapeador de pontos de extremidades".

Clicar com o botão direito do mouse no objeto de conexão de um controlador de domínio de origem e escolher **verificar a topologia de replicação** falha com "não há mais pontos de extremidade disponíveis no mapeador de Endpoint". A mensagem de erro na tela é mostrada abaixo:

Texto do título da caixa de diálogo: verificar o texto da mensagem da caixa de diálogo de topologia de replicação: o seguinte erro ocorreu durante a tentativa de contatar o controlador de domínio: não há mais pontos de extremidade disponíveis no mapeador de Endpoint.

O comando **replicate Now** no Active Directory sites e serviços retorna "não há mais pontos de extremidade disponíveis no mapeador de pontos de extremidades."
Clicar com o botão direito do mouse no objeto de conexão de um controlador de domínio de origem e escolher **replicar agora** falha com "não há mais pontos de extremidade disponíveis no mapeador de Endpoint".
A mensagem de erro na tela é mostrada abaixo:

Texto do título da caixa de diálogo: replicar agora texto da mensagem de diálogo: o seguinte erro ocorreu durante a tentativa de sincronizar o contexto \<%directory partition name%> de nomenclatura do controlador de domínio \<Source DC> para o controlador de domínio \<Destination DC> :

Não há mais pontos de extremidade disponíveis no mapeador de pontos de extremidades.
A operação não continuará

Os eventos NTDS KCC, NTDS General ou Microsoft-Windows-ActiveDirectory_DomainService com o status-2146893022 são registrados no log dos serviços de diretório Visualizador de Eventos.

Active Directory eventos que normalmente citam o status-2146893022 incluem, mas não estão limitados a:

| ID do evento | Origem do Evento | Cadeia de eventos|
| --- | --- | --- |
| 1655 | NTDS geral | Active Directory tentou se comunicar com o catálogo global a seguir e as tentativas não foram bem-sucedidas. |
| 1925 | KCC DO NTDS | Falha na tentativa de estabelecer um link de replicação para a seguinte partição de diretório gravável. |
| 1265 | KCC DO NTDS | Falha na tentativa do Knowledge Consistency Checker (KCC) de adicionar um contrato de replicação para a seguinte partição de diretório e controlador de domínio de origem. |

## <a name="cause"></a>Causa

As etapas a seguir mostram o fluxo de trabalho RPC começando com o registro do aplicativo de servidor com o mapeador de ponto de extremidade RPC (EPM) na etapa 1 para a passagem de dados do cliente RPC para o aplicativo cliente na etapa 7.

### <a name="adds-rpc-workflow"></a>Adiciona o fluxo de trabalho RPC

1. O aplicativo de servidor registra seus pontos de extremidade com o mapeador de ponto de extremidades RPC (EPM)
1. O cliente faz uma chamada RPC (em nome de um usuário, um sistema operacional ou uma operação iniciada pelo aplicativo)
1. O RPC do lado do cliente entra em contato com os computadores de destino EPM e solicita que o ponto de extremidade conclua a chamada do cliente
1. O EPM da máquina do servidor responde com um ponto de extremidade
1. O RPC do lado do cliente entra em contato com o aplicativo do servidor
1. O aplicativo de servidor executa a chamada, retorna o resultado para o RPC do cliente
1. RPC do lado do cliente passa o resultado de volta para o aplicativo cliente

A falha 1753 é gerada por uma falha entre as etapas #3 e #4. Especificamente, o erro 1753 significa que o cliente RPC (DC de destino) foi capaz de contatar o servidor RPC (DC de origem) pela porta 135, mas o EPM no servidor RPC (DC de origem) não pôde localizar o aplicativo RPC de interesse e retornou o erro do lado do servidor 1753. A presença do erro 1753 indica que o cliente RPC (DC de destino) recebeu a resposta de erro do lado do servidor do servidor RPC (DC de origem de replicação do AD) pela rede.

As causas raiz específicas para o erro 1753 incluem:

* O aplicativo de servidor nunca foi iniciado (ou seja, a etapa #1 no diagrama "mais informações" localizado acima nunca foi tentado).
* O aplicativo de servidor foi iniciado, mas houve alguma falha durante a inicialização que impediu o registro com o mapeador de ponto de extremidade RPC (ou seja, a etapa #1 no diagrama "mais informações" acima foi tentado, mas falhou).
* O aplicativo do servidor foi iniciado, mas subsequentemente morreu. (ou seja, a etapa #1 no diagrama "mais informações" acima foi concluída com êxito, mas foi desfeita posteriormente porque o servidor morreu).
* O aplicativo de servidor desregistrou manualmente seus pontos de extremidade (semelhante a 3, mas intencionalmente. Provavelmente não, mas incluído para fins de integridade.)
* O cliente RPC (DC de destino) contatou um servidor RPC diferente do que o pretendido devido a um nome para o erro de mapeamento de IP no arquivo DNS, WINS ou host/Lmhosts.

O erro 1753 não é causado por:

* A falta de conectividade de rede entre o cliente RPC (DC de destino) e o servidor RPC (DC de origem) na porta 135
* A falta de conectividade de rede entre o servidor RPC (DC de origem) usando a porta 135 e o cliente RPC (DC de destino) na porta efêmera.
* Uma incompatibilidade de senha ou a incapacidade pelo DC de origem para descriptografar um pacote criptografado Kerberos

## <a name="resolutions"></a>Resoluções

Verifique se o serviço que está registrando seu serviço com o mapeador de ponto de extremidade foi iniciado

* Para os DCs do Windows 2000 e do Windows Server 2003: Verifique se o controlador de domínio de origem foi inicializado no modo normal.
* Para o Windows Server 2008 ou o Windows Server 2008 R2: no console do DC de origem, inicie o Gerenciador de serviços (Services. msc) e verifique se o serviço de Active Directory Domain Services está em execução.

Verificar se o cliente RPC (DC de destino) está conectado ao servidor RPC pretendido (DC de origem)

Todos os DCs em uma floresta Active Directory comum registram um registro CNAME do controlador de domínio na _msdcs. \<forest root domain>Zona DNS, independentemente do domínio no qual residem dentro da floresta. O registro CNAME do DC é derivado do atributo objectGUID do objeto de configurações NTDS para cada controlador de domínio.

Ao executar operações baseadas em replicação, um DC de destino consulta o DNS para o registro CNAME de DCs de origem. O registro CNAME contém o nome do computador totalmente qualificado do DC de origem que é usado para derivar o endereço IP de DCs de origem por meio da pesquisa de cache do cliente DNS, pesquisa de arquivo do host/LMHost, registro A/AAAA do host no DNS ou WINS.

Objetos de configurações NTDS obsoletos e mapeamentos de nome-para-IP incorretos em arquivos DNS, WINS, host e LMHOST podem fazer com que o cliente RPC (DC de destino) se conecte ao servidor RPC errado (DC de origem). Além disso, o mapeamento de nome para IP inadequado pode fazer com que o cliente RPC (DC de destino) se conecte a um computador que nem mesmo tenha o aplicativo de servidor RPC de seu interesse (a função Active Directory, nesse caso) instalada. (Exemplo: um registro de host obsoleto para DC2 contém o endereço IP de DC3 ou um computador membro).

Verifique se o objectGUID do DC de origem que existe na cópia dos DCs de destino de Active Directory corresponde ao objectGUID do DC de origem armazenado na cópia dos DCs de origem de Active Directory. Se houver uma discrepância, use repadmin/showobjmeta no objeto de configurações NTDS para ver qual delas corresponde à última promoção do DC de origem (dica: Compare os carimbos de data para o objeto de configurações NTDS data de criação de/showobjmeta em relação à última data da promoção no arquivo. log dos DCs de origem. Talvez seja necessário usar a data da última modificação/criação do DCPROMO. Próprio arquivo de LOG). Se os GUIDs de objeto não forem idênticos, o DC de destino provavelmente terá um objeto de configurações NTDS obsoleto para o DC de origem cujo registro CNAME se refere a um registro de host com um nome inválido para o mapeamento de IP.

No DC de destino, execute IPCONFIG/ALL para determinar quais servidores DNS o DC de destino está usando para a resolução de nomes:

```
c:>ipconfig /all
```

No DC de destino, execute NSLOOKUP no registro CNAME do DC de origem dos DCs totalmente qualificados:

```
c:>nslookup -type=cname <fully qualified cname of source DC> <destination DCs primary DNS Server IP >
c:>nslookup -type=cname <fully qualified cname of source DC> <destination DCs secondary DNS Server IP>
```

Verifique se o endereço IP retornado por NSLOOKUP "possui" o nome do host/identidade de segurança do DC de origem:

```
C:>NBTSTAT -A <IP address returned by NSLOOKUP in the step above>
```

Faça logon no console do DC de origem, execute "IPCONFIG" no prompt CMD e verifique se o DC de origem possui o endereço IP retornado pelo comando NSLOOKUP acima

Verificar o host obsoleto/duplicado para mapeamentos de IP no DNS

```
NSLOOKUP -type=hostname <single label hostname of source DC> <primary DNS Server IP on destination DC>
NSLOOKUP -type=hostname <single label hostname of source DC> <secondary DNS Server IP on destination DC>

NSLOOKUP -type=hostname <fully qualified computer name of source DC> <primary DNS Server IP on destination DC>
NSLOOKUP -type=hostname <fully qualified computer name of source DC> <secondary DNS Server IP on dest. DC>
```

Se existirem endereços IP inválidos em registros de host, investigue se a limpeza de DNS está habilitada e configurada corretamente.

Se os testes acima ou um rastreamento de rede não mostrar uma consulta de nome que retorna um endereço IP inválido, considere entradas obsoletas em arquivos de HOST, arquivos LMHOSTs e servidores WINS. Observe que os servidores DNS também podem ser configurados para executar a resolução de nome de fallback do WINS.

* Verifique se o aplicativo de servidor (Active Directory et al) foi registrado com o mapeador de ponto de extremidade no servidor RPC (DC de origem)
* Active Directory usa uma combinação de portas bem conhecidas e registradas dinamicamente. Esta tabela lista portas e protocolos bem conhecidos usados por Active Directory controladores de domínio.

| Aplicativo do servidor RPC | Porta | TCP | UDP |
| --- | --- | --- | --- |
| Servidor DNS | 53 | X | X |
| Kerberos | 88 | X | X |
| Servidor LDAP | 389 | X | X |
| Microsoft-DS | 445 | X | X |
| LDAP SSL | 636 | X | X |
| Servidor de catálogo global | 3268 | X |   |
| Servidor de catálogo global | 3269 | X |   |

As portas conhecidas não são registradas com o mapeador de ponto de extremidade.

Active Directory e outros aplicativos também registram serviços que recebem portas atribuídas dinamicamente no intervalo de portas efêmeras RPC. Esses aplicativos de servidor RPC são dinamicamente atribuídos a portas TCP entre 1024 e 5000 em computadores com Windows 2000 e Windows Server 2003 e portas entre o 49152 e o 65535 Range em computadores Windows Server 2008 e Windows Server 2008 R2. A porta RPC usada pela replicação pode ser embutida no registro usando as etapas documentadas no [artigo 224196 da base de conhecimento](https://support.microsoft.com/kb/224196). Active Directory continuará a se registrar no EPM quando configurado para usar uma porta embutida em código.

Verifique se o aplicativo do servidor RPC de interesse se registrou com o mapeador de ponto de extremidade RPC no servidor RPC (o DC de origem no caso da replicação do AD).

Há várias maneiras de realizar essa tarefa, mas uma é instalar e executar o PORTQRY de um prompt CMD com privilégios de administrador no console do DC de origem usando a sintaxe:

```
portquery -n <source DC> -e 135 > file.txt
```

Na saída do Portqry, observe os números de porta registrados dinamicamente pela "interface do MS NT Directory DRS" (UUID = 351...) para o protocolo ncacn_ip_tcp. O trecho de código a seguir mostra a saída de PortQuery de exemplo de um controlador de domínio do Windows Server 2008 R2:

```
UUID: e3514235-4b06-11d1-ab04-00c04fc2dcd2 MS NT Directory DRS Interface
ncacn_np:CONTOSO-DC01[\pipe\lsass] 
UUID: e3514235-4b06-11d1-ab04-00c04fc2dcd2 MS NT Directory DRS Interface
ncacn_np:CONTOSO-DC01[\PIPE\protected_storage] 
UUID: e3514235-4b06-11d1-ab04-00c04fc2dcd2 MS NT Directory DRS Interface
ncacn_ip_tcp:CONTOSO-DC01[49156] 
UUID: e3514235-4b06-11d1-ab04-00c04fc2dcd2 MS NT Directory DRS Interface
ncacn_http:CONTOSO-DC01[49157] 
UUID: e3514235-4b06-11d1-ab04-00c04fc2dcd2 MS NT Directory DRS Interface
ncacn_http:CONTOSO-DC01[6004]
```

Outras maneiras possíveis de resolver esse erro:

* Verifique se o controlador de domínio de origem está inicializado no modo normal e se a função do sistema operacional e do controlador de domínio no controlador de domínio de origem foi totalmente iniciada.
* Verifique se o serviço Domínio do Active Directory está em execução. Se o serviço estiver parado ou não configurado com valores de inicialização padrão, redefina os valores de inicialização padrão, reinicialize o controlador de domínio modificado e repita a operação.
* Verifique se o valor de inicialização e o status do serviço para o serviço RPC e o localizador RPC estão corretos para a versão do sistema operacional do cliente RPC (DC de destino) e do servidor RPC (DC de origem). Se o serviço estiver parado ou não configurado com valores de inicialização padrão, redefina os valores de inicialização padrão, reinicialize o controlador de domínio modificado e repita a operação.
   * Além disso, verifique se o contexto de serviço corresponde às configurações padrão listadas na tabela a seguir.

      | Serviço | Status padrão (tipo de inicialização) no Windows Server 2003 e posterior | Status padrão (tipo de inicialização) no Windows Server 2000 |
      | --- | --- | --- |
      | Chamada de Procedimento Remoto | Iniciado (automático) | Iniciado (automático) |
      | Localizador de chamada de procedimento remoto | Nulo ou parado (manual) | Iniciado (automático) |

* Verifique se o tamanho do intervalo de portas dinâmicas não foi restrito. A sintaxe NETSH do Windows Server 2008 e do Windows Server 2008 R2 para enumerar o intervalo de portas RPC é mostrada abaixo:

   ```
   netsh int ipv4 show dynamicport tcp
   netsh int ipv4 show dynamicport udp
   netsh int ipv6 show dynamicport tcp
   netsh int ipv6 show dynamicport udp
   ```

* Verifique se as definições de porta codificadas definidas em KB 224196 se enquadram no intervalo de portas dinâmicas para a versão do sistema operacional dos DCs de origem. Examine o [artigo 224196 da base de conhecimento](https://support.microsoft.com/kb/224196) e verifique se a porta embutida em código está dentro do intervalo de portas efêmeras para a versão do sistema operacional do DC de origem.

* Verifique se a chave ClientProtocols existe em HKLM\Software\Microsoft\Rpc e contém os 5 valores padrão a seguir:

   ```
   ncacn_http REG_SZ rpcrt4.dll
   ncacn_ip_tcp REG_SZ rpcrt4.dll
   ncacn_nb_tcp REG_SZ rpcrt4.dll
   ncacn_np REG_SZ rpcrt4.dll
   ncacn_ip_udp REG_SZ rpcrt4.dll
   ```

## <a name="more-information"></a>Mais informações

Exemplo de um nome incorreto para o mapeamento de IP que causa o erro de RPC 1753 vs.-2146893022: o nome principal de destino está incorreto

O domínio contoso.com consiste em DC1 e DC2 com os endereços IP x. x. 1.1 e x. x. 1.2. Os registros do host "A"/"AAAA" para DC2 estão corretamente registrados em todos os servidores DNS configurados para DC1. Além disso, o arquivo de HOSTs no DC1 contém um mapeamento de entrada DC2s nome de host totalmente qualificado para o endereço IP x. x. 1.2. Posteriormente, o endereço IP DC2's muda de X. X. 1.2 para X. X. 1.3 e um novo computador membro é adicionado ao domínio com o endereço IP X. 1.2. As tentativas de replicação do AD disparadas pelo comando **replicar agora** no snap-in Active Directory sites e serviços falham com o erro 1753, conforme mostrado no rastreamento abaixo:

```
F# SRC    DEST    Operation
1 x.x.1.1 x.x.1.2 ARP:Request, x.x.1.1 asks for x.x.1.2
2 x.x.1.2 x.x.1.1 ARP:Response, x.x.1.2 at 00-13-72-28-C8-5E
3 x.x.1.1 x.x.1.2 TCP:Flags=......S., SrcPort=50206, DstPort=DCE endpoint resolution(135)
4 x.x.1.2 x.x.1.1 ARP:Request, x.x.1.2 asks for x.x.1.1
5 x.x.1.1 x.x.1.2 ARP:Response, x.x.1.1 at 00-15-5D-42-2E-00
6 x.x.1.2 x.x.1.1 TCP:Flags=...A..S., SrcPort=DCE endpoint resolution(135)
7 x.x.1.1 x.x.1.2 TCP:Flags=...A...., SrcPort=50206, DstPort=DCE endpoint resolution(135)
8 x.x.1.1 x.x.1.2 MSRPC:c/o Bind: UUID{E1AF8308-5D1F-11C9-91A4-08002B14A0FA} EPT(EPMP)
9 x.x.1.2 x.x.1.1 MSRPC:c/o Bind Ack: Call=0x2 Assoc Grp=0x5E68 Xmit=0x16D0 Recv=0x16D0
10 x.x.1.1 x.x.1.2 EPM:Request: ept_map: NDR, DRSR(DRSR) {E3514235-4B06-11D1-AB04-00C04FC2DCD2} [DCE endpoint resolution(135)]
11 x.x.1.2 x.x.1.1 EPM:Response: ept_map: 0x16C9A0D6 - EP_S_NOT_REGISTERED
```

No quadro **10**, o DC de destino consulta o mapeador de ponto de extremidade DCS de origem pela porta 135 para o Active Directory UUID de classe de serviço de replicação E351...

No quadro **11**, o DC de origem, nesse caso, um computador membro que ainda não hospeda a função de controlador de domínio e, portanto, não registrou o E351... O UUID para o serviço de replicação com seu EPM local responde com o erro simbólico EP_S_NOT_REGISTERED que mapeia para o erro decimal 1753, erro hexadecimal 0x6d9 e erro amigável "não há mais pontos de extremidade disponíveis no mapeador de Endpoint".

Posteriormente, o computador membro com o endereço IP x. x. 1.2 é promovido como uma réplica "MayberryDC" no domínio contoso.com. Novamente, o comando **replicate Now** é usado para disparar a replicação, mas esse tempo falha com o erro na tela "o nome principal de destino está incorreto". O computador cujo adaptador de rede é atribuído o endereço IP x. x. 1.2 é um controlador de domínio, atualmente é inicializado no modo normal e registrou o E351... UUID do serviço de replicação com seu EPM local, mas ele não possui o nome ou a identidade de segurança de DC2 e não pode descriptografar a solicitação Kerberos do DC1 para que a solicitação agora falhe com o erro "o nome da entidade de destino está incorreto". O erro é mapeado para erro decimal-2146893022/erro hexadecimal 0x80090322.

Esses mapeamentos inválidos de host para IP podem ser causados por entradas obsoletas em arquivos do host/LMHOST, os registros do host A/AAAA no DNS ou WINS.

Resumo: Este exemplo falhou porque um mapeamento de host para IP inválido (no arquivo de HOST, nesse caso) fez com que o DC de destino fosse resolvido para um controlador de domínio de "origem" que não tenha o serviço de Active Directory Domain Services em execução (ou até mesmo instalado para esse assunto) para que o SPN de replicação não tenha sido registrado e o DC de origem retornou o erro 1753. No segundo caso, um mapeamento de host para IP inválido (novamente no arquivo de HOST) fez com que o DC de destino se conectasse a um controlador de domínio que registrou o E351... o SPN de replicação, mas essa origem tinha um nome de host e uma identidade de segurança diferentes do DC de origem pretendido, de modo que as tentativas falharam com Error-2146893022: o nome principal de destino está incorreto.

## <a name="related-topics"></a>Tópicos Relacionados

* [Solução de problemas Active Directory operações que falham com o erro 1753: não há mais pontos de extremidade disponíveis no mapeador de pontos de extremidades.](https://support.microsoft.com/kb/2089874)
* [Artigo 839880 da base de problemas ao solucionar erros do mapeador de ponto de extremidade RPC usando as ferramentas de suporte do Windows Server 2003 do CD do produto](https://support.microsoft.com/kb/839880)
* [Artigo 832017 do KB visão geral do serviço e requisitos de porta de rede para o sistema Windows Server](https://support.microsoft.com/kb/832017/)
* [Artigo 224196 da base de conhecimento restringindo o tráfego de replicação Active Directory e o tráfego RPC de cliente para uma porta específica](https://support.microsoft.com/kb/224196/)
* [Artigo 154596 da base de conhecimento como configurar a alocação de porta dinâmica RPC para trabalhar com firewalls](https://support.microsoft.com/kb/154596)
* [Como funciona o RPC](/windows/win32/rpc/how-rpc-works)
* [Como o servidor se prepara para uma conexão](/windows/win32/rpc/how-the-server-prepares-for-a-connection)
* [Como o cliente estabelece uma conexão](/windows/win32/rpc/how-the-client-establishes-a-connection)
* [Registrando a interface](/windows/win32/rpc/registering-the-interface)
* [Disponibilizando o servidor na rede](/windows/win32/rpc/making-the-server-available-on-the-network)
* [Registrando pontos de extremidade](/windows/win32/rpc/registering-endpoints)
* [Escutando chamadas de cliente](/windows/win32/rpc/listening-for-client-calls)
* [Como o cliente estabelece uma conexão](/windows/win32/rpc/how-the-client-establishes-a-connection)
* [Restringindo o tráfego de replicação Active Directory e o tráfego RPC de cliente para uma porta específica](https://support.microsoft.com/kb/224196)
* [SPN para um controlador de domínio de destino no AD DS](/openspecs/windows_protocols/ms-drsr/41efc56e-0007-4e88-bafe-d7af61efd91f)
