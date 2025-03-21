# SHOOPELOGIN
Este repositório contém um conjunto de testes automatizados de login desenvolvidos com o Cypress. O objetivo é validar o comportamento do sistema de autenticação da página de login da Shopee em diferentes cenários.

📋 Objetivos dos Testes
Os testes cobrem os seguintes cenários:

✅ Login bem-sucedido: Verificação se um usuário válido consegue logar com sucesso.
❌ Erro para e-mail inválido: Exibição de mensagem de erro para e-mail no formato incorreto.
❌ Erro para senha incorreta: Mensagem de erro ao digitar a senha errada.
⚠️ Campos de login vazios: Teste de tentativa de login sem preencher os campos obrigatórios.
🚫 Bloqueio de conta após tentativas múltiplas inválidas.
🔒 Validação de ocultação de caracteres no campo de senha.
↩️ Logout correto após login bem-sucedido.
🔐 Acesso negado a páginas restritas sem login.
🔎 Verificação do título correto da página de login.


🛠️ Configuração do Ambiente
Pré-requisitos
Node.js instalado (versão recomendada: >= 14.x)
NPM instalado (geralmente já incluso com o Node.js)
Acesso à internet para instalação das dependências
Instalação do Cypress
Clone o repositório:
git clone https://github.com/seu-usuario/nome-do-repositorio.git
cd nome-do-repositorio

Ajustes Necessários
Seletores de Elementos: Certifique-se de que os seletores (input[name='email'], input[name='senha'], etc.) estejam corretos para o ambiente de teste.
URL de Testes: No arquivo login.spec.js, ajuste a URL (https://shopee.com.br/flash_sale) se necessário.
Dados Sensíveis: Evite expor informações sensíveis (e-mails e senhas reais) em repositórios públicos.

/nome-do-repositorio
│── cypress/
│ └── integration/
│ └── login.spec.js # Arquivo com os testes automatizados de login
│── cypress.json # Configurações do Cypress
│── package.json # Dependências do projeto
│── README.md # Instruções do projeto (este arquivo)

📄 Licença
Este projeto é apenas para fins educacionais e de teste. Certifique-se de seguir a política de privacidade e termos de uso da Shopee ao realizar testes

