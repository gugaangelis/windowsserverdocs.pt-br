---
ms.assetid: 5ab76733-804d-4f30-bee6-cb672ad5075a
title: Solução de problemas de implantação de controlador de domínio
author: iainfoulds
ms.author: iainfou
manager: daveba
ms.date: 03/20/2019
ms.topic: article
ms.openlocfilehash: 8c850a9a09af97d9aa377b79aaa87d06aa0d916c
ms.sourcegitcommit: 1dc35d221eff7f079d9209d92f14fb630f955bca
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88940606"
---
# <a name="troubleshooting-domain-controller-deployment"></a>Solução de problemas de implantação de controlador de domínio

>Aplica-se a: Windows Server 2016

Este tópico abrange metodologia detalhada sobre solução de problemas de configuração do controlador de domínio e implantação.

## <a name="introduction-to-troubleshooting"></a>Introdução à solução de problemas

![Solução de problemas](media/Troubleshooting-Domain-Controller-Deployment/adds_deploy_troubleshooting.png)

## <a name="built-in-logs-for-troubleshooting"></a>Logs internos para solução de problemas

Os logs internos são os instrumentos mais importantes para solucionar problemas de promoção e rebaixamento do controlador de domínio. Todos esses logs são habilitados e configurados para o máximo de detalhamento, por padrão.

| Fase | Log |
|--|--|
| Gerenciador do Servidor ou operações ADDSDeployment do Windows PowerShell | - %systemroot%\debug\dcpromoui.log<p>-%SystemRoot%\debug\DCPROMOUI *. log |
| Instalação/Promoção do controlador de domínio | - %systemroot%\debug\dcpromo.log<p>-%SystemRoot%\debug\dcpromo *. log<p>-Evento viewer\Windows Windows\Sistema<p>-Evento viewer\Windows logs\Application<p>-Viewer\Applications de eventos e serviço logs\Directory de serviços<p>-Evento viewer\Applications e serviços de replicação do logs\File<p>-Viewer\Applications de eventos e replicação de serviços logs\DFS |
| Atualização de floresta ou domínio | -%SystemRoot%\debug\adprep \\ <datetime> \adprep.log<p>-%SystemRoot%\debug\adprep \\ <datetime> \csv.log<p>-%SystemRoot%\debug\adprep \\ <datetime> \dspecup.log<p>-%SystemRoot%\debug\adprep \\ <datetime> \ldif.log * |
| Mecanismo de implantação de ADDSDeployment do Windows PowerShell no Gerenciador do Servidor | -Viewer\Applications de eventos e serviços logs\Microsoft\Windows\DirectoryServices-Deployment\Operational |
| Serviço do Windows | - %systemroot%\Logs\CBS\\*<p>-% SystemRoot% \servicing\sessions\sessions.xml<p>- %systemroot%\winsxs\poqexec.log<p>-% SystemRoot% \winsxs\pending.xml |

### <a name="tools-and-commands-for-troubleshooting-domain-controller-configuration"></a>Ferramentas e comandos para solução de problemas de configuração do controlador de domínio

Para solucionar problemas não explicados pelos logs, use as seguintes ferramentas como um ponto de partida:

-   Dcdiag.exe

-   Repadmin.exe

-   [AutoRuns.exe](/sysinternals/downloads/autoruns), Gerenciador de tarefas e MSInfo32.exe

