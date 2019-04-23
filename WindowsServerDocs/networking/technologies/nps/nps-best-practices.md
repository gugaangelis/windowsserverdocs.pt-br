---
title: Práticas recomendadas do Servidor de Políticas de Rede
description: Este tópico fornece as práticas recomendadas para implantar e gerenciar o servidor de políticas de rede no Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 90e544bd-e826-4093-8c3b-6a6fc2dfd1d6
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6cbd606edec06a80767ee997ef6a1397b66b7843
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834227"
---
# <a name="network-policy-server-best-practices"></a>Práticas recomendadas do Servidor de Políticas de Rede

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este tópico para saber mais sobre as práticas recomendadas para implantar e gerenciar o servidor de políticas de rede \(NPS\).

As seções a seguir fornecem práticas recomendadas para diferentes aspectos da implantação do NPS.

## <a name="accounting"></a>Contabilização

A seguir estão as práticas recomendadas para registro em log do NPS.

Há dois tipos de estatísticas ou o log, no NPS:

- Log de eventos do NPS. Você pode usar o log de eventos para registrar eventos NPS nos logs de eventos de sistema e segurança. Isso é usado principalmente para auditoria e solucionar problemas de tentativas de conexão.

- Registro em log solicitações de contabilização e autenticação de usuário. Você pode registrar as solicitações de autenticação e estatísticas em arquivos de log no formato de texto ou banco de dados, ou você pode fazer logon para um procedimento armazenado em um banco de dados do SQL Server 2000. Log de solicitação é usado principalmente para fins de análise e a cobrança de conexão e também é útil como ferramenta de investigação de segurança, fornecendo a você um método de rastrear a atividade de um invasor.

Para tornar o uso mais eficiente de registro em log do NPS:

- Ativar o registro em log \(inicialmente\) para os registros de contabilização e autenticação. Modifique essas seleções depois de determinar o que é apropriado para seu ambiente.

- Certifique-se de que o log de eventos está configurado com uma capacidade suficiente manter seus logs.

- Fazer backup todos os arquivos de log regularmente, porque eles não podem ser recriados quando eles forem danificados ou excluídos.

- Use o atributo de classe RADIUS para rastrear o uso e simplificar a identificação do departamento ou usuário para cobrança pelo uso. Embora o atributo de classe gerado automaticamente é exclusivo para cada solicitação, podem existir registros duplicados em casos em que a resposta para o servidor de acesso é perdida e a solicitação é enviada novamente. Você talvez precise excluir solicitações duplicadas de seus logs para rastrear com precisão o uso.

- Se seus servidores de acesso de rede e servidores de proxy RADIUS periodicamente enviam mensagens de solicitação de conexão fictícia para o NPS para verificar se o NPS está online, use o **executar o ping de nome de usuário** configuração do registro. Essa configuração define o NPS para rejeitar automaticamente essas solicitações de conexão false sem processá-las. Além disso, o NPS não registrar transações que envolvem o nome de usuário fictício em quaisquer arquivos de log, que torna mais fácil interpretar o log de eventos.

- Desabilite o encaminhamento de notificação NAS. Você pode desabilitar o encaminhamento de início e grupo de mensagens de parada de servidores de acesso de rede (NASs) para os membros de um servidor RADIUS remoto que está configurado no NPS. Para obter mais informações, consulte [desabilitar o encaminhamento de notificação NAS](nps-disable-nas-notifications.md).

Para obter mais informações, consulte [configurar política de servidor de contabilidade de rede](nps-accounting-configure.md).

