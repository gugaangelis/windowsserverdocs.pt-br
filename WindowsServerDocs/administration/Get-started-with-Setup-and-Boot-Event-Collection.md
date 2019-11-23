---
title: Introdução à Coleta de Eventos de Instalação e Inicialização
description: Configuração de coletores e metas de coleta de eventos de instalação e inicialização
ms.prod: windows-server
ms.service: na
manager: DonGill
ms.technology: server-sbec
ms.localizationpriority: medium
ms.date: 10/16/2017
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: fc239aec-e719-47ea-92fc-d82a7247b3f8
author: jaimeo
ms.author: jaimeo
ms.openlocfilehash: d1d24cdf481f19b37093f76cf8741702e1b4de60
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370515"
---
# <a name="get-started-with-setup-and-boot-event-collection"></a>Introdução à Coleta de Eventos de Instalação e Inicialização

>Aplica-se a: Windows Server

  
## <a name="overview"></a>Visão geral  
A Coleta de eventos de instalação e inicialização é um novo recurso do Windows Server 2016 que permite a você designar um computador "coletor" que pode obter diversos eventos importantes que ocorrem em outros computadores durante a inicialização ou o processo de instalação. Você pode analisar posteriormente os eventos coletados com os cmdlets do Visualizador de Eventos, Analisador de Mensagem, Wevtutil ou Windows PowerShell.  
  
Anteriormente, esses eventos têm sido impossíveis de monitorar porque a infraestrutura necessária para coletá-los não existe até que um computador já esteja configurado. Os tipos de eventos de configuração e de inicialização que você pode monitorar incluem:  
  
-   Carregamento de módulos kernel e drivers  
  
-   Enumeração de dispositivos e inicialização de seus drivers (incluindo "dispositivos" como o tipo de CPU)  
  
-   Verificação e a montagem de sistemas de arquivos  
  
-   Inicialização de arquivos executáveis  
  
-   Inicialização e conclusões de atualizações do sistema  
  
-   Os pontos em que o sistema fica disponível para logon, estabelece conexão com um controlador de domínio, a conclusão do serviço é iniciada e a disponibilidade de compartilhamentos de rede  
  
O computador coletor deve estar executando o Windows Server 2016 (pode estar no Server com o modo de Experiência Desktop ou Server Core). O computador de destino deve estar executando o Windows 10 ou o Windows Server 2016. Você também pode executar esse serviço em uma máquina virtual hospedada em um computador que **não** está executando o Windows Server 2016. As seguintes combinações de computadores de destino e coletor virtualizados funcionam:  
  
|Host de virtualização|Máquina virtual de coletor|Máquina virtual de destino|  
|-----------------------|-----------------------------|--------------------------|  
|Windows 8.1|sim|sim|  
|Windows 10|sim|sim|  
|Windows Server 2016|sim|sim|  
|Windows Server 2012 R2|sim|não|  
  
## <a name="installing-the-collector-service"></a>Instalação do serviço do coletor  
A partir do Windows Server 2016, o serviço de coletor de eventos está disponível como um recurso opcional. Nesta versão, você pode instalá-lo usando DISM.exe com este comando em um prompt de comandos com privilégios elevados do Windows PowerShell:  
  
`dism /online /enable-feature /featurename:SetupAndBootEventCollection`  
  
Este comando cria um serviço chamado BootEventCollector e o inicia com um arquivo de configuração vazio.  
  
Confirme se a instalação terá êxito verificando `get-service -displayname *boot*`. O Coletor de eventos de inicialização deve estar em execução. Ele é executado sob a Conta de serviço de rede e cria um arquivo de configuração vazio (Active.xml) em **%SystemDrive%\ProgramData\Microsoft\BootEventCollector\Config**.  
  
Você também pode instalar o serviço de Coleta de eventos de inicialização e instalação com o assistente para Adicionar funções e recursos no Gerenciador do Servidor.  
  
## <a name="configuration"></a>Configuração  
Você deve configurar dois itens para coletar eventos de instalação e inicialização.  
  
-   Nos computadores de destino que enviarão os eventos (ou seja, os computadores cuja instalação e inicialização você deseja monitorar), habilite o transporte KDNET/EVENT-NET e habilite o encaminhamento de eventos.  
  
-   No computador coletor, especifique quais computadores devem aceitar eventos e onde salvá-los.  
  
> [!NOTE]  
> Você não pode configurar um computador para enviar eventos de instalação ou inicialização para si mesmo. Mas, se você deseja monitorar dois computadores, é possível definir essas configurações para enviar os eventos uns aos outros.  
  
