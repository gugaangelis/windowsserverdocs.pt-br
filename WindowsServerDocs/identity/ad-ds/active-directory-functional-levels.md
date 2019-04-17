---
ms.assetid: f964d056-11bf-4d9b-b5ab-dceaad8bfbc3
title: "Níveis funcionais do Windows Server 2016"
description: 
author: MicrosoftGuyJFlo
ms.author: joflore
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.custom: it-pro
ms.reviewer: maheshu
ms.technology: identity-adds
ms.openlocfilehash: a39955cf088ce7ce8bef20c70b83d49c6d508497
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="forest-and-domain-functional-levels"></a>Níveis funcionais de floresta e domínio

>Aplica-se a: Windows Server

Com o fim da vida útil do Windows 2003, (controladores de domínio do Windows 2003) precisam ser atualizado para o Windows Server 2008, 2012 ou 2016. Como resultado, qualquer controlador de domínio que executa o Windows Server 2003 deve ser removido do domínio. O nível funcional do domínio e da floresta deve ser aumentado para pelo menos Windows Server 2008 para impedir que um controlador de domínio que executa uma versão anterior do Windows Server sejam adicionados ao ambiente.

Recomendamos que os clientes atualizar seus nível funcional do domínio (DFL) e o nível funcional da floresta (FFL) como parte disso, desde que o DFL 2003 e FFL foram preteridas no Windows Server 2016 e eles não terão suporte em futuras versões.

Para clientes que precisam de mais tempo para avaliar a mover seus DFL & FFL de 2003, o DFL 2003 e FFL continuarão a ser compatível com Windows 10 e Windows Server 2016 fornecidas são todos os controladores de domínio na floresta e domínio no Windows Server 2008, 2008R2, 2012, 2012R2, ou 2016.

