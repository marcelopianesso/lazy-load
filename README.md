# Carregamento sob demanda das imagens (lazy load)

Utilizando o IntersectionObserver para carregar as imagens quando aparecem na tela.

Para funcionar em dispositivos movéis, utilizar bliblioteca do [IntersectionObserver]

```html
    <!-- Elemento load imagem -->
    <img class="lazy-img" data-src="image/01.jpg" />

    <!-- Elemento load background imagem -->
    <div class="lazy-background" data-background="image/01.jpg"></div>

    <!-- Elemento load before background imagem -->
    <div class="lazy-before-background" data-before-background="image/01.jpg"></div>
    <!-- css load before background imagem -->
    .element-class:before { background-image: var(--before-background); }

    <!-- Elemento load after background imagem -->
    <div class="lazy-after-background" data-after-background="image/01.jpg"></div>
    <!-- css load after background imagem -->
    .element-class:before { background-image: var(--after-background); }
```

```javascript
    // Inicialização com vanilla JS:
    
    // Inicia o script com o carregamento do dom da página
    document.addEventListener("DOMContentLoaded", function() {

        // Define a classe dos elementos
        var lazyImages = document.querySelectorAll('.lazy-img');
        var lazyBackground = document.querySelectorAll('.lazy-background');
        var lazyAfterBackground = document.querySelectorAll('.lazy-after-background');
        var lazyBeforeBackground = document.querySelectorAll('.lazy-before-background');

        // Configurações do observador
        var lazyConfig = {
            // Quando a imagem fica dentro de 50px no eixo Y, inicia o download
            rootMargin: '50px 0px',
            threshold: 0.01
        };

        // O observador das imagens na página
        var lazyObserver = new IntersectionObserver(onIntersection, lazyConfig);

        // Percorre todas as imagens
        lazyImages.forEach(image => {
            lazyObserver.observe(image);
        });

        // Percorre todos os elementos com background
        lazyBackground.forEach(image => {
            lazyObserver.observe(image);
        });

        // Percorre todos os elementos com background before
        lazyBeforeBackground.forEach(image => {
            lazyObserver.observe(image);
        });

        // Percorre todos os elementos com background after
        lazyAfterBackground.forEach(image => {
            lazyObserver.observe(image);
        });

        function onIntersection(entries) {
            entries.forEach(entry => {
                // Testa se está no viewport
                if (entry.intersectionRatio > 0) {
                    // Para de observar o elemento
                    lazyObserver.unobserve(entry.target);
                    preloadImage(entry.target);
                }
            });
        }

        // Carrega as imagens
        function preloadImage(image) {
            var lazyImage = image;
            // Testa se é img ou background
            if (lazyImage.dataset.src) {
                lazyImage.src = lazyImage.dataset.src;
                lazyImage.classList.add('fade-in');
                lazyImage.classList.remove('lazy-img');
            } else if (image.dataset.background) {
                lazyImage.style.backgroundImage = 'url(' + lazyImage.dataset.background + ')';
                lazyImage.classList.add('fade-in');
                lazyImage.classList.remove('lazy-background');
            } else if (lazyImage.dataset.afterBackground) {
                lazyImage.style = '--after-background:url("' + lazyImage.dataset.afterBackground + '")';
                lazyImage.classList.add('fade-in');
                lazyImage.classList.remove('lazy-after-background');
            } else if (lazyImage.dataset.beforeBackground) {
                lazyImage.style = '--before-background:url("' + lazyImage.dataset.beforeBackground + '")';
                lazyImage.classList.add('fade-in');
                lazyImage.classList.remove('lazy-before-background');
            }
        }
    });
```


[IntersectionObserver]: https://github.com/w3c/IntersectionObserver/tree/master/polyfill