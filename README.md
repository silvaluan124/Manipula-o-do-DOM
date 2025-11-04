<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Mini SPA - JavaScript Avançado</title>
  <link rel="stylesheet" href="css/style.css" />
</head>
<body>
  <header>
    <h1>Minha SPA em JavaScript</h1>
    <nav>
      <a href="#/" data-link>Home</a>
      <a href="#/sobre" data-link>Sobre</a>
      <a href="#/contato" data-link>Contato</a>
    </nav>
  </header>

  <main id="app"></main>

  <footer>
    <p>Desenvolvido para atividade de JavaScript avançado</p>
  </footer>

  <script type="module" src="js/main.js"></script>
</body>
</html>
body {
  font-family: Arial, sans-serif;
  margin: 0;
  background-color: #f7f7f7;
}

header {
  background: #007bff;
  color: white;
  padding: 15px;
  text-align: center;
}

nav a {
  color: white;
  margin: 0 10px;
  text-decoration: none;
  font-weight: bold;
}

nav a:hover {
  text-decoration: underline;
}

main {
  padding: 20px;
  background: white;
  max-width: 800px;
  margin: 20px auto;
  border-radius: 8px;
  box-shadow: 0 2px 6px rgba(0,0,0,0.1);
}

footer {
  text-align: center;
  font-size: 0.9em;
  padding: 15px;
  background: #eee;
}
import { loadRoute } from "./router.js";
import { renderTemplate } from "./templates.js";

window.addEventListener("DOMContentLoaded", () => {
  loadRoute();
});

window.addEventListener("hashchange", () => {
  loadRoute();
});
import { renderTemplate } from "./templates.js";

export function loadRoute() {
  const path = location.hash || "#/";
  let page = "home";

  if (path === "#/sobre") page = "about";
  if (path === "#/contato") page = "contact";

  renderTemplate(page);
}
import { validarFormulario } from "./formValidation.js";

export function renderTemplate(page) {
  fetch(`pages/${page}.html`)
    .then(res => res.text())
    .then(html => {
      document.getElementById("app").innerHTML = html;
      if (page === "contact") {
        const form = document.querySelector("form");
        form.addEventListener("submit", validarFormulario);
      }
    })
    .catch(() => {
      document.getElementById("app").innerHTML = "<p>Erro ao carregar a página.</p>";
    });
}
export function validarFormulario(event) {
  event.preventDefault();

  const nome = document.getElementById("nome").value.trim();
  const email = document.getElementById("email").value.trim();
  const mensagem = document.getElementById("mensagem").value.trim();
  const aviso = document.getElementById("aviso");

  if (!nome || !email || !mensagem) {
    aviso.textContent = "⚠️ Todos os campos são obrigatórios!";
    aviso.style.color = "red";
    return;
  }

  if (!email.includes("@") || !email.includes(".")) {
    aviso.textContent = "⚠️ Digite um e-mail válido!";
    aviso.style.color = "red";
    return;
  }

  aviso.textContent = "✅ Formulário enviado com sucesso!";
  aviso.style.color = "green";
  event.target.reset();
}
