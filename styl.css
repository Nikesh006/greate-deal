document.addEventListener('DOMContentLoaded', () => {
    // --- DOM Elements ---
    const searchInput = document.getElementById('searchInput');
    const categoryFilters = document.getElementById('categoryFilters');
    const productGrid = document.getElementById('productGrid');
    const initialMessage = document.getElementById('initialMessage');
    const loader = document.getElementById('loader');
    const noResultsMessage = document.getElementById('noResultsMessage');
    const resultsTitle = document.getElementById('resultsTitle');
    const resultsCount = document.getElementById('resultsCount');

    let currentProducts = [];

    // --- Main Setup ---
    function initialize() {
        populateCategoryFilters();
        displayProducts(realProductDatabase); // Show all products initially
        searchInput.addEventListener('input', handleSearch);
        setupFilterButtons();
    }

    // --- Data & Filtering Logic ---
    function groupAndSortProducts(products) {
        const grouped = products.reduce((acc, product) => {
            const productName = product.name;
            if (!acc[productName]) {
                acc[productName] = {
                    imageUrl: product.imageUrl,
                    category: product.category,
                    sellers: []
                };
            }
            acc[productName].sellers.push({
                store: product.store,
                price: product.price,
                productUrl: product.productUrl
            });
            return acc;
        }, {});

        for (const productName in grouped) {
            grouped[productName].sellers.sort((a, b) => a.price - b.price);
        }
        return grouped;
    }

    function handleSearch() {
        const query = searchInput.value.toLowerCase();
        const activeCategory = document.querySelector('.filter-btn.active').dataset.category;

        let filteredProducts = realProductDatabase;

        // Apply category filter first
        if (activeCategory !== 'all') {
            filteredProducts = filteredProducts.filter(p => p.category === activeCategory);
        }

        // Apply search query
        if (query) {
            filteredProducts = filteredProducts.filter(p => p.name.toLowerCase().includes(query));
        }
        
        displayProducts(filteredProducts);
    }
    
    // --- UI Rendering ---
    function displayProducts(products) {
        productGrid.innerHTML = ''; // Clear the grid
        initialMessage.classList.add('hidden');
        loader.classList.add('hidden');
        noResultsMessage.classList.add('hidden');

        currentProducts = products;
        const groupedProducts = groupAndSortProducts(products);
        const productCount = Object.keys(groupedProducts).length;

        updateResultsHeader(productCount);

        if (productCount === 0) {
            productGrid.appendChild(noResultsMessage);
            noResultsMessage.classList.remove('hidden');
            return;
        }

        for (const productName in groupedProducts) {
            const product = groupedProducts[productName];
            productGrid.appendChild(createProductCard(productName, product));
        }
    }

    function createProductCard(productName, product) {
        const card = document.createElement('div');
        card.className = 'product-card';
        const bestPrice = product.sellers[0].price;

        const sellersHtml = product.sellers.map((seller, index) => `
            <li class="seller-item ${index === 0 ? 'is-best' : ''}">
                <a href="${seller.productUrl}" target="_blank" rel="noopener noreferrer">
                    <span class="seller-info">
                        ${storeLogos[seller.store] || ''}
                        <span class="seller-name">${seller.store}</span>
                    </span>
                    <span class="seller-price">₹${seller.price.toLocaleString('en-IN')}</span>
                </a>
            </li>
        `).join('');

        card.innerHTML = `
            <div class="product-image-wrapper">
                <img src="${product.imageUrl}" alt="${productName}" class="product-image" onerror="this.src='https://placehold.co/300x300/f3f4f6/9ca3af?text=Image+Error'">
            </div>
            <div class="product-info">
                <h3 class="product-name">${productName}</h3>
                <p class="best-price">Starts from <span>₹${bestPrice.toLocaleString('en-IN')}</span></p>
                <ul class="seller-list">${sellersHtml}</ul>
            </div>
        `;
        return card;
    }

    function populateCategoryFilters() {
        const categories = ['all', ...new Set(realProductDatabase.map(p => p.category))];
        categoryFilters.innerHTML = categories.map(cat => `
            <button class="filter-btn ${cat === 'all' ? 'active' : ''}" data-category="${cat}">
                ${cat.charAt(0).toUpperCase() + cat.slice(1)}
            </button>
        `).join('');
    }

    function setupFilterButtons() {
        categoryFilters.addEventListener('click', (e) => {
            if (e.target.classList.contains('filter-btn')) {
                document.querySelector('.filter-btn.active').classList.remove('active');
                e.target.classList.add('active');
                handleSearch(); // Re-run search with the new filter
            }
        });
    }

    function updateResultsHeader(count) {
        const activeCategory = document.querySelector('.filter-btn.active').textContent.trim();
        resultsTitle.textContent = `Showing ${activeCategory} Products`;
        resultsCount.textContent = `${count} item${count !== 1 ? 's' : ''}`;
    }

    // --- Start the App ---
    initialize();
});
