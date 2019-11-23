---
title: Práticas recomendadas do Servidor de Políticas de Rede
description: Este tópico fornece as práticas recomendadas para implantar e gerenciar o servidor de políticas de rede no Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 90e544bd-e826-4093-8c3b-6a6fc2dfd1d6
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 364783e2188152fc5c57bba04991ae124b0bb8d2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405498"
---
# <a name="network-policy-server-best-practices"></a>Práticas recomendadas do Servidor de Políticas de Rede

>Aplicável a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este tópico para saber mais sobre as práticas recomendadas para implantar e gerenciar o servidor de políticas de rede \(NPS\).

As seções a seguir fornecem as práticas recomendadas para diferentes aspectos da implantação do NPS.

## <a name="accounting"></a>Contabilização

A seguir estão as práticas recomendadas para o log do NPS.

Há dois tipos de contabilidade, ou registro em log, no NPS:

- Log de eventos para NPS. Você pode usar o log de eventos para registrar eventos do NPS nos logs de eventos do sistema e de segurança. Isso é usado principalmente para auditoria e solução de problemas de tentativas de conexão.

- Registrando solicitações de estatísticas e autenticação de usuário. Você pode registrar solicitações de estatísticas e autenticação de usuário em arquivos de log em formato de texto ou de banco de dados, ou pode fazer logon em um procedimento armazenado em um banco de dados SQL Server 2000. O log de solicitações é usado principalmente para fins de análise e cobrança de conexão e também é útil como uma ferramenta de investigação de segurança, fornecendo um método de rastrear a atividade de um invasor.

Para fazer o uso mais eficaz do log do NPS:

- Ative o registro em log \(inicialmente\) para os registros de autenticação e de estatísticas. Modifique essas seleções depois de determinar o que é apropriado para o seu ambiente.

- Verifique se o log de eventos está configurado com uma capacidade suficiente para manter seus logs.

- Faça backup de todos os arquivos de log regularmente, pois eles não podem ser recriados quando forem danificados ou excluídos.

- Use o atributo de classe RADIUS para acompanhar o uso e simplificar a identificação de qual departamento ou usuário será cobrado pelo uso. Embora o atributo de classe gerado automaticamente seja exclusivo para cada solicitação, registros duplicados podem existir nos casos em que a resposta ao servidor de acesso é perdida e a solicitação é reenviada. Talvez seja necessário excluir solicitações duplicadas de seus logs para acompanhar com precisão o uso.

- Se os servidores de acesso à rede e os servidores proxy RADIUS enviarem periodicamente mensagens de solicitação de conexão fictícia ao NPS para verificar se o NPS está online, use a configuração do registro **ping User-Name** . Essa configuração configura o NPS para rejeitar automaticamente essas solicitações de conexão falsas sem processá-las. Além disso, o NPS não registra as transações que envolvem o nome de usuário fictício em nenhum arquivo de log, o que torna o log de eventos mais fácil de interpretar.

- Desabilite o encaminhamento de notificação do NAS. Você pode desabilitar o encaminhamento de mensagens de início e de parada de servidores de acesso à rede (NASs) para membros de um grupo de servidores RADIUS remotos configurado no NPS. Para obter mais informações, consulte [desabilitar o encaminhamento de notificação do nas](nps-disable-nas-notifications.md).

Para obter mais informações, consulte [Configure Network Policy Server Accounting](nps-accounting-configure.md).

