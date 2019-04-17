---
title: "Visão geral do técnica de políticas de restrição do software"
description: "Segurança do Windows Server"
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
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="software-restriction-policies-technical-overview"></a>Visão geral do técnica de políticas de restrição do software

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico descreve as diretivas de restrição de software, quando e como usá-lo, as alterações que foram implementadas em versões anteriores e fornece links para recursos adicionais para ajudá-lo a criar e implantar políticas de restrição de software, a partir do Windows Server 2008 e Windows Vista.

## <a name="introduction"></a>Introdução
Políticas de restrição de software fornecem aos administradores um mecanismo baseado em política de grupo para identificar o software e controlar a capacidade de executar no computador local. Essas políticas podem ser usadas para proteger computadores que executam sistemas operacionais do Microsoft Windows (começando com o Windows Server 2003 e Windows XP Professional) contra conflitos conhecidos e proteger os computadores contra ameaças de segurança, como vírus mal-intencionados e programas de cavalo de Troia. Você também pode usar políticas de restrição de software para criar uma configuração altamente restrita para computadores, em que você permitir que apenas especificamente identificados aplicativos sejam executados. Políticas de restrição de software são integradas ao Microsoft Active Directory e política de grupo. Você também pode criar políticas de restrição de software em computadores autônomos.

Políticas de restrição de software são políticas de confiança, que são regulamentos definidos por um administrador para restringir scripts e outro código que não é totalmente confiável seja executado. A extensão de políticas de restrição de Software para Editor de política de Grupo Local fornece uma interface de usuário único por meio do qual as configurações para restringir o uso de aplicativos podem ser gerenciadas no computador local ou em um domínio inteiro.

## <a name="procedures"></a>Procedimentos

-   [Administrar políticas de restrição de Software](administer-software-restriction-policies.md)

    -   [Determinar lista permitir negação e inventário de aplicativos para políticas de restrição de Software](determine-allow-deny-list-and-application-inventory-for-software-restriction-policies.md)

    -   [Trabalhar com regras de políticas de restrição de Software](work-with-software-restriction-policies-rules.md)

    -   [Usar políticas de restrição de Software para ajudar a proteger seu computador contra vírus de Email](use-software-restriction-policies-to-help-protect-your-computer-against-an-email-virus.md)

-   [Solucionar problemas de políticas de restrição de Software](troubleshoot-software-restriction-policies.md)

## <a name="software-restriction-policy-usage-scenarios"></a>Cenários de uso de política de restrição de software
Os usuários de negócios colaborar usando email, mensagens instantâneas e aplicativos ponto a ponto. Como aumentam esses colaborações, especialmente com o uso da Internet na computação comercial, então fazer as ameaças de código mal-intencionado, como worms, vírus e ameaças invasor ou um usuário mal-intencionado.

Os usuários podem receber um código hostil em muitas formas, desde nativo Windows executáveis arquivos (.exe), a macros em documentos (como arquivos. doc), scripts (como arquivos. vbs). Usuários mal-intencionados ou invasores costumam usam métodos de engenharia social para que os usuários para executar o código contendo a vírus e worms. (É um termo para fazer com que pessoas revelem suas senhas ou alguma forma de informações de segurança de engenharia Social.) Se esse código for ativado, ele pode gerar ataques de negação de serviço na rede, enviar dados confidenciais ou particulares com a Internet, colocar a segurança do computador em risco ou danificar o conteúdo da unidade de disco rígido.

Os usuários e as organizações de TI devem ser capazes de determinar qual software é seguro executar e que não é. Com o grande número e formulários que pode executar código hostil, isso se torna uma tarefa difícil.

Para ajudar a proteger seus computadores de rede do código hostil e software desconhecido ou sem suporte, as organizações podem implementar políticas de restrição de software como parte da sua estratégia de segurança geral.

Os administradores podem usar políticas de restrição de software para as seguintes tarefas:

-   Definir o que é o código confiável

-   Criar uma política de grupo flexível para controlar a scripts, arquivos executáveis e controles ActiveX

