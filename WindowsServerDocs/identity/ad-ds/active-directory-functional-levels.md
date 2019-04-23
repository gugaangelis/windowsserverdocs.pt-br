---
ms.assetid: f964d056-11bf-4d9b-b5ab-dceaad8bfbc3
title: Níveis funcionais do Windows Server 2016
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 10/29/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.custom: it-pro
ms.reviewer: maheshu
ms.technology: identity-adds
ms.openlocfilehash: ea56c718394d145a36145d32e5769661a62efd56
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840997"
---
# <a name="forest-and-domain-functional-levels"></a>Níveis funcionais de floresta e domínio

>Aplica-se a: Windows Server

Níveis funcionais de determinam os recursos de domínio ou floresta de serviços de domínio Active Directory (AD DS) disponíveis. Eles também determinam quais sistemas operacionais do Windows Server podem ser executados em controladores de domínio no domínio ou floresta. No entanto, os níveis funcionais não afetam quais sistemas operacionais que você pode executar em estações de trabalho e servidores de membro que ingressaram no domínio ou floresta.

Quando você implanta o AD DS, defina os níveis funcionais de domínio e floresta para o valor mais alto que pode dar suporte a seu ambiente. Dessa forma, você pode usar tantos recursos do AD DS quanto possível. Quando você implanta uma nova floresta, você precisará definir o nível funcional de floresta e, em seguida, defina o nível funcional do domínio. Você pode definir o nível funcional de domínio para um valor que é maior do que o nível funcional da floresta, mas não é possível definir o nível funcional de domínio para um valor que é menor do que o nível funcional da floresta.

Com o fim da vida útil do Windows 2003, controladores de domínio (DCs) do Windows 2003 precisam ser atualizados para o Windows Server 2008, 2008R2, 2012, 2012 R2, 2016 ou 2019. Como resultado, qualquer controlador de domínio que executa o Windows Server 2003 deve ser removido do domínio.

