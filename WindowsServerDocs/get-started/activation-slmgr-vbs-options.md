---
title: Opções Slmgr.vbs para obter informações de ativação de volume
description: Lista as opções disponíveis para o script Slmgr.vbs e descreve como usá-las
ms.date: 09/24/2019
ms.topic: article
author: Teresa-Motiv
ms.author: v-tea
manager: dcscontentpm
ms.localizationpriority: medium
appliesto:
- Windows Server 2012 R2
- Windows 10
- Windows 8.1
ms.openlocfilehash: c461c04a0ce95cb4b4228462896510a2bbbd8575
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87941748"
---
# <a name="slmgrvbs-options-for-obtaining-volume-activation-information"></a>Opções Slmgr.vbs para obter informações de ativação de volume

Veja a seguir a descrição da sintaxe do script Slmgr.vbs. As tabelas neste artigo descrevem cada opção de linha de comando.

```cmd
slmgr.vbs [<ComputerName> [<User> <Password>]] [<Options>]
```

> [!NOTE]
> Neste artigo, colchetes \[] incluem argumentos opcionais e colchetes angulares \<> incluem espaços reservados. Ao digitar essas instruções, omita os colchetes e substitua os espaços reservados usando valores correspondentes.

> [!NOTE]
> Para obter informações sobre outros produtos de software que usam a ativação de volume, confira os documentos específicos de cada um desses aplicativos.

## <a name="using-slmgr-on-remote-computers"></a>Usando Slmgr em computadores remotos

Para gerenciar clientes remotos, use a VAMT (Ferramenta de Gerenciamento de Ativação de Volume), versão 1.2 ou posterior, ou crie scripts WMI personalizados que considerem as diferenças entre as plataformas. Para saber mais sobre as propriedades da WMI e os métodos para ativação de volume, confira [Propriedades e métodos da WMI para ativação de volume](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn502536(v=ws.11)).

> [!IMPORTANT]
> Por causa de mudanças na WMI no Windows 7 e Windows Server 2008 R2, o script Slmgr.vbs não serve para funcionar entre plataformas. Não há suporte para o uso de Slmgr.vbs com o intuito de gerenciar um sistema Windows 7 ou Windows Server 2008 R2 do sistema operacional Windows Vista&reg;. Se você tentar gerenciar um sistema mais antigo do Windows 7 ou Windows Server 2008 R2, isso gerará um erro específico de incompatibilidade de versão. Por exemplo, executar o **cscript slmgr.vbs \<vista\_machine\_name\> /dlv** produz o seguinte resultado:
>
>> Microsoft (R) Windows Script Host versão 5.8 Copyright (C) Microsoft Corporation. Todos os direitos reservados.
>>
>> O computador remoto não dá suporte a esta versão do SLMgr.vbs

## <a name="general-slmgrvbs-options"></a>Opções gerais do Slmgr.vbs

|Opção |Descrição |
| - | - |
|\[\<ComputerName\>] |Nome de um computador remoto (o padrão é o computador local) |
|\[\<User\>] |Conta no computador remoto com o privilégio pedido |
|\[\<Password\>] |Senha da conta no computador remoto com os privilégios pedidos |

## <a name="global-options"></a>Opções globais

