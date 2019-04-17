---
ms.assetid: 6086947f-f9ef-4e18-9f07-6c7c81d7002c
title: Ferramentas de serviço de tempo do Windows e configurações
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: networking
ms.openlocfilehash: 70b7ee4a9955e023d1664a3c29295a22cd5dc75b
ms.sourcegitcommit: fd6a46b702b235f38d90814dd769106ab37cd61b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/15/2018
---
# <a name="windows-time-service-tools-and-settings"></a>Ferramentas de serviço de tempo do Windows e configurações

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


**Nesta seção**  
  
-   [Ferramentas de serviço de tempo do Windows](#w2k3tr_times_tools_dyax)  
  
-   [Entradas de registro de serviço de tempo do Windows](#w2k3tr_times_tools_uhlp)  
  
-   [Configurações de política de grupo de serviços de tempo do Windows](#w2k3tr_times_tools_vwtt)  
  
-   [Portas de rede usadas pelo serviço de tempo do Windows](#w2k3tr_times_tools_suxb)  
  
-   [Informações relacionadas](#w2k3tr_times_tools_qoep)  
  
> [!NOTE]  
> Este tópico contém informações somente sobre ferramentas e configurações para o serviço de tempo do Windows (W32Time). Se você quiser sincronizar o tempo para um computador cliente ingressado no domínio, consulte [configurar um computador cliente para sincronização de tempo de domínio automática](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816884%28v%3dws.10%29). Para tópicos adicionais sobre como configurar o serviço de tempo do Windows, consulte a lista de tópicos na seção [onde encontrar informações de configuração serviço de tempo de Windows](https://technet.microsoft.com/library/cc773061.aspx).  
  
> [!CAUTION]  
> Você não deve usar o comando Net time para configurar ou definir o tempo quando o serviço de tempo do Windows está em execução.  

Além disso, em computadores mais antigos que executam o Windows XP ou anterior, o período de líquido comando /querysntp exibe o nome de um servidor de rede NTP (Time Protocol) com o qual um computador está configurado para sincronizar, mas esse servidor NTP é usado somente quando o cliente de hora do computador está configurado como NTP ou AllSync. Esse comando foi preterido desde então.  
  
A maioria dos computadores membros do domínio têm um tipo de cliente do tempo de NT5DS, o que significa que eles sincronizem o tempo desde a hierarquia de domínio. A exceção apenas típica é o controlador de domínio que funciona como o mestre de operações de emulador domínio primário (PDC) do controlador de domínio raiz da floresta, que geralmente é configurado para sincronizar o tempo com uma fonte de horário externo. Para exibir a configuração de cliente do tempo de um computador, execute o W32tm//query /configuration comando em um Prompt de comando com privilégios elevados na partir do Windows Server 2008 e Windows Vista e ler o **tipo** linha na saída do comando. Para obter mais informações, consulte [como tempo serviço funciona Windows ](https://go.microsoft.com/fwlink/?LinkId=117753). Você pode executar o comando **reg consulta HKLM\SYSTEM\CurrentControlSet\Services\W32Time\Parameters** e ler o valor de **NtpServer** na saída do comando.  
  
> [!IMPORTANT]  
> Antes do Windows Server 2016, o serviço W32Time não foi projetado para atender às necessidades do aplicativo de detecção de hora.  No entanto, as atualizações para Windows Server 2016 agora permitem que você implemente uma solução para 1 MS precisão em seu domínio.  Consulte [Windows 2016 precisão tempo](accurate-time.md) e [limite de suporte para configurar o serviço de tempo do Windows para ambientes de alta precisão](https://go.microsoft.com/fwlink/?LinkID=179459) para obter mais informações.  
  
## <a name="w2k3tr_times_tools_dyax"></a>Ferramentas de serviço de tempo do Windows  
As ferramentas a seguir estão associadas com o serviço de tempo do Windows.  
  
#### <a name="w32tmexe-windows-time"></a>W32tm.exe: tempo do Windows  
**Categoria**  

Essa ferramenta é instalada como parte do Windows XP, Windows Vista, Windows 7, Windows Server 2003, Windows Server 2003 R2, Windows Server 2008 e instalações do Windows Server 2008 R2 padrão.  
  
**Compatibilidade de versão**  
  
Essa ferramenta funciona no Windows XP, Windows Vista, Windows 7, Windows Server 2003, Windows Server 2003 R2, Windows Server 2008 e instalações do Windows Server 2008 R2 padrão.  
  
W32tm.exe é usado para definir as configurações de serviço de tempo do Windows. Ele também pode ser usado para diagnosticar problemas com o serviço de tempo. W32tm.exe é a ferramenta de linha de comando preferencial para configurar, monitoramento ou solução de problemas do serviço de tempo do Windows.  
  
As tabelas a seguir descrevem os parâmetros que são usados com W32tm.exe.  
  
**Parâmetros principais W32tm.exe**  
  
|Parâmetro|Descrição|  
|-------------|---------------|  
|W32tm /?|Ajuda da linha de comando w32tm|  
|W32tm//register|Registra o serviço de tempo para ser executado como um serviço e adiciona a configuração padrão ao registro.|  
|W32tm//unregister|O serviço de tempo cancela o registro e remove todas as informações de configuração do registro.|  
|w32tm//monitor<br /><br />[/domain:<domain name>] [/computers:<name>[,<name>[,<name>...]]] [/Threads:<num>]|Domínio - especifica qual domínio para monitorar. Se nenhum nome de domínio for fornecido, ou o domínio nem computadores opção é especificada, o domínio padrão é usado. Essa opção pode ser usada mais de uma vez.<br /><br />Computadores - monitora a lista fornecida de computadores. Nomes de computador são separados por vírgulas, sem espaços. Se um nome é prefixado com um ' *', ele é tratado como um PDC. Essa opção pode ser usada mais de uma vez.<br /><br />Threads - Especifica o número de computadores a serem analisados simultaneamente. O valor padrão é 3. Intervalo permitido é 1 a 50.|  
|w32tm /ntte <NT time epoch>|Converter uma hora do sistema NT, em (10 ^ -7) s intervalos de 0 h 1 - janeiro de 1601, em um formato legível.|  
|w32tm /ntpte <NTP time epoch>|Converter um horário NTP, em (2 ^ -32) s intervalos de 0 h 1 - Jan de 1900, em um formato legível.|  
|w32tm//resync<br /><br />[/computer:<computer>]<br /><br />[/nowait]<br /><br />[/Rediscover]<br /><br />[/Soft]|Informe um computador que ele deve sincronizar novamente seu relógio assim que possível, eliminando todas as estatísticas de erro acumuladas.<br /><br />computador:<computer> -Especifica o computador deve sincronizar novamente. Se não for especificado, o computador local será sincronizar novamente.<br /><br />Nowait - não Aguarde ressincronizar ocorram; retorne imediatamente. Caso contrário, aguarde o ressincronizar ser concluída antes de retornar.<br /><br />Redescubra - detectar novamente a configuração de rede e redescobrir as fontes de rede e ressincronizar.<br /><br />Soft - Ressincronize usando estatísticas de erro existentes. Não é útil, fornecido para compatibilidade.|  
|w32tm /stripchart<br /><br />/computer:<target><br /><br />[/Period:<refresh>]<br /><br />[/dataonly]<br /><br />[/Samples:<count>]<br/><br/>[/rdtsc]<br/>|Exiba um gráfico do deslocamento entre esse computador e outro computador.<br /><br />computador:<target> -o computador para medir o deslocamento em relação.<br /><br />período:<refresh> - o tempo entre os exemplos, em segundos. O padrão é 2s.<br /><br />Dataonly - exibir apenas os dados sem elementos gráficos.<br /><br />Exemplos:<count> - coletar <count>amostras e parar. Se não for especificado, as amostras são coletadas até **Ctrl + C** é pressionado.<br/><br/>RDTSC: para cada exemplo, essa opção imprime valores separados por vírgula juntamente com os cabeçalhos RdtscStart, RdtscEnd, FileTime, RoundtripDelay, NtpOffset em vez do gráfico de texto.<br/><ul><li>[RdtscStart – RDTSC (leitura TimeStamp contador)](https://en.wikipedia.org/wiki/Time_Stamp_Counter) valor coletados antes que a solicitação NTP foi gerada.</li><li>RdtscEnd – valor RDTSC (leitura TimeStamp contador) coletado apenas depois que a resposta NTP foi recebida e processada.</li><li>FileTime – valor FILETIME Local usado na solicitação NTP.</li><li>RoundtripDelay – o tempo decorrido em segundos entre gera a solicitação NTP e processamento da resposta recebida do NTP, calculado em conformidade com cálculos de ida e volta NTP.</li><li>NTPOffset – tempo deslocamento em segundos entre o computador local e o servidor NTP, calculado em conformidade com cálculos de deslocamento NTP.</li></ul>|
|w32tm /config<br /><br />[/computer:<target>]<br /><br />[/Update]<br /><br />[/manualpeerlist:<peers>]<br /><br />[/syncfromflags:<source>]<br /><br />[/LocalClockDispersion:<seconds>]<br /><br />[/Reliable: (Sim & #124; não)]<br /><br />[/largephaseoffset:<milliseconds>]|computador:<target> -ajusta a configuração de <target>. Se não for especificado, o padrão é o computador local.<br /><br />Update - notifica o serviço de tempo que a configuração foi alterada, fazendo com que as alterações entrem em vigor.<br /><br />manualpeerlist:<peers> -define a lista de par manual <peers>, que é uma lista delimitada por espaços de endereços DNS e/ou IP. Ao especificar vários pares, essa opção deve ser entre aspas.<br /><br />syncfromflags:<source> -define que o cliente NTP deve sincronizar a partir de fontes. <source> Deve ser uma lista separada por vírgulas dessas palavras-chave (não diferencia maiusculas de minúsculas):<br /><br />MANUAL - inclui pares da lista manual de par.<br /><br />DOMHIER - sincronizar a partir de um controlador de domínio (DC) na hierarquia de domínios.<br /><br />LocalClockDispersion:<seconds> -configura a precisão de internal relógio W32Time assumirá quando ele não é possível adquirir o tempo desde que ele tenha configurado fontes.<br /><br />confiável: (Sim & #124; não) – defina se este computador é uma fonte de tempo confiável.<br /><br />Essa configuração só é significativa em controladores de domínio.<br /><br />Sim - este computador é um serviço de tempo confiável.<br /><br />NÃO - este computador não é um serviço de tempo confiável.<br /><br />largephaseoffset:<milliseconds> -define a diferença de tempo entre o local e o tempo que W32Time considerarão um aumento de rede.|  
|w32tm /tz|Exiba as configurações de fuso horário atual.|  
|w32tm /dumpreg<br /><br />[/Subkey:<key>]<br /><br />[/computer:<target>]|Exiba os valores associados a uma determinada chave do registro.<br /><br />A chave padrão é HKLM\System\CurrentControlSet\Services\W32Time<br /><br />(a chave de raiz para o serviço de tempo).<br /><br />subchave:<key> -exibe os valores associados a subchave <key>da chave padrão.<br /><br />computador:<target> -consulta configurações do registro para computador <target>|  
|w32tm//query [/computer:<target>] {/source & #124; /configuration & #124; /Peers & #124; /status} [/verbose]|Este parâmetro foi disponibilizado pela primeira vez em que as versões de cliente de tempo do Windows do Windows Vista e Windows Server 2008.<br /><br />Exiba informações de serviço de tempo do Windows do computador.<br /><br />**computador:<target>** -consultar as informações de **<target>**. Se não for especificado, o valor padrão é o computador local.<br /><br />**Origem** -exibir a fonte de tempo.<br /><br />**Configuração** -exibir a configuração do tempo de execução e onde a configuração é proveniente. No modo detalhado, exibe o indefinida ou não utilizados configuração também.<br /><br />**pares** -exibir uma lista de pares e seu status.<br /><br />**status** -status do serviço de tempo do Windows de exibição.<br /><br />**detalhada** -definir o modo detalhado para exibir mais informações.|  
|w32tm//debug {/disable & #124; {/Enable /file:<name> /size:<bytes> /entries:<value> [/truncate]}}|Este parâmetro foi disponibilizado pela primeira vez em que as versões de cliente de tempo do Windows do Windows Vista e Windows Server 2008.<br /><br />Habilite ou desabilite o log privado do serviço de tempo do Windows do computador local.<br /><br />**Desabilitar** -desabilitar o log privado.<br /><br />**Habilitar** -habilitar o log privado.<br /><br />-   **arquivo:<name>** -especificar o nome de arquivo absolutos.<br />-   **Tamanho:<bytes>** -especificar o tamanho máximo de registro em log circular.<br />-   **entradas:<value>** -contém uma lista dos sinalizadores, especificadas pelo número e separados por vírgulas, que especificam os tipos de informações que deve ser registrada. Números válidos são de 0 a 300. Um intervalo de números é válido, além de números únicos, como 0-100,103 106. Valor de 0 a 300 é para todas as informações de registro em log.<br /><br />**trunca** -trunca o arquivo se ele existir.|  
  
Para saber mais sobre **W32tm.exe**, consulte o Centro de Ajuda e suporte no Windows XP, Windows Vista, Windows 7, Windows Server 2003, Windows Server 2003 R2, Windows Server 2008 e Windows Server 2008 R2.  
  
## <a name="w2k3tr_times_tools_uhlp"></a>Entradas de registro de serviço de tempo do Windows  
As seguintes entradas do registro estão associadas com o serviço de tempo do Windows.  
  
Essa informação é fornecida como uma referência para uso em solução de problemas ou verificando que as configurações necessárias sejam aplicadas. É recomendável que você não diretamente editar o registro a menos que não haja nenhuma outra alternativa. Modificações no registro não são validadas pelo editor do registro ou pelo Windows antes que eles são aplicados e como resultado, os valores incorretos podem ser armazenados. Isso pode resultar em erros irrecuperáveis no sistema.  
  
Quando possível, use a política de grupo ou outras ferramentas do Windows, como o Console de gerenciamento Microsoft (MMC), para realizar tarefas em vez de edição do registro diretamente. Se você deve editar o registro, tenha muito cuidado.  
  
> [!WARNING]  
> Alguns dos valores predefinidos que são configurados no sistema administrativas modelo arquivo (System.adm) para as configurações de política de grupo (GPO) do objeto são diferentes das entradas do registro correspondentes padrão. Se você pretende usar um GPO para definir qualquer configuração de tempo do Windows, certifique-se de que você reveja [predefinição valores para as configurações de política de grupo de serviço de tempo do Windows são diferentes das entradas de registro de serviço de tempo do Windows correspondentes no Windows Server 2003](https://go.microsoft.com/fwlink/?LinkId=186066). Esse problema se aplica ao Windows Server 2008 R2, Windows Server 2008, Windows Server 2003 R2 e Windows Server 2003.  
  
Muitas entradas do registro para o serviço de tempo do Windows são iguais a configuração de política de grupo de mesmo nome. As configurações de política de grupo correspondem às entradas do registro de mesmo nome localizado em:  
  
>**HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\\**

  
Há várias chaves do registro neste local do registro. As configurações de tempo do Windows são armazenadas nos valores em todas essas chaves:
* [Parâmetros](#Parameters)
* [Config](#Configuration)
* [NtpClient](#NtpClient)
* [NtpServer](#NtpServer)
  
> [!NOTE]  
> Muitos dos valores na seção W32Time do registro são usados internamente pelo W32Time para armazenar informações. Esses valores não devem ser alterados manualmente a qualquer momento. Não modifique as configurações nesta seção, a menos que você esteja familiarizado com a configuração e tiver certeza de que o novo valor funcionará conforme esperado. As seguintes entradas do registro estão localizadas em:

>>**HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\\**  
  
Alguns dos parâmetros são armazenados em marcas de escala de relógio no registro e alguns são em segundos. Para converter o tempo de marcas de escala de relógio em segundos:  
  
-   1 minuto = 60 s  
  
-   1 s = 1000 ms  
  
-   1 ms = 10.000 escalas de relógio em um sistema Windows, conforme descrito em [DateTime.Ticks propriedade ](https://msdn.microsoft.com/en-us/library/system.datetime.ticks.aspx).  
  
Por exemplo, 5 minutos ficariam 5 * 60\ * 1000\ * 10000 = 3000000000 escalas de relógio. 

Todas as versões incluem o Windows 7, Windows 8, Windows 10, Windows Server 2008 e Windows Server 2008 R2, Windows Server 2012, Windows Server 2012R2, Windows Server 2016.  Algumas entradas só estão disponíveis em versões mais recentes do Windows.


#### <a name="Parameters"></a>HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\Parameters

|Entrada do registro|Versão|Descrição|
|------------------------------------|---------------|----------------------------|
|AllowNonstandardModeCombinations|Todos os|Essa entrada indica que as combinações de modo não padrão são permitidas em sincronização entre pares. O valor padrão para membros do domínio é 1. O valor padrão para clientes independentes e servidores é 1.|
|NtpServer|Todos os|Essa entrada especifica uma lista delimitada por espaço dos pares de que um computador obtém carimbos de data / hora, que consiste em um ou mais nomes DNS ou endereços IP por linha. Cada nome DNS ou o endereço IP listado deve ser exclusivo. Computadores conectados a um domínio devem sincronizar com uma fonte de tempo mais confiável, como o relógio EUA oficial.  <ul><li>0x01 SpecialInterval </li><li>0x02 UseAsFallbackOnly</li><li>0x04 SymmetricActive - para obter mais informações sobre esse modo, consulte [servidor de horário do Windows: 3.3 modos de operação ](https://go.microsoft.com/fwlink/?LinkId=208012).</li><li>0x08 cliente</li></ul><br />Não há nenhum valor padrão para essa entrada do registro em membros do domínio. O valor padrão em clientes independentes e servidores é time.windows.com, 0x1.<br /><br />Observação: Para obter mais informações sobre servidores NTP disponíveis, consulte [artigo da Base de Conhecimento Microsoft 262680 - uma lista dos servidores de tempo simples (SNTP Network Time Protocol) que estão disponíveis na Internet](https://go.microsoft.com/fwlink/?LinkId=186067)|
|ServiceDll|Todos os|Essa entrada é mantida pelo W32Time. Ele contém dados reservados que são usados pelo sistema operacional Windows, e todas as alterações essa configuração podem causar resultados imprevisíveis. O local padrão para esse DLL membros do domínio e clientes independentes e servidores é windir%\System32\W32Time.dll %.  |
|ServiceMain|Todos os|Essa entrada é mantida pelo W32Time. Ele contém dados reservados que são usados pelo sistema operacional Windows, e todas as alterações essa configuração podem causar resultados imprevisíveis. O valor padrão em membros do domínio é SvchostEntry_W32Time. O valor padrão em clientes independentes e servidores é SvchostEntry_W32Time.  "|
|Tipo|Todos os|Essa entrada indica que pares para aceitar a sincronização de:  <ul><li>**NoSync**. O serviço de tempo não sincroniza com outras fontes.</li><li>**NTP.** O serviço de tempo sincroniza dos servidores especificados no **NtpServer**. Entrada do registro.</li><li>**Nt5DS**. O serviço de tempo sincroniza da hierarquia de domínio.  </li><li>**AllSync**. O serviço de tempo usa todos os mecanismos de sincronização disponíveis.  </li></ul>O valor padrão em membros do domínio é **NT5DS**. O valor padrão em clientes autônomos e servidores é **NTP**.   |

#### <a name="Configuration"></a>HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\Config

|Entrada do registro|Versão|Descrição|
|------------------------------------|---------------|----------------------------|
|AnnounceFlags|Todos os|Essa entrada controla se este computador está marcado como um servidor de horário confiável. Um computador não está marcado como confiável, a menos que ele também está marcado como um servidor de horário.<br /> -0x00 não é um servidor de horário  <br /> -servidor de horário 0x01 sempre  <br /> -servidor de horário automático 0x02  <br /> -servidor de horário confiável na 0x04 sempre  <br /> -servidor de horário confiável automático 0x08  <br />O valor padrão para membros do domínio é 10. O valor padrão para clientes independentes e servidores é 10.|
|EventLogFlags|Todos os|Essa entrada controla os eventos que registra em log o serviço de tempo.  <br />-Tempo atalhos: 0x1  <br />-Alteração de origem: 0x2  <br />O valor padrão em membros do domínio é 2. O valor padrão em clientes independentes e servidores é 2.  |
|FrequencyCorrectRate|Todos os|Essa entrada controla a taxa na qual o relógio foi corrigido. Se esse valor for muito pequeno, o relógio está instável e overcorrects. Se o valor for muito grande, o relógio leva muito tempo para sincronizar. O valor padrão em membros do domínio é 4. O valor padrão em clientes independentes e servidores é 4.  <br /><br />Observe que 0 é um valor inválido para a entrada do registro FrequencyCorrectRate. No Windows Server 2003, Windows Server 2003 R2, Windows Server 2008 e Windows Server 2008 R2 computadores, se o valor for definido como 0 o serviço de tempo do Windows será alterada automaticamente-lo como 1.  |
|HoldPeriod|Todos os|Essa entrada controla o período de tempo para o qual a detecção de pico é desabilitada para trazer o relógio do local para sincronização rapidamente. Um pico é um exemplo de tempo indicando que esse tempo é desativar um número de segundos e geralmente é recebido, depois de exemplos de boa hora devolvidos consistentemente. O valor padrão em membros do domínio é 5. O valor padrão em clientes independentes e servidores é 5.  |
|LargePhaseOffset|Todos os|Essa entrada especifica que uma vez deslocamento maior ou igual a esse valor em 10<sup>-7</sup> segundos é considerado um pico. Uma interrupção de rede, como uma grande quantidade de tráfego pode causar um aumento. Um pico será ignorado, a menos que ela persista por um longo período de tempo. O valor padrão em membros do domínio é 50000000. O valor padrão em clientes independentes e servidores é 50000000.  |
|LastClockRate|Todos os|Essa entrada é mantida pelo W32Time. Ele contém dados reservados que são usados pelo sistema operacional Windows, e todas as alterações essa configuração podem causar resultados imprevisíveis. O valor padrão em membros do domínio é 156250. O valor padrão em clientes independentes e servidores é 156250.  |
|LocalClockDispersion|Todos os|Essa entrada controla a dispersão (em segundos) que você deve considerar quando o tempo único origem é o relógio CMOS interno. O valor padrão em membros do domínio é 10. O valor padrão em clientes independentes e servidores é 10.|
|MaxAllowedPhaseOffset|Todos os|Essa entrada especifica a diferença máxima (em segundos) para o qual W32Time tenta ajustar o relógio do computador usando a velocidade do relógio. Quando o deslocamento excede essa taxa, W32Time define o relógio do computador diretamente. O valor padrão para membros do domínio é 300. O valor padrão para clientes independentes e servidores é 1.  [Veja a seguir para obter mais informações](#MaxAllowedPhaseOffset).|
|MaxClockRate|Todos os|Essa entrada é mantida pelo W32Time. Ele contém dados reservados que são usados pelo sistema operacional Windows, e todas as alterações essa configuração podem causar resultados imprevisíveis. O valor padrão para membros do domínio é 155860. O valor padrão para clientes independentes e servidores é 155860.  |
|MaxNegPhaseCorrection|Todos os|Essa entrada especifica a maior correção negativa em segundos que faz com que o serviço. Se o serviço determina que uma alteração maior do que isso é necessária, ele registra um evento. Caso especial: 0xFFFFFFFF significa sempre fazer a correção do tempo. O valor padrão para membros do domínio é 0xFFFFFFFF. O valor padrão para clientes independentes e servidores é 54.000 (15 horas).  |
|MaxPollInterval|Todos os|Essa entrada especifica o maior intervalo, em segundos log2, permitido para o intervalo de sondagem do sistema. Observe que enquanto um sistema deve sondar de acordo com o intervalo programado, um provedor pode se recusar a produzir amostras quando solicitado para fazer isso. O valor padrão para controladores de domínio é 10. O valor padrão para membros do domínio é 15. O valor padrão para clientes independentes e servidores é 15.  |
|MaxPosPhaseCorrection|Todos os|Essa entrada especifica a maior correção positiva tempo em segundos que faz com que o serviço. Se o serviço determina que uma alteração maior do que isso é necessária, ele registra um evento. Caso especial: 0xFFFFFFFF significa sempre fazer a correção do tempo. O valor padrão para membros do domínio é 0xFFFFFFFF. O valor padrão para clientes independentes e servidores é 54.000 (15 horas).  |
|MinClockRate|Todos os|Essa entrada é mantida pelo W32Time. Ele contém dados reservados que são usados pelo sistema operacional Windows, e todas as alterações essa configuração podem causar resultados imprevisíveis. O valor padrão para membros do domínio é 155860. O valor padrão para clientes independentes e servidores é 155860.  |
|MinPollInterval|Todos os|Essa entrada especifica o intervalo mínimo, em segundos log2, permitido para o intervalo de sondagem do sistema. Observe que, enquanto um sistema não solicitar amostras com mais frequência do que isso, um provedor pode produzir amostras às vezes que não seja o intervalo agendada. O valor padrão para controladores de domínio é 6. O valor padrão para membros do domínio é 10. O valor padrão para clientes independentes e servidores é 10.  |
|PhaseCorrectRate|Todos os|Essa entrada controla a taxa na qual o erro de fase seja corrigido. Especificar um valor pequeno corrige o erro de fase rapidamente, mas pode fazer com que o relógio instável. Se o valor for muito grande, leva mais tempo para corrigir o erro de fase. <br /><br />O valor padrão em membros do domínio é 1. O valor padrão em clientes independentes e servidores é 7.<br /><br />Observação: 0 é um valor inválido para a entrada do registro PhaseCorrectRate. No Windows Server 2003, Windows Server 2003 R2, Windows Server 2008 e Windows Server 2008 R2 computadores, se o valor for definido como 0, o serviço de tempo do Windows automaticamente altera para 1.  |
|PollAdjustFactor|Todos os|Essa entrada controla a decisão para aumentar ou diminuir o intervalo de sondagem para o sistema. Quanto maior o valor, menor a quantidade de erro que faz com que o intervalo de sondagem ser reduzidas. O valor padrão em membros do domínio é 5. O valor padrão em clientes independentes e servidores é 5. |
|SpikeWatchPeriod|Todos os|Essa entrada especifica a quantidade de tempo que um deslocamento suspeito deve persistir antes que ele é aceito como correta (em segundos). O valor padrão em membros do domínio é 900. O valor padrão em clientes autônomos e estações de trabalho é 900.  |
|TimeJumpAuditOffset|Todos os|Um inteiro não assinado que indica o limite de auditoria de salto de tempo, em segundos. Se o serviço de tempo ajusta o relógio local, definindo o relógio diretamente, e a correção de tempo é mais do que esse valor, o serviço de tempo registra um evento de auditoria.|
|UpdateInterval|Todos os|Essa entrada especifica o número de marcas de escala de relógio entre os ajustes de correção de fase. O valor padrão para controladores de domínio é 100. O valor padrão para membros do domínio é 30.000. O valor padrão para clientes independentes e servidores é 360.000.  <br /><br />**Observação**: Zero é um valor inválido para a entrada do registro UpdateInterval. Em computadores que executam o Windows Server 2003, Windows Server 2003 R2, Windows Server 2008 e Windows Server 2008 R2, se o valor for definido como 0 o serviço de tempo do Windows automaticamente altera para 1.<br /><br />As seguintes entradas do três registro não fazem parte da configuração W32Time padrão, mas podem ser adicionadas ao registro para obter os recursos de registro em log maior. As informações registradas no log de evento do sistema podem ser modificadas alterando o valor da configuração de EventLogFlags no Editor de objeto de diretiva de grupo. Por padrão, o serviço de tempo cria um log no Visualizador de eventos sempre que ele alterna para uma nova fonte de tempo.<br /><br />**Aviso**: alguns dos valores predefinidos que são configurados no sistema administrativas modelo arquivo (System.adm) para as configurações de política de grupo (GPO) do objeto são diferentes dos correspondente padrão entradas do registro. Se você pretende usar um GPO para definir qualquer configuração de tempo do Windows, certifique-se de que você reveja [predefinição valores para as configurações de política de grupo de serviço de tempo do Windows são diferentes das entradas de registro de serviço de tempo do Windows correspondentes no Windows Server 2003](https://go.microsoft.com/fwlink/?LinkId=186066). Esse problema se aplica ao Windows Server 2008 R2, Windows Server 2008, Windows Server 2003 R2 e Windows Server 2003. |
|UtilizeSslTimeData|Lançar o Windows 10 versão 1511|Entrada de 1 indica que o W32Time usará vários carimbos SSL para propagar um relógio que é bastante impreciso.|

As seguintes entradas do registro devem ser adicionadas para habilitar o registro em log W32Time:  

|Entrada do registro|Versão|Descrição|
|------------------------------------|---------------|----------------------------|
|FileLogEntries|Todos os|Essa entrada controla a quantidade de entradas criadas no arquivo de log de tempo do Windows. O valor padrão é none, que não registra qualquer atividade de tempo do Windows. Valores válidos são de 0 a 300. Esse valor não afeta as entradas do log de eventos normalmente criadas pelo tempo do Windows|
|FileLogName|Todos os|Essa entrada controla o local e nome de arquivo do log de tempo do Windows. O valor padrão é em branco e não deve ser alterado, a menos que **FileLogEntries** é alterada. Um valor válido é um caminho completo e o nome de arquivo que o tempo do Windows usará para criar o arquivo de log. Esse valor não afeta as entradas do log de eventos normalmente criadas pelo tempo do Windows.  |
|FileLogSize|Todos os|Essa entrada controla o comportamento de registro em log circular dos arquivos de log de tempo do Windows. Quando **FileLogEntries** e **FileLogName** são definidos, essa entrada define o tamanho, em bytes, para permitir que o arquivo de log alcançar antes de sobrescrever as entradas de log mais antigas com novas entradas. Qualquer número positivo é válido e 3000000 é recomendado. Esse valor não afeta as entradas do log de eventos normalmente criadas pelo tempo do Windows.  |



#### <a name="NtpClient"></a>HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\NtpClient

|Entrada do registro|Versão|Descrição|
|------------------------------------|---------------|----------------------------|
|AllowNonstandardModeCombinations|Todos os|Essa entrada indica que as combinações de modo não padrão são permitidas em sincronização entre pares. O valor padrão para membros do domínio é 1. O valor padrão para clientes independentes e servidores é 1.|
|CompatibilityFlags|Todos os|Essa entrada especifica os seguintes sinalizadores de compatibilidade e valores: <br /><br />-DispersionInvalid: 0x00000001  <br />-IgnoreFutureRefTimeStamp: 0x00000002  <br /> -AutodetectWin2K: 0x80000000  <br />-AutodetectWin2KStage2: 0x40000000  <br /><br />O valor padrão para membros do domínio é 0x80000000. O valor padrão para clientes independentes e servidores é 0x80000000.  |
|CrossSiteSyncFlags|Todos os|Essa entrada determina se o serviço escolhe parceiros de sincronização fora do domínio do computador. Os valores e as opções são:  <br /><br />-None: 0  <br />-PdcOnly: 1  <br />-Todos os itens: 2  <br /><br />Esse valor será ignorado se o valor NT5DS não for definido. O valor padrão para membros do domínio é 2. O valor padrão para clientes independentes e servidores é 2.  |
|DllName|Todos os|Essa entrada especifica o local da DLL para o provedor de tempo.  <br /><br />O local padrão para esse DLL membros do domínio e clientes independentes e servidores é windir%\System32\W32Time.dll %.|
|Habilitado|Todos os|Essa entrada indica se o provedor NtpClient está habilitado no serviço de tempo atual.  <br /><ul><li>Sim 1</li><li>0 não</li></ul>O valor padrão em membros do domínio é 1. O valor padrão em clientes independentes e servidores é 1.|
|EventLogFlags|Todos os|Essa entrada especifica os eventos registrados pelo serviço de tempo do Windows.<ul><li>Alterações de acessibilidade 0x1</li><li>0x2 amostras grandes inclinação (Isso é aplicável ao Windows Server 2003, Windows Server 2003 R2, Windows Server 2008 e Windows Server 2008 R2 somente)</li></ul>O valor padrão em membros do domínio é 0x1. O valor padrão em clientes independentes e servidores é 0x1.|
|InputProvider|Todos os|Essa entrada indica se o provedor NtpClient está habilitado.  <ul><li>Sim 1  </li><li>0 não </li></ul>O valor padrão em membros do domínio é 1. O valor padrão em clientes independentes e servidores é 1.  |
|LargeSampleSkew|Todos os|Essa entrada especifica a inclinação amostra grande para fazer logon em segundos. Para estar em conformidade com as especificações de segurança e Exchange Commission (s), ele deve ser definido para três segundos. Eventos serão registrados para essa configuração somente quando EventLogFlags explicitamente está configurado para a inclinação da amostra grande 0x2. O valor padrão em membros do domínio é 3. O valor padrão em clientes independentes e servidores é 3.  |
|ResolvePeerBackOffMaxTimes|Todos os|Essa entrada especifica o número máximo de vezes para duas vezes o intervalo de espera quando repetidos tenta localizar um par para sincronizar com falha. Um valor de zero significa que o intervalo de espera é sempre o mínimo. O valor padrão em membros do domínio é 7. O valor padrão em clientes independentes e servidores é 7. |
|ResolvePeerBackoffMinutes|Todos os|Essa entrada especifica o intervalo inicial de espera, em minutos, antes de tentar localizar um par para sincronizar com. O valor padrão em membros do domínio é 15. O valor padrão em clientes independentes e servidores é 15.  |
|SpecialPollInterval|Todos os|Essa entrada especifica o intervalo de sondagem especiais em segundos por pares manuais. Quando o sinalizador SpecialInterval 0x1 é ativado, W32Time usa esse intervalo de sondagem em vez de um intervalo de sondagem determinar pelo sistema operacional. O valor padrão em membros do domínio é 3600. O valor padrão em clientes independentes e servidores é 604.800.<br/><br/>Novo para compilação 1702, SpecialPollInterval está contida pelos valores do registro MinPollInterval e MaxPollInterval Config.|
|SpecialPollTimeRemaining|Todos os|Essa entrada é mantida pelo W32Time. Ele contém dados reservados que são usados pelo sistema operacional Windows. Ele especifica o tempo em segundos, antes de W32Time sincronizar novamente depois que o computador for reiniciado. Todas as alterações essa configuração podem causar resultados imprevisíveis. O valor padrão em ambos os membros do domínio e em servidores e clientes autônomos for deixado em branco.  |

#### <a name="NtpServer"></a>HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\NtpServer

|Entrada do registro|Versão|Descrição|
|------------------------------------|---------------|----------------------------|
|AllowNonstandardModeCombinations|Todos os|Essa entrada indica que as combinações de modo não padrão são permitidas em sincronização entre clientes e servidores. O valor padrão para membros do domínio é 1. O valor padrão para clientes independentes e servidores é 1.|
|DllName|Todos os|Essa entrada especifica o local da DLL para o provedor de tempo.<br /><br />O local padrão para esse DLL membros do domínio e clientes independentes e servidores é windir%\System32\W32Time.dll %.  |
|Habilitado|Todos os|Essa entrada indica se o provedor NtpServer está habilitado no serviço de tempo atual. <ul><li>Sim 1</li><li>0 não</li></ul>O valor padrão em membros do domínio é 1. O valor padrão em clientes independentes e servidores é 1.  |
|InputProvider|Todos os|Essa entrada indica se o provedor NtpServer está habilitado.  <ul><li>Sim 1  </li><li>0 não </li></ul>O valor padrão em membros do domínio é 1. O valor padrão em clientes independentes e servidores é 1.  |

#### <a name="MaxAllowedPhaseOffset"></a>Informações de MaxAllowedPhaseOffset
Em ordem para W32Time definir o relógio do computador gradualmente, o deslocamento deve ser menor que o valor de MaxAllowedPhaseOffset e satisfazem a seguinte equação ao mesmo tempo:  
```  
|CurrentTimeOffset| / (PhaseCorrectRate*UpdateInterval) < SystemClockRate / 2  
``` 
O CurrentTimeOffset é medido em marcas de escala do relógio, onde 1 ms = 10.000 clock marcas de escala em um sistema Windows.  
  
SystemClockRate e PhaseCorrectRate também são medidos em tiques do relógio. Para obter o SystemClockRate, você pode usar o comando a seguir e convertê-lo de segundos para marcas de escala usando a fórmula de segundos de clock * 1000\ * 10000:  
  
```  
W32tm /query /status /verbose  
ClockRate: 0.0156000s  
```  
  
SystemclockRate é a taxa do relógio do sistema. Usando 156000 segundos como um exemplo, o SystemclockRate pode ser = 0.0156000 * 1000 \ * 10000 = 156000 escalas de relógio.  
  
MaxAllowedPhaseOffset também é em segundos. Para convertê-lo para marcas de escala de clock, multiplique MaxAllowedPhaseOffset * 1000\ * 10000.  
  
Os dois exemplos a seguir mostram como aplicar  
  
**Exemplo 1**: horário difere em 4 minutos (por exemplo, seu tempo é 11:05 AM e a amostra de horário recebida de um par acredita-se que seja correto é 11:09:00).  
```
phasecorrectRate = 1  
  
UpdateInterval = 30000 (clock ticks)  
  
systemclockRate = 156000 (clock ticks)  
  
MaxAllowedPhaseOffset = 10min = 600 seconds = 600*1000\*10000=6000000000 clock ticks  
  
|currentTimeOffset| = 4mins = 4*60\*1000\*10000 = 2400000000 ticks  
  
Is CurrentTimeOffset < MaxAllowedPhaseOffset?  
  
2400000000 < 6000000000 = TRUE  
```
E ele satisfazem a equação acima? 
```
(|CurrentTimeOffset| / (PhaseCorrectRate*UpdateInterval) < SystemClockRate / 2)  
  
Is 2,400,000,000 / (30000*1) < 156000/2  
  
Is 100,000 < 78,000  
  
NO/FALSE  
```  
Portanto, W32tm definiria o relógio de volta imediatamente.  
  
> [!NOTE]  
> Nesse caso, se você quiser definir o relógio volta lentamente, você precisa ajustar os valores de PhaseCorrectRate ou updateInterval no registro também para garantir que os resultados da equação em TRUE.  
  
**Exemplo 2**: horário difere em 3 minutos.  
```  
phasecorrectRate = 1  
  
UpdateInterval = 30000 (clock ticks)  
  
systemclockRate = 156000 (clock ticks)  
  
MaxAllowedPhaseOffset = 10min = 600 seconds = 600*1000\*10000=6000000000 clock ticks  
  
currentTimeOffset = 3mins = 3*60\*1000\*10000 = 1800000000 clock ticks  
  
Is CurrentTimeOffset < MaxAllowedPhaseOffset?  
  
1800000000 < 6000000000 = TRUE  
```  
E ele satisfazem a equação acima?
```
(|CurrentTimeOffset| / (PhaseCorrectRate*UpdateInterval) < SystemClockRate / 2)  
  
Is 3 mins (1,800,000,000) / (30000*1) < 156000/2  
  
Is 60,000 < 78,000  
  
YES/TRUE  
```  
Nesse caso o relógio será definido lentamente.  
  
## <a name="w2k3tr_times_tools_vwtt"></a>Configurações de política de grupo de serviços de tempo do Windows  
Você pode configurar a maioria dos parâmetros de W32Time usando o Editor de objeto de política de grupo. Isso inclui a configuração de um computador para ser um NTPServer ou NTPClient, configurar o mecanismo de sincronização de tempo e configurar um computador para ser uma fonte de tempo confiável.  
  
> [!NOTE]  
> Configurações de política de grupo para o serviço de tempo do Windows podem ser configuradas em controladores de domínio do Windows Server 2008 R2, Windows Server 2003 R2, Windows Server 2008 e Windows Server 2003 e podem ser aplicadas somente a computadores que executam o Windows Server 2003, Windows Server 2003 R2, Windows Server 2008 e Windows Server 2008 R2.  
  
Você pode encontrar a política de grupo configurações usadas para configurar W32Time no snap-in Editor de objeto de política de grupo nos seguintes locais:  
  
-   Serviço de tempo do computador \ Modelos Templates\System\Windows  
  
    Configurar **configurações globais** aqui.  
  
-   Provedores de Service\Time do computador \ Modelos Templates\System\Windows tempo  
  
    Configurar **Windows NTP Client** configurações aqui.  
  
    Habilitar **Windows NTP Client** aqui.  
  
    Habilitar **Windows NTP Server** aqui.  
  
> [!WARNING]  
> Alguns dos valores predefinidos que são configurados no sistema administrativas modelo arquivo (System.adm) para as configurações de política de grupo (GPO) do objeto são diferentes das entradas do registro correspondentes padrão. Se você pretende usar um GPO para definir qualquer configuração de tempo do Windows, certifique-se de que você reveja [predefinição valores para as configurações de política de grupo de serviço de tempo do Windows são diferentes das entradas de registro de serviço de tempo do Windows correspondentes no Windows Server 2003](https://go.microsoft.com/fwlink/?LinkId=186066). Esse problema se aplica ao Windows Server 2008 R2, Windows Server 2008, Windows Server 2003 R2 e Windows Server 2003.  
  
A tabela a seguir lista as configurações de política de grupo globais que estão associadas com o serviço de tempo do Windows e o valor definido previamente associado a cada configuração. Para obter mais informações sobre cada configuração, veja as entradas do registro correspondente no "[entradas de registro de serviço de tempo do Windows](#w2k3tr_times_tools_uhlp)" anteriormente esse assunto. As seguintes configurações estão contidas em um único GPO chamado **configurações globais**.  
  
**Configurações de política de grupo global associadas ao tempo do Windows**  
  
|Configuração de política de grupo|Valor predefinido|  
|------------------------|------------------|  
|AnnounceFlags|10|  
|EventLogFlags|2|  
|FrequencyCorrectRate|4|  
|HoldPeriod|5|  
|LargePhaseOffset|1280000|  
|LocalClockDispersion|10|  
|MaxAllowedPhaseOffset|300|  
|MaxNegPhaseCorrection|54.000 (15 horas)|  
|MaxPollInterval|15|  
|MaxPosPhaseCorrection|54.000 (15 horas)|  
|MinPollInterval|10|  
|PhaseCorrectRate|7|  
|PollAdjustFactor|5|  
|SpikeWatchPeriod|90|  
|UpdateInterval|100|  
  
A tabela a seguir lista as configurações disponíveis para o **configurar o Windows NTP Client** GPO e os valores predefinidos que são associados com o serviço de tempo do Windows. Para obter mais informações sobre cada configuração, veja as entradas do registro correspondente no "[entradas de registro de serviço de tempo do Windows](#w2k3tr_times_tools_uhlp)" anteriormente esse assunto.  
  
**Configurações de política de grupo do cliente NTP associadas ao tempo do Windows**  
  
|Configuração de política de grupo|Valor padrão|  
|------------------------|-----------------|  
|NtpServer|time.windows.com, 0x1|  
|Tipo|Opções padrão:<br /><br />-   **NTP.** Use em computadores que não tenham ingressados em um domínio.<br />-   **NT5DS.** Use em computadores que fazem parte de um domínio.|  
|CrossSiteSyncFlags|2|  
|ResolvePeerBackoffMinutes|15|  
|ResolvePeerBackoffMaxTimes|7|  
|SpecialPollInterval|3600|  
|EventLogFlags|0|  
  
## <a name="w2k3tr_times_tools_suxb"></a>Portas de rede usadas pelo serviço de tempo do Windows  
Tempo do Windows segue a especificação NTP, que requer o uso de porta UDP 123 para todas as comunicações de sincronização de tempo. Essa porta é reservada pelo tempo do Windows e permanece reservado o tempo todo. Sempre que o computador sincroniza seu relógio ou fornece tempo para outro computador, essa comunicação é realizada na porta UDP 123.  
  
> [!NOTE]  
> Se você tiver um computador com vários adaptadores de rede (também chamados de um computador de hospedagem múltipla), você não poderá habilitar seletivamente o serviço de tempo do Windows com base no adaptador de rede.  
  
## <a name="w2k3tr_times_tools_qoep"></a>Informações relacionadas  
Os recursos a seguir contêm informações adicionais que são relevantes para esta seção.  
  
-   RFC *1305* no banco de dados IETF RFC  

