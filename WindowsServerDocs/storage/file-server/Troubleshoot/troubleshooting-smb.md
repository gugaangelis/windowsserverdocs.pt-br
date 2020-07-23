---
title: Solução de problemas avançada do protocolo SMB
description: Apresenta os métodos avançados de solução de problemas de protocolo SMB.
author: Deland-Han
manager: dcscontentpm
ms.topic: article
ms.author: delhan
ms.date: 12/25/2019
ms.openlocfilehash: 21b090e8e70f287e9609d28588403e3aa0988ce4
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86966298"
---
# <a name="advanced-troubleshooting-server-message-block-smb"></a>Solução de problemas avançada do protocolo SMB

O SMB é um protocolo de transporte de rede para operações de sistemas de arquivos para permitir que um cliente acesse recursos em um servidor. A principal finalidade do protocolo SMB é habilitar o acesso do sistema de arquivos remoto entre dois sistemas em TCP/IP.

A solução de problemas de SMB pode ser extremamente complexa. Este artigo não é um guia de solução de problemas completo, é uma breve introdução para entender as noções básicas de como solucionar problemas de SMB com eficiência.

## <a name="tools-and-data-collection"></a>Ferramentas e coleta de dados

Um aspecto importante da qualidade da solução de problemas de SMB é comunicar a terminologia correta. Portanto, este artigo apresenta a terminologia básica do SMB para garantir a precisão da coleta e análise de dados.

> [!Note]
> O *servidor SMB (SRV)* refere-se ao sistema que está hospedando o sistema de arquivos, também conhecido como o servidor de arquivos. *O cliente SMB (CLI)* refere-se ao sistema que está tentando acessar o sistema de arquivos, independentemente da versão ou edição do sistema operacional.

Por exemplo, se você usar o Windows Server 2016 para alcançar um compartilhamento SMB hospedado no Windows 10, o Windows Server 2016 será o cliente SMB e o Windows 10 o servidor SMB.

### <a name="collect-data"></a>Coletar dados

Antes de solucionar problemas de SMB, recomendamos que você primeiro colete um rastreamento de rede nos lados do cliente e do servidor. As seguintes diretrizes se aplicam:

- Em sistemas Windows, você pode usar o NetShell (netsh), Monitor de Rede, o analisador de mensagens ou o Wireshark para coletar um rastreamento de rede.

- Dispositivos de terceiros geralmente têm uma ferramenta de captura de pacote in-box, como tcpdump (Linux/FreeBSD/UNIX) ou pktt (NetApp). Por exemplo, se o cliente SMB ou o servidor SMB for um host UNIX, você poderá coletar dados executando o seguinte comando:
  
  ```cmd
  # tcpdump -s0 -n -i any -w /tmp/$(hostname)-smbtrace.pcap
  ```
  
  Pare de coletar dados usando **Ctrl + C** do teclado.

Para descobrir a origem do problema, você pode verificar os rastreamentos bilaterais: CLI, SRV ou em algum lugar entre eles.

#### <a name="using-netshell-to-collect-data"></a>Usando o NetShell para coletar dados

Esta seção fornece as etapas para usar o NetShell para coletar o rastreamento de rede.

> [!NOTE]  
> Um rastreamento netsh cria um arquivo ETL. Os arquivos ETL podem ser abertos somente no analisador de mensagens (MA) e Monitor de Rede 3,4 (defina o analisador para Monitor de Rede analisadores de análise \> ).