|Opção |Descrição |
| - | - |
|\/ipk&nbsp;&lt;ProductKey&gt; |Tenta instalar uma chave do produto (Product Key) 5×5. A chave de produto fornecida pelo parâmetro é confirmada como válida e aplicável para o sistema operacional instalado.<br />Se não, será retornado um erro.<br />Se a chave for válida e aplicável, a chave será instalada. Se uma chave já estiver instalada, ela será substituída silenciosamente.<br />Para evitar instabilidade no serviço de licenças, o sistema ou o Serviço de Proteção de Software deverá ser reiniciado.<br />A operação deverá ser executada de uma janela de prompt de comandos com privilégios elevados ou o valor de Registro das Operações do Usuário Padrão deverá estar configurado de modo a permitir que usuários sem privilégios tenham acesso extra ao Serviço de Proteção de Software. |
|/ato&nbsp;\[\<Activation&nbsp;ID\>] |Em edições de varejo e sistemas de volume com uma chave de host KMS (Serviço de Gerenciamento de Chaves) ou uma chave MAK (chave de ativação múltipla), o **/ato** solicitará ao Windows uma tentativa de ativação online.<br />Para sistemas com uma GVLK (chave de licença de volume genérico) instalada, isso solicitará uma tentativa de ativação KMS. Os sistemas que tiverem sido configurados para suspender tentativas automáticas de ativação do KMS ( **/stao**) ainda tentarão realizar a ativação KMS quando **/ato** for executado.<br />**Observação**: No Windows 8 (e Windows Server 2012), a opção **/stao** foi preterida. Use a opção **/act-type** como substituta.<br />O parâmetro \<**Activation ID**\> expande o suporte a **/ato** para identificar uma edição do Windows instalada no computador. Especificar o parâmetro \<**Activation ID**\> isolará os efeitos da opção na edição associada à ID de ativação indicada. Execute **slmgr.vbs /dlv all** para obter os IDs de ativação da versão instalada do Windows. Se você precisar de suporte para outros aplicativos, confira as orientações fornecidas sobre cada aplicativo para ver mais instruções.<br />A ativação KMS não pede privilégios elevados. Mas a ativação online pede elevação, ou o valor de registro das Operações do Usuário Padrão deverá estar configurado de modo a permitir que usuários sem privilégios tenham acesso extra ao Serviço de Proteção de Software. |
|\/dli&nbsp;\[<ID de&nbsp;ativação\>&nbsp;\|&nbsp;All\] |Exibe informações sobre a licença.<br />Por padrão, **/dli** exibe as informações sobre a licença da edição do Windows ativa e instalada. Especificar o parâmetro \<**Activation ID**\> exibirá as informações sobre a licença da edição especificada associada à ID de ativação indicada. Especificar **All** como parâmetro exibirá as informações sobre a licença de todos os produtos aplicáveis instalados.<br />A operação não pede privilégios elevados. |
|\/dlv&nbsp;\[<ID de&nbsp;ativação\>&nbsp;\|&nbsp;All\] |Exibir informações detalhadas sobre a licença.<br />Por padrão, **/dlv** exibe as informações sobre a licença do sistema operacional instalado. Especificar o parâmetro \<**Activation ID**\> exibirá as informações sobre a licença da edição especificada associada à ID de ativação indicada. Especificar o parâmetro **All** exibirá as informações sobre a licença de todos os produtos aplicáveis instalados.<br />A operação não pede privilégios elevados. |
|\/xpr \[\<Activation&nbsp;ID\>] |Exibe a data de expiração da ativação do produto. Está, por padrão, relacionada à edição atual do Windows e é útil principalmente para clientes KMS, pois as ativações MAK e de varejo são permanentes.<br />Especificar o parâmetro \<**Activation ID**\> exibirá a data de término da ativação da edição especificada associada à ID de ativação. Essa operação não pede privilégios elevados. |

## <a name="advanced-options"></a>Opções avançadas

