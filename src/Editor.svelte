<script>
import FillnTheBlank from '../clsSMFill/FillnTheBlanks.svelte';
import FillnTheBlanksPreview from '../clsSMFill/FillnTheBlanksPreview.svelte';
import swal from 'sweetalert';
import { getDefaultXMl } from '../clsSMFill/defaultXML.svelte';
import { XMLToJSON } from '../clsSMFill/HelperAI.svelte';

    
  
//Variables
let toggleEditor = true;
let myData = getDefaultXMl("editor_item_1.xml");
let obj = XMLToJSON(myData);
let matching = obj.smxml.text.__cdata;


//functions
function showAuthoring() {
    toggleEditor = true;
}
function showReview() {
    toggleEditor = false;
}
function showDropdown() {
    const dropdown = document.getElementById("myDropdown");
        let myDisplay = dropdown.style.display;
        if(myDisplay == 'block') {
            dropdown.style.display = 'none';
        } else {
            dropdown.style.display = 'block';
        }
}

//sweetalert
function showXML() {
    swal({
        title: "XML",
        text: myData,
        buttons: ["Cancel","Done"],
    }).then((val) => {
        console.log(val)
    });
}
</script>

<style>
    #navbar {
        position: fixed;
        top: 0;
        left: 0;
        width: 100%;
        height: 3.5rem;
        background-color: #ffffff;
        border-bottom: 2px solid #d9e7fd;
        display: flex;
        padding-left: 0.5rem;
    }
    button {
        width: 100px;
        border: none;
        padding: 0.5rem;
        margin: 0.5rem 0.5rem 0.5rem 0;
        background-color:#d9e7fd;
    }

    /*Custom Toggle Button*/

    /* box around the slider */
    .switch {
        position: relative;
        display: inline-block;
        width: 40px;
        height: 20px;
    }

    /* hiding default html checkbox */
    .switch input {
        opacity: 0;
        width: 0;
        height: 0;
    }

    #custButton {
        position: absolute;
        right: 2rem;
        top: 1rem;
    }
    #custButton[title]:hover::after {
        color: #333;
        border: none;
        outline: none;
    }
    /* slider */
    .slider {
        position: absolute;
        top: 0;
        bottom: 0;
        left: 0;
        right: 0;
        background-color: #ccc;
        transition: .4s;
    }
    .slider:before {
        position: absolute;
        content:"";
        height: 16px;
        width: 16px;
        left: 2px;
        bottom: 2px;
        background-color: #ffffff;
        transition: .4s;
    }
    input:checked + .slider {
        background-color: #f44336;
    }
    input:focus + .slider {
        box-shadow: 0 0 1px #2196F3;
    }
    input:checked + .slider:before {
        -webkit-transform: translateX(19px);
        -ms-transform: translateX(19px);
        transform: translateX(19px);
    }
    /* Rounded sliders */
    .slider.round {
        border-radius: 20px;
    }

    .slider.round:before {
        border-radius: 50%;
    }

    /*dropdown*/
    .dropBtn {
        background-color: rgba(192, 192, 192, 0.5);
        width: 2rem;
        height: 2rem;
        border: none;
        border-radius: 2rem;
        cursor: pointer;
        margin-top: 12px;
        transition: all .3s;
    }
    .dropBtn:hover, .dropBtn:focus {
        background-color: rgba(192, 192, 192, 1);
        transition: all .3s;
    }
    .dropBtn i {
        font-size: 2rem;
        position: absolute;
        margin-top: -1rem;
        margin-left: -1rem;
    }
    #myDropdown {
        display: none;
        width: 100px;
        margin: 10px -35px;
        border: 1px solid #00bfb6;
        padding: 10px;
        text-align: center;
        color: #00bfb6;
        font-family: arial;
        position: relative; 
        background-color: white;
        border-radius: 5px;
    }
    .sb10:before {
        content: "";
        width: 0px;
        height: 0px;
        position: absolute;
        border-left: 10px solid transparent;
        border-right: 10px solid transparent;
        border-top: 10px solid transparent;
        border-bottom: 10px solid #00bfb6;
        right: 50%;
        top: -19px;
}

    .sb10:after {
        content: "";
        width: 0px;
        height: 0px;
        position: absolute;
        border-left: 10px solid transparent;
        border-right: 10px solid transparent;
        border-top: 10px solid transparent;
        border-bottom: 10px solid #fff;
        right: 50%;
        top: -18px;
    }
    a[href="#xml"] {
        text-decoration: none;
        color: #333;
        font-size: 1rem;
    }
</style>

<nav id="navbar">
    <button type="button" on:click={showAuthoring}>Authoring</button>
    <button type="button" on:click={showReview}>Preview</button>
    <div class="dropdown">
        <button type="button" class="dropBtn" id="dropBtn" on:click={showDropdown}><i class='bx bxs-chevron-down-circle'></i></button>
        <div id="myDropdown" class="sb10">
            <a href="#xml" on:click="{showXML}">XML</a>
        </div>
    </div>
    <!-- toggle switch -->
    {#if !toggleEditor}
    <div id="custButton" title="Review Correct Answer">
        <label class="switch">
            <input type="checkbox" />
            <span class="slider round"></span>
        </label>
    </div>
    {/if}
</nav>

{#if toggleEditor}
    <FillnTheBlank/>
{:else}
    <FillnTheBlanksPreview />
{/if}