### <a name="configuring-a-target-computer"></a>Configurar um computador de destino  
Em cada computador de destino, você primeiro habilita o transporte KDNET/EVENT-NET e, em seguida, habilita o envio de eventos ETW por meio de transporte e reinicia o computador de destino. EVENT-NET é um protocolo de transporte no kernel semelhante ao KDNET (o protocolo depurador de kernel). EVENT-NET transmite somente eventos e não permite o acesso do depurador. Esses dois protocolos são mutuamente exclusivos; você só pode ativar um por vez.  
  
É possível habilitar o transporte de evento remotamente (com o Windows PowerShell) ou localmente.  
  
##### <a name="to-enable-event-transport-remotely"></a>Para habilitar o transporte de evento remotamente  
  
1.  Se você já tiver configurado a opção remota do Windows PowerShell para o computador de destino, pule para a Etapa 3. Caso contrário, no computador de destino, abra um prompt de comando e execute o seguinte comando:  
  
    winrm quickconfig  
  
2.  Responda às solicitações e reinicie o computador de destino. Se os computadores de destino não estiverem no mesmo domínio do computador do coletor, talvez seja necessário defini-los como hosts confiáveis. Para fazer isso:  
  
3.  No computador do coletor, execute um destes comandos:  
  
    -   Em um prompt do Windows PowerShell: `Set-Item -Force WSMan:\localhost\Client\TrustedHosts "<target1>,<target2>,..."`, seguido por `Set-Item -Force WSMan:\localhost\Client\AllowUnencrypted true` em que \<target1 >, etc. são os nomes ou endereços IP dos computadores de destino.  
  
    -   Ou em um prompt de comando: **WinRM Set WinRM/config/Client @ {TrustedHosts = "\<target1 >,\<target2 >,..."; AllowUnencrypted = "true"}**  
  
        > [!IMPORTANT]  
        > Isto define uma comunicação não criptografada, portanto, não faça fora de um ambiente de laboratório.  
  
4.  Teste a conexão remota indo para o computador do coletor e executando um destes comandos do Windows PowerShell:  
  
    Se o computador de destino estiver no mesmo domínio que o computador do coletor, execute `New-PSSession -Computer <target> | Remove-PSSession`  
  
    Se o computador de destino não estiver no mesmo domínio, execute `New-PSSession -Computer  <target>  -Credential Administrator | Remove-PSSession`, que solicitará as credenciais.  
  
    Se o comando não retorna nada, a comunicação remota foi bem-sucedida.  
  
5.  No computador de destino, abra um prompt do Windows PowerShell com privilégios elevados e execute este comando:  
  
    `Enable-SbecBcd -ComputerName <target_name> -CollectorIP <ip> -CollectorPort <port> -Key <a.b.c.d>`  
  
    Aqui < target_name > é o nome do computador de destino, \<> IP é o endereço IP do computador do coletor. \<porta > é o número da porta em que o coletor será executado. A <a.b.c.d> é uma chave de criptografia necessária para a comunicação, que consiste em quatro cadeias de caracteres alfanuméricos separadas por pontos. Essa mesma chave é usada no computador coletor. Se você não inserir uma chave, o sistema gera uma chave aleatória; você precisará disso para o computador coletor, portanto, anote-a.  
  
6.  Se você já tiver um computador coletor configurado, atualize o arquivo de configuração no computador coletor com as informações para o novo computador de destino. Consulte a seção "Configurar o computador coletor" para obter mais detalhes.  
  
##### <a name="to-enable-event-transport-locally-on-the-target-computer"></a>Para habilitar o transporte de evento localmente no computador de destino  
  
1.  Inicie um prompt de comando elevado e, em seguida, execute estes comandos:  
  
    **bcdedit/Event Sim**  
  
    **bcdedit/eventsettings net HostIP: 1.2.3.4 Port: 50000 Key: a. b. c. d**  
  
    Aqui, "1.2.3.4" é um exemplo; substitua pelo endereço IP do computador coletor. Substitua "50000" pelo número da porta em que o coletor será executado e "a.b.c.d" pela chave de criptografia necessária para a comunicação. Essa mesma chave é usada no computador coletor. Se você não inserir uma chave, o sistema gera uma chave aleatória; você precisará disso para o computador coletor, portanto, anote-a.  
  
2.  Se você já tiver um computador coletor configurado, atualize o arquivo de configuração no computador coletor com as informações para o novo computador de destino. Consulte a seção "Configurar o computador coletor" para obter mais detalhes.  
  
**Agora que o próprio transporte de evento está habilitado, você deve habilitar o sistema para realmente enviar eventos ETW por meio desse transporte.**  
  
