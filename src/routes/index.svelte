<script lang="ts">
    import { onMount } from 'svelte';


  const loadingImg = 'https://i.stack.imgur.com/h6viz.gif';
  const fetchURL = 'https://dog.ceo/api/breeds/image/random';
    let data = '';
    let breedName = 'Loading';
    let URLList = [];
    let currImgPos = 0;
  $: currURL = loadingImg;

    onMount( () => {
        // to prevent unnecessarily fetching when writing code...
        setTimeout(() => {
            fetchImage('DontIncrementImgPos');
        }, 1000);

    });

    function logSimpleURL() {
        let simpleURLList = URLList;
        simpleURLList = simpleURLList.map(el => {
      return el.split("/")[4];
        });
        console.log('simpleURLList', simpleURLList);

    };

    async function fetchImage(isIncrementImgPos) {
    currURL = loadingImg;
        const response = await fetch(fetchURL);
    data = await response.json();
        setFetchedData(isIncrementImgPos);
    };

  function updateCurrURL() {
    currURL = URLList[currImgPos];
  };

    function setFetchedData(isIncrementImgPos) {
        URLList = [...URLList, data.message];
    if (isIncrementImgPos === 'incrementImgPos') currImgPos+=1;
    updateCurrURL();
    logSimpleURL();
        getBreedName();
    };

    function getBreedName() {
    updateCurrURL();
        breedName = currURL?.split("/")[4];
    // if (typeof(breedName) === 'undefined') return breedName = 'Loading';
        breedName = breedName?.split("-")?.join(' ');
        breedName = breedName?.charAt(0)?.toUpperCase() + breedName?.slice(1);
    };

    function handlePrev() {
    currImgPos-=1;
        getBreedName();
    };

    async function handleNext() {
    if ( currURL === URLList[URLList.length - 1]) {
      await fetchImage('incrementImgPos');
        } else {
      currImgPos+=1;
      getBreedName();
    };
    };

</script>


<div class='btn-container'>
  <button class='arrow' disabled={URLList[currImgPos-1] === undefined} on:click={handlePrev}>
    <div class='unicode-holder-left'>◀</div>
  </button>

  <button class='arrow' on:click={handleNext}>
    <div class='unicode-holder-right'>▶</div>
  </button>
</div>


<div class='container'>
  <h1 class="breed-name" title={currURL} >{breedName}</h1>
  <img id='myImg' src={currURL} alt="Dog" />
</div>




<style lang="postcss">
    .container {
        display: flex;
        justify-content: center;
        flex-direction: column;
    }
    .breed-name{
        text-align: center;
    }
    #myImg {
        min-height: 40vh;
        max-height: 80vh;
        min-width: 40vw;
        max-width: 97vw;
    }
    .btn-container {
        position: absolute;
        top: 94px;
        display: flex;
        justify-content: space-between;
        align-items: flex-end;
        width: 97vw;
    }
    button {
        cursor: pointer;
        user-select: none;
    }
    .arrow {
        height: 80vh;
        width: 40px;
        background: rgba(255,255,255,0.6);
        border-radius: 0px;
    }
</style>
