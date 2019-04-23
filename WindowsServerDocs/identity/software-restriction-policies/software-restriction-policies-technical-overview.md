---
title: Visão geral técnica das políticas de restrição de software
description: Segurança do Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-software-restriction-policies
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: dc7013b0-0efd-40fd-bd6d-75128adbd0b8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: d007d55ced9c6a18581eaedb4edb66db9eeccab9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830847"
---
# <a name="software-restriction-policies-technical-overview"></a>Visão geral técnica das políticas de restrição de software

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico descreve as políticas de restrição de software, quando e como usar o recurso, as alterações que foram implementados em versões anteriores e fornece links para recursos adicionais para ajudá-lo a criar e implantar políticas de restrição de software a partir do Windows Server 2008 e Windows Vista.

## <a name="introduction"></a>Introdução
Políticas de restrição de software oferecem aos administradores um mecanismo baseado em diretiva de grupo para identificar o software e controlar sua capacidade de executar no computador local. Essas políticas podem ser usadas para proteger os computadores que executam sistemas operacionais de Microsoft Windows (começando com o Windows Server 2003 e Windows XP Professional) contra conflitos conhecidos e proteger os computadores contra ameaças de segurança, como vírus mal-intencionado e cavalos de Troia. Você também pode usar as políticas de restrição de software para criar uma configuração altamente restrita para computadores em que você permite apenas aplicativos especialmente identificados a serem executados. As políticas de restrição de software são integradas com o Microsoft Active Directory e a Política de Grupo. Você também pode criar políticas de restrição de software em computadores autônomos.

As políticas de restrição de software são políticas de confiança, que são normas definidas por um administrador para restringir scripts e outro código que não seja totalmente confiável de ser executado. A extensão de diretivas de restrição de Software para o Editor de diretiva de grupo de Local fornece uma única interface de usuário por meio do qual as configurações para restringir o uso de aplicativos podem ser gerenciadas no computador local ou em um domínio inteiro.

## <a name="procedures"></a>Procedimentos

-   [Administrar políticas de restrição de Software](administer-software-restriction-policies.md)

    -   [Determinar a lista de permissão-restrição e inventário de aplicativos para políticas de restrição de Software](determine-allow-deny-list-and-application-inventory-for-software-restriction-policies.md)

    -   [Trabalhar com regras de políticas de restrição de Software](work-with-software-restriction-policies-rules.md)

    -   [Usar políticas de restrição de Software para ajudar a proteger seu computador contra um vírus de Email](use-software-restriction-policies-to-help-protect-your-computer-against-an-email-virus.md)

-   [Solucionar problemas de políticas de restrição de Software](troubleshoot-software-restriction-policies.md)

## <a name="software-restriction-policy-usage-scenarios"></a>Cenários de uso de política de restrição de software
Os usuários empresariais colaboram usando o email, mensagens instantâneas e aplicativos ponto a ponto. À medida que aumentam essas colaborações, especialmente com o uso da Internet na computação comercial, então, fazer as ameaças contra código mal-intencionado, como worms, vírus e o usuário mal-intencionado ou ameaças do invasor.

Os usuários podem receber um código hostil de várias formas, variando de arquivos executáveis de Windows nativos (.exe arquivos), a macros em documentos (como arquivos. doc), scripts (como arquivos. vbs). Usuários mal-intencionados ou invasores geralmente usam métodos de engenharia social para obter os usuários executem o código que contém a vírus e worms. (É um termo para fazer com que pessoas a revelar sua senha ou alguma forma de informações de segurança de engenharia Social.) Se esse código está ativado, ele pode gerar os ataques de negação de serviço na rede, enviar dados confidenciais ou particulares para a Internet, colocar a segurança do computador em risco ou danificar o conteúdo da unidade de disco rígido.

Os usuários e organizações de TI devem ser capazes de determinar qual software é seguro para execução e que não é. Com a números grandes e formulários que o código hostil pode assumir, isso se torna uma tarefa difícil.

Para ajudar a proteger computadores de sua rede contra código hostil e software desconhecido ou sem suporte, as organizações podem implementar políticas de restrição de software como parte de sua estratégia geral de segurança.

