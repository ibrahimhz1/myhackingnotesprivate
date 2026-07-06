
```js
function search() {
    var xhr = new XMLHttpRequest();
    var url = "http://localhost:8000/data.json";

    xhr.onreadystatechange = function () {
        if (this.readyState == 4 && this.status == 200) {
            console.log(this.response);

            let myObj = this.response;
            console.log("Type of parsed response: " + typeof myObj);
            // let myObj2 = JSON.parse(myObj); // Safe way to parse JSON and to prevent XSS, it checks the input and only allows valid JSON to be parsed 
            eval("var myObj2 = " + myObj); // Unsafe way to parse JSON and can lead to XSS, since it is valid javascript code, it can execute any code that is passed to it, including malicious code.
            console.log("Type of parsed response: " + typeof myObj2); 
            let page = document.getElementById("page");
            page.innerText = myObj2.information;
        }
    }
    xhr.open("GET", url, true);
    xhr.send();
}
```