-   [Monitor de Rede 3.4](https://www.microsoft.com/download/en/details.aspx?displaylang=en&id=4865) (ou uma ferramenta de captura e análise de rede de terceiros)

### <a name="general-methodology-for-troubleshooting-domain-controller-configuration"></a>Metodologia geral para solução de problemas de configuração do controlador de domínio

1.  Um simples problema de sintaxe causou o erro?

    1.  Você digitou incorretamente ou se esqueceu de fornecer um argumento para o ADDSDeployment do Windows PowerShell? Por exemplo, se estiver usando o ADDSDeployment do Windows PowerShell, você se esqueceu de acrescentar o argumento necessário **-domainname** com um nome válido?

    2.  Examine a saída do console do Windows PowerShell com cuidado para ver exatamente por que há erro na análise da linha de comando fornecida.

2.  O erro é uma falha de pré-requisito?

    1.  Muitos erros que apareciam como promoção fatal estão agora impedidos pelo verificador de pré-requisitos.

    2.  Examine o texto dos erros de pré-requisito com cuidado. Eles fornecem a orientação necessária para resolver a maioria dos problemas, já que são cenários controlados.

3.  É erro na promoção e, portanto, fatal?

    1.  Examine os resultados com cuidado: muitos erros têm explicações simples, como senhas incorretas, resolução de nome de rede ou controladores de domínio offline críticos.

    2.  Examine o Dcpromoui.log e o dcpromo.log quanto aos erros mostrados na saída, depois trabalhe neles na ordem de chegada para ver indicações do motivo para a ocorrência da falha.

        1.  Compare sempre com um log de exemplo de trabalho

        2.  Examine se há erros nos logs ADPrep somente se os resultados indicarem um problema ao ampliar o esquema ou ao preparar a floresta ou o domínio.

        3.  Examine o log de eventos DirectoryServices-Deployment quanto a erros só se faltarem detalhes no Dcpromoui.log ou se ele terminar arbitrariamente devido a uma exceção sem tratamento no processo de configuração.

    3.  Examine os logs de eventos dos Serviços de Diretório, do Sistema e do Aplicativo quanto a outros indicadores de um problema de configuração. Muitas vezes, a promoção do controlador de domínio é apenas um sintoma de outra configuração incorreta de rede que afetaria todos os sistemas distribuídos.

    4.  Use dcdiag.exe e repadmin.exe para validar a integridade geral da floresta e indique configurações incorretas sutis que possam impedir outras promoções do controlador de domínio.

    5.  Use o AutoRuns.exe, o Gerenciador de Tarefas ou o MSinfo32.exe para examinar o computador quanto a software de terceiros que possam estar interferindo.

        1.  Remova o software de terceiros (não apenas desabilite o software; Isso não impede o carregamento dos drivers).

    6.  Instale o NetMon 3.4 no computador com erro para promover também o controlador de domínio do parceiro de replicação e analisar o processo de promoção com capturas de rede de dupla face.

        1.  Compare isso com o seu ambiente de laboratório de trabalho para entender o que é uma promoção íntegra e onde está a falha.

        2.  Neste ponto, é provável que ocorram erros com os objetos de floresta, as alterações de segurança não padrão ou a rede, e este novo controlador de domínio é vítima de configurações incorretas em DNS, firewalls, software de proteção contra invasão do host ou outros fatores externos.

## <a name="troubleshooting-events-and-error-messages"></a>Solucionando problemas de eventos e mensagens de erro

A promoção e o rebaixamento do controlador de domínio sempre retornam um código no final da operação e, ao contrário da maioria dos programas, não retorna zero como bem-sucedido. Para ver o código no final de uma configuração de controlador de domínio, há várias opções:

1. Ao usar o Gerenciador do Servidor, examine os resultados da promoção nos dez segundos anteriores à reinicialização automática.

2. Ao usar o ADDSDeployment do Windows PowerShell, examine os resultados da promoção nos dez segundos anteriores à reinicialização automática. Como alternativa, opte por não reiniciar automaticamente após a conclusão. Você deve adicionar o pipeline **Format-List** para facilitar a leitura da saída. Por exemplo:

   ```
   Install-addsdomaincontroller <options> -norebootoncompletion:$true | format-list
   ```

   Erros na validação e verificação dos pré-requisitos não resultam em uma reinicialização, portanto são visíveis em todos os casos. Por exemplo:

   ![Solução de problemas](media/Troubleshooting-Domain-Controller-Deployment/ADDS_PSPrereqError.png)

3. Em qualquer cenário, examine o dcpromo.log e o dcpromoui.log.

   > [!NOTE]
   > Alguns dos erros listados abaixo não são mais possíveis em razão de alterações na configuração do sistema operacional e do controlador de domínio em sistemas operacionais posteriores. Os novos códigos de ADDSDeployment do Windows PowerShell também previnem alguns erros, mas o dcpromo.exe /unattend não. Esta é outra razão para mudar toda a automação atual do DCPromo preterido para o ADDSDeployment do Windows PowerShell.

### <a name="promotion-and-demotion-success-codes"></a>Códigos de sucesso de promoção e rebaixamento

| Código do Erro | Explicação | Observação |
|--|--|--|
| 1 | Saída, sucesso | Você ainda deve reinicializar, isso apenas indica que o sinalizador de reinicialização automática foi removido |
| 2 | Saída, sucesso, necessário reinicializar |  |
| 3 | Saída, sucesso, com uma falha não crítica | Geralmente observado ao retornar o aviso de Delegação de DNS. Se a delegação de DNS não estiver sendo configurada, use:<p>-creatednsdelegation:$false |
| 4 | Saída, sucesso, com uma falha não crítica, necessário reinicializar | Geralmente observado ao retornar o aviso de Delegação de DNS. Se a delegação de DNS não estiver sendo configurada, use:<p>-creatednsdelegation:$false |

### <a name="promotion-and-demotion-failure-codes"></a>Códigos de falha de promoção e rebaixamento

Promoção e rebaixamento retornam os seguintes códigos de mensagem de falha. É também provável que seja uma mensagem de erro estendida; sempre leia todo o erro com atenção, e não apenas a parte numérica.

| Código do Erro | Explicação | Resoluções sugeridas |
|--|--|--|
| 11 | Promoção do controlador de domínio já em execução | Não execute mais de uma instância de promoção do controlador de domínio ao mesmo tempo para o mesmo computador de destino |
| 12 | O usuário deve ser um administrador | Faça logon como um membro do grupo Administradores interno e certifique-se de estar sendo elevado com UAC |
| 13 | Autoridade de Certificação instalada | Não é possível rebaixar o controlador de domínio, porque ele é também uma Autoridade de Certificação. Não remova a AC antes do inventário cuidadoso do seu uso - se for emissão de certificados, a remoção da função causará uma interrupção. Não é recomendável executar CAs em controladores de domínio |
| 14 | Executando em modo de Inicialização segura | Inicialize o servidor em modo normal |
| 15 | A alteração de função está em andamento ou é necessário reinicializar | É necessário reiniciar o servidor (devido a alterações de configuração anteriores) antes da promoção |
| 16 | Executando na plataforma errada | *Não é provável obter esse erro* |
| 17 | Não existem unidades NTFS 5 | Este erro não é possível no Windows Server 2012, que exige que pelo menos o %systemdrive% seja formatado com NTFS |
| 18 | Espaço insuficiente em windir | Libere espaço no volume %systemdrive% usando cleanmgr.exe |
| 19 | Alteração de nome pendente, é necessário reinicializar | Reinicialize o servidor |
| 20 | O nome do computador apresenta sintaxe inválida | Renomeie o computador com um nome válido |
| 21 | Este controlador de domínio detém funções FSMO, é um GC e/ou é um servidor DNS | Add **-demoteoperationmasterrole** ao usar **-forceremoval**. |
| 22 | O protocolo TCP/IP precisa ser instalado ou não está funcionando | Verifique se computador tem o TCP/IP configurado, associado e funcionando corretamente |
| 23 | O cliente DNS precisa ser configurado primeiro | Defina um servidor DNS primário ao adicionar um novo controlador de domínio a um domínio |
| 24 | As credenciais fornecidas são inválidas ou faltam elementos obrigatórios | Verifique se o nome de usuário e a senha estão corretos |
| 25 | Não foi possível localizar o controlador de domínio para o domínio especificado | Valide as configurações e regras de firewall do cliente DNS |
| 26 | Não foi possível ler a lista de domínios a partir da floresta | Valide as configurações, a funcionalidade LDAP e as regras de firewall do cliente DNS |
| 27 | Nome de domínio ausente | Especifique um domínio ao promover ou rebaixar |
| 28 | Nome de domínio inválido | Escolha um nome de domínio DNS diferente, válido, ao promover |
| 29 | O domínio pai não existe | Verifique o domínio pai especificado ao criar um novo domínio filho ou domínio de árvore |
| 30 | O domínio não está na floresta | Verifique o nome de domínio fornecido |
| 31 | O domínio filho já existe | Especifique um nome de domínio diferente |
| 32 | Nome de domínio NetBIOS inválido | Especifique um nome de domínio NetBIOS válido |
| 33 | O caminho para os arquivos IFM é inválido | Valide o caminho para a pasta Instalar da Mídia |
| 34 | O banco de dados IFM é inválido | Use a pasta Instalar da Mídia correta para este sistema operacional e função (mesma versão de sistema operacional, mesmo tipo de controlador de domínio - RODC versus RWDC) |
| 35 | SYSKEY ausente | Instalação da Mídia é criptografada e você deve fornecer uma SYSKEY válida para usá-la |
| 37 | O caminho para Banco de Dados NTDS ou seus logs é inválido | Altere o caminho do banco de dados e dos logs para um volume fixo de NTFS, não para uma unidade mapeada ou um caminho UNC |
| 38 | O volume não tem espaço suficiente para banco de dados ou logs de NTDS | Libere espaço usando cleanmgr.exe, adicione mais espaço em disco e limpe manualmente o espaço movendo dados desnecessários para outro lugar |
| 39 | O caminho para o SYSVOL é inválido | Altere o caminho da pasta SYSVOL para um volume fixo de NTFS, não para uma unidade mapeada ou um caminho UNC |
| 40 | Nome do site inválido | Forneça um nome de site que existe |
| 41 | Necessário especificar uma senha para o modo de segurança | Forneça uma senha para a conta DSRM. Ela não pode ficar em branco, independentemente da configuração da política de senha |
| 42 | A senha do modo de segurança não satisfaz os critérios (promoção apenas) | Forneça uma senha para a conta DSRM que atenda às regras configuradas da política de senha |
| 43 | A senha de administrador não satisfaz os critérios (rebaixamento apenas) | Forneça uma senha para a conta do administrador local que atenda às regras configuradas da política de senha |
| 44 | O nome especificado para a floresta é inválido | Especifique um nome de domínio DNS de raiz da floresta válido |
| 45 | Já existe uma floresta com o nome especificado | Especifique um nome de domínio DNS de raiz da floresta diferente |
| 46 | O nome especificado para a árvore é inválido | Especifique um nome de domínio DNS de árvore válido |
| 47 | Já existe uma árvore com o nome especificado | Escolha um nome de domínio DNS de árvore diferente |
| 48 | O nome da árvore não se encaixa na estrutura da floresta | Escolha um nome de domínio DNS de árvore diferente |
| 49 | O domínio especificado não existe | Verifique o nome de domínio digitado |
| 50 | Durante o rebaixamento, o último controlador de domínio foi detectado, embora não esteja, ou o último controlador de domínio foi especificado, mas não está. | Não especifique **Último Controlador de Domínio no Domínio** (**-lastdomaincontrollerindomain**), a menos que seja verdadeiro. Use **-ignorelastdcindomainmismatch** para substituir se esse for realmente o último controlador de domínio e houver metadados do controlador de domínio fantasma |
| 51 | Existem partições do aplicativo neste controlador de domínio | Especifique para **Remover Partições do Aplicativo** (**-removeapplicationpartitions**) |
| 52 | O argumento de linha de comando necessário está ausente (isto é, um arquivo de resposta deve ser especificado na linha de comando) | *Visto apenas com dcpromo/unattend, que é preterido. Consulte a documentação mais antiga* |
| 53 | Falha ao promover/rebaixar. O computador deve ser reinicializado para limpeza | Examine logs e erros estendidos |
| 54 | Falha ao promover/rebaixar | Examine logs e erros estendidos |
| 55 | Promoção/rebaixamento cancelado pelo usuário | Examine logs e erros estendidos |
| 56 | Promoção/rebaixamento cancelado pelo usuário. O computador deve ser reinicializado para limpeza | Examine logs e erros estendidos |
| 58 | Um nome de site deve ser especificado durante a promoção de RODC | É necessário especificar um local para um RODC. Ele não detectará automaticamente um como um RWDC |
| 59 | Durante o rebaixamento, este controlador de domínio é o último servidor DNS para uma de suas zonas | Especifique que este é o **Último Servidor DNS no Domínio** ou use **-ignorelastdnsserverfordomain** |
| 60 | Um controlador de domínio que executa o Windows Server 2008 ou posterior deve estar presente no domínio para promover o RODC | Promova pelo menos um controlador de domínio gravável do modelo Windows Server 2008 ou posterior |
| 61 | Não é possível instalar os Serviços de Domínio Active Directory com DNS em um domínio existente que já não hospede DNS | *Não é possível obter este erro* |
| 62 | O arquivo de resposta não tem uma seção [DCInstall] | *Visto apenas com dcpromo/unattend, que é preterido. Consulte a documentação mais antiga.* |
| 63 | O nível funcional da floresta está abaixo do Windows Server 2003 | Eleve o nível funcional da floresta para, pelo menos, Windows Server 2003 Native. Windows 2000 e Windows NT 4.0 não são mais sistemas operacionais com suporte |
| 64 | Falha ao promover, em razão de falha na detecção binária do componente | Instale a função AD DS |
| 65 | Falha ao promover, em razão de falha na instalação binária do componente | Instale a função AD DS |
| 66 | Falha ao promover, em razão de falha na detecção do sistema operacional | Examine logs e erros estendidos. O servidor falha ao retornar sua versão do sistema operacional. É provável que o computador tenha de ser reinstalado, já que sua integridade geral é altamente suspeita |
| 68 | Parceiro de replicação inválido | Use repadmin.exe ou o Windows PowerShell **Get \\ -ADReplication** \* para validar a integridade do controlador de domínio do parceiro |
| 69 | A porta necessária já está em uso por outro aplicativo | Use **netstat.exe -anob** para localizar os processos incorretamente atribuídos a portas AD DS reservadas |
| 70 | O controlador de domínio raiz da floresta deve ser um GC | *Visto apenas com dcpromo/unattend, que é preterido. Consulte a documentação mais antiga* |
| 71 | Servidor DNS já instalado | Não especifique para instalar o DNS (**-installDNS**) se o serviço DNS já está instalado |
| 72 | O computador está executando Serviços de Área de Trabalho Remota em modo não administrativo | Não é possível promover este controlador de domínio, já que ele é também um servidor RDS configurado para mais de dois usuários administrativos. Não remova o RDS antes do inventário cuidadoso do seu uso - se ele estiver sendo usado por aplicativos ou usuários finais, a remoção causará uma interrupção. |
| 73 | O nível funcional da floresta especificado é inválido. | Especifique um nível funcional de floresta válido |
| 74 | O nível funcional do domínio especificado é inválido. | Especifique um nível funcional de domínio válido |
| 75 | Não foi possível determinar a política de replicação de senha padrão. | Confirme se a política de replicação de senha RODC existe e está acessível |
| 76 | Os grupos de segurança replicados/não replicados especificados são inválidos | Confirme se você digitou contas de domínio e usuário válidas ao especificar uma política de replicação de senha |
| 77 | O argumento especificado é inválido | Examine logs e erros estendidos |
| 78 | Falha ao examinar a Floresta do Active Directory | Examine logs e erros estendidos |
| 79 | O RODC não pode ser promovido porque rodcprep não foi executado | Use o Windows Server 2012 para preparar a floresta ou use **adprep.exe /rodcprep** |
| 80 | Domainprep não foi executado | Use o Windows Server 2012 para preparar o domínio ou use **adprep.exe /domainprep** |
| 81 | Forestprep não foi executado | Use o Windows Server 2012 ara preparar a floresta ou use **adprep.exe /forestprep** |
| 82 | Incompatibilidade do esquema de floresta | Use o Windows Server 2012 ara preparar a floresta ou use **adprep.exe /forestprep** |
| 83 | SKU incompatível | *Não é provável obter esse erro* |
| 84 | Não foi possível detectar uma conta do controlador de domínio | Confirme se os controladores de domínio existentes têm o atributo de controle de conta de usuário correto definido. |
| 85 | Não é possível selecionar uma conta do controlador de domínio para a fase 2 | Retornado se você especificar "Usar Conta Existente", mas não for encontrada uma conta ou ocorrer um erro durante a pesquisa de conta. Verifique se você forneceu a conta pré-configurada do RODC correta |
| 86 | Necessário executar promoção da fase 2 | Retornado se você promover um controlador de domínio adicional, mas já existir uma conta e "Permitir Reinstalação" não tiver sido especificado |
| 87 | Existe uma conta de controlador de domínio de tipo conflitante | Renomeie o computador antes de promover, se não estiver tentando anexar a um controlador de domínio desocupado. Você deve anexar à conta do controlador de domínio desocupada usando **-UseExistingAccount** e o argumento somente leitura ou gravável correto, dependendo do tipo de conta |
| 88 | O administrador do servidor especificado não é válido | Você especificou uma conta inválida para a delegação de administração do RODC. Verifique se a conta especificada é um usuário ou grupo válido |
| 89 | O mestre RID do domínio especificado está offline. | Use **netdom.exe query fsmo** para detectar o mestre RID. Coloque-o online e torne-o acessível para o controlador de domínio que você está promovendo |
| 90 | O mestre de nomeação de domínios está offline. | Use **netdom.exe query fsmo** para detectar o mestre de nomeação de domínios. Coloque-o online e torne-o acessível para o controlador de domínio que você está promovendo |
| 91 | Falha ao detectar se o processo é wow64 | *Não é possível obter mais este erro, o sistema operacional é de 64 bits* |
| 92 | O processo Wow64 não é compatível | *Não é possível obter mais este erro, o sistema operacional é de 64 bits* |
| 93 | O serviço do controlador de domínio não está sendo executado para o rebaixamento não rigoroso. | Inicie o serviço AD DS |
| 94 | A senha do administrador local não atende ao requisito: em branco ou não obrigatório | Forneça uma senha que não esteja em branco e assegure que a política de senha local exija uma senha |
| 95 | Não é possível rebaixar o controlador de domínio mais recente do Windows Server 2008 ou posterior no domínio onde existem RODCs dinâmicos | Primeiro, você deve rebaixar todos os RODCs para poder rebaixar todos os controladores de domínio graváveis do Windows Server 2008 ou posterior |
| 96 | Não é possível desinstalar binários DS | *Visto apenas com dcpromo/unattend, que é preterido. Consulte a documentação mais antiga* |
| 97 | Versão de nível funcional da floresta mais elevada do que a do sistema operacional de domínio filho | Forneça um domínio filho funcional igual ou superior ao nível funcional da floresta |
| 98 | A instalação/desinstalação dos arquivos binários do componente está em andamento. | *Visto apenas com dcpromo/unattend, que é preterido. Consulte a documentação mais antiga* |
| 99 | O nível funcional da floresta está muito baixo (o erro é apenas do Windows Server 2012) | Eleve o nível funcional da floresta para, pelo menos, Windows Server 2003 Native. Windows 2000 e Windows NT 4.0 não são mais sistemas operacionais com suporte |
| 100 | O nível funcional do domínio está muito baixo (o erro é apenas o Windows Server 2012) | Eleve o nível funcional do domínio para, pelo menos, Windows Server 2003 Native. Windows 2000 e Windows NT 4.0 não são mais sistemas operacionais com suporte |

## <a name="known-issues-and-common-support-scenarios"></a>Problemas conhecidos e cenários de suporte comuns

A seguir, problemas comuns observados durante o processo de desenvolvimento do Windows Server 2012. Todos esses problemas são estruturais e têm uma solução alternativa válida ou uma técnica mais adequada para evitá-los em primeiro lugar. Muitos desses comportamentos são idênticos no Windows Server 2008 R2 e em sistemas operacionais mais antigos, mas a regravação de implantação do AD DS traz sensibilidade maior aos problemas.

| Problema | O rebaixamento de um controlador de domínio deixa o DNS executando sem zonas |
|--|--|
| Sintomas | O servidor ainda atenderá solicitações de DNS, mas não terá informações sobre a zona |
| Solução e notas | Ao remover a função AD DS, remova também a função Servidor DNS ou defina o serviço Servidor DNS como desabilitado. Lembre-se de apontar o cliente DNS para um servidor diferente. Se estiver usando o Windows PowerShell, execute o seguinte depois de rebaixar o servidor:<p>Código-desinstalar-WindowsFeature DNS<p>ou<p>Conjunto de código-DNS-de-início do serviço desabilitado<br />parar o DNS do serviço |

| Problema | A promoção de um Windows Server 2012 para um domínio de rótulo único existente não configura updatetopleveldomain=1 ou allowsinglelabeldnsdomain=1 |
|--|--|
| Sintomas | O registro dinâmico do DNS não ocorre |
| Solução e notas | Defina esses valores usando as políticas de grupo do Netlogon e DNS. A Microsoft começou a bloquear a criação de domínio de rótulo único no Windows Server 2008; você pode usar o ADMT ou a Ferramenta de Renomeação de Domínio para alterar para uma estrutura de domínio DNS aprovada. |

| Problema | O rebaixamento do último controlador de domínio falhará em um domínio se houver contas RODC pré-criadas, desocupadas |
|--|--|
| Sintomas | O rebaixamento falha com a seguinte mensagem:<p>**Dcpromo.General.54**<p>Os Serviços de Domínio Active Directory não encontraram outro Controlador de Domínio Active Directory para transferir os dados restantes na partição do diretório CN=Schema,CN=Configuration,DC=corp,DC=contoso,DC=com.<p>"O formato do nome do domínio especificado é inválido." |
| Solução e notas | Remova todas as contas do RODC pré-criadas restantes antes rebaixar um domínio, usando a limpeza de metadados **Dsa.msc** ou **Ntdsutil.exe**. |

| Problema | A preparação automatizada de floresta e domínio não executa GPPREP |
|--|--|
| Sintomas | A funcionalidade de planejamento de domínio cruzado para Política de Grupo, Modo de Planejamento de Conjunto Resultante de Política (RSOP), requer sistema de arquivos atualizado e permissões do Active Directory para a Política de Grupo existente. Sem Gpprep, não é possível usar o Planejamento RSOP nos domínios. |
| Solução e notas | Execute **adprep.exe /gpprep** manualmente para todos os domínios que não foram previamente preparados para o Windows Server 2003, o Windows Server 2008 ou o Windows Server 2008 R2. Os administradores devem executar GPPrep apenas uma vez na história de um domínio, não com cada atualização. Não é executado por adprep automático, porque se você já tiver definido as permissões personalizadas adequadas, causaria a replicação de todo o conteúdo do SYSVOL, em todos os controladores de domínio. |

| Problema | A instalação da mídia falha ao verificar quando apontar para um caminho UNC |
|--|--|
| Sintomas | Erro retornado:<p>Código-não foi possível validar o caminho de mídia. Exceção chamando "GetDatabaseInfo" com argumentos "2". A pasta não é válida. |
| Solução e notas | Você deve armazenar os arquivos IFM em um disco local, não em um caminho UNC remoto. Este bloqueio intencional impede a promoção parcial do servidor devido a uma interrupção na rede. |

| Problema | Aviso de delegação de DNS mostrado duas vezes durante a promoção de controlador de domínio |
|--|--|
| Sintomas | Aviso retornado *duas vezes* ao promover usando o Windows PowerShell ADDSDeployment:<p>Código-"uma delegação para este servidor DNS não pode ser criada porque a zona pai autoritativa não pode ser encontrada ou não executa o servidor DNS do Windows. Se você estiver integrando com uma infraestrutura de DNS existente, deverá criar manualmente uma delegação para esse servidor DNS na zona pai para garantir a resolução de nome confiável de fora do domínio. Caso contrário, nenhuma ação é necessária. " |
| Solução e notas | Ignorar. O ADDSDeployment do Windows PowerShell mostra o aviso pela primeira vez durante a verificação de pré-requisitos, em seguida, novamente durante a configuração do controlador de domínio. Se você não quiser configurar a delegação de DNS, use o argumento:<p>Código--CreateDNSDelegation: $false<p>*Não* ignore as verificações de pré-requisitos para suprimir esta mensagem |

| Problema | A especificação de credenciais UPN ou de não domínio durante a configuração retorna erros falsos |
|--|--|
| Sintomas | O Gerenciador do Servidor retorna o erro:<p>Exceção de código chamando "DNSOption" com argumentos "6"<p>O ADDSDeployment do Windows PowerShell retorna erro:<p>Falha na verificação de código das permissões de usuário. Você deve fornecer o nome do domínio ao qual essa conta de usuário pertence. |
| Solução e notas | Certifique-se de estar fornecendo credenciais de domínio válidas na forma de **domínio\usuário**. |

| Problema | A remoção da função DirectoryServices-DomainController usando Dism.exe leva a um servidor não inicializável. |
|--|--|
| Sintomas | Se estiver usando Dism.exe para remover a função AD DS antes de rebaixar sem problemas um controlador de domínio, o servidor já não será iniciado mais normalmente e mostrará erro:<p>Status do código: 0x000000000<br />Info: ocorreu um erro inesperado. |
| Solução e notas | Inicialize no Modo de Reparo de Serviços de Diretório usando *Shift+F8*. Adicione a função AD DS novamente e depois rebaixe à força o controlador de domínio. Como alternativa, restaure o Estado do Sistema do backup. Não use Dism.exe para remoção da função AD DS; o utilitário não tem conhecimento de controladores de domínio. |

| Problema | A instalação de uma nova floresta falha ao definir forestmode para Win2012 |
|--|--|
| Sintomas | A promoção usando o ADDSDeployment do Windows PowerShell retorna o erro:<p>Código-teste. VerifyDcPromoCore. DCPromo. General. 74<p>Falha na verificação de pré-requisitos para a promoção do controlador de domínio. O nível funcional de domínio especificado é inválido |
| Solução e notas | Não especifique um modo funcional de floresta do Win2012 sem especificar *também* um modo funcional de domínio do Win2012. Aqui está um exemplo que irá funcionar sem erros:<p>Código--ForestMode Win2012-DomainMode Win2012] |

