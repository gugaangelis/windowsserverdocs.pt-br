---
title: Visão geral técnica das políticas de restrição de software
description: Segurança do Windows Server
ms.topic: article
ms.assetid: dc7013b0-0efd-40fd-bd6d-75128adbd0b8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: f98075cd8e662b3344f426bd8d69181994096a5f
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87953045"
---
# <a name="software-restriction-policies-technical-overview"></a>Visão geral técnica das políticas de restrição de software

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico descreve as diretivas de restrição de software, quando e como usar o recurso, quais alterações foram implementadas nas versões anteriores e fornece links para recursos adicionais para ajudá-lo a criar e implantar diretivas de restrição de software a partir do Windows Server 2008 e do Windows Vista.

## <a name="introduction"></a>Introdução
As diretivas de restrição de software fornecem aos administradores um mecanismo controlado por Política de Grupo para identificar o software e controlar sua capacidade de execução no computador local. Essas políticas podem ser usadas para proteger computadores que executam sistemas operacionais Microsoft Windows (começando com o Windows Server 2003 e o Windows XP Professional) contra conflitos conhecidos e protegem os computadores contra ameaças à segurança, como vírus mal-intencionados e programas de cavalo de Troia. Você também pode usar as políticas de restrição de software para criar uma configuração altamente restrita para computadores em que você permite apenas aplicativos especialmente identificados a serem executados. As políticas de restrição de software são integradas com o Microsoft Active Directory e a Política de Grupo. Você também pode criar políticas de restrição de software em computadores autônomos.

As políticas de restrição de software são políticas de confiança, que são normas definidas por um administrador para restringir scripts e outro código que não seja totalmente confiável de ser executado. A extensão de diretivas de restrição de software para o Editor de Política de Grupo Local fornece uma única interface de usuário por meio da qual as configurações para restringir o uso de aplicativos podem ser gerenciadas no computador local ou em um domínio.

## <a name="procedures"></a>Procedimentos

-   [Administrar políticas de restrição de software](administer-software-restriction-policies.md)

    -   [Determinar lista de permissões de negação e inventário de aplicativos para diretivas de restrição de software](determine-allow-deny-list-and-application-inventory-for-software-restriction-policies.md)

    -   [Trabalhar com regras de políticas de restrição de software](work-with-software-restriction-policies-rules.md)

    -   [Use as políticas de restrição de software para ajudar a proteger seu computador contra um vírus de email](use-software-restriction-policies-to-help-protect-your-computer-against-an-email-virus.md)

-   [Solucionar problemas de políticas de restrição de software](troubleshoot-software-restriction-policies.md)

## <a name="software-restriction-policy-usage-scenarios"></a>Cenários de uso da diretiva de restrição de software
Os usuários corporativos colaboram usando email, mensagens instantâneas e aplicativos ponto a ponto. À medida que essas colaborações aumentam, especialmente com o uso da Internet em computação comercial, as ameaças de código mal-intencionado, como worms, vírus e ameaças de usuários mal-intencionados ou invasores.

Os usuários podem receber código hostil em várias formas, desde arquivos executáveis nativos do Windows (arquivos. exe) até macros em documentos (como arquivos. doc), para scripts (como arquivos. vbs). Usuários mal-intencionados ou invasores geralmente usam métodos de engenharia social para fazer com que os usuários executem código que contenha vírus e worms. (A engenharia social é um termo para enganar as pessoas na revelação de sua senha ou alguma forma de informações de segurança.) Se esse código estiver ativado, ele poderá gerar ataques de negação de serviço na rede, enviar dados confidenciais ou privados para a Internet, colocar a segurança do computador em risco ou danificar o conteúdo da unidade de disco rígido.

As organizações de ti e os usuários devem ser capazes de determinar qual software é seguro para ser executado e o que não é. Com os grandes números e formulários que o código hostil pode tomar, isso torna-se uma tarefa difícil.

Para ajudar a proteger seus computadores de rede de código hostil e software desconhecido ou sem suporte, as organizações podem implementar políticas de restrição de software como parte de sua estratégia geral de segurança.

