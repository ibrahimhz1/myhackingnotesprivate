
```html
<!DOCTYPE html>

<html lang="en">

  

<head>

    <meta charset="UTF-8">

    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <title>Stored XSS vulnerability in replace function</title>

</head>

  

<body>

    <h1>Encoding angle brackets with replace() function</h1>

  

    <p id="page"></p>

    <input type="text" id="input" placeholder="Enter your name">

    <input type="button" value="Submit" onclick="replaceAngleBracketsEncode(input.value)">

  

    <script>

        function replaceAngleBracketsEncode(input) {

            // var result = input.replace(/</g, "&lt;").replace(/>/g, "&gt;"); // safe way to replace all occurrences of < and > with their HTML entity equivalents

            var result = input.replace('<', '&lt;').replace('>', '&gt;'); // unsafe way to replace only the first occurrence of < and > with their HTML entity equivalents

  

            var paraElement = document.getElementById("page");

            paraElement.innerHTML = result;

  

            console.log("replaceAngleBracketsEncode: " + result);

        }

    </script>

</body>

  

</html>
```