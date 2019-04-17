---
title: Práticas recomendadas de servidor de política de rede
description: Este tópico fornece as melhores práticas para implantar e gerenciar o servidor de política de rede no Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 90e544bd-e826-4093-8c3b-6a6fc2dfd1d6
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5a9a68965d0d19504352ad67f7ab268b3d344fb2
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="network-policy-server-best-practices"></a>Práticas recomendadas de servidor de política de rede

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este tópico para saber mais sobre as práticas recomendadas para implantar e gerenciar o servidor de política de rede \(NPS\).

As seguintes seções fornecem as práticas recomendadas para diferentes aspectos da implantação do NPS.

## <a name="accounting"></a>Contabilidade

A seguir está as práticas recomendadas para registro em log NPS.

Existem dois tipos de contabilidade ou fazer logon, NPS:

- Log de eventos do NPS. Você pode usar o log de eventos para registrar eventos NPS nos logs de eventos do sistema e segurança. Isso é usado principalmente para auditoria e solucionar problemas de tentativas de conexão.

- Log de autenticação do usuário e solicitações de contabilidade. Você pode fazer logon solicitações de autenticação e estatísticas do usuário para arquivos de log no formato de texto ou banco de dados, ou você pode fazer logon em um procedimento armazenado em um banco de dados do SQL Server 2000. Solicitação de registro em log é usado principalmente para fins de faturamento e análise de conexão e também é útil como uma ferramenta de investigação de segurança, oferecendo um método de rastrear a atividade de um invasor.

Para tornar mais eficiente o uso de log do NPS:

- Ative registro em log \(initially\) para autenticação e os registros de contabilidade. Modificar essas seleções depois de determinar o que é adequado para seu ambiente.

- Certifique-se de que o log de eventos está configurado com capacidade suficiente para manter os logs.

- Fazer backup de todos os arquivos de log regularmente porque eles não podem ser recriados quando eles forem danificados ou excluídos.

- Use o atributo Class do RAIO para rastrear o uso e simplificar a identificação de qual departamento ou usuário cobrar por uso. Embora o atributo de classe gerado automaticamente seja exclusivo para cada solicitação, podem existir registros duplicados em casos em que a resposta ao servidor de acesso é perdida e a solicitação é reenviada. Talvez seja necessário excluir as solicitações duplicadas dos logs para rastrear o uso com precisão.

- Se seus servidores de acesso à rede e servidores proxy RADIUS periodicamente enviam mensagens de solicitação de conexão fictício para NPS para verificar se o servidor NPS está online, use o **executar ping nome de usuário** configuração do registro. Esta configuração define NPS para rejeitar automaticamente essas solicitações de conexão false sem processá-los. Além disso, o NPS não registrar transações envolvendo o nome de usuário fictício em quaisquer arquivos de log, que facilita o log de eventos interpretar.

- Desative o encaminhamento de notificação NAS. Você pode desativar o encaminhamento de iniciar e parar mensagens de servidores de acesso à rede (NASs) aos membros de um servidor remoto RADIUS agrupar que está configurado no NPS. Para obter mais informações, consulte [desativar encaminhamento de notificação NAS](nps-disable-nas-notifications.md).

Para obter mais informações, consulte [configurar rede política servidor Accounting](nps-accounting-configure.md).

