# TMDB Movie Explorer 🎬

Una aplicación web moderna y responsiva para explorar películas populares y buscar títulos utilizando la API de TMDB. Construida con Bootstrap 5 y JavaScript vanila.

## 🚀 Características
- **Diseño Oscuro Moderno**: Interfaz elegante con sombras y efectos hover.
- **Búsqueda Dinámica**: Caja de búsqueda autodesplegable con filtrado en tiempo real (debounced).
- **Paginación Personalizada**: Organizada exactamente en 12 películas por página.
- **Responsive**: Diseño adaptable a móviles, tablets y escritorio.
- **Preparado para CI/CD**: Configuración lista para GitHub Pages y Vercel.

---

## 🛠️ Configuración de Variables de Entorno

Para mantener tu API Key de TMDB segura y no exponerla en el código fuente de tu repositorio, sigue estas instrucciones:

### 1. En Vercel
1. Ve a la configuración de tu proyecto en el dashboard de Vercel.
2. Selecciona **Settings** > **Environment Variables**.
3. Añade una nueva variable:
   - **Key**: `TMDB_API_KEY`
   - **Value**: `Tu_API_Key_de_TMDB`
4. En **Settings** > **General**, cambia el **Build Command** por el siguiente:
   ```bash
   sed -i "s/TMDB_API_KEY_PLACEHOLDER/$TMDB_API_KEY/g" index.html
   ```
5. Despliega de nuevo. Vercel reemplazará el marcador en el archivo `index.html` durante la compilación.

### 2. En GitHub Pages (vía GitHub Actions)
1. Ve a tu repositorio en GitHub > **Settings** > **Secrets and variables** > **Actions**.
2. Añade un "New repository secret":
   - **Name**: `TMDB_API_KEY`
   - **Secret**: `Tu_API_Key_de_TMDB`
3. Crea un archivo en `.github/workflows/deploy.yml` con el siguiente contenido:

```yaml
name: Deploy to GitHub Pages

on:
  push:
    branches: ["main"]

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Inject API Key
        run: |
          sed -i "s/TMDB_API_KEY_PLACEHOLDER/${{ secrets.TMDB_API_KEY }}/g" index.html

      - name: Deploy to GitHub Pages
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./
```

---

## 📝 Notas de Implementación
- Se utiliza la API v3 de TMDB.
- La paginación de 12 elementos requiere una lógica interna para solicitar páginas de la API (que entrega 20) y segmentarlas correctamente.
- Se incluye un fallback para imágenes de póster no encontradas.
