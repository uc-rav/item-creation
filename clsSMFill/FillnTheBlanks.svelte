<script>
    import { getDefaultXMl } from './defaultXML.svelte';
    import { XMLToJSON } from './HelperAI.svelte';

    let myData = getDefaultXMl("editor_item_1.xml");
    let obj = XMLToJSON(myData);
    let matching = obj.smxml.text.__cdata;
    
    console.log(myData);
    console.log(matching);
    console.log(matching.match(/[%{](.*?)[%]/g));
    let newString = matching.replace(/[%{](.*?)[%]/g,'<input type="text"  placeholder="Textbox" style="height: 30px; width: 150px; margin: 5px 5px 5px 5px; border-left: 5px solid #d9e7fd; border-radius: 5px; border-top: 1px solid #d9e7fd; border-bottom: 1px solid #d9e7fd; border-right: 1px solid #d9e7fd; "/>');
    console.log(newString);

    //context-menu 
   
    window.addEventListener("contextmenu",function(event) {
        event.preventDefault();
        console.log(event)
        let contextElement = document.getElementById("context-menu");
        contextElement.style.top = event.clientY + "px";
        console.log(event.offsetY);
        contextElement.style.left = event.clientX + "px";
        console.log(event.offsetX);
        contextElement.style.display = "block";
        contextElement.style.transform = "scale(1)";
        contextElement.style.transition = "transform 200ms ease-in-out";
    });
    function exitContext() {
        document.getElementById("context-menu").style.display = "none";
    }
    function addRecord() {
        // let cursorPos = document.getElementById(FillnTheBlanks_Body).caret().start();
        // console.log("mycursor position",cursorPos);
        document.getElementById("context-menu").style.display = "none";
        swal({
            title: "Add Responce",
            text: "Write correct answer here!",
            content: "input",
            buttons: ["Cancel","Done"]
        }).then((value) => {
            console.log(value);
            if(value) {
                concatString();
            }
        });
    }
    function concatString() {
        alert("add resposnce");
        let myElement = document.getElementById("FillnTheBlanks_Body");
        let startPosition = myElement.selectionStart;
        let endPosition = myElement.selectionEnd;
        console.log(startPosition);
        console.log(endPosition);

    }
</script>

<style>
    #FillnTheBlanks {
        position: fixed;
        top: calc(55% - 150px);
        left: 5rem;
        right: 5rem;
        height: 300px;
        background-color: rgb(255, 255, 255);
        border: 2px solid #d9e7fd;
        border-radius: 5px;
        box-shadow: rgba(0, 0, 0, 0.6) 0px 5px 15px;
    }
    #FillnTheBlanks_Header {
        position: relative;
        top: 0;
        left: 0;
        width: 100%;
        background-color: #d9e7fd;
        height: 2.5rem;
    }
    #FillnTheBlanks_Body {
        outline: 0px solid transparent;
        padding: 1rem;
    }

    /*Custom context Menu*/
    #context-menu {
        position: fixed;
        z-index: 10000;
        width: 150px;
        background-color: #e5e4e2;
        padding: 0.2rem;
        border-radius: 5px;
        transform: scale(0);
        transform-origin: top left;
    }
    #context-menu hr {
        margin: 0.2rem 0px 0.1rem 0;
        border: 0;
        height: 1px;
        background: #333;
        background-image: linear-gradient(to right, #ccc, #333, #ccc);
    }
    #context-menu .items i {
        font-size: 1.7rem;
        margin-right: 0.2rem;
    }
    #context-menu .items span{
        position: absolute;
        margin-top: 2px;
    }
    #context-menu .items {
        background-color: #e5e4e2;
        transition: all .2s;
    }
    #context-menu .items:hover {
        background-color: silver;
        transition: all .2s;
        border-radius: 5px;
    }
</style>

<nav id="navbar">  
</nav>
<div id="FillnTheBlanks" >
    <div id="FillnTheBlanks_Header">
    </div>
    <div id="FillnTheBlanks_Body" role="textbox" contenteditable="true" spellcheck="false" class="myDiv"> 
        {@html newString}       
        <!-- Custom context-menu -->
        <div id="context-menu" contenteditable="false">
            <div class="items" id="addResponce" on:click={addRecord}>
                <i class='bx bxs-plus-square'></i><span>Add Responce</span>
            </div>
            <hr>
            <div class="items" id="exitTab" on:click={exitContext}>
                <i class='bx bxs-exit' ></i><span>Exit</span>
            </div>
        </div>
    </div>
</div>