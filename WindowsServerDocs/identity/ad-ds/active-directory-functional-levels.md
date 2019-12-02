---
ms.assetid: f964d056-11bf-4d9b-b5ab-dceaad8bfbc3
title: Níveis funcionais do Windows Server 2016
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 10/29/2018
ms.topic: article
ms.prod: windows-server
ms.custom: it-pro
ms.reviewer: maheshu
ms.technology: identity-adds
ms.openlocfilehash: 7f16d58eb6c5074c75f49ba7936c4d312a3dbda4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71390983"
---
# <a name="forest-and-domain-functional-levels"></a>Níveis funcionais de floresta e domínio

>Aplica-se a: Windows Server

Os níveis funcionais determinam os recursos de domínio ou floresta do AD DS (Active Directory Domain Services) disponíveis. Eles também determinam quais sistemas operacionais do Windows Server você pode executar em controladores de domínio no domínio ou na floresta. No entanto, os níveis funcionais não afetam quais sistemas operacionais podem ser executados em estações de trabalho e servidores membro que ingressaram no domínio ou na floresta.

Ao implantar o AD DS, defina os níveis funcionais de domínio e floresta com o valor mais alto compatível com seu ambiente. Dessa forma, você pode usar o máximo de recursos de AD DS possível. Ao implantar uma nova floresta, é solicitado que você defina o nível funcional da floresta e, em seguida, defina o nível funcional do domínio. Você pode definir o nível funcional do domínio para um valor maior que o nível funcional da floresta, mas não pode definir o nível funcional do domínio com um valor inferior ao nível funcional da floresta.

Com o fim da vida útil do Windows 2003, os DCs (controladores de domínio) do Windows 2003 precisam ser atualizados para o Windows Server 2008, 2008 R2, 2012, 2012 R2, 2016 ou 2019. Como resultado, qualquer controlador de domínio que execute o Windows Server 2003 deve ser removido do domínio.

