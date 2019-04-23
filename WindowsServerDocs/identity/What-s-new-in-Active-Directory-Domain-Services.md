---
ms.assetid: 9a06cd41-426f-4cb9-89cf-f5be730e0b79
title: O que&#39;s novo nos serviços de domínio do Active Directory
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.service: ''
ms.suite: na
ms.technology: active-directory-domain-services
ms.tgt_pltfrm: na
ms.topic: article
author: Femila
ms.author: billmath
ms.date: 05/31/2017
ms.openlocfilehash: ffa8bcb43b17ae8779c70d499bff27a8f77cce75
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841327"
---
# <a name="what39s-new-in-active-directory-domain-services"></a>O que&#39;s novo nos serviços de domínio do Active Directory 

>Aplica-se a: Windows Server 2016

Os seguintes novos recursos nos serviços de domínio Active Directory (AD DS) melhoram a capacidade para as organizações a proteger ambientes do Active Directory e ajuda a migrar para implantações somente em nuvem e implantações híbridas, onde alguns aplicativos e serviços são hospedado na nuvem e outros são hospedados no local. As melhorias incluem:  
  
-   [Gerenciamento de acesso privilegiado](https://technet.microsoft.com/library/mt150258.aspx   
)  
  
- [Estendendo os recursos de nuvem para dispositivos Windows 10 por meio do ingresso do Active Directory do Azure](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-overview/)   
  
- [Experiências de conectar dispositivos ingressados no domínio ao AD do Azure para Windows 10](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-devices-group-policy/)   
  
- [Habilitar o Microsoft Passport for Work em sua organização](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-passport-deployment/)    
  
-  [Substituição dos níveis funcionais de replicação FRS (serviço) e Windows Server 2003](ad-ds/active-directory-functional-levels.md)  
  
  
## <a name="BKMK_PAM"></a>Gerenciamento de acesso privilegiado  
Gerenciamento de acesso privilegiado (PAM) ajuda a reduzir a segurança preocupações para os ambientes do Active Directory que são causados por tais pass-the-hash, spear phishing e tipos semelhantes de ataques de técnicas de roubo de credenciais. Ele fornece uma nova solução de acesso administrativo, o que é configurada usando o Microsoft Identity Manager (MIM). Apresenta o PAM:  
  
-   Um novo bastiões floresta do Active Directory, o que é provisionado por MIM. A floresta de bastiões tem uma relação de confiança do PAM especial com uma floresta existente. Ele fornece um novo ambiente do Active Directory que é conhecido esteja livre de qualquer atividade mal-intencionada e isolamento de uma floresta existente para o uso de contas privilegiadas.  
  
-   Novos processos em MIM para solicitar privilégios administrativos, junto com novos fluxos de trabalho com base na aprovação de solicitações.  
  
-   Novo sombra entidades de segurança (grupos) que são provisionadas na floresta de bastiões por MIM em resposta a solicitações de privilégio administrativo. As entidades de segurança de sombra tem um atributo que referencia o SID de um grupo administrativo em uma floresta existente. Isso permite que o grupo de sombras para acessar os recursos em uma floresta existente sem alterar quaisquer listas de controle de acesso (ACLs).  
  
-   Um recurso de links expirando, que permite a associação de limite de tempo em um grupo de sombra. Um usuário pode ser adicionado ao grupo para o tempo necessário para executar uma tarefa administrativa. A associação de limite de tempo é expresso por um valor time-to-live (TTL) que é propagado para um tempo de vida do tíquete Kerberos.  
  
    > [!NOTE]  
    > Expirando links estão disponíveis em todos os atributos vinculados. Mas o atributo de membro/memberOf vinculado a relação entre um grupo e um usuário é o exemplo de somente onde uma solução completa, como o PAM é pré-configurado para usar o recurso de links prestes a expirar.  
  
-   Aprimoramentos do KDC são feitos em controladores de domínio do Active Directory para restringir a vida útil do tíquete Kerberos para o valor mais baixo possível time-to-live (TTL) em casos em que um usuário tem várias associações de limite de tempo em grupos administrativos. Por exemplo, se você for adicionado A um grupo de limite de tempo, em seguida, quando você faz logon, a vida útil tíquete (TGT) de concessão de tíquete Kerberos é igual ao tempo ter restantes na grupo. Se você também for um membro de outro grupo de limite de tempo B, que tem uma TTL menor que um grupo, em seguida, o tempo de vida TGT é igual ao tempo que restante no grupo B.  
  
-   Novos recursos de monitoramento para ajudá-lo facilmente identificam quem solicitou acesso, o que o acesso foi concedido e quais atividades foram executadas.  
  
**Requisitos**  
  
-   Microsoft Identity Manager  
  
-   Nível funcional do Windows Server 2012 R2 ou superior de floresta do Active Directory.  
  
## <a name="BKMK_AzureADJoin"></a>Ingresso no Azure AD  
Ingresso no Active Directory do Azure aprimora a experiências de identidade para enterprise, business e EDU clientes – com recursos aprimorados para dispositivos corporativos e pessoais.  
  
Benefícios:  
  