No Windows Server 2008 e maiores níveis funcionais de domínio, replicação do serviço de arquivos distribuído (DFS) é usada para replicar o conteúdo da pasta SYSVOL entre controladores de domínio. Se você criar um novo domínio no nível funcional de domínio do Windows Server 2008 ou superior, replicação DFS automaticamente é usada para replicar SYSVOL. Se você criou no domínio em um nível funcional inferior, você precisará migrar usando FRS para replicação do DFS para SYSVOL. Para obter as etapas de migração, você pode qualquer siga o [procedimentos no TechNet](https://technet.microsoft.com/library/dd640019(v=WS.10).aspx) ou você pode consultar o [simplificada conjunto de etapas no blog da equipe de armazenamento arquivo de gabinete](http://blogs.technet.com/b/filecab/archive/2014/06/25/streamlined-migration-of-frs-to-dfsr-sysvol.aspx).

Os Windows Server 2003 floresta e domínio níveis funcionais continuam a ter suporte, mas as organizações devem aumentar o nível funcional para o Windows Server 2008 (ou superior, se possível) para garantir a compatibilidade de replicação SYSVOL e suporte no futuro. Além disso, há muitos outros benefícios e os recursos disponíveis em níveis funcionais mais altos maior. Consulte os seguintes recursos para obter mais informações:

## <a name="windows-server-2016"></a>Windows Server 2016

Sistema de operacional do controlador de domínio que têm suporte:

* Windows Server 2016

### <a name="windows-server-2016-forest-functional-level-features"></a>Recursos de nível funcionais da floresta de Windows Server 2016

* Todos os recursos que estão disponíveis no Windows Server 2012R2 nível funcional da floresta e os recursos a seguir, estão disponíveis:
    * [Acesso privilegiado gerenciamento (PAM) usando o Microsoft Identity Manager (MIM)] (https://docs.microsoft.com/windows-server/identity/what's-new-active-directory-domain-services#a-namebkmkpamaprivileged-access-management)

### <a name="windows-server-2016-domain-functional-level-features"></a>Recursos de nível funcionais de domínio do Windows Server 2016

* Todos os recursos do Active Directory padrão, todos os recursos do nível funcional do Windows Server 2012R2 domínio, além dos seguintes recursos:
    * Controladores de domínio podem dar suporte a Implantando uma pública chave somente segredos do usuário NTLM. 
    * Controladores de domínio podem dar suporte a rede permitindo NTLM quando um usuário está restrito a específico dispositivos ingressados em domínio. 
    * Kerberos clientes se autentiquem com êxito com a extensão de renovação de PKInit obterão a identidade de chave pública nova SID.

    Para obter mais informações, consulte [What's New na autenticação Kerberos](https://docs.microsoft.com/windows-server/security/kerberos/whats-new-in-kerberos-authentication) e [que há de novo na proteção de credenciais](https://docs.microsoft.com/windows-server/security/credentials-protection-and-management/whats-new-in-credential-protection)

## <a name="windows-server-2012r2"></a>Windows Server 2012R2

Sistema de operacional do controlador de domínio que têm suporte:

* Windows Server 2016
* Windows Server 2012 R2

### <a name="windows-server-2012r2-forest-functional-level-features"></a>Windows Server 2012R2 floresta nível recursos funcionais

* Todos os recursos que estão disponíveis no Windows Server 2012 floresta recursos funcionais de nível, mas não adicionais.

### <a name="windows-server-2012r2-domain-functional-level-features"></a>Windows Server 2012R2 domínio nível recursos funcionais

* Todos os recursos do Active Directory padrão, todos os recursos do nível funcional de domínio do Windows Server 2012, além dos seguintes recursos:
    * Proteções de DC voltada para usuários protegido. Protegidas autenticação de usuários para um Windows Server 2012 R2 domínio não poderá mais:
        * Autenticar com a autenticação NTLM
        * Usar pacotes de codificação DES ou RC4 na pré-autenticação Kerberos
        * Ser recebido com a delegação restrita ou irrestrita
        * Renovar tíquetes de usuário (TGTs) além do tempo de vida inicial 4 horas
    * Políticas de autenticação
        * Nova floresta do Active Directory políticas que podem ser aplicadas a contas em domínios do Windows Server 2012 R2 para controlar quais hosts uma conta pode logon do e aplicar condições de controle de acesso para autenticação para os serviços executados como uma conta.
    * Silos de política de autenticação
        * Nova floresta do Active Directory objeto baseado, que pode criar um relacionamento entre contas de usuário, serviço gerenciado e computador, a ser usada para classificar contas para políticas de autenticação ou para isolamento de autenticação.

## <a name="windows-server-2012"></a>Windows Server 2012

Sistema de operacional do controlador de domínio que têm suporte:

* Windows Server 2016
* Windows Server 2012 R2
* Windows Server 2012

### <a name="windows-server-2012-forest-functional-level-features"></a>Recursos de nível funcionais da floresta de Windows Server 2012

* Todos os recursos que estão disponíveis no Windows Server 2008 R2 de floresta recursos funcionais de nível, mas não adicionais.

### <a name="windows-server-2012-domain-functional-level-features"></a>Recursos de nível funcionais de domínio do Windows Server 2012

* Todos os recursos do Active Directory padrão, todos os recursos do nível funcional do Windows Server 2008R2 domínio, além dos seguintes recursos:
    * Suporte do KDC para declarações, autenticação composta e Kerberos proteção política de modelo administrativo do KDC tem duas configurações (sempre fornecer declarações e falhar solicitações de autenticação unarmored) que exigem o nível funcional de domínio do Windows Server 2012. Para obter mais informações, consulte [What's New na autenticação Kerberos](https://technet.microsoft.com/en-us/library/hh831747.aspx)

## <a name="windows-server-2008r2"></a>Windows Server 2008R2

Sistema de operacional do controlador de domínio que têm suporte:

* Windows Server 2016
* Windows Server 2012 R2
* Windows Server 2012
* Windows Server 2008 R2

### <a name="windows-server-2008r2-forest-functional-level-features"></a>Windows Server 2008R2 floresta nível recursos funcionais

* Todos os recursos que estão disponíveis no Windows Server 2003 de floresta nível funcional, mais os seguintes recursos:
    * Lixeira do Active Directory, que fornece a capacidade de restaurar objetos excluídos em sua totalidade durante a execução do AD DS.

### <a name="windows-server-2008r2-domain-functional-level-features"></a>Windows Server 2008R2 domínio nível recursos funcionais

* Todos os recursos do Active Directory padrão, todos os recursos do nível funcional de domínio do Windows Server 2008, além dos seguintes recursos:
    * Garantia do mecanismo de autenticação, que empacota as informações sobre o tipo de método de logon (cartão inteligente ou nome de usuário/senha) que é usado para autenticar usuários de domínio dentro de cada Kerberos token de usuário. Quando esse recurso é habilitado em um ambiente de rede que implantou uma infraestrutura de gerenciamento de identidade federada, como os serviços de Federação do Active Directory (AD FS), as informações no token podem ser extraídas sempre que um usuário tenta acessar qualquer aplicativo com reconhecimento de declarações que tenha sido desenvolvido para determinar a autorização com base no método de logon do usuário.
    * Gerenciamento de SPN automático para serviços em execução em um computador específico no contexto de uma conta de serviço gerenciado quando o nome ou host DNS das mudanças de conta de computador. Para saber mais sobre contas de serviço gerenciado, consulte [contas de serviço Step-by-Step guia](https://go.microsoft.com/fwlink/?LinkId=180401).

## <a name="windows-server-2008"></a>Windows Server 2008

Sistema de operacional do controlador de domínio que têm suporte:

* Windows Server 2016
* Windows Server 2012 R2
* Windows Server 2012
* Windows Server 2008 R2
* Windows Server 2008

### <a name="windows-server-2008-forest-functional-level-features"></a>Recursos de nível funcionais da floresta de Windows Server 2008

* Todos os recursos que estão disponíveis no nível funcional da floresta do Windows Server 2003, mas não há recursos adicionais estão disponíveis. 

### <a name="windows-server-2008-domain-functional-level-features"></a>Recursos de nível funcionais de domínio do Windows Server 2008

* Todos da padrão AD DS possui, todos os recursos do Windows Server 2003 funcional do nível de domínio, e os recursos a seguir estão disponíveis:
    * Suporte de replicação distribuído (DFS File System) para o Windows Server 2003 Volume do sistema (SYSVOL)
        * Suporte de replicação do DFS fornece replicação mais robusta e detalhada dos conteúdos SYSVOL.
        [!NOTE]>
        >A partir do Windows Server 2012 R2, o serviço de replicação de arquivos (FRS) foi preterido. Um novo domínio é criado em um controlador de domínio que é executado pelo menos Windows Server 2012 R2 deve ser definido como o nível funcional de domínio do Windows Server 2008 ou superior.

    * Baseado em domínio namespaces DFS em execução no modo Windows Server 2008, que inclui suporte para enumeração baseada em acesso e escalabilidade. Namespaces baseados em domínio no modo do Windows Server 2008 também exigem da floresta para usar o nível funcional da floresta do Windows Server 2003. Para obter mais informações, consulte [escolher um tipo de Namespace](https://go.microsoft.com/fwlink/?LinkId=180400).
    * Suporte Advanced Encryption Standard (AES 128 e AES 256) para o protocolo Kerberos. Em ordem para TGTs ser emitido usando AES, o nível funcional do domínio deve ser Windows Server 2008 ou superior e a senha de domínio precisa ser alterado. 
        * Para obter mais informações, consulte [Kerberos aprimoramentos](https://technet.microsoft.com/library/cc749438(ws.10).aspx).
        [!NOTE]>
        >Autenticação erros podem ocorrer em um controlador de domínio depois que o nível funcional do domínio é gerado para o Windows Server 2008 ou superior se o controlador de domínio já duplicou a alteração DFL, mas ainda não tiver atualizado a senha krbtgt. Nesse caso, uma reinicialização do serviço KDC no controlador de domínio irá disparar uma atualização na memória da nova senha krbtgt e resolver erros de autenticação relacionados.

    * [Último Logon interativo](https://go.microsoft.com/fwlink/?LinkId=180387) informações exibe as seguintes informações:
        * O número total de tentativas de logon com falha em um servidor Windows Server 2008 ingressado no domínio ou uma estação de trabalho do Windows Vista
        * O número total de tentativas de logon com falha após um logon bem-sucedido para um servidor Windows Server 2008 ou uma estação de trabalho do Windows Vista
        * O tempo da última tentativa de logon com falha em um Windows Server 2008 ou uma estação de trabalho do Windows Vista
        * O tempo do último logon bem-sucedido tentativa em um servidor Windows Server 2008 ou uma estação de trabalho do Windows Vista
    * Políticas de senha refinadas tornam possível para que você especificar políticas de bloqueio de conta e senha para usuários e grupos de segurança globais em um domínio. Para obter mais informações, consulte [guia passo a passo de configuração de política de bloqueio de conta e de senha refinadas](https://go.microsoft.com/fwlink/?LinkID=91477).
    * Áreas de trabalho virtuais pessoais
        * Para usar a funcionalidade adicional fornecida pela guia Área de trabalho virtuais pessoais na caixa de diálogo Propriedades da conta de usuário no Active Directory Users and Computers, seu esquema do AD DS deve ser estendida para o Windows Server 2008 R2 (versão do esquema do objeto = 47). Para obter mais informações, consulte [implantação de áreas de trabalho virtuais usando RemoteApp e Conexão de área de trabalho Step-by-Step guia](https://go.microsoft.com/fwlink/?LinkId=183552).

## <a name="windows-server-2003"></a>Windows Server 2003

Sistema de operacional do controlador de domínio que têm suporte:

* Windows Server 2012 R2
* Windows Server 2012
* Windows Server 2008 R2
* Windows Server 2008
* Windows Server 2003

### <a name="windows-server-2003-forest-functional-level-features"></a>Recursos de nível funcionais da floresta de Windows Server 2003

* Todos os recursos padrão AD DS e os recursos a seguir, estão disponíveis:
    * Relação de confiança de floresta
    * Alteração de nome de domínio
    * Replicação de valor vinculado
        - Replicação de valor vinculado torna possível para que você alterar a associação de grupo para armazenar e replicar valores de membros individuais, em vez de replicar a associação inteira como uma única unidade. Armazenar e replicar os valores de membros individuais usa menos largura de banda de rede e menos ciclos de processador durante a replicação e impede que você perder atualizações quando você adicionar ou remove membros de vários simultaneamente em controladores de domínio diferentes.
    * A capacidade de implantar um controlador de domínio somente leitura (RODC)
    * Algoritmos de verificador de consistência de Conhecimento (KCC) e escalabilidade aprimorada
        - O gerador de topologia entre sites (ISTG) usa algoritmos aprimorados que são dimensionados para dar suporte a florestas com um número maior de sites do AD DS pode dar suporte no nível funcional da floresta do Windows 2000. O algoritmo de eleições aprimorado do ISTG é um mecanismo menos intrusiva para escolher o ISTG no nível funcional da floresta do Windows 2000.
    * A capacidade de criar instâncias da classe auxiliar dinâmica chamada **dynamicObject** em uma partição de diretório de domínio
    * A capacidade de converter um **inetOrgPerson** instância de objeto em uma **usuário** instância de objeto e para concluir a conversão na direção oposta
    * A capacidade de criar instâncias dos novos tipos de grupo para dar suporte a autorização baseada em função. 
        - Esses tipos são chamados grupos de aplicativos básicos e grupos de consulta LDAP.
    * Desativação e redefinição de atributos e classes no esquema. Os atributos a seguir podem ser reutilizados: ldapDisplayName, schemaIdGuid, OID e mapiID.
    * Baseado em domínio namespaces DFS em execução no modo Windows Server 2008, que inclui suporte para enumeração baseada em acesso e escalabilidade. Para obter mais informações, consulte [escolher um tipo de Namespace](https://go.microsoft.com/fwlink/?LinkId=180400).

### <a name="windows-server-2003-domain-functional-level-features"></a>Recursos de nível funcionais de domínio do Windows Server 2003

* Todos os recursos padrão AD DS, todos os recursos que estão disponíveis no nível funcional de domínio nativo do Windows 2000 e os recursos a seguir estão disponíveis:
    * A ferramenta de gerenciamento de domínio, Netdom.exe, que torna possível para renomear controladores de domínio
    * Atualizações de carimbo de data / hora de logon
        * O **lastLogonTimestamp** atributo é atualizado com a última vez de logon do usuário ou computador. Esse atributo é replicado no domínio.
    * A capacidade de definir o **userPassword** atributo como a senha efetiva no **inetOrgPerson** e objetos de usuário
    * A capacidade de redirecionar os usuários e computadores contêineres
        * Por padrão, dois contêineres conhecidos são fornecidos para hospedar o computador e contas de usuário, ou seja, cn = computadores,<domain root> e cn = usuários,<domain root>. Esse recurso permite que a definição de um novo local conhecida para essas contas.
    * A capacidade do Gerenciador de autorização para armazenar suas políticas de autorização no AD DS
    * Delegação restrita
        * Delegação restrita torna possível aplicativos podem tirar proveito da delegação segura de credenciais do usuário por meio de autenticação Kerberos.
        * Você pode restringir delegação destino específico apenas aos serviços.
    * Autenticação seletiva
        * É possível que você especifique os usuários e grupos de uma floresta confiável que têm permissão para autenticar servidores de recursos em uma floresta confiante torna a autenticação seletiva.

## <a name="windows-2000"></a>Windows 2000

Sistema de operacional do controlador de domínio que têm suporte:

* Windows Server 2008 R2
* Windows Server 2008
* Windows Server 2003
* Windows 2000

### <a name="windows-2000-native-forest-functional-level-features"></a>Recursos de nível funcionais de floresta nativo Windows 2000

* Todos os recursos do padrão AD DS estão disponíveis.

### <a name="windows-2000-native-domain-functional-level-features"></a>Recursos de nível funcionais de domínio nativo Windows 2000

* Todos os recursos padrão AD DS e os seguintes recursos de diretório estiver disponível, incluindo:
    * Grupos universais para grupos de distribuição e segurança.
    * Aninhamento de grupo
    * Conversão de grupo, que permite a conversão entre grupos de segurança e distribuição
    * Histórico SID (identificador) de segurança

## <a name="next-steps"></a>Próximas etapas

* [Aumentar o nível funcional do domínio](https://technet.microsoft.com/library/cc753104.aspx)  
* [Aumentar o nível funcional da floresta](https://technet.microsoft.com/library/cc730985.aspx)
