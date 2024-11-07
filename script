// Initialize local storage and page load
document.addEventListener("DOMContentLoaded", loadStores);

const storeForm = document.getElementById("store-form");
const stockForm = document.getElementById("stock-form");
const orderForm = document.getElementById("order-form");

const storeSelect = document.getElementById("select-store");
const preferredStoreSelect = document.getElementById("select-preferred-store");
const stockList = document.getElementById("stock-list");
const orderList = document.getElementById("order-list");

let stores = [];
let stock = [];
let orders = [];

// Page Navigation Functions
function openUserMode() {
    document.getElementById("landing-page").classList.add("hidden");
    document.getElementById("user-mode").classList.remove("hidden");
    updateStoreDropdown();
}

function openStoreMode() {
    document.getElementById("landing-page").classList.add("hidden");
    document.getElementById("store-mode").classList.remove("hidden");
    updateStoreDropdown();
}

function goToLandingPage() {
    document.getElementById("landing-page").classList.remove("hidden");
    document.getElementById("store-mode").classList.add("hidden");
    document.getElementById("user-mode").classList.add("hidden");
}

// Load stores from local storage on page load
function loadStores() {
    stores = JSON.parse(localStorage.getItem("stores")) || [];
    stock = JSON.parse(localStorage.getItem("stock")) || [];
    orders = JSON.parse(localStorage.getItem("orders")) || [];
    updateStoreDropdown();
    displayStock();
}

// Register a new store
storeForm.addEventListener("submit", (e) => {
    e.preventDefault();
    const storeName = document.getElementById("store-name").value;
    const location = document.getElementById("location").value;

    stores.push({ name: storeName, location });
    localStorage.setItem("stores", JSON.stringify(stores));

    updateStoreDropdown();
    storeForm.reset();
});

function updateStoreDropdown() {
    storeSelect.innerHTML = '<option value="">Select Store</option>';
    preferredStoreSelect.innerHTML = '<option value="">Select Store</option>';

    stores.forEach(store => {
        const option = document.createElement("option");
        option.value = store.name;
        option.textContent = store.name;
        storeSelect.appendChild(option);
        preferredStoreSelect.appendChild(option.cloneNode(true));
    });
}

// Add stock to a store
stockForm.addEventListener("submit", (e) => {
    e.preventDefault();
    const storeName = storeSelect.value;
    const itemName = document.getElementById("item-name").value;
    const itemQuantity = parseInt(document.getElementById("item-quantity").value, 10);

    if (storeName) {
        stock.push({ store: storeName, item: itemName, quantity: itemQuantity });
        localStorage.setItem("stock", JSON.stringify(stock));

        displayStock();
        stockForm.reset();
    }
});

// Display stock for all stores
function displayStock() {
    stockList.innerHTML = '';
    stock.forEach(item => {
        const li = document.createElement("li");
        li.textContent = `${item.store}: ${item.item} - ${item.quantity} available`;
        stockList.appendChild(li);
    });
}

// Place an order
orderForm.addEventListener("submit", (e) => {
    e.preventDefault();
    const searchItem = document.getElementById("search-item").value.toLowerCase();
    const deliveryPreference = document.getElementById("delivery-preference").value;
    const preferredStore = preferredStoreSelect.value;

    let storeOptions;

    if (deliveryPreference === "fastest") {
        storeOptions = stock.filter(item => item.item.toLowerCase() === searchItem);
    } else {
        storeOptions = stock.filter(item => item.store === preferredStore && item.item.toLowerCase() === searchItem);
    }

    if (storeOptions.length > 0) {
        const store = storeOptions[0].store;
        orders.push({ item: searchItem, store, delivery: deliveryPreference });
        localStorage.setItem("orders", JSON.stringify(orders));
        displayOrders();
    } else {
        alert("Item not available in selected stores");
    }

    orderForm.reset();
});

// Display all orders
function displayOrders() {
    orderList.innerHTML = '';
    orders.forEach(order => {
        const li = document.createElement("li");
        li.textContent = `Order: ${order.item} from ${order.store} (Delivery: ${order.delivery})`;
        orderList.appendChild(li);
    });
}

// Show preferred store select based on delivery preference
document.getElementById("delivery-preference").addEventListener("change", (e) => {
    if (e.target.value === "preferred") {
        preferredStoreSelect.classList.remove("hidden");
    } else {
        preferredStoreSelect.classList.add("hidden");
    }
});
