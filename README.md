
Tout fichier `.html` ajouté dans le dossier **Automates** apparaîtra automatiquement sur la page d’accueil.

---

## 🌐 Page d’accueil - index.html

Copier-coller le code ci-dessous dans le fichier `index.html` à la racine du dépôt.

```html
<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8" />
  <title>Automates Pôle IIRD</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />

  <style>
    body {
      margin: 0;
      font-family: "Segoe UI", Roboto, Arial, sans-serif;
      background: linear-gradient(135deg, #0f2027, #203a43, #2c5364);
      min-height: 100vh;
      color: #ffffff;
      display: flex;
      align-items: center;
      justify-content: center;
    }

    .container {
      width: 100%;
      max-width: 900px;
      padding: 40px 20px;
      text-align: center;
    }

    h1 {
      margin-bottom: 10px;
      font-size: 2.5rem;
      letter-spacing: 1px;
    }

    .subtitle {
      opacity: 0.8;
      margin-bottom: 40px;
    }

    .buttons {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(220px, 1fr));
      gap: 20px;
    }

    .btn {
      background: rgba(255, 255, 255, 0.12);
      border: 1px solid rgba(255, 255, 255, 0.25);
      padding: 18px;
      border-radius: 12px;
      color: #fff;
      text-decoration: none;
      font-weight: 500;
      transition: all 0.25s ease;
      backdrop-filter: blur(8px);
    }

    .btn:hover {
      background: rgba(255, 255, 255, 0.25);
      transform: translateY(-4px);
      box-shadow: 0 10px 25px rgba(0, 0, 0, 0.25);
    }

    .loading,
    .error {
      opacity: 0.8;
      font-size: 1.1rem;
    }

    footer {
      margin-top: 50px;
      font-size: 0.85rem;
      opacity: 0.6;
    }
  </style>
</head>
<body>

  <div class="container">
    <h1>Automates Pôle IIRD</h1>
    <div class="subtitle">
      Accès aux automates HTML disponibles
    </div>

    <div id="content" class="loading">
      Chargement des automates...
    </div>

    <footer>
      © Automates HTML - GitHub Pages
    </footer>
  </div>

  <script>
    const owner = "pablocorr";
    const repo = "polerd.github.io";
    const path = "Automates";

    const apiUrl = `https://api.github.com/repos/${owner}/${repo}/contents/${path}`;
    const contentDiv = document.getElementById("content");

    fetch(apiUrl)
      .then(response => {
        if (!response.ok) {
          throw new Error("Erreur lors du chargement GitHub");
        }
        return response.json();
      })
      .then(files => {
        const htmlFiles = files.filter(file =>
          file.type === "file" && file.name.endsWith(".html")
        );

        if (htmlFiles.length === 0) {
          contentDiv.innerHTML = "<div class='error'>Aucun automate trouvé.</div>";
          return;
        }

        const buttonsContainer = document.createElement("div");
        buttonsContainer.className = "buttons";

        htmlFiles.forEach(file => {
          const link = document.createElement("a");
          link.className = "btn";
          link.href = `${path}/${file.name}`;
          link.textContent = file.name.replace(".html", "");
          buttonsContainer.appendChild(link);
        });

        contentDiv.replaceWith(buttonsContainer);
      })
      .catch(err => {
        console.error(err);
        contentDiv.innerHTML =
          "<div class='error'>Impossible de charger la liste des automates.</div>";
      });
  </script>

</body>
</html>

