##### 一个线条的动画

````
<!doctype html>
<html>
     <head>
            <meta charset="UTF-8">
            <meta name="Generator" content="EditPlus®">
            <meta name="Author" content="">
            <meta name="Keywords" content="">
            <meta name="Description" content="">
            <title>Document</title>
            <style>
                #wrap{
                    width:500px;
                    height:500px;
                    border-radius:50%;
                    background-color:#E4393C;
                    margin:100px auto;
                    position:relative;
                }
                #wrap:befor{
                    content:"";
                    display:table;
                }
                #line{
                    width:0px;
                    height:5px;
                    border-radius:5px;
                    background-color:#fff;
                    margin:0 auto;
                    position:absolute;
                    top:338px;
                    left:0px;
                    right:0px;
                    transition:width 1s linear;
                }
                #wrap:hover #line{
                    width:200px;
                }
            </style>
     </head>
     <body>
        <div id="wrap">
            <div id="line"></div>
        </div>
     </body>
</html>
````