Políticas de restrição de software são impostas pelo sistema operacional e por aplicativos (como aplicativos de scripts) que estão em conformidade com políticas de restrição de software.

Especificamente, os administradores podem usar políticas de restrição de software para as seguintes finalidades:

-   Especificar qual software (arquivos executáveis) pode ser executado em computadores cliente

-   Impedir que os usuários executem programas específicos em computadores compartilhados

-   Especificar quem pode adicionar editores confiáveis em computadores cliente

-   Defina o escopo das políticas de restrição de software (especificar se políticas afetam todos os usuários ou um subconjunto de usuários em computadores cliente)

-   Impedir que arquivos executáveis em execução no computador local, unidade organizacional (UO), site ou domínio. Isso seria adequado em casos quando você não estiver usando políticas de restrição de software para resolver problemas potenciais com usuários mal-intencionados.

## <a name="BKMK_Diffs_Changes"></a>Diferenças e as alterações na funcionalidade
Não há nenhuma alteração na funcionalidade no SRP para o Windows Server 2012 e Windows 8.

**Versões compatíveis**

Políticas de restrição de software só podem ser configuradas em e aplicadas a computadores que executam pelo menos Windows Server 2003, incluindo o Windows Server 2012 e pelo menos Windows XP, incluindo o Windows 8.

> [!NOTE]
> Algumas edições do início de sistema operacional Windows cliente com o Windows Vista não têm diretivas de restrição de Software. Computadores não administrados em um domínio por política de grupo não podem receber políticas distribuídas.

**Comparando funções de controle de aplicativo em políticas de restrição de Software e do AppLocker**

A tabela a seguir compara os recursos e funções do recurso de políticas de restrição de Software (SRP) e do AppLocker.