| Problema | Quando você clica em Verificar na área de seleção Instalar da Mídia, nada parece ocorrer |
|--|--|
| Sintomas | Quando você especifica um caminho para uma pasta IFM, clicar no botão **Verificar** nunca retorna uma mensagem ou nada parece ocorrer. |
| Solução e notas | O botão **Verificar** só retornará erros se houver problemas. Caso contrário, tornará o botão **Avançar** selecionável, se você tiver fornecido um caminho de IFM. Você deve clicar em **Verificar** para prosseguir, se selecionou IFM. |

| Problema | O rebaixamento com o Gerenciador do Servidor não fornece comentários até ser concluído. |
|--|--|
| Sintomas | Ao usar o Gerenciador do Servidor para remover a função AD DS e rebaixar um controlador de domínio, não há comentários contínuos até o rebaixamento completo ou a falha. |
| Solução e notas | Essa é uma limitação do Gerenciador do Servidor. Para comentários, use o cmdlet ADDSDeployment do Windows PowerShell:<p>Código-desinstalar-addsdomaincontroller |

| Problema | A opção Verificar em Instalar da Mídia não detecta que a mídia RODC forneceu o controlador de domínio gravável, ou vice-versa. |
|--|--|
| Sintomas | Ao promover um novo controlador de domínio usando IFM e fornecendo uma mídia incorreta ao IFM - tal como a mídia RODC para um controlador de domínio gravável ou a mídia RWDC para um RODC - o botão Verificar não retorna um erro. Mais tarde, a promoção falha com o erro:<p>Código-ocorreu um erro ao tentar configurar esta máquina como um controlador de domínio. <br />A promoção de instalação-da-mídia de um DC somente leitura não pode ser iniciada porque o banco de dados de origem especificado não é permitido. Somente bancos de dados de outros RODCs podem ser usados para a promoção IFM de um RODC. |
| Solução e notas | Verificar só valida a integridade global do IFM. Não fornece o tipo de IFM errado a um servidor. Reinicie o servidor antes de tentar a promoção novamente com a mídia correta. |

