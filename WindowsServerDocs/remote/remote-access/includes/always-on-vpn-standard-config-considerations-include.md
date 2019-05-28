## <a name="standard-configuration-considerations"></a>Considerações sobre a configuração padrão

VPN Always On tem muitas opções de configuração. No entanto, você escolha sua configuração de VPN, no entanto, inclua as seguintes informações:

-   **Tipo de Conexão.** Seleção de protocolo de Conexão é importante e, por fim, anda de mãos dadas com o tipo de autenticação que você usará. Para obter detalhes sobre os protocolos de encapsulamento disponíveis, consulte [tipos de conexão de VPN](https://docs.microsoft.com/windows/security/identity-protection/vpn/vpn-connection-type/).

-   **O roteamento.** Nesse contexto, as regras de roteamento determinam se os usuários podem usar outras rotas de rede enquanto estiver conectado à VPN.

    -   _Túnel dividido_ permite o acesso simultâneo a outras redes, como a Internet.

    -   _Criar túneis à força_ requer que todo o tráfego vá exclusivamente por meio da VPN e não permite o acesso simultâneo a outras redes.

-   **Disparando.** _Disparando_ determina como e quando uma conexão VPN é iniciada (por exemplo, quando um aplicativo é aberto, quando o dispositivo está ligado, manualmente pelo usuário). Para o disparo de opções, consulte o [opções disparada automaticamente o perfil VPN](https://docs.microsoft.com/windows/security/identity-protection/vpn/vpn-auto-trigger-profile/).

-   **Autenticação de dispositivo ou usuário.** VPN Always On usa certificados de dispositivo e de conexão iniciadas pelo dispositivo por meio de um recurso chamado [túnel do dispositivo](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/vpn-device-tunnel-config). Essa conexão pode ser iniciado automaticamente e é persistente, que se assemelha a uma conexão de túnel de infraestrutura do DirectAccess.

>[!TIP]
>Ao migrar do DirectAccess para VPN Always On, considere começar com as opções de configuração que são comparáveis ao ter e, em seguida, expanda a partir daí.

Usando certificados de usuário, o cliente sempre VPN se conecta automaticamente, mas isso é feito no nível do usuário (após a entrada do usuário) em vez de no nível do dispositivo (antes da entrada do usuário). A experiência é contínua ainda para o usuário, mas dá suporte a mecanismos de autenticação mais avançados, como o Windows Hello para empresas.