No Windows Server 2008 e superiores níveis funcionais de domínio, replicação do serviço de arquivos distribuído (DFS) é usada para replicar o conteúdo da pasta SYSVOL entre controladores de domínio. Se você criar um novo domínio no nível funcional de domínio do Windows Server 2008 ou superior, a replicação DFS é automaticamente usada para replicar o SYSVOL. Se você criou o domínio em um nível funcional mais baixo, você precisará migrar do uso de FRS para replicação do DFS do SYSVOL. Para obter as etapas de migração, você pode a seguir a [procedimentos no TechNet](https://technet.microsoft.com/library/dd640019(v=WS.10).aspx) ou você pode consultar a [simplificada do conjunto de etapas no blog do gabinete do arquivo de equipe de armazenamento](http://blogs.technet.com/b/filecab/archive/2014/06/25/streamlined-migration-of-frs-to-dfsr-sysvol.aspx).

## <a name="windows-server-2019"></a>Windows Server 2019

Não há nenhuma nova floresta ou níveis funcionais de domínio adicionados nesta versão.

O requisito mínimo para adicionar um controlador de domínio do Windows Server 2019 é um nível funcional do Windows Server 2008 R2.

## <a name="windows-server-2016"></a>Windows Server 2016

Sistema de operacional do controlador de domínio com suporte:

* Windows Server 2019
* Windows Server 2016

### <a name="windows-server-2016-forest-functional-level-features"></a>Recursos de nível funcional de floresta Windows Server 2016

* Todos os recursos que estão disponíveis no Windows Server 2012 R2 nível funcional da floresta e os recursos a seguir, estão disponíveis:
   * [Gerenciamento de acesso privilegiado (PAM) usando o Microsoft Identity Manager (MIM)](https://docs.microsoft.com/windows-server/identity/whats-new-active-directory-domain-services#a-namebkmkpamaprivileged-access-management)

### <a name="windows-server-2016-domain-functional-level-features"></a>Recursos de nível funcional de domínio Windows Server 2016

* Todos os recursos do Active Directory padrão, todos os recursos do nível funcional do Windows Server 2012 R2 domínio, além dos seguintes recursos:
   * Controladores de domínio podem dar suporte a distribuição automática do NTLM e outros segredos baseado em senha em uma conta de usuário configurado para exigir autenticação PKI. Essa configuração também é conhecido como "Cartão inteligente necessário para logon interativo"
   * Controladores de domínio podem dar suporte a rede, permitindo que NTLM quando um usuário é restrito a específicos de dispositivos ingressados no domínio.
   * Os clientes do Kerberos autenticar com êxito com a extensão de atualização de PKInit obterá a identidade de chave pública nova SID.

    Para obter mais informações, consulte [o que há de novo na autenticação Kerberos](https://docs.microsoft.com/windows-server/security/kerberos/whats-new-in-kerberos-authentication) e [Novidades na proteção de credenciais](https://docs.microsoft.com/windows-server/security/credentials-protection-and-management/whats-new-in-credential-protection)

## <a name="windows-server-2012r2"></a>Windows Server 2012R2

Sistema de operacional do controlador de domínio com suporte:

* Windows Server 2019
* Windows Server 2016
* Windows Server 2012 R2

### <a name="windows-server-2012r2-forest-functional-level-features"></a>Windows Server 2012 R2 floresta recursos de nível funcional

* Todos os recursos que estão disponíveis no Windows Server 2012 recursos de nível, mas nenhum adicional funcionais da floresta.

### <a name="windows-server-2012r2-domain-functional-level-features"></a>Windows Server 2012 R2 domínio recursos de nível funcional

* Todos os recursos do Active Directory padrão, todos os recursos do nível funcional do domínio Windows Server 2012, além dos seguintes recursos:
   * Proteções do lado do controlador de domínio para usuários protegidos. Protegido por autenticação dos usuários para um Windows Server 2012 R2 pode deixar de domínio:
      * Autenticar com a autenticação NTLM
      * Usar conjuntos de criptografia DES ou RC4 na pré-autenticação do Kerberos
      * Ser delegadas com delegação restrita ou irrestrita
      * Renovar tíquetes de usuário (TGTs) além do tempo de vida inicial de 4 horas
   * Políticas de autenticação
      * Nova floresta do Active Directory políticas que podem ser aplicadas a contas em domínios do Windows Server 2012 R2 para controlar quais hosts uma conta pode logon do e aplicar condições de controle de acesso para autenticação para serviços em execução como uma conta.
   * Silos de política de autenticação
      * Nova floresta com base no objeto de Active Directory, que pode criar uma relação entre contas de usuário, serviço gerenciado e computador, a ser usado para classificar as contas para as políticas de autenticação ou para o isolamento de autenticação.

## <a name="windows-server-2012"></a>Windows Server 2012

Sistema de operacional do controlador de domínio com suporte:

* Windows Server 2019
* Windows Server 2016
* Windows Server 2012 R2
* Windows Server 2012

### <a name="windows-server-2012-forest-functional-level-features"></a>Recursos de nível funcional de floresta Windows Server 2012

* Todos os recursos que estão disponíveis no Windows Server 2008 R2 recursos de nível, mas nenhum adicional funcionais da floresta.

### <a name="windows-server-2012-domain-functional-level-features"></a>Recursos de nível funcional de domínio do Windows Server 2012

* Todos os recursos do Active Directory padrão, todos os recursos do nível funcional do Windows Server 2008 R2 domínio, além dos seguintes recursos:
   * Suporte do KDC para declarações, autenticação composta e proteção diretiva de modelo administrativo KDC tem duas configurações (sempre fornecer declarações e recusar solicitações de autenticação) que exigem o nível funcional de domínio do Windows Server 2012 do Kerberos. Para obter mais informações, consulte [que há de novo na autenticação Kerberos](https://technet.microsoft.com/library/hh831747.aspx)

## <a name="windows-server-2008r2"></a>Windows Server 2008R2

Sistema de operacional do controlador de domínio com suporte:

* Windows Server 2019
* Windows Server 2016
* Windows Server 2012 R2
* Windows Server 2012
* Windows Server 2008 R2

### <a name="windows-server-2008r2-forest-functional-level-features"></a>Windows Server 2008 R2 floresta recursos de nível funcional

* Todos os recursos disponíveis no nível funcional de floresta do Windows Server 2003, além dos seguintes recursos.
   * Lixeira do Active Directory, que fornece a capacidade de restaurar totalmente os objetos excluídos durante a execução do AD DS.

### <a name="windows-server-2008r2-domain-functional-level-features"></a>Windows Server 2008 R2 domínio recursos de nível funcional

* Todos os recursos do Active Directory padrão, todos os recursos do nível funcional de domínio do Windows Server 2008, além dos seguintes recursos:
   * Garantia do mecanismo de autenticação, que agrupa informações sobre o tipo do método de logon (cartão inteligente ou nome de usuário/senha) usado para autenticar os usuários de domínio dentro do token Kerberos de cada usuário. Quando esse recurso está habilitado em um ambiente de rede que implantou uma infraestrutura de gerenciamento de identidade federada, como os serviços de Federação do Active Directory (AD FS), as informações no token podem ser extraídas sempre que um usuário tenta acessar qualquer aplicativo com reconhecimento de declarações que foi desenvolvido para determinar a autorização com base no método de logon do usuário.
   * Gerenciamento automático de SPN para serviços em execução em um determinado computador sob o contexto de uma conta de serviço gerenciado quando o nome ou host DNS das alterações de conta de computador. Para obter mais informações sobre as contas de serviço gerenciado, consulte [guia do passo a passo de contas de serviço](https://go.microsoft.com/fwlink/?LinkId=180401).

## <a name="windows-server-2008"></a>Windows Server 2008

Sistema de operacional do controlador de domínio com suporte:

* Windows Server 2019
* Windows Server 2016
* Windows Server 2012 R2
* Windows Server 2012
* Windows Server 2008 R2
* Windows Server 2008

### <a name="windows-server-2008-forest-functional-level-features"></a>Recursos de nível funcional de floresta Windows Server 2008

* Todos os recursos que estão disponíveis no nível funcional de floresta Windows Server 2003, mas não há recursos adicionais estão disponíveis. 

### <a name="windows-server-2008-domain-functional-level-features"></a>Recursos de nível funcional de domínio do Windows Server 2008

* Todos os padrão do AD DS recursos, todos os recursos do que o nível funcional do domínio de Windows Server 2003, e os recursos a seguir estão disponíveis:
   * Suporte de replicação Distributed File System (DFS) para o Windows Server 2003 Volume do sistema (SYSVOL)
      * Suporte de replicação DFS fornece replicação mais robusta e detalhada do conteúdo de SYSVOL.
        [!NOTE]>
        >Começando com o Windows Server 2012 R2, a replicação FRS (serviço) foi preterido. Um novo domínio é criado em um controlador de domínio que executa pelo menos o Windows Server 2012 R2 deve ser definido para o nível funcional de domínio do Windows Server 2008 ou superior.

   * Baseado em domínio namespaces do DFS em execução no modo Windows Server 2008, que inclui suporte para a enumeração baseada em acesso e maior escalabilidade. Namespaces baseados em domínio no modo do Windows Server 2008 também exigem a floresta para usar o nível funcional de floresta do Windows Server 2003. Para obter mais informações, consulte [escolher um tipo de Namespace](https://go.microsoft.com/fwlink/?LinkId=180400).
   * Suporte avançado de padrão de criptografia (AES 128 e AES 256) para o protocolo Kerberos. Em ordem para TGTs a ser emitido usando AES, o nível funcional do domínio deve ser Windows Server 2008 ou superior e a senha de domínio precisa ser alterado. 
      * Para obter mais informações, consulte [aperfeiçoamentos de Kerberos](https://technet.microsoft.com/library/cc749438(ws.10).aspx).
        [!NOTE]>
        >Erros de autenticação podem ocorrer em um controlador de domínio depois que o nível funcional do domínio é gerado para o Windows Server 2008 ou superior se o controlador de domínio já replicou a alteração DFL, mas ainda não foi atualizado a senha de krbtgt. Nesse caso, uma reinicialização do serviço KDC no controlador de domínio irá disparar uma atualização na memória da nova senha krbtgt e resolva erros de autenticação relacionadas.

   * [Último Logon interativo](https://go.microsoft.com/fwlink/?LinkId=180387) informações exibe as seguintes informações:
      * O número total de tentativas de logon com falha em um servidor do Windows Server 2008 ingressado no domínio ou uma estação de trabalho do Windows Vista
      * O número total de tentativas de logon com falha após um logon bem-sucedido em um servidor Windows Server 2008 ou uma estação de trabalho do Windows Vista
      * A hora da última tentativa de logon com falha em um Windows Server 2008 ou uma estação de trabalho do Windows Vista
      * O horário do último logon bem-sucedido tentativa em um servidor Windows Server 2008 ou uma estação de trabalho do Windows Vista
   * Políticas de senha refinada tornam possível para você especificar políticas de bloqueio de conta e senha para usuários e grupos de segurança global em um domínio. Para obter mais informações, consulte [guia passo a passo para configuração de política de bloqueio de conta e de senha refinada](https://go.microsoft.com/fwlink/?LinkID=91477).
   * Áreas de trabalho virtuais pessoais
      * Para usar a funcionalidade adicional fornecida pela guia Área de trabalho Virtual Pessoal na caixa de diálogo Propriedades da conta de usuário no Active Directory Users and Computers, o esquema do AD DS deve ser estendido para o Windows Server 2008 R2 (versão do esquema do objeto = 47). Para obter mais informações, consulte [Implantando áreas de trabalho virtuais usando RemoteApp e guia de passo a passo de Conexão de área de trabalho](https://go.microsoft.com/fwlink/?LinkId=183552).

## <a name="windows-server-2003"></a>Windows Server 2003

Sistema de operacional do controlador de domínio com suporte:

* Windows Server 2012 R2
* Windows Server 2012
* Windows Server 2008 R2
* Windows Server 2008
* Windows Server 2003

### <a name="windows-server-2003-forest-functional-level-features"></a>Recursos de nível funcional de floresta Windows Server 2003

* Todos os recursos padrão AD DS e os recursos a seguir, estão disponíveis:
   * Relação de confiança de floresta
   * Renomeação de domínio
   * Replicação de valor vinculado
      - Replicação de valor vinculado torna possível para você alterar a associação de grupo para armazenar e replicar valores de membros individuais em vez de replicar a associação inteira como uma única unidade. Armazenando e replicando os valores dos membros individuais usa menos largura de banda de rede e menos ciclos do processador durante a replicação e impede a perda de atualizações quando você adicionar ou remover vários membros simultaneamente em diferentes controladores de domínio.
   * A capacidade de implantar um controlador de domínio somente leitura (RODC)
   * Algoritmos do Knowledge Consistency Checker (KCC) e a escalabilidade aprimorada
      - O gerador de topologia entre sites (ISTG) usa algoritmos aprimorados que são dimensionados para dar suporte a florestas com um número maior de sites que pode dar suporte a AD DS no nível funcional de floresta do Windows 2000. O algoritmo de eleição do ISTG aprimorado é um mecanismo menos intrusiva para escolha do ISTG no nível funcional de floresta do Windows 2000.
   * A capacidade de criar instâncias da classe auxiliar dinâmica chamada **dynamicObject** em uma partição de diretório de domínio
   * A habilidade de converter um **inetOrgPerson** instância de objeto em um **usuário** instância de objeto e para concluir a conversão na direção oposta
   * A capacidade de criar instâncias de novos tipos de grupo para dar suporte à autorização baseada em função. 
      - Esses tipos são chamados de grupos de aplicativos básicos e grupos de consulta LDAP.
   * Desativação e redefinição de atributos e classes no esquema. Os seguintes atributos podem ser reutilizados: ldapDisplayName, schemaIdGuid, OID e mapiID.
   * Baseado em domínio namespaces do DFS em execução no modo Windows Server 2008, que inclui suporte para a enumeração baseada em acesso e maior escalabilidade. Para obter mais informações, consulte [escolher um tipo de Namespace](https://go.microsoft.com/fwlink/?LinkId=180400).

### <a name="windows-server-2003-domain-functional-level-features"></a>Recursos de nível funcional de domínio Windows Server 2003

* Todos os recursos do AD DS do padrão, todos os recursos que estão disponíveis no nível funcional de domínio nativo do Windows 2000 e os recursos a seguir estão disponíveis:
   * A ferramenta de gerenciamento de domínio, Netdom.exe, que torna possível a renomeação de controladores de domínio
   * Atualizações do carimbo de data / hora de logon
      * O atributo **lastLogonTimestamp** é atualizado com o último horário de logon do usuário ou computador. Este atributo é replicado no domínio.
   * A capacidade de definir a **userPassword** atributo como a senha efetiva **inetOrgPerson** e objetos de usuário
   * A capacidade de redirecionar os usuários e computadores contêineres
      * Por padrão, dois contêineres conhecidos são fornecidos para hospedar contas de usuário e computador, ou seja, cn = Computers,<domain root> e cn = Users,<domain root>. Esse recurso permite a definição de um novo local conhecido para essas contas.
   * A capacidade de Gerenciador de autorização armazene diretivas de autorização no AD DS
   * Delegação restrita
      * A delegação restrita possibilita aos aplicativos tirarem vantagem da delegação segura de credenciais de usuário por meio de autenticação baseada em Kerberos.
      * Você pode restringir a delegação apenas a serviços de destino específico.
   * Autenticação seletiva
      * A autenticação seletiva tornam possível para que você possa especificar os usuários e grupos de uma floresta confiável que terão permissão para autenticar servidores de recursos em uma floresta confiante.

## <a name="windows-2000"></a>Windows 2000

Sistema de operacional do controlador de domínio com suporte:

* Windows Server 2008 R2
* Windows Server 2008
* Windows Server 2003
* Windows 2000

### <a name="windows-2000-native-forest-functional-level-features"></a>Recursos de nível funcional de floresta nativo Windows 2000

* Todos os recursos padrão do AD DS estão disponíveis.

### <a name="windows-2000-native-domain-functional-level-features"></a>Recursos de nível funcional de domínio nativo Windows 2000

* Todos os recursos padrão AD DS e os seguintes recursos de diretório estiver disponível, incluindo:
   * Grupos universais para grupos de distribuição e de segurança.
   * Aninhamento de grupo
   * Conversão de grupo, que permite a conversão entre grupos de segurança e distribuição
   * Histórico de SID (identificador) de segurança

## <a name="next-steps"></a>Próximas etapas

* [Aumentar o nível funcional do domínio](https://technet.microsoft.com/library/cc753104.aspx)  
* [Aumentar o nível funcional de floresta](https://technet.microsoft.com/library/cc730985.aspx)