| Problema | Falha ao promover um RODC para uma conta de computador pré-criada |
|--|--|
| Sintomas | Ao usar o ADDSDeployment do Windows PowerShell para promover um novo RODC com uma conta de computador pré-configurada, você recebe o erro:<p>O conjunto de parâmetros de código não pode ser resolvido usando os parâmetros nomeados especificados.    <br />InvalidArgument: ParameterBindingException<br />    + FullyQualifiedErrorId: AmbiguousParameterSet, Microsoft. DirectoryServices. Deployment. PowerShell. Commands. Install |
| Solução e notas | Não forneça parâmetros já definidos em uma conta RODC pré-criada. Elas incluem:<p>Código--readonlyreplica<br />-installdns<br />-donotconfigureglobalcatalog<br />-SiteName<br />-installdns |

| Problema | Ao desmarcar/selecionar "Reiniciar cada servidor de destino automaticamente, se necessário" nada ocorre |
|--|--|
| Sintomas | Se você selecionar (ou não selecionar) a opção Gerenciador do Servidor **reiniciar cada servidor de destino automaticamente se necessário** whendemoting um controlador de domínio por meio da remoção de função, o servidor sempre será reiniciado, independentemente de sua escolha. |
| Solução e notas | Isso é intencional. O processo de rebaixamento reinicia o servidor, independentemente dessa configuração. |

