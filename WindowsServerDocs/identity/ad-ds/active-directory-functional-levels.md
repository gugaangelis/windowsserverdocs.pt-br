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
ms.openlocfilehash: c1e2108084b03fabbf7c6a18c2ecbcaf3cbd1dd9
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70868259"
---
# <a name="forest-and-domain-functional-levels"></a>Níveis funcionais de floresta e domínio

>Aplica-se a: Windows Server

Os níveis funcionais determinam os recursos de domínio ou floresta Active Directory Domain Services (AD DS) disponíveis. Eles também determinam quais sistemas operacionais Windows Server você pode executar em controladores de domínio no domínio ou na floresta. No entanto, os níveis funcionais não afetam os sistemas operacionais que podem ser executados em estações de trabalho e servidores membro que ingressaram no domínio ou na floresta.

Ao implantar AD DS, defina os níveis funcionais de domínio e floresta para o valor mais alto com o qual seu ambiente pode dar suporte. Dessa forma, você pode usar o máximo de recursos de AD DS possível. Quando você implanta uma nova floresta, é solicitado que você defina o nível funcional da floresta e, em seguida, defina o nível funcional do domínio. Você pode definir o nível funcional do domínio para um valor maior que o nível funcional da floresta, mas não pode definir o nível funcional do domínio como um valor inferior ao nível funcional da floresta.

Com o fim da vida útil do Windows 2003, os DCs (controladores de domínio) do Windows 2003 precisam ser atualizados para o Windows Server 2008, 2008R2, 2012, 2012R2, 2016 ou 2019. Como resultado, qualquer controlador de domínio que executa o Windows Server 2003 deve ser removido do domínio.