|Função de controle do aplicativo|SRP|AppLocker|
|----------------|----|-------|
|Escopo|As políticas SRP podem ser aplicadas a todos os sistemas de operacionais Windows a partir do Windows XP e Windows Server 2003.|As políticas do AppLocker se aplicam apenas ao Windows Server 2008 R2, Windows Server 2012, Windows 7 e Windows 8.|
|Criação de políticas|As políticas SRP são mantidas por meio da política de grupo e apenas o administrador do GPO pode atualizar a política SRP. O administrador no computador local pode modificar as políticas SRP definidas no GPO local.|As políticas do AppLocker são mantidas por meio da política de grupo e apenas o administrador do GPO pode atualizar a política. O administrador no computador local pode modificar as políticas do AppLocker definidas no GPO local.<br /><br />O AppLocker permite a personalização das mensagens de erro para direcionar usuários para uma página da Web para obter ajuda.|
|Manutenção de políticas|As políticas SRP devem ser atualizadas usando o snap-in de política de segurança Local (se as políticas forem criadas localmente) ou o Console de gerenciamento de política de grupo (GPMC).|As políticas do AppLocker podem ser atualizadas usando o política de segurança Local snap-in (se as políticas forem criadas localmente), ou do GPMC ou os cmdlets do Windows PowerShell do AppLocker.|
|Aplicação de políticas|As políticas SRP são distribuídas por meio da política de grupo.|As políticas do AppLocker são distribuídas por meio da política de grupo.|
|Modo de imposição|A SRP funciona no "Negar modo de lista" onde os administradores podem criar regras para arquivos que não desejam permitir nesta empresa enquanto o restante do arquivo podem ser executados por padrão.<br /><br />SRP também pode ser configurada no "modo de lista de permissão", de forma que o por padrão todos os arquivos sejam bloqueados e os administradores precisem criar regras para os arquivos que desejam permitir.|AppLocker por padrão funciona no "modo de permissão lista" onde somente os arquivos têm permissão para ser executado para os quais há é uma regra de permissão correspondente.|
|Tipos de arquivo que podem ser controlados|A SRP pode controlar os seguintes tipos de arquivo:<br /><br />-Executáveis<br />-Dlls<br />-Scripts<br />-Instaladores Windows<br /><br />A SRP não pode controlar cada tipo de arquivo separadamente. Todas as regras SRP estão em uma coleção de regras única.|O AppLocker pode controlar os seguintes tipos de arquivo:<br /><br />-Executáveis<br />-Dlls<br />-Scripts<br />-Instaladores Windows<br />-Empacotado aplicativos e instaladores (Windows Server 2012 e Windows 8)<br /><br />O AppLocker mantém uma coleção de regras separada para cada um dos cinco tipos de arquivo.|
|Tipos de arquivo designados|A SRP dá suporte a uma lista extensa de tipos de arquivos que são considerados executáveis. Os administradores podem adicionar extensões de arquivos que devem ser considerados executáveis.|O AppLocker não dá suporte a isso. O AppLocker atualmente aceita as seguintes extensões de arquivo:<br /><br />-Executáveis (.exe,. com)<br />-Dlls (. dll,. ocx)<br />-Scripts (. vbs,. js,. ps1,. cmd,. bat)<br />-Instaladores Windows (. msi,. mst,. msp)<br />-Instaladores de aplicativos empacotados (. AppX)|
|Tipos de regra|A SRP aceita quatro tipos de regras:<br /><br />-Hash<br />-Path<br />-Assinatura<br />-Zona da Internet|O AppLocker aceita três tipos de regras:<br /><br />-Hash<br />-Path<br />-Fornecedor|
|Editando o valor de hash|A SRP permite que os administradores fornecer valores de hash personalizados.|O AppLocker calcula o valor de hash em si. Internamente, ele usa o hash SHA1 Authenticode para executáveis portáteis (Exe e Dll), Windows Installers e um hash de arquivo simples SHA1 para o restante.|
|Suporte para diferentes níveis de segurança|Com SRP, os administradores podem especificar as permissões com as quais um aplicativo pode ser executado. Assim, um administrador pode configurar uma regra, esse bloco de notas sempre seja executado com permissões restritas e nunca com privilégios administrativos.<br /><br />SRP no Windows Vista e versões anteriores com suporte a vários níveis de segurança. No Windows 7, essa lista era restrita a apenas dois níveis: não permitido e irrestrito (usuário básico se converte em não permitido).|O AppLocker não dá suporte a níveis de segurança.|
|Gerenciar aplicativos empacotados e instaladores de aplicativos empacotados|Não é possível|. AppX é um tipo de arquivo válido que o AppLocker pode gerenciar.|
|Direcionando uma regra para um usuário ou um grupo de usuários|As regras SRP se aplicam a todos os usuários em um computador específico.|As regras do AppLocker podem ser direcionadas para um usuário específico ou um grupo de usuários.|
|Suporte para exceções de regra|A SRP não dá suporte a exceções de regra|As regras do AppLocker podem ter exceções que permitem que os administradores criem regras como "Permitir tudo do Windows, exceto Regedit.exe".|
|Suporte para o modo de auditoria|A SRP não dá suporte a modo de auditoria. A única maneira de testar as políticas SRP é configurar um ambiente de teste e executar algumas experiências.|O AppLocker dá suporte a modo de auditoria que permite que os administradores testem o efeito de sua política no ambiente de produção real sem afetar a experiência do usuário. Quando estiver satisfeito com os resultados, você pode começar a impor a política.|
|Suporte para exportar e importar políticas|A SRP não dá suporte a importação/exportação de política.|O AppLocker dá suporte à importação e exportação de políticas. Isso permite que você crie a política do AppLocker em um computador de exemplo, teste-a e, em seguida, exporte essa política e importá-lo novamente para o GPO desejado.|
|Imposição de regra|Internamente, a imposição de regras SRP acontece no modo de usuário que é menos seguro.|Internamente, as regras do AppLocker para Exes e Dlls são impostas no modo de kernel, que é mais seguro do que as impor no modo de usuário.|