| Problema | Dcpromo.log mostra "[erro] a configuração de segurança nos arquivos do servidor falhou com 2" |
|--|--|
| Sintomas | O rebaixamento de um controlador de domínio é concluído sem problemas, mas o exame do log dcpromo mostra erro:<p>Código-[erro] falha na configuração de segurança em arquivos de servidor com 2 |
| Solução e notas | Ignore, o erro é esperado e superficial. |

| Problema | A verificação do pré-requisito adprep falha com o erro "Não é possível executar a verificação de conflitos de esquema do Exchange" |
|--|--|
| Sintomas | Ao tentar promover um controlador de domínio do Windows Server 2012 para uma floresta existente do Windows Server 2003, Windows Server 2008 ou Windows Server 2008 R2, a verificação dos pré-requisitos falha com o erro:<p>Falha na verificação de código dos pré-requisitos para o AD Prep. Não é possível executar a verificação de conflito de esquema do Exchange para o domínio  *<domain name>* (exceção: o servidor RPC está indisponível)<p>O adprep.log mostra o erro:<p>Código-Adprep não pôde recuperar dados do servidor *<domain controller>*<p>por meio de Instrumentação de Gerenciamento do Windows (WMI). |
| Solução e notas | O novo controlador de domínio não pode acessar a WMI através de protocolos DCOM/RPC entre os controladores de domínio existentes. Até o momento, houve três causas para isso:<p>-Uma regra de firewall bloqueia o acesso aos controladores de domínio existentes<p>-A conta de serviço de rede está ausente no privilégio "logon como um serviço" (SeServiceLogonRight) nos controladores de domínio existentes<p>-O NTLM está desabilitado em controladores de domínio, usando as políticas de segurança descritas na [introdução à restrição de autenticação NTLM](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd560653(v=ws.10)) |

