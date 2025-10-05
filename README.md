<!DOCTYPE html>
<html lang="pl">
<head>
  <meta charset="UTF-8">
  <title>Fotos – Biblioteka zdjęć</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <style>
    body {
      font-family: 'Segoe UI', sans-serif;
      margin: 0;
      background-color: #f0f2f5;
    }

    header {
      background-color: #4a90e2;
      color: white;
      padding: 20px;
      text-align: center;
      font-size: 2rem;
    }

    #uploadSection {
      background: white;
      padding: 30px;
      max-width: 600px;
      margin: 30px auto;
      border-radius: 10px;
      box-shadow: 0 4px 12px rgba(0,0,0,0.1);
    }

    input, select, textarea, button {
      width: 100%;
      margin-bottom: 15px;
      padding: 10px;
      font-size: 1rem;
      border-radius: 6px;
      border: 1px solid #ccc;
    }

    button {
      background-color: #4a90e2;
      color: white;
      border: none;
      cursor: pointer;
    }

    button:hover {
      background-color: #357ab8;
    }

    .category-section {
      margin: 40px;
    }

    .category-title {
      font-size: 1.5rem;
      margin-bottom: 20px;
      color: #333;
    }

    .gallery {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
      gap: 20px;
    }

    .photo-card {
      background: white;
      border-radius: 10px;
      box-shadow: 0 4px 10px rgba(0,0,0,0.1);
      overflow: hidden;
      transition: transform 0.3s ease;
    }

    .photo-card:hover {
      transform: scale(1.03);
    }

    .photo-card img {
      width: 100%;
      height: auto;
      display: block;
    }

    .photo-info {
      padding: 10px;
    }

    .photo-info h3 {
      margin: 0 0 5px;
      font-size: 1.1rem;
    }

    .photo-info p {
      margin: 0;
      font-size: 0.9rem;
      color: #555;
    }
  </style>
</head>
<body>

  <header>📷 Fotos – Twoja Biblioteka Zdjęć</header>

  <div id="uploadSection">
    <input type="file" id="photoInput" accept="image/*">
    <input type="text" id="titleInput" placeholder="Tytuł zdjęcia">
    <textarea id="descInput" placeholder="Opis zdjęcia"></textarea>
    <select id="categorySelect">
      <option value="Podróże">Podróże</option>
      <option value="Rodzina">Rodzina</option>
      <option value="Przyroda">Przyroda</option>
      <option value="Zwierzęta">Zwierzęta</option>
      <option value="Inne">Inne</option>
    </select>
    <button onclick="addPhoto()">Dodaj zdjęcie</button>
  </div>

  <div id="categoriesContainer"></div>

  <script>
    const categoriesContainer = document.getElementById('categoriesContainer');
    const categoryMap = {};

    function createCategorySection(category) {
      const section = document.createElement('div');
      section.className = 'category-section';
      section.innerHTML = `<div class="category-title">${category}</div><div class="gallery" id="gallery-${category}"></div>`;
      categoriesContainer.appendChild(section);
      categoryMap[category] = section.querySelector('.gallery');
    }

    function addPhoto() {
      const fileInput = document.getElementById('photoInput');
      const title = document.getElementById('titleInput').value.trim();
      const desc = document.getElementById('descInput').value.trim();
      const category = document.getElementById('categorySelect').value;

      if (!fileInput.files[0]) return;

      const reader = new FileReader();
      reader.onload = function(e) {
        if (!categoryMap[category]) {
          createCategorySection(category);
        }

        const card = document.createElement('div');
        card.className = 'photo-card';
        card.innerHTML = `
          <img src="${e.target.result}" alt="${title}">
          <div class="photo-info">
            <h3>${title || 'Bez tytułu'}</h3>
            <p>${desc || 'Brak opisu'}</p>
          </div>
        `;
        categoryMap[category].prepend(card);
      };
      reader.readAsDataURL(fileInput.files[0]);

      // Reset form
      fileInput.value = '';
      document.getElementById('titleInput').value = '';
      document.getElementById('descInput').value = '';
    }
  </script>

</body>
</html>
