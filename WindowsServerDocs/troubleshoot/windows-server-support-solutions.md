---
title: "Principais soluções de suporte do Windows Server"
description: "Obter links para soluções de problemas do Windows Server"
ms.prod: windows-server-threshold
ms.service: na
manager: alant
ms.technology: server-general
ms.date: 09/06/2017
ms.topic: article
author: kaushika-msft
ms.author: elizapo
ms.openlocfilehash: 070331c8886011035e712f485af45d36b77613fa
ms.sourcegitcommit: 58dde3f9ae761116b2c9f71c6917f96bff075af1
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/08/2017
---
# <a name="top-support-solutions-for-windows-server-2016"></a>Principais soluções de suporte do Windows Server 2016

A Microsoft lança regularmente atualizações e soluções para o Windows Server. Para garantir que os servidores podem receber atualizações futuras, incluindo atualizações de segurança, é importante mantê-los atualizados. Confira [Histórico de atualizações do Windows 10 e Windows Server 2016](https://support.microsoft.com/en-us/help/4000825/windows-10-windows-server-2016-update-history) para obter uma lista completa das atualizações lançadas.

Essas são as principais soluções do Suporte da Microsoft para a maioria dos problemas comuns enfrentados ao usar o Windows Server 2016. Os links a seguir incluem links para artigos da KB, atualizações e artigos de biblioteca.

## <a name="solutions-for-installing-or-upgrading-windows-server"></a>Soluções para instalar ou atualizar o Windows Server

- [Resolver erros de upgrade do Windows 10: informações técnicas para profissionais de TI](\windows\deployment\upgrade\resolve-windows-10-upgrade-errors)
- [Atualização da pilha de manutenção para Windows 10 versão 1607 e Windows Server 2016: 8 de agosto de 2017](https://support.microsoft.com/en-US/help/4035631)
- [Atualização de compatibilidade a fim de atualizar para Windows 10 versão 1607 e Windows Server 2016: 3 de agosto de 2017](https://support.microsoft.com/en-US/help/4033524)
- [Não há suporte para uma upgrade in-loco do sistema com base em VMs do Azure com base no Windows](https://support.microsoft.com/en-US/help/4014997)
- [Opções de upgrade e conversão para o Windows Server 2016](..\get-started\supported-upgrade-paths.md)
- [Matriz de upgrade e migração da função de servidor para Windows Server 2016](..\get-started\server-role-upgradeability-table.md)
- [Instalação e upgrade do Windows Server](..\get-started\installation-and-upgrade.md)
- [Notas de versão: problemas importantes no Windows Server 2016](..\get-started\windows-server-2016-ga-release-notes.md)
- [Recomendações para mudar para o Windows Server 2016](..\get-started\recommendations-moving-to-server2016.md)

## <a name="solutions-for-volume-activation"></a>Soluções de ativação de volume
- [Ativação do Windows Server 2016](../get-started/server-2016-activation.md)
- [analisar e selecionar métodos de ativação](https://technet.microsoft.com/library/jj134256(ws.11).aspx)
- [Códigos de Erro de Ativação para Ativação de Volume](https://technet.microsoft.com/library/dn502528.aspx)
- [Como solucionar problemas do Serviço de Gerenciamento de Chaves (KMS)](https://technet.microsoft.com/library/ee939272.aspx)
- [Solução de problemas de ativação do volume](https://technet.microsoft.com/library/ff793439.aspx)
- [Códigos de Erro de Ativação](https://technet.microsoft.com/library/ff793399.aspx)
- [Pode ocorrer falha de instalação do Windows com o erro "A chave do produto (Product Key) digitada não corresponde a nenhum das imagens do Windows disponíveis para instalação. Insira uma chave do produto (Product Key) diferente"](https://support.microsoft.com/help/2796988/windows-8-or-windows-server-2012-installation-may-fail-with-error-mess)

## <a name="solutions-related-to-dcpromo-and-installing-domain-controllers"></a>Soluções relacionados à DCPromo e à instalação de controladores de domínio
- [Requisitos de porta do Active Directory e do Active Directory Domain Services](https://technet.microsoft.com/library/dd772723(v=ws.10).aspx)
- [Portas de Firewall do Active Directory – Vamos tentar simplificar](http://blogs.msmvps.com/acefekay/2011/11/01/active-directory-firewall-ports-let-s-try-to-make-this-simple/)
- [Suporte ao Exchange Server para Windows Server 2016](https://technet.microsoft.com/library/ff728623(v=exchg.150).aspx)
- [Uso de Ntdsutil.exe para transferir ou executar funções FSMO de um controlador de domínio.](http://support.microsoft.com/kb/255504)
- [Solução de problemas de implantação de controlador de domínio](../identity/ad-ds/deploy/troubleshooting-domain-controller-deployment.md)
- [Solucionar problemas do assistente de instalação do Active Directory](https://msdn.microsoft.com/library/bb727058.aspx)
- [Problemas conhecidos relacionados à instalação e remoção de AD DS](https://technet.microsoft.com/library/cc754463(v=ws.10).aspx)

## <a name="solutions-for-active-directory-federation-services-ad-fs"></a>Soluções para Serviços de Federação do Active Directory (AD FS)
- [Como configurar o registro automático de dispositivos ingressados em domínio do Windows com o Azure Active Directory](/azure/active-directory/active-directory-conditional-access-automatic-device-registration-setup)
- [Configurar a emissão de declarações](/azure/active-directory/device-management-hybrid-azuread-joined-devices-setup#step-2-setup-issuance-of-claims)
- [Configurar o AD FS para autenticar usuários armazenados nos diretórios do LDAP](../identity/ad-fs/operations/configure-ad-fs-to-authenticate-users-stored-in-ldap-directories.md)
- [Suporte do AD FS para associação de nome de host alternativo para autenticação de certificado](../identity/ad-fs/operations/ad-fs-support-for-alternate-hostname-binding-for-certificate-authentication.md)
- [Proteger contra ataques de senha](https://blogs.technet.microsoft.com/tspring/2017/01/20/federated-to-microsoft-cloud-and-account-lockouts/)
- [Atualizando para o AD FS no Windows Server 2016 por meio de um banco de dados WID](../identity/ad-fs/deployment/upgrading-to-ad-fs-in-windows-server-2016.md)
- [Logon do Windows 10 – habilitar a autenticação de dispositivo com o AD FS](../identity/ad-fs/operations/configure-device-based-conditional-access-on-premises.md)
- [Gerenciamento de certificados SSL no AD FS e no Windows Server 2016](../identity/ad-fs/operations/manage-ssl-certificates-ad-fs-wap-2016.md)
- [Políticas de controle de acesso no AD FS para Windows Server 2016](../identity/ad-fs/operations/access-control-policies-in-ad-fs.md)

## <a name="solutions-related-to-active-directory-replication"></a>Soluções relacionadas à replicação do Active Directory

- [Solucionar problemas de replicação do Active Directory](../identity/ad-ds/manage/troubleshoot/troubleshooting-active-directory-replication-problems.md)
- [Baixar a ferramenta de Status de Replicação do Active Directory do Centro de Download da Microsoft](http://www.microsoft.com/en-in/download/details.aspx?id=30005)
- [e2e: como solucionar erros comuns de replicação do Active Directory](http://support.microsoft.com/kb/3108513)
- [Solução de problemas do AD Erro de replicação 8606: não foram fornecidos os atributos suficientes para criar um objeto](http://support.microsoft.com/kb/2028495)
- [As IDs de evento 2108 e 1084 ocorrem durante a duplicação de entrada do Active Directory no Windows 2000 Server e no Windows Server 2003](http://support.microsoft.com/kb/837932)
- [Solução de problemas do AD Erro de replicação 8451: a operação de replicação encontrou um erro de banco de dados](http://support.microsoft.com/kb/2645996)
- [Solução de problemas do AD Erro de replicação 1127: ao acessar o disco rígido, uma operação de disco falhou mesmo após várias tentativas](http://support.microsoft.com/kb/2025726)
- [Limpar os metadados do servidor](https://technet.microsoft.com/en-us/library/cc816907.aspx)