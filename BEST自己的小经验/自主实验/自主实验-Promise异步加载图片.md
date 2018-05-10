

    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <meta http-equiv="X-UA-Compatible" content="ie=edge">
        <title>异步加载图片</title>
    </head>
    <body>
        <h1>我是大标题</h1>
        <ul id="list">
            <!-- <li></li> -->
        </ul>
        <div id="imgBox">
            
        </div>
    
        <script>
            var imgBox = document.getElementById('imgBox');
    
            var data = [{src: "./personQR.jpg"}, 
                {src: "./publicQR.jpg"},
                {src: "./personQR.jpg"},
                {src: "./publicQR.jpg"},
                {src: "./personQR.jpg"},
                {src: "./publicQR.jpg"}];
    
            function loadAsyncImg(url) {
                return new Promise((res, rej) => {
                    var img = new Image();
                    img.onload = function() {
                        res(img);
                    };
                    
                    img.onerror = function() {
                        rej(new Error('Could not load image at' + url));
                    };
    
                    img.src = url;
                    imgBox.appendChild(img);
                });
            }
    
            for(let i=0;i<data.length;i++){
                loadAsyncImg(data[i].src);
            }
        </script>
    </body>
    </html>