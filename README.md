# HACKATHON-2025
Criar uma api multilingue para um projeto Hackathon 2025 que acontecerá no Senac Ribeirão Preto/SP, com pagina de login; cadastro de clientes com "n" endereços, e pagina principal (show modal; Web).

| Etapa | Tarefa                                            |
| ----- | ------------------------------------------------- |
| 1     | Estruturar textos em arquivos por idioma          |
| 2     | Criar endpoints que respeitam o parâmetro `lang`  |
| 3     | Adaptar front-end para mudar textos dinamicamente |
| 4     | Criar banco com relacionamento cliente-endereço   |
| 5     | Implementar modais multilíngues no front-end      |

Recursos da API multilíngue:
🚀 Suporte a vários idiomas (via ?lang=pt).

👤 Cadastro e login de cliente.

📦 Clientes com múltiplos endereços.

🖥️ Modal na interface web com tradução.

⚙️ Fácil expansão para mais idiomas e páginas.
---------------------------------------------------------------------------------------------------------------------------------------------------------------------
Preparar um repositório base no GitHub com a estrutura completa para o projeto:

🔗 Repositório GitHub: https://github.com/seu-usuario/api-multilingue-clientes

Estrutura do Projeto:
api-multilingue-clientes/
├── api/
│   ├── lang/
│   │   ├── en.json
│   │   └── pt.json
│   ├── cliente.php
│   ├── endereco.php
│   └── login.php
├── public/
│   ├── index.html
│   ├── login.html
│   ├── cadastro.html
│   └── js/
│       └── app.js
├── db/
│   └── conexao.php
├── sql/
│   └── estrutura.sql
└── README.md
----------------------------------------------------------------------------------------------------------------------------------------------------------------------
Banco de Dados MySQL (estrutura.sql)

CREATE TABLE clientes (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nome VARCHAR(255),
    email VARCHAR(255) UNIQUE,
    senha VARCHAR(255)
);

CREATE TABLE enderecos (
    id INT AUTO_INCREMENT PRIMARY KEY,
    cliente_id INT,
    rua VARCHAR(255),
    numero VARCHAR(50),
    cidade VARCHAR(100),
    estado VARCHAR(100),
    cep VARCHAR(20),
    FOREIGN KEY (cliente_id) REFERENCES clientes(id) ON DELETE CASCADE
);
---------------------------------------------------------------------------------------------------------------------------------------------------------------------
Arquivos de tradução (ex: api/lang/pt.json)
json
{
  "login": "Entrar",
  "register": "Cadastrar",
  "email": "Email",
  "password": "Senha",
  "add_address": "Adicionar Endereço",
  "logout": "Sair"
}
api/lang/en.json:
{
  "login": "Login",
  "register": "Register",
  "email": "Email",
  "password": "Password",
  "add_address": "Add Address",
  "logout": "Logout"
}
--------------------------------------------------------------------------------------------------------------------------------------------------------------------
Carregador de idioma (api/lang.php)

<?php
function getLang($langCode) {
    $file = __DIR__ . "/lang/{$langCode}.json";
    if (!file_exists($file)) {
        $file = __DIR__ . "/lang/en.json"; // fallback
    }
    return json_decode(file_get_contents($file), true);
}
--------------------------------------------------------------------------------------------------------------------------------------------------------------------
 Exemplo de endpoint de login (api/login.php)

<?php
include_once 'lang.php';

header('Content-Type: application/json');

$lang = $_GET['lang'] ?? 'en';
$labels = getLang($lang);

echo json_encode([
    "email_label" => $labels['email'],
    "password_label" => $labels['password'],
    "login_button" => $labels['login']
]);
--------------------------------------------------------------------------------------------------------------------------------------------------------------------
Front-end dinâmico (HTML + JS)
login.html

<select id="langSelect">
  <option value="en">English</option>
  <option value="pt">Português</option>
</select>

<form>
  <label id="emailLabel"></label>
  <input type="email" />
  
  <label id="passwordLabel"></label>
  <input type="password" />
  
  <button id="loginBtn" type="submit"></button>
</form>

<script src="js/app.js"></script>

js/app.js

function carregarIdioma(lang) {
  fetch(`/api/login.php?lang=${lang}`)
    .then(res => res.json())
    .then(data => {
      document.getElementById("emailLabel").textContent = data.email_label;
      document.getElementById("passwordLabel").textContent = data.password_label;
      document.getElementById("loginBtn").textContent = data.login_button;
    });
}

document.getElementById("langSelect").addEventListener("change", function () {
  carregarIdioma(this.value);
});

// Carrega idioma padrão
carregarIdioma("en");
---------------------------------------------------------------------------------------------------------------------------------------------------------------
Página principal com Modal (index.html)

<button onclick="abrirModal()">Show Modal</button>

<div id="modal" class="modal">
  <div class="modal-content">
    <span onclick="fecharModal()" class="close">&times;</span>
    <p id="modalMessage"></p>
  </div>
</div>

<script>
function abrirModal() {
  document.getElementById("modalMessage").textContent = "👋 Hello!";
  document.getElementById("modal").style.display = "block";
}

function fecharModal() {
  document.getElementById("modal").style.display = "none";
}
</script>

<style>
.modal { display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: #000000a0; }
.modal-content { margin: 10% auto; padding: 20px; background: white; width: 300px; border-radius: 10px; }
.close { float: right; cursor: pointer; }
</style>
----------------------------------------------------------------------------------------------------------------------------------------------------------------
🛠️ Funcionalidades Implementadas:

API Multilíngue: Suporte a múltiplos idiomas utilizando arquivos JSON para traduções.

Login e Cadastro de Clientes: Formulários com validação e integração com a API.

Cadastro de Múltiplos Endereços: Cada cliente pode ter vários endereços associados.

Página Principal com Modal: Interface web com modal responsivo.

Banco de Dados MySQL: Script SQL para criação das tabelas necessárias.

🚀 Como Utilizar
Clonar o Repositório:

bash
Copiar
Editar
git clone https://github.com/seu-usuario/api-multilingue-clientes.git
Configurar o Banco de Dados:

Importe o arquivo sql/estrutura.sql no seu servidor MySQL.

Atualize as credenciais de acesso ao banco no arquivo db/conexao.php.

Executar o Projeto:

Utilize um servidor local (como XAMPP ou WAMP) e aponte para o diretório public/.

Acessar as Páginas:

Login: http://localhost/api-multilingue-clientes/public/login.html

Cadastro: http://localhost/api-multilingue-clientes/public/cadastro.html

Página Principal: http://localhost/api-multilingue-clientes/public/index.html

📄 Documentação Adicional
Para mais detalhes sobre a estrutura e funcionalidades do projeto, consulte o arquivo README.md no repositório.