- Para fornecer failover e redundância com o log do SQL Server, coloque dois computadores executando o SQL Server em várias sub-redes. Use o SQL Server **Assistente para criação de publicação** para configurar a replicação de banco de dados entre os dois servidores. Para obter mais informações, consulte [documentação técnica do SQL Server](https://msdn.microsoft.com/library/ms130214.aspx) e [replicação do SQL Server](https://msdn.microsoft.com/library/ms151198.aspx).

## <a name="authentication"></a>Autenticação

A seguir está as práticas recomendadas para autenticação.

- Use métodos de autenticação baseada em certificado como Protected Extensible Authentication Protocol \(PEAP\) e Extensible Authentication Protocol \(EAP\) para autenticação forte. Não use métodos de autenticação somente de senha porque eles são vulneráveis a uma variedade de ataques e não são seguros. Para autenticação segura de sem fio, usando PEAP\-MS\-CHAP v2 é recomendável, porque o servidor NPS comprova sua identidade para clientes sem fio usando um certificado de servidor, enquanto os usuários provam sua identidade com seu nome de usuário e senha.  Para obter mais informações sobre como usar NPS em sua implantação sem fio, consulte [baseada em senha implantar 802.1 X autenticado acesso sem fio](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/wireless/a-deploy-8021x-wireless-access).
- Implantar sua própria autoridade de certificação \(CA\) com o Active Directory&reg; \(AD CS\) os serviços de certificado quando você usa métodos de autenticação forte baseada em certificado, como PEAP e EAP, que exigem o uso de um certificado de servidor em servidores NPS. Você também pode usar a autoridade de certificação para registrar certificados de computador e certificados de usuário. Para obter mais informações sobre como implantar certificados de servidor para servidores NPS e acesso remoto, consulte [implantar certificados de servidor para 802.1 X com e sem fio implantações](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments).

## <a name="client-computer-configuration"></a>Configuração do computador cliente

A seguir está as práticas recomendadas para configuração do computador cliente.

- Configure automaticamente todos os seu membro do domínio 802.1 X computadores cliente usando a política de grupo. Para obter mais informações, consulte a seção "Políticas de rede (IEEE 802.11) do configurar acesso sem fio" no tópico [implantação de acesso sem fio](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/wireless/e-wireless-access-deployment#bkmk_policies).

## <a name="installation-suggestions"></a>Sugestões de instalação

A seguir está as práticas recomendadas para instalar o NPS.

- Antes de instalar o NPS, instalar e testar cada um dos seus servidores de acesso de rede usando métodos de autenticação local antes de configurá-los como clientes RADIUS no NPS.

- Depois de instalar e configurar o NPS, salvar a configuração usando o comando do Windows PowerShell [exportar NpsConfiguration](https://technet.microsoft.com/en-us/library/jj872749.aspx). Salve a configuração do NPS com esse comando sempre que você reconfigurar o servidor NPS.

>[!CAUTION]
>- O arquivo de configuração do NPS exportado contém segredos compartilhados não criptografados para clientes RADIUS e membros de grupos de servidores remotos RADIUS. Por isso, certifique-se de que você salve o arquivo em um local seguro.
>- O processo de exportação não inclui as configurações de registro em log para o Microsoft SQL Server no arquivo exportado. Se você importar o arquivo exportado para outro servidor NPS, você deve configurar manualmente log do SQL Server no novo servidor.

## <a name="performance-tuning-nps"></a>NPS de ajuste de desempenho

A seguir está as práticas recomendadas para NPS de ajuste de desempenho.

- Para otimizar os tempos de resposta de autenticação e autorização de NPS e minimizar o tráfego de rede, instale o NPS em um controlador de domínio.

- Quando são usados nomes principais universais \(UPNs\) ou domínios do Windows Server 2008 e Windows Server 2003, o NPS usa catálogo global para autenticar usuários. Para reduzir o tempo que leva para fazer isso, instale o NPS no servidor de catálogo global ou um servidor que está na mesma sub-rede como o servidor de catálogo global.

- Quando você tiver grupos de servidores remotos RADIUS configurados e, em políticas de solicitação de Conexão de NPS, você limpar o **registrar informações de estatísticas nos servidores no grupo de servidores remotos RADIUS a seguir** caixa de seleção, esses grupos ainda são enviados rede acesso servidor \(NAS\) iniciar e parar de mensagens de notificação. Isso cria o tráfego de rede desnecessário. Para eliminar esse tráfego, desative o encaminhamento de notificações para servidores individuais em cada grupo de servidores remotos RADIUS desmarcando a **encaminhar inicial de rede e interromper notificações para esse servidor** caixa de seleção.

## <a name="using-nps-in-large-organizations"></a>Usando NPS em grandes organizações

A seguir está as práticas recomendadas para usar o NPS em grandes organizações.

- Se você estiver usando políticas de rede para restringir o acesso apenas determinados grupos, crie um grupo universal para todos os usuários para o qual você deseja permitir o acesso e, em seguida, criar uma política de rede que conceda acesso a esse grupo universal. Não coloque todos os seus usuários diretamente no grupo universal, especialmente se você tiver um grande número de itens em sua rede. Em vez disso, crie grupos separados que são membros do grupo universal e adicione os usuários a esses grupos.

- Use um nome de entidade de usuário para se referir aos usuários sempre que possível. Um usuário pode ter o mesmo nome principal do usuário, independentemente de associação ao domínio. Essa prática oferece escalabilidade que talvez seja necessário em organizações com um grande número de domínios.

- Se você instalou o servidor de política de rede \(NPS\) em um computador que não seja um controlador de domínio e o servidor NPS está recebendo um grande número de solicitações de autenticação por segundo, você pode melhorar o desempenho de NPS, aumentando o número de autenticações simultâneas permitido entre o servidor NPS e o controlador de domínio. Para obter mais informações, consulte 

## <a name="security-issues"></a>Problemas de segurança

A seguir está as práticas recomendadas para reduzir problemas de segurança.

Quando você estiver administrando um servidor NPS remotamente, não envie dados importantes ou confidenciais (por exemplo, senhas ou segredos compartilhados) pela rede em texto sem formatação. Há dois métodos recomendados para administração remota de servidores NPS:

- Use serviços de área de trabalho remota para acessar o servidor NPS. Quando você usa os serviços de área de trabalho remota, os dados não são enviados entre cliente e servidor. Apenas a interface do usuário do servidor (por exemplo, o sistema operacional de desktop e a imagem do console NPS) é enviada para o cliente de serviços de área de trabalho remota, que é chamado de Conexão de área de trabalho remota no Windows&reg; 10. O cliente envia teclado e mouse de entrada, que são processadas localmente pelo servidor com serviços de área de trabalho remota habilitado. Quando os usuários de serviços de área de trabalho remota logon, eles podem exibir somente as sessões de cliente individuais, que são gerenciadas pelo servidor e são independentes umas das outras. Além disso, a Conexão de área de trabalho remota fornece criptografia de 128 bits entre cliente e servidor.

- Use o protocolo IPSec (IPsec) para criptografar dados confidenciais. Você pode usar IPsec para criptografar a comunicação entre o servidor NPS e o computador cliente remoto que você está usando para administrar NPS. Para administrar o servidor remotamente, você pode instalar o [Remote Server Administration Tools para Windows 10](https://www.microsoft.com/download/details.aspx?id=45520) no computador cliente. Após a instalação, use o Console de gerenciamento Microsoft (MMC) para adicionar o snap-in do servidor NPS para o console.

>[!IMPORTANT]
>Você pode instalar Remote Server Administration Tools para Windows 10 somente na versão completa do Windows 10 Professional ou o Windows 10 Enterprise.

Para obter mais informações sobre o NPS, consulte [servidor de política de rede (NPS)](nps-top.md).