1. No servidor SMB e no cliente SMB, crie uma pasta **temporária** na unidade **C**. Em seguida, execute o seguinte comando:

   ```cmd
   netsh trace start capture=yes report=yes scenario=NetConnection level=5 maxsize=1024 tracefile=c:\\Temp\\%computername%\_nettrace.etl**
   ```
   
   Se você estiver usando o PowerShell, execute os seguintes cmdlets:
   
   ```PowerShell
   New-NetEventSession -Name trace -LocalFilePath "C:\Temp\$env:computername`_netCap.etl" -MaxFileSize 1024

   Add-NetEventPacketCaptureProvider -SessionName trace -TruncationLength 1500

   Start-NetEventSession trace
   ```
   
2. Reproduza o problema.

3. Pare o rastreamento executando o seguinte comando:

   ```cmd
   netsh trace stop
   ```
   
   Se você estiver usando o PowerShell, execute os seguintes cmdlets:

   ```PowerShell
   Stop-NetEventSession trace  
   Remove-NetEventSession trace
   ```

> [!NOTE] 
> Você deve rastrear apenas uma quantidade mínima dos dados transferidos. Para problemas de desempenho, sempre use um rastreamento bom e ruim, se a situação permitir.

### <a name="analyze-the-traffic"></a>Analisar o tráfego

O SMB é um protocolo de nível de aplicativo que usa TCP/IP como o protocolo de transporte de rede. Portanto, um problema SMB também pode ser causado por problemas de TCP/IP.

Verifique se o TCP/IP apresenta qualquer um desses problemas:

1. O handshake de três vias TCP não é concluído. Isso normalmente indica que há um bloqueio de firewall ou que o serviço do servidor não está em execução.

2. Retransmissãos estão ocorrendo. Isso pode causar transferências de arquivos lentas devido à limitação de congestionamento de TCP composta.

3. Cinco retransmissãos seguidas por uma redefinição de TCP podem significar que a conexão entre os sistemas foi perdida ou que um dos serviços SMB falhou ou parou de responder.

4. A janela de recepção TCP está diminuindo. Isso pode ser causado por um armazenamento lento ou por algum outro problema que impede que os dados sejam recuperados do buffer do Winsock do driver de função auxiliar (AFD).

Se não houver problema de TCP/IP perceptível, procure erros de SMB. Para fazer isso, siga estas etapas:

1. Sempre verifique os erros de SMB em relação à especificação do protocolo MS-SMB2. Muitos erros de SMB são benignos (não prejudiciais). Consulte as informações a seguir para determinar por que o SMB retornou o erro antes de concluir que o erro está relacionado a qualquer um dos seguintes problemas:

   - O tópico de [sintaxe da mensagem MS-SMB2](/openspecs/windows_protocols/ms-smb2/6eaf6e75-9c23-4eda-be99-c9223c60b181) detalha cada comando SMB e suas opções.
    
   - O tópico de [processamento de cliente MS-SMB2](/openspecs/windows_protocols/ms-smb2/df0625a5-6516-4fbe-bf97-01bef451cab2) detalha como o cliente SMB cria solicitações e responde a mensagens do servidor.

   - O tópico [processamento de servidor MS-SMB2](/openspecs/windows_protocols/ms-smb2/e1d08834-42e0-41ca-a833-fc26f5132a6f) detalha como o servidor SMB cria solicitações e responde às solicitações do cliente.

2. Verifique se um comando TCP Reset é enviado imediatamente após um \_ comando fsctl Validate \_ Negotiate \_ info (Validate Negotiate). Nesse caso, consulte as seguintes informações:

   - A sessão SMB deve ser encerrada (redefinição TCP) quando o processo de negociação de validação falha no cliente ou no servidor.

   - Esse processo pode falhar porque um otimizador de WAN está modificando o pacote de negociação SMB.

   - Se a conexão foi encerrada prematuramente, identifique a última comunicação do Exchange entre o cliente e o servidor.

#### <a name="analyze-the-protocol"></a>Analisar o protocolo

Examine os detalhes reais do protocolo SMB no rastreamento de rede para entender os comandos e as opções exatas que são usadas.

> [!NOTE]
> Somente o analisador de mensagem pode analisar os comandos SMBv3 e Version posterior.

- Lembre-se de que o SMB faz apenas o que é instruído a fazer.

- Você pode aprender muito sobre o que o aplicativo está tentando fazer examinando os comandos SMB.

Compare os comandos e as operações com a especificação de protocolo para garantir que tudo esteja operando corretamente. Se não estiver, colete dados que estejam mais próximos ou em um nível mais baixo para procurar mais informações sobre a causa raiz. Para fazer isso, siga estas etapas:

1. Colete uma captura de pacote padrão.

2. Execute o comando **netsh** para rastrear e coletar detalhes sobre se há problemas na pilha de rede ou Descartes em aplicativos da Windows Filtering Platform (WFP), como firewall ou programa antivírus.

3. Se todas as outras opções falharem, colete um t. cmd se você suspeitar que o problema ocorre no próprio SMB ou se nenhum dos outros dados for suficiente para identificar uma causa raiz.

Por exemplo:

- Você experimenta transferências de arquivos lentas para um único servidor de arquivos.

- Os rastreamentos em duas laterais mostram que o SRV responde lentamente a uma solicitação de leitura.

- A remoção de um programa antivírus resolve as transferências de arquivos lentas.

- Você entra em contato com o programa antivírus manufactory para resolver o problema.

> [!NOTE]
> Opcionalmente, você **também** pode desinstalar temporariamente o programa antivírus durante a solução de problemas.

#### <a name="event-logs"></a>Logs de eventos

Tanto o cliente SMB quanto o servidor SMB têm uma estrutura detalhada do log de eventos, conforme mostrado na captura de tela a seguir. Colete os logs de eventos para ajudar a encontrar a causa raiz do problema.

![Logs de eventos](media/troubleshooting-smb-1.png)

## <a name="smb-related-system-files"></a>Arquivos do sistema relacionados ao SMB

Esta seção lista os arquivos do sistema relacionados ao SMB. Para manter os arquivos do sistema atualizados, verifique se o [pacote cumulativo de atualizações](https://support.microsoft.com/help/4498140/windows-10-update-history) mais recente está instalado.

Binários de cliente SMB listados em **% windir% \\ System32 \\ drivers**:

- RDBSS.sys

- MRXSMB.sys

- MRXSMB10.sys

- MRXSMB20.sys

- MUP.sys

- SMBdirect.sys

Os binários do servidor SMB listados em **% windir% \\ System32 \\ drivers**:

- SRVNET.sys

- SRV.sys

- SRV2.sys

- SMBdirect.sys

- Em **% windir% \\ System32**

- srvsvc.dll

![Componentes SMB](media/troubleshooting-smb-2.png)

### <a name="update-suggestions"></a>Sugestões de atualização

Recomendamos que você atualize os seguintes componentes antes de solucionar problemas de SMB:

- Um servidor de arquivos requer armazenamento de arquivos. Se o seu armazenamento tiver o componente iSCSI, atualize esses componentes.

- Atualize os componentes de rede.

- Para melhorar o desempenho e a estabilidade, atualize o Windows Core.

## <a name="reference"></a>Referência

[Cenário de intercâmbio de pacotes do protocolo SMB da Microsoft](/windows/win32/fileio/microsoft-smb-protocol-packet-exchange-scenario)
