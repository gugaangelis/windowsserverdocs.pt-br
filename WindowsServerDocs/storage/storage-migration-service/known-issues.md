---
title: Problemas conhecidos do serviço de migração de armazenamento
description: Problemas conhecidos e solução de problemas de suporte para serviço de migração de armazenamento, por exemplo, como coletar logs do Microsoft Support.
author: nedpyle
ms.author: nedpyle
manager: siroy
ms.date: 02/27/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: storage
ms.openlocfilehash: f5fefab2c1b7ba8b62ffd6734217eab9a13ae95e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888867"
---
# <a name="storage-migration-service-known-issues"></a>Problemas conhecidos do serviço de migração de armazenamento

Este tópico contém respostas para problemas conhecidos ao usar [serviço de migração de armazenamento](overview.md) a migração de servidores.

## <a name="collecting-logs"></a> Como coletar arquivos de log ao trabalhar com o Microsoft Support

O serviço de migração de armazenamento contém logs de eventos para o serviço Orchestrator e o serviço de Proxy. O servidor urchestrator sempre contém os dois logs de eventos e os servidores de destino com o serviço de proxy instalado contêm os logs de proxy. Esses logs estão localizados em:

- Logs de aplicativos e serviços \ Microsoft \ Windows \ StorageMigrationService
- Logs de aplicativos e serviços \ Microsoft \ Windows \ StorageMigrationService Proxy