| Problema | A criação de uma nova floresta AD DS sempre mostra um aviso de DNS |
|--|--|
| Sintomas | Ao criar uma nova floresta do AD DS e a zona DNS no novo controlador de domínio para ele mesmo, você sempre recebe uma mensagem de aviso:<p>Código-foi detectado um erro na configuração de DNS. <br />Nenhum dos servidores DNS usados por este computador respondeu dentro do intervalo de tempo limite.<br />(código de erro 0x000005B4 "ERROR_TIMEOUT") |
| Solução e notas | Ignorar. Este aviso é intencional no primeiro controlador de domínio no domínio raiz de uma nova floresta, caso você tenha a intenção de apontar para um servidor e uma zona DNS existente. |

| Problema | O argumento -whatif do Windows PowerShell retorna informações incorretas do servidor DNS |
|--|--|
| Sintomas | Se você usar o argumento **-whatif** ao configurar um controlador de domínio **-installdns:$true** implícito ou explícito, a saída resultante mostrará:<p>Código-"servidor DNS: não" |
| Solução e notas | Ignorar. DNS instalado e configurado corretamente. |

| Problema | Após a promoção, o logon falha com "Não há memória suficiente disponível para processar este comando" |
|--|--|
| Sintomas | Depois de promover um novo controlador de domínio e, em seguida, fazer logoff e tentar fazer logon interativamente, você recebe um erro:<p>Código-não há armazenamento suficiente disponível para processar este comando |
| Solução e notas | O controlador de domínio não foi reinicializado após a promoção, seja devido a um erro ou porque você especificou o argumento ADDSDeployment do Windows PowerShell **-norebootoncompletion**. Reinicie o controlador de domínio. |

