<div class="navigation">
  <a href="/Image compressor pro">Home</a> | 
  <a href="/index.html-2">Tool Version 1</a> | 
  <a href="/Tools-index.html-2">Tool Version 2</a>
</div><!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ImageCompressorPro</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <header>
        <h1>ImageCompressorPro</h1>
        <nav>
            <a href="#">Home</a> | <a href="#">How It Works</a> | <a href="#">Blog</a> | <a href="#">Contact</a>
        </nav>
    </header>
    <main>
        <section class="compression-tool">
            <h2>Free Online Image Compression Tool</h2>
            <p>Reduce your image file size without losing quality. Supports JPG, PNG, and WEBP formats. Fast, secure, and completely free!</p>
            <div class="drop-zone" id="dropZone">
                <p>Drag & Drop Your Images Here</p>
                <input type="file" id="imageInput" accept="image/*" multiple>
            </div>
            <div class="controls">
                <label for="compressionLevel">Compression Level:</label>
                <input type="range" id="compressionLevel" min="1" max="100" value="80">
                <span id="compressionValue">80%</span>
                <label for="outputFormat">Output Format:</label>
                <select id="outputFormat">
                    <option value="keep">Keep Original</option>
                    <option value="jpg">JPG</option>
                    <option value="png">PNG</option>
                    <option value="webp">WEBP</option>
                </select>
            </div>
            <div class="preview" id="preview">
                <div>
                    <p>Original</p>
                    <p>Original Size: <span id="originalSize">--</span></p>
                </div>
                <div>
                    <p>Compressed</p>
                    <p>Compressed Size: <span id="compressedSize">--</span></p>
                </div>
            </div>
            <button id="compressButton" disabled>Compress Image</button>
            <a id="downloadLink" style="display: none;">Download Compressed Image</a>
        </section>
        <footer>
            <div class="feature">
                <img src="https://img.icons8.com/ios-filled/50/000000/bolt.png" alt="Fast">
                <p>Fast Processing<br>Our tool compresses your images in seconds using advanced algorithms.</p>
            </div>
            <div class="feature">
                <img src="https://img.icons8.com/ios-filled/50/000000/lock.png" alt="Secure">
                <p>Secure & Private<br>All image processing happens in your browser. Your files never leave your device.</p>
            </div>
        </footer>
    </main>
    <script src="script.js"></script>
</body>
</html>body {
    font-family: Arial, sans-serif;
    margin: 0;
    padding: 0;
    background-color: #f0f2f5;
    color: #333;
}

header {
    text-align: center;
    padding: 20px;
    background-color: #fff;
}

nav a {
    margin: 0 10px;
    text-decoration: none;
    color: #333;
}

.compression-tool {
    max-width: 600px;
    margin: 20px auto;
    padding: 20px;
    background: #fff;
    border-radius: 8px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    text-align: center;
}

.drop-zone {
    border: 2 dashed #ccc;
    padding: 50px;
    margin-bottom: 20px;
    background: #fafafa;
}

.drop-zone.dragover {
    background: #e1e1e1;
}

.controls label {
    display: block;
    margin: 10px 0 5px;
}

#compressionLevel {
    width: 100%;
}

#compressionValue {
    margin-left: 10px;
}

.preview {
    display: flex;
    justify-content: space-around;
    margin: 20px 0;
}

button {
    background-color: #007BFF;
    color: white;
    padding: 10px 20px;
    border: none;
    border-radius: 5px;
    cursor: pointer;
}

button:disabled {
    background-color: #ccc;
    cursor: not-allowed;
}

footer {
    display: flex;
    justify-content: space-around;
    padding: 20px;
    background: #fff;
    margin-top: 20px;
}

.feature img {
    vertical-align: middle;
}

.feature p {
    display: inline-block;
    margin-left: 10px;
}document.addEventListener('DOMContentLoaded', () => {
    const dropZone = document.getElementById('dropZone');
    const imageInput = document.getElementById('imageInput');
    const compressButton = document.getElementById('compressButton');
    const downloadLink = document.getElementById('downloadLink');
    const compressionLevel = document.getElementById('compressionLevel');
    const compressionValue = document.getElementById('compressionValue');
    const originalSize = document.getElementById('originalSize');
    const compressedSize = document.getElementById('compressedSize');
    const outputFormat = document.getElementById('outputFormat');
    const preview = document.getElementById('preview');

    // Update compression value display
    compressionLevel.addEventListener('input', () => {
        compressionValue.textContent = `${compressionLevel.value}%`;
    });

    // Drag and Drop functionality
    dropZone.addEventListener('dragover', (e) => {
        e.preventDefault();
        dropZone.classList.add('dragover');
    });

    dropZone.addEventListener('dragleave', () => {
        dropZone.classList.remove('dragover');
    });

    dropZone.addEventListener('drop', (e) => {
        e.preventDefault();
        dropZone.classList.remove('dragover');
        const files = e.dataTransfer.files;
        handleFiles(files);
    });

    // File input change
    imageInput.addEventListener('change', (e) => {
        handleFiles(e.target.files);
    });

    function handleFiles(files) {
        if (files.length > 0) {
            const file = files[0]; // Handle one file at a time for simplicity
            const img = new Image();
            img.onload = () => {
                const canvas = document.createElement('canvas');
                canvas.width = img.width;
                canvas.height = img.height;
                const ctx = canvas.getContext('2d');
                ctx.drawImage(img, 0, 0);
                originalSize.textContent = `${(file.size / 1024).toFixed(2)} KB`;
                compressButton.disabled = false;

                compressButton.onclick = () => {
                    const quality = compressionLevel.value / 100;
                    canvas.toBlob((blob) => {
                        const compressedUrl = URL.createObjectURL(blob);
                        compressedSize.textContent = `${(blob.size / 1024).toFixed(2)} KB`;
                        downloadLink.href = compressedUrl;
                        downloadLink.download = `compressed_${file.name.split('.')[0]}.${outputFormat.value || file.name.split('.').pop()}`;
                        downloadLink.style.display = 'block';
                    }, outputFormat.value === 'keep' ? file.type : `image/${outputFormat.value}`, quality);
                };
            };
            img.src = URL.createObjectURL(file);
        }
    }
});
