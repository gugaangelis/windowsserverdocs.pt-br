---
title: Problemas conhecidos da ativação do KMS
description: Descreve problemas comuns que podem ocorrer durante o processo de ativação do KMS e oferece resoluções e diretrizes
ms.topic: article
ms.date: 10/3/2019
ms.technology: server-general
ms.assetid: ''
author: Teresa-Motiv
ms.author: v-tea
manager: dcscontentpm
ms.localizationpriority: medium
ms.openlocfilehash: dab8294837a5f9116328e59364de9beb139a4b77
ms.sourcegitcommit: 9855d6b59b1f8722f39ae74ad373ce1530da0ccf
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/04/2019
ms.locfileid: "71963002"
---
# <a name="kms-activation-known-issues"></a>Ativação do KMS: problemas conhecidos

Este artigo descreve dúvidas e problemas comuns que podem surgir durante as ativações do KMS (Serviço de Gerenciamento de Chaves) e fornece diretrizes para solucionar os problemas.

> [!NOTE]
> Se você suspeita que seu problema esteja relacionado ao DNS, confira [Procedimentos comuns de solução de problemas do KMS e DNS](common-troubleshooting-procedures-kms-dns.md).

## <a name="should-i-back-up-kms-host-information"></a>Devo fazer backup das informações do host KMS?

O backup não é necessário para hosts KMS. No entanto, se você usar uma ferramenta para limpar rotineiramente logs de eventos, o histórico de ativação armazenado nos logs poderá ser perdido. Se você usar o log de eventos para rastrear ou documentar ativações do KMS, exporte periodicamente o log de eventos do Serviço de Gerenciamento de Chaves da pasta Logs de Aplicativos e Serviços do Visualizador de Eventos.

Se você usar o System Center Operations Manager, o banco de dados do System Center Data Warehouse armazenará dados do log de eventos para gerar relatórios; portanto, não é necessário fazer backup dos logs de eventos separadamente.

## <a name="is-the-kms-client-computer-activated"></a>O computador cliente KMS está ativado?

No computador cliente KMS, abra painel de controle do **Sistema** e procure a mensagem **O Windows está ativado**. Como alternativa, execute Slmgr.vbs e use a opção de linha de comando **/dli**.

## <a name="the-kms-client-computer-does-not-activate"></a>O computador cliente KMS não é ativado

