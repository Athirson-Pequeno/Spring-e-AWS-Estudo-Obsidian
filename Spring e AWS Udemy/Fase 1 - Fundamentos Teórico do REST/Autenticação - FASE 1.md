Começando falando sobre JWT

![[Pasted image 20240307084820.png]]

Resumindo o gráfico.

O cliente manda suas credenciais para a API e a API verifica se estão corretas e o nível de acesso que elas tem, depois retorna para o cliente um token JWT, o cliente armazena esse token e envia nas próximas solicitações, se esse token estiver correto e tiver acesso, ele pode acessar o end point, se não a API retorna algum status de erro como 401, 403 que são erros de acesso. O token pode ter validade baseada através do tempo em que foi gerado, 20min, 1 hora, 300 horas, depende das configurações, também pode-se fazer um refresh token, isso geraria um novo token.

![[Pasted image 20240307085300.png]]

Vermelho = cabeçalho

um ponto (.)

rosa = payload (carga útil tem os dados que serão transmitidos no token, como informações do usuário ou outras informações de autorização.)

um ponto (.)

azul = assinatura (é o que define se o token é ou não válido)