|Opção |Descrição |
| - | - |
|\/cpky |Algumas operações de manutenção pede que a chave de produto esteja disponível no Registro durante operações OOBE (configuração inicial pelo usuário). A opção **/cpky** remove a chave de produto do Registro para evitar que seja roubada por códigos mal-intencionados.<br />Para instalações de varejo que implantem chaves, a prática recomendada é executar essa opção. A opção não é necessária para chaves de host KMS e chaves MAK, pois esse já é o comportamento padrão dessas chaves. A opção é pedida para outros tipos de chave, cujo comportamento padrão não é apagar a chave do Registro.<br />A operação deve ser executada em uma janela do prompt de comandos com privilégios elevados. |
|\/ilc&nbsp;&lt;license_file&gt; |Esta opção instala o arquivo de licença especificado pelo parâmetro pedido. Essas licenças podem ser instaladas como medida de solução de problemas, a fim de dar suporte à ativação baseada em token, ou como parte da instalação manual de um aplicativo onboard.<br />As licenças não são validadas durante esse processo: a validação de licenças está fora do escopo do Slmgr.vbs. Em vez disso, a validação é executada pelo Serviço de Proteção de Software, em runtime.<br />A operação deverá ser executada de uma janela de prompt de comandos com privilégios elevados ou o valor de Registro das **Operações do Usuário Padrão** deverá estar configurado de modo a permitir que usuários sem privilégios tenham acesso extra ao Serviço de Proteção de Software. |
|\/rilc |Esta opção reinstala todas as licenças armazenadas em %SystemRoot%\system32\oem e %SystemRoot%\System32\spp\tokens. Elas são cópias "em boas condições" armazenadas durante a instalação.<br />As licenças correspondentes no armazenamento confiável são substituídas. As licenças adicionais – por exemplo, ILs (licenças de publicação) de TAs (autoridades confiáveis) e licenças para aplicativos – não são afetadas.<br />A operação deverá ser executada em uma janela de prompt de comandos com privilégios elevados ou o valor de Registro das **Operações do Usuário Padrão** deverá estar configurado de modo a permitir que usuários sem privilégios tenham acesso extra ao Serviço de Proteção de Software. |
|\/rearm |Esta opção redefine os temporizadores de ativação. O processo **/rearm** também é chamado por **sysprep /generalize**.<br />Esta operação não fará nada se a entrada do Registro **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\SoftwareProtectionPlatform\SkipRearm** for definida como **1**. Confira [Configurações do Registro para ativação de volume](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn502532(v=ws.11)) para obter detalhes sobre essa entrada do Registro.<br />A operação deverá ser executada em uma janela de prompt de comandos com privilégios elevados ou o valor de Registro das **Operações do Usuário Padrão** deverá estar configurado de modo a permitir que usuários sem privilégios tenham acesso extra ao Serviço de Proteção de Software. |
|\/rearm-app &lt;ID do&nbsp;Aplicativo&gt; |Redefine o status de licença do aplicativo especificado. |
|\/rearm-sku &lt;ID do&nbsp;Aplicativo&gt; |Redefine o status de licença do SKU (unidade de manutenção de estoque) especificado. |
|\/upk&nbsp;\[&lt;ID do&nbsp;Aplicativo&gt;] |Esta opção desinstala a chave de produto da edição atual do Windows. Depois de ser reinicializado, o sistema estará no estado não licenciado até que uma nova chave de produto seja instalada.<br />Também há a opção de usar o parâmetro \<**Activation ID**\> para especificar um produto diferente instalado.<br />A operação deve ser executada em uma janela de prompt de comandos com privilégios elevados. |
|\/dti&nbsp;\[\<Activation&nbsp;ID\>] |Exibe o ID de instalação para a ativação offline. |
|\/atp &lt;ID de&nbsp;confirmação&gt; |Ativa o produto usando a ID de confirmação fornecida pelo usuário. |

## <a name="kms-client-options"></a>opções do cliente KMS