Os Administradores podem usar as políticas de restrição de software para as seguintes tarefas:

-   Definir o que é código confiável

-   Criar uma Política de Grupo flexível para scripts de regulação, arquivos executáveis e controles ActiveX

As políticas de restrição de software são aplicadas pelo sistema operacional e por aplicativos (como aplicativos de script) que atendem às políticas de restrição de software.

Especificamente, os administradores podem usar as políticas de restrição de software para as seguintes finalidades:

-   Especificar quais software (arquivos executáveis) pode ser executados em computadores cliente

-   Evitar que usuários executem programas específicos em computadores compartilhados

-   Especifique quem pode adicionar editores confiáveis aos computadores cliente

-   Definir o escopo das diretivas de restrição de software (especificar se as políticas afetam todos os usuários ou um subconjunto de usuários em computadores cliente)

-   Evitar que arquivos executáveis sejam executados no computador local, na unidade organizacional (OU), no site, ou no domínio. Isso seria apropriado em casos em que você não estiver usando políticas de restrição de software para resolver possíveis problemas com usuários mal-intencionados.

## <a name="BKMK_Diffs_Changes"></a>As diferenças e as alterações na funcionalidade
Não há nenhuma alteração na funcionalidade do SRP para o Windows Server 2012 e Windows 8.

**Versões com suporte**

Políticas de restrição de software só pode ser configuradas no e aplicadas a computadores que executam pelo menos Windows Server 2003, incluindo o Windows Server 2012 e pelo menos Windows XP, incluindo o Windows 8.

> [!NOTE]
> Determinadas edições do início de sistema operacional do cliente Windows com o Windows Vista não tem diretivas de restrição de Software. Não é administrados em um domínio pela diretiva de grupo de computadores podem não receber políticas distribuídas.

**Comparando funções de controle de aplicativo em diretivas de restrição de Software e o AppLocker**

A tabela a seguir compara os recursos e as funções do AppLocker e SRP (Políticas de Restrição de Software).

