---
ms.assetid: 9a06cd41-426f-4cb9-89cf-f5be730e0b79
title: "O que & #39; s novo nos serviços de domínio do Active Directory"
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.service: 
ms.suite: na
ms.technology: active-directory-domain-services
ms.tgt_pltfrm: na
ms.topic: article
author: Femila
ms.author: billmath
ms.date: 05/31/2017
ms.openlocfilehash: 56716e2e0d633a19e9c6e02bc829874bbe863833
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="what39s-new-in-active-directory-domain-services"></a>O que & #39; s novo nos serviços de domínio do Active Directory 

>Aplica-se a: Windows Server 2016

Os seguintes novos recursos nos serviços de domínio do Active Directory (AD DS) melhoram a capacidade para as empresas protejam ambientes do Active Directory e ajudá-los a migrar para implantações somente na nuvem e implantações híbridas, onde alguns aplicativos e serviços são hospedados na nuvem e outros são hospedados no local. As melhorias incluem:  
  
-   [Gerenciamento de acesso privilegiado](https://technet.microsoft.com/library/mt150258.aspx   
)  
  
- [Estender as capacidades de nuvem para dispositivos Windows 10 por meio de participar do Azure Active Directory](https://azure.microsoft.com/en-us/documentation/articles/active-directory-azureadjoin-overview/)   
  
- [Conectando dispositivos ingressados em domínio no Azure AD para o Windows 10 experiências](https://azure.microsoft.com/en-us/documentation/articles/active-directory-azureadjoin-devices-group-policy/)   
  
- [Habilitar o Microsoft Passport for Work em sua organização](https://azure.microsoft.com/en-us/documentation/articles/active-directory-azureadjoin-passport-deployment/)    
  
-  [Depreciação dos níveis funcionais de serviço de replicação de arquivos (FRS) e Windows Server 2003](ad-ds/active-directory-functional-levels.md)  
  
  
## <a name="BKMK_PAM"></a>Gerenciamento de acesso privilegiado  
Gerenciamento de acesso privilegiado (PAM) ajuda a reduzir a segurança técnicas de roubo de credenciais de preocupações em ambientes do Active Directory que são causados por tais pass-the-hash, lança phishing e tipos de ataques semelhantes. Ele fornece uma nova solução de acesso administrativo que é configurada usando Microsoft Identity Manager (MIM). PAM apresenta:  
  
-   Bastião uma nova floresta do Active Directory, que é provisionada por MIM. Floresta bastião tem uma relação de confiança PAM especial com uma floresta existente. Ele fornece um novo ambiente do Active Directory que é conhecido por ser gratuito de qualquer atividade maliciosa e isolamento de uma floresta existente para o uso de contas privilegiadas.  
  
-   Novos processos em MIM para solicitar privilégios administrativos, juntamente com novos fluxos de trabalho com base na aprovação de solicitações.  
  
-   Nova sombra entidades de segurança (grupos) provisionados na floresta bastião por MIM em resposta a solicitações de privilégios administrativos. Os objetos de segurança de sombra tem um atributo que referencia o SID de um grupo administrativo em uma floresta existente. Isso permite que o grupo de sombra para acessar recursos em uma floresta existente sem alterar qualquer listas de controle de acesso (ACLs).  
  
-   Um recurso de links vencido, que permite a participação de tempo associado em um grupo de sombra. Um usuário pode ser adicionado ao grupo suficientes tempo necessário para executar uma tarefa administrativa. A associação ao tempo associado é expresso por um valor de (vida útil TTL) tempo de vida é propagado para um ciclo de vida de tíquete Kerberos.  
  
    > [!NOTE]  
    > Links vencidos estão disponíveis em todos os atributos vinculados. Mas o atributo vinculado membro/membro relação entre um grupo e um usuário é o exemplo somente onde uma solução completa como PAM é pré-configurado para usar o recurso de links vencido.  
  
-   Aprimoramentos de KDC são feitos em controladores de domínio do Active Directory para restringir o tempo de vida de tíquete Kerberos para o valor mais baixo possível tempo de vida (TTL) em casos em que um usuário tem várias associações de tempo limite em grupos administrativos. Por exemplo, se forem adicionados a um grupo de tempo associado A, em seguida, quando você fizer logon, a vida útil de tíquete (TGT) de concessão de tíquete Kerberos é igual ao tempo de ainda resta na grupo. Se você também é um membro do grupo de tempo associado outro B, que tem um TTL menor do que um grupo, em seguida, o tempo de vida do TGT é igual para o tempo que restante no grupo B.  
  
-   Novos recursos de monitoramento para ajudá-lo facilmente identificam quem solicitou acesso, que o acesso foi concedido e quais atividades foram executadas.  
  
**Requisitos**  
  
-   Microsoft Identity Manager  
  
-   Nível funcional do Windows Server 2012 R2 ou superior de floresta do Active Directory.  
  
## <a name="BKMK_AzureADJoin"></a>Ingresso do Azure AD  
Participe de diretório Active Azure aprimora a experiências de identidade para empresas, empresas e EDU clientes - com recursos aperfeiçoados para dispositivos pessoais e corporativos.  
  
Benefícios:  
  
-   **A disponibilidade das configurações modernos** em dispositivos Windows corp. Serviços de oxigênio não mais exigem uma conta pessoal da Microsoft: eles são executados agora desativar contas de trabalho existentes dos usuários para garantir a conformidade. Serviços de oxigênio funcionará em computadores que fazem parte de um domínio do Windows no local, e computadores e dispositivos que são "ingressados" em seu locatário do Azure AD ("domínio de nuvem"). Essas configurações incluem:  
  
    -   Roaming ou personalização, configurações de acessibilidade e credenciais  
  
    -   Backup e restauração  
  
    -   Acesso a Microsoft Store com a conta corporativa  
  
    -   Blocos dinâmicos e notificações  
  
-   **Acessar recursos organizacionais** em dispositivos móveis (telefones, phablets) que não não possível ingressar em um domínio do Windows, sejam eles pertencem corp ou BYOD  
  
-   **Logon único em** para Office 365 e outros aplicativos organizacionais, sites e recursos.  
  
-   **Em dispositivos BYOD**, adicionar uma conta corporativa (de um domínio local ou Azure AD) para um dispositivo pessoais e aproveite o SSO para recursos de trabalho, por meio de aplicativos e na web, de forma que ajuda a garante a conformidade com os novos recursos, como Atestado condicional controle de conta e a integridade do dispositivo.  
  
-   **Integração com o MDM** permite o registro automático de dispositivos para o MDM (Intune ou terceiros)  
  
-   **Configurar o modo de "quiosque" e dispositivos compartilhados** para vários usuários em sua organização  
  
-   **Experiência do desenvolvedor** permite que você crie aplicativos que atendem às corporativos e pessoais contextos com uma pilha de programação compartilhado.  
  
-   **Geração de imagens** opção permite que você escolha entre imagens e permitir que os usuários configurem dispositivos pertencentes aos corp diretamente durante a experiência de primeira execução.  
  
Para obter mais informações, consulte [Windows 10 para a empresa: maneiras de usar dispositivos para trabalhar](https://azure.microsoft.com/en-us/documentation/articles/active-directory-azureadjoin-windows10-devices-overview/?rnd=1).  
  
## <a name="BKMK_IDLocker"></a>O Microsoft Passport  
O Microsoft Passport é uma nova autenticação baseada em teclas abordar os consumidores, que vai além de senhas e organizações. Essa forma de autenticação depende de violação, roubo e credenciais de phishing resistente.  
  
O usuário faz logon no dispositivo com uma PIN ou biometria informações de logon que são vinculadas a um certificado ou um par de chaves assimétrica. Os provedores de identidade (IDPs) validar o usuário por meio do mapeamento a chave pública do usuário para IDLocker e fornece o log de informações por meio de um tempo de senha (OTP), Phonefactor ou um mecanismo de notificação diferentes.  
  
Para obter mais informações, consulte [autenticar identidades sem senhas por meio do Microsoft Passport](https://azure.microsoft.com/en-us/documentation/articles/active-directory-azureadjoin-passport/)  
  
## <a name="BKMK_FRSDeprecation"></a>Depreciação dos níveis funcionais de serviço de replicação de arquivos (FRS) e Windows Server 2003  
Embora o serviço de replicação de arquivos (FRS) e os níveis funcionais do Windows Server 2003 foram preteridos em versões anteriores do Windows Server, é importante lembrar que o sistema operacional Windows Server 2003 não é mais compatível. Como resultado, qualquer controlador de domínio que executa o Windows Server 2003 deve ser removido do domínio. O nível funcional do domínio e da floresta deve ser aumentado para pelo menos Windows Server 2008 para impedir que um controlador de domínio que executa uma versão anterior do Windows Server sejam adicionados ao ambiente.  
  
No Windows Server 2008 e maiores níveis funcionais de domínio, replicação do serviço de arquivos distribuído (DFS) é usada para replicar o conteúdo da pasta SYSVOL entre controladores de domínio. Se você criar um novo domínio no nível funcional de domínio do Windows Server 2008 ou superior, replicação DFS automaticamente é usada para replicar SYSVOL. Se você criou no domínio em um nível funcional inferior, você precisará migrar usando FRS para replicação do DFS para SYSVOL. Para obter as etapas de migração, você pode qualquer siga o [procedimentos no TechNet](https://technet.microsoft.com/library/dd640019(v=WS.10).aspx) ou você pode consultar o [simplificada conjunto de etapas no blog da equipe de armazenamento arquivo de gabinete](http://blogs.technet.com/b/filecab/archive/2014/06/25/streamlined-migration-of-frs-to-dfsr-sysvol.aspx).  
  
Os Windows Server 2003 floresta e domínio níveis funcionais continuam a ter suporte, mas as organizações devem aumentar o nível funcional para o Windows Server 2008 (ou superior, se possível) para garantir a compatibilidade de replicação SYSVOL e suporte no futuro. Além disso, há muitos outros benefícios e os recursos disponíveis em níveis funcionais mais altos maior. Consulte os seguintes recursos para obter mais informações:  
  
-   [Noções básicas sobre o domínio do Active Directory Services níveis funcionais (AD DS)](ad-ds/active-directory-functional-levels.md)  
  
-   [Aumentar o nível funcional do domínio](https://technet.microsoft.com/library/cc753104.aspx)  
  
-   [Aumentar o nível funcional da floresta](https://technet.microsoft.com/library/cc730985.aspx)  
  
