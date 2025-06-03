# Drawing-Tool
Draw Building Layout
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Draw Layout</title>
    <style>
        canvas {
            border: 1px solid black;
            cursor: crosshair;
        }
        #saveButton {
            margin-top: 10px;
        }
    </style>
</head>
<body>
    <h2>Draw the Building Layout</h2>
    <canvas id="drawingCanvas" width="500" height="500"></canvas>
    <br>
    <button id="saveButton">Save Drawing as Image</button>

    <script>
        const canvas = document.getElementById('drawingCanvas');
        const ctx = canvas.getContext('2d');
        let drawing = false;

        // Start drawing on mouse down
        canvas.addEventListener('mousedown', (e) => {
            drawing = true;
            ctx.beginPath();
            ctx.moveTo(e.offsetX, e.offsetY);
        });

        // Draw as mouse moves
        canvas.addEventListener('mousemove', (e) => {
            if (drawing) {
                ctx.lineTo(e.offsetX, e.offsetY);
                ctx.stroke();
            }
        });

        // Stop drawing on mouse up
        canvas.addEventListener('mouseup', () => {
            drawing = false;
        });

        // Save the drawing as an image (PNG)
        document.getElementById('saveButton').addEventListener('click', () => {
            const dataUrl = canvas.toDataURL('image/png');  // Get the drawing as a PNG image (Base64)
            console.log('Drawing saved:', dataUrl);
            alert('Your drawing has been saved as an image. You can upload it later to your PDF.');
            
            // If you want to enable the user to download the image, you can use this code:
            const link = document.createElement('a');
            link.href = dataUrl;
            link.download = 'drawing.png';  // The image will be saved as 'drawing.png'
            link.click();
        });
    </script>
</body>
</html>