Verifique se o limite de ativação do KMS foi atingido. No computador host KMS, execute Slmgr.vbs e use a opção de linha de comando **/dli** para determinar a contagem atual do host. Até que o host KMS tenha uma contagem de 25, os computadores cliente com Windows 7 não poderão ser ativados. Os clientes KMS do Windows Server 2008 R2 exigem uma contagem do KMS de 5 para ativação. Para obter mais informações sobre os requisitos do KMS, confira o [Guia de planejamento de ativação de volume](http://go.microsoft.com/fwlink/?linkid=155926). 

Abra o computador cliente KMS e examine se o Log de eventos do aplicativo tem a ID de evento 12289. Confira esse evento para obter as seguintes informações:

- É o código de resultado **0**? Qualquer outra coisa é um erro.
- O nome do host KMS no evento está correto?
- A porta KMS está correta?
- O host KMS está acessível?
- Se o cliente está executando um firewall que não é da Microsoft, a porta de saída precisa ser configurada?

No computador host KMS, examine se o log de eventos do KMS tem a ID de evento 12290. Confira esse evento para obter as seguintes informações:

- O host KMS registrou uma solicitação do computador cliente? Verifique se o nome do computador cliente KMS está listado. Verifique se o cliente e o host KMS podem se comunicar. O cliente recebeu a resposta?
- Se nenhum evento foi registrado do cliente KMS, a solicitação não atingiu o host KMS ou o host KMS não pôde processá-lo. Verifique se os roteadores não bloqueiam o tráfego usando a porta TCP 1688 (se a porta padrão foi usada) e se o tráfego com estado para o cliente KMS está permitido.

## <a name="what-does-this-error-code-mean"></a>O que esse código de erro significa?

Exceto para os eventos do KMS que têm a ID de evento 12290, o Windows registra todos os eventos de ativação no log de eventos do Aplicativo sob o nome do provedor de eventos Microsoft-Windows-Security-SPP. O Windows registra eventos do KMS no log do Serviço de Gerenciamento de Chaves na pasta Aplicativos e Serviços. Os profissionais de TI podem executar Slui.exe para exibir uma descrição da maioria dos códigos de erro relacionados à ativação. A sintaxe geral desse comando é a seguinte:

```cmd
slui.exe 0x2a ErrorCode
```

Por exemplo, se a ID de evento 12293 contiver o código de erro 0x8007267C, você poderá exibir uma descrição desse erro executando o seguinte comando:

```cmd
slui.exe 0x2a 0x8007267C
```

Para obter mais informações sobre códigos de erro específicos e como resolvê-los, confira [Resolver códigos de erro de ativação comuns](activation-error-codes.md).

## <a name="clients-are-not-adding-to-the-kms-count"></a>Os clientes não estão sendo adicionados à contagem do KMS

Para redefinir a CMID (ID do computador cliente) e outras informações de ativação do produto, execute **sysprep /generalize** ou **slmgr /rearm**. Caso contrário, cada computador cliente terá aparência idêntica e o host KMS não os contará como clientes KMS separados.

## <a name="kms-hosts-are-unable-to-create-srv-records"></a>Os hosts KMS não podem criar registros SRV

O DNS (Sistema de Nomes de Domínio) pode restringir o Acesso de gravação ou pode não dar suporte ao DDNS (DNS dinâmico). Nesse caso, permita que o Acesso de gravação do host KMS ao banco de dados DNS ou crie o SRV (serviço) RR (registro de recurso) manualmente. Para obter mais informações sobre problemas do KMS e DNS, confira [Procedimentos comuns de solução de problemas do KMS e DNS](common-troubleshooting-procedures-kms-dns.md).

## <a name="only-the-first-kms-host-is-able-to-create-srv-records"></a>Apenas o primeiro host KMS pode criar registros SRV

Se a organização tiver mais de um host KMS, os outros hosts poderão não atualizar SRV RR, a menos que as permissões padrão do SRV sejam alteradas. Para obter mais informações sobre problemas do KMS e DNS, confira [Procedimentos comuns de solução de problemas do KMS e DNS](common-troubleshooting-procedures-kms-dns.md).

## <a name="i-installed-a-kms-key-on-the-kms-client"></a>Instalei uma chave KMS no cliente KMS

As chaves KMS devem ser instaladas somente em hosts KMS, não em clientes KMS. Execute **slmgr.vbs -ipk &lt;SetupKey&gt;** . Para tabelas de chaves que você pode usar para configurar o computador como um cliente KMS, confira [chaves de instalação do cliente KMS](KMSclientkeys.md). Essas chaves são conhecidas publicamente e específicas da edição. Exclua todos os SRV RRs desnecessários do DNS e reinicie os computadores.

## <a name="a-kms-host-failed"></a>Um host KMS falhou

Se um host KMS falhar, você deverá instalar uma chave de host KMS em um novo host e ativá-lo. Verifique se o novo host KMS tem um SRV RR no banco de dados DNS. Se você instalar o host KMS usando o mesmo nome do computador e endereço IP como o host KMS com falha, o novo host KMS poderá usar o registro SRV do DNS do host com falha. Se o novo host tiver um nome do computador diferente, você poderá remover manualmente o SRV RR do DNS do host com falha ou (se a limpeza estiver habilitada no DNS) deixar o DNS removê-lo automaticamente. Se a rede estiver usando o DDNS, o novo host KMS criará automaticamente um novo SRV RR no servidor DNS. O novo host KMS inicia a coleta de solicitações de renovação do cliente e começará a ativar os clientes assim que o limite de ativação do KMS for atingido.

Se os clientes KMS usarem a descoberta automática, eles selecionarão automaticamente outro host KMS se o host KMS original não responder às solicitações de renovação. Se os clientes não usarem a descoberta automática, você deverá atualizar manualmente os computadores cliente KMS que foram atribuídos ao host KMS com falha executando **slmgr.vbs /skms**. Para evitar esse cenário, configure os clientes KMS para usar a descoberta automática. Para obter mais informações, confira o [Guia de implantação de ativação de volume](http://go.microsoft.com/fwlink/?linkid=150083).
