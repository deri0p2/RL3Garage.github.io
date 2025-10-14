index.html
<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Portfólio de Derick</title>
  <style>
    :root {
      --azul: #005eff;
      --cinza: #f5f7fa;
      --texto: #333;
    }

    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
    }

    body {
      font-family: "Poppins", Arial, sans-serif;
      background: var(--cinza);
      color: var(--texto);
      line-height: 1.6;
    }

    header {
      background: var(--azul);
      color: white;
      text-align: center;
      padding: 40px 20px;
      box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
    }

    header h1 {
      font-size: 2.2em;
      margin-bottom: 10px;
    }

    header p {
      font-size: 1.1em;
      opacity: 0.9;
    }

    nav {
      background: white;
      text-align: center;
      padding: 12px;
      position: sticky;
      top: 0;
      box-shadow: 0 2px 8px rgba(0,0,0,0.08);
    }

    nav a {
      color: var(--azul);
      text-decoration: none;
      margin: 0 14px;
      font-weight: 600;
    }

    nav a:hover {
      text-decoration: underline;
    }

    section {
      padding: 60px 20px;
      max-width: 900px;
      margin: auto;
    }

    h2 {
      text-align: center;
      color: var(--azul);
      margin-bottom: 30px;
    }

    .sobre {
      text-align: center;
      font-size: 1.1em;
    }

    .projetos {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
      gap: 20px;
    }

    .card {
      background: white;
      border-radius: 10px;
      box-shadow: 0 4px 12px rgba(0, 0, 0, 0.08);
      padding: 20px;
      text-align: center;
      transition: transform 0.3s ease;
    }

    .card:hover {
      transform: translateY(-5px);
    }

    .card h3 {
      margin-bottom: 10px;
      color: var(--azul);
    }

    .contato {
      text-align: center;
    }

    .botao {
      display: inline-block;
      background: var(--azul);
      color: white;
      padding: 12px 20px;
      border-radius: 8px;
      text-decoration: none;
      margin-top: 10px;
      font-weight: bold;
      transition: background 0.3s ease;
    }

    .botao:hover {
      background: #0040c1;
    }

    footer {
      background: var(--azul);
      color: white;
      text-align: center;
      padding: 18px;
      margin-top: 40px;
    }
  </style>
</head>
<body>
  <header>
    <h1>Derick Lima</h1>
    <p>Desenvolvedor • Criador • Apaixonado por tecnologia</p>
  </header>

  <nav>
    <a href="#sobre">Sobre</a>
    <a href="#projetos">Projetos</a>
    <a href="#contato">Contato</a>
  </nav>

  <section id="sobre">
    <h2>Sobre mim</h2>
    <p class="sobre">
      Sou uma pessoa curiosa e criativa, gosto de aprender coisas novas e criar projetos usando tecnologia.
      Este site foi criado como um portfólio pessoal para mostrar um pouco do que eu posso fazer.
    </p>
  </section>

  <section id="projetos">
    <h2>Projetos</h2>
    <div class="projetos">
      <div class="card">
        <h3>💻 Projeto 1</h3>
        <p>Um site feito com HTML, CSS e JavaScript simples, mas bonito e responsivo.</p>
      </div>
      <div class="card">
        <h3>📱 Projeto 2</h3>
        <p>Aplicativo web para ajudar nos estudos e organização de tarefas.</p>
      </div>
      <div class="card">
        <h3>🎮 Projeto 3</h3>
        <p>Protótipo de jogo simples inspirado em shooters como Phantom Forces.</p>
      </div>
    </div>
  </section>

  <section id="contato">
    <h2>Contato</h2>
    <div class="contato">
      <p>Quer falar comigo? Envie um e-mail clicando abaixo 👇</p>
      <a href="mailto:seuemail@exemplo.com" class="botao">Enviar e-mail</a>
    </div>
  </section>

  <footer>
    <p>© 2025 • Criado por Derick Lima</p>
  </footer>
</body>
</html>