Os Administradores podem usar as políticas de restrição de software para as seguintes tarefas:

-   Definir o que é código confiável

-   Criar uma Política de Grupo flexível para scripts de regulação, arquivos executáveis e controles ActiveX

As políticas de restrição de software são aplicadas pelo sistema operacional e por aplicativos (como aplicativos de script) que atendem às políticas de restrição de software.

Especificamente, os administradores podem usar as políticas de restrição de software para as seguintes finalidades:

-   Especificar quais softwares (arquivos executáveis) podem ser executados em computadores cliente

-   Evitar que usuários executem programas específicos em computadores compartilhados

-   Especifique quem pode adicionar editores confiáveis a computadores cliente

-   Definir o escopo das diretivas de restrição de software (especifique se as políticas afetam todos os usuários ou um subconjunto de usuários em computadores cliente)

-   Evitar que arquivos executáveis sejam executados no computador local, na unidade organizacional (OU), no site, ou no domínio. Isso seria apropriado em casos em que você não estiver usando políticas de restrição de software para resolver possíveis problemas com usuários mal-intencionados.

## <a name="differences-and-changes-in-functionality"></a><a name="BKMK_Diffs_Changes"></a>Diferenças e alterações na funcionalidade
Não há nenhuma alteração na funcionalidade do SRP para Windows Server 2012 e Windows 8.

**Versões compatíveis**

As diretivas de restrição de software só podem ser configuradas e aplicadas a computadores que executam, no mínimo, o Windows Server 2003, incluindo o Windows Server 2012 e, no mínimo, o Windows XP, incluindo o Windows 8.

> [!NOTE]
> Determinadas edições do sistema operacional cliente do Windows a partir do Windows Vista não têm diretivas de restrições de software. Os computadores não administrados em um domínio por Política de Grupo podem não receber políticas distribuídas.

**Comparando as funções do controle de aplicativo nas Políticas de Restrição de Software e no AppLocker**

A tabela a seguir compara os recursos e as funções do AppLocker e SRP (Políticas de Restrição de Software).

