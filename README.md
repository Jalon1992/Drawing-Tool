# Drawing-Tool
Draw Building Layout
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Draw Layout</title>
    <style><label for="shape">Select Shape:</label>
<select id="shape">
    <option value="line">Line</option>
    <option value="rectangle">Rectangle</option>
    <option value="circle">Circle</option>
</select>const shapeSelect = document.getElementById('shape');
let currentShape = shapeSelect.value;  // Default shape is "line"

shapeSelect.addEventListener('change', (e) => {
    currentShape = e.target.value;  // Update the shape when the user changes selection
});

let startX, startY;

canvas.addEventListener('mousedown', (e) => {
    startX = e.offsetX;
    startY = e.offsetY;
    drawing = true;
});

canvas.addEventListener('mousemove', (e) => {
    if (drawing) {
        const endX = e.offsetX;
        const endY = e.offsetY;
        ctx.clearRect(0, 0, canvas.width, canvas.height);  // Clear the canvas while drawing
        
        // Redraw existing strokes (important for shapes and free drawing)
        strokes.forEach(stroke => {
            ctx.beginPath();
            ctx.moveTo(stroke[0].x, stroke[0].y);
            stroke.forEach(point => {
                ctx.lineTo(point.x, point.y);
                ctx.stroke();
            });
        });

        if (currentShape === 'line') {
            // Draw line
            ctx.beginPath();
            ctx.moveTo(startX, startY);
            ctx.lineTo(endX, endY);
            ctx.stroke();
        } else if (currentShape === 'rectangle') {
            // Draw rectangle
            const width = endX - startX;
            const height = endY - startY;
            ctx.strokeRect(startX, startY, width, height);
        } else if (currentShape === 'circle') {
            // Draw circle
            const radius = Math.sqrt(Math.pow(endX - startX, 2) + Math.pow(endY - startY, 2));
            ctx.beginPath();
            ctx.arc(startX, startY, radius, 0, 2 * Math.PI);
            ctx.stroke();
        }
    }
});

canvas.addEventListener('mouseup', () => {
    drawing = false;

    // Store the shape in the strokes array (save its data)
    const shapeData = { type: currentShape, startX, startY, endX, endY };
    strokes.push(shapeData);
});


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