| Problema | O botão Avançar não está disponível na página Opções do Controlador de Domínio |
|--|--|
| Sintomas | Embora você tenha definido uma senha, o botão **Avançar** na página **Opções do Controlador de Domínio** do Gerenciador do Servidor não está disponível. Não há sites listados no menu **Nome do site**. |
| Solução e notas | Você tem vários sites do AD DS e, em pelo menos um, as sub-redes estão ausentes; esse futuro controlador de domínio pertence a uma dessas sub-redes. Você deve selecionar manualmente a sub-rede no menu suspenso Nome do site. Você também deve verificar todos os sites de AD usando DSSITE.MSC ou use o seguinte comando do Windows PowerShell para encontrar todas as sub-redes que faltam nos sites:<p>Código-Get-adreplicationsite-Filter \* -Property sub-redes &#124; Where-Object {! $ _. sub-redes-EQ " \* "} &#124; Format-Table Name |

| Problema | A promoção ou rebaixamento falha com a mensagem "o serviço não pode ser iniciado" |
|--|--|
| Sintomas | Se você tentar a promoção, o rebaixamento ou a clonagem de um controlador de domínio, receberá um erro:<p>Código-o serviço não pode ser iniciado porque está desabilitado ou não tem nenhum dispositivo habilitado associado a ele "(0x80070422)<p>O erro pode ser interativo, um evento ou gravado em um log como dcpromoui.log ou dcpromo.log |
| Solução e notas | O serviço Servidor de Função de SD (DsRoleSvc) está desabilitado. Por padrão, esse serviço é instalado durante a instalação da função AD DS e definido para um tipo de inicialização manual. Não desabilite este serviço. Defina-o novamente como Manual e permita que as operações de função de SD iniciem e parem sob demanda. Este comportamento ocorre por design. |