|Função de controle de aplicativo|SRP|AppLocker|
|----------------|----|-------|
|Escopo|As políticas SRP podem ser aplicadas a todos os sistemas operacionais Windows, começando no Windows XP e Windows Server 2003.|As políticas do AppLocker se aplicam somente ao Windows Server 2008 R2, ao Windows Server 2012, ao Windows 7 e ao Windows 8.|
|Criação de política|As políticas de SRP são mantidas por meio de Política de Grupo e somente o administrador do GPO pode atualizar a política de SRP. O administrador no computador local pode modificar as políticas de SRP definidas no GPO local.|As políticas do AppLocker são mantidas por meio de Política de Grupo e somente o administrador do GPO pode atualizar a política. O administrador no computador local pode modificar as políticas do AppLocker definidas no GPO local.<p>O AppLocker permite personalização de mensagens de erro para direcionar usuários a uma página da Web para obter ajuda.|
|Manutenção de política|As políticas de SRP devem ser atualizadas usando o snap-in Diretiva de segurança local (se as políticas forem criadas localmente) ou o Console de Gerenciamento de Política de Grupo (GPMC).|As políticas do AppLocker podem ser atualizadas usando o snap-in política de segurança local (se as políticas forem criadas localmente) ou o GPMC ou os cmdlets do AppLocker do Windows PowerShell.|
|Aplicativo de política|As políticas de SRP são distribuídas por meio de Política de Grupo.|As políticas do AppLocker são distribuídas por meio de Política de Grupo.|
|Modo de imposição|O SRP funciona no "modo de lista de negações", em que os administradores podem criar regras para arquivos que não desejam permitir nesta empresa, enquanto o restante do arquivo pode ser executado por padrão.<p>O SRP também pode ser configurado no "modo de lista de permissões" de forma que o por padrão todos os arquivos sejam bloqueados e os administradores precisem criar regras de permissão para arquivos que desejam permitir.|Por padrão, o AppLocker funciona no "modo de lista de permissões", no qual somente os arquivos podem ser executados para os quais há uma regra de permissão correspondente.|
|Tipos de arquivo que podem ser controlados|O SRP pode controlar os seguintes tipos de arquivo:<p>-Executáveis<br />-DLLs<br />-Scripts<br />-Instaladores do Windows<p>O SRP não pode controlar cada tipo de arquivo separadamente. Todas as regras de SRP estão em uma única coleção de regras.|O AppLocker pode controlar os seguintes tipos de arquivo:<p>-Executáveis<br />-DLLs<br />-Scripts<br />-Instaladores do Windows<br />-Aplicativos e instaladores empacotados (Windows Server 2012 e Windows 8)<p>O AppLocker mantém uma coleção de regras separada para cada um dos cinco tipos de arquivo.|
|Tipos de arquivo designados|O SRP dá suporte a uma lista extensível de tipos de arquivos que são considerados executáveis. Os administradores podem adicionar extensões para arquivos que devem ser considerados executáveis.|O AppLocker não oferece suporte a isso. Atualmente, o AppLocker dá suporte às seguintes extensões de arquivo:<p>-Executáveis (. exe,. com)<br />-DLLs (. ocx,. dll)<br />-Scripts (. vbs,. js,. ps1,. cmd,. bat)<br />-Instaladores do Windows (. msi,. MST,. msp)<br />-Instaladores de aplicativo empacotados (. AppX)|
|Tipos de regras|O SRP dá suporte a quatro tipos de regras:<p>-Hash<br />-Caminho<br />-Assinatura<br />-Zona da Internet|O AppLocker dá suporte a três tipos de regras:<p>-Hash<br />-Caminho<br />-Publicador|
|Editando o valor de hash|O SRP permite que os administradores forneçam valores de hash personalizados.|O AppLocker computa o valor de hash em si. Internamente, ele usa o hash de Authenticode SHA1 para executáveis portáteis (exe e dll) e instaladores do Windows e um hash de arquivo simples SHA1 para o restante.|
|Suporte para diferentes níveis de segurança|Com os administradores do SRP, é possível especificar as permissões com as quais um aplicativo pode ser executado. Portanto, um administrador pode configurar uma regra de modo que o bloco de notas sempre seja executado com permissões restritas e nunca com privilégios administrativos.<p>O SRP no Windows Vista e versões anteriores ofereciam suporte a vários níveis de segurança. No Windows 7, essa lista foi restrita a apenas dois níveis: não permitido e irrestrito (o usuário básico se traduz para não permitido).|O AppLocker não oferece suporte a níveis de segurança.|
|Gerenciar aplicativos empacotados e instaladores de aplicativo empacotados|Não habilitado|. Appx é um tipo de arquivo válido que o AppLocker pode gerenciar.|
|Direcionando uma regra para um usuário ou um grupo de usuários|As regras de SRP se aplicam a todos os usuários em um determinado computador.|As regras do AppLocker podem ser direcionadas a um usuário específico ou a um grupo de usuários.|
|Suporte para exceções de regra|O SRP não oferece suporte a exceções de regras|As regras do AppLocker podem ter exceções que permitem que os administradores criem regras como "permitir tudo do Windows, exceto Regedit.exe".|
|Suporte para o modo de auditoria|O SRP não oferece suporte ao modo de auditoria. A única maneira de testar as políticas de SRP é configurar um ambiente de teste e executar alguns experimentos.|O AppLocker dá suporte ao modo de auditoria, que permite aos administradores testar o efeito de sua política no ambiente de produção real sem afetar a experiência do usuário. Quando estiver satisfeito com os resultados, você poderá começar a impor a política.|
|Suporte para exportar e importar políticas|O SRP não oferece suporte à importação/exportação de política.|O AppLocker dá suporte à importação e à exportação de políticas. Isso permite que você crie a política do AppLocker em um computador de exemplo, teste-a e, em seguida, exporte essa política e importe-a de volta para o GPO desejado.|
|Imposição de regras|Internamente, a imposição de regras de SRP ocorre no modo de usuário, o que é menos seguro.|Internamente, as regras do AppLocker para EXEs e DLLs são impostas no modo kernel, o que é mais seguro do que aplicá-las no modo de usuário.|