|Opção |Descrição |
| - | - |
|\/skms \<Name\[:Port]&nbsp;\|&nbsp;\:&nbsp;port\> \[\<Activation&nbsp;ID\>] |Esta opção especifica o nome e opcionalmente a porta do computador host KMS a ser contatado. Definir este valor desativa a detecção automática do host KMS.<br />Se o host KMS usar apenas o IPv6 (protocolo IP versão 6), o endereço precisará ser especificado no formato \<hostname\>:\<port\>. Os endereços IPv6 contêm dois-pontos (:), que o script Slmgr.vbs não interpreta corretamente.<br />A operação deve ser executada em uma janela do prompt de comandos com privilégios elevados. |
|\/skms-domain&nbsp;&lt;FQDN&gt; \[\<Activation&nbsp;ID\>] |Define o domínio DNS específico no qual encontram-se todos os registros SRV do KMS. A configuração não terá qualquer efeito se o host KMS único e específico for definido usando a opção **/skms**. Use esta opção, especialmente em ambientes com namespace não contíguo, para forçar o KMS a ignorar a lista de pesquisa de sufixos de DNS e procurar registros do host KMS no domínio DNS especificado. |
|\/ckms&nbsp;\[\<Activation&nbsp;ID\>] |Esta opção remove, do Registro, as informações sobre o nome, endereço e porta do host KSM especificado e restaura o comportamento de descoberta automática do KMS.<br />A operação deve ser executada em uma janela do prompt de comandos com privilégios elevados. |
|\/skhc |Esta opção habilita o armazenamento em cache do host KMS (padrão). Depois que o cliente descobre um host KMS em funcionamento, essa configuração impede que a prioridade e a ponderação do DNS (Sistema de Nomes de Domínio) afetem a comunicação adicional com o host. Se o sistema não puder mais entrar em contato com o host KMS em funcionamento, o cliente tentará descobrir um novo host.<br />A operação deve ser executada em uma janela do prompt de comandos com privilégios elevados. |
|\/ckhc |Esta opção desabilita o armazenamento em cache do host KMS. Esta configuração instrui o cliente a usar a descoberta automática de DNS cada vez que ele tentar uma ativação KMS (recomendada ao usar prioridade e ponderação).<br />A operação deve ser executada em uma janela do prompt de comandos com privilégios elevados. |

## <a name="kms-host-configuration-options"></a>Opções de configuração do host KMS

|Opção |Descrição |
| - | - |
|\/sai&nbsp;&lt;Interval&gt; |Esta opção define um intervalo em minutos para clientes não ativados tentarem a conexão com o KMS. O intervalo de ativação deve estar entre 15 minutos e 30 dias, porém o valor padrão (duas horas) é recomendado.<br />Inicialmente, o cliente KMS adota esse intervalo do Registro, mas volta para a configuração KMS depois de receber a primeira resposta do KMS.<br />A operação deve ser executada em uma janela do prompt de comandos com privilégios elevados. |
|\/sri &lt;Interval&gt; |Esta opção define o intervalo de renovação, em minutos, para que os clientes ativados tentem a conexão com o KMS. O intervalo de renovação deve estar entre 15 minutos e 30 dias. Inicialmente, esta opção é definida tanto no servidor como no cliente KMS. O valor padrão é 10.080 segundos (7 dias).<br />Inicialmente, o cliente KMS adota esse intervalo do registro, mas troca para a configuração do KMS depois de receber a primeira resposta do KMS.<br />A operação deve ser executada em uma janela do prompt de comandos com privilégios elevados. |
|\/sprt&nbsp;&lt;Porta&gt; |Esta opção define a porta na qual o host KMS escutará as solicitações de ativação do cliente. A porta TCP padrão é 1688.<br />A operação deve ser executada em uma janela de prompt de comandos com privilégios elevados. |
|\/sdns |Habilita a publicação de DNS pelo host KMS (padrão).<br />A operação deve ser executada em uma janela do prompt de comandos com privilégios elevados. |
|\/cdns |Desabilita a publicação de DNS pelo host KMS.<br />A operação deve ser executada em uma janela do prompt de comandos com privilégios elevados. |
|\/spri |Define a prioridade do KMS como normal (padrão).<br />A operação deve ser executada em uma janela do prompt de comandos com privilégios elevados. |
|\/cpri |Define a prioridade do KMS como baixa.<br />Use esta opção para minimizar a contenção do KMS em um ambiente de host articulado. Observe que isso pode resultar na privação do KMS, dependendo de quais outros aplicativos ou funções do servidor estiverem ativos. Use com cautela.<br />A operação deve ser executada em uma janela do prompt de comandos com privilégios elevados. |
|\/act-type \[\<Activation-Type\>] \[\<Activation&nbsp;ID\>] |Esta opção define um valor no Registro que limita a ativação de volume a um único tipo. O tipo de ativação **1** limita a ativação apenas ao Active Directory, **2** limita a ativação à ativação do KMS e **3** à ativação baseada em token. A opção **0** permite qualquer tipo de ativação e é o valor padrão. |