-   **Disponibilidade das configurações modernos** em dispositivos Windows corp. Serviços de oxigênio não exigem uma conta pessoal da Microsoft: eles agora executarem desativar contas de trabalho existentes dos usuários para garantir a conformidade. Serviços de oxigênio funcionará em computadores que ingressaram em um domínio do Windows no local, e PCs e dispositivos que são "ingressar" em seu locatário do AD do Azure ("domínio de nuvem"). Essas configurações incluem:  
  
    -   Roaming ou personalização, configurações de acessibilidade e credenciais  
  
    -   Backup e restauração  
  
    -   Acesso ao Microsoft Store com conta de trabalho  
  
    -   Notificações e blocos dinâmicos  
  
-   **Acessar recursos organizacionais** em dispositivos móveis (celulares, phablets) que não podem ser unidos a um domínio do Windows, independentemente de estarem corporativo ou de BYOD  
  
-   **Logon único** para Office 365 e outros aplicativos, sites e recursos organizacionais.  
  
-   **Em dispositivos BYOD**, adicione uma conta de trabalho (de um domínio local ou do Azure AD) para um dispositivo pessoal e aproveitam o SSO para recursos de trabalho, por meio de aplicativos e na web, de forma que ajuda a garantir a conformidade com os novos recursos, como a conta condicional Atestado de integridade do dispositivo e de controle.  
  
-   **Integração do MDM** lhe permite registrar automaticamente dispositivos para o MDM (Intune ou terceiros)  
  
-   **Configurar o modo de "quiosque" e dispositivos compartilhados** para vários usuários em sua organização  
  
-   **Experiência do desenvolvedor** permite que você crie aplicativos que atenda às necessidades empresariais e contextos pessoais com uma pilha de programação compartilhado.  
  
-   **Geração de imagens** opção permite que você escolha entre a geração de imagens e permitir que os usuários para configurar dispositivos de propriedade corp diretamente durante a experiência de primeira execução.  
  
Para obter mais informações, consulte [Windows 10 para a empresa: Maneiras de usar dispositivos para trabalho](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-windows10-devices-overview/?rnd=1).  
  
## <a name="BKMK_IDLocker"></a>O Microsoft Passport  
O Microsoft Passport é uma nova autenticação baseada em chave abordar a empresas e consumidores, que vai além de senhas. Essa forma de autenticação se baseia em violação, roubo e credenciais pescam resistente.  
  
O usuário faz logon no dispositivo com uma biometria ou PIN informações de logon que esteja vinculadas a um certificado ou um par de chaves assimétrica. Os provedores de identidade (IDPs) validar o usuário mapeando a chave pública do usuário para IDLocker e fornece log de informações por meio de um tempo de OTP (senha), Phonefactor ou um mecanismo de notificação diferente.  
  
Para obter mais informações, consulte [autenticando identidades sem senhas por meio do Microsoft Passport](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-passport/)  
  
## <a name="BKMK_FRSDeprecation"></a>Substituição dos níveis funcionais de replicação FRS (serviço) e Windows Server 2003  
Embora a replicação FRS (serviço) e os níveis funcionais do Windows Server 2003 foram preteridos em versões anteriores do Windows Server, é importante lembrar a que o sistema operacional Windows Server 2003 não é mais suportado. Como resultado, qualquer controlador de domínio que executa o Windows Server 2003 deve ser removido do domínio. O nível funcional de domínio e floresta deve ser aumentado para pelo menos Windows Server 2008 para impedir que um controlador de domínio que executa uma versão anterior do Windows Server do que está sendo adicionado ao ambiente.  
  
No Windows Server 2008 e superiores níveis funcionais de domínio, replicação do serviço de arquivos distribuído (DFS) é usada para replicar o conteúdo da pasta SYSVOL entre controladores de domínio. Se você criar um novo domínio no nível funcional de domínio do Windows Server 2008 ou superior, a replicação DFS é automaticamente usada para replicar o SYSVOL. Se você criou o domínio em um nível funcional mais baixo, você precisará migrar do uso de FRS para replicação do DFS do SYSVOL. Para obter as etapas de migração, você pode a seguir a [procedimentos no TechNet](https://technet.microsoft.com/library/dd640019(v=WS.10).aspx) ou você pode consultar a [simplificada do conjunto de etapas no blog do gabinete do arquivo de equipe de armazenamento](http://blogs.technet.com/b/filecab/archive/2014/06/25/streamlined-migration-of-frs-to-dfsr-sysvol.aspx).  
  
Os Windows Server 2003 domínios e florestas níveis funcionais de continuam a ter suporte, mas as organizações devem aumentar o nível funcional para o Windows Server 2008 (ou superior, se possível) para garantir a compatibilidade de replicação do SYSVOL e suporte no futuro. Além disso, há muitos outros benefícios e recursos disponíveis em níveis mais altos funcionais superiores. Confira os recursos a seguir para saber mais:  
  
-   [Noções básicas sobre o domínio do Active Directory (AD DS) os níveis funcionais de serviços](ad-ds/active-directory-functional-levels.md)  
  
-   [Aumentar o nível funcional do domínio](https://technet.microsoft.com/library/cc753104.aspx)  
  
-   [Aumentar o nível funcional de floresta](https://technet.microsoft.com/library/cc730985.aspx)  
  