Se você precisar coletar esses logs para exibição offline ou para enviar ao Microsoft Support, há um script do PowerShell disponível no GitHub de software livre:

 [Auxiliar de serviço de migração de armazenamento](https://aka.ms/smslogs) 

Examine o arquivo Leiame para uso.

## <a name="storage-migration-service-doesnt-show-up-in-windows-admin-center-unless-managing-windows-server-2019"></a>Serviço de migração de armazenamento não apareça no Windows Admin Center, a menos que o gerenciamento do Windows Server 2019

Ao usar a versão de 1809 do Windows Admin Center para gerenciar um orquestrador de 2019 do Windows Server, você não vir a opção de ferramenta para o serviço de migração de armazenamento. 

A extensão de serviço de migração de armazenamento do Windows Admin Center é associado com a versão para gerenciar somente a versão do Windows Server 2019 1809 ou sistemas operacionais posteriores. Se você usá-lo para gerenciar sistemas de operacionais mais antigos do Windows Server ou visualizações insider, a ferramenta não será exibida. Esse comportamento é previsto no design. 

Para resolver, usar ou atualizar para o Windows Server 2019 build 1809 ou posterior.

## <a name="storage-migration-service-doesnt-let-you-choose-static-ip-on-cutover"></a>Serviço de migração de armazenamento não permitem que você escolha um IP estático na substituição

Ao usar a versão 0.57 do serviço de migração de armazenamento de extensão no Windows Admin Center e você atingir a fase de transferência do sistema, você não pode selecionar um endereço IP estático para um endereço. Você é forçado a usar o DHCP.

Para resolver esse problema, no do Windows Admin Center, procure **as configurações** > **extensões** para um alerta informando que o serviço de migração de armazenamento 0.57.2 a versão atualizada está disponível para instalação. Você talvez precise reiniciar o guia do navegador para o Centro de administração do Windows.

## <a name="storage-migration-service-cutover-validation-fails-with-error-access-is-denied-for-the-token-filter-policy-on-destination-computer"></a>Falha de validação de substituição de serviço de migração de armazenamento com o erro "Acesso negado para a política de filtro de token no computador de destino"

Ao executar a validação de substituição, você receberá o erro "Falha: Acesso negado para a política de filtro de token no computador de destino." Isso ocorre mesmo se você forneceu credenciais de administrador local correto para computadores de origem e de destino.

Esse problema é causado por um defeito de código no Windows Server 2019. O problema ocorrerá quando você estiver usando o computador de destino como um orquestrador de serviços de migração de armazenamento.

Para contornar esse problema, instale o serviço de migração de armazenamento em um computador Windows Server 2019 que não é o destino de migração pretendido, em seguida, se conectar ao servidor com Windows Admin Center e executar a migração.

Pretendemos corrigir esse problema em uma versão posterior do Windows Server. Abra um caso de suporte por meio [Microsoft Support](https://support.microsoft.com) para solicitar um backport dessa correção ser criado.

## <a name="storage-migration-service-isnt-included-in-windows-server-2019-evaluation-edition"></a>Serviço de migração de armazenamento não está incluído na edição de avaliação do Windows Server de 2019

Ao usar o Windows Admin Center para se conectar a um [versão de avaliação do Windows Server 2019](https://www.microsoft.com/evalcenter/evaluate-windows-server-2019) não existe uma opção para gerenciar o serviço de migração de armazenamento. Serviço de migração de armazenamento também não está incluído nas funções e recursos.

Esse problema é causado por um problema de manutenção na mídia de avaliação do Windows Server 2019. 

Para contornar esse problema, instale um varejo, a versão de licença de Volume, OEM ou MSDN do Windows Server 2019 e não ativá-lo. Sem ativação, todas as edições do Windows Server operam no modo de avaliação de 180 dias. 

Podemos corrigir esse problema em uma versão posterior do Windows Server 2019.  

## <a name="storage-migration-service-times-out-downloading-the-transfer-error-csv"></a>Serviço de migração de armazenamento atingir o tempo limite baixando o erro de transferência de CSV

Ao usar o Windows Admin Center ou PowerShell para baixar o operações detalhadas somente erros CSV log de transferência, você recebe o erro:

 >   Log de transferência - Verifique se o compartilhamento de arquivos é permitido em seu firewall. : Esta operação de solicitação enviada para baseaddress="NET.TCP://localhost:6080/vmmhelperservice/: 28940/sms/service/1/transferência não recebeu uma resposta dentro do tempo limite configurado (00: 01:00). O tempo alocado para essa operação pode ter sido uma parte de um tempo limite maior. Isso pode ser porque o serviço ainda está processando a operação ou porque o serviço não conseguiu enviar uma mensagem de resposta. Por favor, considere aumentar o tempo limite da operação (por conversão o canal/proxy para IContextChannel e definindo a propriedade OperationTimeout) e certifique-se de que o serviço é capaz de se conectar ao cliente.

Esse problema é causado por um número muito grande de arquivos transferidos que não podem ser filtrados em um minuto o tempo de limite padrão permitido pelo serviço de migração de armazenamento. 

Para contornar esse problema:

1. No computador do orchestrator, edite o *%SYSTEMROOT%\SMS\Microsoft.StorageMigration.Service.exe.config* de arquivos usando o Notepad.exe para alterar o "sendTimeout" do seu padrão de 1 minuto para 10 minutos

   ```
     <bindings>
      <netTcpBinding>
        <binding name="NetTcpBindingSms"
                 sendTimeout="00:01:00"
   ```

2. Reinicie o serviço de "Serviço de migração de armazenamento" no computador do orchestrator. 
3. No computador do orchestrator, inicie Regedit.exe
4. Localize e clique na seguinte subchave do Registro: 

   `HKEY_LOCAL_MACHINE\\Software\\Microsoft\\SMSPowershell`

5. No menu Editar, aponte para Novo e clique em Valor DWORD. 
6. Digite "WcfOperationTimeoutInMinutes" para o nome do DWORD e pressione ENTER.
7. Clique com botão direito "WcfOperationTimeoutInMinutes" e, em seguida, clique em modificar. 
8. Na caixa de Base de dados, clique em "Decimal"
9. Na caixa de dados de valor, digite "10" e, em seguida, clique em Okey.
10. Saia do Editor do registro.
11. Tentativa de baixar o arquivo CSV somente erros novamente. 

Pretendemos alterar esse comportamento em uma versão posterior do Windows Server 2019.  

## <a name="cutover-fails-when-migrating-between-networks"></a>Falha de substituição durante a migração entre redes

Ao migrar para uma VM de destino em execução em uma rede diferente que a fonte, como uma instância de IaaS do Azure, substituição não for concluída quando a fonte estava usando um endereço IP estático. 

Esse comportamento ocorre por design, para evitar problemas de conectividade após a migração de usuários, aplicativos e scripts de conexão por meio do endereço IP. Quando o endereço IP é movido do computador de origem antigo para o novo destino de destino, ele não corresponder a novas informações de sub-rede de rede e talvez o DNS e WINS.

Para solucionar esse problema é executar uma migração para um computador na mesma rede. Em seguida, mover o computador para uma nova rede e reatribuir suas informações de IP. Por exemplo, se migrando para o Azure IaaS, primeiro migrar para uma VM local e usar as migrações do Azure para deslocar a VM no Azure.  

Podemos corrigir esse problema em uma versão posterior do Windows Server 2019. Vamos agora vai permitem que você especifique as migrações que não alteram as configurações de rede do servidor de destino. Podemos pode liberar uma atualização para a versão existente do Windows Server 2019 como parte do ciclo de atualização mensal normal. 


## <a name="see-also"></a>Consulte também

- [Visão geral do serviço de migração de armazenamento](overview.md)