## <a name="system-requirements"></a>Requisitos do sistema
Políticas de restrição de software só podem ser configuradas em e aplicadas a computadores que executam pelo menos Windows Server 2003 e pelo menos Windows XP. Política de grupo é necessária para distribuir objetos de política de grupo que contenham políticas de restrição de software.

## <a name="software-restriction-policies-components-and-architecture"></a>Arquitetura e componentes de políticas de restrição de software
Políticas de restrição de software fornecem um mecanismo para o sistema operacional e os aplicativos em conformidade com políticas de restrição de software para restringir a execução de tempo de execução de programas de software.

Em um nível alto, políticas de restrição de software consistem nos seguintes componentes:

-   API de políticas de restrição de software. As Interfaces de programação de aplicativo (APIs) são usadas para criar e configurar as regras que constituem a política de restrição de software. Também há APIs para consultar, processamento de políticas de restrição de software e impõe políticas de restrição de software.

-   Uma ferramenta de gerenciamento de políticas de restrição de software. Isso consiste no **políticas de restrição de Software** extensão do **Editor de objeto de política de Grupo Local** snap-in, que os administradores usam para criar e editar as políticas de restrição de software.

-   Um conjunto de APIs de sistema operacional e aplicativos que chamam as políticas de restrição de software APIs para fornecer imposição das políticas de restrição de software em tempo de execução.

-   Política de grupo e do Active Directory. Políticas de restrição de software dependem a infraestrutura de política de grupo para propagar as diretivas de restrição do Active Directory para os clientes apropriados e para definir o escopo e filtrar o aplicativo dessas políticas aos computadores de destino apropriados.

-   APIs de WinVerify confiar que são usados para processar arquivos executáveis assinados e Authenticode.

-   Visualizador de eventos. As funções usadas por eventos de log de políticas de restrição de software nos logs do Visualizador de eventos.

-   Defina de políticas resultantes (RSoP), que podem ajudar no diagnóstico da política em vigor que será aplicada a um cliente.