## <a name="token-based-activation-configuration-options"></a>Opções de configuração de ativação baseada em token

|Opção |Descrição |
| - | - |
|/lil |Lista as licenças de publicação de ativação instaladas baseadas em token. |
|\/ril&nbsp;&lt;ILID&gt;&nbsp;&lt;ILvID&gt; |Remove uma licença de publicação de ativação instaladas baseadas em token.<br />A operação deve ser executada em uma janela de prompt de comandos com privilégios elevados. |
|\/stao |Define o sinalizador **Token-based Activation Only**, desabilitando a ativação KMS automática.<br />A operação deve ser executada em uma janela do prompt de comandos com privilégios elevados.<br />Esta opção foi removida no Windows Server 2012 R2 e Windows 8.1. Use a opção **/act–type**. |
|\/ctao |Apague o sinalizador **Token-based Activation Only** (padrão), habilitando a ativação KMS automática.<br />A operação deve ser executada em uma janela do prompt de comandos com privilégios elevados.<br />Esta opção foi removida no Windows Server 2012 R2 e Windows 8.1. Use a opção **/act–type**</strong> como substituta. |
|\/ltc |Lista certificados de ativação baseada em token que podem ativar softwares instalados. |
|\/fta &lt;Impressão digital&nbsp;do certificado&gt; \[&lt;PIN&gt;] |Força a ativação baseada em token usando o certificado identificado. O PIN (número de identificação pessoal) será fornecido para desbloquear a chave privada sem solicitação de PIN se você usar certificados protegidos pelo hardware (por exemplo, cartões inteligentes). |

## <a name="active-directory-based-activation-configuration-options"></a>Opções de configuração da ativação baseada no Active Directory

|Opção |Descrição |
| - | - |
|\/ad-activation-online &lt;Chave do produto (Product&nbsp;Key)&gt; \[\<Activation&nbsp;Object&nbsp;name\>] |Coleta dados do Active Directory e inicia sua ativação de floresta usando as credenciais executadas no prompt de comando. O acesso de administrador local não é necessário. No entanto, o acesso de leitura/gravação ao contêiner do objeto de ativação do domínio raiz da floresta é necessário. |
|\/ad-activation-get-IID &lt;Chave do produto (Product&nbsp;Key)&gt; |Esta opção inicia a ativação da floresta do Active Directory no modo de telefone. O resultado é a IID (ID de instalação), que poderá ser usada para ativar a floresta pelo telefone se a conectividade com a Internet não estiver disponível. Ao fornecer o IID na chamada telefônica de ativação, um CID é retornado, sendo usado para concluir a ativação. |
|\/ad-activation-apply-cid &lt;Chave do produto (Product&nbsp;Key)&gt; &lt;ID de&nbsp;confirmação&gt; \[\<Activation&nbsp;Object&nbsp;name>] |Ao usar esta opção, digite o CID informado na chamada telefônica de ativação para concluir a ativação |
|\[/name: &lt;AO_Name&gt;] |Opcionalmente, você pode acrescentar a opção **/name** a qualquer um desses comandos para especificar um nome para o objeto de ativação armazenado no Active Directory. O nome não deve exceder 40 caracteres Unicode. Use aspas duplas para definir explicitamente a cadeia de caracteres de nome.<br />No Windows Server 2012 R2 e Windows 8.1, você pode acrescentar o nome diretamente após **/ad-activation-online &lt;Chave do produto (Product&nbsp;Key)&gt;** e **/ad-activation-apply-cid** sem ter de usar a opção **/name**. |
|\/ao-list |Exibe todos os objetos de ativação disponíveis para o computador local. |
|\/del-ao &lt;AO_DN&gt;<br />\/del-ao &lt;AO_RDN&gt; |Exclui, da floresta, o objeto de ativação especificado. |

## <a name="additional-references"></a>Referências adicionais

- [Referência técnica da ativação de volume](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn502529%28v%3dws.11%29)
- [Visão geral da Ativação de Volume](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831612%28v%3dws.11%29)