|Função de controle de aplicativo|SRP|AppLocker|
|----------------|----|-------|
|Escopo|As políticas SRP podem ser aplicadas a todos os sistemas operacionais Windows, começando no Windows XP e Windows Server 2003.|As políticas do AppLocker se aplicam somente ao Windows Server 2008 R2, Windows Server 2012, Windows 7 e Windows 8.|
|Criação de política|As políticas SRP são mantidas por meio da Política de Grupo e apenas o administrador do GPO pode atualizar a política SRP. O administrador no computador local pode modificar as políticas SRP definidas no GPO local.|As políticas do AppLocker são mantidas por meio da Política de Grupo e apenas o administrador do GPO pode atualizar a política. O administrador no computador local pode modificar as políticas do AppLocker definidas no GPO local.<br /><br />O AppLocker permite personalização de mensagens de erro para direcionar usuários a uma página da Web para obter ajuda.|
|Manutenção de política|As políticas SRP devem ser atualizadas por meio do snap-in de política de segurança local (se as políticas forem criadas localmente) ou do GPMC (Console de Gerenciamento de Política de Grupo).|As políticas do AppLocker podem ser atualizadas por meio do snap-in de política de segurança local (se as políticas forem criadas localmente), do GPMC ou dos cmdlets do Windows PowerShell do AppLocker.|
|Aplicação de políticas|As políticas SRP são distribuídas por meio de Política de Grupo.|As políticas do AppLocker são distribuídas por meio de Política de Grupo.|
|Modo de imposição|SRP funciona em "Negar modo de lista" em que os administradores podem criar regras para arquivos que não deseja permitir nesta empresa enquanto o restante do arquivo podem ser executados por padrão.<br /><br />SRP também pode ser configurada na seção "Permitir modo de lista", de modo que o por padrão, todos os arquivos são bloqueados e os administradores precisam para criar permita as regras para arquivos que deseja permitir.|AppLocker por padrão funciona na seção "Permitir modo de lista" em que somente os arquivos podem ser executados para o qual existe é uma correspondência de regra de permissão.|
|Tipos de arquivo que podem ser controlados|A SRP pode controlar os seguintes tipos de arquivo:<br /><br />-Executáveis<br />-Dlls<br />-Scripts<br />-Instaladores do Windows<br /><br />A SRP não pode controlar cada tipo de arquivo separadamente. Todas as regras SRP estão em uma coleção de regras única.|O AppLocker pode controlar os seguintes tipos de arquivo:<br /><br />-Executáveis<br />-Dlls<br />-Scripts<br />-Instaladores do Windows<br />-Aplicativos empacotados e instaladores (Windows Server 2012 e Windows 8)<br /><br />O AppLocker mantém uma coleção de regras separada para cada um dos cinco tipos de arquivo.|
|Tipos de arquivo designados|A SRP dá suporte a uma lista extensa de tipos de arquivos que são considerados executáveis. Os administradores podem adicionar extensões de arquivos que devem ser considerados executáveis.|O AppLocker não dá suporte a isso. O AppLocker atualmente aceita as seguintes extensões de arquivo:<br /><br />-Executáveis (.exe,. com)<br />-Dlls (. ocx,. dll)<br />-   Scripts (.vbs, .js, .ps1, .cmd, .bat)<br />-Instaladores do Windows (. msi,. mst,. msp)<br />-Instaladores de aplicativos empacotados (AppX)|
|Tipos de regra|A SRP aceita quatro tipos de regras:<br /><br />-   Hash<br />-Path<br />-Assinatura<br />-Zona da Internet|O AppLocker aceita três tipos de regras:<br /><br />-   Hash<br />-Path<br />-   Publisher|
|Editando o valor de hash|O SRP permite que os administradores fornecer valores de hash personalizado.|O AppLocker calcula o valor de hash em si. Internamente, ele usa o hash SHA1 Authenticode para arquivos executáveis portáteis (Exe e Dll) e instaladores do Windows e um hash de arquivo simples de SHA1 para o restante.|
|Suporte para diferentes níveis de segurança|Com o SRP, os administradores podem especificar as permissões com a qual um aplicativo pode ser executado. Portanto, um administrador pode configurar uma regra de tal esse bloco de notas sempre é executado com permissões restritas e nunca com privilégios administrativos.<br /><br />A SRP no Windows Vista e em versões anteriores dava suporte a vários níveis de segurança. No Windows 7, essa lista foi restrita a apenas dois níveis: Não permitido e irrestrito (usuário básica resulta em não permitido).|O AppLocker não dá suporte a níveis de segurança.|
|Gerenciar aplicativos empacotados e instaladores de aplicativos empacotados|Não é possível|.appx é um tipo de arquivo válido que o AppLocker pode gerenciar.|
|Direcionando uma regra para um usuário ou um grupo de usuários|As regras SRP se aplicam a todos os usuários em um computador específico.|As regras do AppLocker podem ser direcionadas para um usuário ou um grupo de usuários específico.|
|Suporte às exceções de regra|A SRP não dá suporte às exceções de regra|As regras do AppLocker podem ter exceções que permitem que os administradores criem regras, como "Permitir tudo do Windows, exceto para Regedit.exe".|
|Suporte ao modo de auditoria|A SRP não dá suporte ao modo de auditoria. A única maneira de testar as políticas SRP é configurar um ambiente de teste e executar algumas experiências.|O AppLocker dá suporte ao modo de auditoria que permite que os administradores testem o efeito de sua política no ambiente de produção real sem afetar a experiência do usuário. Quando estiver satisfeito com os resultados, você pode começar a impor a política.|
|Suporte para exportar e importar políticas|A SRP não dá suporte à importação/exportação de política.|O AppLocker dá suporte à importação e exportação de políticas. Isso permite que você crie a política do AppLocker em um computador de exemplo, teste-a e, em seguida, exporte essa política e importe-a novamente para o GPO desejado.|
|Imposição de política|Internamente, a imposição de regras SRP acontece no modo de usuário, que é menos seguro.|Internamente, as regras do AppLocker para Dlls e Exes são impostas no modo de kernel que é mais seguro que impô-las no modo de usuário.|