Para obter mais informações sobre a arquitetura SRP, como SRP gerencia regras, processos e interações, consulte [como Software funcionam de políticas de restrição](https://technet.microsoft.com/library/cc786941(v=WS.10).aspx) na biblioteca técnica do Windows Server 2003.

## <a name="BKMK_Best_Practices"></a>Práticas recomendadas

### <a name="do-not-modify-the-default-domain-policy"></a>Não modifique a política de domínio padrão.

-   Se você não editar a política de domínio padrão, você sempre tem a opção de reaplicando a política de domínio padrão se algo der errado com sua política de domínio personalizado.

### <a name="create-a-separate-group-policy-object-for-software-restriction-policies"></a>Crie um objeto de política de grupo separados para políticas de restrição de software.

-   Se você criar um separado política objeto grupo (GPO) para as políticas de restrição de software, você pode desabilitar políticas de restrição de software em caso de emergência sem desabilitar o resto da sua política de domínio.

### <a name="if-you-experience-problems-with-applied-policy-settings-restart-windows-in-safe-mode"></a>Se você tiver problemas com as configurações de política aplicada, reinicie o Windows no modo de segurança.

-   Políticas de restrição de software não se aplicam quando o Windows é iniciado no modo de segurança. Se você acidentalmente bloquear uma estação de trabalho com as políticas de restrição de software, reinicie o computador no modo de segurança, faça logon como um administrador local, modificar a política, execute **gpupdate**, reinicie o computador e faça logon normalmente.

### <a name="use-caution-when-defining-a-default-setting-of-disallowed"></a>Tenha cuidado ao definir uma configuração padrão não permitido.

-   Ao definir uma configuração padrão de **não permitido**, todos os softwares é permitido, exceto aqueles que foram explicitamente permitidos. Qualquer arquivo que você deseja abrir deve ter uma restrição de software políticas de regra que permite que ele abrir.

-   Para impedir que os administradores bloqueiem seu próprio acesso do sistema, quando o nível de segurança padrão é definido como **não permitido**, quatro regras de caminho do registro são criadas automaticamente. Você pode excluir ou modificar essas regras de caminho do registro; No entanto, isso não é recomendado.

### <a name="for-best-security-use-access-control-lists-in-conjunction-with-software-restriction-policies"></a>Para maior segurança, use listas de controle de acesso em conjunto com as políticas de restrição de software.

-   Os usuários podem tentar burlar as políticas de restrição de software renomeando ou movendo arquivos não permitidos ou substituindo os arquivos sem restrições. Como resultado, é recomendável que você use listas de controle de acesso (ACLs) para negar aos usuários o acesso necessário para executar essas tarefas.

### <a name="test-new-policy-settings-thoroughly-in-test-environments-before-applying-the-policy-settings-to-your-domain"></a>Teste novas configurações de política completamente em ambientes de teste antes de aplicar as configurações de política para seu domínio.

-   Novas configurações de política podem atuar de forma diferente do esperado. Testando diminui a chance de se encontrar um problema ao implantar configurações de política em sua rede.

-   Você pode configurar um domínio de teste, separado do domínio da sua organização, no qual testar novas configurações de política. Você também pode testar as configurações de política, criando um GPO de teste e criar um link para uma unidade organizacional de teste. Quando você testar exaustivamente as configurações de política com usuários de teste, você pode vincular o GPO de teste ao seu domínio.

-   Não defina programas ou arquivos para **não permitido** sem testar para ver o que pode ser o efeito. Restrições em determinados arquivos sério podem afetar a operação de seu computador ou rede.

-   Informações inseridas incorretamente ou erros de digitação podem resultar em uma configuração de política não executa conforme o esperado. Testar novas configurações de política antes de aplicá-las pode impedir que um comportamento inesperado.

### <a name="filter-user-policy-settings-based-on-membership-in-security-groups"></a>Filtre as configurações de política de usuário com base na participação em grupos de segurança.

-   Você pode especificar os usuários ou grupos para que você não deve ser uma configuração de política para aplicar limpando a **Aplicar política de grupo** e **leitura** caixas de seleção, que estão localizadas na **segurança** guia da caixa de diálogo Propriedades do GPO.

-   Quando a permissão de leitura é negada, a configuração de política não é descarregada pelo computador. Como resultado, largura de banda menor é consumida baixando configurações de política desnecessárias, que permite que a rede funcione mais rapidamente. Para negar a permissão de leitura, selecione **negar** para o **leitura** caixa de seleção, que está localizada no **segurança** guia da caixa de diálogo Propriedades do GPO.

### <a name="do-not-link-to-a-gpo-in-another-domain-or-site"></a>Não vincule para um GPO em outro domínio ou site.

-   Criando um link para um GPO em outro domínio ou site pode resultar em desempenho fraco.

## <a name="additional-resources"></a>Recursos adicionais

|Tipo de conteúdo|Referências|
|--------|-------|
|**Planejamento**|[Referência técnica de políticas de restrição de software](https://technet.microsoft.com/library/cc728085(v=WS.10).aspx)|
|**Operações**|[Administrar políticas de restrição de Software](administer-software-restriction-policies.md)|
|**Solução de problemas**|[Políticas de restrição de software (2003) de solução de problemas](https://technet.microsoft.com/library/cc737011(v=WS.10).aspx)|
|**Segurança**|[Políticas de ameaças e contramedidas de restrição de Software (2008)](https://technet.microsoft.com/library/dd349795(v=WS.10).aspx)<br /><br />[Políticas de ameaças e contramedidas de restrição de Software (2008 R2)](https://technet.microsoft.com/library/hh125926(v=WS.10).aspx)|
|**Ferramentas e configurações**|[Políticas de restrição de software ferramentas e configurações (2003)](https://technet.microsoft.com/library/cc782454(v=WS.10).aspx)|
|**Recursos da comunidade**|[Bloqueio do aplicativo com políticas de restrição de Software](https://technet.microsoft.com/magazine/2008.06.srp.aspx?pr=blog)|