## <a name="system-requirements"></a>Requisitos de sistema
As diretivas de restrição de software só podem ser configuradas e aplicadas a computadores que executam pelo menos o Windows Server 2003 e, pelo menos, o Windows XP. Política de Grupo é necessário para distribuir Política de Grupo objetos que contêm políticas de restrição de software.

## <a name="software-restriction-policies-components-and-architecture"></a>Componentes e arquitetura de diretivas de restrição de software
As diretivas de restrição de software fornecem um mecanismo para o sistema operacional e os aplicativos em conformidade com as diretivas de restrição de software para restringir a execução de programas de software em tempo de execução.

Em um alto nível, as políticas de restrição de software consistem nos seguintes componentes:

-   API de diretivas de restrição de software. As interfaces de programação de aplicativo (APIs) são usadas para criar e configurar as regras que constituem a diretiva de restrição de software. Também há APIs de diretivas de restrição de software para consulta, processamento e aplicação de diretivas de restrição de software.

-   Uma ferramenta de gerenciamento de diretivas de restrição de software. Isso consiste na extensão de **diretivas de restrição de software** do snap-in **Editor de objeto de política de grupo local** , que os administradores usam para criar e editar as diretivas de restrição de software.

-   Um conjunto de APIs de sistema operacional e aplicativos que chamam as APIs de diretivas de restrição de software para fornecer a imposição das diretivas de restrição de software em tempo de execução.

-   Active Directory e Política de Grupo. As diretivas de restrição de software dependem da infraestrutura de Política de Grupo para propagar as diretivas de restrição de software do Active Directory para os clientes apropriados, bem como para definir o escopo e a filtragem da aplicação dessas políticas aos computadores de destino apropriados.

-   As APIs de confiança Authenticode e WinVerify que são usadas para processar arquivos executáveis assinados.

-   Visualizador de Eventos. As funções usadas pelas diretivas de restrição de software registram eventos nos logs de Visualizador de Eventos.

-   Conjunto de diretivas resultante (RSoP), que pode ajudar no diagnóstico da política efetiva que será aplicada a um cliente.

Para obter mais informações sobre a arquitetura SRP, como o SRP gerencia regras, processos e interações, consulte [como as diretivas de restrição de software funcionam](/previous-versions/windows/it-pro/windows-server-2003/cc786941(v=ws.10)) na biblioteca técnica do Windows Server 2003.

## <a name="best-practices"></a><a name="BKMK_Best_Practices"></a>Práticas recomendadas

### <a name="do-not-modify-the-default-domain-policy"></a>Não modifique a política de domínio padrão.

-   Se você não editar a política de domínio padrão, terá sempre a opção de reaplicar a política de domínio padrão se algo der errado com a política de domínio personalizada.

### <a name="create-a-separate-group-policy-object-for-software-restriction-policies"></a>Crie um objeto Política de Grupo separado para as diretivas de restrição de software.

-   Se você criar um objeto de Política de Grupo separado (GPO) para diretivas de restrição de software, poderá desabilitar as diretivas de restrição de software em uma emergência sem desabilitar o restante da diretiva de domínio.

### <a name="if-you-experience-problems-with-applied-policy-settings-restart-windows-in-safe-mode"></a>Se você tiver problemas com as configurações de política aplicadas, reinicie o Windows no modo de segurança.

-   As diretivas de restrição de software não se aplicam quando o Windows é iniciado no modo de segurança. Se você bloquear acidentalmente uma estação de trabalho com diretivas de restrição de software, reinicie o computador no modo de segurança, faça logon como administrador local, modifique a política, execute o **gpupdate**, reinicie o computador e faça logon normalmente.

### <a name="use-caution-when-defining-a-default-setting-of-disallowed"></a>Tenha cuidado ao definir uma configuração padrão de não permitido.

-   Quando você define uma configuração padrão de não **permitido**, todo o software não é permitido, exceto pelo software que foi explicitamente permitido. Qualquer arquivo que você queira abrir deve ter uma regra de diretivas de restrição de software que permita que ele seja aberto.

