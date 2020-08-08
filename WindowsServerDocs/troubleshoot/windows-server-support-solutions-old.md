---
title: Principais soluções de suporte do Windows Server
description: Obter links para soluções de problemas do Windows Server
manager: alant
ms.date: 03/16/2018
ms.topic: article
author: kaushika-msft
ms.author: elizapo
ms.openlocfilehash: ba9654fd60534e53211d65cfd2006320cea20383
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87997277"
---
# <a name="top-support-solutions-for-windows-server-2016"></a>Principais soluções de suporte do Windows Server 2016

A Microsoft lança regularmente atualizações e soluções para o Windows Server. Para garantir que os servidores podem receber atualizações futuras, incluindo atualizações de segurança, é importante mantê-los atualizados. Confira [Histórico de atualizações do Windows 10 e Windows Server 2016](https://support.microsoft.com/help/4000825/windows-10-windows-server-2016-update-history) para obter uma lista completa das atualizações lançadas.

Essas são as principais soluções do Suporte da Microsoft para a maioria dos problemas comuns enfrentados ao usar o Windows Server 2016. Os links a seguir incluem links para artigos da KB, atualizações e artigos de biblioteca.

>[!TIP]
> Está procurando informações sobre versões anteriores do Windows Server? Confira as outras [bibliotecas do Windows Server](/previous-versions/windows/) em docs.microsoft.com. Você também pode [pesquisar informações específicas neste site](/search/index?dataSource=previousVersions&search=Windows+Server).

## <a name="solutions-for-installing-or-upgrading-windows-server"></a>Soluções para instalar ou atualizar o Windows Server

- [Resolver erros de upgrade do Windows 10: informações técnicas para profissionais de TI](/windows/deployment/upgrade/resolve-windows-10-upgrade-errors)
- [Atualização da pilha de manutenção para Windows 10 versão 1607 e Windows Server 2016: 8 de agosto de 2017](https://support.microsoft.com/help/4035631)
- [Atualização de compatibilidade a fim de atualizar para Windows 10 versão 1607 e Windows Server 2016: 3 de agosto de 2017](https://support.microsoft.com/help/4033524)
- [Não há suporte para uma upgrade in-loco do sistema com base em VMs do Azure com base no Windows](https://support.microsoft.com/help/4014997)
- [Opções de atualização e conversão para o Windows Server 2016](../get-started/supported-upgrade-paths.md)
- [Matriz de atualização e migração da função de servidor para Windows Server 2016](../get-started/server-role-upgradeability-table.md)
- [Instalação e atualização do Windows Server](../get-started/installation-and-upgrade.md)
- [Notas de versão: Problemas importantes no Windows Server 2016](../get-started/windows-server-2016-ga-release-notes.md)
- [Recomendações para mudar para o Windows Server 2016](../get-started/recommendations-moving-to-server2016.md)

## <a name="solutions-for-volume-activation"></a>Soluções de ativação de volume
- [Ativação do Windows Server 2016](../get-started/server-2016-activation.md)
- [Revisar e Selecionar Métodos de Ativação](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj134256(v=ws.11))
- [Códigos de Erro de Ativação para Ativação de Volume](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn502528(v=ws.11))
- [Como solucionar problemas do Serviço de Gerenciamento de Chaves (KMS)](/previous-versions/tn-archive/ee939272(v=technet.10))
- [Solução de problemas de ativação do volume](/previous-versions/tn-archive/ff793439(v=technet.10))
- [Códigos de Erro de Ativação](/previous-versions/ff793399(v=technet.10))
- [A instalação do Windows pode falhar com o erro "a chave do produto inserida não corresponde a nenhuma das imagens do Windows disponíveis para instalação. Insira uma chave de produto diferente "](https://support.microsoft.com/help/2796988/windows-8-or-windows-server-2012-installation-may-fail-with-error-mess)

## <a name="solutions-related-to-dcpromo-and-installing-domain-controllers"></a>Soluções relacionados à DCPromo e à instalação de controladores de domínio
- [Requisitos de porta do Active Directory e do Active Directory Domain Services](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd772723(v=ws.10))
- [Active Directory portas de firewall – vamos tentar tornar isso simples](http://blogs.msmvps.com/acefekay/2011/11/01/active-directory-firewall-ports-let-s-try-to-make-this-simple/)
- [Suporte ao Exchange Server para Windows Server 2016](/Exchange/plan-and-deploy/supportability-matrix?view=exchserver-2019)
- [Uso de Ntdsutil.exe para transferir ou executar funções FSMO de um controlador de domínio.](https://support.microsoft.com/kb/255504)
- [Solução de problemas de implantação de controlador de domínio](../identity/ad-ds/deploy/troubleshooting-domain-controller-deployment.md)
- [Solução de problemas do Assistente de instalação do Active Directory](/previous-versions/windows/it-pro/windows-2000-server/bb727058(v=technet.10))
- [Problemas conhecidos relacionados à instalação e remoção de AD DS](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754463(v=ws.10))

## <a name="solutions-for-active-directory-federation-services-ad-fs"></a>Soluções para Serviços de Federação do Active Directory (AD FS)
- [Como configurar o registro automático de dispositivos ingressados no domínio do Windows com o Azure Active Directory](/azure/active-directory/active-directory-conditional-access-automatic-device-registration-setup)
- [Configurar a emissão de declarações](/azure/active-directory/device-management-hybrid-azuread-joined-devices-setup#step-2-setup-issuance-of-claims)
- [Configurar o AD FS para autenticar usuários armazenados nos diretórios do LDAP](../identity/ad-fs/operations/configure-ad-fs-to-authenticate-users-stored-in-ldap-directories.md)
- [Suporte do AD FS para associação de nome de host alternativo para autenticação de certificado](../identity/ad-fs/operations/ad-fs-support-for-alternate-hostname-binding-for-certificate-authentication.md)
- [Proteger contra ataques de senha](/archive/blogs/tspring/federated-to-microsoft-cloud-and-account-lockouts)
- [Como atualizar para o AD FS no Windows Server 2016 por meio de um banco de dados WID](../identity/ad-fs/deployment/upgrading-to-ad-fs-in-windows-server.md)
- [Logon do Windows 10 – habilitar a autenticação de dispositivo com o AD FS](../identity/ad-fs/operations/configure-device-based-conditional-access-on-premises.md)
- [Gerenciamento de certificados SSL no AD FS e no Windows Server 2016](../identity/ad-fs/operations/manage-ssl-certificates-ad-fs-wap.md)
- [Políticas de controle de acesso no AD FS para Windows Server 2016](../identity/ad-fs/operations/access-control-policies-in-ad-fs.md)

## <a name="solutions-related-to-active-directory-replication"></a>Soluções relacionadas à replicação do Active Directory

- [Como solucionar problemas de replicação do Active Directory](../identity/ad-ds/manage/troubleshoot/troubleshooting-active-directory-replication-problems.md)
- [Baixar a ferramenta de Status de Replicação do Active Directory do Centro de Download da Microsoft](https://www.microsoft.com/en-in/download/details.aspx?id=30005)
- [e2e: como solucionar erros comuns de replicação do Active Directory](https://support.microsoft.com/kb/3108513)
- [Solução de problemas do AD Erro de replicação 8606: não foram fornecidos os atributos suficientes para criar um objeto](https://support.microsoft.com/kb/2028495)
- [As IDs de evento 2108 e 1084 ocorrem durante a duplicação de entrada do Active Directory no Windows 2000 Server e no Windows Server 2003](https://support.microsoft.com/kb/837932)
- [Solução de problemas do AD Erro de replicação 8451: a operação de replicação encontrou um erro de banco de dados](https://support.microsoft.com/kb/2645996)
- [Solução de problemas do AD Erro de replicação 1127: ao acessar o disco rígido, uma operação de disco falhou mesmo após várias tentativas](https://support.microsoft.com/kb/2025726)
- [Limpar os metadados do servidor](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc816907(v=ws.10))