Nos níveis funcionais de domínio do Windows Server 2008 e superior, a replicação do DFS (serviço de arquivos distribuído) é usada para replicar o conteúdo da pasta SYSVOL entre controladores de domínio. Se você criar um novo domínio no nível funcional de domínio do Windows Server 2008 ou superior, a Replicação do DFS será usada automaticamente para replicar o SYSVOL. Se você criou o domínio em um nível funcional inferior, será necessário migrar do uso do FRS para a Replicação do DFS para o SYSVOL. Para obter as etapas de migração, siga os [procedimentos no TechNet](https://technet.microsoft.com/library/dd640019(v=WS.10).aspx) ou consulte o [conjunto de etapas otimizadas no blog do Gabinete de arquivos da equipe de armazenamento](http://blogs.technet.com/b/filecab/archive/2014/06/25/streamlined-migration-of-frs-to-dfsr-sysvol.aspx).

## <a name="windows-server-2019"></a>Windows Server 2019

Não há novos níveis funcionais de floresta ou domínio adicionados nesta versão.

O requisito mínimo para adicionar um controlador de domínio do Windows Server 2019 é um nível funcional do Windows Server 2008. O domínio também precisa usar o DFS-R como o mecanismo para replicar o SYSVOL.

## <a name="windows-server-2016"></a>Windows Server 2016

Sistemas operacionais do controlador de domínio compatíveis:

* Windows Server 2019
* Windows Server 2016

### <a name="windows-server-2016-forest-functional-level-features"></a>Recursos de nível funcional de floresta do Windows Server 2016

* Todos os recursos disponíveis no nível funcional de floresta do Windows Server 2012R2 e os seguintes recursos estão disponíveis:
   * [PAM (Privileged Access Management) usando o MIM (Microsoft Identity Manager)](https://docs.microsoft.com/windows-server/identity/whats-new-active-directory-domain-services#a-namebkmkpamaprivileged-access-management)

### <a name="windows-server-2016-domain-functional-level-features"></a>Recursos do nível funcional do domínio do Windows Server 2016

* Todos os recursos padrão do Active Directory, todos os recursos do nível funcional de domínio do Windows Server 2012R2, além destes:
   * Os DCs podem dar suporte à distribuição automática de NTLM e a outros segredos baseados em senha em uma conta de usuário configurada para exigir autenticação PKI. Essa configuração também é conhecida como "Cartão inteligente necessário para logon interativo"
   * Os DCs podem dar suporte à permissão de NTLM de rede quando um usuário é restrito a dispositivos específicos ingressados no domínio.
   * Os clientes Kerberos que se autenticam com êxito com a Extensão PKInit Freshness obterão o novo SID de identidade de chave pública.

    Para obter mais informações, consulte [Novidades na autenticação Kerberos](https://docs.microsoft.com/windows-server/security/kerberos/whats-new-in-kerberos-authentication) e [Novidades na proteção de credenciais](https://docs.microsoft.com/windows-server/security/credentials-protection-and-management/whats-new-in-credential-protection)

## <a name="windows-server-2012r2"></a>Windows Server 2012R2

Sistemas operacionais do controlador de domínio compatíveis:

* Windows Server 2019
* Windows Server 2016
* Windows Server 2012 R2

### <a name="windows-server-2012r2-forest-functional-level-features"></a>Recursos de nível funcional de floresta do Windows Server 2012R2

* Todos os recursos que estão disponíveis no nível funcional de floresta do Windows Server 2012, mas nenhum recurso adicional.

### <a name="windows-server-2012r2-domain-functional-level-features"></a>Recursos de nível funcional de domínio do Windows Server 2012R2

* Todos os recursos padrão do Active Directory, todos os recursos do nível funcional de domínio do Windows Server 2012, além destes:
   * Proteções no lado do DC para usuários protegidos. Usuários protegidos que se autenticam em um domínio do Windows Server 2012 R2 não podem mais:
      * Autenticar-se com a autenticação NTLM
      * Usar pacotes de criptografia DES ou RC4 na pré-autenticação Kerberos
      * Ser delegados com delegação restrita ou irrestrita
      * Renovar tíquetes de usuário (TGTs) além do tempo de vida inicial de quatro horas
   * Políticas de autenticação
      * Novas políticas do Active Directory baseadas em floresta que podem ser aplicadas a contas em domínios do Windows Server 2012 R2 para controlar em quais hosts uma conta pode se conectar e aplicar condições de controle de acesso para autenticação em serviços em execução como uma conta.
   * Silos de política de autenticação
      * Novo objeto do Active Directory baseado em floresta que pode criar uma relação entre usuário, serviço gerenciado e computador, contas a serem usadas para classificar contas para políticas de autenticação ou para isolamento de autenticação.

## <a name="windows-server-2012"></a>Windows Server 2012

Sistemas operacionais do controlador de domínio compatíveis:

* Windows Server 2019
* Windows Server 2016
* Windows Server 2012 R2
* Windows Server 2012

### <a name="windows-server-2012-forest-functional-level-features"></a>Recursos de nível funcional de floresta do Windows Server 2012

* Todos os recursos que estão disponíveis no nível funcional de floresta do Windows Server 2008 R2, mas nenhum recurso adicional.

### <a name="windows-server-2012-domain-functional-level-features"></a>Recursos do nível funcional do domínio do Windows Server 2012

* Todos os recursos padrão do Active Directory, todos os recursos do nível funcional de domínio do Windows Server 2008R2, além destes:
   * O suporte ao KDC para declarações, autenticação composta e política de modelo administrativo KDC de proteção Kerberos tem duas configurações (Sempre fornecer declarações e Reprovar solicitações de autenticação desprotegidas) que exigem o nível funcional de domínio do Windows Server 2012. Para obter mais informações, consulte [Novidades na autenticação Kerberos](https://technet.microsoft.com/library/hh831747.aspx)

## <a name="windows-server-2008r2"></a>Windows Server 2008R2

Sistemas operacionais do controlador de domínio compatíveis:

* Windows Server 2019
* Windows Server 2016
* Windows Server 2012 R2
* Windows Server 2012
* Windows Server 2008 R2

### <a name="windows-server-2008r2-forest-functional-level-features"></a>Recursos de nível funcional de floresta do Windows Server 2008R2

* Todos os recursos disponíveis no nível funcional de floresta do Windows Server 2003, além dos seguintes recursos.
   * Lixeira do Active Directory, que fornece a capacidade de restaurar totalmente os objetos excluídos durante a execução do AD DS.

### <a name="windows-server-2008r2-domain-functional-level-features"></a>Recursos de nível funcional de domínio do Windows Server 2008R2

* Todos os recursos padrão do Active Directory, todos os recursos do nível funcional de domínio do Windows Server 2008, além destes:
   * Garantia do mecanismo de autenticação, que agrupa informações sobre o tipo do método de logon (cartão inteligente ou nome de usuário/senha) usado para autenticar os usuários de domínio dentro do token Kerberos de cada usuário. Quando esse recurso é habilitado em um ambiente de rede que implantou uma infraestrutura de gerenciamento de identidades federadas, como os Serviços de Federação do Active Directory (AD FS), as informações no token podem ser extraídas sempre que um usuário tentar acessar quaisquer aplicativos com reconhecimento de declaração desenvolvidos para determinar a autorização com base no método de logon de um usuário.
   * Gerenciamento automático de SPN para serviços executados em um computador particular, no mesmo contexto de uma Conta de Serviço Gerenciado, quando o nome ou o nome de host de DNS da conta do computador mudar. Para obter mais informações sobre Contas de Serviço Gerenciadas, consulte [Guia Passo a Passo de Contas de Serviço](https://go.microsoft.com/fwlink/?LinkId=180401).

## <a name="windows-server-2008"></a>Windows Server 2008

Sistemas operacionais do controlador de domínio compatíveis:

* Windows Server 2019
* Windows Server 2016
* Windows Server 2012 R2
* Windows Server 2012
* Windows Server 2008 R2
* Windows Server 2008

### <a name="windows-server-2008-forest-functional-level-features"></a>Recursos de nível funcional de floresta do Windows Server 2008

* Todos os recursos que estão disponíveis no nível funcional de floresta do Windows Server2003, mas não há recurso adicional disponível. 

### <a name="windows-server-2008-domain-functional-level-features"></a>Recursos do nível funcional do domínio do Windows Server 2008

* Todos os recursos padrão do AD DS, todos os recursos do nível funcional de domínio do Windows Server 2003 e os seguintes recursos estão disponíveis:
  * Suporte à replicação do Sistema de Arquivos Distribuído (DFS) para o SYSVOL (volume do sistema) do Windows Server 2003
    * O suporte à Replicação do DFS oferece replicação mais robusta e detalhada de conteúdo do SYSVOL.

      > [!NOTE]
      > Desde o Windows Server 2012 R2, o FRS (serviço de replicação de arquivos) foi preterido. Um novo domínio criado em um controlador de domínio que executa pelo menos o Windows Server 2012 R2 deve ser definido para o nível funcional do domínio do Windows Server 2008 ou superior.

  * Namespaces do DFS baseados em domínio em execução no modo Windows Server 2008, que inclui suporte para a enumeração baseada em acesso e maior escalabilidade. Os namespaces baseados em domínio no modo Windows Server 2008 também exigem que a floresta use o nível funcional de floresta do Windows Server 2003. Para obter mais informações, consulte [Escolher um tipo de namespace](https://go.microsoft.com/fwlink/?LinkId=180400).
  * Suporte da criptografia AES (AES 128 e AES 256) para o protocolo Kerberos. Para que os TGTs sejam emitidos usando a AES, o nível funcional do domínio deve ser o Windows Server 2008 ou superior e a senha do domínio precisa ser alterada. 
    * Para obter mais informações, consulte [Aprimoramentos do Kerberos](https://technet.microsoft.com/library/cc749438(ws.10).aspx).

      > [!NOTE]
      >Os erros de autenticação poderão ocorrer em um controlador de domínio depois que o nível funcional do domínio for elevado para o Windows Server 2008 ou superior se o controlador de domínio já tiver replicado a alteração de DFL, mas ainda não tiver atualizado a senha krbtgt. Nesse caso, uma reinicialização do serviço KDC no controlador de domínio vai disparar uma atualização na memória da nova senha krbtgt e resolver os erros de autenticação relacionados.

  * As informações sobre o [último logon interativo](https://go.microsoft.com/fwlink/?LinkId=180387) exibem o seguinte:
     * O número total de falhas em tentativas de logon em um servidor do Windows Server 2008 que ingressou em um domínio ou uma estação de trabalho do Windows Vista
     * O número total de falhas em tentativas de logon após um logon bem-sucedido em um servidor do Windows Server 2008 ou uma estação de trabalho do Windows Vista
     * O horário da última falha em tentativa de logon em uma estação de trabalho do Windows Vista ou Windows Server 2008
     * O horário da última tentativa de logon bem-sucedida em uma estação de trabalho do Windows Vista ou Windows Server 2008
  * Políticas refinadas de senha possibilitam que você especifique as políticas de bloqueio de senha e de conta para grupos de segurança global e usuários em um domínio. Para obter mais informações, consulte o [Guia passo a passo para configuração de políticas refinadas de bloqueio de senhas e contas](https://go.microsoft.com/fwlink/?LinkID=91477).
  * Áreas de Trabalho Virtual Pessoal
     * Para usar a funcionalidade adicional fornecida pela guia Área de Trabalho Virtual Pessoal na caixa de diálogo Propriedades da Conta de Usuário em Usuários e Computadores do Active Directory, o esquema do AD DS deve ser estendido para o Windows Server 2008 R2 (versão do objeto de esquema = 47). Para obter mais informações, veja [Guia Passo a Passo de Implantação de Áreas de Trabalho Virtuais Pessoais Usando Conexão de RemoteApp e Área de Trabalho](https://go.microsoft.com/fwlink/?LinkId=183552).

## <a name="windows-server-2003"></a>Windows Server 2003

Sistemas operacionais do controlador de domínio compatíveis:

* Windows Server 2012 R2
* Windows Server 2012
* Windows Server 2008 R2
* Windows Server 2008
* Windows Server 2003

### <a name="windows-server-2003-forest-functional-level-features"></a>Recursos de nível funcional de floresta do Windows Server 2003

* Todos os recursos padrão do AD DS e os seguintes recursos estão disponíveis:
   * Relação de confiança de floresta
   * Renomeação de domínio
   * Replicação de valor vinculado
      - A replicação de valor vinculado possibilita a você alterar a associação de grupo para armazenar e replicar valores de membros individuais em vez de replicar a associação inteira como uma única unidade. Armazenar e replicar os valores de membros individuais usa menos largura de banda de rede e menos ciclos de processador durante a replicação e impede que você perca atualizações ao adicionar ou remover vários membros simultaneamente em controladores de domínio diferentes.
   * A capacidade de implantar um RODC (controlador de domínio somente leitura)
   * Algoritmos e escalabilidade aprimorados do KCC (Knowledge Consistency Checker)
      - O ISTG (gerador de topologia entre sites) usa algoritmos aprimorados, que geram um escalonamento para oferecer suporte a florestas com um número de sites maior que o compatível com o AD DS no nível funcional de floresta do Windows 2000. O algoritmo de eleição do ISTG aprimorado é um mecanismo menos invasivo para a escolha do ISTG no nível funcional de floresta do Windows 2000.
   * A capacidade de criar instâncias da classe auxiliar dinâmica nomeada **dynamicObject** em uma partição do diretório de domínio
   * A capacidade de converter uma instância de objeto **inetOrgPerson** em uma instância de objeto **Usuário** e para concluir a conversão na direção oposta
   * A capacidade de criar instâncias de novos tipos de grupo para dar suporte à autorização baseada em função. 
      - Esses tipos são chamados de grupos básicos de aplicativos e grupos de consulta LDAP.
   * Desativação e redefinição de atributos e classes no esquema. Os seguintes atributos podem ser reutilizados: ldapDisplayName, schemaIdGuid, OID e mapiID.
   * Namespaces do DFS baseados em domínio em execução no modo Windows Server 2008, que inclui suporte para a enumeração baseada em acesso e maior escalabilidade. Para obter mais informações, consulte [Escolher um tipo de namespace](https://go.microsoft.com/fwlink/?LinkId=180400).

### <a name="windows-server-2003-domain-functional-level-features"></a>Recursos do nível funcional do domínio do Windows Server 2003

* Todos os recursos padrão do AD DS, todos os recursos disponíveis no nível funcional de domínio nativo do Windows 2000 e os seguintes recursos estão disponíveis:
   * A ferramenta de gerenciamento de domínio, Netdom.exe, que possibilita renomear controladores de domínio
   * Atualizações de carimbo de data/hora de logon
      * O atributo **lastLogonTimestamp** é atualizado com o último horário de logon do usuário ou computador. Este atributo é replicado no domínio.
   * A capacidade de definir o atributo **userPassword** como a senha efetiva em **inetOrgPerson** e nos objetos de usuário
   * A capacidade de redirecionar contêineres de Usuários e Computadores
      * Por padrão, dois contêineres conhecidos são fornecidos para hospedar contas de computador e usuário, a saber: cn=Computers,<domain root> e cn=Users,<domain root>. Esse recurso permite a definição de um novo local conhecido para essas contas.
   * A capacidade do Gerenciador de Autorização armazenar as políticas de autorização no AD DS
   * Delegação restrita
      * A delegação restrita possibilita aos aplicativos aproveitarem a delegação segura de credenciais de usuário por meio da autenticação baseada em Kerberos.
      * Você pode restringir a delegação a apenas serviços de destino específicos.
   * Autenticação seletiva
      * A autenticação seletiva possibilita especificar os usuários e grupos de uma floresta confiável que terão permissão para autenticar servidores de recursos em uma floresta confiável.

## <a name="windows-2000"></a>Windows 2000

Sistemas operacionais do controlador de domínio compatíveis:

* Windows Server 2008 R2
* Windows Server 2008
* Windows Server 2003
* Windows 2000

### <a name="windows-2000-native-forest-functional-level-features"></a>Recursos de nível funcional de floresta nativos do Windows 2000

* Todos os recursos padrão do AD DS estão disponíveis.

### <a name="windows-2000-native-domain-functional-level-features"></a>Recursos de nível funcional de domínio nativos do Windows 2000

* Todos os recursos padrão do AD DS e os seguintes recursos de diretório estão disponíveis, incluindo:
   * Grupos universais para grupos de segurança e de distribuição.
   * Aninhamento de grupo
   * Conversão de grupo, que permite a conversão entre grupos de segurança e de distribuição
   * Histórico de SID (identificador de segurança)

## <a name="next-steps"></a>Próximas etapas

* [Aumentar o nível funcional do domínio](https://technet.microsoft.com/library/cc753104.aspx)  
* [Aumentar o nível funcional da floresta](https://technet.microsoft.com/library/cc730985.aspx)