- Para fornecer failover e redundância com o log de SQL Server, coloque dois computadores executando SQL Server em sub-redes diferentes. Use o assistente de SQL Server **criar publicação** para configurar a replicação de banco de dados entre os dois servidores. Para obter mais informações, consulte [SQL Server documentação técnica](https://msdn.microsoft.com/library/ms130214.aspx) e [replicação do SQL Server](https://msdn.microsoft.com/library/ms151198.aspx).

## <a name="authentication"></a>Autenticação

A seguir estão as práticas recomendadas para autenticação.

- Use métodos de autenticação baseados em certificado, como o protocolo de autenticação extensível protegido \(PEAP\) e protocolo de autenticação extensível \(\) EAP para autenticação forte. Não use métodos de autenticação somente de senha porque eles são vulneráveis a uma variedade de ataques e não são seguros. Para uma autenticação sem fio segura, o uso do PEAP\-MS\-CHAP v2 é recomendado, pois o NPS comprova sua identidade para clientes sem fio usando um certificado de servidor, enquanto os usuários provam sua identidade com seu nome de usuário e senha.  Para obter mais informações sobre como usar o NPS em sua implantação sem fio, consulte [implantar o acesso sem fio autenticado baseado em senha 802.1 x](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/wireless/a-deploy-8021x-wireless-access).
- Implante sua própria autoridade de certificação \(\) de AC com Active Directory&reg; serviços de certificados \(o AD CS\) quando você usa métodos de autenticação baseados em certificado fortes, como PEAP e EAP, que exigem o uso de um certificado de servidor no NPSs. Você também pode usar sua autoridade de certificação para registrar certificados de computador e certificados de usuário. Para obter mais informações sobre como implantar certificados de servidor em servidores de acesso remoto e NPS, consulte [implantar certificados de servidor para implantações com e sem fio 802.1 x](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments).

> [!IMPORTANT]
> O NPS (servidor de diretivas de rede) não oferece suporte ao uso de caracteres ASCII estendidos em senhas.

## <a name="client-computer-configuration"></a>Configuração do computador cliente

A seguir estão as práticas recomendadas para a configuração do computador cliente.

- Configure automaticamente todos os computadores cliente 802.1 X membro do domínio usando Política de Grupo. Para obter mais informações, consulte a seção "configurar políticas de rede sem fio (IEEE 802,11)" no tópico [implantação de acesso sem fio](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/wireless/e-wireless-access-deployment#bkmk_policies).

## <a name="installation-suggestions"></a>Sugestões de instalação

A seguir estão as práticas recomendadas para instalar o NPS.

- Antes de instalar o NPS, instale e teste cada um dos seus servidores de acesso à rede usando métodos de autenticação local antes de configurá-los como clientes RADIUS no NPS.

- Depois de instalar e configurar o NPS, salve a configuração usando o comando do Windows PowerShell [Export-NpsConfiguration](https://technet.microsoft.com/library/jj872749.aspx). Salve a configuração do NPS com esse comando sempre que reconfigurar o NPS.

>[!CAUTION]
>- O arquivo de configuração do NPS exportado contém segredos compartilhados não criptografados para clientes RADIUS e membros de grupos de servidores RADIUS remotos. Por isso, lembre-se de salvar o arquivo em um local seguro.
>- O processo de exportação não inclui configurações de log para Microsoft SQL Server no arquivo exportado. Se você importar o arquivo exportado para outro NPS, será necessário configurar manualmente SQL Server log no novo servidor.

## <a name="performance-tuning-nps"></a>NPS de ajuste de desempenho

A seguir estão as práticas recomendadas para o ajuste de desempenho do NPS.

- Para otimizar a autenticação NPS e os tempos de resposta de autorização e minimizar o tráfego de rede, instale o NPS em um controlador de domínio.

- Quando os nomes de entidade de segurança universais \(UPNs\) ou os domínios do Windows Server 2008 e do Windows Server 2003 são usados, o NPS usa o catálogo global para autenticar os usuários. Para minimizar o tempo necessário para fazer isso, instale o NPS em um servidor de catálogo global ou em um servidor que esteja na mesma sub-rede que o servidor de catálogo global.

- Quando você tiver grupos de servidores RADIUS remotos configurados e, em políticas de solicitação de conexão do NPS, desmarque a caixa de seleção **registrar as informações de contabilização nos servidores no seguinte grupo de servidores remotos RADIUS** , esses grupos ainda serão enviados ao servidor de acesso à rede \(nas\) iniciar e parar mensagens de notificação. Isso cria tráfego de rede desnecessário. Para eliminar esse tráfego, desabilite o encaminhamento de notificação do NAS para servidores individuais em cada grupo de servidores RADIUS remotos desmarcando a caixa de seleção **iniciar a rede e parar notificações neste servidor** .

## <a name="using-nps-in-large-organizations"></a>Usando o NPS em grandes organizações

A seguir estão as práticas recomendadas para usar o NPS em grandes organizações.

- Se você estiver usando políticas de rede para restringir o acesso a todos os grupos, exceto certos, crie um grupo universal para todos os usuários para os quais você deseja permitir o acesso e crie uma política de rede que conceda acesso a esse grupo universal. Não coloque todos os seus usuários diretamente no grupo universal, especialmente se você tiver um grande número deles em sua rede. Em vez disso, crie grupos separados que sejam membros do grupo universal e adicione usuários a esses grupos.

- Use um nome principal de usuário para se referir aos usuários sempre que possível. Um usuário pode ter o mesmo nome de entidade de usuário, independentemente da Associação de domínio. Essa prática fornece escalabilidade que pode ser necessária em organizações com um grande número de domínios.

- Se você instalou o servidor de políticas de rede \(\) NPS em um computador que não seja um controlador de domínio e o NPS estiver recebendo um grande número de solicitações de autenticação por segundo, você poderá melhorar o desempenho do NPS aumentando o número de autenticações simultâneas permitidas entre o NPS e o controlador de domínio. Para obter mais informações, consulte . 

## <a name="security-issues"></a>Problemas de segurança

A seguir estão as práticas recomendadas para a redução de problemas de segurança.

Quando você estiver administrando um NPS remotamente, não envie dados confidenciais (por exemplo, segredos ou senhas compartilhadas) pela rede em texto sem formatação. Há dois métodos recomendados para a administração remota do NPSs:

- Use Serviços de Área de Trabalho Remota para acessar o NPS. Quando você usa Serviços de Área de Trabalho Remota, os dados não são enviados entre o cliente e o servidor. Somente a interface do usuário do servidor (por exemplo, a imagem do console do sistema operacional e do NPS) é enviada para o cliente Serviços de Área de Trabalho Remota, chamado Conexão de Área de Trabalho Remota no Windows&reg; 10. O cliente envia a entrada de teclado e mouse, que é processada localmente pelo servidor que tem Serviços de Área de Trabalho Remota habilitado. Quando Serviços de Área de Trabalho Remota usuários fazem logon, eles podem exibir apenas suas sessões de cliente individuais, que são gerenciadas pelo servidor e são independentes umas das outras. Além disso, Conexão de Área de Trabalho Remota fornece criptografia de 128 bits entre o cliente e o servidor.

- Use o IPsec (Internet Protocol Security) para criptografar dados confidenciais. Você pode usar o IPsec para criptografar a comunicação entre o NPS e o computador cliente remoto que você está usando para administrar o NPS. Para administrar o servidor remotamente, você pode instalar o [ferramentas de administração de servidor remoto para Windows 10](https://www.microsoft.com/download/details.aspx?id=45520) no computador cliente. Após a instalação, use o MMC (console de gerenciamento Microsoft) para adicionar o snap-in do NPS ao console do.

>[!IMPORTANT]
>Você pode instalar o Ferramentas de Administração de Servidor Remoto para Windows 10 somente na versão completa do Windows 10 Professional ou Windows 10 Enterprise.

Para obter mais informações sobre o NPS, consulte [servidor de diretivas de rede (NPS)](nps-top.md).

