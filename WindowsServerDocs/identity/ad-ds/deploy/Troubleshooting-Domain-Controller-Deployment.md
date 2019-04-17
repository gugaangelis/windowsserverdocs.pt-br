---
ms.assetid: 5ab76733-804d-4f30-bee6-cb672ad5075a
title: "Solucionando problemas de implantação do controlador de domínio"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 254209b0da6a7bc0cd5f3880e14a7e5b16822cdd
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="troubleshooting-domain-controller-deployment"></a>Solucionando problemas de implantação do controlador de domínio

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico aborda a metodologia detalhada sobre solução de implantação e a configuração de controlador de domínio.  
  
-   [Introdução à solução de problemas](../../ad-ds/deploy/Troubleshooting-Domain-Controller-Deployment.md#BKMK_Intro)  
  
-   [Opções de solução de problemas](../../ad-ds/deploy/Troubleshooting-Domain-Controller-Deployment.md#BKMK_Options)  
  
## <a name="BKMK_Intro"></a>Introdução à solução de problemas  
![Solução de problemas](media/Troubleshooting-Domain-Controller-Deployment/adds_deploy_troubleshooting.png)  
  
## <a name="BKMK_Options"></a>Opções de solução de problemas  
  
### <a name="logging-options"></a>Opções de registro em log  
Os logs internos são o instrumento mais importante para solucionar problemas de promoção de controlador de domínio e rebaixamento. Todos esses logs são habilitados e configurados para detalhamento máximo por padrão.  
  
|Fase|Log|  
|---------|-------|  
|Operações de Gerenciador do servidor ou ADDSDeployment Windows PowerShell|-%systemroot%\debug\dcpromoui.log<br /><br />-%systemroot%\debug\dcpromoui*.log|  
|Instalação/promoção do controlador de domínio|-%systemroot%\debug\dcpromo.log<br /><br />-%systemroot%\debug\dcpromo*.log<br /><br />-Evento viewer\Windows logs\System<br /><br />-Evento viewer\Windows logs\Application<br /><br />-Evento viewer\Applications e serviços logs\Directory serviço<br /><br />-Evento viewer\Applications e serviços logs\File replicação do serviço<br /><br />-Evento viewer\Applications e serviços logs\DFS replicação|  
|Atualização de domínio ou floresta|-%systemroot%\debug\adprep\\<datetime>\adprep.log<br /><br />-%systemroot%\debug\adprep\\<datetime>\csv.log<br /><br />-%systemroot%\debug\adprep\\<datetime>\dspecup.log<br /><br />-%systemroot%\debug\adprep\\<datetime>\ldif.log*|  
|Mecanismo de implantação do servidor do Windows PowerShell Manager ADDSDeployment|-Evento viewer\Applications e serviços Deployment\Operational logs\Microsoft\Windows\DirectoryServices|  
|Serviços do Windows|-%systemroot%\Logs\CBS\\*<br /><br />-%systemroot%\servicing\sessions\sessions.xml<br /><br />-%systemroot%\winsxs\poqexec.log<br /><br />-%systemroot%\winsxs\pending.xml|  
  
### <a name="tools-and-commands-for-troubleshooting-domain-controller-configuration"></a>Ferramentas e os comandos de solução de problemas de configuração do controlador de domínio  
Para solucionar problemas não explicados pelos logs, use as ferramentas a seguir como ponto de partida:  
  
-   Dcdiag.exe  
  
-   Repadmin.exe  
  
-   [AutoRuns.exe](https://technet.microsoft.com/sysinternals/bb963902.aspx), Gerenciador de tarefas e MSInfo32.exe  
  
-   [3.4 do Monitor de rede](https://www.microsoft.com/download/en/details.aspx?displaylang=en&id=4865) (ou uma ferramenta de captura e análise de rede terceiros)  
  
### <a name="general-methodology-for-troubleshooting-domain-controller-configuration"></a>Metodologia geral para solução de problemas de configuração do controlador de domínio  
  
1.  Um problema de sintaxe simples fazer com que o erro?  
  
    1.  Você incorretamente ou se esqueça de fornecer um argumento para ADDSDeployment Windows PowerShell? Por exemplo, se usando ADDSDeployment Windows PowerShell, você esqueceu de adicionar argumento necessário **- domainname** com um nome válido?  
  
    2.  Examine a saída do console do Windows PowerShell com cuidado para ver por que ele é Falha ao analisar a linha de comando fornecida.  
  
2.  O erro é uma pré-requisito falha?  
  
    1.  Muitos erros que apareciam como resultados de promoção fatal agora são impedidos pelo verificador de pré-requisito.  
  
    2.  Examinar o texto dos erros pré-requisito com cuidado, eles fornecem a orientação necessária para resolver a maioria dos problemas, como são controlados cenários.  
  
3.  É o erro na promoção e, portanto, fatal?  
  
    1.  Examine os resultados com cuidado: muitos erros têm a explicações simples como senhas incorretas, resolução de nome de rede ou controladores de domínio offline críticos.  
  
    2.  Examine a Dcpromoui.log e dcpromo.log para os erros mostrados na saída do, em seguida, funciona para trás deles para ver as indicações de motivo da falha.  
  
        1.  Sempre compare com um log de amostra de funcionar  
  
        2.  Examine os logs de ADPrep para erros somente se os resultados indicarem um problema de estender o esquema ou a preparação do domínio ou floresta.  
  
        3.  Examine o log de eventos de implantação DirectoryServices para erros somente se o Dcpromoui.log não tem detalhes ou termina arbitrariamente devido a uma exceção não tratada no processo de configuração.  
  
    3.  Examine os logs de eventos de serviços de diretório, sistema e aplicativo para outros indicadores de um problema de configuração. Muitas vezes, a promoção de controlador de domínio é apenas um sintoma de outros erros de configuração de rede que possam afetar sistemas distribuídos tudo.  
  
    4.  Use dcdiag.exe e repadmin.exe para validar a integridade geral da floresta e indicar configurações incorretas sutis que podem impedir que ainda mais promoção de controlador de domínio.  
  
    5.  Use AutoRuns.exe, Gerenciador de tarefas ou MSinfo32.exe para examinar o computador para o software de terceiros que pode estar interferindo.  
  
        1.  Remover o software de terceiros (não simplesmente desabilitar o software; que não impede o carregamento de drivers).  
  
    6.  Instale NetMon 3.4 no computador que não pode ser bem promover o controlador de domínio do parceiro de replicação e analisar o processo de promoção com capturas de rede dupla face.  
  
        1.  Compare isso para o seu ambiente de laboratório de trabalho para entender a aparência de uma promoção saudável e onde ele está falhando.  
  
        2.  Neste ponto, os erros são mais prováveis com os objetos de floresta, alterações de segurança não padrão ou a rede, e esse novo controlador de domínio for vítima de configurações incorretas de DNS, firewalls, software de proteção de invasões de host ou outros fatores externos.  
  
### <a name="troubleshooting-specific-problems"></a>Solução de problemas específicos  
  
#### <a name="events-and-error-messages"></a>Mensagens de erro e eventos  
Promoção de controlador de domínio e rebaixamento sempre retorna um código no final da operação e ao contrário da maioria dos programas, faça zero não retorno para o sucesso. Para ver o código no final de uma configuração de controlador de domínio, você tem várias opções:  
  
1.  Ao usar o Gerenciador do servidor, examine os resultados de promoção em dez segundos antes da reinicialização automática.  
  
2.  Ao usar ADDSDeployment Windows PowerShell, examine os resultados de promoção em dez segundos antes da reinicialização automática. Como alternativa, optar por não reiniciar automaticamente após a conclusão. Você deve adicionar o **Format-List** pipeline para facilitar a leitura de saída. Por exemplo:  
  
    ```  
    Install-addsdomaincontroller <options> -norebootoncompletion:$true | format-list  
  
    ```  
  
    Erros de validação de pré-requisito e a verificação não passar uma reinicialização, portanto, elas são visíveis em todos os casos. Por exemplo:  
  
  ![Solução de problemas](media/Troubleshooting-Domain-Controller-Deployment/ADDS_PSPrereqError.png)  
  
3.  Em qualquer cenário, examine a dcpromo.log e dcpromoui.log.  
  
    > [!NOTE]  
    > Alguns dos erros listados abaixo não são possíveis devido a alterações de configuração de controlador de sistema operacional e domínio em sistemas operacionais posteriores. Os novos códigos ADDSDeployment Windows PowerShell também impede que determinados erros, mas o dcpromo.exe//unattend não; Esse é outro motivo convincente para alternar todos da sua automação atual do DCPromo preterida para ADDSDeployment Windows PowerShell.  
  
Promoção e rebaixamento códigos de retorno o sucesso a seguinte mensagem.  
  
|Código de erro|Explicação|Observação|  
|--------------|---------------|--------|  
|1|Sair, êxito|Você ainda deve reinicializar, esse apenas as anotações que a reinicialização automática sinalizador foi removido|  
|2|Sair, sucesso, é necessário reinicializar||  
|3|Sair, sucesso, com uma falha não críticas|Normalmente visto quando retornar o aviso de delegação de DNS. Se não configurar delegação de DNS, use:<br /><br />-creatednsdelegation: $false|  
|4|Sair, sucesso, com uma falha não críticas, é necessário reinicializar|Normalmente visto quando retornar o aviso de delegação de DNS. Se não configurar delegação de DNS, use:<br /><br />-creatednsdelegation: $false|  
  
Promoção e rebaixamento códigos de retorno a falha a seguinte mensagem. Também há chances de ser uma mensagem de erro estendida; sempre ler o erro inteiro com cuidado, não apenas a parte numérica.  
  
|Código de erro|Explicação|Resolução sugerida|  
|--------------|---------------|------------------------|  
|11|Promoção de controlador de domínio já estiver em execução|Não execute que uma instância de promoção de controlador de domínio ao mesmo tempo para o mesmo computador de destino|  
|12|Usuário deve ser administrador|Faça logon como um membro do integrados administradores de grupo e garantir que sejam elevar com o UAC|  
|13|Autoridade de certificação está instalada|Não é possível rebaixar neste controlador de domínio, como também é uma autoridade de certificação. Fazer não remove a CA antes de inventário cuidadosamente sua utilização - se ele está emitindo certificados, removendo a função fará com que uma interrupção. Executar CAs em controladores de domínio não é recomendado|  
|14|Em execução no modo de segurança para inicialização|Inicialize o servidor no modo normal|  
|15|Alteração da função está em andamento ou precisa reinicializar|Você deve reiniciar o servidor (devido a alterações de configuração anterior) antes de promoção|  
|16|Em execução na plataforma errada|*Provavelmente não obter esse erro*|  
|17|Não existem nenhum unidades NTFS 5|Esse erro não é possível no Windows Server 2012, que requer pelo menos % systemdrive % ser formatada com NTFS|  
|18|Não há espaço suficiente em windir|Liberar espaço no volume % systemdrive % usando cleanmgr.exe|  
|19|Nome alterar pendente, precisa reinicializar|Reinicialize o servidor|  
|20|Nome do computador é sintaxe inválida|Renomeie o computador com um nome válido|  
|21|Neste controlador de domínio contém funções FSMO, é um GC, e/ou é um servidor DNS|Adicionar **- demoteoperationmasterrole** ao usar **- forceremoval**.|  
|22|TCP/IP deve ser instalado ou não está funcionando|Verifique se o computador tiver TCP/IP configurado, vinculados e funcionando corretamente|  
|23|Cliente DNS precisa ser configurado pela primeira vez|Configurar um servidor DNS primário ao adicionar um novo controlador de domínio em um domínio|  
|24|Credenciais fornecidas são inválidos ou ausentes elementos necessários|Verifique se seu nome de usuário e senha está correta|  
|25|Controlador de domínio para o domínio especificado não pôde ser localizado|Validar as configurações do cliente DNS, as regras de firewall|  
|26|Lista de domínios não pôde ser lido da floresta|Validar as configurações do cliente DNS, funcionalidade LDAP, as regras de firewall|  
|27|Nome de domínio ausente|Especificar um domínio quando elevar ou rebaixar|  
|28|Nome de domínio ruim|Escolha um nome de domínio DNS diferente e válido ao promover|  
|29|Domínio pai não existe|Verifique o domínio pai especificado ao criar um novo domínio filho ou o domínio de árvore|  
|30|Domínio não na floresta|Verifique se o nome de domínio fornecido|  
|31|Já existe um domínio filho|Especifique um nome de domínio diferente|  
|32|Nome de domínio NetBIOS ruim|Especifique um nome de domínio NetBIOS válido|  
|33|Caminho para os arquivos IFM é inválido|Validar seu caminho para a pasta de instalação de mídia|  
|34|O banco de dados IFM é ruim|Use a instalação da mídia correta para essa função (mesma versão do sistema operacional, mesmo tipo de controlador de domínio - RODC versus RWDC) e o sistema operacional|  
|35|SYSKEY ausente|A instalação da mídia é criptografada e você deve fornecer um SYSKEY válido para usá-lo|  
|37|Caminho para o banco de dados NTDS ou seus logs é inválido|Alterar o caminho do banco de dados e Logs para um volume NTFS fixo, não uma unidade mapeada ou caminho UNC|  
|38|Volume não tem espaço suficiente para o banco de dados NTDS ou logs|Liberar espaço usando cleanmgr.exe, adicionar mais espaço em disco, limpar manualmente o espaço, movendo dados desnecessários em outro lugar|  
|39|Caminho para SYSVOL é inválido|Alterar o caminho da pasta SYSVOL para um volume NTFS fixo, não uma unidade mapeada ou caminho UNC|  
|40|Nome do site inválido|Forneça um nome de site que existe|  
|41|Para especificar uma senha para o modo de segurança|Fornecer uma senha para a conta DSRM, ele não pode ser em branco não importa como a política de senha é configurada|  
|42|Senha do modo de segurança não atende aos critérios (promoção somente)|Fornecer uma senha para a conta DSRM que atenda às regras configuradas da política de senha|  
|43|Senha de administrador não atende aos critérios (rebaixamento somente)|Fornecer uma senha para a conta de administrador local que atenda a política de senha configurada regras|  
|44|O nome da floresta especificado é inválido|Especifique um nome de domínio DNS válido floresta raiz|  
|45|Uma floresta com o nome especificado já existir|Escolha um nome de domínio DNS diferentes floresta raiz|  
|46|O nome especificado para a árvore é inválido|Especifique um nome de domínio DNS árvore válido|  
|47|Uma árvore com o nome especificado já existir|Escolha um nome de domínio DNS árvore diferente|  
|48|O nome da árvore não se encaixa na estrutura de floresta|Escolha um nome de domínio DNS árvore diferente|  
|49|Não existe no domínio especificado|Verifique se o nome de domínio digitado|  
|50|Durante a diminuir níveis, o último controlador de domínio foi detectado mesmo que não é, ou o último controlador de domínio foi especificado, mas não é|Não especifique **último controlador de domínio no domínio** (**- lastdomaincontrollerindomain**), a menos que ele é true. Use **- ignorelastdcindomainmismatch** substituir se isso é realmente o último controlador de domínio e não há metadados de controlador de domínio fantasma|  
|51|Partições de aplicativo existem no controlador de domínio|Especificar a **remover aplicativo partições** (**- removeapplicationpartitions**)|  
|52|Necessário argumento de linha de comando está ausente (ou seja, um arquivo de resposta deve ser especificado na linha de comando)|*Somente visto com dcpromo//unattend, que foi preterido. Consulte a documentação mais antiga*|  
|53|O promoção/rebaixamento falhou, o computador deve ser reinicializado para limpeza|Examinar os logs e erro estendido|  
|54|O promoção/rebaixamento falhou|Examinar os logs e erro estendido|  
|55|O promoção/rebaixamento foi cancelado pelo usuário|Examinar os logs e erro estendido|  
|56|O promoção/rebaixamento foi cancelado pelo usuário, o computador deve ser reinicializado para limpar|Examinar os logs e erro estendido|  
|58|Um nome de site deve ser especificado durante a promoção de RODC|Você deve especificar um site para um RODC, ele não detectará automaticamente um como um RWDC|  
|59|Durante a diminuir níveis, neste controlador de domínio é o último servidor DNS para uma de suas zonas|Especifique que esse é o **último servidor DNS no domínio** ou use **- ignorelastdnsserverfordomain**|  
|60|Um controlador de domínio que executam o Windows Server 2008 ou posterior deve estar presente no domínio para promover RODC|Promover pelo menos um controlador de domínio gravável de modelo posterior ou Windows Server 2008|  
|61|Você não pode instalar os serviços de domínio do Active Directory com DNS em um domínio existente que não hospeda DNS|*Não é possível obter esse erro*|  
|62|Arquivo de resposta não tem uma seção [DCInstall]|*Somente visto com dcpromo//unattend, que foi preterido. Consulte a documentação mais antiga.*|  
|63|Nível funcional da floresta está abaixo servidor windows 2003|Aumentar o nível funcional da floresta para pelo menos Windows Server 2003 nativo. Windows 2000 e Windows NT 4.0 não estão mais sistemas operacionais com suporte|  
|64|Promo falhou porque a detecção de binários de componente falhou|Instale a função AD DS|  
|65|Promo falhou porque Falha na instalação do componente binário|Instale a função AD DS|  
|66|Promo falhou porque Falha na detecção de sistema operacional|Examinar os logs; e erro estendido o servidor está falhando retornar a versão do sistema operacional. É provável que o computador precisa ser reinstalado, como sua integridade geral é altamente suspeita|  
|68|Parceiros de replicação são inválido|Use repadmin.exe ou o **obter-ADReplication\ *** do Windows PowerShell para validar a integridade do controlador de domínio de parceiro|  
|69|Porta necessária já está em uso por algum outro aplicativo|Use **netstat.exe - anob** para localizar os processos que são atribuídos incorretamente a portas reservadas do AD DS|  
|70|Controlador de domínio raiz da floresta deve ser um GC|*Somente visto com dcpromo//unattend, que foi preterido. Consulte a documentação mais antiga*|  
|71|Servidor DNS já está instalado|Não especifique para instalar o DNS (**- installDNS**) se o serviço DNS já está instalado|  
|72|Computador estiver executando os serviços de área de trabalho remota no modo de administrador não|Você não pode promover neste controlador de domínio, como também é um servidor RDS configurado para mais de dois usuários de administrador. Não remova RDS antes de você inventariar cuidadosamente sua utilização - se ele está sendo usado por aplicativos ou os usuários finais, remoção fará com que uma interrupção de fazer|  
|73|O nível funcional da floresta especificada é inválido.|Especificar um nível funcional da floresta válido|  
|74|O nível funcional do domínio especificado é inválido.|Especificar um nível funcional do domínio válido|  
|75|Não é possível determinar a política de replicação de senha padrão.|Validar se a política de replicação de Senha RODC existe e está acessível|  
|76|Grupos de segurança especificados replicados/não replicado são inválidos|Validar que você digitou contas de usuário e domínio válidas ao especificar uma política de replicação de senha|  
|77|O argumento especificado é inválido|Examinar os logs e erro estendido|  
|78|Falha ao examinar floresta do Active Directory|Examinar os logs e erro estendido|  
|79|Não pode ser promovido RODC porque rodcprep ainda não foi executado.|Use o Windows Server 2012 para preparar a floresta ou use **adprep.exe /rodcprep**|  
|80|DomainPrep ainda não foi executado.|Usar o Windows Server 2012 para preparar o domínio ou usar **adprep.exe /domainprep**|  
|81|ForestPrep ainda não foi executado.|Use o Windows Server 2012 para preparar a floresta ou use **adprep.exe /forestprep**|  
|82|Incompatibilidade de esquema de floresta|Use o Windows Server 2012 para preparar a floresta ou use **adprep.exe /forestprep**|  
|83|SKU sem suporte|*Provavelmente não obter esse erro*|  
|84|Não é possível detectar uma conta de controlador de domínio|Provar que controladores de domínio existentes tem atributo de controle de conta de usuário correto definido.|  
|85|Não é possível selecionar uma conta de controlador de domínio para o estágio 2|Retornado se você especificar "Conta existente do uso", mas qualquer nenhuma conta encontrado ou houver um erro durante a pesquisa de conta. Certifique-se de que você forneceu a conta correta de RODC preparada|  
|86|É necessário para executar a promoção do estágio 2|Retornado se você promover um controlador de domínio adicional, mas existe uma conta existente e "Permitir que reinstalar" não foi especificado|  
|87|Existe uma conta de controlador de domínio do tipo conflitante|Renomeie o computador antes de promover, se não tentando se conectar a um controlador de domínio não ocupada. Você deve anexar para o controlador de domínio não ocupado conta usando **- useexistingaccount** e o argumento somente leitura ou gravável correto, dependendo do tipo de conta|  
|88|O administrador de servidor especificado não é válido|Você especificou uma conta para delegação de administração do RODC inválida. Verifique se a conta especificada é um usuário ou grupo válido|  
|89|Mestre RID para o domínio especificado está offline.|Use **netdom.exe consulta fsmo** para detectar o mestre RID. Colocá-lo online e torná-lo acessível para o controlador de domínio que você está promovendo|  
|90|Mestre de nomeação de domínio está offline.|Use **netdom.exe consulta fsmo** para detectar o mestre de nomeação de domínio. Colocá-lo online e torná-lo acessível para o controlador de domínio que você está promovendo|  
|91|Falha ao detectar se o processo é wow64|*Não é possível obter esse erro mais nada, o sistema operacional é 64 bits*|  
|92|Não há suporte para o processo WOW64|*Não é possível obter esse erro mais nada, o sistema operacional é 64 bits*|  
|93|Serviço de controlador de domínio não está funcionando para rebaixamento não forçado|Iniciar o serviço do AD DS|  
|94|Senha de administrador local não atender requisito: em branco ou não é necessária|Fornecer uma senha não está em branco e certifique-se de que a política de senha local requer uma senha|  
|95|Não é possível rebaixar último Windows Server 2008 ou posterior controlador de domínio no domínio onde existem RODCs ao vivo|Você deve primeiro rebaixar todos os RODCs antes de rebaixar todos os controladores de domínio gravável posteriores ou Windows Server 2008|  
|96|Não é possível desinstalar binários DS|*Somente visto com dcpromo//unattend, que foi preterido. Consulte a documentação mais antiga*|  
|97|Floresta funcional nível versão maior do que o sistema de operacional domínio filho|Fornecer um domínio filho funcional mesmo ou maior do que o nível funcional da floresta|  
|98|Componente binário instalação/desinstalação está em andamento.|*Somente visto com dcpromo//unattend, que foi preterido. Consulte a documentação mais antiga*|  
|99|É muito baixo nível funcional da floresta (erro é Windows Server 2012 apenas)|Aumentar o nível funcional da floresta para pelo menos Windows Server 2003 nativo. Windows 2000 e Windows NT 4.0 não estão mais sistemas operacionais com suporte|  
|100|Nível funcional do domínio é muito baixo (erro é Windows Server 2012 apenas)|Aumentar o nível funcional de domínio para pelo menos Windows Server 2003 nativo. Windows 2000 e Windows NT 4.0 não estão mais sistemas operacionais com suporte|  
  
#### <a name="knownlikely-issues-and-support-scenarios"></a>Problemas conhecidos/provavelmente e cenários de suporte  
A seguir estão alguns problemas comuns vistos durante o processo de desenvolvimento do Windows Server 2012. Todos esses problemas são "por design" e tem uma solução alternativa válida ou técnica mais apropriada para evitá-los em primeiro lugar. Muitos desses comportamentos são idênticos no Windows Server 2008 R2 e sistemas operacionais mais antigos, mas a reconfiguração de implantação do AD DS traz sensibilidade questões sobre questões.  
  
|Problema|Rebaixar um controlador de domínio deixa em execução com nenhum zonas DNS|  
|---------|-----------------------------------------------------------------|  
|Sintomas|Servidor ainda responde às solicitações DNS, mas não tem nenhuma informação de zona|  
|Resolução e anotações|Ao remover a função AD DS, também remover a função de servidor DNS ou definir o serviço de servidor DNS como desabilitada. Lembre-se apontar o cliente DNS para outro servidor que ele próprio. Se estiver usando o Windows PowerShell, execute o seguinte depois que você rebaixar o servidor:<br /><br />Código - windowsfeature desinstalar dns<br /><br />ou<br /><br />Código - conjunto de serviço dns - starttype desabilitado<br />Parar o serviço dns|  
  
|Problema|Promover um Windows Server 2012 em um domínio de rótulo único existente não configura updatetopleveldomain = 1 ou allowsinglelabeldnsdomain = 1|  
|---------|----------------------------------------------------------------------------------------------------------------------------------------------------|  
|Sintomas|Registro dinâmico de DNS não ocorre|  
|Resolução e anotações|Defina esses valores usando as políticas de grupo logon de rede e DNS. A Microsoft começou a bloquear a criação de rótulo único domínio no Windows Server 2008; Você pode usar o ADMT ou a ferramenta de renomeie de domínio para mudar para uma estrutura de domínio DNS aprovada.|  
  
|Problema|Rebaixamento do último controlador de domínio em um domínio falha quando há contas RODC desocupadas, pré-criados|  
|---------|------------------------------------------------------------------------------------------------------------|  
|Sintomas|Rebaixamento falha com a mensagem:<br /><br />**Dcpromo.General.54**<br /><br />Serviços de domínio do Active Directory não conseguiu encontrar outro Active Directory controlador de domínio transferir os dados restantes na partição de diretório CN = Schema, CN = Configuration, DC = corp, DC = contoso, DC = com.<br /><br />"O formato do nome do domínio especificado é inválido."|  
|Resolução e anotações|Remover qualquer restante pré-criada contas RODC antes de rebaixá um domínio, usando **Dsa.msc** ou **Ntdsutil.exe metadados limpeza**.|  
  
|Problema|Preparação de floresta e domínio automatizada não executa GPPREP|  
|---------|---------------------------------------------------------------|  
|Sintomas|Funcionalidade de planejamento entre domínios para política de grupo, modo de planejamento do conjunto resultante de diretivas (RSOP), requer o sistema de arquivos atualizado e permissões do Active Directory para GP existente. Sem Gpprep, você não pode usar o RSOP planejamento entre domínios.|  
|Resolução e anotações|Executar **adprep.exe /gpprep** manualmente para todos os domínios que não foram preparados anteriormente para o Windows Server 2003, Windows Server 2008 ou Windows Server 2008 R2. Os administradores devem executar GPPrep apenas uma vez no histórico de um domínio, não com cada atualização. Ele não é executado por adprep automática porque se você já definiu as permissões adequadas personalizadas, ele faria com que todo o conteúdo SYSVOL replicar novamente em todos os controladores de domínio.|  
  
|Problema|Instalação de mídia não é confirmada quando apontando para um caminho UNC|  
|---------|------------------------------------------------------------------|  
|Sintomas|Erro retornado:<br /><br />Código - não foi possível validar o caminho de mídia. Exceção chamada "GetDatabaseInfo" com "2" argumentos. A pasta não é válida.|  
|Resolução e anotações|Você deve armazenar arquivos IFM em um disco local, não um caminho UNC remoto. Este bloco intencional evita a promoção de servidor parcial devido a uma interrupção na rede.|  
  
|Problema|Aviso de delegação de DNS mostrado duas vezes durante a promoção de controlador de domínio|  
|---------|-------------------------------------------------------------------------|  
|Sintomas|Aviso retornado *duas vezes* ao promover usando o ADDSDeployment Windows PowerShell:<br /><br />Código - "não é possível criar uma delegação para esse servidor DNS porque a zona pai autoritativo não foi encontrada ou não executa Windows DNS server. Se você estiver integrando com uma infraestrutura DNS existente, você deve criar manualmente uma delegação para esse servidor DNS na zona pai para garantir a resolução de nomes confiável de fora do domínio. Caso contrário, nenhuma ação é necessária."|  
|Resolução e anotações|Ignore. ADDSDeployment Windows PowerShell mostra o aviso primeiro durante a verificação de pré-requisitos, e depois novamente durante o passo de configuração do controlador de domínio. Se você não quiser configurar delegação de DNS, use o argumento:<br /><br />Código - - creatednsdelegation: $false<br /><br />Fazer *não* ignore as verificações de pré-requisito para suprimir esta mensagem|  
  
|Problema|Especificar o nome UPN ou credenciais de domínio não durante a configuração retorna erros enganosos|  
|---------|--------------------------------------------------------------------------------------------|  
|Sintomas|Gerenciador do servidor retorna o erro:<br /><br />Código - exceção chamada "DNSOption" com "6" argumentos<br /><br />ADDSDeployment Windows PowerShell retorna o erro:<br /><br />Falha no código - verificação de permissões de usuário. Você deve fornecer o nome do domínio ao qual essa conta de usuário pertence.|  
|Resolução e anotações|Certifique-se de que você está fornecendo credenciais de domínio válido na forma de **domínio \ usuário**.|  
  
|Problema|Removendo a função DirectoryServices-DomainController usando o Dism.exe leva a servidor não inicializável|  
|---------|---------------------------------------------------------------------------------------------------|  
|Sintomas|Se usando o Dism.exe para remover a função AD DS antes de rebaixá normalmente um controlador de domínio, o servidor não for inicializado normalmente e mostra o erro:<br /><br />Código - Status: 0x000000000<br />Informações: Ocorreu um erro inesperado.|  
|Resolução e anotações|Inicializar no modo de reparo de serviços de diretório usando *Shift + F8*. Adicione novamente a função do AD DS e, em seguida, rebaixar forçadamente o controlador de domínio. Como alternativa, restaure o estado do sistema de backup. Não use Dism.exe para remoção de função do AD DS; o utilitário não tem conhecimento dos controladores de domínio.|  
  
|Problema|Instalar uma nova floresta Falha ao definir forestmode como Win2012|  
|---------|--------------------------------------------------------------------|  
|Sintomas|Promoção usando o Windows PowerShell do ADDSDeployment retorna o erro:<br /><br />Código - Test.VerifyDcPromoCore.DCPromo.General.74<br /><br />Falha na verificação de pré-requisitos para promoção de controlador de domínio. O nível funcional do domínio especificado é inválido|  
|Resolução e anotações|Não especifique um modo funcional da floresta de Win2012 sem *também* especificando um modo de Win2012 funcional de domínio. Aqui está um exemplo que funcionará sem erros:<br /><br />Código - - forestmode Win2012 - domainmode Win2012]|  
  
|||  
|-|-|  
|Problema|Clique em verificar a instalação da área de seleção de mídia aparece nada|  
|Sintomas|Quando você especifica um caminho para uma pasta IFM, clicando no **verificar** botão nunca retorna uma mensagem ou parece fazer nada.|  
|Resolução e anotações|O **verificar** botão retorna apenas erros quando há problemas. Caso contrário, facilita a **próxima** botão selecionável se você tiver fornecido um caminho IFM. Você deve clicar **verificar** proceder se você tiver selecionado IFM.|  
  
|||  
|-|-|  
|Problema|Rebaixando com o Gerenciador do servidor não fornece comentários até concluído.|  
|Sintomas|Ao usar o Gerenciador do servidor para remover a função AD DS e rebaixar um controlador de domínio, não há nenhum feedback contínua fornecido até que o rebaixamento é concluída ou falhe.|  
|Resolução e anotações|Esta é uma limitação do Gerenciador de servidores. Comentários, use o cmdlet ADDSDeployment do Windows PowerShell:<br /><br />Código - Uninstall-addsdomaincontroller|  
  
|||  
|-|-|  
|Problema|Instalação de mídia verificar não detecta que a mídia RODC fornecida para o controlador de domínio gravável ou vice-versa.|  
|Sintomas|Ao promover um novo controlador de domínio usando IFM e fornecer mídia incorreta para IFM - como mídia RODC para um controlador de domínio gravável ou mídia RWDC para um RODC - o botão Verificar não retornam um erro. Mais tarde, promoção falha com erro:<br /><br />Código - ocorreu um erro ao tentar configurar este computador como um controlador de domínio. <br />A promoção Install-From-Media de um Read-Only DC não pode ser iniciado porque o banco de dados de origem especificado não é permitido. Somente bancos de dados de outros RODCs podem ser usados para promoção IFM de um RODC.|  
|Resolução e anotações|Verifique se apenas valida a integridade geral do IFM. Não forneça ao tipo incorreto de IFM para um servidor. Reinicie o servidor antes de tentar promoção novamente com a mídia correta.|  
  
|||  
|-|-|  
|Problema|Promover um RODC em uma conta de computador pré-criado falhar|  
|Sintomas|Ao usar ADDSDeployment Windows PowerShell para promover um novo RODC com uma conta de computador preparados, receba o erro:<br /><br />Código - conjunto de parâmetro não pode ser resolvido usando os parâmetros nomeados especificado.    <br />InvalidArgument: ParameterBindingException<br />    + FullyQualifiedErrorId: AmbiguousParameterSet, Microsoft.DirectoryServices.Deployment.PowerShell.Commands.Install|  
|Resolução e anotações|Não forneça parâmetros já já definidos em uma conta RODC pré-criados. Esses cenários incluem:<br /><br />Código - - readonlyreplica<br />-installdns<br />-donotconfigureglobalcatalog<br />-nome do site<br />-installdns|  
  
|||  
|-|-|  
|Problema|Desmarcar/selecionar "Reiniciar cada servidor de destino automaticamente se necessário" não faz nada|  
|Sintomas|Se selecionar (ou não selecionando) a opção de Gerenciador do servidor **reiniciar cada servidor de destino automaticamente, se necessário** whendemoting um controlador de domínio por meio de remoção de função, o servidor sempre reinicia, independentemente de preferência.|  
|Resolução e anotações|Isso é intencional. O processo de rebaixamento reinicia o servidor, essa configuração independentemente.|  
  
|||  
|-|-|  
|Problema|Dcpromo.log mostra "segurança de configuração [Erro] em arquivos de servidor falhou com 2"|  
|Sintomas|Rebaixamento de um controlador de domínio for concluído sem problemas, mas o exame do log dcpromo mostra o erro:<br /><br />Falha no código - [Erro] definindo a segurança em arquivos de servidor com 2|  
|Resolução e anotações|Ignorar, erro é esperado e estética.|  
  
|||  
|-|-|  
|Problema|Verificação de pré-requisito adprep falha com erro "Não é possível executar a verificação de conflito de esquema do Exchange"|  
|Sintomas|Quando você tenta promover um controlador de domínio do Windows Server 2012 em uma existente do Windows Server 2003, Windows Server 2008 ou Windows Server 2008 R2 floresta, a verificação de pré-requisitos falha com erro:<br /><br />Falha no código - verificação de pré-requisitos para preparação de anúncios. Não é possível executar a verificação de conflito de esquema do Exchange para domínio *<domain name>* (exceção: o servidor RPC não está disponível)<br /><br />O adprep.log mostra o erro:<br /><br />Código - Adprep não pôde recuperar dados do servidor*<domain controller>*<br /><br />Por meio de instrumentação de gerenciamento do Windows (WMI).|  
|Resolução e anotações|O novo controlador de domínio não pode acessar o WMI por meio de protocolos DCOM/RPC em relação aos controladores de domínio existentes. Até o momento, houve três causas para isso:<br /><br />-Um firewall regra bloqueia o acesso aos controladores de domínio existentes<br /><br />-A conta de serviço de rede está ausente no "Logon como um serviço" privilégio (SeServiceLogonRight) nos controladores de domínio existentes<br /><br />-NTLM está desabilitada em controladores de domínio, usando políticas de segurança descritas em [apresentando a restrição de autenticação NTLM](https://technet.microsoft.com/library/dd560653(WS.10).aspx)|  
  
|||  
|-|-|  
|Problema|Criando uma nova floresta do AD DS sempre mostra o aviso de DNS|  
|Sintomas|Quando criar uma nova floresta do AD DS e criar a zona DNS no novo controlador de domínio para si mesmo, você sempre recebem mensagem de aviso:<br /><br />Código - um erro foi detectado no passo de configuração DNS. <br />Nenhum dos servidores DNS usados por este computador respondeu dentro do intervalo de tempo limite.<br />(código de erro 0x000005B4 "ERROR_TIMEOUT")|  
|Resolução e anotações|Ignore. Esse aviso é intencional no primeiro controlador de domínio no domínio raiz de uma nova floresta, no caso de você pretendia apontar para um servidor DNS existente e zona.|  
  
|||  
|-|-|  
|Problema|Windows PowerShell - whatif argumento retorna informações incorretas de servidor DNS|  
|Sintomas|Se você usar o **- whatif** argumento quando estiver configurando um controlador de domínio com implícito ou explícito **- installdns: $true**, a saída resultante mostra:<br /><br />Código - "servidor DNS: não"|  
|Resolução e anotações|Ignore. DNS é instalado e configurado corretamente.|  
  
|||  
|-|-|  
|Problema|Depois de promoção, logon falha com "armazenamento insuficiente está disponível para processar este comando"|  
|Sintomas|Depois de promover um novo controlador de domínio e, em seguida, faça logoff e tente fazer logon interativamente, você recebe o erro:<br /><br />Código - armazenamento insuficiente está disponível para processar este comando|  
|Resolução e anotações|O controlador de domínio não foi reinicializado após a promoção, devido a um erro ou porque você especificou o argumento ADDSDeployment Windows PowerShell **- norebootoncompletion **. Reinicie o controlador de domínio.|  
  
|||  
|-|-|  
|Problema|O botão Avançar não está disponível na página Opções de controlador de domínio|  
|Sintomas|Mesmo que você tiver definido uma senha, o **próxima** botão o **opções de controlador de domínio** página no Gerenciador do servidor não está disponível. Não há nenhum site listado no **nome do Site** menu.|  
|Resolução e anotações|Você tem vários sites AD DS e pelo menos um está ausente sub-redes; neste controlador de domínio futuras pertence a uma dessas sub-redes. Você deve selecionar manualmente a sub-rede no menu de lista suspensa de nome do Site. Você também deve revisar todos os sites AD usando DSSITE.MSC ou use o seguinte comando do Windows PowerShell para localizar todos os sites sub-redes ausentes:<br /><br />Código - adreplicationsite get-filtro *-sub-redes propriedade & #124; WHERE-object {! $_.subnets - eq "\ *"} & #124; nome da tabela de formato|  
  
|||  
|-|-|  
|Problema|Promoção ou rebaixamento falha com a mensagem "o serviço não pode ser iniciado"|  
|Sintomas|Se você tentar promoção, rebaixamento ou clonagem de um controlador de domínio você receber o erro:<br /><br />Código - o serviço não pode ser iniciado, ou porque ela é desabilitada ou não tem dispositivos ativados associados a ele"(0x80070422)<br /><br />O erro pode ser interativo, um evento, ou gravado em um log como dcpromoui.log ou dcpromo.log|  
|Resolução e anotações|O serviço de servidor de função DS (DsRoleSvc) está desabilitado. Por padrão, esse serviço é instalado durante a instalação de função do AD DS e defina como um tipo de inicialização Manual. Não desative esse serviço. Defina-o novamente como Manual e permitir que as operações de função DS iniciar e pará-lo sob demanda. Esse comportamento ocorre por design.|  
  
|||  
|-|-|  
|Problema|Gerenciador do servidor ainda avisa que você precisa promover DC|  
|Sintomas|Se você promove um controlador de domínio usando preteridas dcpromo.exe//unattend ou atualiza um controlador de domínio existente do Windows Server 2008 R2 in-loco para o Windows Server 2012, o Gerenciador do servidor ainda mostra a tarefa de configuração pós-implantação **promover esse servidor para um controlador de domínio **.|  
|Resolução e anotações|Clique no link de aviso pós-implantação e a mensagem desaparecerá definitivamente. Esse comportamento é superficial e esperado.|  
  
|||  
|-|-|  
|Problema|Script de implantação do Gerenciador do servidor ausentes instalação da função|  
|Sintomas|Se você promove um controlador de domínio usando o Gerenciador do servidor e salve o script de implantação do Windows PowerShell, elas não incluem o cmdlet de instalação de função e os argumentos (install-windowsfeature-nomear serviços de domínio do ad - includemanagementtools). Sem a função, o controlador de domínio não pode ser configurado.|  
|Resolução e anotações|Adicionar manualmente esse cmdlet e argumentos para os scripts. Esse comportamento é esperado e por design.|  
  
|||  
|-|-|  
|Problema|Script de implantação do Gerenciador do servidor não foi nomeado PS1|  
|Sintomas|Se você promove um controlador de domínio usando o Gerenciador do servidor e salve o script de implantação do Windows PowerShell, o arquivo é chamado com um nome temporário aleatório e não como um arquivo PS1.|  
|Resolução e anotações|Renomeie o arquivo manualmente. Esse comportamento é esperado e por design.|  
  
|Problema|Dcpromo//unattend permite que os níveis funcionais sem suporte|  
|-|-|  
|Sintomas|Se você promover um controlador de domínio usando dcpromo//unattend com o arquivo de resposta de exemplo a seguir:<br /><br />Código:<br /><br />[DCInstall]<br />NewDomain = floresta<br /><br />ReplicaOrNewDomain = domínio<br /><br />NewDomainDNSName = corp.contoso.com<br /><br />SafeModeAdminPassword =Safepassword@6<br /><br />DomainNetbiosName = corp<br /><br />DNSOnNetwork = Yes<br /><br />AutoConfigDNS = Yes<br /><br />RebootOnSuccess = NoAndNoPromptEither<br /><br />RebootOnCompletion = não<br /><br />*DomainLevel = 0*<br /><br />*ForestLevel = 0*<br /><br />Promoção falha com os seguintes erros no dcpromoui.log:<br /><br />Código - dcpromoui EA4.5B8 0089 13:31:50.783 Enter CArgumentsSpec::ValidateArgument DomainLevel<br /><br />Dcpromoui EA4.5B8 008A 13:31:50.783 valor para DomainLevel é 0<br /><br />Dcpromoui EA4.5B8 008B 13:31:50.783 código de saída é 77<br /><br />Dcpromoui EA4.5B8 008C 13:31:50.783 o argumento especificado é inválido.<br /><br />Log de fechamento de 13:31:50.783 008D EA4.5B8 Dcpromoui<br /><br />Dcpromoui 0032 EA4.5B8 13:31:50.830 código de saída é 77<br /><br />Nível 0 é o Windows 2000, que não tem suporte no Windows Server 2012.|  
|Resolução e anotações|Não use preteridas dcpromo//unattend e entender o que ele permite especificar configurações inválidas que falham posterior. Esse comportamento é esperado e por design.|  

|Problema|Promoção "travamentos" em criação de objeto de configurações NTDS, nunca é concluída|  
|-|-|  
|Sintomas|Se você promover uma réplica DC ou RODC, a promoção atinge "objeto de configurações de criação NTDS" e nunca receita ou é concluída. Os logs interrompe a atualização também.|  
|Resolução e anotações|Isso é um problema conhecido causado fornecendo credenciais de conta de administrador local interno com uma senha correspondente ao domínio conta de administrador interno. Isso faz com que uma falha para baixo no mecanismo de instalação de núcleo que faz o erro não, mas em vez disso, aguardará indefinidamente (quase loop). Isso é esperado - embora indesejável - comportamento.<br /><br />Para corrigir o servidor:<br /><br />1. Reiniciá-lo.<br /><br />1. No AD, exclua a conta do computador do servidor membro (ele não ainda será uma conta de DC)<br /><br />1. No servidor, forçadamente disjoin-lo do domínio<br /><br />1. No servidor, remova a função AD DS.<br /><br />1. Reinicialização<br /><br />1. Adicionar novamente o AD DS função e nova promoção, garantindo que você sempre ofereça o ***domain\admin*** formatado credenciais para promoção de controlador de domínio e não apenas a conta de administrador local interno|  
