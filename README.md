# MEME-MAKER
+ 그림판 Javascript 연습 코딩
  <details>
    <summary>결과미리보기</summary>
  </details>
# 개발기간
+ 2023.8.18
# 개발환경
+ VSCODE 1.80.1
+ GOOGLE CHROME 115.0.5790.110
# 사용된 기술 스택
+ HTML
+ CSS
+ JAVASCRIPT
# 주요기능
+ canvas 태그를 활용한 draw & fill
  <details>
    <summary>Javascript Code : app.js</summary>
    
      const fileInput = document.querySelector("#file");
      const saveBtn = document.querySelector("#save");
      const textInput = document.querySelector("#text");
      const modeBtn = document.querySelector("#mode-btn");
      const destroyBtn = document.querySelector("#destroy-btn");
      const eraserBtn = document.querySelector("#eraser-btn");
      const color = document.querySelector("#color");
      const colorOptions = Array.from(document.querySelectorAll(".color-option"));
      const canvas = document.querySelector("canvas");
      const ctx = canvas.getContext("2d");
      const lineWidth = document.querySelector("#line-width");
      
      const CANVAS_WIDTH = 800;
      const CANVAS_HEIGHT = 800;
      
      canvas.width = CANVAS_WIDTH;
      canvas.height = CANVAS_HEIGHT;
      ctx.lineWidth = lineWidth.value;
      ctx.linecap = "round";
      let isPainting = false;
      let isFilling = false;
      
      function onMove(event) {
        if(isPainting){
          ctx.lineTo(event.offsetX, event.offsetY);
          ctx.stroke();
          return;
        }
        ctx.moveTo(event.offsetX, event.offsetY);
      }
      
      function startPainting(event){
        isPainting = true;
      
      }
      function cancelPainting(event){
        isPainting = false;
        ctx.beginPath();
      }
      
      function onLineWidthChange(event){
        ctx.lineWidth = event.target.value;
      }
      
      function onColorChange(event){
        ctx.strokeStyle = event.target.value;
        ctx.fillStyle = event.target.value;
      
      }
      
      function onColorClick(event){
        const colorValue = event.target.dataset.color;
        ctx.strokeStyle = colorValue;
        ctx.fillStyle = colorValue;
        color.value = colorValue;
      }
      
      function onModeClick() {
        if(isFilling){
          isFilling = false;
          modeBtn.innerHTML = "<strong>🖌️Draw</strong> or 🧺Fill";
        } else {
          isFilling = true;
          modeBtn.innerHTML = "🖌️Draw or <strong>🧺Fill</strong>";
      
        }
      }
      
      function onCanvasClick(){
        if(isFilling){
          ctx.fillRect(0, 0, CANVAS_WIDTH, CANVAS_HEIGHT);
        }
      }
      
      function onDestroyClick(){
        ctx.fillStyle = "white";
        ctx.fillRect(0, 0, CANVAS_WIDTH, CANVAS_HEIGHT);
      }
      
      function onEraserClick(){
        ctx.strokeStyle = "white";
        isFilling = false;
        modeBtn.innerText = "Fill";
      }
      
      
      function onFileChange(event){
        const file = event.target.files[0];
        const url = URL.createObjectURL(file);
        const image = new Image();
        image.src = url;
        image.onload = function(){
          ctx.drawImage(image, 0, 0, CANVAS_WIDTH, CANVAS_HEIGHT);
          fileInput.value = null;
        }
      }
      
      function onDoubleClick(event){
        const text = textInput.value;
        if (text !== "") {
          ctx.save();
          ctx.lineWidth = 1;
          ctx.font = "68px serif";
          ctx.fillText(text, event.offsetX, event.offsetY);
          ctx.restore();
        }
      }
      
      function onSaveClick(){
        const url = canvas.toDataURL();
        const a = document.createElement("a");
        a.href = url;
        a.download = "myDrawing.png";
        a.click();
      }
      
      
      
      canvas.addEventListener("dblclick", onDoubleClick);
      canvas.addEventListener("mousemove", onMove);
      canvas.addEventListener("mousedown", startPainting);
      canvas.addEventListener("mouseup", cancelPainting);
      canvas.addEventListener("mouseleave",cancelPainting);
      canvas.addEventListener("click",onCanvasClick);
      
      lineWidth.addEventListener("change", onLineWidthChange);
      color.addEventListener("change", onColorChange);
      
      colorOptions.forEach(color => color.addEventListener("click", onColorClick));
      
      modeBtn.addEventListener("click",onModeClick);
      destroyBtn.addEventListener("click", onDestroyClick);
      eraserBtn.addEventListener("click", onEraserClick);
      fileInput.addEventListener("change", onFileChange);
      saveBtn.addEventListener("click", onSaveClick);
  </details>
    <details>
    <summary>HTML Code : index.html</summary>

      <!DOCTYPE html>
      <html lang="ko">
      
      <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Mime Maker</title>
        <link rel="stylesheet" href="/style.css">
      </head>
      
      <body>
        <div class="color-options">
          <input type="color" id="color">
          <div class="color-option" style="background-color:#1abc9c" data-color="#1abc9c"></div>
          <div class="color-option" style="background-color:#3498db" data-color="#3498db"></div>
          <div class="color-option" style="background-color:#34495e" data-color="#34495e"></div>
          <div class="color-option" style="background-color:#27ae60" data-color="#27ae60"></div>
          <div class="color-option" style="background-color:#8e44ad" data-color="#8e44ad"></div>
          <div class="color-option" style="background-color:#f1c40f" data-color="#f1c40f"></div>
          <div class="color-option" style="background-color:#e74c3c" data-color="#e74c3c"></div>
          <div class="color-option" style="background-color:#95a5a6" data-color="#95a5a6"></div>
          <div class="color-option" style="background-color:#d35400" data-color="#d35400"></div>
          <div class="color-option" style="background-color:#f39c12" data-color="#f39c12"></div>
          <div class="color-option" style="background-color:#c0392b" data-color="#c0392b"></div>
        </div>
        <canvas></canvas>
        <div class="btns">
          <input id="line-width" type="range" min="1" max="10" value="1" step="0.1" />
          <button id="mode-btn"><strong>🖌️Draw</strong> or 🧺Fill</button>
          <button id="destroy-btn">💥Destroy</button>
          <button id="eraser-btn">❌Erase</button>
          <label for="file">
            📂Add Photo
            <input type="file" accept="image/*" id="file" /></label>
          <input type="text" placeholder="Add text here" id="text" />
          <button id="save">🖼️Save image</button>
        </div>
        <script src="/app.js"></script>
      </body>
      
      </html>
    </details>
    <details>
    <summary>CSS Code : style.css</summary>
      
      @import "reset.css";
  
      body {
        display: flex;
        justify-content: space-between;
        gap: 20px;
        align-items: center;
        background-color: gainsboro;
        padding: 20px;
        font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
      }
      
      
      .btns {
        display: flex;
        flex-direction: column;
        gap: 20px;
      }
      
      canvas {
        width: 800px;
        height: 800px;
        background-color: white;
        border-radius: 10px;
      }
      
      .color-options {
        display: flex;
        flex-direction: column;
        gap: 20px;
        align-items: center;
      }
      
      .color-option{
        width: 50px;
        height: 50px;
        border-radius: 50%;
        cursor: pointer;
        border: 5px solid white;
        transition: transform ease-in-out .1s;
      }
      
      .color-option:hover {
        transform: scale(1.2);
      }
      
      input#color {
        background-color: white;
      }
      
      button,label {
        all:unset;
        padding: 10px 0px;
        text-align: center;
        background-color: royalblue;
        color: white;
        font-weight: 500;
        cursor: pointer;
        border-radius: 10px;
        transition: opacity linear .1s;
      }
      
      button:hover {
        opacity: 0.85;
      }
      
      input#text {
        all:unset;
        padding: 10px 0px;
        text-align: center;
        border-radius: 10px;
        font-weight: 500;
        background-color: white;
      }
      
      input#file {
        display: none;
      }
      
      strong {
        color: black;
        font-weight: 800;
        font-size: 25px;
      }
    </details>
