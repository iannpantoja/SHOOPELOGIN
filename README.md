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







/// <reference types="cypress" />

Cypress.on('uncaught:exception', (err, runnable) => {
    return false; // Ignora erros não relacionados ao Cypress
});

describe("Login", () => {
    const url = "https://shopee.com.br/flash_sale";
    
    beforeEach(() => {
        cy.visit(url);
        // Fechar o pop-up se existir (com um timeout maior para garantir)
        cy.get('body').then((body) => {
            if (body.find('#popup-sair').length > 0) {
                cy.get("#popup-sair", { timeout: 10000 }).should('be.visible').click();
            }
        });
    });
    
    it("Deve fazer login com sucesso", () => {
        // Ajuste os seletores conforme necessário para os campos de login
        cy.get("input[name='email']").type("iannaires@gmail.com", { delay: 100 }); // Substituir seletor conforme necessário
        cy.get("input[name='senha']").type("senhaSegura123", { log: false }); // Substituir seletor conforme necessário
        cy.get("button[type='submit']").should("be.visible").click(); // Ajustar o seletor para o botão de login
        cy.get(".success-message").should("be.visible").and("contain", "Login realizado"); // Ajuste o seletor conforme necessário
    });
    
    it("Deve exibir erro para e-mail inválido", () => {
        cy.get("input[name='email']").type("email_invalido", { delay: 100 }); // Ajustar o seletor
        cy.get("input[name='senha']").type("senha123", { log: false }); // Ajustar o seletor
        cy.get("button[type='submit']").should("be.visible").click(); // Ajustar o seletor
        cy.get(".error-message").should("be.visible").and("contain", "E-mail ou senha inválidos"); // Ajuste o seletor conforme necessário
    });
    
    it("Deve exibir erro para senha incorreta", () => {
        cy.get("input[name='email']").type("iannaires@gmail.com", { delay: 100 }); // Ajustar o seletor
        cy.get("input[name='senha']").type("senhaErrada", { log: false }); // Ajustar o seletor
        cy.get("button[type='submit']").should("be.visible").click(); // Ajustar o seletor
        cy.get(".error-message").should("be.visible").and("contain", "E-mail ou senha inválidos"); // Ajuste o seletor
    });
    
    it("Deve exibir erro ao tentar logar sem preencher os campos", () => {
        cy.get("button[type='submit']").should("be.visible").click(); // Ajustar o seletor
        cy.get(".error-message").should("be.visible").and("contain", "Preencha todos os campos"); // Ajuste o seletor conforme necessário
    });
    
    it("Deve bloquear usuário após múltiplas tentativas de login inválidas", () => {
        for (let i = 0; i < 5; i++) {
            cy.get("input[name='email']").type("iannaires@gmail.com", { delay: 100 }); // Ajustar o seletor
            cy.get("input[name='senha']").type("senhaErrada", { log: false }); // Ajustar o seletor
            cy.get("button[type='submit']").should("be.visible").click(); // Ajustar o seletor
            cy.get(".error-message").should("be.visible").and("contain", "E-mail ou senha inválidos"); // Ajuste o seletor
        }
        cy.get(".error-message").should("be.visible").and("contain", "Usuário bloqueado"); // Ajuste o seletor conforme necessário
    });
    
    it("Deve verificar se o botão de login está desativado quando os campos estão vazios", () => {
        cy.get("button[type='submit']").should("be.disabled"); // Ajuste o seletor conforme necessário
    });
    
    it("Deve validar se o campo de senha esconde os caracteres digitados", () => {
        cy.get("input[name='senha']").type("senhaSegura123"); // Ajustar o seletor
        cy.get("input[name='senha']").should("have.attr", "type", "password"); // Ajuste o seletor conforme necessário
    });
    
    it("Deve validar se o usuário consegue fazer logout corretamente", () => {
        cy.get("input[name='email']").type("iannaires@gmail.com", { delay: 100 }); // Ajustar o seletor
        cy.get("input[name='senha']").type("senhaSegura123", { log: false }); // Ajustar o seletor
        cy.get("button[type='submit']").should("be.visible").click(); // Ajustar o seletor
        cy.get(".logout-button").should("be.visible").click(); // Ajuste o seletor conforme necessário
        cy.get(".success-message").should("be.visible").and("contain", "Logout realizado"); // Ajuste o seletor conforme necessário
    });
    
    it("Deve exibir mensagem de erro ao tentar acessar página restrita sem login", () => {
        cy.visit("https://shopee.com.br/flash_sale");
        cy.get(".error-message").should("be.visible").and("contain", "Acesso negado"); // Ajuste o seletor conforme necessário
    });
    
    it("Deve verificar se a página de login contém o título correto", () => {
        cy.title().should("contain", "Shopee"); // Ajuste o título conforme necessário



        
    });
    
});









