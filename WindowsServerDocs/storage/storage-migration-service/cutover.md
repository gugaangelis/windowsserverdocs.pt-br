---
title: Como a transferência funciona no serviço de migração de armazenamento
description: Resumo e detalhes de estágios de transferência no serviço de migração de armazenamento
author: t-chrche
ms.author: t-chrche
manager: nedpyle
ms.date: 08/31/2020
ms.topic: article
ms.openlocfilehash: 985fd14b7791d28246b8e9186ca83216df734875
ms.sourcegitcommit: a640c2d7f2d21d7cd10a9be4496e1574e5e955f0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/04/2020
ms.locfileid: "89448908"
---
# <a name="how-cutover-works-in-storage-migration-service"></a>Como a transferência funciona no serviço de migração de armazenamento

A transferência é a fase de migração que move a identidade de rede do computador de origem para o computador de destino. Após a transferência, o computador de origem ainda conterá os mesmos arquivos que antes, mas ele não estará disponível para usuários e aplicativos.

## <a name="summary"></a>Resumo

![Captura de tela de configuração de transferência ](media/cutover/cutover_configuration.png)
 __Figura 1: configuração de transferência do serviço de migração de armazenamento__

Antes de iniciar a transferência, você fornece as informações de configuração de rede necessárias para recortar do computador de origem para o computador de destino. Você também pode escolher um novo nome exclusivo para o computador de origem ou deixar que o serviço de migração de armazenamento crie um aleatório.

Em seguida, o serviço de migração de armazenamento executa as seguintes etapas para cortar o computador de origem para o computador de destino:

1. Nós nos conectamos aos computadores de origem e de destino. Eles já devem ter as seguintes regras de firewall habilitadas para entrada:
    * Compartilhamento de arquivos e impressoras (SMB-in), porta TCP 445
    * Serviço Netlogon (NP-in), porta TCP 445
    * Instrumentação de Gerenciamento do Windows (DCOM-in), porta TCP 135
    * Instrumentação de Gerenciamento do Windows (WMI-in), TCP, qualquer porta

2. Definimos permissões de segurança no computador de destino em Active Directory Domain Services para corresponder às permissões do computador de origem.