## <a name="system-requirements"></a>Requisitos de sistema
Políticas de restrição de software só podem ser configuradas no e aplicadas a computadores que executam pelo menos Windows Server 2003 e pelo menos Windows XP. Diretiva de grupo é necessária para distribuir objetos de diretiva de grupo que contêm políticas de restrição de software.

## <a name="software-restriction-policies-components-and-architecture"></a>Arquitetura e componentes de políticas de restrição de software
Políticas de restrição de software fornecem um mecanismo para o sistema operacional e aplicativos em conformidade com políticas de restrição de software para restringir a execução de tempo de execução de programas de software.

Em um alto nível, diretivas de restrição de software consistem nos seguintes componentes:

-   API de diretivas de restrição de software. As Interfaces de programação de aplicativo (APIs) são usadas para criar e configurar as regras que constituem a diretiva de restrição de software. Também há políticas de restrição de software APIs para consultar, processamento e impor políticas de restrição de software.

-   Uma ferramenta de gerenciamento de políticas de restrição de software. Isso consiste na **diretivas de restrição de Software** extensão da **Editor de objeto de diretiva de Grupo Local** snap-in, que os administradores usam para criar e editar as políticas de restrição de software.

-   Um conjunto de APIs do sistema operacional e aplicativos que chamam as políticas de restrição de software APIs para fornecer a imposição de diretivas de restrição de software em tempo de execução.

-   Active Directory e diretiva de grupo. Diretivas de restrição de software dependem da infra-estrutura de diretiva de grupo para propagar as diretivas de restrição do Active Directory para os clientes apropriados e para definir o escopo e o aplicativo dessas políticas apropriadas de filtragem computadores de destino.

-   Authenticode e APIs WinVerify confiar que são usados para processar arquivos executáveis assinados.

-   Visualizador de eventos. As funções usadas por eventos de log de políticas de restrição de software para os logs do Visualizador de eventos.

-   Resultante conjunto de diretivas (RSoP), que pode ajudar a diagnosticar da política em vigor que será aplicada a um cliente.