##### <a name="to-enable-sending-of-etw-events-through-the-transport-remotely"></a>Para habilitar o envio de eventos ETW por meio do transporte remotamente  
  
1.  No computador coletor, abra um prompt de comandos com privilégios elevados do Windows PowerShell.  
  
2.  Execute `Enable-SbecAutologger -ComputerName <target_name>`, onde <target_name> é o nome do computador de destino.  
  
Se não puder configurar a comunicação remota do Windows PowerShell, você sempre pode habilitar o envio de eventos diretamente no computador de destino.  
  
##### <a name="to-enable-sending-of-etw-events-through-the-transport-locally"></a>Para habilitar o envio de eventos ETW por meio de transporte localmente  
  
1.  No computador de destino, inicie o Regedit.exe e localize esta chave do registro:  
  
    **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\WMI\AutoLogger**. Várias sessões de log são listadas como chaves subsites sob essa chave. **Setup Platform**, **NT Kernel Logger**e **Microsoft-Windows-Setup** são opções possíveis para uso com a instalação e a coleta de eventos de inicialização, mas a opção recomendada é o **EventLog-System**. Essas chaves são detalhadas em [Configurar e iniciar uma sessão de agente de log automático](https://msdn.microsoft.com/library/windows/desktop/aa363687(v=vs.85).aspx).  
  
2.  Na chave EventLog-System, altere o valor da **LogFileMode** de **0x10000180** para **0x10080180**. Para obter mais informações sobre os detalhes dessas configurações, consulte [Constantes de modo de registro em log](https://msdn.microsoft.com/library/windows/desktop/aa364080(v=vs.85).aspx).  
  
3.  Opcionalmente, você também pode habilitar o encaminhamento de dados de verificação de bug para o computador coletor. Para fazer isso, localize a chave do registro HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager e crie a chave **Debug Print Filter** com um valor de **0x1**.  
  
4.  Reinicie o computador de destino.  
  
### <a name="choosing-the-network-adapter"></a>Escolher o adaptador de rede  
Se o computador de destino tiver mais de um adaptador de rede, o driver KDNET escolherá o primeiro suportado listado. Você pode especificar um adaptador de rede específico a ser usado para encaminhamento de eventos da instalação com estas etapas:  
  
##### <a name="to-specify-a-network-adapter"></a>Para especificar um adaptador de rede  
  
1.  No computador de destino, abra o Gerenciador de dispositivos, expanda **Adaptadores de rede**, encontre o adaptador de rede que você deseja usar e clique nele com o botão direito do mouse.  
  
2.  No menu que abre, clique em **Propriedades** e, em seguida, clique na guia **Detalhes**. Expanda o menu no campo **Propriedade** , role até encontrar as **Informações de local** (a lista provavelmente não está em ordem alfabética) e, em seguida, clique nelas. O valor será uma cadeia de caracteres do formulário **barramento PCI X, dispositivo Y, função Z**. Anote o X.Y.Z; estes são os parâmetros de barramento que você precisa para o comando a seguir.  
  
3.  Execute um destes comandos:  
  
    Em um prompt elevado do Windows PowerShell: `Enable-SbecBcd -ComputerName <target_name> -CollectorIP <ip> -CollectorPort <port> -Key <a.b.c.d> -BusParams <X.Y.Z>`  
  
    Em um prompt de comandos com privilégios elevados: **bcdedit /eventsettings net hostip:aaa porta: 50000 chave: bbb busparams:X.Y.Z**  
  
### <a name="validate-target-computer-configuration"></a>Validar a configuração do computador de destino  
Para verificar as configurações no computador de destino, abra um prompt de comandos com privilégios elevados e execute **bcdedit /enum**. Ao terminar, execute **bcdedit /eventsettings**. Você pode verificar os seguintes valores:  
  
-   Chave  
  
-   Debugtype = NET  
  
-   HostIP = \<endereço IP do > do coletor  
  
-   Port = número da porta \<que você especificou para o coletor usar >  
  
-   DHCP = Sim  
  
Além disso, verifique se você habilitou **bcdedit /event**, pois **/debug** e **/event** são mutuamente exclusivos. Você só pode executar um ou outro. Da mesma forma, você não pode misturar /eventsettings com /debug ou /dbgsettings com /event.  
  
Observe também que o conjunto de eventos não funciona ao defini-lo para uma porta serial.  
  
### <a name="configuring-the-collector-computer"></a>Configurar o computador coletor  
O serviço coletor recebe os eventos e os salva em arquivos ETL. Esses arquivos ETL podem ser lidos por outras ferramentas, como os cmdlets Visualizador de Eventos, Analisador de Mensagem, Wevtutil e Windows PowerShell.  
  
Como o formato ETW não permite que você especifique o nome do computador de destino, os eventos para cada computador de destino devem ser salvos em um arquivo separado. As ferramentas de exibição podem mostrar o nome de um computador, mas será o nome do computador no qual a ferramenta será executada.  
  
Mais exatamente, é atribuído um toque dos arquivos ETL a cada computador de destino. Cada nome de arquivo inclui um índice de 000 até um valor máximo que você configura (até 999). Quando o arquivo atinge o tamanho máximo configurado, ele alterna a gravação de eventos para o próximo arquivo. Após o arquivo mais alto possível, alterna para o índice de arquivo 000. Assim, os arquivos são automaticamente reciclados, limitando o uso de espaço em disco. Você também pode definir as políticas de retenção externa adicionais para limitar ainda mais o uso do disco; por exemplo, você pode excluir arquivos anteriores a um número específico de dias.  
  
Arquivos ETL coletados normalmente são mantidos no diretório **c:\ProgramData\Microsoft\BootEventCollector\Etl** (que pode ter subdiretórios adicionais). Você pode encontrar o arquivo de log mais recente classificando-o de acordo com a hora da última modificação. Também existe um log de status (normalmente em **c:\ProgramData\Microsoft\BootEventCollector\Logs**), que registra sempre que o coletor alterna a gravação para um novo arquivo.  
  
Também há um log de coletor, que registra informações sobre o próprio coletor. Você pode manter esse log no formato ETW (em que eventos serão relatados para o serviço de log do Windows; este é o padrão) ou em um arquivo (normalmente em **c:\ProgramData\Microsoft\BootEventCollector\Logs**). O uso de um arquivo pode ser útil se você deseja habilitar modos detalhados que produzem uma grande quantidade de dados. Você também pode definir o log para gravar em uma saída padrão executando o coletor da linha de comando.  
  
**Criando o arquivo de configuração do coletor**  
  
Quando você habilita o serviço, três arquivos de configuração XML são criados e armazenados em **c:\ProgramData\Microsoft\BootEventCollector\Config**:  
  
-   **Active.XML** Esse arquivo contém a configuração ativa atual do serviço coletor.  Após a instalação, esse arquivo tem o mesmo conteúdo que Empty.xml. Você o salva neste arquivo ao definir uma nova configuração de coletor.  
  
-   **Empty.XML** Esse arquivo contém os elementos de configuração mínimos necessários com o conjunto de valores padrão. Ele não permite qualquer coleção; permite apenas que o serviço coletor inicie no modo ocioso.  
  
-   **Example.XML** Esse arquivo fornece exemplos e explicações sobre os elementos de configuração possíveis.  
  
**Escolhendo um limite de tamanho de arquivo**  
  
Uma das decisões que você precisa tomar é definir um limite de tamanho de arquivo. O limite de tamanho de arquivo recomendado depende do volume esperado de eventos e espaço em disco disponível. Arquivos menores são mais convenientes da perspectiva de limpeza dos dados antigos. No entanto, cada arquivo contém a sobrecarga de um cabeçalho de 64 KB e a leitura de muitos arquivos para obter o histórico combinado pode ser inconveniente. O limite de tamanho mínimo de arquivo absoluto é 256 KB. Um limite de tamanho de arquivo prático razoável deve ser superior a 1 MB e 10 MB provavelmente é um bom valor típico. Um limite superior seria razoável se você espera muitos eventos.  
  
Há vários detalhes para lembrar-se em relação ao arquivo de configuração:  
  
-   O endereço do computador de destino. Você pode usar o respectivo endereço IPv4, um endereço MAC ou um GUID SMBIOS. Tenha esses fatores em mente ao escolher o endereço a ser usado:  
  
    -   O endereço IPv4 funciona melhor com atribuição estática dos endereços IP. No entanto, os endereços IP estáticos ainda devem estar disponíveis pelo DHCP.  
  
    -   Um endereço MAC ou o GUID SMBIOS é conveniente quando eles são conhecidos com antecedência, mas os endereços IP são atribuídos dinamicamente.  
  
    -   Endereços IPv6 não são compatíveis com o protocolo EVENT-NET.  
  
    -   É possível especificar várias maneiras para identificar o computador. Por exemplo, se o hardware físico está prestes a ser substituído, você poderá inserir os endereços MAC antigos e os novos, e qualquer um deles será aceito.  
  
-   A chave de criptografia usada para a comunicação com o computador do coletor  
  
-   O nome do computador de destino. Você pode usar o endereço IP, o nome do host ou qualquer outro nome como o nome do computador.  
  
-   O nome do arquivo ETL a ser usado e a configuração de tamanho anel dele  
  
##### <a name="to-create-the-configuration-file"></a>Para criar o arquivo de configuração  
  
1.  Abra um prompt do Windows PowerShell com privilégios elevados e altere os diretórios para %SystemDrive%\ProgramData\Microsoft\BootEventCollector\Config.  
  
2.  Tipo `notepad .\newconfig.xml` e pressione ENTER.  
  
3.  Copie este exemplo de configuração para a janela do Bloco de notas:  
  
    ```  
    <collector configVersionMajor="1" statuslog="c:\ProgramData\Microsoft\BootEventCollector\Logs\statuslog.xml">  
      <common>  
        <collectorport value="50000"/>  
        <forwarder type="etl">  
          <set name="file" value="c:\ProgramData\Microsoft\BootEventCollector\Etl\{computer}\{computer}_{#3}.etl"/>  
          <set name="size" value="10mb"/>  
          <set name="nfiles" value="10"/>  
          <set name="toxml" value="none"/>  
        </forwarder>  
        <target>  
          <ipv4 value="192.168.1.1"/>  
          <key value="a.b.c.d"/>  
          <computer value="computer1"/>  
        </target>  
        <target>  
          <ipv4 value="192.168.1.2"/>  
          <key value="d1.e2.f3.g4"/>  
          <computer value="computer2"/>  
        </target>  
      </common>  
    </collector>  
    ```  
  
    > [!NOTE]  
    > O nó raiz é \<> do coletor. Seus atributos especificam a versão da sintaxe do arquivo de configuração e o nome do arquivo de log de status.  
    >   
    > O \<> de elementos comuns agrupa vários destinos especificando os elementos de configuração comuns para eles, muito parecido com um grupo de usuários pode ser usado para especificar as permissões comuns para vários usuários.  
    >   
    > O elemento \<collectorport > define o número da porta UDP em que o coletor escutará os dados de entrada. Essa é a mesma porta especificada na etapa de configuração de destino para Bcdedit. O coletor oferece suporte a apenas uma porta e todos os destinos devem se conectar a mesma porta.  
    >   
    > O elemento > do encaminhador \<especifica como os eventos ETW recebidos dos computadores de destino serão encaminhados. Há apenas um tipo de encaminhador, que os grava nos arquivos ETL. Os parâmetros especificam o padrão de nome de arquivo, o limite de tamanho de cada arquivo no anel e o tamanho do anel para cada computador. A configuração "toxml" especifica que os eventos ETW serão gravados no formato binário como foram recebidos, sem a conversão para XML. Consulte a seção "Conversão de evento XML" para obter informações sobre como decidir se deve conferir os eventos para XML ou não. O padrão de nome de arquivo contém essas substituições: {computer} para o nome do computador e {#3} para o índice do arquivo no anel.  
    >   
    > Neste arquivo de exemplo, dois computadores de destino são definidos com o elemento de > de destino \<. Cada definição especifica o endereço IP com \<> IPv4, mas você também pode usar o endereço MAC (por exemplo, < Mac Value = "11:22:33:44:55:66"\/> ou < Mac Value = "11-22-33-44-55-66"\/>) ou SMBIOS GUID (por exemplo, < GUID Value = "{269076F9-4B77-46E1-B03B-CA5003775B88}"\/>) para identificar o computador de destino. Observe também a chave de criptografia (a mesma especificada ou gerada com Bcdedit no computador de destino) e o nome do computador.  
  
4.  Insira os detalhes de cada computador de destino como um elemento de destino de \<separado > no arquivo de configuração e, em seguida, salve newconfig. xml e feche o bloco de notas.  
  
5.  Aplique a nova configuração com `$result = (Get-Content .\newconfig.xml | Set-SbecActiveConfig); $result`. A saída deve ser retornada com o campo Sucesso como "true". Se você receber outro resultado, consulte a seção de Solução de problemas deste tópico.  
  
Você sempre pode verificar a configuração ativa atual com `(Get-SbecActiveConfig).text`.  
  
Você pode executar uma verificação de validade do arquivo de configuração com `$result = (Get-Content .\newconfig.xml | Check-SbecConfig); $result`.  
  
Embora o comando do Windows PowerShell para aplicar uma nova configuração automaticamente atualize o serviço sem que seja preciso reiniciá-lo, você sempre pode reiniciar o serviço sozinho com qualquer um desses comandos:  
  
-   Com o Windows PowerShell: `Restart-Service BootEventCollector`  
  
-   Em um prompt de comando comum: **sc stop BootEventCollector; sc start BootEventCollector**  
  
## <a name="configuring-nano-server-as-a-target-computer"></a>Configurar o Nano Server como um computador de destino  
A interface mínima oferecida pelo Nano Server às vezes pode dificultar o diagnóstico de problemas com ele. Você pode configurar a imagem do Nano Server para participar da Coleta de Eventos de Instalação e Inicialização automaticamente, enviando dados de diagnóstico em um computador coletor sem nenhuma outra intervenção. Para fazer isso, execute estas etapas:  
  
#### <a name="to-configure-nano-server-as-a-target-computer"></a>Para configurar o Nano Server como um computador de destino  
  
1.  Crie sua imagem básica do Nano Server. Para obter detalhes, consulte [Introdução ao Nano Server](https://technet.microsoft.com/library/mt126167.aspx) .  
  
2.  Configure um computador coletor como na seção "Configurar o computador coletor" deste tópico.  
  
3.  Adicione as chaves do registro de Agente de log automático para habilitar o envio de mensagens de diagnóstico. Para fazer isso, você monta o VHD do Nano Server criado na Etapa 1, carrega o hive do registro e, em seguida, adiciona chaves do registro específicas. Neste exemplo, a imagem do Nano Server está em C:\NanoServer; o caminho pode ser diferente, portanto, ajuste as etapas.  
  
    1.  No computador coletor, copie a pasta **..\Windows\System32\WindowsPowerShell\v1.0\Modules\BootEventCollector** e cole-a no diretório **..\Windows\System32\WindowsPowerShell\v1.0\Modules** no computador que você está usando para modificar o VHD do Nano Server.  
  
    2.  Inicie um console do Windows PowerShell com permissões elevadas e execute `Import-Module BootEventCollector` .  
  
    3.  Atualize o registro de VHD do Nano Serve para habilitar Agentes de log automático. Para fazer isso, execute `Enable-SbecAutoLogger -Path C:\NanoServer\Workloads\IncludingWorkloads.vhd`. Isto adiciona uma lista básica do ambiente mais típico e eventos de inicialização; você pode pesquisar outros em [Controlar sessões de rastreamento de eventos](https://msdn.microsoft.com/library/windows/desktop/aa363694(v=vs.85).aspx).  
  
4.  Atualize configurações de BCD na imagem do Nano Server para habilitar o sinalizador de Eventos e defina o computador coletor para certifique-se de que eventos de diagnósticos são enviados para o servidor correto. Observe que o endereço IPv4 do computador coletor, a porta TCP e a chave de criptografia configurada no arquivo Active.XML do coletor (descrito em outro lugar neste tópico). Use este comando em um console do Windows PowerShell com permissões elevadas: `Enable-SbecBcd -Path C:\NanoServer\Workloads\IncludingWorkloads.vhd -CollectorIp 192.168.100.1 -CollectorPort 50000 -Key a.b.c.d`  
  
5.  Atualize o computador coletor para receber eventos enviados pelo computador do Nano Server adicionando o intervalo de endereços IPv4, o endereço IPv4 específico ou o endereço MAC do Nano Server no arquivo Active.XML no computador do coletor (consulte a seção "Configurar o computador coletor"deste tópico).  
  
## <a name="starting-the-event-collector-service"></a>Iniciar o serviço coletor de eventos  
Depois que um arquivo de configuração válido é salvo no computador do coletor e um computador de destino está configurado, assim que o computador de destino for reiniciado, a conexão com o coletor é feita e os eventos serão coletados.  
  
O log para o serviço coletor propriamente dito (que é distinto dos dados de configuração e de inicialização coletados pelo serviço) pode ser encontrado em Microsoft-Windows-BootEvent-Collector/Admin. Para obter uma interface gráfica dos eventos, use o Visualizador de Eventos. Crie um novo modo de exibição; expanda **Logs de serviços e aplicativos** e, em seguida, expanda **Microsoft** e **Windows**. Encontre o **BootEvent-Collector**, expanda-o e encontre **Admin**.  

-   Com o Windows PowerShell: `Get-WinEvent -LogName Microsoft-Windows-BootEvent-Collector/Admin`  
  
-   Em um prompt de comando comum: **wevtutil qe Microsoft-Windows-BootEvent-Collector/Admin**  
  
## <a name="troubleshooting"></a>Solução de problemas  
  
### <a name="troubleshooting-installation-of-the-feature"></a>Solução de problemas de instalação do recurso  
  
||Erro|Descrição do erro|Sintoma|Problema em potencial|  
|-|---------|---------------------|-----------|---------------------|  
|Dism.exe|87|A opção de nome do recurso não é reconhecida nesse contexto||-Isso pode acontecer ao escrever incorretamente o nome do recurso. Verifique se a ortografia está correta e tente novamente.<br />-Confirme se este recurso está disponível na versão do sistema operacional que você está usando. No Windows PowerShell, execute **dism /online /get-features &#124; ?{$_ -match "boot"}** . Se nenhuma correspondência for retornada, você provavelmente está executando uma versão que não oferece suporte a esse recurso.|  
|Dism.exe|0x800f080c|O nome do \<de recursos > é desconhecido.||Mesmo que acima|  
  
### <a name="troubleshooting-the-collector"></a>Solução de problemas do coletor  
  
**Logout**  
O Coletor registra em log seus próprios eventos como provedor ETW Microsoft-Windows-BootEvent-Collector. É o primeiro lugar que você deve procurar por problemas do coletor de solução de problemas. Você pode encontrá-los no Visualizador de Eventos em Logs de aplicativos e serviços > Microsoft > Windows > BootEvent-Collector > Administrador ou você pode lê-los em uma janela de comando com qualquer um desses comandos:  
  
Em um prompt de comando comum: **wevtutil qe Microsoft-Windows-BootEvent-Collector/Admin**  
  
Em um prompt do Windows PowerShell: `Get-WinEvent -LogName Microsoft-Windows-BootEvent-Collector/Admin` (você pode acrescentar `-Oldest` para retornar a lista em ordem cronológica, com eventos mais antigos primeiro)  
  
Você pode ajustar o nível de detalhe nos logs de "erro", por meio de "aviso", "informações" (padrão), "detalhada" e "depuração". Níveis mais detalhados de "informações" são úteis para diagnosticar problemas de conexão com os computadores de destino, mas eles podem gerar uma grande quantidade de dados, portanto, use-os com cuidado.  
  
Defina o nível de log mínimo no elemento > do coletor de \<do arquivo de configuração. Por exemplo: < Collector configVersionMajor = "1" minlog\="Verbose" >.  
  
O nível de detalhe faz um registro para cada pacote recebido à medida que é processado. O nível de depuração adiciona mais detalhes de processamento e esvazia o conteúdo de todos os pacotes ETW recebidos.  
  
No nível de depuração, talvez seja útil gravar o log em um arquivo em vez de tentar exibi-lo no sistema de registro em log comum. Para fazer isso, adicione um elemento adicional no elemento > do coletor de \<do arquivo de configuração:  
  
< coletor configVersionMajor = "1" minlog = "debug" log\="c:\ProgramData\Microsoft\BootEventCollector\Logs\log.txt" >  
      
 **Uma abordagem sugerida para solucionar problemas do coletor:**  
   
1. Primeiro, verifique se o coletor recebeu a conexão do destino (ele criará o arquivo somente quando o destino começa a enviar as mensagens) com   
   ```  
   Get-SbecForwarding  
   ```  
   Se retorna que há uma conexão a partir deste destino, o problema pode ser nas configurações do agente de log automático. Se retorna nada, o problema é com a conexão KDNET. Para diagnosticar problemas de conexão KDNET, tente verificar a conexão de ambas as extremidades (ou seja, de um coletor e do destino).  
  
2. Para ver o diagnóstico estendido do coletor, adicione-o ao elemento > do coletor de \<do arquivo de configuração:  
   \<coletor... minlog = "Verbose" >  
   Isso permitirá mensagens sobre cada pacote recebido.  
3. Verifique se todos os pacotes são recebidos. Opcionalmente, você talvez queira gravar o log no modo detalhado diretamente em um arquivo e não por ETW. Para fazer isso, adicione isso ao elemento > do coletor de \<do arquivo de configuração:  
   \<coletor... minlog = "log" detalhado = "c:\ProgramData\Microsoft\BootEventCollector\Logs\log.txt" >  
      
4. Verifique os logs de evento para todas as mensagens sobre os pacotes recebidos. Verifique se todos os pacotes são recebidos. Se os pacotes são recebidos, mas de forma incorreta, verifique as mensagens de eventos para obter detalhes.  
5. Do lado do destino, o KDNET grava algumas informações de diagnósticos no registro. Examinar   
   **HKLM\SYSTEM\CurrentControlSet\Services\kdnet** para mensagens.  
   KdInitStatus (DWORD) será = 0, em caso de sucesso e mostra um código de erro no erro  
   KdInitErrorString = explicação sobre o erro (também contém mensagens informativas se não houver nenhum erro)  
  
6. Execute Ipconfig.exe no destino e procure o nome do dispositivo relatado. Se KDNET for carregado adequadamente, o nome do dispositivo deve ser parecido com "kdnic" em vez do nome do cartão do fornecedor original.  
7. Verifique se o DHCP está configurado para o destino. O KDNET requer DHCP.  
8. Confirme se o coletor está na mesma rede de destino. Caso contrário, verifique se o roteamento está configurado corretamente, especificamente a configuração do gateway padrão para o DHCP.  
  
  
**Status da conexão**  
  
Você pode verificar a lista atual de conexões estabelecidas e informações sobre onde os dados estão sendo encaminhados com `Get-SbecForwarding`.  
  
Você também pode obter o recente histórico de alterações de status de conexões com `Get-SbecHistory`.  
  
### <a name="troubleshooting-setting-a-new-configuration"></a>Solução de problemas de nova configuração  
Se você aplicou a configuração com o comando do Windows PowerShell `$result = (Get-Content .\newconfig.xml | Set-SbecActiveConfig); $result`, a variável `$result` conterá informações sobre a implantação. Você pode consultar essa variável para obter informações diferentes:  
  
Obtenha informações sobre erros com `$result.ErrorString`. Se quaisquer erros forem informados aqui, a nova configuração não será aplicada e a configuração antiga não será alterada.  
  
Obtenha avisos com `$result.WarningString`.  
  
Obtenha informações sobre os detalhes da configuração com `$result.InfoString`.  
  
Você pode obter o resultado completo com `$result | fl *`.  
Como alternativa, se você não quiser salvar o resultado em uma variável, é possível usar `Get-Content .\newconfig.xml | Set-SbecActiveConfig | fl *`.  
  
### <a name="troubleshooting-target-computers"></a>Solucionar problemas de computadores de destino  
  
||Erro|Descrição do erro|Sintoma|Problema em potencial|  
|-|---------|---------------------|-----------|---------------------|  
|Computador de destino||O destino não está se conectando ao coletor||-O computador de destino não reiniciou depois que foi configurado. Reinicie o computador de destino.<br />-O computador de destino tem configurações incorretas de BCD. Verifique as configurações na seção "Validar configurações do computador de destino". Corrija conforme necessário e reinicie o computador de destino.<br />-O driver KDNET/EVENT-NET não foi capaz de se conectar a um adaptador de rede ou conectou-se ao adaptador de rede errado. No Windows PowerShell, execute `gwmi Win32_NetworkAdapter` e verifique a saída para um com o ServiceName **kdnic**. Se o adaptador de rede errado estiver marcado, execute novamente as etapas descritas em "Para especificar um adaptador de rede". Se o adaptador de rede não aparecer, é possível que o driver não ofereça suporte para qualquer um dos seus adaptadores de rede.<br>**Consulte também** "Uma abordagem sugerida para o coletor de solução de problemas" acima, especificamente as Etapas 5 a 8.|  
|Coletor||Não vejo mais eventos após migrar a VM na qual o coletor está hospedado.||Verifique se o endereço IP do computador do coletor não foi alterado. Caso afirmativo, consulte "Habilitar o envio de eventos ETW por meio de transporte remoto".|  
|Coletor||Os arquivos ETL não são criados.|`Get-SbecForwarding` mostra que o destino se conectou, sem erros, mas os arquivos ETL não são criados.|O computador de destino provavelmente não enviou quaisquer dados ainda; os arquivos ETL são criados apenas quando os dados são recebidos.|  
|Coletor||Um evento não está aparecendo no arquivo de ETL.|O computador de destino enviou a um evento, mas quando o arquivo ETL é lido com o Visualizador de Eventos do Analisador de Mensagem, o evento não está presente.|-O evento ainda pode estar no buffer. Os eventos não são gravados no arquivo ETL até que um buffer completo de 64 KB seja coletado ou um tempo limite de aproximadamente 10 a 15 segundos sem nenhum novo evento ocorrer. Espere até que o tempo limite expire ou limpe os buffers com `Save-SbecInstance`.<br />-O manifesto de evento não está disponível no computador coletor ou o computador onde o Visualizador de Eventos ou o Analisador de Mensagem é executado.  Nesse caso, o Coletor talvez não consiga processam o evento (verifique o log do Coletor) ou o visualizador talvez não consiga mostrá-lo.  É uma prática recomendada ter todos os manifestos instalados no computador do coletor e instalar atualizações no computador coletor antes de instalá-los nos computadores de destino.|