-   Para proteger os administradores contra bloqueios fora do sistema, quando o nível de segurança padrão é definido como não **permitido**, quatro regras de caminho do registro são criadas automaticamente. Você pode excluir ou modificar essas regras de caminho do registro; no entanto, isso não é recomendado.

### <a name="for-best-security-use-access-control-lists-in-conjunction-with-software-restriction-policies"></a>Para obter a melhor segurança, use listas de controle de acesso em conjunto com as políticas de restrição de software.

-   Os usuários podem tentar burlar as diretivas de restrição de software renomeando ou movendo arquivos não permitidos ou substituindo arquivos irrestritos. Como resultado, é recomendável que você use ACLs (listas de controle de acesso) para negar aos usuários o acesso necessário para executar essas tarefas.

### <a name="test-new-policy-settings-thoroughly-in-test-environments-before-applying-the-policy-settings-to-your-domain"></a>Teste as novas configurações de política minuciosamente em ambientes de teste antes de aplicar as configurações de política ao seu domínio.

-   Novas configurações de política podem atuar de forma diferente do esperado originalmente. O teste diminui a chance de encontrar um problema ao implantar configurações de política em toda a rede.

-   Você pode configurar um domínio de teste, separado do domínio da sua organização, no qual testar novas configurações de política. Você também pode testar as configurações de política criando um GPO de teste e vinculando-o a uma unidade organizacional de teste. Quando tiver testado completamente as configurações de política com os usuários de teste, você poderá vincular o GPO de teste ao seu domínio.

-   Não defina programas ou arquivos como não **permitidos** sem teste para ver qual pode ser o efeito. Restrições em determinados arquivos podem afetar seriamente a operação do computador ou da rede.

-   As informações inseridas incorretamente ou erros de digitação podem resultar em uma configuração de política que não é executada conforme o esperado. O teste de novas configurações de política antes de aplicá-las pode impedir um comportamento inesperado.

### <a name="filter-user-policy-settings-based-on-membership-in-security-groups"></a>Filtre as configurações de política de usuário com base na associação em grupos de segurança.

-   Você pode especificar os usuários ou grupos para os quais não deseja que uma configuração de política seja aplicada desmarcando as caixas de seleção **aplicar política de grupo** e **ler** , localizadas na guia **segurança** da caixa de diálogo Propriedades do GPO.

-   Quando a permissão de leitura é negada, a configuração de política não é baixada pelo computador. Como resultado, menos largura de banda é consumida baixando configurações de política desnecessárias, o que permite que a rede funcione mais rapidamente. Para negar a permissão de leitura, marque a caixa de seleção **negar** para a **leitura** , que está localizada na guia **segurança** da caixa de diálogo Propriedades do GPO.

### <a name="do-not-link-to-a-gpo-in-another-domain-or-site"></a>Não vincule a um GPO em outro domínio ou site.

-   A vinculação a um GPO em outro domínio ou site pode resultar em baixo desempenho.

## <a name="additional-resources"></a>Recursos adicionais

|Tipo de conteúdo|Referências|
|--------|-------|
|**Planejamento**|[Referência técnica das diretivas de restrição de software](/previous-versions/windows/it-pro/windows-server-2003/cc728085(v=ws.10))|
|**Operações**|[Administrar políticas de restrição de software](administer-software-restriction-policies.md)|
|**Solução de problemas**|[Solução de problemas de diretivas de restrição de software (2003)](/previous-versions/windows/it-pro/windows-server-2003/cc737011(v=ws.10))|
|**Segurança**|[Ameaças e contramedidas para políticas de restrição de software (2008)](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd349795(v=ws.10))<p>[Ameaças e contramedidas para políticas de restrição de software (2008 R2)](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/hh125926(v=ws.10))|
|**Ferramentas e configurações**|[Ferramentas e configurações de diretivas de restrição de software (2003)](/previous-versions/windows/it-pro/windows-server-2003/cc782454(v=ws.10))|
|**Recursos da comunidade**|[Bloqueio de Aplicativo com Políticas de Restrição de Software](/previous-versions/technet-magazine/cc510322(v=msdn.10)?pr=blog)|
