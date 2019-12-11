---
title: Resolver códigos de erro de ativação do Windows
description: Saiba como solucionar problemas de códigos de erro de ativação
ms.topic: article
ms.date: 9/18/2019
ms.technology: server-general
ms.assetid: ''
author: kaushika-msft
ms.author: kaushika-msft; v-tea
ms.localizationpriority: medium
ms.openlocfilehash: d4d9a8917bf455d8ed84207e2f9ecc6d13d01c3d
ms.sourcegitcommit: b18ee742662b24b25d29ef1079b1c49f220f1d57
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/02/2019
ms.locfileid: "74691870"
---
# <a name="resolve-windows-activation-error-codes"></a>Resolver códigos de erro de ativação do Windows

> **Usuários domésticos**  
> Este artigo destina-se ao uso de agentes de suporte e profissionais de TI. Caso esteja buscando mais informações sobre as mensagens de erro de ativação do Windows, confira [Obter ajuda com erros de ativação do Windows](https://support.microsoft.com/help/10738/windows-10-get-help-with-activation-errors).  

Este artigo fornece informações de solução de problemas para ajudá-lo a responder às mensagens de erro que você possa receber ao tentar usar um MAK (chave de ativação múltipla) ou o KMS (Serviço de Gerenciamento de Chaves) para executar a Ativação de Volume em um ou mais computadores baseados no Windows. Procure o código de erro na tabela a seguir e, em seguida, selecione o link para ver mais informações sobre esse código de erro e como resolvê-lo.

Para obter mais informações sobre a ativação de volume, confira [Planejar a ativação de volume](https://docs.microsoft.com/windows/deployment/volume-activation/plan-for-volume-activation-client).

Para obter mais informações sobre a ativação de volume para versões atuais e recentes do Windows, confira [Ativação de volume [cliente]](https://docs.microsoft.com/windows/deployment/volume-activation/volume-activation-windows-10).

Para obter mais informações sobre a ativação de volume para versões mais antigas do Windows, confira a KB 929712, [Informações sobre a Ativação de Volume para Windows Vista, Windows Server 2008, Windows Server 2008 R2 e Windows 7](https://support.microsoft.com/en-us/help/929712/volume-activation-information-for-windows-vista-windows-server-2008-wi).

## <a name="diagnostic-tool"></a>Ferramenta de diagnóstico

O SaRA (Assistente de Recuperação e Suporte da Microsoft) simplifica a solução de problemas de ativação do Windows KMS. Baixe a ferramenta de diagnóstico [aqui](https://aka.ms/SaRA-WindowsActivation).

Essa ferramenta tentará ativar o Windows. Se ele retornar um código de erro de ativação, a ferramenta exibirá soluções direcionadas para códigos de erro conhecidos.

Há suporte para os seguintes códigos de erro: 0xC004F038, 0xC004F039, 0xC004F041, 0xC004F074, 0xC004C008.

## <a name="summary-of-error-codes"></a>Resumo dos códigos de erro

|Código de erro |Mensagem de erro |Tipo de&nbsp;ativação|
|-----------|--------------|----------------|
|[0x8004FE21](#0x8004fe21-this-computer-is-not-running-genuine-windows) |Este computador não está executando o Windows original.  |MAK<br />Cliente KMS |
|[0x80070005](#0x80070005-access-denied) |Acesso negado A ação solicitada necessita de privilégios elevados. |MAK<br />Cliente KMS<br />Host KMS |
|[0x8007007b](#0x8007007b-dns-name-does-not-exist) |0x8007007b Nome DNS inexistente. |Cliente KMS |
|[0x80070490](#0x80070490-the-product-key-you-entered-didnt-work) |A chave do produto inserida não funcionou. Verifique a chave do produto (Product Key) e tente novamente ou insira uma diferente. |MAK |
|[0x800706BA](#0x800706ba-the-rpc-server-is-unavailable) |O servidor RPC não está disponível. |Cliente KMS |
|[0x8007232A](#0x8007232a-dns-server-failure) |Falha do servidor DNS.  |Host KMS  |
|[0x8007232B](#0x8007232b-dns-name-does-not-exist) |O nome do DNS não existe. |Cliente KMS |
|[0x8007251D](#0x8007251d-no-records-found-for-dns-query) |Nenhum registro foi encontrado para uma consulta ao DNS. |Cliente KMS |
|[0x80092328](#0x80092328-dns-name-does-not-exist) |O nome do DNS não existe.  |Cliente KMS |
|[0xC004B100](#0xc004b100-the-activation-server-determined-that-the-computer-could-not-be-activated) |O servidor de ativação determinou que a chave do produto (Product Key) não pode ser ativada. |MAK |
|[0xC004C001](#0xc004c001-the-activation-server-determined-the-specified-product-key-is-invalid) |O servidor de ativação determinou que a chave do produto especificada não é válida |MAK|
|[0xC004C003](#0xc004c003-the-activation-server-determined-the-specified-product-key-is-blocked) |O servidor de ativação determinou que a chave do produto (Product Key) especificada está bloqueada |MAK |
|[0xC004C008](#0xc004c008-the-activation-server-determined-that-the-specified-product-key-could-not-be-used) |O servidor de ativação determinou que a chave do produto (Product Key) especificada não pode ser usada. |KMS |
|[0xC004C020](#0xc004c020-the-activation-server-reported-that-the-multiple-activation-key-has-exceeded-its-limit) |O servidor de ativação relatou que a Chave de Ativação Múltipla excedeu seu limite. |MAK |
|[0xC004C021](#0xc004c021-the-activation-server-reported-that-the-multiple-activation-key-extension-limit-has-been-exceeded) |O servidor de ativação relatou que o limite de extensão da Chave de Ativação Múltipla foi excedido. |MAK |
|[0xC004F009](#0xc004f009-the-software-protection-service-reported-that-the-grace-period-expired) |O Serviço de Proteção de Software relatou que o período de cortesia expirou. |MAK |
|[0xC004F00F](#0xc004f00f-the-software-licensing-server-reported-that-the-hardware-id-binding-is-beyond-level-of-tolerance) |O Servidor de Licenciamento de Software relatou que a ID de hardware associada está além do nível de tolerância. |MAK<br />Cliente KMS<br />Host KMS |
|[0xC004F014](#0xc004f014-the-software-protection-service-reported-that-the-product-key-is-not-available) |O Serviço de Proteção de Software relatou que a chave do produto não está disponível |MAK<br />Cliente KMS |
|[0xC004F02C](#0xc004f02c-the-software-protection-service-reported-that-the-format-for-the-offline-activation-data-is-incorrect) |O Serviço de Proteção de Software relatou que o formato dos dados de ativação offline está incorreto. |MAK<br />Cliente KMS |
|[0xC004F035](#0xc004f035-invalid-volume-license-key) |O Serviço de Proteção de Software relatou que o computador não pôde ser ativado com uma chave do produto (Product Key) de licença de volume. |Cliente KMS<br />Host KMS |
|[0xC004F038](#0xc004f038-the-count-reported-by-your-key-management-service-kms-is-insufficient) |O Serviço de Proteção de Software relatou que não foi possível ativar o computador. A contagem relatada pelo KMS (Serviço de Gerenciamento de Chaves) é insuficiente. Entre em contato com o administrador do sistema. |Cliente KMS |
|[0xC004F039](#0xc004f039-the-key-management-service-kms-is-not-enabled) |O Serviço de Proteção de Software relatou que não foi possível ativar o computador. O KMS (Serviço de Gerenciamento de Chaves) não está habilitado. |Cliente KMS |
|[0xC004F041](#0xc004f041-the-software-protection-service-determined-that-the-key-management-server-kms-is-not-activated) |O Serviço de Proteção de Software determinou que o KMS (Servidor de Gerenciamento de Chaves) não está ativado. O KMS precisa ser ativado.  |Cliente KMS |
|[0xC004F042](#0xc004f042-the-software-protection-service-determined-that-the-specified-key-management-service-kms-cannot-be-used) |O Serviço de Proteção de Software concluiu que não é possível usar o KMS (Serviço de Gerenciamento de Chaves) especificado. |Cliente KMS |
|[0xC004F050](#0xc004f050-the-software-protection-service-reported-that-the-product-key-is-invalid) |O Serviço de Proteção de Software relatou que a chave do produto é inválida. |MAK<br />KMS<br />Cliente KMS |
|[0xC004F051](#0xc004f051-the-software-protection-service-reported-that-the-product-key-is-blocked) |O Serviço de Proteção de Software relatou que a chave do produto está bloqueada. |MAK<br />KMS |
|[0xC004F064](#0xc004f064-the-software-protection-service-reported-that-the-non-genuine-grace-period-expired) |O Serviço de Proteção de Software relatou que o período de cortesia não original expirou. |MAK |
|[0xC004F065](#0xc004f065-the-software-protection-service-reported-that-the-application-is-running-within-the-valid-non-genuine-period) |O Serviço de Proteção de Software relatou que o aplicativo está em execução dentro do período não original válido. |MAK<br />Cliente KMS |
|[0xC004F06C](#0xc004f06c-the-request-timestamp-is-invalid) |O Serviço de Proteção de Software relatou que não foi possível ativar o computador. O KMS (Serviço de Gerenciamento de Chaves) concluiu que o carimbo de data/hora da solicitação não é válido.  |Cliente KMS |
|[0xC004F074](#0xc004f074-no-key-management-service-kms-could-be-contacted) |O Serviço de Proteção de Software relatou que não foi possível ativar o computador. Não foi possível entrar em contato com o KMS (Serviço de Gerenciamento de Chaves). Consulte o Log de Eventos do Aplicativo para obter informações adicionais.  |Cliente KMS |

## <a name="causes-and-resolutions"></a>Causas e resoluções

### <a name="0x8004fe21-this-computer-is-not-running-genuine-windows"></a>0x8004FE21 Este computador não está executando o Windows original  

#### <a name="possible-cause"></a>Causa possível

Esse problema pode ocorrer por vários motivos. O motivo mais provável é que os MUI Packs (pacotes de idiomas) foram instalados em computadores que executam edições do Windows que não estão licenciadas para pacotes de idiomas adicionais.  

> [!NOTE]
> Esse problema não é necessariamente uma indicação de falsificação. Alguns aplicativos podem instalar o suporte multilíngue, mesmo quando essa edição do Windows não está licenciada para esses pacotes de idiomas.  

Esse problema também poderá ocorrer se o Windows for modificado por malware para permitir a instalação de recursos adicionais. Esse problema também pode ocorrer se determinados arquivos do sistema estiverem corrompidos.  

#### <a name="resolution"></a>Resolução

Para resolver esse problema, é necessário reinstalar o sistema operacional.  

### <a name="0x80070005-access-denied"></a>0x80070005 Acesso negado

O texto completo dessa mensagem de erro é semelhante ao seguinte:

> Acesso negado A ação solicitada necessita de privilégios elevados.

#### <a name="possible-cause"></a>Causa possível

O UAC (Controle de Conta de Usuário) proíbe os processos de ativação de serem executados em uma janela de prompt de comandos sem privilégios elevados.  

#### <a name="resolution"></a>Resolução

Execute **slmgr.vbs** em um prompt de comandos com privilégios elevados. Para fazer isso, no **menu Iniciar**, clique com o botão direito do mouse em **cmd.exe** e, em seguida, selecione **Executar como administrador**.  

### <a name="0x8007007b-dns-name-does-not-exist"></a>0x8007007b Nome DNS inexistente

#### <a name="possible-cause"></a>Causa possível

Esse problema poderá ocorrer se o cliente KMS não puder localizar os registros de recurso de serviços do KMS no DNS.  

#### <a name="resolution"></a>Resolução

Para obter mais informações sobre como solucionar esses problemas relacionados ao DNS, confira [Procedimentos comuns de solução de problemas do KMS e DNS](common-troubleshooting-procedures-kms-dns.md).  

### <a name="0x80070490-the-product-key-you-entered-didnt-work"></a>0x80070490 A chave do produto (Product Key) inserida não funcionou

O texto completo desse erro é semelhante ao seguinte:
> A chave do produto (Product Key) inserida não funcionou. Verifique a chave do produto (Product Key) e tente novamente ou insira uma diferente.  

#### <a name="possible-cause"></a>Causa possível

Esse problema ocorre porque o MAK inserido não era válido ou devido a um problema conhecido no Windows Server 2019.  

#### <a name="resolution"></a>Resolução

Para solucionar esse problema e ativar o computador, execute **slmgr -ipk <chave 5x5>** em um prompt de comandos com privilégios elevados.

### <a name="0x800706ba-the-rpc-server-is-unavailable"></a>0x800706BA O servidor RPC está indisponível

#### <a name="possible-cause"></a>Causa possível

As configurações do firewal não foram definidas no host KMS ou os registros de SRV do DNS estão obsoletos.  

#### <a name="resolution"></a>Resolução

No host KMS, verifique se uma exceção de firewall está habilitada para o Serviço de Gerenciamento de Chaves (porta TCP 1688).

Verifique se os registros SRV do DNS apontam para um host KMS válido. 

Solucione os problemas da rede.  

Para obter mais informações sobre como solucionar esses problemas relacionados ao DNS, confira [Procedimentos comuns de solução de problemas do KMS e DNS](common-troubleshooting-procedures-kms-dns.md).  

### <a name="0x8007232a-dns-server-failure"></a>0x8007232A Falha do servidor DNS

#### <a name="possible-cause"></a>Causa possível

O sistema tem problemas na rede ou no DNS.

#### <a name="resolution"></a>Resolução

Solucione os problemas na rede e no DNS.  

### <a name="0x8007232b-dns-name-does-not-exist"></a>0x8007232B Nome DNS inexistente

#### <a name="possible-cause"></a>Causa possível

O cliente KMS não pode encontrar registros de recurso de servidor KMS (SRV RRs) no DNS.  

#### <a name="resolution"></a>Resolução

Verifique se um host KMS foi instalado e se a publicação do DNS está habilitada (padrão). Se o DNS não estiver disponível, aponte o cliente KMS para o host KMS usando **slmgr.vbs /skms <*nome_do_host_KMS*>** .  

Caso não tenha um host KMS, obtenha e instale um MAK. Em seguida, ative o sistema.

Para obter mais informações sobre como solucionar esses problemas relacionados ao DNS, confira [Procedimentos comuns de solução de problemas do KMS e DNS](common-troubleshooting-procedures-kms-dns.md).  

### <a name="0x8007251d-no-records-found-for-dns-query"></a>0x8007251D Nenhum registro encontrado para a consulta DNS

#### <a name="possible-cause"></a>Causa possível

O cliente KMS não pode encontrar os registros SRV do KMS no DNS.

#### <a name="resolution"></a>Resolução

Solucione os problemas de conexão da rede e do DNS. Para obter mais informações sobre como solucionar problemas relacionados ao DNS, confira [Procedimentos comuns de solução de problemas do KMS e DNS](common-troubleshooting-procedures-kms-dns.md).  

### <a name="0x80092328-dns-name-does-not-exist"></a>0x80092328 Nome DNS inexistente

#### <a name="possible-cause"></a>Causa possível

Esse problema poderá ocorrer se o cliente KMS não puder localizar os registros de recurso de serviços do KMS no DNS.

#### <a name="resolution"></a>Resolução

Para obter mais informações sobre como solucionar esses problemas relacionados ao DNS, confira [Procedimentos comuns de solução de problemas do KMS e DNS](common-troubleshooting-procedures-kms-dns.md).  

### <a name="0xc004b100-the-activation-server-determined-that-the-computer-could-not-be-activated"></a>0xC004B100 O servidor de ativação determinou que a chave do produto (Product Key) não pode ser ativada

#### <a name="possible-cause"></a>Causa possível

Não há suporte para o MAK.  

#### <a name="resolution"></a>Resolução

Para solucionar esse problema, verifique se o MAK que está sendo usado é o MAK fornecido pela Microsoft. Para verificar se o MAK é válido, contate os [Centros de Ativação de Licenciamento da Microsoft](https://www.microsoft.com/en-us/Licensing/existing-customer/activation-centers).

### <a name="0xc004c001-the-activation-server-determined-the-specified-product-key-is-invalid"></a>0xC004C001 O servidor de ativação determinou que a chave do produto (Product Key) especificada é inválida

#### <a name="possible-cause"></a>Causa possível

O MAK inserido não é válido.

#### <a name="resolution"></a>Resolução

Verifique se a chave é o MAK que foi fornecido pela Microsoft. Para obter assistência adicional, contate os [Centros de Ativação de Licenciamento da Microsoft](https://www.microsoft.com/en-us/Licensing/existing-customer/activation-centers).

### <a name="0xc004c003-the-activation-server-determined-the-specified-product-key-is-blocked"></a>0xC004C003 O servidor de ativação determinou que a chave do produto (Product Key) especificada está bloqueada

#### <a name="possible-cause"></a>Causa possível

A MAK encontra-se bloqueada no servidor de ativação.

#### <a name="resolution"></a>Resolução

Para obter um novo MAK, contate os [Centros de Ativação de Licenciamento da Microsoft](https://www.microsoft.com/en-us/Licensing/existing-customer/activation-centers). Depois de obter o novo MAK, tente instalar e ativar o Windows novamente.  

### <a name="0xc004c008-the-activation-server-determined-that-the-specified-product-key-could-not-be-used"></a>0xC004C008 O servidor de ativação determinou que a chave do produto (Product Key) especificada não pode ser usada

#### <a name="possible-cause"></a>Causa possível

A chave KMS excedeu o limite de ativação. Uma chave do host KMS pode ser ativada até 10 vezes em até seis computadores diferentes.  

#### <a name="resolution"></a>Resolução

Caso precise de ativações adicionais, contate os [Centros de Ativação de Licenciamento da Microsoft](https://www.microsoft.com/en-us/Licensing/existing-customer/activation-centers).  

### <a name="0xc004c020-the-activation-server-reported-that-the-multiple-activation-key-has-exceeded-its-limit"></a>0xC004C020 O servidor de ativação relatou que a chave de ativação múltipla excedeu o limite

#### <a name="possible-cause"></a>Causa possível

O MAK excedeu o limite de ativação. Por design, os MAKs podem ser ativados um número limitado de vezes.

#### <a name="resolution"></a>Resolução

Caso precise de ativações adicionais, contate os [Centros de Ativação de Licenciamento da Microsoft](https://www.microsoft.com/en-us/Licensing/existing-customer/activation-centers).

### <a name="0xc004c021-the-activation-server-reported-that-the-multiple-activation-key-extension-limit-has-been-exceeded"></a>0xC004C021 O servidor de ativação relatou que o limite de extensão da chave de ativação múltipla foi excedido

#### <a name="possible-cause"></a>Causa possível

O MAK excedeu o limite de ativação. Por design, os MAKs são ativados um número limitado de vezes.

#### <a name="resolution"></a>Resolução

Caso precise de ativações adicionais, contate os [Centros de Ativação de Licenciamento da Microsoft](https://www.microsoft.com/en-us/Licensing/existing-customer/activation-centers).

### <a name="0xc004f009-the-software-protection-service-reported-that-the-grace-period-expired"></a>0xC004F009 O Serviço de Proteção de Software relatou que o período de cortesia expirou

#### <a name="possible-cause"></a>Causa possível

O período de cortesia expirou antes do sistema ser ativado. Agora, o sistema está em estado de Notificações.  

#### <a name="resolution"></a>Resolução

Para obter assistência, contate os [Centros de Ativação de Licenciamento da Microsoft](https://www.microsoft.com/en-us/Licensing/existing-customer/activation-centers).

### <a name="0xc004f00f-the-software-licensing-server-reported-that-the-hardware-id-binding-is-beyond-level-of-tolerance"></a>0xC004F00F O Servidor de Licenciamento de Software relatou que a ID de hardware associada está além do nível de tolerância

#### <a name="possible-cause"></a>Causa possível

O hardware foi alterado ou os drivers foram atualizadas no sistema.  

#### <a name="resolution"></a>Resolução

Caso esteja usando a ativação MAK, use a ativação online ou por telefone para reativar o sistema durante o período de cortesia sem tolerância.  

Caso esteja usando a ativação KMS, reinicie o Windows ou execute **slmgr.vbs /ato**.

### <a name="0xc004f014-the-software-protection-service-reported-that-the-product-key-is-not-available"></a>0xC004F014 O Serviço de Proteção de Software relatou que a chave do produto (Product Key) não está disponível

#### <a name="possible-cause"></a>Causa possível

Não há chaves de produtos instaladas no sistema.  

#### <a name="resolution"></a>Resolução

Caso esteja usando a ativação MAK, instale uma chave do produto (Product Key) MAK. 

Caso esteja usando a ativação KMS, verifique o arquivo Pid.txt (localizado na mídia de instalação na pasta \sources) para obter uma chave de instalação do KMS. Instale a chave.

### <a name="0xc004f02c-the-software-protection-service-reported-that-the-format-for-the-offline-activation-data-is-incorrect"></a>0xC004F02C O Serviço de Proteção de Software relatou que o formato dos dados de ativação offline está incorreto

#### <a name="possible-cause"></a>Causa possível

O sistema detectou que os dados digitados durante a ativação por telefone não são válidos.  

#### <a name="resolution"></a>Resolução

Verifique se o CID foi inserido corretamente.  

### <a name="0xc004f035-invalid-volume-license-key"></a>0xC004F035 Chave de licença de volume inválida

O texto completo dessa mensagem de erro é semelhante ao seguinte:

> Erro: Chave de licença de volume inválida. Para ativá-la, você precisará alterar a chave do produto (Product Key) para uma chave comercial ou um MAK (chave de ativação múltipla) válido. É necessário ter uma licença de sistema operacional qualificado E uma licença de atualização do Windows 7 de licença de volume ou uma licença integral do Windows 7 adquirida no varejo. QUALQUER OUTRA INSTALAÇÃO DESTE SOFTWARE VIOLA O CONTRATO E AS LEIS DE DIREITOS AUTORAIS APLICÁVEIS.  

O texto do erro está correto, mas é ambíguo. Esse erro indica que o computador não tem um marcador do Windows em seu BIOS que o identifica como um sistema OEM que está executando uma edição de qualificação do Windows. Essas informações são obrigatórias para a ativação do cliente KMS. O significado mais específico desse código é “Erro: Chave de licença de volume inválida”

#### <a name="possible-cause"></a>Causa possível

As edições de volume do Windows 7 são licenciadas somente para atualização. A Microsoft não dá suporte para a instalação de um sistema operacional de volume em um computador que não tenha um sistema operacional qualificado instalado.  

#### <a name="resolution"></a>Resolução

Para ativar, é necessário fazer o seguinte:

- Altere a chave do produto (Product Key) para uma chave comercial ou MAK (chave de ativação múltipla) válida. É necessário ter uma licença de sistema operacional qualificado E uma licença de atualização do Windows 7 de licença de volume ou uma licença integral do Windows 7 adquirida no varejo.
  > [!NOTE]
  > Se você receber o erro 0x80072ee2 quando tentar ativar, use o método de ativação por telefone a seguir.
- Ative por telefone seguindo estas etapas:
   1. Execute **slmgr /dti** e registre o valor da ID da Instalação. </li>
   1. Entre em contato com os [Centros de Ativação de Licenciamento da Microsoft](https://www.microsoft.com/en-us/Licensing/existing-customer/activation-centers) e forneça a ID de Instalação para receber uma ID de Confirmação.</li>
   1. Para ativar usando a ID de Confirmação, execute **slmgr /atp &lt;ID de Confirmação&gt;** .

### <a name="0xc004f038-the-count-reported-by-your-key-management-service-kms-is-insufficient"></a>0xC004F038 A contagem relatada pelo KMS (Serviço de Gerenciamento de Chaves) é insuficiente

O texto completo dessa mensagem de erro é semelhante ao seguinte:

> O Serviço de Proteção de Software relatou que não foi possível ativar o computador. A contagem relatada pelo KMS (Serviço de Gerenciamento de Chaves) é insuficiente. Entre em contato com o administrador do sistema.  

#### <a name="possible-cause"></a>Causa possível

A contagem no host KMS não é alta o bastante. Para o Windows Server, a contagem do KMS precisa ser superior ou igual a 5. Para o Windows (cliente), a contagem do KMS precisa ser superior ou igual a 25.  

#### <a name="resolution"></a>Resolução
Para usar o KMS para ativar o Windows, você precisará ter mais computadores no pool do KMS. Para obter a contagem atual no host KMS, execute **Slmgr.vbs /dli**.  

### <a name="0xc004f039-the-key-management-service-kms-is-not-enabled"></a>0xC004F039 O KMS (Serviço de Gerenciamento de Chaves) não está habilitado

O texto completo dessa mensagem de erro é semelhante ao seguinte:

> O Serviço de Proteção de Software relatou que não foi possível ativar o computador. O KMS (Serviço de Gerenciamento de Chaves) não está habilitado.  

#### <a name="possible-cause"></a>Causa possível

O KMS não respondeu à solicitação do KMS.

#### <a name="resolution"></a>Resolução

Solucione o problema de conexão da rede entre o host KMS e o cliente. Verifique se a porta TCP 1688 (padrão) não está bloqueada por um firewall ou se ela foi filtrada de outro modo.

### <a name="0xc004f041-the-software-protection-service-determined-that-the-key-management-server-kms-is-not-activated"></a>0xC004F041 O Serviço de Proteção de Software determinou que o KMS (Servidor de Gerenciamento de Chaves) não está ativado

O texto completo dessa mensagem de erro é semelhante ao seguinte:

> O Serviço de Proteção de Software determinou que o KMS (Servidor de Gerenciamento de Chaves) não está ativado. O KMS precisa ser ativado.  

#### <a name="possible-cause"></a>Causa possível

O host KMS não está ativado.  

#### <a name="resolution"></a>Resolução

Ative o host KMS usando a ativação online ou por telefone.  

### <a name="0xc004f042-the-software-protection-service-determined-that-the-specified-key-management-service-kms-cannot-be-used"></a>0xC004F042 O Serviço de Proteção de Software determinou que o KMS (Serviço de Gerenciamento de Chaves) especificado não pode ser usado

#### <a name="possible-cause"></a>Causa possível

Esse erro ocorre quando um cliente KMS contata um host KMS que não pôde ativar o software cliente. Isso pode ser comum em ambientes mistos que contêm hosts KMS específicos de aplicativo e do sistema operacional, por exemplo.  

#### <a name="resolution"></a>Resolução

Verifique se, caso você use hosts KMS específicos para ativar aplicativos ou sistemas operacionais específicos, os clientes KMS se conectam aos hosts corretos.

### <a name="0xc004f050-the-software-protection-service-reported-that-the-product-key-is-invalid"></a>0xC004F050 O Serviço de Proteção de Software relatou que a chave do produto (Product Key) é inválida

#### <a name="possible-cause"></a>Causa possível

Isso pode ser causado por um erro de digitação na chave KMS ou ao digitar uma chave Beta em uma versão lançada do sistema operacional.  

#### <a name="resolution"></a>Resolução

Instale a chave KMS adequada na versão do Windows correspondente. Verifique a ortografia. Se a chave estiver sendo copiada e colada, verifique se os travessões não foram substituídos pelos hifens na chave.  

### <a name="0xc004f051-the-software-protection-service-reported-that-the-product-key-is-blocked"></a>0xC004F051 O Serviço de Proteção de Software relatou que a chave do produto (Product Key) está bloqueada

#### <a name="possible-cause"></a>Causa possível

O servidor de ativação determinou que a Microsoft bloqueou a chave do produto (Product Key).  

#### <a name="resolution"></a>Resolução

Obtenha uma nova chave MAK ou KMS, instale-a no sistema e ative-a.

### <a name="0xc004f064-the-software-protection-service-reported-that-the-non-genuine-grace-period-expired"></a>0xC004F064 O Serviço de Proteção de Software relatou que o período de cortesia não original expirou

#### <a name="possible-cause"></a>Causa possível

O WAT (Windows Activation Tools) determinou que o sistema não é original.  

#### <a name="resolution"></a>Resolução

Para obter assistência, contate os [Centros de Ativação de Licenciamento da Microsoft](https://www.microsoft.com/en-us/Licensing/existing-customer/activation-centers).

### <a name="0xc004f065-the-software-protection-service-reported-that-the-application-is-running-within-the-valid-non-genuine-period"></a>0xC004F065 O Serviço de Proteção de Software relatou que o aplicativo está em execução dentro do período não original válido

#### <a name="possible-cause"></a>Causa possível

O Windows Activation Tools determinou que o sistema não é original. O sistema continuará sendo executado durante o período de cortesia não original.  

#### <a name="resolution"></a>Resolução

Obtenha e instale uma chave do produto (Product Key) original e ative o sistema durante o período de cortesia. Caso contrário, o sistema entrará no estado de Notificações ao término do período de cortesia.

### <a name="0xc004f06c-the-request-timestamp-is-invalid"></a>0xC004F06C O carimbo de data/hora da solicitação é inválido

O texto completo dessa mensagem de erro é semelhante ao seguinte:

> O Serviço de Proteção de Software relatou que não foi possível ativar o computador. O KMS (Serviço de Gerenciamento de Chaves) concluiu que o carimbo de data/hora da solicitação não é válido.  

#### <a name="possible-cause"></a>Causa possível

O horário do sistema no computador cliente está muito diferente do horário no host KMS. A sincronização é importante para a segurança do sistema e da rede por uma série de motivos.  

#### <a name="resolution"></a>Resolução

Corrija esse problema alterando a hora do sistema no cliente para sincronizá-la com o host KMS. Recomendamos que você use uma fonte de tempo do protocolo NTP ou o Active Directory Domain Services para a sincronização de hora. Esse problema usa o horário UTP e não depende da seleção de fuso horário.  

### <a name="0xc004f074-no-key-management-service-kms-could-be-contacted"></a>0xC004F074 Não foi possível contatar o KMS (Serviço de Gerenciamento de Chaves)

O texto completo dessa mensagem de erro é semelhante ao seguinte:

> O Serviço de Proteção de Software relatou que não foi possível ativar o computador. Não foi possível entrar em contato com o KMS (Serviço de Gerenciamento de Chaves). Consulte o Log de Eventos do Aplicativo para obter informações adicionais.  

#### <a name="possible-cause"></a>Causa possível

Todos os sistemas do host KMS retornaram um erro.  

#### <a name="resolution"></a>Resolução

No Log de Eventos do Aplicativo, identifique cada evento que tenha a ID do Evento 12288 e que esteja associado à tentativa de ativação. Solucione os erros desses eventos.

Para obter mais informações sobre como solucionar problemas relacionados ao DNS, confira [Procedimentos comuns de solução de problemas do KMS e DNS](common-troubleshooting-procedures-kms-dns.md).  