3. Criamos uma conta de usuário local temporária no computador de origem. Se o computador estiver ingressado no domínio, o nome de usuário da conta será "MsftSmsStorMigratSvc". Desabilite a [política de filtro de token de conta local](https://support.microsoft.com/help/951016/description-of-user-account-control-and-remote-restrictions-in-windows) no computador de origem para permitir a conta e, em seguida, conecte-se ao computador de origem. Podemos tornar essa conta temporária para que, quando reiniciar e remover o computador de origem do domínio posteriormente, ainda possamos acessar o computador de origem.

4. Repetimos a etapa anterior no computador de destino.

5. Removemos o computador de origem do domínio para liberar sua conta de Active Directory, que o computador de destino passará mais tarde.

6. Mapeamos as interfaces de rede no computador de origem e renomeamos o computador de origem.

7. Adicionamos o computador de origem de volta ao domínio. O computador de origem agora tem uma nova identidade e está disponível para administradores, mas não para usuários e aplicativos.

8. No computador de origem, removemos qualquer nome de computador alternativo remanescente, removemos a conta local temporária que criamos e reabilitamos a política de filtro de token de conta local.

9. Removemos o computador de destino do domínio.

10. Substituimos os endereços IP no computador de destino pelas informações de IP fornecidas pela origem e renomeamos o computador de destino para o nome original do computador de origem.

11. Nós adicionamos o computador de destino de volta ao domínio. Quando Unido, ele usa a conta de computador Active Directory original do computador de origem. Isso preserva as associações de grupo e as ACLs de segurança. O computador de destino agora tem a identidade do computador de origem.

12. No computador de destino, removemos qualquer nome de computador alternativo remanescente, removemos a conta local temporária que criamos e reabilitamos a política de filtro de token de conta local, concluindo a transferência.

Depois que a transferência for concluída, o computador de destino terá feito a identidade do computador de origem e você poderá encerrar o computador de origem.

## <a name="detailed-stages"></a>Estágios detalhados

![Captura de tela da fase de transferência ](media/cutover/cutover_stage_description.png)
 __Figura 2: serviço de migração de armazenamento mostrando uma descrição do estágio de transferência__

Você pode controlar o progresso da transferência por meio de descrições de cada estágio que aparecem como mostrado na figura acima. A tabela a seguir mostra cada estágio possível junto com seu progresso, descrição e notas de esclarecimento.

|  Progresso | Descrição                                                                                               |  Observações |
|:-----|:--------------------------------------------------------------------------------------------------------------------|:---|
|  0% | A transferência está ociosa. |   |
| 2%  | Conectando ao computador de origem... |   Certifique-se de que os [requisitos para computadores de origem e de destino](https://docs.microsoft.com/windows-server/storage/storage-migration-service/overview#security-requirements-the-storage-migration-service-proxy-service-and-firewall-ports) sejam atendidos.|
| 5%  | Conectando ao computador de destino... |   |
| 6%  | Definindo permissões de segurança no objeto de computador em Active Directory... |   Replica as permissões de segurança do objeto de Active Directory do computador de origem no computador de destino.|
| 8%  | Certificando-se de que a conta temporária que criamos foi excluída com êxito no computador de origem... |   Garante que possamos criar uma conta temporária com o mesmo nome.|
| 11 | Criando uma conta de usuário local temporária no computador de origem... |   Se o computador de origem estiver ingressado no domínio, o nome de usuário da conta temporária será "MsftSmsStorMigratSvc". A senha consiste em 127 caracteres largos Unicode aleatórios com letras, números, símbolos e alterações de maiúsculas e minúsculas. Se o computador de origem estiver em um grupo de trabalho, usaremos as credenciais de origem originais.|
| 13 | Definindo a política de filtro de token de conta local no computador de origem... |   Desabilita a política para que possamos nos conectar à origem quando ela não estiver ingressada no domínio. Saiba mais sobre a política de filtro de token de conta local [aqui](https://support.microsoft.com/help/951016/description-of-user-account-control-and-remote-restrictions-in-windows).|
| 16% | Conectando-se ao computador de origem usando a conta de usuário local temporário... |   |
| 19% | Certificando-se de que a conta temporária que criamos foi excluída com êxito no computador de destino... |   |
| 22% | Criando uma conta de usuário local temporária no computador de destino... | Se o computador de destino estiver ingressado no domínio, o nome de usuário da conta temporária será "MsftSmsStorMigratSvc". A senha consiste em 127 caracteres largos Unicode aleatórios com letras, números, símbolos e alterações de maiúsculas e minúsculas. Se o computador de destino estiver em um grupo de trabalho, usaremos as credenciais de destino originais. |
| 25% | Definindo a política de filtro de token de conta local no computador de destino... | Desabilita a política para que possamos nos conectar ao destino quando ela não estiver ingressada no domínio. Saiba mais sobre a política de filtro de token de conta local [aqui](https://support.microsoft.com/help/951016/description-of-user-account-control-and-remote-restrictions-in-windows).   |
| 27% | Conectando-se ao computador de destino usando a conta de usuário local temporário... |   |
| 30% | Removendo o computador de origem do domínio... |   |
| 31 | Coletando os endereços IP do computador de origem. |   Aplica-se somente a computadores de origem do Linux. |
| 33% | Reiniciando o computador de origem... (1ª reinicialização) |   |
| 36% | Aguardando o computador de origem responder após a primeira reinicialização... |   Provavelmente não responderá se o computador de origem não estiver coberto por uma sub-rede DHCP, mas você selecionou DHCP durante a configuração de rede.|
| 38% | Mapeando interfaces de rede no computador de origem... |   |
| 41% | Renomeando o computador de origem... |   |
| 42% | Reiniciando o computador de origem... (1ª reinicialização) |   Aplica-se somente a computadores de origem do Linux.|
| 43% | Reiniciando o computador de origem... (2º reinício) |   Aplica-se somente a computadores de origem do Windows Server 2003 ingressados no domínio.|
| 43% | Aguardando o computador de origem responder após a primeira reinicialização... |   |
| 43% | Aguardando o computador de origem responder após a segunda reinicialização... |   |
| 44% | Adicionando o computador de origem ao domínio... |   |
| 47% | Reiniciando o computador de origem... (1ª reinicialização) |   |
| 50% | Reiniciando o computador de origem... (2º reinício) |   |
| 51% | Reiniciando o computador de origem... (3º reinício) |   Aplica-se somente a computadores de origem do Windows Server 2003.|
| 52% | Aguardando o computador de origem responder... |   |
| 52% | Aguardando o computador de origem responder após a primeira reinicialização... |   |
| 55% | Aguardando o computador de origem responder após a segunda reinicialização... |   |
| 56% | Aguardando o computador de origem responder após a terceira reinicialização... |   |
| 57% | Removendo nomes de computador alternativos na origem... |   Garante que a origem não seja acessível a outros usuários e aplicativos. Para obter mais informações, consulte [netdom computername](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/cc835082(v=ws.11)). |
| 58% | Removendo uma conta local temporária que criamos no computador de origem... |   |
| 61% | Redefinindo a política de filtro de token de conta local no computador de origem... |   Habilita a política.|
| 63% | Removendo o computador de destino do domínio... |   |
| 66% | Reiniciando o computador de destino... (1ª reinicialização) |   |
| 69% | Aguardando o computador de destino responder após a primeira reinicialização... |   |
| 72% | Mapeando interfaces de rede no computador de destino... |  Mapeia cada adaptador de rede e endereço IP do computador de origem para o computador de destino, substituindo as informações de rede do destino.   |
| 75% | Renomeando o computador de destino... |   |
| 77% | Adicionando o computador de destino ao domínio... |  O computador de destino assume o objeto de Active Directory do computador de origem antigo. Isso poderá falhar se o usuário de destino não for um membro de admins. do domínio ou não tiver direitos de administrador para o computador de origem Active Directory objeto. Você pode especificar credenciais de destino alternativas na etapa "Inserir credenciais" antes do início da transferência.|
| 80% | Reiniciando o computador de destino... (1ª reinicialização) |   |
| 83% | Reiniciando o computador de destino... (2º reinício) |   |
| 84% | Aguardando o computador de destino responder... |   |
| 86% | Aguardando o computador de destino responder após a primeira reinicialização... |   |
| 88% | Aguardando o computador de destino responder após a segunda reinicialização... |   |
| 91% | Aguardando o computador de destino responder com o novo nome... |  Pode levar muito tempo devido a Active Directory e replicação de DNS. |
| 93% | Removendo nomes de computador alternativos no destino... |   Garante que o nome de destino foi substituído.|
| 94% | Removendo uma conta local temporária que criamos no computador de destino...|   |
| 97% | Redefinindo a política de filtro de token de conta local no computador de destino... |   Habilita a política.|
| (100%) | Com sucesso |   |

## <a name="faq"></a>Perguntas frequentes

### <a name="__is-domain-controller-migration-supported__"></a>__Há suporte para a migração do controlador de domínio?__

Não no momento, mas veja a [página de perguntas frequentes](https://docs.microsoft.com/windows-server/storage/storage-migration-service/faq#is-domain-controller-migration-supported) para obter uma solução alternativa.


## <a name="known-issues"></a>Problemas conhecidos
>Verifique se você atendeu os requisitos da [visão geral do serviço de migração de armazenamento](overview.md) e instalou o Windows Update mais recente no computador que executa o serviço de migração de armazenamento.

Consulte a [página problemas conhecidos](https://docs.microsoft.com/windows-server/storage/storage-migration-service/known-issues) para obter mais informações sobre os seguintes problemas.
* [__A validação de transferência do serviço de migração de armazenamento falha com o erro "o acesso foi negado para a política de filtro de token no computador de destino"__](https://docs.microsoft.com/windows-server/storage/storage-migration-service/known-issues#storage-migration-service-cutover-validation-fails-with-error-access-is-denied-for-the-token-filter-policy-on-destination-computer)

* [__Erro "falha na CLUSCTL_RESOURCE_NETNAME_REPAIR_VCO em relação ao recurso do NetName" e a transferência de cluster do Windows Server 2008 R2 falha__](https://docs.microsoft.com/windows-server/storage/storage-migration-service/known-issues#error-clusctl_resource_netname_repair_vco-failed-against-netname-resource-and-windows-server-2008-r2-cluster-cutover-fails)

* [__A transferência trava em "38% mapeando interfaces de rede no computador de origem..." ao usar IPs estáticos__](https://docs.microsoft.com/windows-server/storage/storage-migration-service/known-issues#cutover-hangs-on-38-mapping-network-interfaces-on-the-source-computer-when-using-static-ips)

* [__A transferência trava em "38% mapeando interfaces de rede no computador de origem..."__](https://docs.microsoft.com/windows-server/storage/storage-migration-service/known-issues#cutover-hangs-on-38-mapping-network-interfaces-on-the-source-computer)

## <a name="additional-references"></a>Referências adicionais

- [Visão geral do serviço de migração de armazenamento](overview.md)
- [Migrar um servidor de arquivos usando o serviço de migração de armazenamento](migrate-data.md)
- [Perguntas frequentes sobre serviços de migração de armazenamento](faq.md)
- [Problemas conhecidos do serviço de migração de armazenamento](known-issues.md)
