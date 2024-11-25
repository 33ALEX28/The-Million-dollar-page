const grid = document.getElementById("grid");
const status = document.getElementById("status");
const connectButton = document.getElementById("connect-wallet");

let walletAddress = null;
const PIXEL_PRICE = 1; // 1 Popcat per pixel

// Initialize a 100x100 grid (1 million pixels)
for (let i = 0; i < 10000; i++) {
    const pixel = document.createElement("div");
    pixel.classList.add("pixel");
    pixel.dataset.index = i;
    pixel.addEventListener("click", () => purchasePixel(i));
    grid.appendChild(pixel);
}

// Connect wallet
connectButton.addEventListener("click", async () => {
    try {
        const { solana } = window;
        if (!solana || !solana.isPhantom) {
            alert("Phantom Wallet not found. Please install it.");
            return;
        }
        const response = await solana.connect();
        walletAddress = response.publicKey.toString();
        status.textContent = `Connected: ${walletAddress}`;
    } catch (error) {
        console.error("Connection failed:", error);
    }
});

// Purchase a pixel
async function purchasePixel(index) {
    if (!walletAddress) {
        alert("Please connect your wallet first.");
        return;
    }

    const pixel = document.querySelector(`.pixel[data-index='${index}']`);
    if (pixel.classList.contains("owned")) {
        alert("This pixel is already owned.");
        return;
    }

    try {
        // Dummy transaction (replace with real Solana transaction logic)
        console.log(`Purchasing pixel ${index} for 1 Popcat token...`);
        
        // Assume transaction successful
        pixel.classList.add("owned");
        pixel.style.backgroundColor = "blue"; // Example customization
        alert(`Pixel ${index} purchased successfully!`);
    } catch (error) {
        console.error("Transaction failed:", error);
        alert("Failed to purchase pixel. Try again.");
    }
}