- Para fornecer redundância e failover com o registro em log do SQL Server, coloque dois computadores executando o SQL Server em sub-redes diferentes. Usar o SQL Server **criar Assistente de publicação** para configurar a replicação de banco de dados entre os dois servidores. Para obter mais informações, consulte [documentação técnica do SQL Server](https://msdn.microsoft.com/library/ms130214.aspx) e [replicação do SQL Server](https://msdn.microsoft.com/library/ms151198.aspx).

## <a name="authentication"></a>Autenticação

A seguir estão as práticas recomendadas para autenticação.

- Usar métodos de autenticação baseada em certificado, como Protected Extensible Authentication Protocol \(PEAP\) e o protocolo de autenticação extensível \(EAP\) para autenticação forte. Não use métodos de autenticação somente de senha porque eles são vulneráveis a uma variedade de ataques e não são seguros. Para autenticação sem fio segura usando PEAP\-MS\-CHAP v2 é recomendada, pois o NPS comprova sua identidade para os clientes sem fio usando um certificado de servidor, enquanto os usuários comprovem sua identidade com seu nome de usuário e senha.  Para obter mais informações sobre como usar o NPS em sua implantação sem fio, consulte [baseado em senha implantar acesso autenticado 802.1 X sem fio](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/wireless/a-deploy-8021x-wireless-access).
- Implantar sua própria autoridade de certificação \(autoridade de certificação\) com o Active Directory&reg; serviços de certificados \(AD CS\) quando você usa métodos de autenticação forte baseada em certificado, como PEAP e EAP, que exigir o uso de um certificado de servidor em NPSs. Você também pode usar sua autoridade de certificação para registrar certificados de computador e certificados de usuário. Para obter mais informações sobre como implantar certificados de servidor para servidores NPS e acesso remoto, consulte [implantar certificados de servidor para 802.1 X com fio e implantações sem fio](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments).

## <a name="client-computer-configuration"></a>Configuração do computador cliente

A seguir estão as práticas recomendadas para configuração do computador cliente.

- Configure todos seu membro de domínio 802.1 X computadores cliente automaticamente usando a diretiva de grupo. Para obter mais informações, consulte a seção "Configurar Wireless Network (IEEE 802.11) Policies" no tópico [implantação do acesso sem fio](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/wireless/e-wireless-access-deployment#bkmk_policies).

## <a name="installation-suggestions"></a>Sugestões de instalação

A seguir estão as práticas recomendadas para a instalação do NPS.

- Antes de instalar o NPS, instale e teste cada um dos seus servidores de acesso de rede usando métodos de autenticação local antes de você configurá-los como clientes RADIUS no NPS.

- Depois de instalar e configurar o NPS, salve a configuração usando o comando do Windows PowerShell [NpsConfiguration exportação](https://technet.microsoft.com/library/jj872749.aspx). Salve a configuração do NPS com este comando sempre que você reconfigurar o NPS.

>[!CAUTION]
>- O arquivo de configuração do NPS exportado contém segredos compartilhados não criptografados para clientes RADIUS e membros de grupos de servidores RADIUS remotos. Por isso, certifique-se de que você salve o arquivo em um local seguro.
>- O processo de exportação não inclui configurações de registro em log para o Microsoft SQL Server no arquivo exportado. Se você importar o arquivo exportado para outro NPS, você deverá configurar manualmente o log do SQL Server no novo servidor.

## <a name="performance-tuning-nps"></a>NPS de ajuste de desempenho

A seguir estão as práticas recomendadas para NPS de ajuste de desempenho.

- Para otimizar os tempos de resposta de autenticação e autorização do NPS e minimizar o tráfego de rede, instale o NPS em um controlador de domínio.

- Quando os nomes de entidade de segurança universais \(UPNs\) ou domínios do Windows Server 2008 e Windows Server 2003 são usados, o NPS usa o catálogo global para autenticar usuários. Para minimizar o tempo que leva para fazer isso, instale o NPS no servidor de catálogo global ou um servidor que está na mesma sub-rede que o servidor de catálogo global.

- Quando você tiver grupos de servidores remotos RADIUS configurados e, em diretivas de solicitação de Conexão de NPS, você deve limpar os **registrar informações de estatísticas nos servidores no grupo de servidores remotos RADIUS a seguir** caixa de seleção, esses grupos são ainda enviado do servidor de acesso de rede \(NAS\) iniciar e parar as mensagens de notificação. Isso cria o tráfego de rede desnecessário. Para eliminar esse tráfego, desabilitar o encaminhamento de notificação para servidores individuais em cada grupo de servidores remotos RADIUS desmarcando os **encaminhar o início de rede e interromper as notificações para este servidor** caixa de seleção.

## <a name="using-nps-in-large-organizations"></a>Usando o NPS em organizações de grandes porte

A seguir estão as práticas recomendadas para usar o NPS em grandes organizações.

- Se você estiver usando políticas de rede para restringir o acesso apenas determinados grupos, crie um grupo universal para todos os usuários para quem você deseja permitir o acesso e, em seguida, crie uma política de rede que concede acesso para esse grupo universal. Não coloque todos os usuários diretamente para o grupo universal, especialmente se você tiver um grande número em sua rede. Em vez disso, crie grupos separados que são membros do grupo universal e adicionar usuários a esses grupos.

- Use um nome para referir-se aos usuários sempre que possível. Um usuário pode ter o mesmo nome de entidade de segurança do usuário, independentemente da associação de domínio. Essa prática oferece escalabilidade que pode ser necessárias em organizações com um grande número de domínios.

- Se você instalou o servidor de políticas de rede \(NPS\) em um computador que não seja um domínio controlador e o NPS está recebendo um grande número de solicitações de autenticação por segundo, você pode melhorar o desempenho do NPS, aumentando o número de autenticações simultâneas permitidas entre o NPS e o controlador de domínio. Para obter mais informações, consulte . 

## <a name="security-issues"></a>Problemas de segurança

A seguir estão as práticas recomendadas para reduzir problemas de segurança.

Quando você está administrando um NPS remotamente, não envie dados confidenciais (por exemplo, senhas ou segredos compartilhados) pela rede em texto não criptografado. Há dois métodos recomendados para administração remota de NPSs:

- Use os serviços de área de trabalho remota para acessar o NPS. Quando você usa os serviços de área de trabalho remota, os dados não são enviados entre cliente e servidor. Somente a interface do usuário do servidor (por exemplo, a área de trabalho do sistema operacional e a imagem do console do NPS) é enviada para o cliente de serviços de área de trabalho remota, que é chamado de Conexão de área de trabalho remota no Windows&reg; 10. O cliente envia teclado e mouse de entrada, que são processadas localmente pelo servidor que tem serviços de área de trabalho remota habilitada. Quando os usuários de serviços de área de trabalho remota fizerem logon, eles podem exibir somente suas sessões de cliente individual, que são gerenciadas pelo servidor e são independentes umas das outras. Além disso, a Conexão de área de trabalho remota fornece criptografia de 128 bits entre cliente e servidor.

- Use o Internet Protocol security (IPsec) para criptografar dados confidenciais. Você pode usar IPsec para criptografar a comunicação entre o NPS e o computador cliente remoto que você está usando para administrar o NPS. Para administrar o servidor remotamente, você pode instalar o [Remote Server Administration ferramentas para Windows 10](https://www.microsoft.com/download/details.aspx?id=45520) no computador cliente. Após a instalação, use o Microsoft Management Console (MMC) para adicionar o snap-in do NPS para o console.

>[!IMPORTANT]
>Você pode instalar o Remote Server Administration ferramentas para Windows 10 somente na versão completa do Windows 10 Professional ou Windows 10 Enterprise.

Para obter mais informações sobre o NPS, consulte [servidor de diretivas de rede (NPS)](nps-top.md).

