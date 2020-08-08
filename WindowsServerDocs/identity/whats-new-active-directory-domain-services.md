---
ms.assetid: 6a852428-c1ec-4703-b3b3-a4bfdf8cbb9d
title: O que há de novo no Active Directory Domain Services no Windows Server 2016
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/07/2018
ms.topic: article
ms.openlocfilehash: 666b05d4a08ad72a62bffec05051ceed2155c36f
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87956023"
---
# <a name="whats-new-in-active-directory-domain-services-for-windows-server-2016"></a>Novidades no Active Directory Domain Services para Windows Server 2016

>Aplica-se a: Windows Server 2016

Os novos recursos a seguir no Active Directory Domain Services (AD DS) melhoram a capacidade das organizações de protegerem os ambientes de Active Directory e os ajudam a migrar para implantações híbridas e somente na nuvem, em que alguns aplicativos e serviços são hospedados na nuvem e outros são hospedados localmente. As melhorias incluem:

- [Privileged Access Management](/microsoft-identity-manager/pam/privileged-identity-management-for-active-directory-domain-services)

- [Estendendo recursos de nuvem para dispositivos Windows 10 por meio do Azure Active Directory Join](/azure/active-directory/devices/overview)

- [Conectando dispositivos ingressados no domínio ao Azure AD para experiências com o Windows 10](/azure/active-directory/devices/hybrid-azuread-join-plan)

- [Habilitar o Microsoft Passport for Work em sua organização](/windows/security/identity-protection/hello-for-business/hello-identity-verification)

- [Substituição do FRS (serviço de replicação de arquivo) e dos níveis funcionais do Windows Server 2003](ad-ds/active-directory-functional-levels.md)

## <a name="privileged-access-management"></a>Privileged Access Management

O PAM (Privileged Access Management) ajuda a reduzir as preocupações de segurança de Active Directory ambientes causados por técnicas de roubo de credenciais, como Pass-the-hash, spear phishing e tipos de ataques semelhantes. Ele fornece uma nova solução de acesso administrativo que é configurada usando o Microsoft Identity Manager (MIM). O PAM apresenta:

- Uma nova floresta Active Directory de bastiões, que é provisionada pelo MIM. A floresta de bastiões tem uma relação de confiança especial do PAM com uma floresta existente. Ele fornece um novo ambiente de Active Directory que é conhecido como livre de qualquer atividade mal-intencionada e isolamento de uma floresta existente para o uso de contas com privilégios.

- Novos processos no MIM para solicitar privilégios administrativos, juntamente com novos fluxos de trabalho com base na aprovação de solicitações.

- Novas entidades de segurança de sombra (grupos) que são provisionadas na floresta de bastiões pelo MIM em resposta a solicitações de privilégio administrativo. As entidades de segurança de sombra têm um atributo que faz referência ao SID de um grupo administrativo em uma floresta existente. Isso permite que o grupo de sombra acesse recursos em uma floresta existente sem alterar nenhuma lista de controle de acesso (ACLs).

- Um recurso de links de expiração, que habilita a associação de limite de tempo em um grupo de sombra. Um usuário pode ser adicionado ao grupo por apenas tempo suficiente necessário para executar uma tarefa administrativa. A associação de limite de tempo é expressa por um valor de vida útil (TTL) que é propagado para um tempo de vida de tíquete Kerberos.

    > [!NOTE]
    > Os links de expiração estão disponíveis em todos os atributos vinculados. Mas a relação de atributo vinculado member/memberOf entre um grupo e um usuário é o único exemplo em que uma solução completa, como o PAM, é pré-configurada para usar o recurso de links de expiração.

- Os aprimoramentos do KDC são internos para Active Directory controladores de domínio para restringir o tempo de vida do tíquete Kerberos para o valor de TTL (vida útil) mais baixo nos casos em que um usuário tem várias associações de limite de tempo em grupos administrativos. Por exemplo, se você for adicionado a um grupo de limite de tempo A e, quando fizer logon, o tempo de vida do tíquete de concessão de tíquete (TGT) do Kerberos será igual ao horário restante no grupo A. Se você também for um membro de outro grupo de limite de tempo B, que tem um TTL menor do que o grupo A, o tempo de vida de TGT será igual ao horário restante no grupo B.

- Novos recursos de monitoramento para ajudá-lo a identificar facilmente quem solicitou acesso, qual acesso foi concedido e quais atividades foram executadas.

### <a name="requirements-for-privileged-access-management"></a>Requisitos para o Privileged Access Management

- Gerenciador de Identidades da Microsoft

- Active Directory nível funcional de floresta do Windows Server 2012 R2 ou superior.

## <a name="azure-ad-join"></a>Ingresso no AD do Azure

O Azure Active Directory Join aprimora as experiências de identidade para clientes corporativos, empresariais e EDU-com recursos aprimorados para dispositivos corporativos e pessoais.

Benefícios:

- **Disponibilidade de configurações modernas** em dispositivos Windows de propriedade corporativa. Os serviços oxigênios não exigem mais um conta Microsoft pessoal: agora eles executam as contas de trabalho existentes dos usuários para garantir a conformidade. Os serviços oxigênios funcionarão em computadores que ingressaram em um domínio local do Windows, e computadores e dispositivos que estão "Unidos" ao seu locatário do Azure AD ("domínio de nuvem"). Essas configurações incluem:

   - Roaming ou personalização, configurações de acessibilidade e credenciais
   - Backup e restauração
   - Acesso a Microsoft Store com a conta corporativa
   - Blocos dinâmicos e notificações

- **Acesse recursos organizacionais** em dispositivos móveis (telefones, phablets) que não podem ser adicionados a um domínio do Windows, sejam de propriedade corporativa ou BYOD
- **Logon único no** Office 365 e em outros aplicativos, sites e recursos organizacionais.
- **Em dispositivos BYOD**, adicione uma conta de trabalho (de um domínio local ou do Azure AD) a um dispositivo de propriedade pessoal e aproveite o SSO para trabalhar com recursos, por meio de aplicativos e na Web, de uma forma que ajude a garantir a conformidade com novos recursos, como controle de conta condicional e atestado de integridade do dispositivo.
- A **integração de MDM** permite que você registre automaticamente os dispositivos no MDM (Intune ou de terceiros)
- **Configurar o modo "quiosque" e os dispositivos compartilhados** para vários usuários em sua organização
- A **experiência do desenvolvedor** permite que você crie aplicativos que atendem a contextos corporativos e pessoais com uma pilha de programa compartilhada.
- A opção de **geração de imagens** permite escolher entre imagens e permitir que os usuários configurem dispositivos corporativos diretamente durante a experiência de primeira execução.

Para obter mais informações, consulte [introdução ao gerenciamento de dispositivos no Azure Active Directory](/azure/active-directory/devices/overview).

## <a name="windows-hello-for-business"></a>Windows Hello for Business

O Windows Hello para empresas é uma abordagem de autenticação baseada em chave que as organizações e os consumidores, que vão além das senhas. Essa forma de autenticação depende de violações, roubos e credenciais resistentes a Phish.

O usuário faz logon no dispositivo com um registro biométrico ou PIN em informações vinculadas a um certificado ou a um par de chaves assimétricas. Os provedores de identidade (IDPs) validam o usuário mapeando a chave pública do usuário para IDLocker e fornecem informações de logon por meio de senha de uma vez (OTP), telefone ou outro mecanismo de notificação.

Para obter mais informações, consulte [Windows Hello for Business](/windows/security/identity-protection/hello-for-business/hello-identity-verification)

## <a name="deprecation-of-file-replication-service-frs-and-windows-server-2003-functional-levels"></a>Substituição do FRS (serviço de replicação de arquivo) e dos níveis funcionais do Windows Server 2003

Embora o FRS (serviço de replicação de arquivo) e os níveis funcionais do Windows Server 2003 tenham sido preteridos em versões anteriores do Windows Server, ele deixa repetindo que o sistema operacional Windows Server 2003 não é mais suportado. Como resultado, qualquer controlador de domínio que execute o Windows Server 2003 deve ser removido do domínio. O nível funcional de domínio e floresta deve ser elevado ao mínimo do Windows Server 2008 para impedir que um controlador de domínio que executa uma versão anterior do Windows Server seja adicionado ao ambiente.

Nos níveis funcionais de domínio do Windows Server 2008 e superior, a replicação do DFS (serviço de arquivos distribuído) é usada para replicar o conteúdo da pasta SYSVOL entre controladores de domínio. Se você criar um novo domínio no nível funcional de domínio do Windows Server 2008 ou superior, a Replicação do DFS será usada automaticamente para replicar o SYSVOL. Se você criou o domínio em um nível funcional inferior, será necessário migrar do uso do FRS para a Replicação do DFS para o SYSVOL. Para obter as etapas de migração, você pode seguir [estas etapas](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/dd640019\(v=ws.10\)) ou pode consultar o [conjunto simplificado de etapas no blog do gabinete de arquivo da equipe de armazenamento](https://techcommunity.microsoft.com/t5/storage-at-microsoft/bg-p/FileCAB).

Os níveis funcionais de domínio e floresta do Windows Server 2003 continuam com suporte, mas as organizações devem aumentar o nível funcional para o Windows Server 2008 (ou superior, se possível) para garantir a compatibilidade e o suporte à replicação do SYSVOL no futuro. Além disso, há muitos outros benefícios e recursos disponíveis nos níveis funcionais mais altos. Confira os recursos a seguir para saber mais:

- [Compreender o AD DS (Serviços de Domínio Active Directory)](ad-ds/active-directory-functional-levels.md)
- [Aumentar o nível funcional do domínio](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc753104\(v=ws.11\))
- [Aumentar o nível funcional da floresta](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc730985\(v=ws.11\))