| Problema | O Gerenciador do Servidor ainda avisa que é necessário promover o DC |
|--|--|
| Sintomas | Se você promover um controlador de domínio usando o dcpromo.exe /unattend obsolete ou se atualizar um controlador de domínio do Windows Server 2008 R2 existente em vigor para o Windows Server 2012, o Gerenciador do Servidor ainda mostrará a tarefa de configuração pós-implantação **Promover este servidor a um controlador de domínio**. |
| Solução e notas | Clique no link de aviso pós-implantação e a mensagem desaparecerá de vez. Este comportamento é superficial e esperado. |

| Problema | Instalação de função ausente no script de implantação do Gerenciador do Servidor |
|--|--|
| Sintomas | Se você promover um controlador de domínio usando o Gerenciador do Servidor e salvar o script de implantação do Windows PowerShell, ele não incluirá o cmdlet de instalação da função e os argumentos (install-windowsfeature -name ad-domain-services -includemanagementtools). Sem a função, o DC não pode ser configurado. |
| Solução e notas | Adicione manualmente o cmdlet e argumentos aos scripts. Esse comportamento é esperado por design. |

| Problema | O script de implantação do Gerenciador do Servidor não está nomeado como PS1 |
|--|--|
| Sintomas | Se você promover um controlador de domínio usando o Gerenciador do Servidor e salvar o script de implantação do Windows PowerShell, o arquivo será nomeado com um nome temporário aleatório e não como um arquivo PS1. |
| Solução e notas | Renomeie manualmente o arquivo. Esse comportamento é esperado por design. |

| Problema | Dcpromo /unattend permite níveis funcionais sem suporte |
|--|--|
| Sintomas | Se você promover um controlador de domínio usando dcpromo /unattend com o seguinte arquivo de resposta de amostra:<p>Auto-completar<p>DCInstall<br />NewDomain = floresta<p>ReplicaOrNewDomain = domínio<p>NewDomainDNSName = Corp. contoso. com<p>SafeModeAdminPassword =Safepassword@6<p>DomainNetbiosName = Corp<p>DNSOnNetwork = Sim<p>AutoConfigDNS = Sim<p>RebootOnSuccess = NoAndNoPromptEither<p>RebootOnCompletion = não<p>*DomainLevel = 0*<p>*ForestLevel = 0*<p>A promoção falha com os seguintes erros no dcpromoui.log:<p>Código-Dcpromoui EA 4.5 b8 0089 13:31:50.783 digite CArgumentsSpec:: ValidateArgument DomainLevel<p>Dcpromoui EA 4.5 b8 008A 13:31: o valor de 50.783 para DomainLevel é 0<p>Dcpromoui EA 4.5 b8 008B 13:31: o código de saída do 50.783 é 77<p>Dcpromoui EA 4.5 b8 008C 13:31:50.783 o argumento especificado é inválido.<p>Dcpromoui EA 4.5 b8 008D 13:31:50.783 o log de fechamento<p>Dcpromoui EA 4.5 b8 0032 13:31: o código de saída do 50.830 é 77<p>O nível 0 é o Windows 2000, que não é compatível com o Windows Server 2012. |
| Solução e notas | Não use o dcpromo /unattend obsoleto e entenda que ele permite que você especifique configurações inválidas que falham depois. Esse comportamento é esperado por design. |

| Problema | Promoção "paralisa" ao criar o objeto de configurações NTDS, nunca é concluído |
|--|--|
| Sintomas | Se você promover um DC de réplica ou um RODC, a promoção atingirá "criando objeto de configurações NTDS" e nunca continuará ou será concluída. Os logs param de atualizar também. |
| Solução e notas | Este é um problema conhecido causado pelo fornecimento de credenciais da conta interna de Administrador local com uma senha correspondente à conta interna de Administrador do domínio. Isso causa uma falha no mecanismo da instalação principal que não incorre em erro mas, em vez disso, aguarda indefinidamente (quasi-loop). Isso é esperado-embora indesejável-comportamento.<p>Para corrigir o servidor:<p>1. reinicialize-o.<p>1. no AD, exclua a conta de computador membro do servidor (ela ainda não será uma conta DC)<p>1. nesse servidor, desassociá-lo forçosamente do domínio<p>1. nesse servidor, remova a função de AD DS.<p>1. reinicializar<p>1. Adicione novamente a função de AD DS e tente a promoção novamente, garantindo que você sempre forneça as credenciais formatadas ***domain\admin*** para a promoção de DC e não apenas a conta de administrador local interna |
