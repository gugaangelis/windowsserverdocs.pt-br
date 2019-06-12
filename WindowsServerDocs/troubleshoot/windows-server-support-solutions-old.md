---
title: Principais soluções de suporte do Windows Server
description: Obter links para soluções de problemas do Windows Server
ms.prod: windows-server-threshold
ms.service: na
manager: alant
ms.technology: server-general
ms.date: 03/16/2018
ms.topic: article
author: kaushika-msft
ms.author: elizapo
ms.openlocfilehash: 1eb52f28fcd5afe62df33cd56208f2ab506e7194
ms.sourcegitcommit: 48bb3e5c179dc520fa879b16c9afe09e07c87629
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66453153"
---
# <a name="top-support-solutions-for-windows-server-2016"></a>Principais soluções de suporte do Windows Server 2016

A Microsoft lança regularmente atualizações e soluções para o Windows Server. Para garantir que os servidores podem receber atualizações futuras, incluindo atualizações de segurança, é importante mantê-los atualizados. Confira [Histórico de atualizações do Windows 10 e Windows Server 2016](https://support.microsoft.com/en-us/help/4000825/windows-10-windows-server-2016-update-history) para obter uma lista completa das atualizações lançadas.

Essas são as principais soluções do Suporte da Microsoft para a maioria dos problemas comuns enfrentados ao usar o Windows Server 2016. Os links a seguir incluem links para artigos da KB, atualizações e artigos de biblioteca.

>[!TIP]
> Procurando informações sobre versões anteriores do Windows Server? Confira as outras [bibliotecas do Windows Server](/previous-versions/windows/) em docs.microsoft.com. Você também pode [pesquisar neste site](https://docs.microsoft.com/search/index?search=Windows+Server&dataSource=previousVersions) para obter informações específicas.

## <a name="solutions-for-installing-or-upgrading-windows-server"></a>Soluções para instalar ou atualizar o Windows Server

- [Resolva erros de atualização do Windows 10: Informações técnicas para profissionais de TI](https://docs.microsoft.com/windows/deployment/upgrade/resolve-windows-10-upgrade-errors)
- [Atualização da pilha para Windows 10 versão 1607 e Windows Server 2016 de manutenção: 8 de agosto de 2017](https://support.microsoft.com/en-US/help/4035631)
- [Atualização de compatibilidade de atualização para Windows 10 versão 1607 e Windows Server 2016: 3 de agosto de 2017](https://support.microsoft.com/en-US/help/4033524)
- [Não há suporte para uma atualização do sistema no local em VMs do Azure com base em Windows](https://support.microsoft.com/en-US/help/4014997)
- [Opções de atualização e conversão para o Windows Server 2016](../get-started/supported-upgrade-paths.md)
- [Matriz de atualização e migração de função de servidor para o Windows Server 2016](../get-started/server-role-upgradeability-table.md)
- [Atualização e instalação do Windows Server](../get-started/installation-and-upgrade.md)
- [Notas de versão: Problemas importantes no Windows Server 2016](../get-started/windows-server-2016-ga-release-notes.md)
- [Recomendações para mudar para o Windows Server 2016](../get-started/recommendations-moving-to-server2016.md)

## <a name="solutions-for-volume-activation"></a>Soluções de ativação de volume
- [Ativação do Windows Server 2016](../get-started/server-2016-activation.md)
- [Revisar e selecionar métodos de ativação](https://technet.microsoft.com/library/jj134256(ws.11).aspx)
- [Códigos de erro de ativação para ativação de Volume](https://technet.microsoft.com/library/dn502528.aspx)
- [Como solucionar o serviço de gerenciamento de chaves (KMS)](https://technet.microsoft.com/library/ee939272.aspx)
- [Solução de problemas de ativação de volume](https://technet.microsoft.com/library/ff793439.aspx)
- [Códigos de erro de ativação](https://technet.microsoft.com/library/ff793399.aspx)
- [Instalação do Windows pode falhar com o erro "a chave do produto inserida não corresponde a nenhum das imagens do Windows disponíveis para instalação. Insira uma chave de produto diferente"](https://support.microsoft.com/help/2796988/windows-8-or-windows-server-2012-installation-may-fail-with-error-mess)

## <a name="solutions-related-to-dcpromo-and-installing-domain-controllers"></a>Soluções relacionados à DCPromo e à instalação de controladores de domínio
- [Requisitos de porta de serviços de domínio de diretório Active Directory e do Active Directory](https://technet.microsoft.com/library/dd772723(v=ws.10).aspx)
- [Portas de Firewall do Active Directory – vamos tentar tornar isso simples](http://blogs.msmvps.com/acefekay/2011/11/01/active-directory-firewall-ports-let-s-try-to-make-this-simple/)
- [Suporte do Exchange Server para Windows Server 2016](https://technet.microsoft.com/library/ff728623(v=exchg.150).aspx)
- [Usando Ntdsutil.exe para executar ou transferir funções FSMO para um controlador de domínio](https://support.microsoft.com/kb/255504)
- [Solução de problemas de implantação de controlador de domínio](../identity/ad-ds/deploy/troubleshooting-domain-controller-deployment.md)
- [Solucionando problemas de Assistente de instalação do Active Directory](https://msdn.microsoft.com/library/bb727058.aspx)
- [Problemas conhecidos para a instalação e remoção do AD DS](https://technet.microsoft.com/library/cc754463(v=ws.10).aspx)

## <a name="solutions-for-active-directory-federation-services-ad-fs"></a>Soluções para Serviços de Federação do Active Directory (AD FS)
- [Como configurar o registro automático de dispositivos ingressados no domínio do Windows com o Azure Active Directory](/azure/active-directory/active-directory-conditional-access-automatic-device-registration-setup)
- [Configurar a emissão de declarações](/azure/active-directory/device-management-hybrid-azuread-joined-devices-setup#step-2-setup-issuance-of-claims)
- [Configurar o AD FS para autenticar usuários armazenados nos diretórios do LDAP](../identity/ad-fs/operations/configure-ad-fs-to-authenticate-users-stored-in-ldap-directories.md)
- [Suporte do AD FS para associação de nome de host alternativo para autenticação de certificado](../identity/ad-fs/operations/ad-fs-support-for-alternate-hostname-binding-for-certificate-authentication.md)
- [Proteger contra ataques de senha](https://blogs.technet.microsoft.com/tspring/2017/01/20/federated-to-microsoft-cloud-and-account-lockouts/)
- [Como atualizar para o AD FS no Windows Server 2016 por meio de um banco de dados WID](../identity/ad-fs/deployment/upgrading-to-ad-fs-in-windows-server-2016.md)
- [Windows 10 com logon – habilitando a autenticação de dispositivo com o AD FS](../identity/ad-fs/operations/configure-device-based-conditional-access-on-premises.md)
- [Gerenciamento de certificados SSL em AD FS e WAP no Windows Server 2016](../identity/ad-fs/operations/manage-ssl-certificates-ad-fs-wap-2016.md)
- [Políticas de controle de acesso no Windows Server 2016 AD FS](../identity/ad-fs/operations/access-control-policies-in-ad-fs.md)

## <a name="solutions-related-to-active-directory-replication"></a>Soluções relacionadas à replicação do Active Directory

- [Como solucionar problemas de replicação do Active Directory](../identity/ad-ds/manage/troubleshoot/troubleshooting-active-directory-replication-problems.md)
- [Baixe a ferramenta de Status de replicação do Active Directory do Centro de Download da Microsoft](https://www.microsoft.com/en-in/download/details.aspx?id=30005)
- [e2e: Como solucionar erros comuns de replicação do Active Directory](https://support.microsoft.com/kb/3108513)
- [Solucionando problemas de erro de replicação do AD 8606: Foram dados atributos insuficientes para criar um objeto](https://support.microsoft.com/kb/2028495)
- [Evento 2108 e 1084 ocorrem durante a replicação de entrada do Active Directory no Windows 2000 Server e no Windows Server 2003](https://support.microsoft.com/kb/837932)
- [Erro de replicação do AD 8451 de solução de problemas: A operação de replicação encontrou um erro de banco de dados](https://support.microsoft.com/kb/2645996)
- [Erro de solução de problemas de replicação do AD 1127: Ao acessar o disco rígido, uma operação de disco falhou mesmo após várias tentativas](https://support.microsoft.com/kb/2025726)
- [Limpeza de metadados do servidor](https://technet.microsoft.com/library/cc816907.aspx)