Para obter mais informações sobre a arquitetura do SRP, como o SRP gerencia regras, processos e interações, consulte [como Software trabalham de políticas de restrição](https://technet.microsoft.com/library/cc786941(v=WS.10).aspx) na biblioteca técnica do Windows Server 2003.

## <a name="BKMK_Best_Practices"></a>Práticas recomendadas

### <a name="do-not-modify-the-default-domain-policy"></a>Não modifique a política de domínio padrão.

-   Se você não editar a política de domínio padrão, você sempre tem a opção de reaplicar a política de domínio padrão, se algo der errado com a diretiva de domínio personalizado.

### <a name="create-a-separate-group-policy-object-for-software-restriction-policies"></a>Crie um objeto de diretiva de grupo separado para políticas de restrição de software.

-   Se você criar um separado grupo de política de GPO (objeto) para as políticas de restrição de software, você pode desabilitar as políticas de restrição de software em caso de emergência sem desabilitar o restante da sua política de domínio.

### <a name="if-you-experience-problems-with-applied-policy-settings-restart-windows-in-safe-mode"></a>Se você tiver problemas com configurações de diretiva aplicadas, reinicie o Windows no modo de segurança.

-   Políticas de restrição de software não se aplicam ao Windows é iniciado no modo de segurança. Se você acidentalmente bloquear uma estação de trabalho com as políticas de restrição de software, reinicie o computador no modo de segurança, faça logon como um administrador local, modificar a política, execute **gpupdate**, reinicie o computador e, em seguida, fazer logon normalmente.

### <a name="use-caution-when-defining-a-default-setting-of-disallowed"></a>Tenha cuidado ao definir uma configuração padrão não permitido.

-   Quando você define uma configuração padrão de **não permitido**, todo o software não é permitido, exceto para o software que foram explicitamente permitido. Qualquer arquivo que você deseja abrir deve ter uma restrição de software políticas de regra que permite que ele seja aberto.

-   Para proteger os administradores bloqueiem a mesmos fora do sistema, quando o nível de segurança padrão é definido como **não permitido**, quatro regras de caminho do registro são criadas automaticamente. Você pode excluir ou modificar essas regras de caminho do registro; No entanto, isso não é recomendado.

### <a name="for-best-security-use-access-control-lists-in-conjunction-with-software-restriction-policies"></a>Para obter melhor segurança, use as listas de controle de acesso em conjunto com as políticas de restrição de software.

-   Os usuários podem tentar contornar diretivas de restrição de software renomeando ou movendo arquivos não permitidos ou substituindo os arquivos sem restrições. Como resultado, é recomendável que você usa listas de controle de acesso (ACLs) para negar aos usuários o acesso necessário para executar essas tarefas.

### <a name="test-new-policy-settings-thoroughly-in-test-environments-before-applying-the-policy-settings-to-your-domain"></a>Teste novas configurações de política minuciosamente em ambientes de teste antes de aplicar as configurações de política ao seu domínio.

-   Novas configurações de política podem agir de forma diferente do esperado. Teste diminui a chance de encontrar um problema ao implantar as configurações de política em toda a rede.

-   Você pode configurar um domínio de teste, separado do domínio da sua organização, para testar as novas configurações de política. Você também pode testar as configurações de política criando um GPO de teste e vinculá-la a uma unidade organizacional de teste. Ao testar completamente as configurações de política com usuários de teste, você pode vincular o GPO de teste ao seu domínio.

-   Não defina programas ou arquivos a serem **não permitido** sem testar para ver o que pode ser o efeito. Restrições em determinados arquivos podem afetar seriamente a operação do computador ou na rede.

-   Informações inseridas incorretamente ou erros de digitação podem resultar em uma configuração de política que não sejam executados conforme o esperado. Teste as novas configurações de política antes de aplicá-los pode impedir que um comportamento inesperado.

### <a name="filter-user-policy-settings-based-on-membership-in-security-groups"></a>Filtre as configurações de política de usuário com base na associação em grupos de segurança.

-   Você pode especificar usuários ou grupos para os quais não deseja que uma configuração de política para aplicar ao desmarcar a **aplicar diretiva de grupo** e **leitura** caixas de seleção, que estão localizadas no **segurança**guia da caixa de diálogo de propriedades para o GPO.

-   Quando a permissão de leitura for negada, a configuração de política não é descarregada pelo computador. Como resultado, menos largura de banda é consumida, baixando as configurações de diretiva desnecessárias, que permite que a rede funcione mais rapidamente. Para negar a permissão de leitura, selecione **Deny** para o **leitura** caixa de seleção, que está localizada no **segurança** guia da caixa de diálogo de propriedades para o GPO.

### <a name="do-not-link-to-a-gpo-in-another-domain-or-site"></a>Não vincule um GPO em outro domínio ou site.

-   Vinculando a um GPO em outro domínio ou site pode resultar em baixo desempenho.

## <a name="additional-resources"></a>Recursos adicionais

|Tipo de conteúdo|Referências|
|--------|-------|
|**Planejamento**|[Referência técnica de políticas de restrição de software](https://technet.microsoft.com/library/cc728085(v=WS.10).aspx)|
|**Operações**|[Administrar políticas de restrição de Software](administer-software-restriction-policies.md)|
|**Solução de problemas**|[Políticas de restrição de software, solução de problemas (2003)](https://technet.microsoft.com/library/cc737011(v=WS.10).aspx)|
|**Segurança**|[Ameaças e contramedidas para restrição de Software políticas (2008)](https://technet.microsoft.com/library/dd349795(v=WS.10).aspx)<br /><br />[Ameaças e contramedidas para restrição de Software políticas (2008 R2)](https://technet.microsoft.com/library/hh125926(v=WS.10).aspx)|
|**Ferramentas e configurações**|[Políticas de restrição de software ferramentas e configurações (2003)](https://technet.microsoft.com/library/cc782454(v=WS.10).aspx)|
|**Recursos da comunidade**|[Bloqueio de aplicativo com políticas de restrição de Software](https://technet.microsoft.com/magazine/2008.06.srp.aspx?pr=blog)|