Nos níveis funcionais de domínio do Windows Server 2008 e superior, a replicação do serviço de arquivos distribuído (DFS) é usada para replicar o conteúdo da pasta SYSVOL entre controladores de domínio. Se você criar um novo domínio no nível funcional de domínio do Windows Server 2008 ou superior, Replicação do DFS será usado automaticamente para replicar o SYSVOL. Se você criou o domínio em um nível funcional inferior, será necessário migrar do usando o FRS para a Replicação DFS para SYSVOL. Para obter as etapas de migração, você pode seguir os [procedimentos no TechNet](https://technet.microsoft.com/library/dd640019(v=WS.10).aspx) ou pode consultar o [conjunto simplificado de etapas no blog do gabinete de arquivo da equipe de armazenamento](http://blogs.technet.com/b/filecab/archive/2014/06/25/streamlined-migration-of-frs-to-dfsr-sysvol.aspx).

## <a name="windows-server-2019"></a>Windows Server 2019

Não há novos níveis funcionais de floresta ou domínio adicionados nesta versão.

O requisito mínimo para adicionar um controlador de domínio do Windows Server 2019 é um nível funcional do Windows Server 2008. O domínio também precisa usar o DFS-R como o mecanismo para replicar o SYSVOL.

## <a name="windows-server-2016"></a>Windows Server 2016

Sistema operacional do controlador de domínio com suporte:

* Windows Server 2019
* Windows Server 2016

### <a name="windows-server-2016-forest-functional-level-features"></a>Recursos de nível funcional de floresta do Windows Server 2016

* Todos os recursos que estão disponíveis no nível funcional de floresta 2012R2 do Windows Server e os seguintes recursos estão disponíveis:
   * [PAM (Privileged Access Management) usando o Microsoft Identity Manager (MIM)](https://docs.microsoft.com/windows-server/identity/whats-new-active-directory-domain-services#a-namebkmkpamaprivileged-access-management)

### <a name="windows-server-2016-domain-functional-level-features"></a>Recursos de nível funcional de domínio do Windows Server 2016

* Todos os recursos de Active Directory padrão, todos os recursos do nível funcional de domínio 2012R2 do Windows Server, além dos seguintes recursos:
   * Os DCs podem dar suporte à distribuição automática de NTLM e a outros segredos baseados em senha em uma conta de usuário configurada para exigir autenticação PKI. Essa configuração também é conhecida como "cartão inteligente necessário para logon interativo"
   * Os DCs podem dar suporte à permissão de NTLM de rede quando um usuário está restrito a dispositivos ingressados no domínio específico.
   * Os clientes Kerberos que se autenticam com êxito com a extensão de atualização PKInit obterão o novo SID de identidade de chave pública.

    Para obter mais informações, consulte [o que há de novo na autenticação Kerberos](https://docs.microsoft.com/windows-server/security/kerberos/whats-new-in-kerberos-authentication) e [o que há de novo na proteção de credenciais](https://docs.microsoft.com/windows-server/security/credentials-protection-and-management/whats-new-in-credential-protection)

## <a name="windows-server-2012r2"></a>2012R2 do Windows Server

Sistema operacional do controlador de domínio com suporte:

* Windows Server 2019
* Windows Server 2016
* Windows Server 2012 R2

### <a name="windows-server-2012r2-forest-functional-level-features"></a>Recursos de nível funcional de floresta do Windows Server 2012R2

* Todos os recursos que estão disponíveis no nível funcional de floresta do Windows Server 2012, mas sem recursos adicionais.

### <a name="windows-server-2012r2-domain-functional-level-features"></a>Recursos do nível funcional do domínio 2012R2 do Windows Server

* Todos os recursos de Active Directory padrão, todos os recursos do nível funcional de domínio do Windows Server 2012, além dos seguintes recursos:
   * Proteções no lado do DC para usuários protegidos. Usuários protegidos que se autenticam em um domínio do Windows Server 2012 R2 não podem mais:
      * Autenticar com autenticação NTLM
      * Usar conjuntos de codificação DES ou RC4 na pré-autenticação Kerberos
      * Ser delegado com delegação irrestrita ou restrita
      * Renovar tíquetes de usuário (TGTs) Além do tempo de vida inicial de 4 horas
   * Políticas de autenticação
      * Novas políticas de Active Directory baseadas em floresta que podem ser aplicadas a contas em domínios do Windows Server 2012 R2 para controlar em quais hosts uma conta pode se conectar e aplicar condições de controle de acesso para autenticação em serviços em execução como uma conta.
   * Silos de política de autenticação
      * Novo objeto de Active Directory baseado em floresta, que pode criar uma relação entre usuário, serviço gerenciado e computador, contas a serem usadas para classificar contas para políticas de autenticação ou isolamento de autenticação.

## <a name="windows-server-2012"></a>Windows Server 2012

Sistema operacional do controlador de domínio com suporte:

* Windows Server 2019
* Windows Server 2016
* Windows Server 2012 R2
* Windows Server 2012

### <a name="windows-server-2012-forest-functional-level-features"></a>Recursos de nível funcional de floresta do Windows Server 2012

* Todos os recursos que estão disponíveis no nível funcional de floresta do Windows Server 2008 R2, mas sem recursos adicionais.

### <a name="windows-server-2012-domain-functional-level-features"></a>Recursos de nível funcional de domínio do Windows Server 2012

* Todos os recursos de Active Directory padrão, todos os recursos do nível funcional de domínio 2008R2 do Windows Server, além dos seguintes recursos:
   * O suporte do KDC para declarações, autenticação composta e política de modelo administrativo do KDC de proteção Kerberos tem duas configurações (sempre fornecer declarações e falhas de solicitações de autenticação não protegidas) que exigem o nível funcional de domínio do Windows Server 2012. Para obter mais informações, consulte [novidades na autenticação Kerberos](https://technet.microsoft.com/library/hh831747.aspx)

## <a name="windows-server-2008r2"></a>2008R2 do Windows Server

Sistema operacional do controlador de domínio com suporte:

* Windows Server 2019
* Windows Server 2016
* Windows Server 2012 R2
* Windows Server 2012
* Windows Server 2008 R2

### <a name="windows-server-2008r2-forest-functional-level-features"></a>Recursos de nível funcional de floresta do Windows Server 2008R2

* Todos os recursos disponíveis no nível funcional de floresta do Windows Server 2003, além dos seguintes recursos.
   * Lixeira do Active Directory, que fornece a capacidade de restaurar totalmente os objetos excluídos durante a execução do AD DS.

### <a name="windows-server-2008r2-domain-functional-level-features"></a>Recursos do nível funcional do domínio 2008R2 do Windows Server

* Todos os recursos de Active Directory padrão, todos os recursos do nível funcional de domínio do Windows Server 2008, além dos seguintes recursos:
   * Garantia de mecanismo de autenticação, que empacota informações sobre o tipo de método de logon (cartão inteligente ou nome de usuário/senha) que é usado para autenticar usuários de domínio dentro do token Kerberos de cada usuário. Quando esse recurso é habilitado em um ambiente de rede que implantou uma infraestrutura de gerenciamento de identidades federadas, como o Serviços de Federação do Active Directory (AD FS) (AD FS), as informações no token podem ser extraídas sempre que um usuário tentar acessar qualquer um aplicativo com reconhecimento de declaração que foi desenvolvido para determinar a autorização com base no método de logon de um usuário.
   * Gerenciamento automático de SPN para serviços em execução em um determinado computador sob o contexto de uma conta de serviço gerenciado quando o nome ou o nome de host DNS da conta da máquina é alterado. Para obter mais informações sobre contas de serviço gerenciado, consulte [guia passo a passo das contas de serviço](https://go.microsoft.com/fwlink/?LinkId=180401).

## <a name="windows-server-2008"></a>Windows Server 2008

Sistema operacional do controlador de domínio com suporte:

* Windows Server 2019
* Windows Server 2016
* Windows Server 2012 R2
* Windows Server 2012
* Windows Server 2008 R2
* Windows Server 2008

### <a name="windows-server-2008-forest-functional-level-features"></a>Recursos de nível funcional de floresta do Windows Server 2008

* Todos os recursos disponíveis no nível funcional de floresta do Windows Server 2003, mas não há recursos adicionais disponíveis. 

### <a name="windows-server-2008-domain-functional-level-features"></a>Recursos de nível funcional de domínio do Windows Server 2008

* Todos os recursos de AD DS padrão, todos os recursos do nível funcional de domínio do Windows Server 2003 e os seguintes recursos estão disponíveis:
  * Suporte à replicação do Sistema de Arquivos Distribuído (DFS) para o volume do sistema do Windows Server 2003 (SYSVOL)
    * O suporte à Replicação DFS fornece replicação mais robusta e detalhada do conteúdo do SYSVOL.

      > [!NOTE]
      > A partir do Windows Server 2012 R2, o FRS (serviço de replicação de arquivo) foi preterido. Um novo domínio criado em um controlador de domínio que executa pelo menos o Windows Server 2012 R2 deve ser definido para o nível funcional do domínio do Windows Server 2008 ou superior.

  * Namespaces do DFS baseados em domínio em execução no modo Windows Server 2008, que inclui suporte para a enumeração baseada em acesso e maior escalabilidade. Os namespaces baseados em domínio no modo Windows Server 2008 também exigem que a floresta use o nível funcional de floresta do Windows Server 2003. Para obter mais informações, consulte [escolher um tipo de namespace](https://go.microsoft.com/fwlink/?LinkId=180400).
  * Suporte a criptografia AES (AES 128 e AES 256) para o protocolo Kerberos. Para que o TGTs seja emitido usando o AES, o nível funcional do domínio deve ser o Windows Server 2008 ou superior e a senha do domínio precisa ser alterada. 
    * Para obter mais informações, consulte [aprimoramentos do Kerberos](https://technet.microsoft.com/library/cc749438(ws.10).aspx).

      > [!NOTE]
      >Os erros de autenticação poderão ocorrer em um controlador de domínio depois que o nível funcional do domínio for gerado para o Windows Server 2008 ou superior se o controlador de domínio já tiver replicado a alteração DFL, mas ainda não tiver atualizado a senha krbtgt. Nesse caso, uma reinicialização do serviço KDC no controlador de domínio irá disparar uma atualização na memória da nova senha krbtgt e resolver os erros de autenticação relacionados.

  * [Último logon interativo](https://go.microsoft.com/fwlink/?LinkId=180387) Informações exibe as seguintes informações:
     * O número total de tentativas de logon com falha em um servidor Windows Server 2008 ingressado no domínio ou em uma estação de trabalho do Windows Vista
     * O número total de tentativas de logon com falha após um logon bem-sucedido para um servidor do Windows Server 2008 ou uma estação de trabalho do Windows Vista
     * A hora da última tentativa de logon com falha em uma estação de trabalho do Windows Server 2008 ou do Windows Vista
     * A hora da última tentativa de logon bem-sucedida em um servidor Windows Server 2008 ou em uma estação de trabalho do Windows Vista
  * As políticas de senha refinadas possibilitam que você especifique políticas de bloqueio de senha e conta para usuários e grupos de segurança globais em um domínio. Para obter mais informações, consulte [guia passo a passo para configuração de política de bloqueio de conta e senha refinada](https://go.microsoft.com/fwlink/?LinkID=91477).
  * Áreas de trabalho virtuais pessoais
     * Para usar a funcionalidade adicional fornecida pela guia área de trabalho virtual pessoal na caixa de diálogo Propriedades da conta de usuário em Active Directory usuários e computadores, seu esquema de AD DS deve ser estendido para o Windows Server 2008 R2 (versão do objeto de esquema = 47). Para obter mais informações, consulte [implantando áreas de trabalho virtuais pessoais usando conexão de RemoteApp e área de trabalho guia passo a passo](https://go.microsoft.com/fwlink/?LinkId=183552).

## <a name="windows-server-2003"></a>Windows Server 2003

Sistema operacional do controlador de domínio com suporte:

* Windows Server 2012 R2
* Windows Server 2012
* Windows Server 2008 R2
* Windows Server 2008
* Windows Server 2003

### <a name="windows-server-2003-forest-functional-level-features"></a>Recursos de nível funcional de floresta do Windows Server 2003

* Todos os recursos de AD DS padrão e os seguintes recursos estão disponíveis:
   * Relação de confiança de floresta
   * Renomeação de domínio
   * Replicação de valor vinculado
      - A replicação de valor vinculado possibilita alterar a associação de grupo para armazenar e replicar valores para membros individuais em vez de replicar toda a associação como uma única unidade. Armazenar e replicar os valores de membros individuais usa menos largura de banda de rede e menos ciclos de processador durante a replicação e impede que você perca atualizações ao adicionar ou remover vários membros simultaneamente em controladores de domínio diferentes.
   * A capacidade de implantar um RODC (controlador de domínio somente leitura)
   * Algoritmos e escalabilidade aprimorados do Knowledge Consistency Checker (KCC)
      - O ISTG (gerador de topologia entre sites) usa algoritmos aprimorados que são dimensionados para oferecer suporte a florestas com um número maior de sites do que AD DS pode dar suporte no nível funcional de floresta do Windows 2000. O algoritmo de eleição do ISTG aprimorado é um mecanismo menos intrusivo para escolher o ISTG no nível funcional da floresta do Windows 2000.
   * A capacidade de criar instâncias da classe auxiliar dinâmica chamada **DynamicObject** em uma partição de diretório de domínio
   * A capacidade de converter uma instância de objeto **inetOrgPerson** em uma instância de objeto de **usuário** e para concluir a conversão na direção oposta
   * A capacidade de criar instâncias de novos tipos de grupo para dar suporte à autorização baseada em função. 
      - Esses tipos são chamados de grupos básicos de aplicativos e grupos de consulta LDAP.
   * Desativação e redefinição de atributos e classes no esquema. Os seguintes atributos podem ser reutilizados: ldapDisplayName, schemaIdGuid, OID e mapiID.
   * Namespaces do DFS baseados em domínio em execução no modo Windows Server 2008, que inclui suporte para a enumeração baseada em acesso e maior escalabilidade. Para obter mais informações, consulte [escolher um tipo de namespace](https://go.microsoft.com/fwlink/?LinkId=180400).

### <a name="windows-server-2003-domain-functional-level-features"></a>Recursos de nível funcional de domínio do Windows Server 2003

* Todos os recursos de AD DS padrão, todos os recursos disponíveis no nível funcional de domínio nativo do Windows 2000 e os seguintes recursos estão disponíveis:
   * A ferramenta de gerenciamento de domínio, o Netdom. exe, que torna possível renomear controladores de domínio
   * Atualizações de carimbo de data/hora de logon
      * O atributo **lastLogonTimestamp** é atualizado com o último horário de logon do usuário ou computador. Este atributo é replicado no domínio.
   * A capacidade de definir o atributo **userPassword** como a senha efetiva nos objetos **inetOrgPerson** e User
   * A capacidade de redirecionar contêineres de usuários e computadores
      * Por padrão, dois contêineres conhecidos são fornecidos para hospedagem de computadores e contas de usuário,<domain root> ou seja, CN = Computers e CN = Users,.<domain root> Esse recurso permite a definição de um local novo e conhecido para essas contas.
   * A capacidade do Gerenciador de autorização de armazenar suas políticas de autorização no AD DS
   * Delegação restrita
      * A delegação restrita possibilita que os aplicativos tirem proveito da delegação segura de credenciais de usuário por meio da autenticação baseada em Kerberos.
      * Você pode restringir a delegação somente a serviços de destino específicos.
   * Autenticação seletiva
      * A autenticação seletiva possibilita que você especifique os usuários e grupos de uma floresta confiável que têm permissão para se autenticar em servidores de recursos em uma floresta confiável.

## <a name="windows-2000"></a>Windows 2000

Sistema operacional do controlador de domínio com suporte:

* Windows Server 2008 R2
* Windows Server 2008
* Windows Server 2003
* Windows 2000

### <a name="windows-2000-native-forest-functional-level-features"></a>Recursos de nível funcional de floresta nativa do Windows 2000

* Todos os recursos de AD DS padrão estão disponíveis.

### <a name="windows-2000-native-domain-functional-level-features"></a>Recursos de nível funcional de domínio nativo do Windows 2000

* Todos os recursos de AD DS padrão e os seguintes recursos de diretório estão disponíveis, incluindo:
   * Grupos universais para grupos de segurança e de distribuição.
   * Aninhamento de grupo
   * Conversão de grupo, que permite a conversão entre grupos de segurança e de distribuição
   * Histórico de SID (identificador de segurança)

## <a name="next-steps"></a>Próximas etapas

* [Aumentar o nível funcional do domínio](https://technet.microsoft.com/library/cc753104.aspx)  
* [Aumentar o nível funcional da floresta](https://technet.microsoft.com/library/cc730985.aspx)
